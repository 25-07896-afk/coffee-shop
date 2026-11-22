# BeanTrack Management System

Technical Project Documentation & README Specification

---

## Object-Oriented Programming (OOP) Principles

The following are the detailed documentation of how the OOP principles are applied throughout the program:[cite: 4]

### Encapsulation

```java
static class MenuItem {
    String code;
    String name;
    String category;
    double price;

    MenuItem(String code, String name, String category, double price) {
        this.code = code;
        this.name = name;
        this.category = category;
        this.price = price;
    }
}
```[cite: 4]

The `MenuItem` class represents each product available in the coffee shop[cite: 4]. It groups related information such as product code, name, category, and price into one class[cite: 4]. However, its variables currently use default access modifiers rather than `private` access modifiers[cite: 4]. Therefore, full encapsulation is not yet implemented[cite: 4].

```java
static class Order {
    String orderNumber;
    String customerName;
    String seatingArea;
    LocalDateTime orderDate;
    ArrayList<OrderedItem> items = new ArrayList<>();
    double total;
    double cash;
    double change;
    OrderStatus status = OrderStatus.PENDING;
}
```[cite: 4]

The `Order` class groups important customer transaction information, including the order number, customer name, seating area, date, ordered products, payment, and order status[cite: 4]. These variables represent the information associated with each customer transaction[cite: 4].

```java
static class OrderedItem {
    MenuItem item;
    int quantity;

    OrderedItem(MenuItem item, int quantity) {
        this.item = item;
        this.quantity = quantity;
    }

    double getSubtotal() {
        return item.price * quantity;
    }
}
```[cite: 5]

The `OrderedItem` class combines a selected `MenuItem` and its quantity[cite: 5]. Its `getSubtotal()` method calculates the subtotal by multiplying the product price by the quantity ordered[cite: 5].

> **Note:** The current implementation uses classes to organize related data, but the variables are not declared `private` and do not use getters and setters[cite: 5]. Adding `private` fields and `public` accessor methods would provide stronger encapsulation[cite: 5].

### Inheritance

The current program does not directly implement inheritance because none of its classes extends another user-defined class[cite: 5]. Instead, the program uses separate classes to represent different parts of the coffee shop management system[cite: 5].

```java
static class MenuItem {
    String code;
    String name;
    String category;
    double price;
}

static class OrderedItem {
    MenuItem item;
    int quantity;
}

static class Order {
    String orderNumber;
    String customerName;
    String seatingArea;
    ArrayList<OrderedItem> items = new ArrayList<>();
}
```[cite: 5]

These classes have their own responsibilities[cite: 5]. `MenuItem` represents available products, `OrderedItem` represents products included in a customer's cart, and `Order` stores the complete customer transaction[cite: 5]. Although the classes are related through object references, this is an example of object composition rather than inheritance[cite: 6].

### Abstraction

The program applies basic abstraction by organizing specific operations into methods[cite: 6]. Each method performs a particular task while hiding the internal steps from the main program[cite: 6].

```java
static void newOrder() {
    String customerName = getRequiredText("Customer Name: ");
    String seatingArea = selectSeatingArea();
    ArrayList<OrderedItem> cart = new ArrayList<>();
    displayMenu();
    addToCart(cart);
    // Additional order processing
}
```[cite: 6]

The `newOrder()` method handles the process of creating a customer order[cite: 6]. It uses other methods to collect customer information, select a seating area, display the menu, and add products to the cart[cite: 6]. The internal processes are organized into separate methods, making the program easier to understand and maintain[cite: 6].

```java
double calculateTotal() {
    double amount = 0;
    for (OrderedItem item : items) {
        amount += item.getSubtotal();
    }
    return amount;
}
```[cite: 6]

The `calculateTotal()` method hides the detailed computation of the order amount[cite: 6]. Instead of calculating each product subtotal in the main program, the `Order` class provides a method that automatically calculates the total cost of all ordered products[cite: 6].

> **Note:** The current implementation demonstrates method-level abstraction, but it does not use abstract classes or interfaces[cite: 6].

### Polymorphism

