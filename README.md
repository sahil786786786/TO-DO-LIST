# TO-DO-LIST
import json
import os

# File to store data
DATA_FILE = "budget_data.json"

def load_data():
    if os.path.exists(DATA_FILE):
        with open(DATA_FILE, 'r') as file:
            return json.load(file)
    else:
        return {"income": 0, "expenses": []}

def save_data(data):
    with open(DATA_FILE, 'w') as file:
        json.dump(data, file)

def add_income(data, amount):
    data["income"] += amount
    save_data(data)

def add_expense(data, category, amount):
    data["expenses"].append({"category": category, "amount": amount})
    save_data(data)

def calculate_budget(data):
    total_expenses = sum(item["amount"] for item in data["expenses"])
    budget = data["income"] - total_expenses
    return budget

def analyze_expenses(data):
    expenses_by_category = {}
    for item in data["expenses"]:
        category = item["category"]
        amount = item["amount"]
        if category in expenses_by_category:
            expenses_by_category[category] += amount
        else:
            expenses_by_category[category] = amount
    return expenses_by_category

def main():
    data = load_data()
    
    while True:
        print("\nBudget Tracker")
        print("1. Add Income")
        print("2. Add Expense")
        print("3. Calculate Remaining Budget")
        print("4. Analyze Expenses")
        print("5. Exit")

        choice = input("Enter your choice: ")

        if choice == "1":
            amount = float(input("Enter income amount: "))
            add_income(data, amount)
            print("Income added successfully!")

        elif choice == "2":
            category = input("Enter expense category: ")
            amount = float(input("Enter expense amount: "))
            add_expense(data, category, amount)
            print("Expense added successfully!")

        elif choice == "3":
            budget = calculate_budget(data)
            print(f"Remaining Budget: ${budget}")

        elif choice == "4":
            expenses_by_category = analyze_expenses(data)
            print("Expense Analysis:")
            for category, amount in expenses_by_category.items():
                print(f"{category}: ${amount}")

        elif choice == "5":
            print("Exiting...")
            break

        else:
            print("Invalid choice. Please choose again.")

if __name__ == "__main__":
    main()
