# Practical19

## Overview
This project is an ASP.NET Core MVC application that demonstrates user registration and login using ASP.NET Core Identity with a SQL Server database. It includes validation via data annotations, role-based authorization, and UI flow for Admin and User access paths. The app seeds default roles and an admin account at startup. The primary goal is to explore authentication, role management, and controlled access to features based on user roles. 【F:Practical19/Program.cs†L1-L74】【F:Practical19/Controllers/AccountController.cs†L1-L124】【F:Practical19/Controllers/AdminController.cs†L1-L16】【F:Practical19/Controllers/UserController.cs†L1-L15】

## Features
- **Identity-based authentication** with SQL Server using `ApplicationUser` and `ApplicationDbContext`. 【F:Practical19/Program.cs†L1-L34】【F:Practical19/Data/ApplicationDbContext.cs†L1-L16】【F:Practical19/Models/ApplicationUser.cs†L1-L12】
- **Registration and login** flows backed by the database, including sign-in, sign-out, and access denied handling. 【F:Practical19/Controllers/AccountController.cs†L1-L124】
- **Validation** through data annotations in view models (required fields, email format, password confirmation). 【F:Practical19/ViewModels/RegisterViewModel.cs†L1-L22】【F:Practical19/ViewModels/LoginViewModel.cs†L1-L17】
- **Role-based authorization** for Admin and User routes with conditional navigation in the UI. 【F:Practical19/Controllers/AdminController.cs†L1-L16】【F:Practical19/Controllers/UserController.cs†L1-L15】【F:Practical19/Views/Shared/_Layout.cshtml†L23-L80】
- **Role and admin seeding** at startup, including a default admin account. 【F:Practical19/Program.cs†L35-L74】

## Project Structure
```
Practical19/
├── Controllers/        # MVC controllers (Account, Admin, User, Home)
├── Data/               # EF Core DbContext for Identity
├── Models/             # ApplicationUser model
├── ViewModels/         # Login and registration view models + validation
├── Views/              # Razor views for account and role pages
├── Program.cs          # App configuration, Identity setup, role seeding
└── appsettings.json    # Database connection string
```

## Setup
### Prerequisites
- .NET SDK (compatible with the project)
- SQL Server (LocalDB by default)

### Configure the Database
The app is configured to use LocalDB by default:
```
Server=(LocalDB)\MSSQLLocalDB;Database=Practical19;Trusted_Connection=True;MultipleActiveResultSets=true;TrustServerCertificate=true
```
Update the `DefaultConnection` string in `appsettings.json` if needed. 【F:Practical19/appsettings.json†L1-L11】

### Run the Application
From the repository root:
```bash
dotnet restore
dotnet run --project Practical19/Practical19.csproj
```
The application applies migrations and seeds roles/admin at startup. 【F:Practical19/Program.cs†L35-L74】

## Authentication & Authorization Flow
1. **Register**: Users register with full name, email, and password. Data annotations enforce validation. New users are assigned the **User** role automatically and signed in. 【F:Practical19/Controllers/AccountController.cs†L22-L70】【F:Practical19/ViewModels/RegisterViewModel.cs†L1-L22】
2. **Login**: Users authenticate with email and password. Invalid attempts show errors; lockout is enabled on failures. 【F:Practical19/Controllers/AccountController.cs†L73-L110】【F:Practical19/ViewModels/LoginViewModel.cs†L1-L17】
3. **Role-based access**:
   - `/Admin` is restricted to **Admin** role.
   - `/User` is restricted to **User** role.
   - Navigation items appear based on role membership. 【F:Practical19/Controllers/AdminController.cs†L1-L16】【F:Practical19/Controllers/UserController.cs†L1-L15】【F:Practical19/Views/Shared/_Layout.cshtml†L23-L80】
4. **Logout**: Clears the authentication cookie and redirects to the home page. 【F:Practical19/Controllers/AccountController.cs†L111-L119】

## Default Admin Account
On startup, the app seeds roles and an admin user if it doesn't exist:
- **Email**: `admin@gmail.com`
- **Password**: `Admin@123`
This account is assigned the **Admin** role automatically. 【F:Practical19/Program.cs†L52-L74】

## Notes
- The `AssignAdminRole` action can promote a user to Admin by email (POST). 【F:Practical19/Controllers/AccountController.cs†L121-L132】
- Access to `Admin` and `User` views is controlled using the `[Authorize(Roles = "...")]` attribute. 【F:Practical19/Controllers/AdminController.cs†L1-L16】【F:Practical19/Controllers/UserController.cs†L1-L15】
