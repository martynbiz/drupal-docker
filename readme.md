## Quick start

```
cp docker-compose.yml-example docker-compose.yml
docker-compose up
```

## Alternative developer setup

The volumes can been configured in docker-compose.yml so that the developer has ease of access to themees, modules, settings etc. However, this will require a little extra work when setting up.

Change the Drupal volumes in the docker-compose.yml to:

```
volumes:
    - ./modules:/var/www/html/modules
    - ./profiles:/var/www/html/profiles
    - ./themes:/var/www/html/themes
    - ./sites:/var/www/html/sites
```

The installation process will fall over a few times so the following steps will be required at certain points

Write the following file to sites/default/default.settings.php when it complains it's missing:

https://raw.githubusercontent.com/drupal/drupal/refs/heads/11.x/sites/default/default.settings.php

(add an extra line return at the end)

If the extra line return still results in an error when it creates the settings.php file, fix the end of the file from:

```php
# if (file_exists($app_root . '/' . $site_path . '/settings.local.php')) {
#   include $app_root . '/' . $site_path . '/settings.local.php';
# }$databases['default']['default'] = array (
  'database' => 'drupal',
```

To:

```php
# if (file_exists($app_root . '/' . $site_path . '/settings.local.php')) {
#   include $app_root . '/' . $site_path . '/settings.local.php';
# }
$databases['default']['default'] = array (
  'database' => 'drupal',
```

Lastly, if it complains of permissions, the following commands should be run within the container:

```
$ docker exec -it drupal-1 bash

# cd web/
# chown -R www-data:www-data modules/ themes/
# chmod 755 modules/ themes/
```

## Install Drush

```
composer require drush/drush
./vendor/bin/drush --version
```

## Troubleshooting

Turn on error reporting

```
drush config:set system.logging error_level verbose -y
```

Revert theme back to original

```
drush theme:enable nmis
```