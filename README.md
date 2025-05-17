source code 

SupplyChainManagement.py

class Product:
    def __init__(self, pid, name, price):
        self.pid = pid
        self.name = name
        self.price = price

class Supplier:
    def __init__(self, sid, name):
        self.sid = sid
        self.name = name
        self.supplied_products = []

    def supply_product(self, product):
        self.supplied_products.append(product)

class Inventory:
    def __init__(self):
        self.stock = {}

    def add_stock(self, product, quantity):
        if product.pid in self.stock:
            self.stock[product.pid]["quantity"] += quantity
        else:
            self.stock[product.pid] = {"product": product, "quantity": quantity}

    def remove_stock(self, product, quantity):
        if product.pid in self.stock and self.stock[product.pid]["quantity"] >= quantity:
            self.stock[product.pid]["quantity"] -= quantity
            return True
        else:
            print(f"Not enough stock for {product.name}")
            return False

    def view_stock(self):
        for item in self.stock.values():
            print(f"{item['product'].name} - Qty: {item['quantity']}")

class Order:
    def __init__(self, order_id):
        self.order_id = order_id
        self.items = []

    def add_item(self, product, quantity):
        self.items.append((product, quantity))

    def process_order(self, inventory):
        for product, quantity in self.items:
            if not inventory.remove_stock(product, quantity):
                print("Order failed due to insufficient stock.")
                return False
        print("Order processed successfully.")
        return True

# Demo Usage
if __name__ == "__main__":
    # Setup
    p1 = Product(1, "Laptop", 800)
    p2 = Product(2, "Mouse", 20)

    s1 = Supplier(101, "TechSupplier Inc.")
    s1.supply_product(p1)
    s1.supply_product(p2)

    inventory = Inventory()
    inventory.add_stock(p1, 10)
    inventory.add_stock(p2, 100)

    print("\nCurrent Inventory:")
    inventory.view_stock()

    order1 = Order(201)
    order1.add_item(p1, 2)
    order1.add_item(p2, 5)

    print("\nProcessing Order:")
    order1.process_order(inventory)

    print("\nInventory After Order:")
    inventory.view_stock()


