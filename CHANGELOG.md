# Changelog

All notable changes to the Shukebeta.Api.Framework project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.0.1] - 2024-09-30

### Added
- ✅ **Comprehensive unit tests** - Added 15 unit tests covering all major components
  - CustomException tests with proper API compatibility
  - DateTimeExtensions tests for Unix timestamp conversions
  - UnixTimestampHelper tests for timezone handling
  - TextEncryptionHelper tests for secure data encryption
- ✅ **Test infrastructure** - Proper test project structure with NUnit framework
- ✅ **Enhanced documentation** - Improved README with comprehensive examples and API documentation

### Fixed
- 🐛 **Test compatibility** - Fixed CustomException test cases to match current API structure
- 🐛 **Solution structure** - Added test project to solution file with proper configuration

### Changed
- 📦 **Package metadata** - Updated package description and documentation links
- 🔧 **Test project references** - Configured test project to reference source project correctly

### Technical Details
- **Test Coverage**: 15 tests covering core functionality
- **Test Framework**: NUnit 3.13.3 with Moq 4.20.70
- **Quality Assurance**: All tests passing with comprehensive assertions

---

## [1.0.0] - 2024-09-30

### Added
- 🎉 **Initial release** of Shukebeta.Api.Framework
- ✅ **Entity Base Classes**
  - `EntityBase` - Standard entity with Id, CreatedAt, UpdatedAt, DeletedAt
  - `AdminEntityBase` - Extended entity with CreateBy, UpdateBy audit fields
- ✅ **Repository Pattern Implementation**
  - `IRepositoryBase<T>` - Generic repository interface
  - `RepositoryBase<T>` - Full SqlSugar-based implementation with CRUD operations
  - Support for pagination, filtering, and sorting
- ✅ **Extension Methods**
  - `TaskExtensions` - Safe fire-and-forget execution with exception handling
  - `DateTimeExtensions` - Unix timestamp conversions and timezone utilities
  - `EnumExtensions` - Enhanced enum operations
  - `ServiceCollectionExtensions` - DI container configuration helpers
- ✅ **Result Wrappers**
  - `ApiResult` & `ApiResult<T>` - Standardized API responses
  - `SuccessfulResult<T>` - Success response wrapper
  - `FailedResult` & `FailedResult<T>` - Error response wrappers
- ✅ **Exception Handling**
  - `CustomException<T>` - Generic custom exception with typed data
  - `GlobalExceptionHandler` - ASP.NET Core global exception middleware
  - `CustomExceptionHelper` - Fluent exception creation utilities
- ✅ **Infrastructure Utilities**
  - `TransactionHelper` - Simplified database transaction management
  - `DatabaseConnectionOptions` - Database configuration model
  - `DatabaseType` - Supported database enumeration (MySQL, SQL Server, PostgreSQL, Oracle)
- ✅ **Authentication & Security**
  - `JwtConfig` & `JwtToken` - JWT authentication models
  - `TokenHelper` - JWT token generation and validation
  - `TextEncryptionHelper` - Secure data encryption utilities
  - `SaltGenerator` - Cryptographic salt generation
- ✅ **Additional Helpers**
  - `GravatarHelper` - User avatar URL generation
  - `UnixTimestampHelper` - Unix timestamp conversions
  - `ResultHelper` - Result object creation utilities
  - `CommonHelper` - General-purpose utility methods

### Dependencies
- **Target Framework**: .NET 8.0
- **SqlSugarCore**: 5.1.4.189 - ORM and database operations
- **Microsoft.AspNetCore.App**: Framework reference for web applications
- **Microsoft.IdentityModel.Tokens**: 7.5.1 - JWT token handling
- **System.IdentityModel.Tokens.Jwt**: 7.5.1 - JWT implementation
- **NLog**: 5.3.1 - Logging framework integration
- **WeihanLi.Common**: 1.0.64 - Additional utilities

### Package Information
- **Package ID**: Shukebeta.Api.Framework
- **License**: MIT
- **Repository**: https://github.com/shukebeta/Api.Framework
- **Documentation**: Comprehensive README with examples and API reference
- **Symbol Package**: Included for debugging support

---

## Upgrade Notes

### From Local Api.Framework to v1.0.0
1. **Remove local project references** - Delete local `Api.Framework` project references from your solution
2. **Install NuGet package** - Run `dotnet add package Shukebeta.Api.Framework`
3. **No code changes required** - All existing namespaces and APIs remain compatible
4. **Update solution files** - Remove Api.Framework project from solution file
5. **Clean build artifacts** - Delete `bin` and `obj` folders, then rebuild

### Breaking Changes
- **None** - This is the initial stable release

---

## Future Roadmap

### Planned Features
- 🔮 **Enhanced caching support** - Redis and in-memory caching abstractions
- 🔮 **Message queue integration** - RabbitMQ and Azure Service Bus helpers
- 🔮 **OpenAPI documentation** - Swagger/OpenAPI integration utilities
- 🔮 **Health check extensions** - Database and external service health checks
- 🔮 **Audit logging** - Comprehensive entity change tracking
- 🔮 **Multi-tenancy support** - Tenant isolation and management utilities

### Version Strategy
- **Patch versions (1.0.x)** - Bug fixes, small improvements, additional tests
- **Minor versions (1.x.0)** - New features, additional utilities, backward-compatible changes
- **Major versions (x.0.0)** - Breaking changes, architectural improvements

---

*For detailed technical documentation, see [README.md](README.md)*