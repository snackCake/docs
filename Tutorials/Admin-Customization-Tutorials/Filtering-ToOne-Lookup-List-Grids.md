#Filtering To-One Lookup List Grids

In this tutorial, we'll show you how to filter lookup list grids in the Admin. 
For example, let's say you wish to filter a to-one lookup modal to only return records based on a previously selected value.
We can easily do this using a Custom Persistence Handler and a Custom Criteria on your mapped ToOne entity.

Let's illustrate our example by creating a simple Movie Admin interface. One requirement is that
only certain Genres are available for certain Movie Types. So, in our admin, we would like to restrict the lookup
of a Genre to only show those that are available for that format.

##Custom Criteria

First, let's create our movie domain.

```java
@Entity
@Inheritance(strategy = InheritanceType.JOINED)
@Table(name = "MY_MOVIE")
@AdminPresentationClass(friendlyName = "MovieImpl", populateToOneFields = PopulateToOneFieldsEnum.TRUE)
public class MovieImpl implements Movie, AdminMainEntity {

    @Id
    @GeneratedValue(generator = "MovieId", strategy = GenerationType.TABLE)
    @TableGenerator(name = "MovieId", table = "SEQUENCE_GENERATOR", pkColumnName = "ID_NAME", valueColumnName = "ID_VAL", pkColumnValue = "MovieImpl", allocationSize = 50)
    @Column(name = "MOVIE_ID")
    @AdminPresentation(friendlyName = "MovieImpl", visibility = VisibilityEnum.HIDDEN_ALL)
    protected Long id;

    @Column(name = "NAME", nullable = false)
    @AdminPresentation(friendlyName = "MovieImpl_Name", order = 1, prominent = true, gridOrder = 1)
    protected String name;
    
    @Column(name = "TYPE", nullable = false)
    @AdminPresentation(friendlyName = "MovieImpl_Type", order=Presentation.FieldOrder.TYPE,
            fieldType=SupportedFieldType.BROADLEAF_ENUMERATION,
            broadleafEnumeration="com.mycompany.movie.type.MovieType",
            prominent = true, gridOrder = 2)
    protected String type;
    
    @ManyToOne(targetEntity = GenreImpl.class)
    @JoinColumn(name = "GENRE_ID")
    @AdminPresentation(friendlyName = "MovieImpl_Genre")
    @AdminPresentationToOneLookup(lookupDisplayProperty = "name", customCriteria = "movieGenres")
    protected Genre genre;
    
}

```

Notice that we have a ManyToOne mapping to Genre. (In real life this would more likely be a @ManyToMany as a movie can have many Genres, but for this tutorial we'll keep it simple)
One important attribute to take note of is the `customCriteria` declared on the `AdminPresentationToOneLookup` annotation.

By specifying this `customCriteria` on this `@ManyToOne` property, we can create a Custom Persistence Handler that supports a Genre and can determine that this is an association with a Movie.


##Custom Persistence Handler

Now, let's go ahead and create our Genre Persistence Handler class. It might look something like this:


```java
@Component("myGenreCustomPersistenceHandler")
public class GenreCustomPersistenceHandler extends CustomPersistenceHandlerAdapter {

    private static final Log LOG = LogFactory.getLog(GenreCustomPersistenceHandler.class);

    public static final String MOVIE_GENRES = "movieGenres";
    public static final String REQUESTING_ENTITY_ID = "requestingEntityId";

    @Resource(name = "myMovieService")
    protected MovieService movieService;

    @Override
    public Boolean canHandleFetch(PersistencePackage persistencePackage) {
        String ceilingEntityFullyQualifiedClassname = persistencePackage.getCeilingEntityFullyQualifiedClassname();
        try {
            Class testClass = Class.forName(ceilingEntityFullyQualifiedClassname);
            return Genre.class.isAssignableFrom(testClass);
        } catch (ClassNotFoundException e) {
            return false;
        }
    }


    @Override
    public DynamicResultSet fetch(PersistencePackage persistencePackage, CriteriaTransferObject cto, DynamicEntityDao dynamicEntityDao, RecordHelper helper) throws ServiceException {

        Long movieId = null;
        boolean movieFilteredGenres = false;
        for (String criteria : persistencePackage.getCustomCriteria()) {
            if (criteria.startsWith(REQUESTING_ENTITY_ID)) {
                movieId = Long.parseLong(criteria.split("=")[1]);
            } else if (MOVIE_GENRES.equals(criteria)){
                movieFilteredGenres = true;
            }
        }

        if (movieFilteredGenres && movieId != null) {
            FilterMapping genreFilter = getFilterMappingForAllowedGenres(movieId, cto, dynamicEntityDao);
            if (genreFilter != null) {
                cto.getAdditionalFilterMappings().add(genreFilter);
            }
        }

        PersistenceModule myModule = helper.getCompatibleModule(persistencePackage.getPersistencePerspective().getOperationTypes().getFetchType());
        return myModule.fetch(persistencePackage, cto);
    }

    @SuppressWarnings({"rawtypes", "unchecked"})
    protected FilterMapping getFilterMappingForAllowedGenres(Long movieId, CriteriaTransferObject cto, DynamicEntityDao dynamicEntityDao) {

        Movie movie = movieService.findMovieById(movieId);
        if (movie != null && movie.getType() != null) {

            List<Genre> allowedGenres = movieService.findAllowedGenresByType(movie.getType());
            if (allowedGenres != null && allowedGenres.size() > 0) {
                return new FilterMapping()
                        .withDirectFilterValues(allowedGenres)
                        .withRestriction(new Restriction()
                                .withPredicateProvider(new PredicateProvider() {
                                    @Override
                                    public Predicate buildPredicate(CriteriaBuilder builder, FieldPathBuilder fieldPathBuilder,
                                                                    From root, String ceilingEntity,
                                                                    String fullPropertyName, Path explicitPath, List directValues) {
                                        List<Predicate> allowedGenres = new ArrayList<Predicate>();
                                        for (Object o : directValues) {
                                            allowedGenres.add(builder.equal(root.get("id").as(String.class), ((Genre) o).getId().toString()));
                                        }
                                     return builder.or(allowedGenres.toArray(new Predicate[allowedGenres.size()]));
                                    }
                                })
                        );
            }

        }

        return null;
    }


```

There are a lot of things going on here. Let's first examine the `canHandleFetch` and `fetch` methods. We want to implement 
the `canHandleFeth` method for all fetches of the `Genre` entity. However, we just want to filter the results only if it is currently
fetched from the context of editing a Movie entity. We can do this with the `customCriteria` that we added to the Movie entity above.
Now, in the `fetch` method we can inspect the `persistencePackage.getCustomCriteria()` and see the properties that are on there.
If it contains the string "movieGenres" we know its being fetched from a lookup of the Movie entity. The "requestingEntityId"
will give us the ID of the movie that this genre is being fetched for. So, now we can customize the list with a custom `FilterMapping`.
The `getFilterMappingForAllowedGenres()` method in the above example shows how a new Filter Mapping is constructed that retrieves all
the allowed genres based on the saved Movie Type and creates a Predicate Provider that filters the list based on the IDs of the Genres.

Now, when you invoke the lookup modal on the admin, you should only see those Genres that apply to the selected Movie type.
