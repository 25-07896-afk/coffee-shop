# Cafe Severino – BeanTrack Management System

**Description:** "Cafe Severino – BeanTrack Management System" is a console-based coffee shop point-of-sale and order management simulator that lets cashiers create orders, browse menu categories, process payments, generate receipts, monitor order statuses, and view daily sales reports. The program's structure is documented around the four main OOP principles to show how data, behavior, and program flow are organized across the system.

## Object-Oriented Programming Concepts Applied

The following are the detailed documentation of how the OOP principles are applied throughout the program:

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
```
The `MenuItem` class represents each product available in the coffee shop. It groups related information such as product code, name, category, and price into one class. However, its variables currently use default access modifiers rather than `private` access modifiers. Therefore, full encapsulation is not yet implemented.

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
```
The `Order` class groups important customer transaction information, including the order number, customer name, seating area, date, ordered products, payment, and order status. These variables represent the information associated with each customer transaction.

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
```
The `OrderedItem` class combines a selected `MenuItem` and its quantity. Its `getSubtotal()` method calculates the subtotal by multiplying the product price by the quantity ordered.

> **Note:** The current implementation uses classes to organize related data, but the variables are not declared `private` and do not use getters and setters. Adding `private` fields and `public` accessor methods would provide stronger encapsulation.

### Inheritance

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
```
The current program does not directly implement inheritance because none of its classes extends another user-defined class. Instead, the program uses separate classes to represent different parts of the coffee shop management system. These classes have their own responsibilities. `MenuItem` represents available products, `OrderedItem` represents products included in a customer's cart, and `Order` stores the complete customer transaction. Although the classes are related through object references, this is an example of object composition rather than inheritance.

### Abstraction

```java
static void newOrder() {
    String customerName = getRequiredText("Customer Name: ");
    String seatingArea = selectSeatingArea();
    ArrayList<OrderedItem> cart = new ArrayList<>();
    displayMenu();
    addToCart(cart);
    // Additional order processing
}
```
The `newOrder()` method handles the process of creating a customer order. It uses other methods to collect customer information, select a seating area, display the menu, and add products to the cart. The internal processes are organized into separate methods, making the program easier to understand and maintain.

```java
double calculateTotal() {
    double amount = 0;
    for (OrderedItem item : items) {
        amount += item.getSubtotal();
    }
    return amount;
}
```
The `calculateTotal()` method hides the detailed computation of the order amount. Instead of calculating each product subtotal in the main program, the `Order` class provides a method that automatically calculates the total cost of all ordered products.

> **Note:** The current implementation demonstrates method-level abstraction, but it does not use abstract classes or interfaces.

### Polymorphism

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
```
The current program does not explicitly implement polymorphism through method overriding or method overloading. However, it uses an `enum` and a `switch` statement to handle different order status values. The Daily Sales Report uses a `switch` statement to identify and count orders according to their current status. Each status has its own corresponding operation. This demonstrates conditional behavior based on an object's state. However, it is not technically polymorphism because the program does not use method overriding or overloading. To implement actual polymorphism, future improvements may introduce separate classes for different payment methods or order types with overridden methods.

## Program Structure

### Main Class
**ROLE:**
- Serves as the main entry point of the program
- Initializes the coffee shop menu
- Displays the welcome screen and main menu
- Handles user selections and calls the appropriate methods based on the selected option

**RESPONSIBILITIES:**
- `main()`
- `initializeMenu()`
- `header()`
- `displayMainMenu()`

**RELATIONSHIPS:**
- Uses `MenuItem`, `OrderedItem`, and `Order`
- Controls the overall program execution
- Calls different methods to perform coffee shop operations

### MenuItem Class
**ROLE:** Represents an individual product available in the coffee shop

**DATA:**
- Product code
- Product name
- Product category
- Product price

**RESPONSIBILITIES:**
- Store product information
- Provide product details for menu display
- Serve as a reference when adding products to an order

**RELATIONSHIPS:**
- Used by `OrderedItem`
- Stored in the `menuItems` `LinkedHashMap`

### OrderedItem Class
**ROLE:** Represents a product selected by a customer

**DATA:**
- `MenuItem` object
- Product quantity

**RESPONSIBILITIES:**
- Store the selected product and quantity
- Calculate the subtotal using `getSubtotal()`
- Provide product details for the order summary and receipt

**RELATIONSHIPS:**
- Has-a `MenuItem`
- Stored inside an `Order`'s `ArrayList`

### Order Class
**ROLE:** Stores the complete information of a customer transaction

**DATA:**
- Order number
- Customer name
- Seating area
- Order date and time
- Ordered products
- Total amount
- Cash received
- Change
- Order status

**RESPONSIBILITIES:**
- `calculateTotal()`
- `getTotalItems()`
- Store customer transaction details
- Track the current order status

**RELATIONSHIPS:**
- Has-a collection of `OrderedItem` objects
- Uses the `OrderStatus` enum
- Stored in the `orders` `ArrayList`

### OrderStatus Enum
**ROLE:** Defines the available statuses of a customer order

**VALUES:**
- `PENDING`
- `PREPARING`
- `READY`
- `COMPLETED

