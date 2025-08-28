# Contributing to Nexus

Thank you for your interest in contributing to **Nexus**! We welcome contributions from the community and are excited to collaborate with you.

## 📋 Table of Contents

- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Development Setup](#development-setup)
- [How to Contribute](#how-to-contribute)
- [Coding Standards](#coding-standards)
- [Testing Guidelines](#testing-guidelines)
- [Pull Request Process](#pull-request-process)
- [Issue Guidelines](#issue-guidelines)
- [Architecture Guidelines](#architecture-guidelines)
- [Documentation](#documentation)

## 🤝 Code of Conduct

By participating in this project, you agree to abide by our [Code of Conduct](CODE_OF_CONDUCT.md). Please be respectful, inclusive, and professional in all interactions.

## 🚀 Getting Started

### Prerequisites

- **.NET 10 SDK** or later
- **Visual Studio 2022** (recommended) or **VS Code** with C# extension
- **Git** for version control
- **Windows 10/11** for Windows-specific features (Linux support coming soon)

### Development Setup

1. **Fork the repository** on GitHub
2. **Clone your fork** locally:
   ```bash
   git clone https://github.com/YOUR-USERNAME/nexus.git
   cd nexus
   ```
3. **Add the upstream remote**:
   ```bash
   git remote add upstream https://github.com/behl1anmol/nexus.git
   ```
4. **Restore dependencies**:
   ```bash
   dotnet restore
   ```
5. **Build the solution**:
   ```bash
   dotnet build
   ```
6. **Run tests** to ensure everything works:
   ```bash
   dotnet test
   ```

## 🛠 How to Contribute

### Types of Contributions

- 🐛 **Bug fixes**
- ✨ **New features** (automation tools, platform support)
- 📚 **Documentation improvements**
- 🧪 **Test coverage enhancements**
- 🔧 **Performance optimizations**
- 🎨 **Code refactoring**

### Workflow

1. **Create a feature branch** from `main`:
   ```bash
   git checkout -b feature/your-feature-name
   ```
2. **Make your changes** following our coding standards
3. **Add or update tests** for your changes
4. **Update documentation** if necessary
5. **Commit your changes** with clear messages
6. **Push to your fork** and **create a Pull Request**

## 📝 Coding Standards

### General Guidelines

- Follow **C# coding conventions** and **.NET guidelines**
- Use **meaningful names** for variables, methods, and classes
- Write **self-documenting code** with clear intent
- Add **XML documentation** for public APIs
- Keep methods **small and focused** (single responsibility)

### Naming Conventions

```csharp
// Classes: PascalCase
public class ScreenshotService { }

// Interfaces: IPascalCase
public interface IScreenshotService { }

// Methods and Properties: PascalCase
public string GetScreenshot() { }
public bool IsEnabled { get; set; }

// Private fields: _camelCase
private readonly ILogger _logger;

// Local variables: camelCase
var screenshotPath = "path/to/screenshot";

// Constants: PascalCase
public const int MaxRetries = 3;
```

### Code Structure

- Use **dependency injection** for loose coupling
- Follow **clean architecture** principles
- Follow **Result pattern** for error handling (FluentResults)
- Separate concerns with **layered architecture**
- Use **async/await** for I/O operations

### Example Code Style

```csharp
/// <summary>
/// Captures a screenshot of the desktop and returns the result.
/// </summary>
/// <param name="request">The screenshot request parameters</param>
/// <param name="cancellationToken">Cancellation token</param>
/// <returns>A result containing the screenshot data or error information</returns>
public async Task<Result<ScreenshotResponse>> CaptureScreenshotAsync(
    ScreenshotRequest request, 
    CancellationToken cancellationToken)
{
    try
    {
        _logger.LogInformation("Capturing screenshot with format {Format}", request.Format);
        
        var screenshot = await _screenshotService
            .CaptureAsync(request.Format, cancellationToken);
            
        return Result.Ok(new ScreenshotResponse 
        { 
            Data = screenshot.Data,
            Format = screenshot.Format,
            Timestamp = DateTime.UtcNow
        });
    }
    catch (Exception ex)
    {
        _logger.LogError(ex, "Failed to capture screenshot");
        return Result.Fail($"Screenshot capture failed: {ex.Message}");
    }
}
```

## 🧪 Testing Guidelines

### Test Structure

- **Unit Tests**: Test individual components in isolation
- **Integration Tests**: Test component interactions
- **End-to-End Tests**: Test complete workflows

### Testing Framework

- Use **xUnit** for all test projects
- Use **Moq** or **NSubstitute** for mocking
- Use **FluentAssertions** for readable assertions

### Test Naming Convention

```csharp
[Fact]
public async Task CaptureScreenshotAsync_ValidRequest_ReturnsSuccessResult()
{
    // Arrange
    var request = new ScreenshotRequest { Format = ImageFormat.Png };
    var expectedData = new byte[] { 1, 2, 3 };
    
    _mockScreenshotService
        .Setup(x => x.CaptureAsync(It.IsAny<ImageFormat>(), It.IsAny<CancellationToken>()))
        .ReturnsAsync(new Screenshot { Data = expectedData });

    // Act
    var result = await _screenshotHandler.CaptureScreenshotAsync(request, CancellationToken.None);

    // Assert
    result.IsSuccess.Should().BeTrue();
    result.Value.Data.Should().BeEquivalentTo(expectedData);
}
```

### Test Coverage

- Aim for **80%+ code coverage**
- Focus on **business logic** and **critical paths**
- Test **error scenarios** and **edge cases**
- Mock external dependencies

## 📬 Pull Request Process

### Before Submitting

1. **Sync with upstream**:
   ```bash
   git fetch upstream
   git checkout main
   git merge upstream/main
   ```
2. **Rebase your feature branch**:
   ```bash
   git checkout feature/your-feature
   git rebase main
   ```
3. **Run all tests**:
   ```bash
   dotnet test
   ```
4. **Check code formatting**:
   ```bash
   dotnet format
   ```

### PR Requirements

- ✅ **Clear title** describing the change
- ✅ **Detailed description** of what and why
- ✅ **Link to related issues** (if applicable)
- ✅ **All tests passing**
- ✅ **Code coverage maintained** or improved
- ✅ **Documentation updated** (if needed)
- ✅ **No merge conflicts**

### PR Template

```markdown
## Description
Brief description of changes made.

## Type of Change
- [ ] Bug fix (non-breaking change which fixes an issue)
- [ ] New feature (non-breaking change which adds functionality)
- [ ] Breaking change (fix or feature that would cause existing functionality to not work as expected)
- [ ] Documentation update

## Testing
- [ ] Unit tests added/updated
- [ ] Integration tests added/updated
- [ ] Manual testing completed

## Checklist
- [ ] Code follows project coding standards
- [ ] Self-review completed
- [ ] Comments added for hard-to-understand areas
- [ ] Documentation updated
- [ ] Tests added and passing
```

## 🐛 Issue Guidelines

### Bug Reports

Use the **Bug Report** template and include:
- Clear **steps to reproduce**
- **Expected vs actual behavior**
- **Environment details** (OS, .NET version, etc.)
- **Error messages** or **stack traces**
- **Screenshots** (if UI-related)

### Feature Requests

Use the **Feature Request** template and include:
- **Clear description** of the proposed feature
- **Use case** and **motivation**
- **Acceptance criteria**
- **Implementation suggestions** (optional)

## 🏗 Architecture Guidelines

### Project Structure

Follow the established architecture:

```
src/
├── nexus.host/                    # Entry point, MCP server
├── nexus.core/                    # Domain logic, abstractions
├── nexus.infrastructure.Windows/  # Windows-specific implementations
├── nexus.infrastructure.Linux/    # Linux-specific implementations
└── nexus.shared/                  # Common DTOs and utilities
```

### Dependency Flow

- **Host** → **Core** → **Infrastructure**
- **Core** should not depend on **Infrastructure**
- Use **interfaces** for cross-layer communication
- Register dependencies in **DI container**

### Adding New Tools

1. **Define the contract** in `nexus.core`
2. **Implement the service** with proper error handling
3. **Create platform-specific implementations**
4. **Add comprehensive tests**
5. **Update documentation**

## 📚 Documentation

### XML Documentation

Add XML comments to all public APIs:

```csharp
/// <summary>
/// Represents a service for capturing desktop screenshots.
/// </summary>
public interface IScreenshotService
{
    /// <summary>
    /// Captures a screenshot asynchronously.
    /// </summary>
    /// <param name="format">The desired image format</param>
    /// <param name="cancellationToken">Cancellation token</param>
    /// <returns>A task containing the screenshot data</returns>
    Task<Screenshot> CaptureAsync(ImageFormat format, CancellationToken cancellationToken);
}
```

### README Updates

- Update **feature lists** for new tools
- Add **usage examples** for new functionality
- Update **installation instructions** if needed

## 🚦 Getting Help

- 💬 **Discussions**: Use GitHub Discussions for questions
- 🐛 **Issues**: Report bugs via GitHub Issues
- 📧 **Email**: Contact maintainers for sensitive matters

## 🙏 Recognition

Contributors will be recognized in:
- **CONTRIBUTORS.md** file
- **Release notes** for significant contributions
- **GitHub contributors** section

---

Thank you for contributing to Nexus! Your efforts help make desktop automation more accessible to everyone. 🚀
