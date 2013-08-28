# Pricing Shipping

Broadleaf Commerce can be configured for shipping (fulfillment) calculation in a number of ways. Broadleaf provides a few out of the box implementations such as:

- Banded Price
- Banded Weight
- Fixed Price

While these are the default options available to you, it is simple to configure your own fulfillment pricing implementation or hook in 3rd-party pricing (like UPS, Fedex or USPS).

## Defining Terms
 
**FulfillmentOption** (database) - an option a user can select for Fulfillment at the time of order. If you were to offer next-day, standard, ground and air for users to select that would correspond to 4 unique fulfillment options. Usually the set of FulfillmentOptions are displayed to the user to select from. After selection, the FulfillmentOption is associated with a Fulfillment Group

**FulfillmentType** (enum) - the type of fulfillment such as physical, in-store pickup, digital delivery, etc.

**FulfillmentGroup** (database) - a grouping of `OrderItem`s that are all shipped to the same location with the same option and the same `FulfillmentType`. Consider this scenario:

1. A user adds 4 items to their cart
2. A user wants to get 2 of the items next-day and one of the items with standard shipping. The 4th item is a digital item
3. A user wants to send one of the next-day items and the standard shipping item to Albuquerque, and the other next-day item to Dallas

The final configuration in terms of Fulfillment groups would be:

| Name        | Number of items | Fulfillment Type | Speed | Destination
| ----------- | ------------- | ----------------- | ----- | ----------- |
| Fulfillment Group 1 | 1 | 'PHYSICAL_SHIP' | Standard | Albuquerque |
| Fulfillment Group 2 | 1 | 'PHYSICAL_SHIP' | Next-Day | Albuquerque |
| Fulfillment Group 3 | 1 | 'PHYSICAL_SHIP' | Next-Day | Dallas |
| Fulfillment Group 4 | 1 | 'DIGITAL' | N/A | N/A |

**FulfillmentPricingProvider** (service interface) - interface to implement to provide pricing information for a particular `FulfillmentOption` (for estimation) or `FulfillmentOption` + `FulfillmentGroup` combination.

**FulfillmentPricingService** (Broadleaf service) - contains the list of `FulfillmentPricingProviders`. These are looped through to get a final price for the fulfillment group

## Flat Rates per-Sku
Each Sku has a Map of FulfillmentOption -> Price. Assuming that you have set the `USE_FLAT_RATES` flag in each `FulfillmentOption` to true, the system will utilize the price configured for that Sku instead of trying to price it by any other means.

## Banded Price/Weight
Broadleaf provides abilities to configure weight and price bands to give a certain price. For instance, you might offer free shipping for orders > $100 but $10 shipping on everything below $100.

```sql 
-- Insert the options
INSERT INTO BLC_FULFILLMENT_OPTION (FULFILLMENT_OPTION_ID, NAME, LONG_DESCRIPTION, USE_FLAT_RATES, FULFILLMENT_TYPE) VALUES (1, 'Free Shipping Above $100', 'Free Shipping Above $100', FALSE, 'PHYSICAL_SHIP');
INSERT INTO BLC_FULFILLMENT_OPT_BANDED_PRC(FULFILLMENT_OPTION_ID) VALUES (1);

-- Insert the price bands
INSERT INTO BLC_FULFILLMENT_PRICE_BAND (FULFILLMENT_PRICE_BAND_ID, RETAIL_PRICE_MINIMUM_AMOUNT, FULFILLMENT_OPTION_ID, RESULT_AMOUNT, RESULT_AMOUNT_TYPE) VALUES (1, 0.00, 1, 10.00, 'RATE');
INSERT INTO BLC_FULFILLMENT_PRICE_BAND (FULFILLMENT_PRICE_BAND_ID, RETAIL_PRICE_MINIMUM_AMOUNT, FULFILLMENT_OPTION_ID, RESULT_AMOUNT, RESULT_AMOUNT_TYPE) VALUES (2, 100.00, 1, 0.00, 'RATE');
```

Now, for all `FulfillmentGroup`s with a fulfillment type of PHYSICAL_SHIP and have a retail price below below $100, they will be charged $10 for shipping. Anything over $100 will be charged $0 for shipping.

A very similar database configuration can be used for weight. The following SQL imports charge $10 for orders that weigh 10lbs or over:

```sql 
-- Insert the options
INSERT INTO BLC_FULFILLMENT_OPTION (FULFILLMENT_OPTION_ID, NAME, LONG_DESCRIPTION, USE_FLAT_RATES, FULFILLMENT_TYPE) VALUES (1, 'Free Shipping for orders less than 10lbs', 'Free Shipping for orders less than 10lbs', FALSE, 'PHYSICAL_SHIP');
INSERT INTO BLC_FULFILLMENT_OPT_BANDED_WGT(FULFILLMENT_OPTION_ID) VALUES (1);

-- Insert the weight bands
INSERT INTO BLC_FULFILLMENT_WEIGHT_BAND (FULFILLMENT_PRICE_BAND_ID, MINIMUM_WEIGHT, WEIGHT_UNIT_OF_MEASURE, FULFILLMENT_OPTION_ID, RESULT_AMOUNT, RESULT_AMOUNT_TYPE) VALUES (1, 0.00, 'POUNDS', 1, 0.00, 'RATE');
INSERT INTO BLC_FULFILLMENT_WEIGHT_BAND (FULFILLMENT_PRICE_BAND_ID, MINIMUM_WEIGHT, WEIGHT_UNIT_OF_MEASURE, FULFILLMENT_OPTION_ID, RESULT_AMOUNT, RESULT_AMOUNT_TYPE) VALUES (2, 10.00, 'POUNDS', 1, 10.00, 'RATE');
```


## Fixed Price
This is for scenarios where you would like to offer a flat $5 shipping on all purchases. A potential SQL import for this:

```sql
INSERT INTO BLC_FULFILLMENT_OPTION (FULFILLMENT_OPTION_ID, NAME, LONG_DESCRIPTION, USE_FLAT_RATES, FULFILLMENT_TYPE) VALUES (1, 'Standard', '5 - 7 Days', FALSE, 'PHYSICAL_SHIP');
INSERT INTO BLC_FULFILLMENT_OPTION (FULFILLMENT_OPTION_ID, NAME, LONG_DESCRIPTION, USE_FLAT_RATES, FULFILLMENT_TYPE) VALUES (2, 'Priority', '3 - 5 Days', FALSE, 'PHYSICAL_SHIP');
INSERT INTO BLC_FULFILLMENT_OPTION (FULFILLMENT_OPTION_ID, NAME, LONG_DESCRIPTION, USE_FLAT_RATES, FULFILLMENT_TYPE) VALUES (3, 'Express', '1 - 2 Days', FALSE, 'PHYSICAL_SHIP');

INSERT INTO BLC_FULFILLMENT_OPTION_FIXED (FULFILLMENT_OPTION_ID, PRICE) VALUES (1, 5.00);
INSERT INTO BLC_FULFILLMENT_OPTION_FIXED (FULFILLMENT_OPTION_ID, PRICE) VALUES (2, 10.00);
INSERT INTO BLC_FULFILLMENT_OPTION_FIXED (FULFILLMENT_OPTION_ID, PRICE) VALUES (3, 20.00);
```

This will add 3 options for users to select from at checkout.

## Writing your own fulfillment pricing
Content coming soon. In the mean time, check out the `FulfillmentPricingProvider` interface definition.