The current program does not explicitly implement polymorphism through method overriding or method overloading[cite: 6]. However, it uses an `enum` and a `switch` statement to handle different order status values[cite: 7].

```java
enum OrderStatus {
    PENDING, PREPARING, READY, COMPLETED
}

switch (order.status) {
    case PENDING:
        pendingOrders++;
        break;
    case PREPARING:
        preparingOrders++;
        break;
    case READY:
        readyOrders++;
        break;
    case COMPLETED:
        completedOrders++;
        break;
}
```[cite: 7]

The Daily Sales Report uses a `switch` statement to identify and count orders according to their current status[cite: 7]. Each status has its own corresponding operation[cite: 7]. This demonstrates conditional behavior based on an object's state[cite: 7]. However, it is not technically polymorphism because the program does not use method overriding or overloading[cite: 7].

To implement actual polymorphism, future improvements may introduce separate classes for different payment methods or order types with overridden methods[cite: 7].

---

## Program Structure

### Main Class
* **Role:** Serves as the main entry point of the program, initializes the coffee shop menu, displays the welcome screen and main menu, handles user selections, and calls the appropriate methods based on the selected option[cite: 7].
* **Responsibilities:** `main()`, `initializeMenu()`, `header()`, `displayMainMenu()`[cite: 8]
* **Relationships:** Uses `MenuItem`, `OrderedItem`, and `Order`; controls the overall program execution; calls different methods to perform coffee shop operations[cite: 8].

### MenuItem Class
* **Role:** Represents an individual product available in the coffee shop[cite: 8].
* **Data:** Product code, product name, product category, product price[cite: 8].
* **Responsibilities:** Store product information, provide product details for menu display, and serve as a reference when adding products to an order[cite: 8].
* **Relationship:** Used by `OrderedItem`; stored in the `menuItems` `LinkedHashMap`[cite: 8].

### OrderedItem Class
* **Role:** Represents a product selected by a customer[cite: 8].
* **Data:** `MenuItem` object, product quantity[cite: 8].
* **Responsibilities:** Store the selected product and quantity, calculate the subtotal using `getSubtotal()`, and provide product details for the order summary and receipt[cite: 8].
* **Relationship:** Has-a `MenuItem`; stored inside an `Order`'s `ArrayList`[cite: 9].

### Order Class
* **Role:** Stores the complete information of a customer transaction[cite: 9].
* **Data:** Order number, customer name, seating area, order date and time, ordered products, total amount, cash received, change, order status[cite: 9].
* **Responsibilities:** `calculateTotal()`, `getTotalItems()`, store customer transaction details, and track the current order status[cite: 9].
* **Relationship:** Has-a collection of `OrderedItem` objects; uses the `OrderStatus` enum; stored in the `orders` `ArrayList`[cite: 9].

### OrderStatus Enum
* **Role:** Defines the available statuses of a customer order[cite: 9].
* **Values:** `PENDING`, `PREPARING`, `READY`, `COMPLETED`[cite: 9].
* **Responsibilities:** Identify the current status of an order, support order monitoring, and help generate the daily order status summary[cite: 9, 10].
* **Relationship:** Used by the `Order` class, `viewPendingOrders()`, `updateOrderStatus()`, and `dailySalesReport()`[cite: 10].

### Data Storage
* **Role:** Stores coffee shop products and customer transactions while the program is running[cite: 10].
* **Data Structures:**
  1. `LinkedHashMap<String, MenuItem> menuItems` — Stores available coffee shop products, uses the product code as the key, and maintains the insertion order of products[cite: 10].
  2. `ArrayList<Order> orders` — Stores customer transactions and allows the program to display, search, and update orders[cite: 10].
  3. `ArrayList<OrderedItem> items` — Stores the products included in each transaction[cite: 10].
  4. `ArrayList<OrderedItem> cart` — Temporarily stores the customer's selected products before order confirmation[cite: 10].
* **Relationship:** Uses `MenuItem`, `OrderedItem`, and `Order` objects; provides temporary in-memory data storage[cite: 10].

---

## How to Run the Program

### Prerequisites
1. Before running the program, make sure Java Development Kit (JDK) 8 or later is installed[cite: 10]. Open your terminal and check the installation:[cite: 10]
   ```bash
   java -version
   javac -version
   ```[cite: 10]
