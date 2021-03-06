# Lando - WordPress LAMP Stack

## Local Development Environment

Local development environment is handled via [Lando](https://lando.dev/). Lando documentation can be viewed [here](https://docs.lando.dev/)

### Setup, install and run Lando

### 1. Environment Variables

1. Save `dotenv-example` as `.env` in the same root folder.
2. Open `.env` file and amend vales. For database credentials please refer to the lando [lamp documentation](https://docs.lando.dev/config/lamp.html#connecting-to-your-database) or use the below:

```
DB_HOST=database
DB_PASSWORD=lamp
DB_NAME=lamp
DB_USER=lamp
```

### 2. Configure Lando

Lando configuration can be amended via the `.lando.yml` file. For more information on configureing the Lando development environment please refer to the [Lando documentation](https://docs.lando.dev/basics/first-app.html#hello-world).

If Lando is not installed, head over to [Lando documentation](https://docs.lando.dev/basics/installation.html#system-requirements) for system requirements and installation instructions

### 3. Run Lando

Once configured cd into the project directory. Lando can be ran via the below CLI command:

```
lando start
```

After a successful start up you should see something similar to the below:

```
 NAME                  locowise-local
 LOCATION              /Users/some.user/Desktop/locowise-marketing-site
 SERVICES              appserver_nginx, appserver, database, mailhog
 APPSERVER_NGINX URLS  https://localhost:32805
                       http://localhost:32807
                       http://locowise-local.lndo.site:8000/
                       https://locowise-local.lndo.site/
 MAILHOG URLS          http://localhost:32804
                       http://mail.locowise-local.lndo.site:8000/
```

## Database access, Source files and Services

### Database access

To gain access to the database locally use [Sequel Pro](https://www.sequelpro.com/) or another similar application. Use the below connection credentials:

```
Host: 127.0.0.1
Username: lamp
Password: lamp
Database: lamp
Port: [Port number generated by Lado]
```

every time a development environment is started Lando will create random port numbers for services such as databases and mailhog. To get retrieve these port number enter the below terminal commands in the project folder:

```
lando info
```

Lando will list out useful information on the current running environment. The database port number value can be found under the `external_connection` variable.

### 4. Importing a database

Instructions form importing a database via the Lando CLI can be found on the [official Lando documentation](https://docs.lando.dev/config/lamp.html#importing-your-database). Alternatively enter the below command via the Lando CLI:

```
lando db-import database.sql.gz
```

### 5. Update absolute paths in the Database

Absolute paths will require amending when creating another site instance. This can be achieved by executing the below SQL queries via the mysql CLI. The below CLI command Drops into a MySQL shell on the Lando database service:

```
lando mysql lamp
```

**Note: Ensure the OLD_URL and NEW_URL variables are amended with the required URL’s**

```
UPDATE wp_options SET option_value = replace(option_value, 'OLD_URL', 'NEW_URL') WHERE option_name = 'home' OR option_name = 'siteurl';
UPDATE wp_posts SET guid = replace(guid, 'OLD_URL','NEW_URL');
UPDATE wp_posts SET post_content = replace(post_content, 'OLD_URL', 'NEW_URL');
UPDATE wp_postmeta SET meta_value = replace(meta_value,'OLD_URL','NEW_URL');
UPDATE wp_options SET option_value = replace(option_value, 'OLD_URL', 'NEW_URL') WHERE option_name = 'home' OR option_name = 'siteurl';
UPDATE wp_posts SET guid = replace(guid, 'OLD_URL','NEW_URL');
UPDATE wp_posts SET post_content = replace(post_content, 'OLD_URL', 'NEW_URL');
UPDATE wp_postmeta SET meta_value = replace(meta_value,'OLD_URL','NEW_URL');
```

### Source files

By default this repository is setup to use WordPress. However any source files can be added to the project.

The Lando configuration in the repo exposes `/web/public_html` to port 80 so source files should be placed here.

### WP Config

A default `wp-config.php` has been added to the `/web/public_html`. **Do not change or remove this document**.

### Loaded Services

Lando will also load the MailHog service. This service will catch outgoing mail from the development server.

To access the MailHog GUI navigate your browser to [http://mail.website-local.lndo.site:8000](http://mail.website-local.lndo.site:8000/) once Lando has completed loading the development environment.