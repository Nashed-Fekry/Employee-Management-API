# Employee Management API

This project is a simple **CRUD API** built using **ASP.NET Core 8** to manage an organization’s employees. It demonstrates how to create a basic API with PostgreSQL for data storage, DTO (Data Transfer Object) usage for cleaner data handling, and error handling with validation.

## Features

- **CRUD Operations**: Manage employees with the following operations:
  - **GET /Employees**: Retrieve all employees.
  - **POST /Employees**: Add a new employee.
  - **PUT /Employees/{id}**: Update an existing employee.
  - **DELETE /Employees/{id}**: Delete an employee.
  
- **Data Transfer Objects (DTO)**: Clean separation between API data and internal database entities.
  
- **Validation**: Ensures that employee names and departments are not empty, and that salaries are positive numbers.
  
- **PostgreSQL Integration**: Uses PostgreSQL as the primary database for persistent storage.
  
- **Error Handling**: Handles cases where employees are not found or invalid data is provided.

## Requirements

To run this project locally, you will need the following:
- .NET 8 SDK
- PostgreSQL (for database storage)
- A database management tool (such as pgAdmin or DBeaver)

## Getting Started

### 1. Clone the repository
```bash
git clone https://github.com/yourusername/employee-management-api.git
cd employee-management-api
```

### 2. Setup PostgreSQL
1. Ensure PostgreSQL is installed and running.
2. Create a new database named `EmployeesDB` (or any name you prefer).
3. Use the following SQL script to create the `Employees` table:

```sql
CREATE TABLE Employees (
    EmployeeID SERIAL PRIMARY KEY,
    Name VARCHAR(100) NOT NULL,
    Department VARCHAR(100) NOT NULL,
    Salary DECIMAL(18, 2) CHECK (Salary > 0)
);
```

### 3. Update `appsettings.json`
Update your PostgreSQL connection string in `appsettings.json`:

```json
"ConnectionStrings": {
    "DefaultConnection": "Host=localhost;Database=EmployeesDB;Username=postgres;Password=yourpassword"
}
```

Replace `yourpassword` with your actual PostgreSQL password.

### 4. Run Migrations
Use **Entity Framework Core** to apply the migrations and create the database schema.

```bash
dotnet ef migrations add InitialCreate
dotnet ef database update
```

### 5. Run the Application
Now, you can run the application:

```bash
dotnet run
```

The API will be available at `https://localhost:5001` (or `http://localhost:5000` if HTTPS is not enabled).

### 6. Testing the API
You can test the API endpoints using tools like **Postman** or the built-in **Swagger UI**. After running the app, Swagger UI will be available at:

```
https://localhost:5001/swagger/index.html
```

### Endpoints

| HTTP Method | Endpoint                | Description                  |
|-------------|-------------------------|------------------------------|
| GET         | /api/Employees           | Get all employees             |
| GET         | /api/Employees/{id}      | Get a specific employee by ID |
| POST        | /api/Employees           | Add a new employee            |
| PUT         | /api/Employees/{id}      | Update an existing employee   |
| DELETE      | /api/Employees/{id}      | Delete an employee            |

## Testing

The project includes unit tests for the service layer and one integration test. To run the tests, use the following command:

```bash
dotnet test
```

## DTO Usage

This project utilizes **DTOs** (Data Transfer Objects) to transfer only the necessary employee data through the API. The DTO structure ensures data validation and clean separation from the database entities.

## SQL Query for Top 3 Highest Paid Employees by Department

An additional query is provided to retrieve the top 3 highest-paid employees in each department. The query is available in the file `top_3_highest_paid_employees.sql`.

```sql
WITH RankedEmployees AS (
    SELECT EmployeeID, Name, Department, Salary,
           RANK() OVER (PARTITION BY Department ORDER BY Salary DESC) AS Rank
    FROM Employees
)
SELECT EmployeeID, Name, Department, Salary
FROM RankedEmployees
WHERE Rank <= 3;
```

## Technologies Used

- **ASP.NET Core 8**: Web API framework
- **Entity Framework Core**: ORM for database access
- **PostgreSQL**: Database management system
- **Swagger**: API documentation and testing tool

## License

This project is open-source and available under the **MIT License**.

## Contributing

Contributions are welcome! Please feel free to submit a pull request or open an issue for any feature requests, bug reports, or questions.