2. If Java is not installed, download the JDK from: https://www.oracle.com/java/technologies/downloads/[cite: 11]
3. Go to the GitHub repository[cite: 11].
4. Click the green **Code** button and select **Download ZIP**[cite: 11].
5. Extract the ZIP file into a folder on your computer[cite: 11].
6. Open Visual Studio Code or another Java-supported IDE[cite: 11].
7. Select **File**, then choose **Open Folder** and locate the extracted project folder[cite: 11].

### Compile and Run
8. Open the terminal inside the project folder[cite: 11].
9. Compile the Java program using:[cite: 11]
   ```bash
   javac Main.java
   ```[cite: 11]
10. Run the program using:[cite: 11]
    ```bash
    java Main
    ```[cite: 11]
11. The **Cafe Severino - BeanTrack Management System** welcome screen will appear, and the cashier can start using the available features[cite: 11].

---

## How to Navigate the Program

### 1. Main Menu
The main menu provides eight available options:[cite: 11]

| Option | Description |
| :--- | :--- |
| `[1] New Customer Order` | Creates a new customer transaction[cite: 11]. |
| `[2] View Coffee Shop Menu` | Displays all available products, categories, and prices[cite: 11]. |
| `[3] View All Orders` | Displays all recorded customer transactions[cite: 11]. |
| `[4] View Pending Orders` | Displays orders that are not yet completed[cite: 11]. |
| `[5] Update Order Status` | Allows the cashier to mark an order as completed[cite: 11]. |
| `[6] Search Customer Order` | Searches for an order using its order number or customer name[cite: 11]. |
| `[7] Daily Sales Report` | Displays the daily transaction, order status, seating area, and sales summaries[cite: 11]. |
| `[8] Exit System` | Terminates the program[cite: 11]. |

### 2. View Coffee Shop Menu
The menu displays the available products under four categories:[cite: 12]
* Hot Coffee[cite: 12]
* Iced Drinks[cite: 12]
* Coffee Frappe[cite: 12]
* Non-Coffee Frappe[cite: 12]

Each product has a unique product code, name, and price[cite: 12].

### 3. New Customer Order
The cashier enters the customer's name and selects the seating area[cite: 12].

Available seating areas:
* Indoor[cite: 12]
* Outdoor[cite: 12]

The cashier can then select products by entering their product codes and specifying the desired quantities[cite: 12].

### 4. Order Summary
After selecting products, the system displays the order summary containing:[cite: 12]
* Product code[cite: 12]
* Product name[cite: 12]
* Quantity[cite: 12]
* Subtotal[cite: 12]
* Total amount[cite: 12]

The cashier can add another product, review the order, or remove a selected product[cite: 12].

### 5. Payment Processing
After confirming the order, the system proceeds to payment:[cite: 12]
* Displays the total amount[cite: 12].
* Accepts the cash received from the customer[cite: 12].
* Checks whether the payment is sufficient[cite: 12].
* Calculates the customer's change[cite: 12].

The system only accepts the transaction when the cash received is equal to or greater than the total amount[cite: 12].

### 6. Official Receipt
After successful payment, the system automatically generates an official receipt containing:[cite: 12]
* Receipt number[cite: 12]
* Transaction date and time[cite: 12]
* Customer name[cite: 12]
* Seating area[cite: 12]
* Ordered products[cite: 12]
* Quantity and amount[cite: 12]
* Total items[cite: 12]
* Total amount[cite: 12]
* Cash received[cite: 13]
* Change[cite: 13]
* Payment status[cite: 13]
* Order status[cite: 13]

### 7. Order Monitoring
The cashier can view all recorded orders and check their current status[cite: 13].

Available order statuses:
* `PENDING`[cite: 13]
* `PREPARING`[cite: 13]
* `READY`[cite: 13]
* `COMPLETED`[cite: 13]

The current update function allows the cashier to mark an active order as `COMPLETED`[cite: 13].

### 8. Search Customer Order
The system allows the cashier to search for a specific customer transaction using:[cite: 13]
* Order number
