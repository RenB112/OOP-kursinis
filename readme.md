Creating Files:

python

import csv

# Creating inventory.csv
inventory_data = [
    ["Laptop", 1000.0],
    ["Phone", 500.0]
]

with open("inventory.csv", "w") as file:
    writer = csv.writer(file)
    writer.writerows(inventory_data)

# Creating transactions.csv (empty)
with open("transactions.csv", "w"):
    pass

Explanation:

import csv: This imports the CSV module, which provides functionality to read from and write to CSV files.
inventory_data: This is a list containing information about items (name and price).
with open("inventory.csv", "w") as file: This opens (or creates) a file called inventory.csv in write mode ("w").
writer = csv.writer(file): This creates a CSV writer object.
writer.writerows(inventory_data): This writes the rows of inventory_data to the file.
with open("transactions.csv", "w"): This creates an empty file called transactions.csv.


Polymorphism:

class Item:
    def __init__(self, name, price):
        self.name = name
        self.price = price

    def get_description(self):
        pass

class Laptop(Item):
    def __init__(self, name, price, specs):
        super().__init__(name, price)
        self.specs = specs

    def get_description(self):
        return f"{self.name} Laptop with {', '.join(self.specs)}"

class Phone(Item):
    def __init__(self, name, price, storage):
        super().__init__(name, price)
        self.storage = storage

    def get_description(self):
        return f"{self.storage} {self.name}"

Explanation:

Polymorphism: This is an OOP concept where a single function can operate in different ways based on the object it is acting upon.
class Item: This is a base class representing a generic item.
__init__(self, name, price): This is the constructor that initializes the name and price of the item.
get_description(self): This is a method meant to be overridden in derived classes.
class Laptop(Item): This is a derived class representing a laptop.
__init__(self, name, price, specs): This initializes a laptop with its specifications.
get_description(self): This overrides the base class method to provide a specific description for laptops.
class Phone(Item): This is a derived class representing a phone.
__init__(self, name, price, storage): This initializes a phone with its storage capacity.
get_description(self): This overrides the base class method to provide a specific description for phones.

Abstraction:

from abc import ABC, abstractmethod

class Database(ABC):
    @abstractmethod
    def add_data(self, key, value):
        pass

    @abstractmethod
    def get_data(self, key):
        pass

Explanation:

Abstraction: This is an OOP concept that hides the implementation details and only exposes the necessary functionality.
from abc import ABC, abstractmethod: This imports the Abstract Base Class (ABC) module.
class Database(ABC): This is an abstract class that cannot be instantiated directly.
@abstractmethod: This decorator indicates that the method must be implemented by any subclass.
add_data(self, key, value): An abstract method to add data.
get_data(self, key): An abstract method to retrieve data.

Inheritance and Encapsulation:

class Singleton(Database):
    _instance = None

    def __new__(cls, *args, **kwargs):
        if not cls._instance:
            cls._instance = super().__new__(cls, *args, **kwargs)
        return cls._instance

    def __init__(self):
        self.data = {}

    def add_data(self, key, value):
        self.data[key] = value

    def get_data(self, key):
        return self.data.get(key)

Explanation:

Inheritance: This is an OOP concept where a class inherits properties and methods from another class.
Encapsulation: This is an OOP concept where data and methods are bundled together, restricting direct access to some of the object's components.
class Singleton(Database): This class inherits from Database and implements the singleton pattern.
Singleton Pattern: This ensures that a class has only one instance and provides a global point of access to it.
_instance = None: Class variable to store the single instance.
def __new__(cls, *args, **kwargs): This method controls the creation of new instances.
if not cls._instance: Checks if an instance already exists.
cls._instance = super().__new__(cls, *args, **kwargs): Creates a new instance if it does not exist.
return cls._instance: Returns the single instance.
__init__(self): Initializes the instance with an empty dictionary.
add_data(self, key, value): Adds a key-value pair to the dictionary.
get_data(self, key): Retrieves a value based on the key from the dictionary.

Factory Method:

