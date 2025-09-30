# Api.Framework

A common framework library for .NET Web API projects providing base classes, extensions, and utilities.

## Features

- Base entity classes with audit fields
- Repository pattern base classes with SqlSugar integration
- Common exceptions and global exception handling
- JWT authentication helpers
- Database connection management
- Result wrappers and response models
- Extension methods for common operations
- Transaction helpers

## Installation

```bash
dotnet add package YourName.Api.Framework
```

## Usage

### Entity Base Classes

```csharp
public class User : EntityBase
{
    public string Name { get; set; }
    public string Email { get; set; }
}
```

### Repository Pattern

```csharp
public class UserRepository(ISqlSugarClient db) : RepositoryBase<User>(db)
{
    // Custom repository methods
}
```

### API Results

```csharp
public class UsersController : ControllerBase
{
    public ApiResult<List<User>> GetUsers()
    {
        var users = userRepository.GetList();
        return ApiResult<List<User>>.Success(users);
    }
}
```

## Version History

- 1.0.0 - Initial release