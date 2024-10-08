### Example Project Setup with DOTNET 8.03

```bash

mkdir DatingApp   
cd DatingApp  

# Create a new solution file in the current directory. This command initializes a new .NET solution file (.sln) that will contain one or more projects.
dotnet new sln -n API

# Create a new Web API project named API with controllers. This command generates a new Web API project with the specified name API and includes support for controllers.
dotnet new webapi -controllers -n API    

# List all projects in the solution. This command shows all the projects that are currently included in the solution file.
dotnet sln list 

# Add the API project to the solution. This command includes the API project into the solution file, allowing it to be managed and built as part of the solution.
dotnet sln API.sln add API/API.csproj

dotnet sln list


# Build and run the API project. This command compiles and executes the API project, starting the Web API application.
dotnet run

# Run the project and watch for file changes. This command runs the project and automatically restarts it if any source files are modified, which is useful for development and debugging.

dotnet watch
# Or watch --no-hot-reload
dotnet watch --no-hot-reload
```
### Commands

- Create a new .NET project.
```bash
dotnet new
```

- List new command options. The Short Name column has templates to use. Example:
```bash
dotnet new webapi -controllers -n API
```

- Restore dependencies specified in the project.
```bash
dotnet restore
```

- Build a project and all of its dependencies.
```bash
dotnet build
```

- Build and run a project.
```bash
dotnet run
```
- Build and run a project on specific/different port.

```bash
dotnet run --urls "http://127.0.0.1:<new-port>"
```

- Run unit tests using a test runner specified in the project.
```bash
dotnet test
```

- Entity Framework Core command-line tools.
```bash
dotnet ef
```

- .NET Core global tools command-line.
```bash
dotnet tool
```

