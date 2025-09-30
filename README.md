# Shukebeta.Api.Framework

[![NuGet](https://img.shields.io/nuget/v/Shukebeta.Api.Framework.svg)](https://www.nuget.org/packages/Shukebeta.Api.Framework/)
[![NuGet Downloads](https://img.shields.io/nuget/dt/Shukebeta.Api.Framework.svg)](https://www.nuget.org/packages/Shukebeta.Api.Framework/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![.NET 8](https://img.shields.io/badge/.NET-8.0-blue.svg)](https://dotnet.microsoft.com/download/dotnet/8.0)

A comprehensive framework library for .NET 8 Web API projects providing base classes, extensions, and utilities for rapid development.

## 🚀 Features

### Core Components
- **Entity Base Classes** - Ready-to-use base entities with audit fields (CreatedAt, UpdatedAt, DeletedAt)
- **Repository Pattern** - Generic repository base classes with SqlSugar integration
- **Transaction Helpers** - Simplified database transaction management
- **Result Wrappers** - Standardized API response models (ApiResult, SuccessfulResult, FailedResult)

### Extensions & Utilities
- **Task Extensions** - Safe fire-and-forget execution with exception handling
- **DateTime Extensions** - Unix timestamp conversions and timezone utilities
- **Service Collection Extensions** - DI container configuration helpers
- **Enum Extensions** - Enhanced enum operations

### Infrastructure
- **Global Exception Handling** - Centralized exception management
- **JWT Authentication Helpers** - Token generation and validation utilities
- **Database Connection Management** - Multi-database support (MySQL, SQL Server, PostgreSQL, Oracle)
- **Text Encryption Helpers** - Secure data encryption utilities
- **Gravatar Integration** - User avatar generation

## 📦 Installation

### Package Manager Console
```powershell
Install-Package Shukebeta.Api.Framework
```

### .NET CLI
```bash
dotnet add package Shukebeta.Api.Framework
```

### PackageReference
```xml
<PackageReference Include="Shukebeta.Api.Framework" Version="1.0.1" />
```

## 🔧 Quick Start

### 1. Entity Base Classes

```csharp
// Simple entity with audit fields
public class User : EntityBase
{
    public string Name { get; set; } = string.Empty;
    public string Email { get; set; } = string.Empty;
}

// Entity with admin audit fields
public class Product : AdminEntityBase
{
    public string Name { get; set; } = string.Empty;
    public decimal Price { get; set; }
    // Inherits: CreatedAt, UpdatedAt, DeletedAt, CreateBy, UpdateBy
}
```

### 2. Repository Pattern

```csharp
public class UserRepository(ISqlSugarClient db) : RepositoryBase<User>(db)
{
    public async Task<User?> GetByEmailAsync(string email)
    {
        return await GetFirstOrDefaultAsync(u => u.Email == email);
    }
    
    public async Task<PageData<User>> GetUsersPagedAsync(int pageNumber, int pageSize)
    {
        return await GetPagedAsync(pageNumber, pageSize, u => u.CreatedAt, false);
    }
}
```

### 3. API Controllers with Result Wrappers

```csharp
[ApiController]
[Route("api/[controller]")]
public class UsersController(UserRepository userRepository) : ControllerBase
{
    [HttpGet]
    public async Task<ApiResult<PageData<User>>> GetUsers(int page = 1, int size = 10)
    {
        var users = await userRepository.GetUsersPagedAsync(page, size);
        return ApiResult<PageData<User>>.Success(users);
    }
    
    [HttpPost]
    public async Task<ApiResult<User>> CreateUser(CreateUserRequest request)
    {
        var user = new User { Name = request.Name, Email = request.Email };
        var userId = await userRepository.InsertReturnIdentityAsync(user);
        user.Id = userId;
        return ApiResult<User>.Success(user);
    }
}
```

### 4. Safe Fire-and-Forget Operations

```csharp
public class NotificationService(
    IEnumerable<INotificationProvider> providers,
    ILogger<NotificationService> logger)
{
    public void SendNotifications(string message)
    {
        // Safe fire-and-forget with automatic exception handling
        TaskExtensions.SafeFireAndForgetBatch(
            providers,
            async provider => await provider.SendAsync(message),
            logger,
            "Failed to send notification"
        );
    }
}
```

### 5. Transaction Management

```csharp
public class OrderService(ISqlSugarClient db, ILogger<OrderService> logger)
{
    public async Task<Order> CreateOrderAsync(CreateOrderRequest request)
    {
        return await TransactionHelper.ExecuteInTransactionAsync(db, async () =>
        {
            // All operations within a single transaction
            var order = new Order { /* ... */ };
            var orderId = await orderRepository.InsertReturnIdentityAsync(order);
            
            foreach (var item in request.Items)
            {
                await orderItemRepository.InsertAsync(new OrderItem 
                { 
                    OrderId = orderId, 
                    /* ... */ 
                });
            }
            
            return order;
        }, logger);
    }
}
```

### 6. Database Configuration

```csharp
// In Program.cs
builder.Services.Configure<DatabaseConnectionOptions>(
    builder.Configuration.GetSection("Database"));

builder.Services.AddSqlSugar(options =>
{
    options.ConnectionString = connectionString;
    options.DbType = DatabaseType.MySql;
});
```

## 🛠️ Configuration

### Database Connection
```json
{
  "Database": {
    "ConnectionString": "Server=localhost;Database=MyApp;Uid=user;Pwd=password;",
    "DbType": "MySql"
  }
}
```

### JWT Configuration
```json
{
  "Jwt": {
    "Key": "your-secret-key-here",
    "Issuer": "your-app-name",
    "Audience": "your-audience",
    "ExpireMinutes": 60
  }
}
```

## 🧪 Testing

The framework includes comprehensive unit tests covering all major components:

```bash
# Run tests
dotnet test

# Run with coverage
dotnet test --collect:"XPlat Code Coverage"
```

**Test Coverage:**
- ✅ Entity base classes
- ✅ Extension methods (DateTime, Task, Enum)
- ✅ Exception handling
- ✅ Encryption utilities
- ✅ Unix timestamp helpers

## 📚 API Documentation

### Core Classes

#### EntityBase
Base entity with standard audit fields:
- `Id` (long) - Primary key
- `CreatedAt` (long) - Unix timestamp
- `UpdatedAt` (long?) - Unix timestamp  
- `DeletedAt` (long?) - Unix timestamp for soft delete

#### RepositoryBase<T>
Generic repository with common operations:
- `GetAsync(id)` - Get by primary key
- `GetFirstOrDefaultAsync(predicate)` - Get first matching entity
- `GetPagedAsync(page, size, orderBy, desc)` - Paginated results
- `InsertAsync(entity)` - Insert entity
- `UpdateAsync(entity)` - Update entity
- `DeleteAsync(entity)` - Delete entity

#### ApiResult<T>
Standardized API response:
- `Success(data, message?)` - Success response
- `Error(errorCode, message)` - Error response
- `Successful` (bool) - Success indicator
- `Data` (T) - Response data
- `Message` (string) - Response message

## 🔄 Migration Guide

### From Local Api.Framework
1. Remove local `Api.Framework` project references
2. Install the NuGet package: `dotnet add package Shukebeta.Api.Framework`
3. Update namespace imports from `Api.Framework.*` to `Api.Framework.*` (no change needed)
4. All existing code should work without modifications

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/amazing-feature`
3. Make your changes and add tests
4. Ensure all tests pass: `dotnet test`
5. Commit your changes: `git commit -m 'Add amazing feature'`
6. Push to the branch: `git push origin feature/amazing-feature`
7. Open a Pull Request

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🔗 Links

- **NuGet Package**: https://www.nuget.org/packages/Shukebeta.Api.Framework/
- **GitHub Repository**: https://github.com/shukebeta/Api.Framework
- **Issues**: https://github.com/shukebeta/Api.Framework/issues

## 📈 Changelog

See [CHANGELOG.md](CHANGELOG.md) for a complete list of changes and version history.

---

**Made with ❤️ for .NET developers by [@shukebeta](https://github.com/shukebeta)**