class ItemFactory:
    @staticmethod
    def create_item(item_type, name, price, *args, **kwargs):
        if item_type == "Laptop":
            return Laptop(name, price, *args, **kwargs)
        elif item_type == "Phone":
            return Phone(name, price, *args, **kwargs)
        else:
            raise ValueError("Invalid item type")

Explanation:

Factory Method: This is a design pattern that provides an interface for creating objects in a superclass but allows subclasses to alter the type of objects that will be created.
class ItemFactory: This class provides a static method to create items.
@staticmethod: This decorator indicates that the method does not depend on instance variables.
create_item(item_type, name, price, *args, **kwargs): This method creates and returns an instance of Laptop or Phone based on item_type.

Reading from File & Writing to File:

class Store:
    def __init__(self, inventory_file, transaction_file):
        self.inventory_file = inventory_file
        self.transaction_file = transaction_file
        self.inventory = self.load_inventory()

    def load_inventory(self):
        inventory = {}
        with open(self.inventory_file, 'r') as file:
            reader = csv.reader(file)
            for row in reader:
                inventory[row[0]] = float(row[1])
        return inventory

    def save_transaction(self, transaction):
        with open(self.transaction_file, 'a', newline='') as file:
            writer = csv.writer(file)
            writer.writerow(transaction)

Explanation:

class Store: This class manages the inventory and transactions.
__init__(self, inventory_file, transaction_file): Initializes the store with file paths and loads the inventory.
load_inventory(self): Reads the inventory from the CSV file and stores it in a dictionary.
save_transaction(self, transaction): Appends a transaction to the CSV file.

Testing:

class TestDatabase(unittest.TestCase):
    def setUp(self):
        self.db = Singleton()

    def test_singleton(self):
        self.assertIs(self.db, Singleton(), "Singleton instances are not the same")

    def test_add_data(self):
        self.db.add_data("1", "Laptop")
        self.assertEqual(self.db.get_data("1"), "Laptop")

Explanation:

Testing: This is the process of evaluating software to ensure it behaves as expected.
import unittest: This imports the unittest module, which provides a framework for writing and running tests.
class TestDatabase(unittest.TestCase): This class contains test cases for the Singleton class.
setUp(self): This method is called before each test. It initializes a Singleton instance.
test_singleton(self): This test checks that the singleton instance is unique.
test_add_data(self): This test checks that data can be added and retrieved from the singleton instance.

Usage:

# Creating Store instance
store = Store("inventory.csv", "transactions.csv")
print("Inventory loaded:", store.inventory)

# Creating item instances
laptop = ItemFactory.create_item("Laptop", "Dell", 1000, ["Intel i5", "8GB RAM", "256GB SSD"])
phone = ItemFactory.create_item("Phone", "iPhone", 500, "128GB")

print(laptop.get_description())
print(phone.get_description())

# Saving a transaction
transaction = ["Dell", 1, 1000.0]  # Item, Quantity, Total Price
store.save_transaction(transaction)
print("Transaction saved:", transaction)

# Running tests
unittest.main(argv=[''], exit=False)


Sure, let's go through the code step by step and explain each part along with relevant terms.

Creating Files
python

import csv

# Creating inventory.csv
inventory_data = [
    ["Laptop", 1000.0],
    ["Phone", 500.0]
]

with open("inventory.csv", "w") as file:
    writer = csv.writer(file)
    writer.writerows(inventory_data)

# Creating transactions.csv (empty)
with open("transactions.csv", "w"):
    pass
Explanation:

import csv: This imports the CSV module, which provides functionality to read from and write to CSV files.
inventory_data: This is a list containing information about items (name and price).
with open("inventory.csv", "w") as file: This opens (or creates) a file called inventory.csv in write mode ("w").
writer = csv.writer(file): This creates a CSV writer object.
writer.writerows(inventory_data): This writes the rows of inventory_data to the file.
with open("transactions.csv", "w"): This creates an empty file called transactions.csv.
Main Code
Polymorphism
python

class Item:
    def __init__(self, name, price):
        self.name = name
        self.price = price

    def get_description(self):
        pass

