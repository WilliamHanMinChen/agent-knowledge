---
title: Development Environment
layout: default
nav_order: 1
parent: Past Forum Posts
---

Following questions are coming from Technical Issues forum of FIT3047 and FIT3048. 

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=435849)
> 
> **Can I use MySQL instead of MariaDB?**

**Q:**
I followed the macOS environment setup video that recommends installing MariaDB. However, another unit I’m taking requires MySQL. Since MariaDB and MySQL conflict, can I use MySQL instead of MariaDB?

**A:**
No problem. Go ahead. They're mostly interchangeable as long as you don't use both at the same time. 

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=435752)
> 
> **testphp.php not executing, just displaying code in browser**

**Q:**When I was doing the step shown in the first screenshot, I opened testphp.php, but instead of showing the expected PHP info page, it only displayed a single line of code. I've checked that PHP is already installed on my system via the terminal, so I’m not sure where the problem is coming from.

```
Index of /
- .DS_Store
- cgi-bin/
- testphp.php
```

```php
<?php phpinfo();
```

**A:**
Looks like you're using macOS. Refer to the setup video at: [link](https://monash.au.panopto.com/Panopto/Pages/Viewer.aspx?id=d549a86d-2bfc-45ba-9095-b12301811755)

Apache interprets PHP files as text file (thus you see codes instead of executed results) usually means of the following: 
- Apache isn't configured to process PHP scripts (see "Install PHP (and integration with Apache)" chapter of the video), OR
- Above is completed, but the Apache service was not restarted to apply those changes (run `brew services restart httpd`)

**Q:**
I tried the method you suggested, but it didn’t seem to work.

**A:**
Consider a lot more detailed on your question and feedback - what exactly you have tried, before doing so, what specific steps you have gone through, what's already done, what is expected, etc.

Setting up development environment is a relatively big collection of things. It's going to be really hard to guess what gone wrong without details.

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=436851)
> 
> **URL not found**

**Q:**
Hi, the URL shows "it works" without phpmyadmin but not found with phpmyadmin, does it mean there's issue of installing php?

```
> localhost:8080/phpmyadmin|
Not Found
The requested URL was not found on this server.

localhost:8080
It works!
```

**A:**
It looks like Apache (httpd) is not yet set up to work with phpMyAdmin. Also, phpMyAdmin still needs to be installed so that you can access it using this link: `http://localhost:8080/phpmyadmin`

*Does it mean there's issue of installing php?*

For this to check, follow along Bill video from 30:00, were it is shown how to create testphp.php file to check if php is running without any issues.

```
screenshot of a phpinfo() page
```

Before reading full answer, please restart your brew services by running `brew services restart httpd`. This will make sure that httpd is running with recent changes. Then check if url can be accessed.

```
(base) admin@localhost ~ % brew services restart httpd
Stopping 'httpd'... (might take a while)
==> Successfully stopped 'httpd' (label: homebrew.mxcl.httpd)
==> Successfully started 'httpd' (label: homebrew.mxcl.httpd)
(base) admin@localhost ~ %
```

Here are the full steps to follow:

**Install PHP**
You need to install PHP before running phpMyAdmin.

Run `brew services` to check if PHP is installed as shown below. Check if you are missing php.

```
(base) admin@localhost ~ % brew services
Name    Status  User    File
httpd   started admin   ~/Library/LaunchAgents/homebrew.mxcl.httpd.plist
mariadb started admin   ~/Library/LaunchAgents/homebrew.mxcl.mariadb.plist
php     none
php@8.2 none
(base) admin@localhost ~ %
```

If it’s not listed, go back to the video and start from 21:00, where it shows how to install PHP. Follow along till the end.

**Install and configure phpMyAdmin**

If PHP and other things are installed but phpMyAdmin is still missing, continue from 37:00 in the video. Bill shows how to install and configure phpMyAdmin in that part.

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=441560)
> 
> **Still shows "it works" after deleting index.html**

**Q:**
Hi, as I followed the video for macOS at 33:20, it doesn't show the list of the files in www, but still "it works" after I deleted the index.html, what issue could it be?

**A:**
There can be various reason:

The first reason I can think of is: You may have accidentally duplicated the www folder while trying to make it easily accessible. In the duplicated version, you removed the index.html file. To confirm which folder is being used by Apache, run the command: "brew info httpd"

```
==> Dependencies
Required: apr r, apr-util r, brotli r, libnghttp2 r, openss1@3, pcre2

==> Caveats
DocumentRoot is /usr/local/var/www.

The default ports have been set in /usr/local/etc/httpd/httpd.conf to 8080 and in /usr/local/etc/httpd/extra/httpd-ssl.conf to 8443 so that httpd can run without sudo.
```

Once you have that information, it will display the full path for example, /usr/local/var/www as highlighted in image. To open that directory in finder, run the following command:

