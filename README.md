# DotnetLoginRegApp — Contacts REST API

A lightweight ASP.NET Core 8 Web API for managing contacts. Data is stored in an in-memory database, making it ideal for learning, prototyping, and local development without any database setup. Includes Swagger UI for interactive testing out of the box.

---

## Features

- **Get all contacts** — retrieve the full list of stored contacts
- **Add a contact** — create a new contact with name, email, phone, and address
- **Update a contact** — modify an existing contact by its GUID
- **Swagger UI** — auto-generated interactive API docs available in development mode

---

## Tech Stack

| Layer | Technology |
|---|---|
| Framework | ASP.NET Core 8 Web API |
| ORM | Entity Framework Core 7 (InMemory) |
| API Docs | Swashbuckle / Swagger |

> **Note:** Data is stored in memory and is lost when the application restarts. To persist data, swap `UseInMemoryDatabase` for a real provider such as SQL Server or SQLite.

---

## Prerequisites

- [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8)

No database installation required — the in-memory provider handles everything automatically.

---

## Getting Started

### 1. Clone the repository

```bash
git clone <your-repo-url>
cd DotnetLoginRegApp
```

### 2. Run the application

```bash
dotnet run
```

### 3. Open Swagger UI

Navigate to the following URL in your browser to explore and test the API interactively:

```
https://localhost:<port>/swagger
```

The port is printed in the terminal when the app starts.

---

## API Endpoints

Base path: `/api/contacts`

### Get all contacts

```
GET /api/contacts
```

Returns a list of all contacts currently in memory.

**Response**
```json
[
  {
    "id": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
    "fullName": "Jane Doe",
    "email": "jane@example.com",
    "phone": 1234567890,
    "address": "123 Main St"
  }
]
```

---

### Add a contact

```
POST /api/contacts
```

**Request body**
```json
{
  "fullName": "Jane Doe",
  "email": "jane@example.com",
  "phone": 1234567890,
  "address": "123 Main St"
}
```

**Response** — the newly created contact with its generated GUID.

---

### Update a contact

```
PUT /api/contacts/{id}
```

| Parameter | Type | Description |
|---|---|---|
| `id` | GUID | The ID of the contact to update |

**Request body**
```json
{
  "fullName": "Jane Smith",
  "email": "jane.smith@example.com",
  "phone": 9876543210,
  "address": "456 New Ave"
}
```

---

## Project Structure

```
DotnetLoginRegApp/
├── Controllers/
│   └── ContactsController.cs    # GET, POST, PUT endpoint handlers
├── Data/
│   └── ContactsAPIDbContext.cs  # EF Core in-memory DbContext
├── Models/
│   ├── Contact.cs               # Contact entity
│   ├── AddContactRequest.cs     # DTO for creating a contact
│   └── UpdateContactRequest.cs  # DTO for updating a contact
├── appsettings.json
└── Program.cs                   # App setup, DI, middleware
```

---

## Switching to a Persistent Database

To replace the in-memory database with SQL Server:

1. Install the SQL Server EF Core package:
   ```bash
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   ```

2. Add a connection string to `appsettings.json`:
   ```json
   {
     "ConnectionStrings": {
       "Default": "Server=localhost;Database=ContactsDb;Trusted_Connection=True;TrustServerCertificate=True;"
     }
   }
   ```

3. Update `Program.cs` — replace:
   ```csharp
   options.UseInMemoryDatabase("ContactsDb")
   ```
   with:
   ```csharp
   options.UseSqlServer(builder.Configuration.GetConnectionString("Default"))
   ```

4. Add and apply a migration:
   ```bash
   dotnet ef migrations add InitialCreate
   dotnet ef database update
   ```
