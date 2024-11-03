# Create and Interact with a DynamoDB Table

In this example we'll create a DynamoDB table, populate it with sample data, and then perform some basic data queries using the AWS CLI or CloudShell.

## Step 1: Create a Table

- Go to DynamoDB dashboard and choose **Create table**.
- Set the Table name to `items`, define **Partition key** as `id` (String) and **Sort key** as `created_at` (String).

## Step 2: Populate the Table

- Open your terminal in the directory where the `batch-write.json` file is located.
- Run the following command to insert multiple items into the `items` table:

  ```bash
  aws dynamodb batch-write-item --request-items file://batch-write.json
  ```

  > NOTE: Ensure that the AWS CLI is configured on your system. You can also use AWS CloudShell, which comes preconfigured with the AWS CLI.

- Go to **Explore items** in the DynamoDB Console to view the newly added items in the `items` table.

## Step 3: Scan the Table for Specific Data

- To retrieve items in the `items` table where the `category` is `Wearables`, run:

  ```bash
  aws dynamodb scan \
      --table-name items \
      --filter-expression "category = :cat" \
      --expression-attribute-values '{":cat":{"S":"Wearables"}}'
  ```

- To retrieve items with a `rating` of `4.8` or higher, run:

  ```bash
  aws dynamodb scan \
      --table-name items \
      --filter-expression "rating >= :r" \
      --expression-attribute-values '{":r":{"N":"4.8"}}'
  ```

## Step 4: Query the Table Using a Secondary Index

- To query items based on the category and price attributes, run:

  ```bash
  aws dynamodb query \
      --table-name items \
      --key-condition-expression "category = :cat AND price BETWEEN :price1 AND :price2" \
      --expression-attribute-values '{":cat":{"S":"Electronics"}, ":price1":{"N":"20"}, ":price2":{"N":"30"}}'
  ```

- You will get an error.. `ValidationException...` In order to run a query in DynamoDB, you have to provide hash key of the primary index or secondary index. From the table view in the AWS Console choose **Index** and create a **Global Secondary Index** setting `category` as **partition key** (String) and `price` as **sort key** (Number). Name the index `category-price-index`.

- Run the query using the secondary index:

  ```bash
  aws dynamodb query \
      --table-name items \
      --index-name category-price-index \
      --key-condition-expression "category = :cat AND price BETWEEN :price1 AND :price2" \
      --expression-attribute-values '{":cat":{"S":"Accessories"}, ":price1":{"N":"20"}, ":price2":{"N":"30"}}'
  ```