class Laptop(Item):
    def __init__(self, name, price, specs):
        super().__init__(name, price)
        self.specs = specs

    def get_description(self):
        return f"{self.name} Laptop with {', '.join(self.specs)}"

class Phone(Item):
    def __init__(self, name, price, storage):
        super().__init__(name, price)
        self.storage = storage

    def get_description(self):
        return f"{self.storage} {self.name}"
Explanation:

Polymorphism: This is an OOP concept where a single function can operate in different ways based on the object it is acting upon.
class Item: This is a base class representing a generic item.
__init__(self, name, price): This is the constructor that initializes the name and price of the item.
get_description(self): This is a method meant to be overridden in derived classes.
class Laptop(Item): This is a derived class representing a laptop.
__init__(self, name, price, specs): This initializes a laptop with its specifications.
get_description(self): This overrides the base class method to provide a specific description for laptops.
class Phone(Item): This is a derived class representing a phone.
__init__(self, name, price, storage): This initializes a phone with its storage capacity.
get_description(self): This overrides the base class method to provide a specific description for phones.
Abstraction
python

from abc import ABC, abstractmethod

class Database(ABC):
    @abstractmethod
    def add_data(self, key, value):
        pass

    @abstractmethod
    def get_data(self, key):
        pass
Explanation:

Abstraction: This is an OOP concept that hides the implementation details and only exposes the necessary functionality.
from abc import ABC, abstractmethod: This imports the Abstract Base Class (ABC) module.
class Database(ABC): This is an abstract class that cannot be instantiated directly.
@abstractmethod: This decorator indicates that the method must be implemented by any subclass.
add_data(self, key, value): An abstract method to add data.
get_data(self, key): An abstract method to retrieve data.
Inheritance and Encapsulation
python

class Singleton(Database):
    _instance = None

    def __new__(cls, *args, **kwargs):
        if not cls._instance:
            cls._instance = super().__new__(cls, *args, **kwargs)
        return cls._instance

    def __init__(self):
        self.data = {}

    def add_data(self, key, value):
        self.data[key] = value

    def get_data(self, key):
        return self.data.get(key)
Explanation:

Inheritance: This is an OOP concept where a class inherits properties and methods from another class.
Encapsulation: This is an OOP concept where data and methods are bundled together, restricting direct access to some of the object's components.
class Singleton(Database): This class inherits from Database and implements the singleton pattern.
Singleton Pattern: This ensures that a class has only one instance and provides a global point of access to it.
_instance = None: Class variable to store the single instance.
def __new__(cls, *args, **kwargs): This method controls the creation of new instances.
if not cls._instance: Checks if an instance already exists.
cls._instance = super().__new__(cls, *args, **kwargs): Creates a new instance if it does not exist.
return cls._instance: Returns the single instance.
__init__(self): Initializes the instance with an empty dictionary.
add_data(self, key, value): Adds a key-value pair to the dictionary.
get_data(self, key): Retrieves a value based on the key from the dictionary.
Factory Method
python

class ItemFactory:
    @staticmethod
    def create_item(item_type, name, price, *args, **kwargs):
        if item_type == "Laptop":
            return Laptop(name, price, *args, **kwargs)
        elif item_type == "Phone":
            return Phone(name, price, *args, **kwargs)
        else:
            raise ValueError("Invalid item type")
Explanation:

Factory Method: This is a design pattern that provides an interface for creating objects in a superclass but allows subclasses to alter the type of objects that will be created.
class ItemFactory: This class provides a static method to create items.
@staticmethod: This decorator indicates that the method does not depend on instance variables.
create_item(item_type, name, price, *args, **kwargs): This method creates and returns an instance of Laptop or Phone based on item_type.
Reading from File & Writing to File
python

class Store:
    def __init__(self, inventory_file, transaction_file):
        self.inventory_file = inventory_file
        self.transaction_file = transaction_file
        self.inventory = self.load_inventory()

    def load_inventory(self):
        inventory = {}
        with open(self.inventory_file, 'r') as file:
            reader = csv.reader(file)
            for row in reader:
                inventory[row[0]] = float(row[1])
        return inventory

    def save_transaction(self, transaction):
        with open(self.transaction_file, 'a', newline='') as file:
            writer = csv.writer(file)
            writer.writerow(transaction)
