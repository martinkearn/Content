# Pulumi Setup (on windows)

These are the steps to get a Pulumi stack setup and ready to go (windows, .net , azure)

### Pre-Reqs

- [.net core 3.1 SDK or later](https://dotnet.microsoft.com/download)
- PowerShell as administrator
-  [Chocolatey package manager](https://chocolatey.org/)

## Install Pulumi, create and deploy project

In PowerShell:

- `choco install pulumi`
- `az login`

- Create a folder to house the project (`md c:\git\PulumiDemo`)
- Navigate to that folder (`cd c:\git\PulumiDemo`)
- `pulumi new azure-csharp`
- Accept default for name, description and stack name (press enter)
- `uksouth` for location
- `pulumi up`
- `yes` to performing update
- Go to Azure Portal and explore `resourceGroup...` resource group. Just an empty storage container.
- Open `PulumiDemo.csproj` in Visual Studio and explore
  - `MyStack.cs`
  - `Pulumi.yaml` - defines the project
  - `Pulumi.dev.yaml` - configuration value for the stack

