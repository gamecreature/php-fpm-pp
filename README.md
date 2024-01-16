# freebsd-php-fpm-pp

This is a FreeBSD init script to run multiple php-fpm instances.

This script enables the possibility to run a php-fpm instance per pool, which makes them monitorable and controllable.
By having separate processes it becomes possible, to resource and monitor control pools via monit.

## Usage

Simply copy the `php-fpm-pp` file in `/usr/local/etc/rc.d/` and enable php-fpm-pp:

`sysrc php_fpm_pp="YES"`

## Configuration

Place the full php-fpm configuration files in `/usr/local/etc/php-fpm-pp.d`. with the filename `config_name.conf`.
Keeping the pool configuration files in `/usr/local/etc/php-fpm.d/` enables
you to re-enable the normal php-fpm script (if needed).

* Copy and configure the base file `cp /usr/local/etc/php-fpm.conf /usr/local/php-fpm-pp.d/config_name.conf`
* Edit this file:
  * Comment out the 'pid' option. (So it uses the config_name). Very Important!
  * Change the include to include the correct pool config file(s):
    `include=/usr/local/etc/php-fpm.d/pool_name.conf`

## Pid and sock files

The .pid and .sock files are placed in /var/run/php-fpm.d/ as
poolname.sock and config_name.pid

## Managing all pools

```sh
service php-fpm-pp start
service php-fpm-pp stop
service php-fpm-pp configtest
```

Or per configuration

```sh
service php-fpm-pp start config_name
service php-fpm-pp stop config_name
service php-fpm-pp configtest config_name
```

## Pool Specific php.ini configuration

To allow the tweaking of for example the opcache it is possible to supply extra
php-fpm (ini config) flags via the /etc/rc.conf file:

For example:

```sh
php_fpm_pp_pool1_flags="-d opcache.memory_consumption=128 -d opcache.max_accelerated_files=10000"
php_fpm_pp_pool2_flags="-d opcache.memory_consumption=256 -d opcache.max_accelerated_files=20000"
```