Explanation:

class Store: This class manages the inventory and transactions.
__init__(self, inventory_file, transaction_file): Initializes the store with file paths and loads the inventory.
load_inventory(self): Reads the inventory from the CSV file and stores it in a dictionary.
save_transaction(self, transaction): Appends a transaction to the CSV file.
Testing
python

class TestDatabase(unittest.TestCase):
    def setUp(self):
        self.db = Singleton()

    def test_singleton(self):
        self.assertIs(self.db, Singleton(), "Singleton instances are not the same")

    def test_add_data(self):
        self.db.add_data("1", "Laptop")
        self.assertEqual(self.db.get_data("1"), "Laptop")
Explanation:

Testing: This is the process of evaluating software to ensure it behaves as expected.
import unittest: This imports the unittest module, which provides a framework for writing and running tests.
class TestDatabase(unittest.TestCase): This class contains test cases for the Singleton class.
setUp(self): This method is called before each test. It initializes a Singleton instance.
test_singleton(self): This test checks that the singleton instance is unique.
test_add_data(self): This test checks that data can be added and retrieved from the singleton instance.
Usage
python

# Creating Store instance
store = Store("inventory.csv", "transactions.csv")
print("Inventory loaded:", store.inventory)

# Creating item instances
laptop = ItemFactory.create_item("Laptop", "Dell", 1000, ["Intel i5", "8GB RAM", "256GB SSD"])
phone = ItemFactory.create_item("Phone", "iPhone", 500, "128GB")

print(laptop.get_description())
print(phone.get_description())

# Saving a transaction
transaction = ["Dell", 1, 1000.0]  # Item, Quantity, Total Price
store.save_transaction(transaction)
print("Transaction saved:", transaction)

# Running tests
unittest.main(argv=[''], exit=False)
Explanation:

Creating a Store instance with inventory and transaction files.
Printing the loaded inventory.
Creating item instances (Laptop and Phone) using the ItemFactory.
Printing the descriptions of the created items.
Saving a transaction to the transaction file.
Running the unit tests using unittest.main(), with argv=[''] to avoid interference from command-line arguments and exit=False to prevent unittest from shutting down the interpreter.

Purpose of the Code:
The code is designed to simulate the management of a store's inventory and transactions. It covers several key functionalities:

Managing Inventory: Loading, storing, and retrieving information about items available in the store.
Handling Transactions: Recording sales transactions, including item details, quantities, and total prices.
Object-Oriented Principles: Utilizing OOP principles like polymorphism, abstraction, inheritance, and encapsulation to model store items and database interactions.
Design Patterns: Implementing design patterns such as Singleton and Factory Method to manage instances and create objects.
Testing: Providing a framework for testing the core functionalities to ensure correctness and reliability.
Breakdown of Functionality
Creating Initial Files:

inventory.csv: Contains the initial list of items (e.g., laptops and phones) with their prices.
transactions.csv: An initially empty file to record transactions (sales).
Item Management Using OOP:

Polymorphism: The Item class is a base class for all items. Derived classes like Laptop and Phone provide specific implementations of the get_description method.
Abstraction: The Database abstract class defines an interface for database operations without specifying the implementation. This allows for flexibility in how the database is managed.
Inheritance and Encapsulation: The Singleton class inherits from Database and ensures that only one instance of the database exists. It encapsulates the data storage mechanism, providing controlled access through methods.
Creating Items with Factory Method:

ItemFactory: This class uses a static method to create instances of Laptop or Phone based on the specified type. This encapsulates the creation logic and simplifies the process of adding new item types in the future.
Managing Store Inventory:

Store Class: This class handles loading inventory from a CSV file and saving transactions to another CSV file. It abstracts the file operations and provides a clear interface for interacting with the inventory and transaction data.
Testing with Unit Tests:

TestDatabase: This class uses the unittest framework to verify that the Singleton instance is correctly implemented and that data can be added and retrieved as expected.