**RESPONSIBILITIES:**
- Identify the current status of an order
- Support order monitoring
- Help generate the daily order status summary

**RELATIONSHIPS:**
- Used by the `Order` class
- Used in `viewPendingOrders()`
- Used in `updateOrderStatus()`
- Used in `dailySalesReport()`

### Data Storage
**ROLE:** Stores coffee shop products and customer transactions while the program is running

**DATA STRUCTURES:**
1. `LinkedHashMap<String, MenuItem> menuItems`
   - Stores available coffee shop products
   - Uses the product code as the key
   - Maintains the insertion order of products
2. `ArrayList<Order> orders`
   - Stores customer transactions
   - Allows the program to display, search, and update orders
3. `ArrayList<OrderedItem> items`
   - Stores the products included in each transaction
4. `ArrayList<OrderedItem> cart`
   - Temporarily stores the customer's selected products before order confirmation

**RELATIONSHIPS:**
- Uses `MenuItem`, `OrderedItem`, and `Order` objects
- Provides temporary in-memory data storage

## How to Run the Program

### PREREQUISITES
1. Before running the program, make sure Java Development Kit (JDK) 8 or later is installed. Open your terminal and check the installation:

```bash
java -version
javac -version
```
   - If Java is not installed, download the JDK from: https://www.oracle.com/java/technologies/downloads/
2. Go to the GitHub repository
3. Click the green **Code** button and select **Download ZIP**
4. Extract the ZIP file into a folder on your computer.
### AFTERWARDS, COMPILE THE PROGRAM
5. Open Visual Studio Code or another Java-supported IDE.
6. Select **File**, then choose **Open Folder** and locate the extracted project folder.
7. Open the terminal inside the project folder, compile using `javac Main.java`, and run using `java Main`.

```bash
javac Main.java
java Main
```
8. The Cafe Severino - BeanTrack Management System welcome screen will appear, and the cashier can start using the available features.

### TO NAVIGATE THE PROGRAM

1. **Main Menu**
   - `[1] New Customer Order` — Creates a new customer transaction.
   - `[2] View Coffee Shop Menu` — Displays all available products, categories, and prices.
   - `[3] View All Orders` — Displays all recorded customer transactions.
   - `[4] View Pending Orders` — Displays orders that are not yet completed.
   - `[5] Update Order Status` — Allows the cashier to mark an order as completed.
   - `[6] Search Customer Order` — Searches for an order using its order number or customer name2.
   - `[7] Daily Sales Report` — Displays the daily transaction, order status, seating area, and sales summaries.
   - `[8] Exit System` — Terminates the program.

2. **View Coffee Shop Menu**
   - Displays available products under four categories: Hot Coffee, Iced Drinks, Coffee Frappe, and Non-Coffee Frappe.
   - Each product has a unique product code, name, and price.

3. **New Customer Order**
   - Cashier enters the customer's name and selects the seating area (Indoor or Outdoor).
   - Cashier selects products by entering product codes and specifying quantities.

4. **Order Summary**
   - Displays product code, product name, quantity, subtotal, and total amount.
   - Cashier can add another product, review the order, or remove a selected product.

5. **Payment Processing**
   - Displays total amount, accepts cash received, checks payment sufficiency, and calculates change.
   - Accepts transaction only when cash received is equal to or greater than the total amount.

6. **Official Receipt**
   - Automatically generates an official receipt after successful payment with receipt number, date and time, customer name, seating area, ordered products, quantity and amount, total items, total amount, cash received, change, payment status, and order status.

7. **Order Monitoring**
   - Cashier can view all recorded orders and check their current status (`PENDING`, `PREPARING`, `READY`, `COMPLETED`).
   - Current update function allows marking an active order as `COMPLETED`.

8. **Search Customer Order**
   - Search by order number or customer name.
   - Displays order details, ordered products, payment information, and current status.

9. **Daily Sales Report**
   - Automatically generates a report for the current date including total transactions, total items sold, pending orders, preparing orders, ready orders, completed orders, indoor orders, outdoor orders, and total daily sales.

## Sample Output

The following are the expected program outputs based on the actual functions implemented in the system[cite: 13]. Screenshots of the running program can be inserted under each section.

