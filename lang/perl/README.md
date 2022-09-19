This documentation describes a set of guidelines and a variety of resources for *setting up and running* ***Perl*** applications.  There are a plethora of methods to accomplish the described tasks, not all of them are listed here. It is left to the reader to choose the best technology and methods for their particular situation.

## Contents

- [Install](#install-perl)
  - [scoop.sh (windows)](#scoop-sh)
- [Setup w/ Apache (scoop)](#setup-apache-with-perl)
- [Setup Support](#setup-support)
  - [List Installed Perl Modules](#list-installed-perl-modules)
  - [Diagnose DLLs](#diagnose-suspect-dlls)
  - [Char Encoding When Compiling](#compile--encoding-issues)


## Install perl
### scoop.sh
``` scoop install perl ```

## Setup Apache with perl
### Install mode_perl.so for Apache
Scoop does not install the apache perl module.  These steps provide a method for getting mod_perl for Apache web server.

```
  # Download strawberry perl or the preferred perl version w/ mod_perl.
  curl -o mod_perl-2.0.12-strawberryperl-5.32.1.1-64bit.zip "http://home.apache.org/~stevehay/mod_perl-2.0.12-strawberryperl-5.32.1.1-64bit.zip"

  # Extract the mod_perl.so file to apache/modules.
  unzip -p mod_perl-2.0.12-strawberryperl-5.32.1.1-64bit.zip Apache24/modules/mod_perl.so > ~/scoop/apps/apache/current/modules/mod_perl.so

  # Add the following line to the apache config. (~/scoop/apps/run/apache/current/conf/http.conf)
  LoadModule perl_module modules/mod_perl.so

  # Reload apache and perl should be working.
```


## Setup Support

#### List installed perl modules.
If modules are not found, or errors occur, look for the modules here first.

``` instmodsh  # Enter l to list modules. ```

#### Diagnose suspect DLLS.
Installing and Running depends.exe on a suspect DLL file can help diagnose errors like: dll reference not found, type issues.

``` scoop install depends ```

#### Compile & Encoding Issues?
It is possible character encoding might be off when compiling or installing some perl modules.  Some errant files might benefit from a specific character encoded.
``` use utf8; ```
