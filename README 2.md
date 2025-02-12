

# R2 - PresentPals Dataflow Diagrams

## Site Map / Users Access

![Dataflow Diagram](./images/Dataflow%20Diagram_2.png)

## Dataflow & CRUD Processes (incl authentication & authorization)

![DF & CRUD Diagram](./images/DF%20&%20CRUD%20Diagram.png)

# Details
## Register / Login Data
### Admin Users Data:
When a user registers to access the PresentPals application they will become a administrator user by entering the following details:

    - group name,
    - user name,
    - password.

This data will be added to the Users collection in the database creating all the following data fields:

    - groupId [Int, unique],
    - groupname [String, unique],
    - userId [Int, unique],
    - username [String, unique],
    - password [String, hashed].
    - isAdmin [Boolean] (true = administrator).

When a admin user logs into the application their credentials will have access to:

    - Create other users usernames & passwords to log into the application.
    - Delete users.

- Have access to all processes / functions in the application:
    - View the created wishlist(s),
    - Create a wishlist(s),
    - Delete a wishlist(s),
    - Assign users to a wishlist(s),
    - Edit / Assign to buy a item(s) on a wishlist(s),

### Other Users Data:
Upon logging in with a username & password , this will be requested for validation in the Users collection of the database and responded to accept for access, or deny & acknowledged as incorrect details.

The accepted credentials will allow acces to the following:

- User (Child): 
    - Create a wishlist(s),
    - View the created wishlist(s),
    - Edit their profile details.
- Users (Sharer):
    - View the created wishlist(s),
    - Edit / Assign to buy a item(s) on a wishlist(s).

## Home Page Data
This home page is where logged in users can view the wishlists they have been assigned to view.
Once the user selects the wishlist they choose to view, the component will send a request to the wishlists collection in the applications database. The data response from the wishlists collection will be received by the component and rendered in the users browser to view all the details in the wishlist chosen. The Wishlists collection data fields to be responded will be:

    - wishlistName [String],
    - wishlistImage [Image],
    - childUser [String] (child that created wishlist),
    - childItem [String] (added to wishlist by child user),
    - itemPurchased [Boolean] (item marked as purchased by an assigned sharer.Note: child user blocked from viewing this area).
    - purchasedUser [String] (assigned sharer that marked itemPurchased.Note: child user blocked from viewing this area).
    - dateCreated [Date] (date wishlist was created),
    - dateExpired [Date] (rendered as a countdown of days to expiry date!).

## Family & Friends Page Data
The applications area for creating, editing & deleting users profiles will be on this page.
Create users function will add the following data to the Users collection in the database:

    - groupId [Int] (same as currently logged in),
    - userId [Int, unique],
    - username [String, unique],
    - password [String, hashed],
    - firstname [String],
    - lastname [String],
    - email [String],
    - phonenumber [String].

Edit users function will update all of the above data fields.
Delete users function will delete all the above data fields.

## Wishlist Page Data
This page is where application wishlists will be created, edited, deleted, have users assigned and sharer users assigned to purchase items.
The following data fields will be applied to the Wishlists collection in the database from the create wishlist function:

    - wishlistId [Int, unique],
    - wishlistName [String],
    - wishlistImage [Image] (From the external image API),
    - childUser [String] (child that created wishlist),
    - childItem [String] (Item(s) added to wishlist by child user),
    - dateCreated [Date] (date wishlist was created),
    - dateExpired [Date] (date this wishlist will expire).

The following data fields will be applied to assigned wishlistId in the Wishlists collection from the assigned users to wishlist function:

    - assignedUsers [Array],

The following data fields will be applied to the assigned wishlistId in the Wishlists collection from the edit/assign to buy items on wishlist function:

    - itemPurchased [Boolean] (item marked as purchased by an assigned sharer.),
    - purchasedUser [String] (assigned sharer that marked itemPurchased).

The delete wishlist function will delete all these data fields for the selected wishlistId.

# R3 - Application Architecture Diagram

![App Architecture Diagram](./images/Application%20Architecture%20Diagram%202025-02-08.png)

# Details
## User Web Browser
Any users wanting to use the PresentPals application will have to go to PresentPals domain name in their web browser. This will then send HTTP requests to the client side of the application and render the html responses to the users web browser.

## Client Side (Front End)
PresnetPals is using React.js for its ability to create fast, dynamic & interactive user interfaces. It will do this by installing and using a combination of libraries for tasks like:

    - a set of design templates and UI components (Bootstrap),
    - routing (Axios),
    - state management (Redux, Context API).

For the applications code to meet a number of key principles such as:

- The DRY (Don't Repeat Yourself) principle, which is about avoiding duplication of code,
- Modularity refers to breaking down the application into smaller, self-contained pieces (modules), each of which has a specific responsibility. These modules work together to form the complete application but can be developed, tested, and maintained independently.

To achieve this, inside the applications /src folder will be a number of sub folders:

- Components: the Components folder holds the building blocks of our React application’s user interface. These are the reusable UI elements, such as buttons, forms, headers, and entire page layouts.
- Pages: the Pages folder will be used to store page components that represent distinct views or pages in the application. Each page is a high-level component that corresponds to a specific route in the application. These components will contain other smaller components (from the Components folder) that form the complete structure of the page.
- Context: this will be used to organize and manage global state or shared data that needs to be accessed across different parts of the application. In the context of React, it specifically refers to the Context API, which provides a way to share state (or any data) between components without having to pass it explicitly through props at every level.

The client side will be hosted in Netlify.

## Server Side (Back End)
PresentPals is using following technologies here:

- Node.js is a JavaScript runtime built on Chrome's V8 JavaScript engine that allows the execution of JavaScript code on the server side. It enables building scalable backend applications in JavaScript with the following uses:

    - enables you to write server-side code in JavaScript, so both the frontend and backend of your application can use the same language.
    - supports asynchronous programming, which makes it ideal for handling concurrent requests (for instance, handling multiple API calls at once).

- Express.js is a lightweight, flexible, and minimalistic backend framework for Node.js with the following uses:

    - Express simplifies the creation of API routes and handles HTTP requests (GET, POST, PUT, DELETE) to interact with the frontend and database.
    - enables middleware support, which can be used for authentication, logging, validation, etc.
    - it provides a clean and intuitive way to define routes and connect them to functions that handle business logic.

By installing and using a combination of libraries the following tasks will be achieved like:

    - Object Data Modeling (ODM) library for MongoDB and Node.js (Mongoose),
    - password hashing library used to securely hash and compare passwords (Bcrypt),
    - a safe method of authentication and authorization (JWT (JSON Web Token)),
    - to manage environment variables (dotenv).

The server side will be hosted in Render.

## Database Side
The technology for saving PresentPals data will be via a MongoDB database. MongoDB is a NoSQL database management system that stores data in a flexible, document-oriented format.

MongoDB Structure and Terminology:

    - Database: A container for collections. In MongoDB, you can have multiple databases, and each database can have its own collections and documents.
    - Collection: A collection is a group of MongoDB documents (similar to a table in relational databases). A collection doesn’t enforce any schema.
    - Document: A document is a record in MongoDB (similar to a row in relational databases). It is stored in BSON format and has a unique identifier _id.
    - Field: A field is a key-value pair in a document. The key is a string, and the value can be of any data type, including arrays and nested documents.

PresentPals will have 1 database & 2 collections named Users (for the users documents & data fields) & Wishlists (for the wishlists documents & data fields). 

The database side will be hosted in MongoDB Atlas.



