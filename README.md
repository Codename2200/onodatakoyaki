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
	- .env.staging / .env.production
	- .htaccess.staging / .htaccess.production
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
* Prepare `.env` and upload to that folder also (just remove the `.staging` or `.production`
* Now open the `cPanel Terminal` and run the following commands
```
	cd public_html
	composer install
	npm install
	php artisan key:generate
	php artisan migrate
	php artisan db:seed
	composer dumpautoload -o
	php artisan config:cache
```
* [Optional] Upload the `.htaccess` file in the folder with this content
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
* If you encounter an error regarding `Undefined audits` just upload the `Blueprint.php` it is located at:
```
<user>\onodaversioncontrol\vendor\laravel\framework\src\Illuminate\Database\Schema
```
* Upload the `Blueprint.php` in the same path on `public_html` folder
* [Optional] Then run the final commands for optimization
```
composer dumpautoload -o
php artisan config:cache
```
* Done and Enjoy!


### How to create new repository

* Go to cPanel - File Manager
* Create a new folder ```onodaversioncontrolstg```
* You may want to generate new SSH if necessary
* Then go to cPanel - Git Version Control
* Create new one
* Turn off ```Clone a Repository``` then apply required information
* Now go to your local ``Git Extension / Git Bash``` and clone the new repo
* Commit changes. Done!

### Before you Push anything, do the following

* On your repo files you must have one ```.cpanel.yml```
* Sample content is:
```
---
deployment:
  tasks:
    - export domain=staging.onodatakoyaki.com                             # uncomment for staging deployment
    #- export domain=public_html                                          # uncomment for production deployment
    - export path=/home/onodatak/$domain                                  # deployment path
    - /bin/cp -r app $path                                                # copy app folder
    - /bin/cp -r bootstrap $path                                          # copy bootstrap folder
    - /bin/cp -r config $path                                             # copy config folder
    - /bin/cp -r database $path                                           # copy database folder
    - /bin/cp -r public $path                                             # copy public folder
    - /bin/cp -r resources $path                                          # copy resources folder
    - /bin/cp -r routes $path                                             # copy routes folder
    - /bin/cp -r storage $path
    - /bin/cp composer.json $path
    - /bin/cp composer.lock $path
    - /bin/cp package.json $path
    - /bin/cp package-lock.json $path
    - /bin/cp webpack.mix.js $path
    - /bin/cp yarn.lock $path
```

### How to deploy to Onoda Production Site
#### To Deploy website files do the following
* Find the file ```.cpanel.yml```
* Make sure that the domain is pointing to ```export domain=public_html```
* Save the file, commit changes and Push

#### To Deploy database changes
* Open the `cPanel Terminal` and run the following commands
```
    cd public_html
    composer install
    npm install
    php artisan key:generate
    php artisan migrate
```
* Then create/update ```.htaccess``` file
* Staging .htaccess file
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
RewriteCond %{HTTP_HOST} ^staging\.onodatakoyaki\.com$ [OR]
RewriteCond %{HTTP_HOST} ^www\.staging\.onodatakoyaki\.com$
RewriteRule ^/?$ "https\:\/\/staging\.onodatakoyaki\.com\/" [R=301,L]
RewriteCond %{HTTPS} off
RewriteCond %{HTTP:X-Forwarded-SSL} !on
RewriteCond %{HTTP_HOST} ^onodatak\.sg11\.fcomet\.com$ [OR]
RewriteCond %{HTTP_HOST} ^www\.onodatak\.sg11\.fcomet\.com$
RewriteRule ^/?$ "https\:\/\/onodatak\.sg11\.fcomet\.com\/" [R=301,L]
```
* Production htaccess file
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
RewriteCond %{HTTPS} off
RewriteCond %{HTTP:X-Forwarded-SSL} !on
RewriteCond %{HTTP_HOST} ^onodatak\.sg11\.fcomet\.com$ [OR]
RewriteCond %{HTTP_HOST} ^www\.onodatak\.sg11\.fcomet\.com$
RewriteRule ^/?$ "https\:\/\/onodatak\.sg11\.fcomet\.com\/" [R=301,L]
```
### How to delete stocks in a product
* Find product id
```
select * from products where name like 'Showa'
Product id = 23
```
* Find StockParents Id
```
select * from stock_parents where product_id = 23
StockParentId = 7
```
* Find StockInIds
```
select * from stock_ins where stockparent_id = 7
IDS (7, 51)
```
* Find StockOuts
```
select * from stock_outs where stockin_id IN (7, 51)
```
* Find StockLost
```
select * from stock_losts where stockin_id IN (7, 51)
```
* DELETE OPERATIONS
```
delete from stock_losts where stockin_id IN (7, 51)
delete from stock_outs where stockin_id IN (7, 51)
delete from stock_ins where stockparent_id = 7
delete from stock_parents where product_id = 23
```
### June 13 Changes
