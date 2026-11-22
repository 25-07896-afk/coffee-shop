# Cafe Severino – BeanTrack Management System

**Description:** "Cafe Severino – BeanTrack Management System" is a console-based coffee shop point-of-sale and order management simulator that lets cashiers create orders, browse menu categories, process payments, generate receipts, monitor order statuses, and view daily sales reports[cite: 4, 11, 12, 13]. The program's structure is documented around the four main OOP principles to show how data, behavior, and program flow are organized across the system[cite: 4, 7].

## Object-Oriented Programming Concepts Applied

The following are the detailed documentation of how the OOP principles are applied throughout the program[cite: 4]:

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
```
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
```
The `OrderedItem` class combines a selected `MenuItem` and its quantity[cite: 5]. Its `getSubtotal()` method calculates the subtotal by multiplying the product price by the quantity ordered[cite: 5].

> **Note:** The current implementation uses classes to organize related data, but the variables are not declared `private` and do not use getters and setters[cite: 5]. Adding `private` fields and `public` accessor methods would provide stronger encapsulation[cite: 5].

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
The current program does not directly implement inheritance because none of its classes extends another user-defined class[cite: 5]. Instead, the program uses separate classes to represent different parts of the coffee shop management system[cite: 5]. These classes have their own responsibilities[cite: 5]. `MenuItem` represents available products, `OrderedItem` represents products included in a customer's cart, and `Order` stores the complete customer transaction[cite: 5]. Although the classes are related through object references, this is an example of object composition rather than inheritance[cite: 6].

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
The `newOrder()` method handles the process of creating a customer order[cite: 6]. It uses other methods to collect customer information, select a seating area, display the menu, and add products to the cart[cite: 6]. The internal processes are organized into separate methods, making the program easier to understand and maintain[cite: 6].

```java
double calculateTotal() {
    double amount = 0;
    for (OrderedItem item : items) {
        amount += item.getSubtotal();
    }
    return amount;
}
```
The `calculateTotal()` method hides the detailed computation of the order amount[cite: 6]. Instead of calculating each product subtotal in the main program, the `Order` class provides a method that automatically calculates the total cost of all ordered products[cite: 6].

> **Note:** The current implementation demonstrates method-level abstraction, but it does not use abstract classes or interfaces[cite: 6].

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
The current program does not explicitly implement polymorphism through method overriding or method overloading[cite: 6]. However, it uses an `enum` and a `switch` statement to handle different order status values[cite: 7]. The Daily Sales Report uses a `switch` statement to identify and count orders according to their current status[cite: 7]. Each status has its own corresponding operation[cite: 7]. This demonstrates conditional behavior based on an object's state[cite: 7]. However, it is not technically polymorphism because the program does not use method overriding or overloading[cite: 7]. To implement actual polymorphism, future improvements may introduce separate classes for different payment methods or order types with overridden methods[cite: 7].

## Program Structure

### Main Class
**ROLE:**
- Serves as the main entry point of the program[cite: 7]
- Initializes the coffee shop menu[cite: 7]
- Displays the welcome screen and main menu[cite: 7]
- Handles user selections and calls the appropriate methods based on the selected option[cite: 7]

**RESPONSIBILITIES:**
- `main()`[cite: 8]
- `initializeMenu()`[cite: 8]
- `header()`[cite: 8]
- `displayMainMenu()`[cite: 8]

**RELATIONSHIPS:**
- Uses `MenuItem`, `OrderedItem`, and `Order`[cite: 8]
- Controls the overall program execution[cite: 8]
- Calls different methods to perform coffee shop operations[cite: 8]

### MenuItem Class
**ROLE:** Represents an individual product available in the coffee shop[cite: 8].

**DATA:**
- Product code[cite: 8]
- Product name[cite: 8]
- Product category[cite: 8]
- Product price[cite: 8]

**RESPONSIBILITIES:**
- Store product information[cite: 8]
- Provide product details for menu display[cite: 8]
- Serve as a reference when adding products to an order[cite: 8]

**RELATIONSHIPS:**
- Used by `OrderedItem`[cite: 8]
- Stored in the `menuItems` `LinkedHashMap`[cite: 8]

### OrderedItem Class
**ROLE:** Represents a product selected by a customer[cite: 8].

**DATA:**
- `MenuItem` object[cite: 8]
- Product quantity[cite: 8]

**RESPONSIBILITIES:**
- Store the selected product and quantity[cite: 8]
- Calculate the subtotal using `getSubtotal()`[cite: 8]
- Provide product details for the order summary and receipt[cite: 8]

**RELATIONSHIPS:**
- Has-a `MenuItem`[cite: 9]
- Stored inside an `Order`'s `ArrayList`[cite: 9]

### Order Class
**ROLE:** Stores the complete information of a customer transaction[cite: 9].

