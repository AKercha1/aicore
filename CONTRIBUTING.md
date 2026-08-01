# Contributing to AI Core

Thank you for your interest in contributing to AI Core! This document will guide you through the process of setting up your development environment, building and running the code, and submitting your changes for review.

## Table of Contents

- [Getting Started](#getting-started)
- [Development Setup](#development-setup)
- [Building and Running Locally](#building-and-running-locally)
- [Project Structure](#project-structure)
- [Branch Naming Convention](#branch-naming-convention)
- [Pull Request Process](#pull-request-process)
- [Code Style Guidelines](#code-style-guidelines)
- [Testing](#testing)
- [Documentation](#documentation)

## Getting Started

### Prerequisites

Before you begin, ensure you have the following installed:

- **.NET 9.0 SDK** - [Download here](https://dotnet.microsoft.com/download/dotnet/9.0)
- **Docker** - [Download here](https://www.docker.com/get-started)
- **Docker Compose** - Included with Docker Desktop
- **Git** - [Download here](https://git-scm.com/downloads)
- **Visual Studio 2022** (optional but recommended) or VS Code with C# extension

### Repository Overview

AI Core is a C# ASP.NET Core application that provides an AI platform with:
- Main API service (`AiCoreApi/`)
- File Ingestion service (`Ingestion/FileIngestion/`)
- Docker and Kubernetes deployment support
- Comprehensive API documentation

## Development Setup

1. **Fork and Clone the Repository**

   ```bash
   git clone https://github.com/YOUR_USERNAME/aicore.git
   cd aicore
   ```

2. **Add the Upstream Remote**

   ```bash
   git remote add upstream https://github.com/AKercha1/aicore.git
   ```

3. **Install .NET Dependencies**

   Restore NuGet packages for both solutions:

   ```bash
   # Main API solution
   dotnet restore AiCore.sln

   # File Ingestion solution
   dotnet restore Ingestion/FileIngestion/FileIngestion.sln
   ```

4. **Configure Local Environment**

   Create an `appsettings.Development.json` file in `AiCoreApi/` based on `appsettings.json` and configure your local settings including:
   - Database connection string
   - Redis cache connection
   - AI service credentials (Azure OpenAI, etc.)
   - Authentication settings

## Building and Running Locally

### Building the Project

Build the entire solution:

```bash
dotnet build AiCore.sln
```

Build specific projects:

```bash
# Main API only
dotnet build AiCoreApi/AiCoreApi.csproj

# File Ingestion service only
dotnet build Ingestion/FileIngestion/Service/Service.csproj
```

### Running with Docker Compose (Recommended)

The easiest way to run the complete stack locally is using Docker Compose:

```bash
docker-compose up -d
```

This will start:
- AI Core API (port 7878)
- PostgreSQL database (port 5439)
- Redis cache (port 6379)
- File Ingestion service (port 7880)

View logs:

```bash
docker-compose logs -f ai-core-api
```

Stop the services:

```bash
docker-compose down
```

### Running Directly with .NET

For development and debugging, you can run the services directly:

```bash
# Run the main API
cd AiCoreApi
dotnet run

# Run the file ingestion service (in a separate terminal)
cd Ingestion/FileIngestion/Service
dotnet run
```

### Running File Ingestion Service

The File Ingestion service can also be run independently:

```bash
# Using Docker Compose (recommended for ingestion service)
cd Ingestion/FileIngestion
docker-compose up -d

# Or directly with .NET
cd Ingestion/FileIngestion/Service
dotnet run
```

The API will be available at `http://localhost:7878` (or as configured in your launch settings).

### Database Migrations

If you need to apply database migrations:

```bash
cd AiCoreApi
dotnet ef database update
```

## Project Structure

```
aicore/
├── AiCoreApi/              # Main ASP.NET Core API
│   ├── Controllers/        # API controllers
│   ├── Services/          # Business logic services
│   ├── Models/            # Data models
│   ├── Data/              # Data access layer
│   ├── SemanticKernel/    # AI/LLM integration
│   ├── Migrations/        # EF Core migrations
│   └── Common/            # Shared utilities
├── Ingestion/             # File Ingestion Service
│   └── FileIngestion/
│       ├── Service/       # Ingestion API service
│       └── Dockerfile
├── docs/                  # Documentation
│   ├── API/              # API documentation
│   └── Agents/           # Agent guides
├── helm/                  # Kubernetes Helm charts
├── .github/              # GitHub workflows and configuration
├── docker-compose.yml    # Docker Compose configuration
├── Dockerfile           # Main API Docker image
└── AiCore.sln           # Main solution file
```

## Branch Naming Convention

We use a structured branch naming convention to keep our workflow organized:

```
autodev/issue-{number}-short-description
```

**Examples:**
- `autodev/issue-3-add-a-contributing-md`
- `autodev/issue-42-fix-authentication-bug`
- `autodev/issue-15-implement-caching`

**Guidelines:**
- Always branch from `dev`
- Use lowercase with hyphens for the description
- Keep the description short but descriptive
- Include the issue number from GitHub issues

### Creating a New Branch

```bash
git checkout dev
git pull upstream dev
git checkout -b autodev/issue-{number}-your-description
```

## Pull Request Process

### 1. Make Your Changes

- Create a branch following the naming convention
- Make your changes following our [code style guidelines](#code-style-guidelines)
- Commit your changes with clear, descriptive messages

### 2. Test Your Changes

- Build the project: `dotnet build`
- Test locally using Docker Compose or direct .NET execution
- Verify your changes don't break existing functionality

### 3. Update Documentation

If your changes affect:
- API endpoints: Update the relevant files in `docs/API/`
- Agent behavior: Update `docs/Agents/`
- Deployment: Update `README.md` if needed

### 4. Submit Your Pull Request

1. Push your branch to your fork:
   ```bash
   git push origin autodev/issue-{number}-your-description
   ```

2. Create a pull request from your branch to `dev`

3. **Add at least one label** to your PR (required by our workflow):
   - `bug` - for bug fixes
   - `enhancement` - for new features
   - `documentation` - for documentation changes
   - `refactoring` - for code refactoring
   - `test` - for test additions or changes

4. Fill out the PR template with:
   - Description of changes
   - Related issue number
   - Testing performed
   - Screenshots (if applicable)

### 5. Review Process

- All PRs require at least one review before merging
- Address review feedback promptly
- Keep your PR up to date with `dev` branch
- Once approved, maintainers will merge to `dev`

### 6. Merging to Main

The `main` branch is protected and can only receive merges from `dev`. Do not create PRs directly to `main`.

## Code Style Guidelines

### C# Conventions

- Follow standard C# naming conventions:
  - Classes: `PascalCase` (e.g., `UserService`)
  - Methods: `PascalCase` (e.g., `GetUserById`)
  - Properties: `PascalCase` (e.g., `UserName`)
  - Local variables: `camelCase` (e.g., `userId`)
  - Private fields: `_camelCase` with underscore prefix (e.g., `_userRepository`)

- Use meaningful names that describe the purpose
- Keep methods focused and concise
- Add XML documentation comments for public APIs

### Formatting

- Use 4 spaces for indentation
- Maximum line length: 120 characters
- Place opening braces on new line for classes, methods, and control structures

### Example

```csharp
/// <summary>
/// Gets a user by their unique identifier.
/// </summary>
/// <param name="userId">The user identifier.</param>
/// <returns>The user if found; otherwise, null.</returns>
public async Task<User?> GetUserByIdAsync(Guid userId)
{
    if (userId == Guid.Empty)
    {
        throw new ArgumentException("User ID cannot be empty", nameof(userId));
    }

    return await _userRepository.GetByIdAsync(userId);
}
```

### Dependencies

- Do not add new NuGet packages unless absolutely necessary
- Prefer using existing dependencies already in the project
- If adding a new package, discuss it in your PR description

## Testing

Currently, AI Core does not have an automated test suite. We rely on:

1. **Manual Testing** - Test your changes thoroughly before submitting
2. **Code Review** - Peer review catches issues
3. **Integration Testing** - Test the complete flow using Docker Compose

### Manual Testing Checklist

- [ ] Build succeeds: `dotnet build`
- [ ] Application starts without errors
- [ ] New features work as expected
- [ ] Existing features are not broken
- [ ] API endpoints return correct responses
- [ ] Database migrations apply successfully
- [ ] Docker Compose stack runs successfully

We are working on adding automated tests. Contributions to test coverage are welcome!

## Documentation

AI Core has comprehensive documentation:

- **README.md** - Project overview, deployment guides, and feature descriptions
- **docs/API/** - Detailed API documentation for all endpoints
- **docs/Agents/** - Agent development and usage guides

When contributing:
- Keep documentation in sync with code changes
- Use clear, concise language
- Include code examples where helpful
- Follow existing documentation style and formatting

## Getting Help

If you need help:

1. Check existing [GitHub Issues](https://github.com/AKercha1/aicore/issues)
2. Review the documentation in `docs/`
3. Ask questions in a new GitHub issue with the `question` label

## License

By contributing to AI Core, you agree that your contributions will be licensed under the MIT License.

---

Thank you for contributing to AI Core! Your contributions help make this project better for everyone.