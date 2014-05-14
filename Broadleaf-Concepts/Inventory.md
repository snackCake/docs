# Inventory

Broadleaf's basic inventory functionality provides:

- A `quantityAvailable` field for Skus
- Conditional quantity checks for Skus (inventory type of `CHECK_QUANTITY`, `UNAVAILABLE` or `AVAILABLE`)
- All Skus are available by default
- An activity in the `blAddItemWorfklow` and `blUpdateItemWorkflow` to check if inventory is available for a Sku
- An activity in the `blCheckoutWorkflow` to actually decrement inventory on checkout
- A rollback handler within the `blCheckoutWorkflow` to put inventory back if there was an exception after decrementing inventory

## Enabling Inventory in the Admin

Below shows the screen for managing the quantity of a Sku in the admin along with its inventory type:

![inventory admin](admin-inventory.png)

If the inventory type is anything but 'Check Quantity' then the available quantity is ignored.

You can also inventory type on a per-category basis. For instance, you could leave the inventory type for the Sku unselected but then mark then entire 'Hot Sauces' category as 'Check Quantity':

![category inventory](admin-category-inventory.png)

## Enabling Inventory Management in Broadleaf 3.1.1+

There are a few steps that you need to follow to enable the above functionality. You can find these steps within the [[3.1.0 to 3.1.1 Migration]] doc.

## Advanced Inventory

The Broadleaf advanced inventory module expands upon the basic inventory functionality by providing concepts for:

- Managing inventory for a Sku within different fulfillment locations
- Selecting which fulfillment location to take inventory from
- Optimistic row-locking in the database suitable for high-traffic sites

The advanced inventory module is automatically included in all enterprise licenses.

## Interfacing with 3rd-party Inventory Systems

The starting point for interfacing with 3rd-party systems is in the ^[javadoc:org/broadleafcommerce/core/inventory/service/InventoryServiceExtensionHandler]. If there is an active inventory extension handler then this will intercept all calls to the `ContextualInventoryService` and thus the `InventoryService` interface. This is how you can override all of the framework checks for handling inventory in just one place.

If that override is insufficient for your needs it might be more appropriate to wholesale override the `CheckAvailabilityActivity`, `DecrementInventoryActivity` and `DecrementInventoryRollbackHandler`. If you decide to go this route then you can determine if Skus within an Order are eligible for an inventory check via `sku.getInventoryType()`.
