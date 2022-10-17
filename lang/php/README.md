This documentation describes a set of guidelines and a variety of resources for *setting up and running* ***PHP*** applications.  There are a plethora of methods to accomplish the described tasks, not all of them are listed here. It is left to the reader to choose the best technology and methods for their particular situation.

## Contents

- [Install](#install)
  - [scoop.sh (windows)](#scoop-sh)
- [Setup w/ Apache (scoop)](#setup-apache)
- [Setup Support](#setup-support)
	-	[PHP ini File Support](#php-ini-support)
		- [Environment Path](#environment-path-has-php)
    - [PHP Info](#list-php-information)
    - [Show loaded PHP ini](#show-which-php-ini-is-in-memory)
    - [List CLI Modules](#list-cli-modules)
- [LDAP](#ldap)
  - [PHP Extension](#php-extension)
  - [Create Default Paths](#create-default-paths)
  - [Example Config](#example-ldapconf)
  - [Get LDAP Cert](#get-ldap-cert)
  - [PHP LDAP Troubleshooting](#php-ldap-troubleshooting)
- [DB Connection](#db-connect)
  - [Oracle](#oracle)
    - [Download Client Library](#download-client-library)
    - [Setup Extensions](#oracle-php-ini)
 - [PDO](#db-pdo)
    - [Apache Crashes](#apache-crashes-when-using-pdo)


## Install
### scoop.sh
``` scoop install php ```


## Setup Apache

## Setup Support

### PHP ini Support

#### Environment Path has PHP
When running php under Apache, the path variable must have the PHP path.  This should be in the same enviornment where Apache (httpd) is running.

#### List php information.
``` php -i ```

#### Show which php INI is in memory.
```  php --ini ```

#### List Cli Modules
``` php -m ```


## LDAP
The most important part about setting up LDAP with PHP and Apache seems to be the  paths to the ldap.conf and LDAP certs.  PHP uses ldap.conf to locate certs and configure client communication.  Getting the certs can be easily done using the openssl command with the intended ldap server url.

### PHP Extension
Enable the php extension in PHP ini.
``` extension=ldap ```

### Create default paths.
Create the default paths for open ldap.  Create a path for a client LDAP config and a path for the LDAP certificates.

```
# For Windows
c:\openldap\sysconf\
c:\openldap\sysconf\certs
```

### Example ldap.conf
Create an Ldap config at: ```/c/openldap/sysconf/ldap.conf```

```
TLS_REQCERT demand
# TLS_CIPHER_SUITE SHA
# TLS_PROTOCOL_MIN 3.4
TLS_CACERT C:\openldap\sysconf\certs\whitepages.noaa.gov.cert
```

### Get LDAP .cert
Connect to the ldap server using openssl in order to get the ldap certs.  The cert should be placed in the configured ldap cert location listed above in the example config.

```
# Ref: Using openssl to get certificates.
# https://ldapwiki.com/wiki/Obtain%20a%20Certificate%20from%20Server
openssl s_client -showcerts -connect whitepages.noaa.gov:636 -servername whitepages.noaa.gov </dev/null 2>/dev/null > whitepages.noaa.gov.cert
```

### PHP LDAP Troubleshooting
The following script was useful to debug and verify a proper setup for TLS and LDAP certs and config via PHP CLI & Web Server.

```
<?php
// Debug script for PHP LDAP.

ini_set('display_errors', 1);
error_reporting(E_ALL);

$user = '[username]';  // Without brackets, ex. first.last.
$HARD_PASS = '[pass]';  // Hard code a password for web testing.
$pass = $argv && $argv[1]
  ? $argv[1]     // Wrap argugment in '' if using CLI: php test.php 'pa$$'.
  : $HARD_PASS;

$server = "ldaps://whitepages.noaa.gov";
$dn = "uid=".strtolower($user).",ou=people,dc=noaa,dc=gov";

ldap_set_option(null, LDAP_OPT_DEBUG_LEVEL, 9);

// Connect (Create Connection)
$con = ldap_connect($server) or die(" -- Could not connect");
$errno = ldap_errno($con);
if ( $errno ) {
  error_log(" -- LDAP - Connect error $errno  (".ldap_error($con).")");
}

// Use Connection.
$bind = @ldap_bind($con, $dn, $pass);
$errno = ldap_errno($con);
if ( $errno ) {
  error_log(" -- LDAP - Bind error $errno  (".ldap_error($con).")");
}

if ($bind)
  echo ' -- ldap bind is a success!';
else
  echo ' -- ldap bind is not good.';

```


## DB Connect
### Oracle
Download Oracle Instant Client and make sure paths for the application environment (shell) are configured properly.  Once those steps are done, enabling the php extension and provided a good connection string are the last steps.

#### Download Client Library
Install Oracle Instant Client via the preferred method.  Listed below is one example.

```
# Use the scoop Windows package manager to isntall Oracle Onstant Client.
scoop install oracle-instant-client
scoop install oracle-instant-client-sdk  # Not certain this is required.
scoop install oracle-instant-client-odbc # Not certain this is required.
```

#### Oracle PHP Ini
```
# enable the required extension.  Apache restart might be necessary.
extension=oci8_19  # enable for oci_connect functions.  (preferred)
extension=pdo_oci   # enable for PDO with OCI drivers.
```


### PDO

#### Apache Crashes When using PDO.
Pulled from SO.  This apache config addition increases memory stack to help solve the error message listed below.

##### Error Message
AH00428: Parent: child process ```[pid]``` exited with status 3221225477 -- Restarting.

```
<IfModule mpm_winnt_module>
    ThreadStackSize 8888888
</IfModule>
```