**DATA:**
- Order number[cite: 9]
- Customer name[cite: 9]
- Seating area[cite: 9]
- Order date and time[cite: 9]
- Ordered products[cite: 9]
- Total amount[cite: 9]
- Cash received[cite: 9]
- Change[cite: 9]
- Order status[cite: 9]

**RESPONSIBILITIES:**
- `calculateTotal()`[cite: 9]
- `getTotalItems()`[cite: 9]
- Store customer transaction details[cite: 9]
- Track the current order status[cite: 9]

**RELATIONSHIPS:**
- Has-a collection of `OrderedItem` objects[cite: 9]
- Uses the `OrderStatus` enum[cite: 9]
- Stored in the `orders` `ArrayList`[cite: 9]

### OrderStatus Enum
**ROLE:** Defines the available statuses of a customer order[cite: 9].

**VALUES:**
- `PENDING`[cite: 9]
- `PREPARING`[cite: 9]
- `READY`[cite: 9]
- `COMPLETED`[cite: 9]

**RESPONSIBILITIES:**
- Identify the current status of an order[cite: 9, 10]
- Support order monitoring[cite: 10]
- Help generate the daily order status summary[cite: 10]

**RELATIONSHIPS:**
- Used by the `Order` class[cite: 10]
- Used in `viewPendingOrders()`[cite: 10]
- Used in `updateOrderStatus()`[cite: 10]
- Used in `dailySalesReport()`[cite: 10]

### Data Storage
**ROLE:** Stores coffee shop products and customer transactions while the program is running[cite: 10].

**DATA STRUCTURES:**
1. `LinkedHashMap<String, MenuItem> menuItems`
   - Stores available coffee shop products[cite: 10]
   - Uses the product code as the key[cite: 10]
   - Maintains the insertion order of products[cite: 10]
2. `ArrayList<Order> orders`
   - Stores customer transactions[cite: 10]
   - Allows the program to display, search, and update orders[cite: 10]
3. `ArrayList<OrderedItem> items`
   - Stores the products included in each transaction[cite: 10]
4. `ArrayList<OrderedItem> cart`
   - Temporarily stores the customer's selected products before order confirmation[cite: 10]

**RELATIONSHIPS:**
- Uses `MenuItem`, `OrderedItem`, and `Order` objects[cite: 10]
- Provides temporary in-memory data storage[cite: 10]

## How to Run the Program

### PREREQUISITES
1. Before running the program, make sure Java Development Kit (JDK) 8 or later is installed[cite: 10]. Open your terminal and check the installation:[cite: 10]

```bash
java -version
javac -version
```
   - If Java is not installed, download the JDK from: https://www.oracle.com/java/technologies/downloads/[cite: 11]
2. Go to the GitHub repository[cite: 11].
3. Click the green **Code** button and select **Download ZIP**[cite: 11].
4. Extract the ZIP file into a folder on your computer[cite: 11].

### AFTERWARDS, COMPILE THE PROGRAM
5. Open Visual Studio Code or another Java-supported IDE[cite: 11].
6. Select **File**, then choose **Open Folder** and locate the extracted project folder[cite: 11].
7. Open the terminal inside the project folder[cite: 11], compile using `javac Main.java`[cite: 11], and run using `java Main`[cite: 11].

```bash
javac Main.java
java Main
```
8. The Cafe Severino - BeanTrack Management System welcome screen will appear, and the cashier can start using the available features[cite: 11].

### TO NAVIGATE THE PROGRAM

1. **Main Menu**
   - `[1] New Customer Order` — Creates a new customer transaction[cite: 11].
   - `[2] View Coffee Shop Menu` — Displays all available products, categories, and prices[cite: 11].
   - `[3] View All Orders` — Displays all recorded customer transactions[cite: 11].
   - `[4] View Pending Orders` — Displays orders that are not yet completed[cite: 11].
   - `[5] Update Order Status` — Allows the cashier to mark an order as completed[cite: 11].
   - `[6] Search Customer Order` — Searches for an order using its order number or customer name[cite: 11].
   - `[7] Daily Sales Report` — Displays the daily transaction, order status, seating area, and sales summaries[cite: 11].
   - `[8] Exit System` — Terminates the program[cite: 11].

2. **View Coffee Shop Menu**
   - Displays available products under four categories: Hot Coffee, Iced Drinks, Coffee Frappe, and Non-Coffee Frappe[cite: 12].
   - Each product has a unique product code, name, and price[cite: 12].

3. **New Customer Order**
   - Cashier enters the customer's name and selects the seating area (Indoor or Outdoor)[cite: 12].
   - Cashier selects products by entering product codes and specifying quantities[cite: 12].

4. **Order Summary**
   - Displays product code, product name, quantity, subtotal, and total amount[cite: 12].
   - Cashier can add another product, review the order, or remove a selected product[cite: 12].

