# Deploy Laravel on Infomaniak shared hosting

How to deploy a Laravel app (v. 8.x.x) on Infomaniak shared hosting

## Prepare site, database & access on Infomaniak

- Create a site on Infomaniak
- PHP version should be >= 7.3 (ideally 7.4) - [Laravel Server Requirements](https://laravel.com/docs/8.x#server-requirements)
- Enable `proc_open` for the site (https://www.infomaniak.com/en/support/faq/173/enabling-shell-exec-proc-open-etc-functions)
- Create an SSH access for this site
- Create a database with associated user

## Upload your Laravel app

### Via git

1. `cd` to the correct directory
1. `git clone` your project

#### If the project is private and you have 2FA, you can use Github Token

1. Create a token https://github.com/settings/tokens with the correct rights (repo)
1. `git clone` your repo with https
1. Enter your username and the token for the password

## Make the Infomaniak site point to the `/public` folder

## Update the .env file with the correct info

## Configure Laravel

1. `composer install`
1. `php artisan key:generate`
1. `php artisan migrate:fresh --seed`
1. `php artisan storage:link`

## Optimization

- `composer install --optimize-autoloader --no-dev`
- `php artisan config:cache`
- `php artisan route:cache`

> More https://laravel.com/docs/6.x/deployment

---

## To deploy new changes

### Down the site
`php artisan down`

### Update the site
- `git pull`
- `composer install`
- `php artisan migrate`
- Restart FPM (optional) `echo "" | sudo -S service php7.3-fpm reload`
- Restart queue (optional) `php artisan queue:restart`
- Clear cache (optional) `php artisan cache:clear`

### Up the site
`php artisan up`

---

## Issues

**Force HTTPS**

Add the following line to the `public/.htaccess`
```
RewriteCond %{HTTPS} !=on
RewriteRule ^ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
```

**Index column size too large**

Migrate MySQL version to 5.7+

**Can't use Tinker**

Set in `.env` file the following variable:
```
XDG_CONFIG_HOME=./.psysh
```

---

## Infomaniak Support FAQ

> https://www.infomaniak.com/en/support/faq/2119/installing-laravel-51-on-a-web-hosting-or-a-cloud-server