- Clean the output of a project.
```bash
dotnet clean
```
- Use [NuGet](https://www.nuget.org/).
- NuGet command-line.
```bash
dotnet nuget
```
- Create a NuGet package.
```bash
dotnet pack
```

- Publish a .NET project.
```bash
dotnet publish
```

- Migrate a project from project.json to csproj.
```bash
dotnet migrate
```

- Modify solution (SLN) files.
```bash
dotnet sln
```


- List all available project templates.
```bash
dotnet new --list
```

- Display help information for a command.
```bash
dotnet help
```

- Display information about the installed .NET Core SDK.
```bash
dotnet --info
```

- Create/Recreate certs.
```bash
dotnet dev-certs https
```

- Trust certs.
```bash
dotnet dev-certs https --trust
```

- Clean certs.
```bash
dotnet dev-certs https --clean
```

- Entity Framework Core .NET Command-line Tools 8.0.3, with dotnet-ef tool.
```bash
dotnet tool list -g
```

- Install dotnet-ef. Ensure it is the same version as your dotnet.
```bash
dotnet tool install --global dotnet-ef --version 8.0.3
```


- To view a help page of migrations.
```bash
dotnet ef migrations -h
```

- To use.
```bash
dotnet ef
```

- To create a InitialCreate Migration and Path, remember to use `cd API` before.
```bash
dotnet ef migrations add InitialCreate -o Data/Migrations
```
- To Next migrations just need add the name  
```bash
dotnet ef migrations add UserEntityUpdated
```
- To undo this action
```bash
dotnet ef migrations remove
```

- To see the help options on databases.
```bash
dotnet ef database -h
```

- To activate migrations.
```bash
dotnet ef database update
```

- To delete/drop database
```bash
dotnet ef database drop
```

- To uninstall dotnet-ef.
```bash
dotnet tool uninstall --global dotnet-ef
```

- To create a .gitignore template file.
```bash
dotnet new gitignore
```

### Help Examples
- General help.
```bash
dotnet -h
```

- Help for webapi.
```bash
dotnet new webapi -h
```

- Help for subcommand webapi. Example usage: `dotnet new webapi -controllers -n API`
```bash
dotnet new webapi -controllers -h
```
- ADD New package
```bash
dotnet add package Microsoft.AspNetCore.Authentication.JwtBearer
```
- Watch with no hot reaload
```bash
 dotnet watch --no-hot-reload
```

**WORK With differents SDK versions**

- List of vesions of SDK
```bash
dotnet --list-sdks
```
```bash
dotnet --version
```

When we install each dotnet core SDK on OS, the each project can use SDKs version separately. Because the SDK have global installation. We can configuration each project settings by create global.json via this command:
```bash
dotnet new globaljson --force
```
- Edit the version from dotnet --list-sdks
```bash
vim global.json
```

and finally selected the correct version.

The process for selecting an SDK version is:

dotnet searches for a **global.json** file iteratively reverse-navigating the path upward from the current working directory.
dotnet uses the SDK specified in the first **global.json** found.
dotnet uses the latest installed SDK if no **lobal.json** is found.

References: https://learn.microsoft.com/en-us/dotnet/core/tools/global-json?tabs=netcore3x#globaljson-and-the-net-core-cli

Step-by-Step: https://stackoverflow.com/a/42078060/14557383


## Example usin dotnet CLI with in version 6 usin differents versions on global.json
```bash
dotnet new webapi -n Api --no-https 
```

## Example usin dotnet CLI with in version 8 and controllers template
```bash
dotnet new webapi -controllers -n Api
```
## Example Create a Manual Solution file 
```bash
dotnet new sln -n Api

mkdir Api.Api
mkdir Api.Tests
mkdir Api.IntegrationTests

cd Api.Api
dotnet new webapi
cd ..

cd Api.Tests
dotnet new xunit
cd ..

cd Api.IntegrationTests
dotnet new xunit
cd ..
```

```bash
dotnet sln Api.sln add Api.Api/Api.Api.csproj
dotnet sln Api.sln add Api.Tests/Api.Tests.csproj
dotnet sln Api.sln add Api.IntegrationTests/Api.IntegrationTests.csproj

cd Api.Tests
dotnet add reference ../Api.Api/Api.Api.csproj
cd ..

cd Api.IntegrationTests
dotnet add reference ../Api.Api/Api.Api.csproj
cd ..
```


![dotnet_versions](./files/dotnet_versions.png)


# Jupyter Notebooks with .NET and C#

![jupyter NET](./files/Jupyter%20NET.jpg)
![jupyter NET VSCODE](./files/Jupyter%20NET%20VSCODE.jpg)

### Setup Jupyter for C# locally

- Latest dotnet 5.0+ SDK
- Python 3.7+ with pip
### Install jupyterlab to default Python interpreter
```bash
pip install jupyterlab
```
### Install Dotnet Interactive dotnet tool

```bash
dotnet tool install -g Microsoft.dotnet-interactive
```
### Get Dotnet Interactive to register kernels with Jupyter  

```bash
dotnet interactive jupyter install
```
```bash
jupyter kernelspec list
```
```bash
jupyter lab build   
```
```bash
jupyter-lab
```

## Alternative using X Tool on NET CLI 
```bash
dotnet tool install --global x
```
```bash
dotnet tool update -g x
```
```bash
x jupyter-csharp
```
```bash
x jupyter-csharp <a href="https://techstacks.io">https://techstacks.io</a> FindTechStacks "{Ids:[1,2,3],VendorName:'Google',Take:5}"
```
### Output example: 
Saved to: techstacks.io-FindTechStacks.ipynb

Source: https://docs.servicestack.net/jupyter-notebooks-csharp#generate-c-jupyter-notebooks


## Angular CLI 
- Install Current Angular CLI 
```bash
npm install -g @angular/cli
```
- Install different version of Angular NG CLI if you need it  
```bash
npm uninstall -g @angular/cli
```
```bash
npm install -g @angular/cli@14
```
```bash
npm install -g @angular/cli
```
- For specific verion use
```bash
npm install -g @angular/cli@17
```
- To use Angular CLI, just need to check the version
```bash
ng version
```



- New standalone Angular project, in this case we use SPA not SSR, with CSS 
```bash 
ng new [PROJECT NAME]
cd [PROJECT NAME]
ng serve
```

- Create a Modular project 
```bash
ng new Client --standalone=false
```

- To use Serve
```bash
ng serve
```
- To use Serve with open automatically on browser
```bash
ng serve -o
```

## '[optional]' Use mkcert to create  locally-trusted certificates on boostrap project **https://github.com/FiloSottile/mkcert** 
- on Mac '(use the repository readme for more instructions)'
```bash
brew install mkcert
```
```bash
mkcert -install
```
- In the client project (Angular) create a ssl folder
```bash
mkdir ssl
```
```bash
cd ssl
```
```bash
mkcert localhost
```
- next you need to use in the local system or development serve, example here is in angular.js amnd next: 
- 
```json
{
  "serve": {
    "options": {
      "ssl": true,
      "sslCert": "./ssl/localhost.pem",
      "sslKey": "./ssl/localhost-key.pem"
    }
  }
  ...// rest of json
}
```

- Interceptor 
```bash
ng g interceptor [name]
```
- Example interceptor
```bash
ng g interceptor _interceptors/error --skip-tests
```

- Create commands help ng
```bash
ng generate --help
```
- create component help
```bash
ng generate component --help
```
- See how a Component nav could will create but not create at all
```bash
ng generate component nav --dry-run
```
- Create a Component nav using --skip-tests option
```bash
ng generate component nav --skip-tests
```
- Example Create a Service --skip-tests option
```bash
ng g s _services/members --skip-tests
```
- Create a Enviroment like a .env 
```bash
ng g environments
```


