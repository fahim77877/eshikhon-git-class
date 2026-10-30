
# Community Food Bank Management System


# Family Class
class Family:

    def __init__(self, family_id, name, members):
        self.family_id = family_id
        self.name = name
        self.members = members


# FoodBank Class
class FoodBank:

    def __init__(self):
        self.families = {}       # Dictionary
        self.family_ids = set()  # Set
        self.history = []        # List
        self.stock = 0
        self.total_distributed = 0

    # Register Family
    def register_family(self):

        family_id = input("Family ID: ").strip()

        if family_id == "":
            print("Error! Family ID cannot be empty.")
            return

        if family_id in self.family_ids:
            print("Error! Family ID already exists.")
            return

        name = input("Family Name: ").strip()

        if name == "":
            print("Error! Family name cannot be empty.")
            return

        try:
            members = int(input("Number of Members: "))

            if members <= 0:
                print("Error! Members must be greater than 0.")
                return

        except ValueError:
            print("Error! Please enter a valid number.")
            return

        family = Family(family_id, name, members)

        self.families[family_id] = family
        self.family_ids.add(family_id)

        print("Family registered successfully!")

    # Add Food Packs
    def add_food(self):

        try:
            packs = int(input("Packs to add: "))

            if packs <= 0:
                print("Error! Packs must be greater than 0.")
                return

        except ValueError:
            print("Error! Please enter a valid number.")
            return

        self.stock += packs

        print("Food added successfully!")
        print("Current stock:", self.stock)

    # Distribute Food Packs
    def distribute_food(self):

        family_id = input("Family ID: ").strip()

        if family_id not in self.families:
            print("Error! Family not found.")
            return

        try:
            packs = int(input("Packs requested: "))

            if packs <= 0:
                print("Error! Packs must be greater than 0.")
                return

        except ValueError:
            print("Error! Please enter a valid number.")
            return

        if packs > self.stock:
            print("Not enough food!")
            print("Remaining stock:", self.stock)
            return

        self.stock -= packs
        self.total_distributed += packs

        family = self.families[family_id]

        record = (family_id, family.name, packs)  # Tuple
        self.history.append(record)

        print("Food distributed successfully!")
        print("Remaining stock:", self.stock)

    # Show Report
    def show_report(self):

        print("\n===== FOOD BANK REPORT =====")

        print("Available Packs:", self.stock)

        print("\nRegistered Families:")

        if len(self.families) == 0:
            print("No families registered.")

        else:
            for family in self.families.values():
                print(
                    family.family_id,
                    "-",
                    family.name,
                    "-",
                    family.members,
                    "members"
                )

        print("\nDistribution History:")

        if len(self.history) == 0:
            print("No food distributed yet.")

        else:
            for record in self.history:
                print(record[0], "received", record[2], "packs")

        print("\nTotal Distributed:", self.total_distributed)


# Main Program
food_bank = FoodBank()

while True:

    print("\n===== COMMUNITY FOOD BANK =====")
    print("1. Register Family")
    print("2. Add Food Packs")
    print("3. Distribute Food Packs")
    print("4. Show Report")
    print("5. Exit")

    choice = input("Enter choice: ").strip()

    if choice == "1":
        food_bank.register_family()

    elif choice == "2":
        food_bank.add_food()

    elif choice == "3":
        food_bank.distribute_food()

    elif choice == "4":
        food_bank.show_report()

    elif choice == "5":
        print("Thank you for using Community Food Bank!")
        break

    else:
        print("Invalid choice! Please select 1 to 5.")
