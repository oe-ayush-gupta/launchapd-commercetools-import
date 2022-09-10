# CommerceTools Import Utilty (modified readme)

This is the modified Readme for the CommerceTools Import Utility to aid in the import process. Please note that the CSVs undergo significant processing in the utility itself and not just the CommerceTools backend. So not all fields in the CSVs will have a one-to-one corresponding data in the CommerceTools backend, and many fields are for local reference only while creating records in the backend.



## Prerequisites

1. Access to a Commercetools Project and the Merchant Center. If you do not have a Commercetools Project, follow our [Getting started guide](https://docs.commercetools.com/getting-started/initial-setup).

2. [Node.js](https://nodejs.org/en/download/) (version 10 or later) must be installed on your computer.

  

## Setting up your CommerceTools Data project

1. In categories csv, `parentId` field can be used to nest subcategories under categories.

2. Please enter the category slug in snake case as `parent-child-sub-child`. This will help the utility find the appropriate subcategory while importing products.

3. `product-types.json` has configuration for the various custom fields added to the product details. Open the file and edit all the `enum` types as required. Of particular interest are the size and designer fields given in the smaple CSV.

4. Make sure that the extra fields you fill in products.csv adhere to the enum specified in product-types.json. `categories` field should be filled as `Parent>Child>Sub Child` which the utility will lookup in the slugs of the categories csv we provided, using snake case. You can specify multiple categories by separating them with a semicolon.

5. You can use `variantId` field to specify variants of a single product which start from 1 and keep on incrementing. If a product has only 1 variant, it should have variantId 1. Only the first variant needs to have categories and productType field filled in.

6. `baseId` key should be filled in with a random unique number for each product (not each variant). Any number. So each variant of the first product can have baseId 73960 and all the variants of the second product can have baseId 7. Just that baseId for each product should be unique.

7. Prices can be specified for each currency, region and whether its regular price or b2b. For example international price can be specified as `USD 7599`. The German and Italian b2b prices in euros can be specified as `DE-EUR 7050 b2b` and `IT-EUR 7150 b2b`. Multiple prices can be specified by separating them with a semicolon.

8. Prices are also specified in the lowest units of the currency - cents, paise or shillings. For example `USD 7599` means $75.99.

9. The orders are specified as line items. So if an order has milk and eggs it will have two entries with the same orderId - one for the eggs and one for milk. orderId needs to be unique between the orders but shared by all the line items of that order.



## Commands

It is better to run `import:data` to act on the entire data set, which then imports records in a sequence which fulfills foreign key dependencies. For example you need to import categories before importing products, which you need to import before importing orders. You can run individual commands too if you noticed the sequence with which records are imported while running the `import:data` command.

  

1. Clean all existing Project data and import new:

    ```
        npm run start
    ```

2. Clean project data:

    ```
        npm run clean:data
    ```

3. Import Project data:

    ```
        npm run import:data
    ```

4. Clean or import certain data *(e.g. Categories, Products, Customers, etc.)*

    ```
        npm run clean:categories
    ```

    or

    ```
        npm run import:products
    ```
    or

    ```
        npm run import:customers
    ```

  

## Debugging

In case you're unable to find logs, just open the respective script from the `lib` directory and look for the `importItem` function.

For example if you're trying to debug order imports, head to `lib/orders.js` and find the function `importOrders`. There you can add a console log in the catch block to get extensive logs.