# Onoda Takoyaki

The number one takoyaki shop in the Philippines

## How to deploy in Production

There is no complete automatic deployment in Production in the first time. But once all the necessary files are deployed and installed, the next phase of deployment will be a lot easier.

### Prerequisites

If you don't know exactly what to do, contact me.

### Step by step process (first deployment)

* Copy necessary files and folder to public_html 
```
	- app
	- bootstrap
	- config
	- database
	- public
	- resources
	- routes
	- storage
	- artisan
	- composer.json
	- composer.lock
	- package.json
	- package-lock.json
	- server.php
	- webpack.mix.js
	- yarn.lock
```
* Put them into a zip file
* Go to [cPanel File Manager](https://sg11.fcomet.com:2083/cpsess1197729065/frontend/paper_lantern/filemanager/index.html)
* Upload and extract the zip file in `public_html` folder
* Generate a new app key using command: `php artisan key:generate`
* Prepare `.env` and upload to that folder also
* Now open the `cPanel Terminal` and run the following commands
```
    cd public_html
    composer install
    npm install
    php artisan key:generate
```
* Upload the `.htaccess` file in the folder with this content
```
<IfModule mod_rewrite.c>
    <IfModule mod_negotiation.c>
        Options -MultiViews -Indexes
    </IfModule>

    RewriteEngine On

    # Access public folder
    RewriteCond %{REQUEST_URI} !^public/
    RewriteRule ^(.*)$ public/$1 [L] #relative substitution
    
    # Handle Authorization Header
    RewriteCond %{HTTP:Authorization} .
    RewriteRule .* - [E=HTTP_AUTHORIZATION:%{HTTP:Authorization}]

    # Redirect Trailing Slashes If Not A Folder...
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteCond %{REQUEST_URI} (.+)/$
    RewriteRule ^ %1 [L,R=301]

    # Handle Front Controller...
    RewriteCond %{REQUEST_FILENAME} !-d
    RewriteCond %{REQUEST_FILENAME} !-f
    RewriteRule ^ index.php [L]
</IfModule>
RewriteEngine on
RewriteCond %{HTTPS} off
RewriteCond %{HTTP:X-Forwarded-SSL} !on
RewriteCond %{HTTP_HOST} ^onodatakoyaki\.com$ [OR]
RewriteCond %{HTTP_HOST} ^www\.onodatakoyaki\.com$
RewriteRule ^/?$ "https\:\/\/onodatakoyaki\.com\/" [R=301,L]
```
* Now try to visit the site [onodatakoyaki.com](https://onodatakoyaki.com)
* The site must load (hit `CTRL+F5` if not) otherwise repeat the steps above
* Next is you need to setup the database
* Run the following commands
```
    php artisan migrate
    php artisan db:seed
```
* If you encounter an error regarding `Undefined audits` just upload the `Blueprint.php` it is located at:
```
<user>\onodaversioncontrol\vendor\laravel\framework\src\Illuminate\Database\Schema
```
* Upload the `Blueprint.php` in the same path on `public_html` folder
* Then run the final commands for optimization
```
composer dumpautoload -o
php artisan config:cache
```
* Done and Enjoy!


### Step by step process (further deployments)

* TODO