1. **Main Menu**
   Displays the available features of the BeanTrack Management System
   `[<img width="724" height="431" alt="4aea070f-b32a-4909-be35-41aed3c89908" src="https://github.com/user-attachments/assets/41252886-d6b0-4c93-b6d8-9b12399316c5" />
]'

2. **Coffee Shop Menu**
   Displays all available coffee products, categories, product codes, and prices
   `[<img width="738" height="863" alt="265eaba8-7d96-4c4c-a569-a1cfbdb60a85" src="https://github.com/user-attachments/assets/8a4b8f65-9e13-4adf-8270-f65eab6601bb" />
]'

3. **New Customer Order**
   Allows the cashier to enter the customer's name and select the seating area
   `[<img width="764" height="299" alt="a62b0e74-13e2-47ef-bc1b-3d283e6eba83" src="https://github.com/user-attachments/assets/2abfbf2b-dc81-46d3-b20e-2a2792b95349" />
]'

4. **Product Selection**
   Allows the cashier to select products using their assigned product codes and enter the desired quantity
   `[<img width="502" height="643" alt="01368d4d-863e-4931-a411-f094ef8c40d0" src="https://github.com/user-attachments/assets/abd3ea8d-b59b-4baa-8957-2042a7bc7198" />
]'

5. **Order Summary**
   Displays the selected products, quantities, subtotals, and total amount
   `[<img width="768" height="288" alt="97cdf07e-3dd4-49bd-ab39-ce2c160a0958" src="https://github.com/user-attachments/assets/24f4b7b7-e222-4f76-a4ee-6b0cda0b0e4c" />
]'

6. **Payment Processing Receipt**
   Displays the total amount and accepts the cash received from the customer[cite: 14]. receipt after successful payment
   `[<img width="749" height="695" alt="7e64ad4c-bc1e-4543-90bb-6a1127712650" src="https://github.com/user-attachments/assets/5439668d-fda3-4ae1-b792-04635936a112" />
]'


7. **View All Orders**
   Displays the list of customer transactions, including their order numbers, customer names, seating areas, statuses, and total amounts
   `[<img width="834" height="161" alt="405fc52b-758d-4326-a976-8b881189d34a" src="https://github.com/user-attachments/assets/3f7071c2-4f84-445c-b8ee-3762e67dfc3b" />
]'

8. **Pending Orders**
   Displays all orders that have not yet been marked as completed
   `[<img width="474" height="626" alt="c73163c9-58bc-4432-87f3-c71481308e18" src="https://github.com/user-attachments/assets/7cde3fd3-ce49-413a-a192-9cefe7bbfcf5" />
]'

9. **Update Order Status**
    Allows the cashier to search for an order and mark it as completed.
    `[<img width="751" height="611" alt="f009a9a0-bdae-477e-8bc1-27bae83d6005" src="https://github.com/user-attachments/assets/c1aa2c14-8d79-4f5b-82a7-b42901622351" />
]'

10. **Search Customer Order**
    Displays the transaction details when a matching order number or customer name is entered.
    `[<img width="800" height="186" alt="e720ec38-6dc2-42b9-b308-f106f7886884" src="https://github.com/user-attachments/assets/cf532cf2-1563-4519-b4c5-76af3f9c7504" />
]'

11. **Daily Sales Report**
    Displays the daily transaction summary, order status summary, seating area summary, and financial summary.
    `[<img width="751" height="611" alt="f009a9a0-bdae-477e-8bc1-27bae83d6005" src="https://github.com/user-attachments/assets/8b947bab-0460-40bf-9949-02abd47788d6" />
]'
## Author and Acknowledgement

This program, Cafe Severino - BeanTrack Management System, is developed by `Brew Code` from `IT - 2110` in compliance with the final project requirements for the course Object-Oriented Programming

Lead Programmer:
`Pajalla, Shann Raphael C.`

Members:
`Andal, Ralf Terrence L.`
`Panes, Frank Luis A.`


Special thanks to `Mr. Dominic Miko Valdes` for providing guidance and introducing the programming concepts necessary for developing this project.

## Future Enhancements

Future developers may improve the BeanTrack Management System by integrating a graphical user interface (GUI) to provide a more interactive and user-friendly experience for cashiers and coffee shop staff. The system may also be connected to a relational database such as MySQL to permanently store customer information, product details, orders, and payment transactions. This would prevent recorded transactions from being lost whenever the program is closed. Additional features may include inventory management to monitor available ingredients and product stocks, employee account management, discounts, and different payment methods such as cash, GCash, and credit or debit cards. Furthermore, the order monitoring feature may be enhanced to allow cashiers to update orders through each stage, from `PENDING` to `PREPARING`, `READY`, and finally `COMPLETED`. These improvements would help make the system more efficient, reliable, and suitable for actual coffee shop operations.
