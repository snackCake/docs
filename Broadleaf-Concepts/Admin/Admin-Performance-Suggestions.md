# Performance Suggestions

## Startswith for String Filtering

For very large catalogs (1 million+ Skus) filtering in the admin can take a very long time, specifically for String fields. This is partly because the default query is built with wildcards on either side of the search term like:

```sql
SELECT * FROM BLC_SKU WHERE sku.name LIKE "%filter-string%"
```

(such as `%filter-value%`). This does not use indexes very efficiently. By setting the `admin.search.string.onlyStartsWith` property to true in your properties files:

```
admin.search.string.onlyStartsWith=true
```

This modifies the query to instead use a query like `filter-value%`:

```sql
SELECT * FROM BLC_SKU WHERE sku.name LIKE "filter-string%"
```

This modification along with proper indexing has been known to cut filtering time in half.
