# Country and State Changes
In order to better internationalize the Broadleaf domain, there have been changes in the way `BLC_COUNTRY` and `BLC_STATE` are used.
For one, `BLC_STATE` is deprecated in favor of `BLC_COUNTRY_SUB`

The CountrySubdivision entity is a more international friendly entity that define codes for identifying the principal subdivisions (e.g., provinces or states)
for a Country. A Country Subdivision can also have a category defined in `BLC_COUNTRY_SUB_CAT`.
    The Country Subdivision Category is meant to define the type of subdivision, for example: "State", "Province", "Municipality", "Borough", etc...
    The `load_code_tables.sql` script provided in the Heat Clinic DemoSite application gives a list of some of the most common and widely used subdivision categories by the ISO

## Changes in use
In the past, Country and States were tied to an Address. However, this is not an ideal model if you would also like to restrict
the countries that you wish to ship to based on this entity. For example, let's say you only wish
to ship to the "US" and "Mexico". In the past, you would populate the `BLC_COUNTRY` table
with only the "US" and "Mexico." However, you would need to make customizations if you allow your customers
to pay with a billing address in a country other than "US" or "Mexico" since an address is tied to a Country.

To overcome this, the tie between Countries and Addresses have been removed.
More details about the changes to Address and how it now uses the ISO Country model are in the sections below.

In summary, going forward: The `BLC_COUNTRY` and `BLC_COUNTRY_SUB` tables should only be used for filtering and look up purposes only and since the State entity is deprecated, `BLC_STATE` should no longer be used.


## i18n Addresses
The Address domain has been refactored to allow for better internationalization.
In `BLC_ADDRESS`, the `state_prov_region` and the `country` columns have been deprecated.
The foreign key for `country` has also been dropped. The Country entity should
only be used for filtering and lookup purposes now (see section above). These columns will be replaced with
the following:

- `iso_country_alpha2` - a reference to an ISOCountry (i.e. the ISO 3166-1 alpha-2 code for the country where this address resides) (e.g. US, GB, IN)
- `iso_country_sub` - meant to signify the ISO 3166-2 code for the country subdivision (state/region/province) where this address resides. (e.g. US-TX, GB-LND, IN-DL)
- `sub_state_prov_reg` - a friendly name indicating a countries subdivision, i.e. State, Province, Region etc.. (e.g. Texas, London, Delhi)

In `BLC_CUSTOMER_PAYMENT`, the requirement of `billingAddress` has been removed, making it optional.


## ISO Country
Broadleaf adds a new entity called ISOCountry `BLC_ISO_COUNTRY` that represents the ISO 3166 standard published by the International Organization for Standardization (ISO),
and defines codes for the names of countries, dependent territories, and special areas of geographical interest. All Java and Thymeleaf model references to ```address.getCountry()``` and ```address.country``` must be replaced with ```address.getIsoCountryAlpha2()``` and ```address.isoCountryAlpha2```, respectively. Similarly, all references to ```address.state``` and ```address.state.abbreviation``` must be replaced with ```address.stateProvinceRegion```. 

It is recommended that you load this table with all the alpha-2 codes provided in the
DemoSite SQL script `load_i18n_countries.sql`

## Better integration with Service Providers
These i18n enhancements also allow for better integration with Third Party Service Providers such as Digital Wallets and Payment Gateways.
When address information is managed outside of the system, this model aims to minimize data translation as most integrators conform to the ISO 3166-1 naming standard.
While most providers send back a standardized Country code, not all follow the same rules for Country subdivisions such as states, regions and provinces.
In this case, implementations should populate `iso_country_sub` or `sub_state_prov_reg` (preferably both)
based on business requirements and available data.
