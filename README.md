# Grocery Inventory Manager for FreshMart Retail
# Created by: Abhinav
# Submission Date: 2025-07-21
# Bootcamp: Python Programming for Analytics

import pandas as pd
from datetime import datetime

# Try to load existing inventory, or create a new one if not found
try:
    inventory = pd.read_csv("inventory.csv")
except FileNotFoundError:
    inventory = pd.DataFrame(columns=["Product", "Category", "Price", "Quantity", "Last_Updated"])

# Function to add a new product
def add_product():
    product = input("Enter Product Name: ")
    category = input("Enter Category: ")
    price = float(input("Enter Unit Price: "))
    quantity = int(input("Enter Initial Quantity: "))
    last_updated = datetime.now().strftime("%Y-%m-%d %H:%M:%S")

    global inventory
    new_entry = pd.DataFrame([[product, category, price, quantity, last_updated]],
                             columns=inventory.columns)
    
    inventory = pd.concat([inventory, new_entry], ignore_index=True)
    print("✅ Product added successfully by Abhinav!")

# Function to update stock
def update_stock():
    product = input("Enter Product to Update: ")
    action = input("Type 'sale' to decrease or 'restock' to increase: ").lower()
    amount = int(input("Enter Quantity: "))

    if product in inventory['Product'].values:
        idx = inventory[inventory['Product'] == product].index[0]
        if action == "sale":
            if inventory.at[idx, "Quantity"] >= amount:
                inventory.at[idx, "Quantity"] -= amount
                print("📉 Sale recorded successfully.")
            else:
                print("⚠ Not enough stock available for sale.")
        elif action == "restock":
            inventory.at[idx, "Quantity"] += amount
            print("📦 Product restocked successfully.")
        else:
            print("❌ Invalid action entered.")
        inventory.at[idx, "Last_Updated"] = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    else:
        print("❌ Product not found in inventory.")

# Function to display inventory
def show_inventory():
    print("\n📦 Current Inventory Status:")
    print(inventory[['Product', 'Quantity']])
    low_stock = inventory[inventory['Quantity'] < 10]
    if not low_stock.empty:
        print("\n🚨 Low Stock Alert (Below 10 units):")
        print(low_stock[['Product', 'Quantity']])

# Function to save inventory
def save_inventory():
    inventory.to_csv("inventory.csv", index=False)
    print("💾 Inventory saved to inventory.csv successfully!")

# Main menu
def main():
    while True:
        print("\n==== Grocery Inventory Manager ====")
        print("1. Add New Product")
        print("2. Update Stock (Sale/Restock)")
        print("3. Show Inventory")
        print("4. Save & Exit")
        choice = input("Enter your choice (1-4): ")

        if choice == '1':
            add_product()
        elif choice == '2':
            update_stock()
        elif choice == '3':
            show_inventory()
        elif choice == '4':
            save_inventory()
            print("👋 Thank you for using the Inventory Manager. Goodbye!")
            break
        else:
            print("❌ Invalid choice. Please try again.")

if _name_ == "_main_":
    main()
# python
