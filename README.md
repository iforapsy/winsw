winsw: Windows service wrapper in less restrictive license
=========================

[![Build status](https://ci.appveyor.com/api/projects/status/i94752yal9iy77in?svg=true)](https://ci.appveyor.com/project/oleg-nenashev/winsw)
[![NuGet](https://img.shields.io/nuget/v/WinSW.svg)](https://www.nuget.org/packages/WinSW/)

WinSW is an executable binary, which can be used to wrap and manage a custom process as a Windows service.
Once you download the installation package, you can rename `winsw.exe` to any name, e.g. `myService.exe`.

### Why?

See the [project manifest](MANIFEST.md).

### Download

Starting from WinSW `2.x`, the releases are being hosted on [GitHub](https://github.com/kohsuke/winsw/releases) and [nuget.org](https://www.nuget.org/packages/WinSW/).

Due to historical reasons, the project also uses  [Jenkins Maven repository](https://jenkins.io/index.html)  as a secondary source. 
Binaries are available [here](http://repo.jenkins-ci.org/releases/com/sun/winsw/winsw/). 

The executables in all sources are [strong-named assemblies](https://msdn.microsoft.com/en-us/library/wd40t7ad%28v=vs.110%29.aspx), which are being singed by randomly generated keys.
Do not rely on such strong names for security (as well as on other strong names as it recommended by Microsoft). 
They provide a unique identity only.

### Usage

WinSW is being managed by configuration files: [Main XML Configuration file](doc/xmlConfigFile.md) and [EXE Config file](doc/exeConfigFile.md).

Your renamed `winsw.exe` binary also accepts the following commands:

* `install` to install the service to Windows Service Controller.
  This command requires some preliminary steps described in the [Installation Guide](doc/installation.md).
* `uninstall` to uninstall the service. The opposite operation of above.
* `start` to start the service. The service must have already been installed.
* `stop` to stop the service.
* `restart` to restart the service. If the service is not currently running, this command acts like `start`.
* `status` to check the current status of the service.
 * This command prints one line to the console.
 * `NonExistent` indicates the service is not currently installed
 * `Started` to indicate the service is currently running
 * `Stopped` to indicate that the service is installed but not currently running.

### Supported .NET versions

WinSW `1.x` Executable is being built with a .NET Framework `2.0` target, and by defaut it will work only for .NET Framework versions below `3.5`.
On the other hand, the code is known to be compatible with .NET Framework `4.0` and above.
It is possible to declare the support of this framework via the `exe.config` file.
See the [Installation Guide](doc/installation.md) for more details.

WinSW `2.x` offers two executables, which declare .NET Frameworks `2.0` and `4.0` as targets.
Naming and download sources for these binaries are currently in flux.

### Documentation

* [Installation Guide](doc/installation.md) - Describes the installation process for different systems and .NET versions
* [Release notes](CHANGELOG.md)
* Configuration:
 * [Main XML Configuration file](doc/xmlConfigFile.md)
 * [EXE Configuration File](doc/exeConfigFile.md)
 * [Logging and Error Reporting](doc/loggingAndErrorReporting.md)
 * [Extensions](doc/extensions/extensions.md)
* Use-cases:
 * [Self-restarting services](doc/selfRestartingService.md)
 * [Deferred File Operations](doc/deferredFileOperations.md)
* Configuration Management:
 * [Puppet Forge Module](doc/puppetWinSW.md)

### Release lines

#### WinSW 2.x

This is a new release line under active development.

Major changes since 1.x:
* New executable package targeting the .NET Framework `4.0`. .NET Framework `2.0` is still supported.
* Migration of the logging subsystem to `Apache log4net`
* Internal [extension engine](doc/extensions/extensions.md), which allows extending the wrapper's behavior.

See the full changelog in the [release notes](CHANGELOG.md#20).

The version `2.x` is **fully compatible** with the `1.x` configuration file format, 
  hence the upgrade procedure just requires replacement of the executable file.

#### WinSW 1.x

This is an old baseline of WinSW.
Currently it is in the maintenance-only state.
New versions with fixes may be released on-demand.

### Build Environment

* IDE: [Visual Studio Community 2013](http://www.visualstudio.com/en-us/news/vs2013-community-vs.aspx) (free for open-source projects)
* `winsw_key.snk` should be available in the project's root in order to build the executable
 * You can generate the certificate in "Project Settings/Signing"
 * The certificate is in <code>.gitignore</code> list. Please do not add it to the repository
