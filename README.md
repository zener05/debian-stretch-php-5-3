# How to install PHP 5.3 on Debian Stretch
There is a problem with installing older PHP versions on new Debian systems. This is a quick tutorial how to do it.

## Download DEB packages
Luckily we can use repositories from Ubuntu and its Launchpad. I use [repository from SergeyD](https://launchpad.net/~sergey-dryabzhinsky/+archive/ubuntu/php53).

Select all packages that you want to use - depends on your architecture. For example I use these:

- [php53-cli](https://launchpad.net/~sergey-dryabzhinsky/+archive/ubuntu/php53/+files/php53-cli_5.3.29-1sergeyd14.4~xenial1_amd64.deb)
- [php53-common](https://launchpad.net/~sergey-dryabzhinsky/+archive/ubuntu/php53/+files/php53-common_5.3.29-1sergeyd14.4~xenial1_all.deb)
- [php53-fpm](https://launchpad.net/~sergey-dryabzhinsky/+archive/ubuntu/php53/+files/php53-fpm_5.3.29-1sergeyd14.4~xenial1_amd64.deb)
- [php53-mod-curl](https://launchpad.net/~sergey-dryabzhinsky/+archive/ubuntu/php53/+files/php53-mod-curl_5.3.29-1sergeyd14.4~xenial1_amd64.deb)
- [php53-mod-gd](https://launchpad.net/~sergey-dryabzhinsky/+archive/ubuntu/php53/+files/php53-mod-gd_5.3.29-1sergeyd14.4~xenial1_amd64.deb)
- [php53-mod-json](https://launchpad.net/~sergey-dryabzhinsky/+archive/ubuntu/php53/+files/php53-mod-json_5.3.29-1sergeyd14.4~xenial1_amd64.deb)
- [php53-mod-mbstring](https://launchpad.net/~sergey-dryabzhinsky/+archive/ubuntu/php53/+files/php53-mod-mbstring_5.3.29-1sergeyd14.4~xenial1_amd64.deb)
- [php53-mod-pgsql](https://launchpad.net/~sergey-dryabzhinsky/+archive/ubuntu/php53/+files/php53-mod-pgsql_5.3.29-1sergeyd14.4~xenial1_amd64.deb)
- [php53-mod-simplexml](https://launchpad.net/~sergey-dryabzhinsky/+archive/ubuntu/php53/+files/php53-mod-simplexml_5.3.29-1sergeyd14.4~xenial1_amd64.deb)
- [php53-mod-xml](https://launchpad.net/~sergey-dryabzhinsky/+archive/ubuntu/php53/+files/php53-mod-xml_5.3.29-1sergeyd14.4~xenial1_amd64.deb)
- [php53-mod-xsl](https://launchpad.net/~sergey-dryabzhinsky/+archive/ubuntu/php53/+files/php53-mod-xsl_5.3.29-1sergeyd14.4~xenial1_amd64.deb)

## Install packages
Install all packages at once by dpkg.

`sudo dpkg -i *.deb`

## Done
Profit!

Test PHP installation: `/usr/bin/php53 -v`

### Misc
I use PHP as FPM so there should be more things to do.
- Edit file `/etc/init.d/php53-fpm` and update variable `exec_prefix` (missing `/` at the beginning).
- Create PHP-FPM pool file. Sample file is located here - `/etc/php53/fpm/pool.d/pool-www-data.conf.example`. You can use sample config file as yours and just rename it.
- Enable PHP-FPM service (uncomment it in `/etc/default/php53-fpm`)

I use apache2 as webserver and this is my VirtualHost setting for web running on PHP 5.3
```
<VirtualHost *:80>
        ServerName web.example.com
        DocumentRoot "/var/www/web.example.com"

        ProxyPassMatch ^/(.*\.php)$ fcgi://127.0.0.1:9000/var/www/web.example.com

        <Directory /var/www/web.example.com>
                AllowOverride All
        </Directory>
</VirtualHost>
```