5. **Payment Processing**
   - Displays total amount, accepts cash received, checks payment sufficiency, and calculates change[cite: 12].
   - Accepts transaction only when cash received is equal to or greater than the total amount[cite: 12].

6. **Official Receipt**
   - Automatically generates an official receipt after successful payment with receipt number, date and time, customer name, seating area, ordered products, quantity and amount, total items, total amount, cash received, change, payment status, and order status[cite: 12, 13].

7. **Order Monitoring**
   - Cashier can view all recorded orders and check their current status (`PENDING`, `PREPARING`, `READY`, `COMPLETED`)[cite: 13].
   - Current update function allows marking an active order as `COMPLETED`[cite: 13].

8. **Search Customer Order**
   - Search by order number or customer name[cite: 13].
   - Displays order details, ordered products, payment information, and current status[cite: 13].

9. **Daily Sales Report**
   - Automatically generates a report for the current date including total transactions, total items sold, pending orders, preparing orders, ready orders, completed orders, indoor orders, outdoor orders, and total daily sales[cite: 13].

## Sample Output

The following are the expected program outputs based on the actual functions implemented in the system[cite: 13]. Screenshots of the running program can be inserted under each section[cite: 13].

1. **Main Menu**
   Displays the available features of the BeanTrack Management System[cite: 13].
   `[Insert screenshot of the Main Menu here]`[cite: 14]

2. **Coffee Shop Menu**
   Displays all available coffee products, categories, product codes, and prices[cite: 14].
   `[Insert screenshot of the Coffee Shop Menu here]`[cite: 14]

3. **New Customer Order**
   Allows the cashier to enter the customer's name and select the seating area[cite: 14].
   `[Insert screenshot of the New Customer Order here]`[cite: 14]

4. **Product Selection**
   Allows the cashier to select products using their assigned product codes and enter the desired quantity[cite: 14].
   `[Insert screenshot of Product Selection here]`[cite: 14]

5. **Order Summary**
   Displays the selected products, quantities, subtotals, and total amount[cite: 14].
   `[Insert screenshot of the Order Summary here]`[cite: 14]

6. **Payment Processing**
   Displays the total amount and accepts the cash received from the customer[cite: 14]. The system also calculates the change[cite: 14].
   `[Insert screenshot of Payment Processing here]`[cite: 14]

7. **Official Receipt**
   Automatically generates a receipt after successful payment[cite: 14].
   `[Insert screenshot of the Official Receipt here]`[cite: 14]

8. **View All Orders**
   Displays the list of customer transactions, including their order numbers, customer names, seating areas, statuses, and total amounts[cite: 14].
   `[Insert screenshot of View All Orders here]`[cite: 14]

9. **Pending Orders**
   Displays all orders that have not yet been marked as completed[cite: 15].
   `[Insert screenshot of Pending Orders here]`[cite: 15]

10. **Update Order Status**
    Allows the cashier to search for an order and mark it as completed[cite: 15].
    `[Insert screenshot of Update Order Status here]`[cite: 15]

11. **Search Customer Order**
    Displays the transaction details when a matching order number or customer name is entered[cite: 15].
    `[Insert screenshot of Search Customer Order here]`[cite: 15]

12. **Daily Sales Report**
    Displays the daily transaction summary, order status summary, seating area summary, and financial summary[cite: 15].
    `[Insert screenshot of Daily Sales Report here]`[cite: 15]

## Author and Acknowledgement

This program, Cafe Severino - BeanTrack Management System, is developed by `Brew Code` from `IT - 2110` in compliance with the final project requirements for the course Object-Oriented Programming[cite: 15].

Lead Programmer:
`Pajalla, Shann Raphael C.`[cite: 15]

Members:
`Andal, Ralf Terrence L.`[cite: 15]
`Panes, Frank Luis A.`[cite: 15]


Special thanks to `Mr. Dominic Miko Valdes` for providing guidance and introducing the programming concepts necessary for developing this project[cite: 15].

## Future Enhancements

Future developers may improve the BeanTrack Management System by integrating a graphical user interface (GUI) to provide a more interactive and user-friendly experience for cashiers and coffee shop staff[cite: 16]. The system may also be connected to a relational database such as MySQL to permanently store customer information, product details, orders, and payment transactions[cite: 16]. This would prevent recorded transactions from being lost whenever the program is closed[cite: 16]. Additional features may include inventory management to monitor available ingredients and product stocks, employee account management, discounts, and different payment methods such as cash, GCash, and credit or debit cards[cite: 16]. Furthermore, the order monitoring feature may be enhanced to allow cashiers to update orders through each stage, from `PENDING` to `PREPARING`, `READY`, and finally `COMPLETED`[cite: 16]. These improvements would help make the system more efficient, reliable, and suitable for actual coffee shop operations[cite: 16].
