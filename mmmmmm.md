import numpy as np
import pandas as pd

class PoultryFarm:
    def __init__(self, farm_name):
        self.farm_name = farm_name
        self.inventory = pd.DataFrame(columns=['Product', 'Quantity', 'Price'])
        self.sales_record = pd.DataFrame(columns=['Date', 'Product', 'Quantity', 'Total'])
        self.profit = 0

    def add_to_inventory(self, product, quantity, price):
        new_item = pd.DataFrame([[product, quantity, price]], columns=['Product', 'Quantity', 'Price'])
        self.inventory = pd.concat([self.inventory, new_item], ignore_index=True)
        print(f"{quantity} units of {product} added to inventory.")

    def sell_product(self, product, quantity, selling_price):
        if product not in self.inventory['Product'].values:
            print(f"Error: {product} not found in inventory.")
            return

        available_quantity = self.inventory.loc[self.inventory['Product'] == product, 'Quantity'].values[0]

        if available_quantity < quantity:
            print(f"Error: Insufficient stock of {product}. Available: {available_quantity}")
            return

        total_sale = quantity * selling_price
        self.sales_record = pd.concat([self.sales_record,
                                      pd.DataFrame([[pd.Timestamp.now(), product, quantity, total_sale]],
                                                   columns=['Date', 'Product', 'Quantity', 'Total'])],
                                     ignore_index=True)

        self.inventory.loc[self.inventory['Product'] == product, 'Quantity'] -= quantity
        self.profit += total_sale
        print(f"{quantity} units of {product} sold for a total of ${total_sale}. Profit: ${total_sale - (quantity * self.inventory.loc[self.inventory['Product'] == product, 'Price'].values[0])}")

    def display_inventory(self):
        print(f"{self.farm_name} - Current Inventory:")
        print(self.inventory)

    def display_sales_record(self):
        print(f"{self.farm_name} - Sales Record:")
        print(self.sales_record)

    def display_profit(self):
        print(f"{self.farm_name} - Total Profit: ${self.profit}")


# Example Usage:
farm = PoultryFarm("Chaudhry Poultry Farm")

# Add items to inventory
farm.add_to_inventory('Eggs', 100, 0.2)
farm.add_to_inventory('Chicken', 50, 5.0)

# Display inventory
farm.display_inventory()

# Sell products
farm.sell_product('Eggs', 30, 0.3)
farm.sell_product('Chicken', 10, 7.0)

# Display sales record
farm.display_sales_record()

# Display total profit
farm.display_profit()



```python

```