`open /usr/local/var/www`

This will open a Finder window showing all the files in that directory.

If it contains index.html, it means you’ve likely duplicated the folder.

If that’s not the case, I’ll take a look on Thursday and help you figure out what’s going on.

Please note the path demonstrated in this answer is on a Intel Mac. Apple Silicon Macs are different (usually in /opt instead of /usr/local). Make sure you're following what's on your screen/terminal, not solely on those screenshots.

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=449071)
> 
> **phpmyadmin database error**

**Q:**
Hello teaching team, I keep getting this error whenever I try to open privileges for any database. I could not add a new user or connect database to cakephp. For some reason I only started getting this error today and I do not know how to fix it.

```
The phpMyAdmin configuration storage is not completely configured, some extended features have been deactivated. Find out why. Or alternately go to 'Operations' tab of any database to set it up there.

Error, SQL query:
SELECT *, 'r' AS Type" FROM "mysal. procs_priv" WHERE Db = mysql';
MySQL said:
#1030 - Got error 176 "Read page with wrong checksum" from storage engine Aria
```

**A:**
That usually means your database store is corrupted. You can restore the initialised datastore backup come with XAMPP. 

Paste the following prompt into Copilot and follow the instructions: 

> Instructions on using `C:\xampp\mysql\backup` to fix corrupted datastore in `C:\xampp\mysql\data`. But skip instructions on restoring partial files. Do a full restore. Don't reserve files in data folder. Don't mention `ibdata1` at all. Add warning that all previously created databases and users will be reset, where they need to be recreated.

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=465502)
> 
> **localhost error**

**Q:**
i can open my localhost in google but when I click on the onboarding project:

```
> localhost/team000-onboarding-project/

An Internal Error Has Occurred.
Error: An Internal Error Has Occurred.
Back

```

I tried a few ways to fix it but still not working for my onboarding project(try to edit: `config/app_local.php`, or uninstall the xampp and install again, update the composer)

**A:**
That seems to be a very early on (process-wise) error in the bootstrap chain. Since it crashes before CakePHP is involved, means something earlier in the chain is causing issues. 

(You can tell since it's a very "Apache-ish" error message. CakePHP error pages are nicer-looking)

Upon inspecting your team's onboarding git repo it seems the `.htaccess` file (which rewrites all requests to `webroot/index.php` - you may need to know how Apache rewrites URLs to understand what this means, not important at this stage) is missing, thus potentially causing the issue you see. 

Solution: Add the missing file back to your repo. You can borrow the file from CakePHP's official app skeleton: [https://github.com/cakephp/app/blob/HEAD/.htaccess](https://github.com/cakephp/app/blob/HEAD/.htaccess)

**Q:**
We have the .htaccess in our project, but it's on git not the repo, it's still not working with it

```
> localhost/team000-onboarding-project/

Internal Server Error

The server encountered an internal error or misconfiguration and was unable to complete your request.
Please contact the server administrator at postmaster@localhost to inform them of the time this error occurred, and the actions you performed just before this error.
More information about this error may be available in the server error log.

Apache/2.4.58 (Win64) OpenSSL/3.1.3 PHP/8.2.12 Server at localhost Port 80
```

**A:**
I checked the `.htaccess` file in your repo now and it seems to be the right stuff. If it's still triggering error on Apache end, you can check its log in C:\xampp (somewhere in there - usually it's in apache or httpd folder, named "error.log") and find out what's the cause. Maybe something to do with a corrupted configuration file or sort.

**Q:**
now I delate the whole folder of onboarding system and clone again, here it is:

```
Warning: require(C:\xampp\htdocs\team000-onboarding-project/vendor/autoload.php): Failed to open stream: No such file or directory in C: \xampp\htdocs\team000-onboarding-project\webroot\index.php on line 28
Fatal error: Uncaught Error: Failed opening required 'C:\xampp\htdocs\team000-onboarding-project/vendor/autoload.php' (include_path='C:\xampp\php\PEAR') in C:\xampp\htdocs\|team000-onboarding-project\webroot\index.php:28 Stack trace: #0 {main} thrown in C: \xampp\htdocs|team000-onboarding-project|webroot|index.php on line 28
```

**A:**
You need to run "composer install" after you pull from the repo the first time, in order to create the vendor files, etc. These are not stored in git.

**Q:**
I tried everytime when I clone the repository, but it only let me update:

