# Running the Project

This guide provides step-by-step instructions for running the C# .NET 6 project locally.

## Prerequisites

### .NET 6 SDK

You need to install the .NET 6 SDK (Software Development Kit) on your machine. Visit the official Microsoft download page:

- **Download URL**: https://dotnet.microsoft.com/download/dotnet/6.0

Choose the **SDK 6.0.x** (LTS - Long Term Support) for your operating system (Windows, macOS, or Linux).

### Installation Verification

After installation, verify the SDK is installed:

```bash
# Check the installed .NET SDK version
dotnet --version
```

Expected output should start with `6.`, e.g., `6.0.421`.

### IDE (Optional but Recommended)

- **Visual Studio 2022** (17.0 or later)
- **Visual Studio Code** with the C# extension
- **Rider** by JetBrains

## Checking the .NET Version

To confirm the correct .NET version is being used:

```bash
dotnet --version
```

To list all installed SDKs:

```bash
dotnet --list-sdks
```

Ensure that version 6.x.x is listed. If not, install the SDK as described in the Prerequisites section.

## Restoring Dependencies

Restore the project dependencies (NuGet packages) before building:

```bash
dotnet restore
```

This command reads the `.csproj` file and downloads any required NuGet packages.

## Building the Project

Build the project to compile all source code:

```bash
dotnet build
```

For a Release build (optimized for performance):

```bash
dotnet build --configuration Release
```

### Build Output

After a successful build, the compiled binaries are placed in:

```
bin/<Configuration>/net6.0/
```

## Running the Project

### Running the Project

To run the project directly:

```bash
dotnet run
```

For a Release build:

```bash
dotnet run --configuration Release
```

### Running the Compiled Output

Alternatively, run the compiled DLL directly:

```bash
dotnet bin/Release/net6.0/<ProjectName>.dll
```

> Replace `<ProjectName>` with the actual project name (e.g., `MyApp.dll`).

## Configuration Settings

### appsettings.json

The project may include configuration files:

- `appsettings.json` - Default settings (shared across environments)
- `appsettings.{Environment}.json` - Environment-specific overrides (e.g., `appsettings.Development.json`)

### Setting the Environment

Set the `ASPNETCORE_ENVIRONMENT` environment variable:

**Windows (Command Prompt):**

```cmd
set ASPNETCORE_ENVIRONMENT=Development
dotnet run
```

**Windows (PowerShell):**

```powershell
$env:ASPNETCORE_ENVIRONMENT="Development"
dotnet run
```

**Linux / macOS / Windows (WSL):**

```bash
export ASPNETCORE_ENVIRONMENT=Development
dotnet run
```

### Launch Settings

For web projects, the `Properties/launchSettings.json` file controls how the application starts. Review this file to configure:

- Application URLs
- Environment variables

## Troubleshooting Common Issues

### 1. `dotnet` Command Not Found

**Cause**: .NET SDK is not installed or not in the system PATH.

**Solution**: Install the .NET 6 SDK and ensure it is added to your system PATH. Restart your terminal after installation.

### 2. Version Mismatch

**Cause**: A different .NET SDK version is installed (e.g., .NET 7 or 8), or .NET 6 is not installed.

**Solution**:

- Check installed versions: `dotnet --list-sdgs`
- Install the .NET 6 SDK.
- Use a `global.json` file to pin the SDK version:

  ```bash
  dotnet new globaljson --sdk-version 6.0.421
  ```

### 3. NuGet Package Restore Errors

**Cause**: Network issues or NuGet source not accessible.

**Solution**:

- Ensure you have an internet connection.
- Clear the NuGet cache:

  ```bash
  dotnet nuget locals all --clear
  ```

- Restore again:

  ```bash
  dotnet restore
  ```

### 4. Port Already in Use

**Cause**: The configured port is already occupied by another process.

**Solution**:

- Change the port in `Properties/launchSettings.json` or environment variables.
- Kill the process using the port:

  **Linux / macOS:**

  ```bash
  kill $(lsof -t -i :<PORT_NUMBER>)
  ```

  **Windows:**

  ```cmd
  taskkill /F /PID <PROCESS_ID>
  ```

### 5. Build Errors

**Cause**: Compilation errors, missing dependencies, or syntax issues.

**Solution**:

- Check the error messages in the terminal.
- Ensure all required NuGet packages are restored: `dotnet restore`
- Rebuild the project: `dotnet build`

### 6. Runtime Errors

**Cause**: Missing configuration, connection strings, or environment-specific settings.

**Solution**:

- Check `appsettings.json` for required settings.
- Verify environment variables are set correctly.
- Ensure database connections (if applicable) are configured.

## Additional Resources

- **Official .NET Documentation**: https://learn.microsoft.com/dotnet/
- **.NET 6 Announcement**: https://learn.microsoft.com/dotnet/core/whats-new/dotnet-six
- **.NET CLI Reference**: https://learn.microsoft.com/dotnet/core/tools/
- **NuGet Package Manager**: https://www.nuget.org/
- **Troubleshooting .NET**: https://learn.microsoft.com/dotnet/core/porting/
