# AutoFixture.NUnit3

[![License](https://img.shields.io/badge/license-MIT-green)](https://raw.githubusercontent.com/AutoFixture/AutoFixture.NUnit3/master/LICENCE.txt)
[![NuGet version](https://img.shields.io/nuget/v/AutoFixture.NUnit3?logo=nuget)](https://www.nuget.org/packages/AutoFixture.NUnit3)
[![NuGet preview version](https://img.shields.io/nuget/vpre/AutoFixture.NUnit3?logo=nuget)](https://www.nuget.org/packages/AutoFixture.NUnit3)
[![NuGet downloads](https://img.shields.io/nuget/dt/AutoFixture.NUnit3)](https://www.nuget.org/packages/AutoFixture.NUnit3)

[AutoFixture.NUnit3](https://github.com/AutoFixture/AutoFixture.NUnit3) is a .NET library that integrates [AutoFixture](https://github.com/AutoFixture/AutoFixture) with NUnit 3.x, allowing you to effortlessly generate test data for your unit tests.
By automatically populating your test parameters, it helps you write cleaner, more maintainable tests without having to manually construct test objects.

> [!WARNING]
> While this package is still being developed, the NUnit 3 package is deprecated.<br/>
> This package is intended for legacy projects that are still using NUnit 3.x.<br/>
> Use at your own risk.

## Table of Contents

- [Installation](#installation)
- [Getting Started](#getting-started)
- [Integrations](#integrations)
- [License](#license)

## Installation

AutoFixture packages are distributed via NuGet.<br />
To install the packages you can use the integrated package manager of your IDE, the .NET CLI, or reference the package directly in your project file.

```cmd
dotnet add package AutoFixture.NUnit3 --version x.x.x
```

```xml
<PackageReference Include="AutoFixture.NUnit3" Version="x.x.x" />
```

## Getting Started

### Basic Usage

`AutoFixture.NUnit3` provides an `[AutoData]` attribute that automatically populates test method parameters with generated data.

For example, imagine you have a simple calculator class:

```c#
public class Calculator
{
	public int Add(int a, int b) => a + b;
}
```

You can write a test using AutoFixture to provide the input values:

```c#
using NUnit.Framework;
using AutoFixture.NUnit3;

[TestFixture]
public class CalculatorTests
{
    [Test, AutoData]
    public void Add_SimpleValues_ReturnsCorrectResult(
        Calculator calculator, int a, int b)
    {
        // Act
        int result = calculator.Add(a, b);

        // Assert
        Assert.AreEqual(a + b, result);
    }
}
```

### Freezing Dependencies

AutoFixture's `[Frozen]` attribute can be used to ensure that the same instance of a dependency is injected into multiple parameters.

For example, if you have a consumer class that depends on a shared dependency:

```c#
public class Dependency { }

public class Consumer
{
    public Dependency Dependency { get; }

    public Consumer(Dependency dependency)
    {
        Dependency = dependency;
    }
}
```

You can freeze the Dependency so that all requests for it within the test will return the same instance:

```c#
using NUnit.Framework;
using AutoFixture.NUnit3;
using AutoFixture;

[TestFixture]
public class ConsumerTests
{
    [Test, AutoData]
    public void Consumer_UsesSameDependency(
        [Frozen] Dependency dependency, Consumer consumer)
    {
        // Assert
        Assert.AreSame(dependency, consumer.Dependency);
    }
}
```

## Contributing

Contributions to `AutoFixture.NUnit3` are welcome!
If you would like to contribute, please review our [contributing guidelines](https://github.com/AutoFixture/AutoFixture.NUnit3/blob/master/CONTRIBUTING.md) and open an issue or pull request.

## License

AutoFixture is Open Source software and is released under the [MIT license](https://raw.githubusercontent.com/AutoFixture/AutoFixture.NUnit3/master/LICENCE.txt).<br />
The licenses allows the use of AutoFixture libraries in free and commercial applications and libraries without restrictions.

### .NET Foundation

This project is supported by the [.NET Foundation](https://dotnetfoundation.org).
