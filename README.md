Object-Oriented Programming (OOP) Principles
The following are the detailed documentation of how the OOP principles are applied throughout the
program:

Encapsulation
    static Scanner sc = new Scanner(System.in);

    enum OrderStatus {
        PENDING, PREPARING, READY, COMPLETED
    }

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

        Order(String orderNumber, String customerName, String seatingArea) {
            this.orderNumber = orderNumber;
            this.customerName = customerName;
            this.seatingArea = seatingArea;
            this.orderDate = LocalDateTime.now();
        }

        double calculateTotal() {
            double amount = 0;

            for (OrderedItem item : items) {
                amount += item.getSubtotal();
            }

            return amount;
        }

        int getTotalItems() {
            int quantity = 0;

            for (OrderedItem item : items) {
                quantity += item.quantity;
            }

            return quantity;
        }
    }

    // DATA STORAGE
    static LinkedHashMap<String, MenuItem> menuItems =
            new LinkedHashMap<>();

    static ArrayList<Order> orders = new ArrayList<>();

    static int orderCounter = 1001;

    static DateTimeFormatter dateFormat =
            DateTimeFormatter.ofPattern("MMMM dd, yyyy");

    static DateTimeFormatter timeFormat =
            DateTimeFormatter.ofPattern("hh:mm a");

    // INITIALIZE MENU
    static void initializeMenu() {

        addItem("HA", "Hot Americano", "HOT COFFEE", 110);
        addItem("HCL", "Hot Cafe Latte", "HOT COFFEE", 120);
        addItem("HSL", "Hot Spanish Latte", "HOT COFFEE", 140);
        addItem("HC", "Hot Cappuccino", "HOT COFFEE", 120);
        addItem("HCM", "Hot Caramel Macchiato", "HOT COFFEE", 140);
        addItem("HM", "Hot Matcha", "HOT COFFEE", 140);

        addItem("IA", "Iced Americano", "ICED DRINKS", 120);
        addItem("ICL", "Iced Cafe Latte", "ICED DRINKS", 130);
        addItem("ISL", "Iced Spanish Latte", "ICED DRINKS", 160);
        addItem("SSL", "Sea Salt Latte", "ICED DRINKS", 160);
        addItem("BL", "Biscoff Latte", "ICED DRINKS", 160);
        addItem("IWM", "Iced White Mocha", "ICED DRINKS", 160);

        addItem("JC", "Java Chip Frappe", "COFFEE FRAPPE", 180);
        addItem("CF", "Caramel Frappe", "COFFEE FRAPPE", 180);
        addItem("CJ", "Coffee Jelly Frappe", "COFFEE FRAPPE", 180);
        addItem("MM", "Midnight Mocha Frappe", "COFFEE FRAPPE", 180);
        addItem("OC", "Oreo Cheesecake Frappe", "COFFEE FRAPPE", 190);

        addItem("CC", "Chocolate Chip Frappe", "NON-COFFEE FRAPPE", 170);
        addItem("TC", "Triple Chocolate Frappe", "NON-COFFEE FRAPPE", 170);
        addItem("BF", "Blueberry Frappe", "NON-COFFEE FRAPPE", 170);
        addItem("SF", "Strawberry Frappe", "NON-COFFEE FRAPPE", 170);
        addItem("OCN", "Oreo Cheesecake Non-Coffee", "NON-COFFEE FRAPPE", 190);
    }

    static void addItem(String code, String name,
                        String category, double price) {

        menuItems.put(code,
                new MenuItem(code, name, category, price));
    }

    // HEADER
    static void header() {

        System.out.println("\n============================================================");
        System.out.println("                 CAFE SEVERINO");
        System.out.println("          BEANTRACK MANAGEMENT SYSTEM");
        System.out.println("============================================================");
        System.out.println("                  Welcome, Cashier!");
    }

    // MAIN MENU
    static void displayMainMenu() {

        System.out.println("\n============================================================");
        System.out.println("                       MAIN MENU");
        System.out.println("============================================================");

        System.out.println("[1] New Customer Order");
        System.out.println("[2] View Coffee Shop Menu");
        System.out.println("[3] View All Orders");
        System.out.println("[4] View Pending Orders");
        System.out.println("[5] Update Order Status");
        System.out.println("[6] Search Customer Order");
        System.out.println("[7] Daily Sales Report");
        System.out.println("[8] Exit System");

        System.out.println("============================================================");
    }

    // INTEGER INPUT VALIDATION
    static int getInteger(String prompt, int min, int max) {

        while (true) {

            System.out.print(prompt);
            String input = sc.nextLine().trim();

            try {

                int value = Integer.parseInt(input);

                if (value >= min && value <= max) {
                    return value;
                }

                System.out.println("Invalid selection. Please try again.");

            } catch (NumberFormatException e) {

                System.out.println("Please enter a valid number.");
            }
        }
    }

    // CASH INPUT VALIDATION
    static double getCash(String prompt) {

        while (true) {

            System.out.print(prompt);
            String input = sc.nextLine().trim();

            try {

                double value = Double.parseDouble(input);

                if (Double.isFinite(value) && value >= 0) {
                    return value;
                }

                System.out.println("Please enter a valid amount.");

            } catch (NumberFormatException e) {

                System.out.println("Invalid amount. Please try again.");
            }
        }
    }

    // REQUIRED TEXT INPUT
    static String getRequiredText(String prompt) {

        while (true) {

            System.out.print(prompt);
            String value = sc.nextLine().trim();

            if (!value.isEmpty()) {
                return value;
            }

            System.out.println("This field cannot be empty.");
        }
    }

    // DISPLAY COFFEE SHOP MENU
    static void displayMenu() {

        System.out.println("\n====================== COFFEE SHOP MENU ====================");

        String currentCategory = "";

        for (MenuItem item : menuItems.values()) {

            if (!item.category.equals(currentCategory)) {

                currentCategory = item.category;

                System.out.println("\n[" + currentCategory + "]");
                System.out.printf("%-10s %-38s %s%n",
                        "CODE", "PRODUCT NAME", "PRICE");

                System.out.println("------------------------------------------------------------");
            }

            System.out.printf("%-10s %-38s PHP %7.2f%n",
                    item.code, item.name, item.price);
        }

        System.out.println("\n============================================================");
    }

    // SELECT SEATING AREA
    static String selectSeatingArea() {

        System.out.println("\nSELECT SEATING AREA");
        System.out.println("[1] Indoor");
        System.out.println("[2] Outdoor");

        int choice = getInteger("Enter your choice: ", 1, 2);

        return choice == 1 ? "Indoor" : "Outdoor";
    }

    // DISPLAY ORDER SUMMARY
    static void displayCart(ArrayList<OrderedItem> cart) {

        System.out.println("\n======================== ORDER SUMMARY =====================");

        System.out.printf("%-6s %-30s %5s %12s%n",
                "CODE", "PRODUCT NAME", "QTY", "SUBTOTAL");

        System.out.println("------------------------------------------------------------");

        double total = 0;

        for (OrderedItem item : cart) {

            System.out.printf("%-6s %-30s %5d PHP %9.2f%n",
                    item.item.code,
                    item.item.name,
                    item.quantity,
                    item.getSubtotal());

            total += item.getSubtotal();
        }

        System.out.println("------------------------------------------------------------");
        System.out.printf("%-43s PHP %9.2f%n", "TOTAL AMOUNT:", total);
        System.out.println("============================================================");
    }

    // ADD PRODUCT TO CART
    static void addToCart(ArrayList<OrderedItem> cart) {

        while (true) {

            System.out.print("\nEnter Product Code: ");

            String code = sc.nextLine().trim().toUpperCase();

            if (code.equals("X")) {
                return;
            }

            MenuItem selected = menuItems.get(code);

            if (selected == null) {

                System.out.println("Invalid product code.");
                continue;
            }

            System.out.println("\nSelected Product: " + selected.name);
            System.out.printf("Price: PHP %.2f%n", selected.price);

            int quantity = getInteger("Enter Quantity: ", 1, 100);

            OrderedItem existing = null;

            for (OrderedItem item : cart) {

                if (item.item.code.equals(code)) {
                    existing = item;
                    break;
                }
            }

            if (existing != null) {

                existing.quantity += quantity;

            } else {

                cart.add(new OrderedItem(selected, quantity));
            }

            System.out.println("\nProduct successfully added!");

            System.out.println("\n[1] Add Another Product");
            System.out.println("[2] Review Order");
            System.out.println("[3] Remove Product");

            int choice = getInteger("Enter choice: ", 1, 3);

            if (choice == 2) {
                return;
            }

            if (choice == 3) {
                removeCartItem(cart);
            }
        }
    }

    // REMOVE PRODUCT FROM CART
    static void removeCartItem(ArrayList<OrderedItem> cart) {

        if (cart.isEmpty()) {
            System.out.println("Your cart is empty.");
            return;
        }

        displayCart(cart);

        String code = getRequiredText(
                "\nEnter Product Code to Remove: "
        ).toUpperCase();

        boolean removed = cart.removeIf(
                item -> item.item.code.equals(code)
        );

        if (removed) {
            System.out.println("Product successfully removed.");
        } else {
            System.out.println("Product not found in cart.");
        }
    }

    // CREATE NEW ORDER
    static void newOrder() {

        System.out.println("\n====================== NEW CUSTOMER ORDER =================");

        String customerName = getRequiredText("Customer Name: ");

        String seatingArea = selectSeatingArea();

        System.out.println("\nCustomer: " + customerName);
        System.out.println("Seating Area: " + seatingArea);

        ArrayList<OrderedItem> cart = new ArrayList<>();

        displayMenu();

        addToCart(cart);

        while (cart.isEmpty()) {

            System.out.println("Your order is empty. Please add a product.");

            addToCart(cart);
        }

        // ORDER CONFIRMATION
        System.out.println("\nFINAL ORDER REVIEW");

        displayCart(cart);

        System.out.println("[1] Confirm Order");
        System.out.println("[2] Cancel Order");

        int confirmation = getInteger("Enter choice: ", 1, 2);

        if (confirmation == 2) {

            System.out.println("Order cancelled.");
            return;
        }

        // CREATE TRANSACTION
        Order order = new Order(
                String.format("ORD-%04d", orderCounter++),
                customerName,
                seatingArea
        );

        order.items.addAll(cart);
        order.total = order.calculateTotal();

        // PAYMENT PROCESSING
        System.out.println("\n====================== PAYMENT =============================");

        System.out.printf("Total Amount: PHP %.2f%n", order.total);

        while (true) {

            order.cash = getCash("Cash Received: PHP ");

            if (order.cash >= order.total) {
                break;
            }

            System.out.println("Insufficient payment.");

            System.out.printf("Remaining Balance: PHP %.2f%n",
                    order.total - order.cash);
        }

        order.change = order.cash - order.total;

        // SAVE TRANSACTION
        orders.add(order);

        // GENERATE RECEIPT
        printReceipt(order);
    }

    // PROFESSIONAL RECEIPT
    static void printReceipt(Order order) {

        System.out.println("\n\n");
        System.out.println("============================================================");
        System.out.println("                       CAFE SEVERINO");
        System.out.println("                  COFFEE & MORE");
        System.out.println("============================================================");

        System.out.println("                    OFFICIAL RECEIPT");
        System.out.println("------------------------------------------------------------");

        System.out.printf("%-20s : %s%n",
                "Receipt No.", order.orderNumber);

        System.out.printf("%-20s : %s%n",
                "Date",
                order.orderDate.format(dateFormat));

        System.out.printf("%-20s : %s%n",
                "Time",
                order.orderDate.format(timeFormat));

        System.out.printf("%-20s : %s%n",
                "Customer",
                order.customerName);

        System.out.printf("%-20s : %s%n",
                "Seating Area",
                order.seatingArea);

        System.out.println("------------------------------------------------------------");

        System.out.printf("%-30s %5s %12s%n",
                "DESCRIPTION", "QTY", "AMOUNT");

        System.out.println("------------------------------------------------------------");

        for (OrderedItem item : order.items) {

            System.out.printf("%-30s %5d PHP %9.2f%n",
                    item.item.name,
                    item.quantity,
                    item.getSubtotal());
        }

        System.out.println("------------------------------------------------------------");

        System.out.printf("%-38s PHP %9.2f%n",
                "TOTAL ITEMS: " + order.getTotalItems(), order.total);

        System.out.println("------------------------------------------------------------");

        System.out.printf("%-38s PHP %9.2f%n",
                "TOTAL AMOUNT:", order.total);

        System.out.printf("%-38s PHP %9.2f%n",
                "CASH RECEIVED:", order.cash);

        System.out.printf("%-38s PHP %9.2f%n",
                "CHANGE:", order.change);

        System.out.println("------------------------------------------------------------");

        System.out.println("Payment Status : PAID");
        System.out.println("Order Status   : " + order.status);

        System.out.println("============================================================");
        System.out.println("              THANK YOU FOR VISITING!");
        System.out.println("           We hope to serve you again.");
        System.out.println("============================================================");
        System.out.println("                END OF RECEIPT");
        System.out.println("\n");
    }

    // VIEW ALL ORDERS
    static void viewAllOrders() {

        System.out.println("\n========================== ALL ORDERS ======================");

        if (orders.isEmpty()) {

            System.out.println("No transactions recorded yet.");
            return;
        }

        System.out.printf("%-13s %-17s %-11s %-13s %s%n",
                "ORDER NO.", "CUSTOMER", "SEATING", "STATUS", "TOTAL");

        System.out.println("------------------------------------------------------------");

        for (Order order : orders) {

            System.out.printf("%-13s %-17s %-11s %-13s PHP %.2f%n",
                    order.orderNumber,
                    order.customerName,
                    order.seatingArea,
                    order.status,
                    order.total);
        }

        System.out.println("============================================================");
    }

    // VIEW PENDING ORDERS
    static void viewPendingOrders() {

        System.out.println("\n========================== ORDER QUEUE =====================");

        boolean found = false;

        for (Order order : orders) {

            if (order.status != OrderStatus.COMPLETED) {

                found = true;

                System.out.println("\nOrder Number : " + order.orderNumber);
                System.out.println("Customer     : " + order.customerName);
                System.out.println("Seating Area : " + order.seatingArea);
                System.out.println("Order Status : " + order.status);

                for (OrderedItem item : order.items) {

                    System.out.printf(" - %s x%d%n",
                            item.item.name, item.quantity);
                }

                System.out.printf("Total        : PHP %.2f%n", order.total);

                System.out.println("------------------------------------------------------------");
            }
        }

        if (!found) {
            System.out.println("No active orders.");
        }
    }

    // FIND ORDER
    static Order findOrder(String orderNumber) {

        for (Order order : orders) {

            if (order.orderNumber.equalsIgnoreCase(orderNumber)) {
                return order;
            }
        }

        return null;
    }

    // UPDATED: MARK ORDER AS COMPLETED
    static void updateOrderStatus() {

        System.out.println("\n====================== UPDATE ORDER STATUS =================");

        // Display active orders
        viewPendingOrders();

        if (orders.isEmpty()) {
            return;
        }

        // Ask for customer order number
        String orderNumber = getRequiredText(
                "\nEnter Customer Order Number: "
        );

        // Search for the order
        Order order = findOrder(orderNumber);

        if (order == null) {

            System.out.println("\nOrder not found.");
            System.out.println("Please check the Order Number and try again.");
            return;
        }

        // Check if already completed
        if (order.status == OrderStatus.COMPLETED) {

            System.out.println("\nThis order is already COMPLETED.");
            return;
        }

        // Display order information
        System.out.println("\n====================== ORDER DETAILS =======================");

        System.out.println("Order Number : " + order.orderNumber);
        System.out.println("Customer     : " + order.customerName);
        System.out.println("Seating Area : " + order.seatingArea);
        System.out.println("Current Status: " + order.status);

        System.out.printf("Total Amount : PHP %.2f%n", order.total);

        System.out.println("------------------------------------------------------------");

        // Ask for confirmation
        System.out.println("\n[1] Mark as Completed");
        System.out.println("[2] Cancel");

        int choice = getInteger("Enter your choice: ", 1, 2);

        if (choice == 1) {

            order.status = OrderStatus.COMPLETED;

            System.out.println("\n============================================================");
            System.out.println("          ORDER SUCCESSFULLY MARKED AS COMPLETED!");
            System.out.println("============================================================");

            System.out.println("Order Number : " + order.orderNumber);
            System.out.println("Customer     : " + order.customerName);
            System.out.println("Order Status : " + order.status);

            System.out.println("============================================================");

        } else {

            System.out.println("\nOrder status update cancelled.");
            System.out.println("Current Status: " + order.status);
        }
    }

    // SEARCH ORDER
    static void searchOrder() {

        System.out.println("\n========================== SEARCH ORDER ====================");

        String keyword = getRequiredText(
                "Enter Order Number or Customer Name: "
        ).toLowerCase();

        boolean found = false;

        for (Order order : orders) {

            if (order.orderNumber.toLowerCase().contains(keyword)
                    || order.customerName.toLowerCase().contains(keyword)) {

                found = true;

                System.out.println("\nOrder Number : " + order.orderNumber);
                System.out.println("Customer     : " + order.customerName);
                System.out.println("Seating Area : " + order.seatingArea);
                System.out.println("Date         : " +
                        order.orderDate.format(dateFormat));
                System.out.println("Status       : " + order.status);

                displayCart(order.items);

                System.out.printf("Cash Received: PHP %.2f%n", order.cash);
                System.out.printf("Change       : PHP %.2f%n", order.change);

                System.out.println("------------------------------------------------------------");
            }
        }

        if (!found) {
            System.out.println("No matching order found.");
        }
    }

    // DAILY SALES REPORT
    static void dailySalesReport() {

        LocalDate today = LocalDate.now();

        double totalSales = 0;

        int totalTransactions = 0;
        int totalItemsSold = 0;

        int indoorOrders = 0;
        int outdoorOrders = 0;

        int pendingOrders = 0;
        int preparingOrders = 0;
        int readyOrders = 0;
        int completedOrders = 0;

        System.out.println("\n========================= DAILY SALES REPORT ===============");

        System.out.println("Business Date: " + today.format(dateFormat));

        System.out.println("------------------------------------------------------------");

        for (Order order : orders) {

            // Only include orders from the current date
            if (order.orderDate.toLocalDate().equals(today)) {

                totalTransactions++;

                totalSales += order.total;

                totalItemsSold += order.getTotalItems();

                if (order.seatingArea.equals("Indoor")) {
                    indoorOrders++;
                } else {
                    outdoorOrders++;
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
            }
        }

        System.out.println("\nTRANSACTION SUMMARY");
        System.out.println("------------------------------------------------------------");

        System.out.printf("Total Transactions : %d%n", totalTransactions);
        System.out.printf("Total Items Sold   : %d%n", totalItemsSold);

        System.out.println("\nORDER STATUS SUMMARY");
        System.out.println("------------------------------------------------------------");

        System.out.printf("Pending Orders     : %d%n", pendingOrders);
        System.out.printf("Preparing Orders   : %d%n", preparingOrders);
        System.out.printf("Ready Orders       : %d%n", readyOrders);
        System.out.printf("Completed Orders   : %d%n", completedOrders);

        System.out.println("\nSEATING AREA SUMMARY");
        System.out.println("------------------------------------------------------------");

        System.out.printf("Indoor Orders      : %d%n", indoorOrders);
        System.out.printf("Outdoor Orders     : %d%n", outdoorOrders);

        System.out.println("\nFINANCIAL SUMMARY");
        System.out.println("------------------------------------------------------------");

        System.out.printf("TOTAL DAILY SALES  : PHP %.2f%n", totalSales);

        System.out.println("============================================================");

        System.out.println("\n              END OF DAILY SALES REPORT");
        System.out.println("============================================================");
    }

    // MAIN METHOD
    public static void main(String[] args) {

        initializeMenu();

        header();

        boolean running = true;

        while (running) {

            displayMainMenu();

            int choice = getInteger("Enter your choice: ", 1, 8);

            switch (choice) {

                case 1:
                    newOrder();
                    break;

                case 2:
                    displayMenu();
                    break;

                case 3:
                    viewAllOrders();
                    break;

                case 4:
                    viewPendingOrders();
                    break;

                case 5:
                    updateOrderStatus();
                    break;

                case 6:
                    searchOrder();
                    break;

                case 7:
                    dailySalesReport();
                    break;

                case 8:
                    System.out.println("\n============================================================");
                    System.out.println("       Thank you for using BeanTrack Management System!");
                    System.out.println("                    Have a great day!");
                    System.out.println("============================================================");

                    running = false;
                    break;
            }
        }

        sc.close();
    }
}
