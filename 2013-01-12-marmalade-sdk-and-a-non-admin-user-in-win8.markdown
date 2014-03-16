Title: Marmalade SDK and a non-admin user in Win8
Date: 2013-01-12 12:41
Tags: Blackberry, BB10, Marmalade, PlayBook, Windows 8

If you installed and configured for together work Visual C++ 2010 Express Edition and Marmalade SDK under Windows 8 and then try to run a "Hello World" Marmalade example project by using Windows account without administrative rights, you can find in the console window the following error message __"Visual Studio 6.0 requested but not found installed (MSDEV.EXE)"__:

![Error example]({filename}/images/marmalade/marmalade01.png)

That is because you have no available configuration for your account, only defaults.

This issue can be easily fixed in two steps:

**1 Defining S3E_DIR environment variable:**

a) create for you account S3E_DIR environment variable points to s3e subdirectory in you Marmalade SDK installation directory
![Changing environment variables]({filename}/images/marmalade/marmalade02.png)
b) or change type of this environment variable from "user variable" to "system" for account from you installed Marmalade SDK
![Changing S3E_DIR environment variable]({filename}/images/marmalade/marmalade03.png)

**2 Configuring Marmalade to use Visual C++ 2010 Express Edition:**

a) replace mkb.config file located in %USERPROFILE%\AppData\Roaming\Marmalade\ from the account with administrative rights;

b) or just add to the end of the %USERPROFILE%\AppData\Roaming\Marmalade\mkb.config file following lines

```bat
rvct-version=none
default-buildenv=VC10
```

so the entire %USERPROFILE%\AppData\Roaming\Marmalade\mkb.config file will be looked like

```bat
##
# Default options for mkb command.
# You can specify any of the options listed --help in this file.
# For example:
#
# buildenv=VC6
# no-make
##
rvct-version=none
default-buildenv=VC10
```