```
C: \xampp\htdocs\team000-onboarding-project>composer install
Installing dependencies from lock file (including require-dev)
Verifying lock file contents can be installed on current platform.
Your lock file does not contain a compatible set of packages. Please run composer update.
Problem 1
- phpunit/php-code-coverage is locked to version 12.3.2 and an update of this package was not requested.
- phpunit/php-code-coverage 12.3.2 requires php >=8.3 - your php version (8.2.12) does not satisfy that requirement
Problem 2
-phpunit/php-+1le-Iterator 15 Locked to version 6.0.0 and an update of this package was not requested.
- phpunit/php-file-iterator 6.0.0 requires php >=8.3 → your php version (8.2.12) does not satisfy that requirement.
Problem 3
- phpunit/php-invoker is locked to version 6.0.0 and an update of this package was not requested.
- phpunit/php-invoker 6.0.0 requires php >=8.3 → your php version (8.2.12) does not satisfy that requirement.
Problem 4
- phpunit/php-text-template is locked to version 5.0.0 and an update of this package was not requested.
- phpunit/php-text-template 5.0.0 requires php >=8.3 → your php version (8.2.12) does not satisfy that requirement.
Problem 5
- phpunit/php-timer is locked to version 8.0.0 and an update of this package was not requested.
- phpunit/php-timer 8.0.0 requires php >=8.3 → your php version (8.2.12) does not satisfy that requirement.
Problem 6
- phpunit/phpunit is locked to version 12.3.5 and an update of this package was not requested.
- phpunit/phpunit 12.3.5 requires php >=8.3 → your php version (8.2.12) does not satisfy that requirement.
Problem 7
```

then I update:

```
- Installing myclabs/deep-copy (1.13.4): Extracting archive
- Installing phpunit/phpunit (11.5.33): Extracting archive
4 package suggestions were added by new dependencies, use 'composer suggest' to see details.
Generating autoload files
60 packages you are using are looking for funding.
Use the 'composer fund' command to find out more!
PHP CodeSniffer Config installed_paths set to ../../cakephp/cakephp-codesniffer,../../slevomat/coding-standard
No security vulnerability advisories
```

it will be the same as the question I asked at first time:

```
An Internal Error Has Occurred.
Error: An Internal Error Has Occurred.
Back
```

**A:**
You can run `composer update` followed up with `composer install` to fix any PHP version issues come from Composer.

This is because different team members are using different versions of PHP locally, which makes preserved package versions in `composer.lock` file invalid.

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=452948)
> 
> **brew services not running mysql and mariadb**

**Q:**
I have two questions

1) I have been trying to set up my environment again like how we did in FIT3047 and have run in terminal brew services start mariadb, but I get this error. How can I fix this?

```
Bootstrap failed: 5: Input/output error
Try re-running the command as root for richer errors.
Error: Failure while executing; /bin/launchct1 bootstrap gui/501 /Users/yukyunglee/Library/LaunchAgents/homebrew.mxcl.mariadb.plist' exited with 5.
```

2) When I run brew services list, mysql status is always noted as 'stopped' although the it has successfully started. Would appreciate any help, thanks.

```
Name                Status
httpd               started
mariadb             stopped
mongodb-community   none
mysql               stopped
php                 started
```

**A:**
It seems the database storage is corrupted because you have both MariaDB and MySQL installed at the same time. They share the same data storage thus shouldn't be used together. 

Enter the following prompt to Monash Microsoft 365 Copilot where you'll get full instructions on resetting the datastore: 

> Instructions to reset a corrupted mariadb database storage where mariadb is installed via Homebrew on macOS. Remind that such corruption is likely caused by installing both mariadb and mysql on the same Homebrew instance. Use `brew postinstall` to reinitialise database storage instead of `mysql_install_db`. Remind to complete setup and configuration with what's in our environment setup video. 

**Q:**
As directed by copilot, what I've tried so far is:
```
brew services stop mariadb
rm -rf /usr/local/var/mysql
brew postinstall mariadb
brew services start mariadb
mysql_secure_installation
```

After running the command `mysql -u root -p` and receiving feedback stating that MariaDB is running, I ran brew services list and it now returns as 'error'.

```
user@localhost ~ % mysql -u root -p
Enter password:
Welcome to the MariaDB monitor. Commands end with ; or \g.
Your MariaDB connection id is 8
Server version: 12.0.2-MariaDB Homebrew
Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.
Type 'help;' or "\h' for help. Type "\c' to clear the current input statement.
MariaDB [(none)]> exit;
Bye
user@localhost ~ % brew services list
Name                Status      User        File
httpd               started     root        ~/Library/LaunchAgents/homebrew.mxcl.httpd.plist
mariadb             error 1     root        ~/Library/LaunchAgents/homebrew.mxcl.mariadb.plist
mongodb-community   none        user
php                 started     user        ~/Library/LaunchAgents/homebrew.mxcl.php.plist
```

**A:**
It may have something to do with root privileges. Homebrew services are supposed to run on local user privileges, not as root. i.e. No "sudo".

To switch services back to user account, you can stop the service with `sudo brew services stop ...`, then restart services without sudo, like `brew services start ...`
