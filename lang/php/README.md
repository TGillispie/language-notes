This documentation describes a set of guidelines and a variety of resources for *setting up and running* ***PHP*** applications.  There are a plethora of methods to accomplish the described tasks, not all of them are listed here. It is left to the reader to choose the best technology and methods for their particular situation.

## Contents

- [Install](#install)
  - [scoop.sh (windows)](#scoop-sh)
- [Setup w/ Apache (scoop)](#setup-apache)
- [Setup Support](#setup-support)
	-	[PHP ini File Support](#php-ini-support)
		- [Environment Path](#environment-path-has-php)
## Install
### scoop.sh
``` scoop install php ```


## Setup Apache

## Setup Support

### PHP ini Support

#### Environment Path has PHP
When running php under Apache, the path variable must have the PHP path.  This should be in the same enviornment where Apache is running.

#### List php information.
``` php -i ```

#### Show which php INI is in memory.
```  php --ini ```

#### List Cli Modules
``` php -m ```

