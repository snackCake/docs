# REST Endpoints

## Request/Response Formats
All of the endpoints below have the ability to produce and consume both **XML** and **JSON**. To ensure that you are receiving JSON in a response, ensure that this request HTTP header is set:

```
Accept: application/json
```

To tell the endpoint that you are sending it JSON (like in a PUT or POST), use this HTTP header:

```
Content-Type: application/json
```

And of course, to ensure that all requests/responses are in JSON, use both of those headers combined:

```
Accept: application/json
Content-Type: application/json
```

## Functionality

The following provides a list of current RESTful endpoints provided with Broadleaf Commerce:

### Catalog

- **/catalog/product/{id}**: GET
    - Returns a representation of a Broadleaf product by its ID

- **/catalog/search/products**: GET
    - Returns a representation of a paginated list of products along with any search facets that may be used to filter the search
    - Query Params:
        - `q` - a query parameter such as product name or keyword(s)
        - `page` - the page to return in a paginated situation (default=1)
        - `pageSize` - the number of records to return per page (default=15)
        - Accepts search facets (see note below)

- **/catalog/search/category/{categoryId}/products**: GET
    - Returns a representation of a paginated list of products within a category, along with any search facets that may be used to filter the search.
    - Query Params:
        - `categoryId` - the category that you wish to search
        - `q` - a query parameter such as product name or keyword(s)
        - `page` - the page to return in a paginated situation (default=1)
        - `pageSize` - the number of records to return per page (default=15)
        - Accepts search facets (see note below)
  
- **/catalog/product/{id}/skus**: GET
    - Returns a list of skus for a particular product
  
- **/catalog/categories**: GET
    - Returns a representation of a paginated list of product categories
    - Query Params:
        - `name`
        - `limit` (default 20)
        - `offset` (default 0)
       
- **/catalog/category/{id}/categories**: GET
    - Returns a list of subcategories for a particular product category
    - Query Params:
        - `active` (default true)
        - `limit` (default 20)
        - `offset` (default 0)
  
- **/catalog/category/{id}/activeSubcategories**: GET
    - Returns a list of active subcategories for a particular product category
    - Query Params:
        - `limit` (default 20)
        - `offset` (default 0)
       
- **/catalog/category/{id}**: GET
    - Returns a representation of a product category, keyed by ID. Parameters allow one to control how much additional, nested data is returned.
    - Query Params:
        - `productLimit` (default 20)
        - `productOffset` (default 0)
        - `subcategoryOffset` (default 0)
        - `subcategoryDepth` (default 1)
      
- **/catalog/product/{id}/related-products/upsale**: GET
    - Returns a list of related upsale products for a particular catalog product
    - Query Params:
        - `limit` (default 20)
        - `offset` (default 0)
  
- **/catalog/product/{id}/related-products/crosssale**: GET
    - Returns a list of related cross sell products for a particular catalog product
    - Query Params:
        - `limit` (default 20)
        - `offset` (default 0)
  
- **/catalog/product/{id}/product-attributes**: GET
    - Returns a list of product attributes for a particular catalog product
  
- **/catalog/sku/{id}/sku-attributes**: GET
    - Returns a list of sku attributes for a particular catalog sku
  
- **/catalog/sku/{id}**: GET
    - Returns a representation of a particular catalog sku
  
- **/catalog/sku/{id}/media**: GET
    - Returns a list of media items for a particular catalog sku
  
- **/catalog/sku/inventory**: GET
    Returns a list of inventory for each Sku ID passed in
    - Query Params:
        - `id` - a list of Sku IDs to get inventory for
      
- **/catalog/product/{id}/media**: GET
    - Returns a list of media items for a particular catalog product
  
- **/catalog/category/{id}/media**: GET
    - Returns a list of media items for a particular product category
  
- **/catalog/product/{id}/categories**: GET
    - Returns a list of categories for a particular product

>NOTE: When a method specifies that it accepts search facets, you may pass search facets in. These are returned in the result of an initial query. Facets may be something like: `price=range[0.00000:15.00000]&price=range[15.00000:30.00000]`. This will add two values to a facet, allowing for a range in price from $0-$30.

### Order

>**NOTE**: The customer ID *must* be passed on each request. It can be passed on a query parameter or a request header. But it must be keyed by `customerId`

- **/cart**: GET
    - Returns a representation of the customer's shopping cart.
  
- **/cart**: POST
    - Creates a new cart for the customer. If the customer ID is unknown because a customer record does not yet exist, it need not be passed in. A new customer will be created. The new cart along with the customer will be returned.
  
- **/cart/{productId}**: POST
    - Adds the sku and its associated category and product references to the shopping cart. Optionally reprices the order. Returns a representation of the cart.
    - Query Params:
        - `categoryId`
        - `quantity` (default 1)
        - `priceOrder` (default true)
  
- **/cart/items/{itemId}**: DELETE
    - Deletes the item from the cart and optionally reprices the order
    - Query Params:
        - `priceOrder` (default true)
      
- **/cart/items/{itemId}**: PUT
    - Updates the quantity of an item and optionally reprices the cart
    - Query Params:
        - `quantity` (required)
        - `priceOrder` (default true)

- **/cart/items/{itemId}/options**: PUT
    - Updates the product options for a cart item
    - URI Info:
        - Contains info about updated options in the format `productOption.myProductOption`, where `myProductOption` is the option key
    - Query Params:
        - `itemId` (required)
        - `priceOrder` (default true)
  
- **/cart/offer**: POST
    - Adds a promotional code to an order
    - Query Params:
        - `promoCode`
        - `priceOrder` (default true)
  
- **/cart/offer**: DELETE
    - Deletes a promotional code from an order
    - Query Params:
        - `promoCode`
        - `priceOrder` (default true)
  
- **/cart/offers**: DELETE
    - Deletes all promotional codes from an order and optionally reprices
    - Query Params:
        - `priceOrder` (default true)
  
- **/cart/fulfillment/groups**: GET
    - Returns a list of fulfillment groups for a particular order
  
- **/cart/fulfillment/groups**: DELETE
    - Deletes all fulfillment groups from an order
    - Query Params:
        - `priceOrder` (default true)
  
- **/cart/fulfillment/group**: POST
    - Adds a new fulfillment group to the order. Accepts a fulfillment group representation in JSON or XML format.
    - Query Params:
        - `priceOrder` (default true)
  
- **/cart/fulfillment/group/{fulfillmentGroupId}**: PUT
    - Updates the fulfillment group identified by the ID in the URI. Accepts a fulfillment group representation in JSON or XML format.
    - Query Params:
        - `priceOrder` (default true)
  
- **/orders**: GET
    - Returns a list of orders. The order history is for the customer who's ID is passed in.
    - Query Params: 
        - `orderStatus` (default SUBMITTED)

### Checkout

- **/cart/checkout/payments**: GET
    - Returns a list of Order Payments on the order.
  
- **/cart/checkout/payment**: POST
    - Adds a new OrderPayment and any associated PaymentTransactions to the Order. Accepts an OrderPayment representation in JSON or XML format.
  
- **/cart/checkout/payment**: DELETE
    - Deletes the OrderPayment that is represented by the payment passed in. Accepts an OrderPayment representation in JSON or XML format.
  
- **/cart/checkout**: POST
    - Once all the payments have been finalized and the order is ready for checkout. This method allows you to complete checkout.
