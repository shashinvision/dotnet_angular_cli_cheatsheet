# .NET and Angular CLI Cheat Sheet 📋

A comprehensive guide for full-stack developers working with .NET and Angular. This cheat sheet covers everything from initial setup to advanced testing strategies.

## Table of Contents
- [.NET Core Setup](#net-core-setup) 🛠️
- [.NET CLI Commands](#net-cli-commands) 🖥️
- [Entity Framework Core](#entity-framework-core) 🗃️
- [.NET Testing Guide](#net-testing-guide) 🧪
- [Angular CLI](#angular-cli) ⚡
- [Development Tools](#development-tools) 🧰

---

## .NET Core Setup 🛠️

### Linux/Mac Development Environment

#### Fix OmniSharp Issues (Linux/Mac)
```bash
# Ubuntu/Debian
sudo apt update
sudo apt install mono-complete

# macOS
brew install mono

# Manual OmniSharp installation
git clone https://github.com/OmniSharp/omnisharp-roslyn.git
cd omnisharp-roslyn
./build.sh
```

#### Multiple .NET SDK Versions Setup
```bash
# Create dotnet directory
mkdir ~/.dotnet

# Download and install .NET 8
wget https://builds.dotnet.microsoft.com/dotnet/Sdk/8.0.408/dotnet-sdk-8.0.408-linux-x64.tar.gz
tar -zxf dotnet-sdk-8.0.408-linux-x64.tar.gz -C ~/.dotnet

# Download and install .NET 6
wget https://builds.dotnet.microsoft.com/dotnet/Sdk/6.0.425/dotnet-sdk-6.0.425-linux-x64.tar.gz
tar -zxf dotnet-sdk-6.0.425-linux-x64.tar.gz -C ~/.dotnet

# Verify installations
dotnet --list-sdks

# Delete 

sudo rm -rf /usr/local/share/dotnet/sdk/6.0.*
sudo rm -rf /usr/local/share/dotnet/sdk/7.0.*
sudo rm -rf /usr/local/share/dotnet/sdk/8.0.403

## optional delete runtimes

ls /usr/local/share/dotnet/shared/Microsoft.NETCore.App
sudo rm -rf /usr/local/share/dotnet/shared/Microsoft.NETCore.App/6.*
sudo rm -rf /usr/local/share/dotnet/shared/Microsoft.NETCore.App/7.*

# Verify 
dotnet --list-sdks

```

#### Environment Configuration
Add to your `.zshrc` or `.bashrc`:
```bash
export DOTNET_ROOT=$HOME/.dotnet
export PATH=$HOME/.dotnet:$PATH
export DOTNET_ROLL_FORWARD=Major  # Use latest major version
```
Reload configuration:
```bash
source ~/.zshrc  # or source ~/.bashrc
```

#### SSL Certificate Issues (Linux)
```bash
# Install and configure dev certificates
dotnet tool update -g linux-dev-certs
dotnet linux-dev-certs install

# Clean and recreate certificates
dotnet dev-certs https --clean
dotnet dev-certs https --trust
```

---

## .NET CLI Commands 🖥️

### Project Creation and Management

#### Basic Project Setup
```bash
# Create solution directory
mkdir MyApp && cd MyApp

# Create solution file
dotnet new sln -n MyApp

# Create Web API project
dotnet new webapi -controllers -n MyApp.Api

# Add project to solution
dotnet sln add MyApp.Api/MyApp.Api.csproj

# Verify solution structure
dotnet sln list
```

#### Multi-Project Solution Setup
```bash
# Create organized solution structure
dotnet new sln -n MyApp
mkdir MyApp.Api
mkdir MyApp.Core
mkdir MyApp.Infrastructure
mkdir MyApp.Tests
mkdir MyApp.IntegrationTests

# Create projects
cd MyApp.Api && dotnet new webapi && cd ..
cd MyApp.Core && dotnet new classlib && cd ..
cd MyApp.Infrastructure && dotnet new classlib && cd ..
cd MyApp.Tests && dotnet new xunit && cd ..
cd MyApp.IntegrationTests && dotnet new xunit && cd ..

# Add projects to solution
dotnet sln add MyApp.Api/MyApp.Api.csproj
dotnet sln add MyApp.Core/MyApp.Core.csproj
dotnet sln add MyApp.Infrastructure/MyApp.Infrastructure.csproj
dotnet sln add MyApp.Tests/MyApp.Tests.csproj
dotnet sln add MyApp.IntegrationTests/MyApp.IntegrationTests.csproj

# Add project references
cd MyApp.Api && dotnet add reference ../MyApp.Core/MyApp.Core.csproj && cd ..
cd MyApp.Infrastructure && dotnet add reference ../MyApp.Core/MyApp.Core.csproj && cd ..
cd MyApp.Tests && dotnet add reference ../MyApp.Core/MyApp.Core.csproj && cd ..
```

### Essential CLI Commands

#### Project Operations
```bash
# List available templates
dotnet new --list

# Create projects with specific templates
dotnet new webapi -controllers -n MyApi
dotnet new classlib -n MyLibrary
dotnet new console -n MyConsoleApp

# Restore dependencies
dotnet restore

# Build project
dotnet build

# Run project
dotnet run

# Run on specific port
dotnet run --urls "http://127.0.0.1:5001"

# Watch for changes (development)
dotnet watch
dotnet watch --no-hot-reload  # Disable hot reload
dotnet watch --no-hot-reload --project MyApi/MyApi.csproj
```

#### Package Management
```bash
# Add NuGet package
dotnet add package Microsoft.EntityFrameworkCore
dotnet add package Microsoft.AspNetCore.Authentication.JwtBearer

# Remove package
dotnet remove package PackageName

# List packages
dotnet list package

# Update packages
dotnet restore --force
```

#### Version Management
```bash
# Check installed SDKs
dotnet --list-sdks

# Check current version
dotnet --version

# Get system information
dotnet --info

# Create global.json for project-specific SDK
dotnet new globaljson --force
```
Example `global.json`:
```json
{
  "sdk": {
    "version": "8.0.408",
    "rollForward": "latestMinor"
  }
}
```

#### Utilities
```bash
# Create .gitignore
dotnet new gitignore

# Clean build outputs
dotnet clean

# Publish application
dotnet publish -c Release

# Create NuGet package
dotnet pack

# Install global tools
dotnet tool install --global dotnet-ef
dotnet tool list -g
dotnet tool update -g dotnet-ef
```

---

## Entity Framework Core 🗃️

### Installation and Setup
```bash
# Install EF Core tools globally
dotnet tool install --global dotnet-ef --version 8.0.3

# Verify installation
dotnet ef --version

# View available commands
dotnet ef --help
dotnet ef migrations --help
dotnet ef database --help
```

### Migration Commands
```bash
# Create initial migration
dotnet ef migrations add InitialCreate -o Data/Migrations

# Add subsequent migrations
dotnet ef migrations add UserEntityUpdated
dotnet ef migrations add AddProductTable

# Remove last migration
dotnet ef migrations remove

# Apply migrations to database
dotnet ef database update

# Update to specific migration
dotnet ef database update InitialCreate

# Drop database
dotnet ef database drop

# Generate migration script
dotnet ef migrations script

# View migration history
dotnet ef migrations list
```

### Common EF Core Patterns
```bash
# Create migration with custom output folder
dotnet ef migrations add AddAuditFields -o Data/Migrations/Audit

# Create migration for specific context
dotnet ef migrations add UpdateUserModel --context UserDbContext

# Update database for specific environment
dotnet ef database update --environment Production
```

---

## .NET Testing Guide 🧪

### Testing Frameworks Setup

#### xUnit (Recommended)
```xml
<PackageReference Include="Microsoft.NET.Test.Sdk" Version="17.8.0" />
<PackageReference Include="xunit" Version="2.4.2" />
<PackageReference Include="xunit.runner.visualstudio" Version="2.4.5" />
<PackageReference Include="Moq" Version="4.20.69" />
<PackageReference Include="FluentAssertions" Version="6.12.0" />
<PackageReference Include="Microsoft.AspNetCore.Mvc.Testing" Version="8.0.0" />
```

#### NUnit (Alternative)
```xml
<PackageReference Include="Microsoft.NET.Test.Sdk" Version="17.8.0" />
<PackageReference Include="NUnit" Version="3.13.3" />
<PackageReference Include="NUnit3TestAdapter" Version="4.5.0" />
<PackageReference Include="coverlet.collector" Version="3.2.0" />
```

#### xUnit/NUnit packages
```bash
dotnet add package xunit
dotnet add package xunit.runner.visualstudio
```

#### NUnit packages
```bash
dotnet add package NUnit
dotnet add package NUnit3TestAdapter
dotnet add package Microsoft.NET.Test.Sdk
```

#### Shared packages
```bash
dotnet add package Moq
dotnet add package FluentAssertions
dotnet add package AutoFixture
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

### Essential Testing Packages
```bash
# Add testing packages
dotnet add package xunit
dotnet add package xunit.runner.visualstudio
dotnet add package Moq
dotnet add package FluentAssertions
dotnet add package AutoFixture
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

### Testing Patterns and Examples

#### NUnit Basic Test Example
```csharp
[TestFixture]
public class CalculatorTests
{
    private Calculator _calculator;

    [SetUp]
    public void Setup()
    {
        _calculator = new Calculator();
    }

    [Test]
    public void Add_ShouldReturnSum()
    {
        var result = _calculator.Add(2, 3);
        Assert.AreEqual(5, result);
    }

    [TestCase(4, true)]
    [TestCase(5, false)]
    public void IsEven_ShouldReturnExpectedResult(int number, bool expected)
    {
        var result = _calculator.IsEven(number);
        Assert.That(result, Is.EqualTo(expected));
    }

    [Test]
    public void Divide_ByZero_ShouldThrow()
    {
        Assert.Throws<DivideByZeroException>(() => _calculator.Divide(10, 0));
    }
}
```

#### Basic Unit Test Structure
```csharp
// AAA Pattern: Arrange, Act, Assert
public class CalculatorTests
{
    private readonly Calculator _calculator;
    
    public CalculatorTests()
    {
        _calculator = new Calculator();
    }
    
    [Fact]
    public void Add_WithTwoPositiveNumbers_ShouldReturnSum()
    {
        // Arrange
        var a = 5;
        var b = 3;
        
        // Act
        var result = _calculator.Add(a, b);
        
        // Assert
        result.Should().Be(8);
    }
    
    [Theory]
    [InlineData(4, true)]
    [InlineData(5, false)]
    [InlineData(0, true)]
    public void IsEven_WithDifferentNumbers_ShouldReturnExpectedResult(int number, bool expected)
    {
        // Act
        var result = _calculator.IsEven(number);
        
        // Assert
        result.Should().Be(expected);
    }
    
    [Fact]
    public void Divide_ByZero_ShouldThrowException()
    {
        // Arrange
        var a = 10.0;
        var b = 0.0;
        
        // Act & Assert
        Action action = () => _calculator.Divide(a, b);
        action.Should().Throw<DivideByZeroException>();
    }
}
```

#### Mocking with Moq
```csharp
public class UserServiceTests
{
    private readonly Mock<IUserRepository> _mockRepository;
    private readonly UserService _service;
    
    public UserServiceTests()
    {
        _mockRepository = new Mock<IUserRepository>();
        _service = new UserService(_mockRepository.Object);
    }
    
    [Fact]
    public async void GetUser_UserExists_ShouldReturnUser()
    {
        // Arrange
        var userId = 1;
        var expectedUser = new User { Id = userId, Name = "John" };
        
        _mockRepository.Setup(r => r.GetByIdAsync(userId))
                      .ReturnsAsync(expectedUser);
        
        // Act
        var result = await _service.GetUserAsync(userId);
        
        // Assert
        result.Should().NotBeNull();
        result.Should().BeEquivalentTo(expectedUser);
        _mockRepository.Verify(r => r.GetByIdAsync(userId), Times.Once);
    }
}
```

#### Integration Testing
```csharp
public class UsersControllerIntegrationTests : IClassFixture<WebApplicationFactory<Program>>
{
    private readonly WebApplicationFactory<Program> _factory;
    private readonly HttpClient _client;
    
    public UsersControllerIntegrationTests(WebApplicationFactory<Program> factory)
    {
        _factory = factory;
        _client = _factory.CreateClient();
    }
    
    [Fact]
    public async Task GetUsers_ShouldReturnSuccessStatusCode()
    {
        // Act
        var response = await _client.GetAsync("/api/users");
        
        // Assert
        response.Should().BeSuccessful();
        var content = await response.Content.ReadAsStringAsync();
        content.Should().NotBeNullOrEmpty();
    }
}
```

### Test Execution Commands
```bash
# Run all tests
dotnet test

# Run tests with specific framework
dotnet test --framework net8.0

# Run tests with detailed output
dotnet test --verbosity detailed

# Run specific test project
dotnet test MyApp.Tests/

# Filter tests by name
dotnet test --filter "UserServiceTests"

# Filter NUnit tests
dotnet test --filter "FullyQualifiedName~CalculatorTests"

# Run tests with code coverage
dotnet test --collect:"XPlat Code Coverage"

# Generate coverage report
dotnet tool install -g dotnet-reportgenerator-globaltool
reportgenerator -reports:"coverage.cobertura.xml" -targetdir:"coveragereport" -reporttypes:Html

# Run tests in parallel
dotnet test --parallel

# Run tests with custom settings
dotnet test --settings test.runsettings
```

## REPORT TEST: 
### coverlet.collector
```bash
dotnet add package coverlet.collector
```
```bash
dotnet test --collect:"XPlat Code Coverage" /p:Threshold=70 /p:ThresholdStat=Total
```
### reportgenerator
```bash
dotnet tool install -g dotnet-reportgenerator-globaltool
```
```bash
reportgenerator -reports:"./TestResults/**/coverage.cobertura.xml" -targetdir:"coverage" -reporttypes:Html
```
---

## Angular CLI ⚡

### Installation and Version Management
```bash
# Install latest Angular CLI globally
npm install -g @angular/cli

# Install specific version
npm install -g @angular/cli@17
npm install -g @angular/cli@16.2.0

# Uninstall current version
npm uninstall -g @angular/cli

# Check version
ng version
```

### Project Creation
```bash
# Create new standalone project (Angular 17+)
ng new MyApp
cd MyApp
ng serve

# Create modular project (traditional structure)
ng new MyApp --standalone=false

# Create project with specific options
ng new MyApp --routing --style=scss --skip-tests --package-manager=npm

# Serve application
ng serve
ng serve -o  # Open browser automatically
ng serve --port 4201  # Custom port
```

### SSL Configuration

#### Using mkcert (Recommended)
```bash
# Install mkcert (macOS)
brew install mkcert
mkcert -install

# Create SSL certificates
mkdir ssl
cd ssl
mkcert localhost

# Configure Angular CLI
```

Add to `angular.json`:
```json
{
  "serve": {
    "builder": "@angular-devkit/build-angular:dev-server",
    "options": {
      "ssl": true,
      "sslCert": "./ssl/localhost.pem",
      "sslKey": "./ssl/localhost-key.pem"
    }
  }
}
```

#### Using Angular's built-in SSL
```bash
ng serve --ssl
ng serve --ssl --host localhost --port 4200
```

### Code Generation
```bash
# Get help for generators
ng generate --help
ng generate component --help

# Component generation
ng generate component nav --skip-tests
ng g c shared/header --skip-tests

# Service generation
ng g s services/user --skip-tests
ng g s core/services/auth --skip-tests

# Other generators
ng g interface models/user
ng g enum enums/user-role
ng g pipe pipes/capitalize
ng g directive directives/highlight
ng g guard guards/auth
ng g interceptor interceptors/error --skip-tests

# Module generation (for non-standalone apps)
ng g module shared
ng g module feature/user --routing

# Environment files
ng g environments
```

### Advanced Angular CLI Commands
```bash
# Build for production
ng build --prod
ng build --configuration production

# Analyze bundle size
ng build --stats-json
npx webpack-bundle-analyzer dist/stats.json

# Add dependencies
ng add @angular/material
ng add @ngrx/store
ng add @angular/pwa

# Update dependencies
ng update
ng update @angular/core @angular/cli

# Lint and format
ng lint
ng lint --fix

# Test commands
ng test
ng test --watch=false --browsers=ChromeHeadless
ng e2e

# Extract i18n messages
ng extract-i18n

# Dry run (preview changes)
ng generate component nav --dry-run
```

---

## Development Tools 🧰

### Jupyter Notebooks with .NET

#### Setup
```bash
# Install Python dependencies
pip install jupyterlab

# Install .NET Interactive
dotnet tool install -g Microsoft.dotnet-interactive

# Register kernels with Jupyter
dotnet interactive jupyter install

# Verify installation
jupyter kernelspec list

# Build and start Jupyter Lab
jupyter lab build
jupyter-lab
```

#### Alternative with X Tool
```bash
# Install X tool
dotnet tool install --global x
dotnet tool update -g x

# Generate C# Jupyter notebooks
x jupyter-csharp
x jupyter-csharp https://techstacks.io FindTechStacks "{Ids:[1,2,3],VendorName:'Google',Take:5}"
```

### Productivity Tips

#### LazyVim + Tmux Workflow
```bash
# Essential tmux commands for .NET development
tmux new -s dotnet-dev
tmux split-window -h
tmux split-window -v

# Window management
Ctrl+b + %  # Split horizontally
Ctrl+b + "  # Split vertically
Ctrl+b + arrow  # Navigate panes
```

#### Git Integration
```bash
# Create comprehensive .gitignore
dotnet new gitignore

# Common additions for Angular projects
echo "dist/" >> .gitignore
echo "node_modules/" >> .gitignore
echo ".angular/" >> .gitignore
```

### Performance and Debugging

#### .NET Performance
```bash
# Enable detailed logging
export DOTNET_LOGGING_LEVEL=Debug

# Memory profiling
dotnet-trace collect --process-id <PID>

# Performance counters
dotnet-counters monitor --process-id <PID>
```

#### Angular Performance
```bash
# Bundle analysis
ng build --stats-json
npm install -g webpack-bundle-analyzer
webpack-bundle-analyzer dist/stats.json

# Lighthouse CI
npm install -g @lhci/cli
lhci autorun
```

---

## Common Workflows

### Full-Stack Development Setup
```bash
# Backend setup
mkdir MyFullStackApp
cd MyFullStackApp

# Create .NET API
dotnet new sln -n MyApp
dotnet new webapi -controllers -n MyApp.Api
dotnet sln add MyApp.Api/MyApp.Api.csproj

# Frontend setup
ng new client --directory=client --skip-git
cd client
ng serve --port 4200

# Back to API
cd ../MyApp.Api
dotnet watch --no-hot-reload
```

### Docker Integration
```dockerfile
# Dockerfile for .NET API
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY ["MyApp.Api/MyApp.Api.csproj", "MyApp.Api/"]
RUN dotnet restore "MyApp.Api/MyApp.Api.csproj"
COPY . .
WORKDIR "/src/MyApp.Api"
RUN dotnet build "MyApp.Api.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "MyApp.Api.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "MyApp.Api.dll"]
```

### CI/CD Pipeline Example
```yaml
# .github/workflows/dotnet.yml
name: .NET Core CI
on: [push, pull_request]
jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup .NET
      uses: actions/setup-dotnet@v3
      with:
        dotnet-version: '8.0.x'
    
    - name: Restore dependencies
      run: dotnet restore
    
    - name: Build
      run: dotnet build --no-restore
    
    - name: Test
      run: dotnet test --no-build --verbosity normal --collect:"XPlat Code Coverage"
    
    - name: Generate coverage report
      run: |
        dotnet tool install -g dotnet-reportgenerator-globaltool
        reportgenerator -reports:"**/coverage.cobertura.xml" -targetdir:"coveragereport" -reporttypes:Html
```

---

## Troubleshooting

### Common .NET Issues
```bash
# Clear NuGet cache
dotnet nuget locals all --clear

# Reset global tools
dotnet tool uninstall -g dotnet-ef
dotnet tool install -g dotnet-ef

# Fix certificate issues
dotnet dev-certs https --clean
dotnet dev-certs https --trust

# Check process using port
lsof -i :5000
kill -9 <PID>
```

### Common Angular Issues
```bash
# Clear npm cache
npm cache clean --force

# Clear Angular cache
ng cache clean

# Reset node_modules
rm -rfv node_modules package-lock.json
npm install

# Fix permissions (macOS/Linux)
sudo chown -R $(whoami) ~/.npm
```

---

## Best Practices

### Project Structure
```
MyFullStackApp/
├── src/
│   ├── MyApp.Api/          # Web API
│   ├── MyApp.Core/         # Business logic
│   ├── MyApp.Infrastructure/ # Data access
│   └── MyApp.Shared/       # Shared models
├── tests/
│   ├── MyApp.UnitTests/
│   ├── MyApp.IntegrationTests/
│   └── MyApp.AcceptanceTests/
├── client/                 # Angular app
├── docs/                   # Documentation
├── scripts/                # Build scripts
└── docker-compose.yml      # Local development
```

### Code Quality
```bash
# .NET code analysis
dotnet add package Microsoft.CodeAnalysis.Analyzers
dotnet add package StyleCop.Analyzers

# Angular code quality
ng add @angular-eslint/schematics
npm install --save-dev prettier
npm install --save-dev husky lint-staged
```

### Security
```bash
# .NET security packages
dotnet add package Microsoft.AspNetCore.Authentication.JwtBearer
dotnet add package Microsoft.AspNetCore.DataProtection

# Angular security
ng add @angular/cdk
npm install --save helmet
npm install --save express-rate-limit
```

---

## Resources and References
- [.NET Documentation](https://docs.microsoft.com/en-us/dotnet/) 📚
- [Entity Framework Core](https://docs.microsoft.com/en-us/ef/core/) 🗃️
- [Angular Documentation](https://angular.io/docs) ⚡
- [xUnit Testing](https://xunit.net/) 🧪
- [Moq Framework](https://github.com/moq/moq4) 🧩
- [FluentAssertions](https://fluentassertions.com/) ✅

---

*This cheat sheet is designed for intermediate to advanced full-stack developers working with .NET and Angular. Keep it handy for quick reference during development!* 🚀
