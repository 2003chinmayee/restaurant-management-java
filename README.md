# Resutaurant Management System (RMS)
## Java Swing:
 is a GUI toolkit in Java for building windows, buttons, lists, text fields, etc. Key classes: JFrame (window), JPanel (container), JButton, JList, JTextField, JLabel.

## GUIs in Java are event-driven:
 you attach ActionListener, MouseListener, etc., to components. When a user clicks a button, that listener’s actionPerformed() runs.

## Important concept:
 Event Dispatch Thread (EDT) — Swing GUI updates must happen on EDT (framework usually handles this when using SwingUtilities.invokeLater or when app is started correctly).

## Typical pattern used here:
 MVC-like separation:

#### Model = data objects (Staff, Manager, MenuItem, OrderDetail).

#### View = UserInterface_GUI (Swing code).

#### Controller = Controller_GUI (mediates between view and model, calls Database).

#### Persistence here is file-based (plain text files), not an RDBMS.

 “It uses an event-driven Swing GUI, a controller layer that handles business logic, and a simple file-based database for storage.”

## Project purpose

A simple but functional restaurant management desktop app supporting:

Roles: Manager (full access) and Staff (limited).

Menu management (add / edit / delete menu items).

Employee management (Manager only).

Order management (create, add items, edit, close, cancel).

Payment tracking, clock-in/out, daily report generation.

## Main Java source files (what they do) — file-by-file
 these short explanations 

### UserInterface_GUI.java

#### Role:
 The Swing GUI (the window you use). Creates JFrame and many Swing components (buttons, lists, panels, dialogs).

#### Contains: 
UI layout code, component declarations (JButton, JList, JLabel, JTextField), and event handlers that call Controller_GUI methods when users click buttons or make selections.

#### Important to mention: 
This is the UI layer (View). It handles displaying data to users and capturing user interactions, then delegates to controller for processing.

### Controller_GUI.java

#### Role:
 Controller for the GUI. Contains methods to handle login, add/edit staff, manage menu items, create/edit orders, clock in/out, and call Database methods to load/save data.

#### How it works:
 GUI calls methods like login(id, pwd), addStaff(...), saveMenuItem(...). Controller_GUI updates in-memory objects and calls Database to persist changes.

### Database.java

#### Role:
 File I/O and persistence. Reads/writes text files located in dataFiles/.

#### What it does:
 Loads files into lists of model objects and writes updated lists back to files. Also creates report files when necessary.

#### Important detail:
 This project uses a plain-text file format — so the Database class contains parsing/serializing logic. (No SQL.)

### Controller.java

#### Role:
 Controller for the console (text) version. Mirrors Controller_GUI logic but for terminal-based interaction.

#### What it supports:
 The menu you saw in the black terminal — login, show menu list, manage orders, etc.

### RMS.java

#### Role:
 Entry point for the console version. It calls controller main loop (console menu).

#### When used:
 If you run java RMS it launches the text-mode interface.

### Employee.java, Manager.java, Staff.java

#### Role:
 Model classes representing people (common fields like ID, name, password), with methods for wages, clock-in/out state, etc.

Manager probably extends Employee or is similar (depends on code specifics).

### MenuItem.java, OrderDetail.java

#### Role:
 MenuItem models items in restaurant menu (id, name, price, category), OrderDetail models ordered item (menu item + qty + line total).

Orders are constructed from OrderDetail objects and persisted by Database.

### Flow when starting the GUI (step-by-step)

You run java -jar RMS_GUI.jar or java UserInterface_GUI.

Java creates the UserInterface_GUI object (a JFrame) which builds all UI components (panels, lists, buttons).

When the user clicks Login, the GUI calls Controller_GUI.login(id, password).

Controller_GUI calls Database to retrieve stored users from dataFiles/.

If login is successful, the controller enables manager features if user is manager.

When user clicks Show Menu or New Order, GUI displays lists by asking controller for data; controller gets data from Database (in-memory lists loaded at startup).

When the user adds/deletes items, the controller updates model lists and calls Database.save...() to write changes back to files.

Closing or canceling an order triggers Database to append report lines (for daily reports).

### How data is stored

Under the dataFiles/ folder you will find text files such as:

staff.txt or similar — stores staff records line by line

menu_item.txt — stores menu item records

reports/ — folder for generated payment/order reports

Each record line uses a delimiter format (e.g., comma or tab), and Database.java contains the parsing code. (So if you must modify, do so through GUI.)

## Execute
Double click RMS_GUI.jar
## Login
You can use test data for the first time. You can add new staff when you log in as manager.
### Manager
- ID:1000 Password:Java
- ID:5555 Password:kazukazu

### Staff
* ID:1111 Password:password
* ID:3333 Password:david  
(Modifing the data file directoly may make problem.)  

## Show menu
You can see all menu items by clicking ALL button, and items in particular categories by clicking Drink, Alcohol, Main, or Dessert button.  
## Taking order
### Create new order
1. Click "Show menu" button on the left
2. Click "New" button to create new order
![](readme_images/order.jpg)
3. Select adding items by clicking from the menu list on the right side.
4. Enter quantity and click "Add" button on the left side.(If quantity is emputy, one item will be added)
5. You can delete ordered item from the order detail by clicking "Delete" button  

### Edit order
1. Click "Show menu" button on the left
2. Select the order from the order list to edit
3. Click "Edit" button
4. You can add, delete ordered items

### Close or Cancel order
1. Select the order from the order list
2. Click "Close" button or "Cancel" button
3. The order closed or canceled can not edit

## Manage Employees (Manager only)
### Add new staff
1. Click "Manage Employees" Button on the left
2. Click "New" button
3. Fill in all information and click OK

###Edit staff
1. Click "Manage Employees" Button on the left
2. Select a staff from the employees list
3. Click "Edit" button
4. Fill in all information and click OK

###Delete staff
1. Click "Manage Employees" Button on the left
2. Select a staff from the employees list
3. Click "Delete" button

##Manage Menu Items (Manager only)
###Add new item
1. Click "Manage menu items" Button on the left
2. Click "Add new menu item" button
3. Fill in all information and click OK

###Edit menu item
1. Click "Manage menu items" Button on the left
2. Select a menu item from the menu list
3. Click "Edit menu item" button
4. Fill in all information and click OK

###Delete menu item
1. Click "Manage menu items" Button on the left
2. Select a menu item from the menu list
3. Click "Delete menu item" button

##About payments
* When you log in, the system automaticaly set start working time.
* Clock out button will set finish working time of the person currently logged in.
* Manager can make staff clocked out via manage employees, by selecting staff and clicking Clock out button.
* You can see a payment details for a day by clicking "Show payment" button on the left 
