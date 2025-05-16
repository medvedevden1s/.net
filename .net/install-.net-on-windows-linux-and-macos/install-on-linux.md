# Install on Linux

Install the SDK (which includes the runtime) if you want to develop .NET apps. Or, if you only need to run apps, install the Runtime. If you're installing the Runtime, we suggest you install the **ASP.NET Core Runtime** as it includes both .NET and ASP.NET Core runtimes.

Use the `dotnet --list-sdks` and `dotnet --list-runtimes` commands to see which versions are installed. For more information, see [How to check that .NET is already installed](https://learn.microsoft.com/en-us/dotnet/core/install/how-to-detect-installed-versions).

### Ubuntu 24.10 <a href="#ubuntu-2410" id="ubuntu-2410"></a>

.NET is available in the Ubuntu package manager feeds. The Microsoft package repository no longer contains .NET packages for Ubuntu.

#### Install the SDK <a href="#install-the-sdk" id="install-the-sdk"></a>

The .NET SDK allows you to develop apps with .NET. If you install the .NET SDK, you don't need to install the corresponding runtime. To install the .NET SDK, run the following commands:

```bash
sudo apt-get update && \
  sudo apt-get install -y dotnet-sdk-9.0
```

#### Install the runtime <a href="#install-the-runtime" id="install-the-runtime"></a>

The ASP.NET Core Runtime allows you to run apps that were made with .NET that didn't provide the runtime. The following commands install the ASP.NET Core Runtime, which is the most compatible runtime for .NET. In your terminal, run the following commands:

```bash
sudo apt-get update && \
  sudo apt-get install -y aspnetcore-runtime-9.0
```

As an alternative to the ASP.NET Core Runtime, you can install the .NET Runtime, which doesn't include ASP.NET Core support: replace `aspnetcore-runtime-9.0` in the previous command with `dotnet-runtime-9.0`:

```bash
sudo apt-get install -y dotnet-runtime-9.0
```

### Installing .NET on Linux Distributions

.NET supports a wide range of Linux distributions. If you're installing .NET on any of the following operating systems:

* Alpine
* CentOS Stream
* Debian
* Fedora
* openSUSE
* Red Hat Enterprise Linux (RHEL)

Installation steps are generally similar to what we described and well-documented here [Install .NET on Linux](https://learn.microsoft.com/en-us/dotnet/core/install/)
