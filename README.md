# Recipe Web Application Project

## **Overview**
This project was developed as part of the Web Development Environments course for educational purposes. The application provides users with a comprehensive recipe management system, allowing them to search, view, and create recipes while managing their favorites and family recipes.

The project architecture consists of three main components:

### **1. [Backend](https://github.com/arie478/RecipeWebApplication-Backend)**
#### **Application Server**
- Built using **Node.js** and **Express.js**.
- Responsible for handling the business logic of the application, including fetching, processing, managing, and storing information.

#### **Application Database**
- **MySQL** database for managing user information and recipe data.
- Integration with an external API to fetch recipe-related information.

### **2. [Frontend](https://github.com/arie478/RecipeWebApplication-Frontend)**
- Developed using **Vue.js**.
- Manages the display logic, user experience (UX), and client-side operations.
- Communicates with the backend server to fetch and update data.

## **Features**
- **User Authentication:** Register and login functionality.
- **Recipe Search:** Search for recipes using various criteria.
- **Detailed Recipe View:** Display complete recipe information.
- **Recipe Creation Modal:** Users can create and save their own recipes.
- **Favorites Management:** Save and manage favorite recipes.
- **Family Recipes Page:** A dedicated section for family recipes.
- **Responsive Design:** Optimized for both desktop and mobile devices.

## **API Documentation**
The API serves as a contract between the frontend and backend components, defining the available resources and request/response formats.

### **Endpoints**
| Method | Endpoint | Description |
|--------|----------|-------------|
| GET    | /api/recipes | Retrieve all recipes |
| GET    | /api/recipes/:id | Retrieve a specific recipe by ID |
| POST   | /api/recipes | Create a new recipe |
| PUT    | /api/recipes/:id | Update an existing recipe |
| DELETE | /api/recipes/:id | Delete a recipe |
| GET    | /api/users/:id/favorites | Retrieve user’s favorite recipes |
| POST   | /api/users/:id/favorites | Add a recipe to favorites |
| DELETE | /api/users/:id/favorites/:recipeId | Remove a recipe from favorites |

## **Project Structure**
```
project-root/
├── backend/
│   ├── server.js
│   ├── routes/
│   └── models/
├── frontend/
│   ├── src/
│   └── public/
├── database/
└── README.md
```

## **Technologies Used**
- **Frontend:** Vue.js
- **Backend:** Node.js, Express.js
- **Database:** MySQL
- **External API:** Recipe-related API for fetching external data

## **Setup Instructions**

### **Backend Setup**
1. Navigate to the backend directory:
   ```bash
   cd backend
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
3. Start the server:
   ```bash
   npm start
   ```

### **Frontend Setup**
1. Navigate to the frontend directory:
   ```bash
   cd frontend
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
3. Start the development server:
   ```bash
   npm run serve
   ```

### **Database Setup**
1. Create a MySQL database.
2. Import the provided database schema.
3. Update the backend configuration with your database credentials.

## **License**
This project is for educational purposes only and is not intended for commercial use.
