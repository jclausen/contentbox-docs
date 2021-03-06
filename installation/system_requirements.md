# System Requirements

Let's get started with some system requirements for running ContentBox sites:

## Java
ContentBox will run in any Java 1.7+ enabled servlet container as a WAR or self-contained express.

## ColdFusion (CFML) Engine
ContentBox source can be also be deployed to the following ColdFusion CFML engines:

* Adobe ColdFusion 10+
* Lucee 4.5+


## Databases
ContentBox can run on any database engine that Hibernate ORM supports (https://developer.jboss.org/wiki/SupportedDatabases2), but we officially support the following:

* MySQL 5+ (InnoDB ONLY!)
* Microsoft SQL Server 2008+
* Oracle
* PostgreSQL
* Hypersonic
* H2
* Apache Derby

If you have another database engine that needs ContentBox support, please let us know and we will be able to help you (http://www.ortussolutions.com/contact)

### MySQL Caveats 
If you are running MySQL server with `MyISAM` as your default table engine, you will need to either make the default `InnoDB` or use the `InnoDB` dialect.  You can do this by adding a dialect section to the ORM settings in the `Application.cfc`:  `dialect = "MySQLWithInnoDB"`

```js
this.ormsettings = {
    dialect = "MySQLWithInnoDB
};
```

### PostgreSQL Caveats

We have seen some issues on some versions of PostgreSQL where Boolean values of numerical `1` and `0` are not been translated correctly.  If this is the case in your version of PostgreSQL you will see some `cannot cast Boolean to Integer` exceptions.  If this is the case please contact us and we will show you how to update your installation to allow for these conversions.  

## URL Rewriting

ContentBox relies on Search Engine Safe (SES) URLs and URL Routing.  The majority of CFML engines already allow for these types of URLs out of the box and ContentBox supports it out of the box.  However, if you would like to use full URL rewriting (which we recommend, that's where the `index.cfm` is not showing in the URLs) you will need to use a web server rewriting tool and ContentBox will configure it for you.  So before you install Contentbox make sure that you are using one of the supported rewriting engines show below:

* Apache mod_rewrite
* IIS 7 rewrite module
* Tuckey rewrite filter
* Nginx

The ContentBox installer will create rewrite files automatically for you for Apache, IIS7 and express editions.  For Nginx or other rewrite engines you will need to add the rewrites yourself and then manually modify the routing file: `config/routes.cfm` and remove any reference to `index.cfm`.  That's it!

```js
// Base URL
if( len(getSetting('AppMapping') ) lte 1){
    // Remove the index.cfm
	setBaseURL("http://#cgi.HTTP_HOST##getContextRoot()#/index.cfm");
} else {
    // Remove the index.cfm
	setBaseURL("http://#cgi.HTTP_HOST##getContextRoot()#/#getSetting('AppMapping')#/index.cfm");
}
```

## File Permissions

ContentBox needs some files/folders to be writable at runtime.  We use this for our installer, ForgeBox cloud deployments, auto-updates and more.  The following directories need read/write permissions for the installer only to work:

### Installer

```
{Root}/Application.cfc
{Root}/config/ColdBox.cfc
{Root}/coldbox/system/aop/tmp
```

### Engine

Here are the folders for the core

```
{Root}/coldbox/system/aop/tmp
{Root}/modules/contentbox/content
{Root}/modules/contentbox/email_templates
{Root}/modules/contentbox/themes
{Root}/modules/contentbox/modules_user
{Root}/modules/contentbox/updates
{Root}/modules/contentbox/widgets
```


> **Info** If you will not be using any ForgeBox download deployments then you do not need to enable write permissions for `themes, modules_user, and widgets`.