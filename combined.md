# forum post development environment

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
I followed the macOS environment setup video that recommends installing MariaDB. However, another unit I'm taking requires MySQL. Since MariaDB and MySQL conflict, can I use MySQL instead of MariaDB?

**A:**
No problem. Go ahead. They're mostly interchangeable as long as you don't use both at the same time. 

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=435752)
> 
> **testphp.php not executing, just displaying code in browser**

**Q:**When I was doing the step shown in the first screenshot, I opened testphp.php, but instead of showing the expected PHP info page, it only displayed a single line of code. I've checked that PHP is already installed on my system via the terminal, so I'm not sure where the problem is coming from.

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
I tried the method you suggested, but it didn't seem to work.

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

If it's not listed, go back to the video and start from 21:00, where it shows how to install PHP. Follow along till the end.

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

If it contains index.html, it means you've likely duplicated the folder.

If that's not the case, I'll take a look on Thursday and help you figure out what's going on.

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

---

# forum post cakephp basics

---
title: CakePHP Basics
layout: default
nav_order: 3
parent: Past Forum Posts
---

Following questions are coming from Technical Issues forum of FIT3047 and FIT3048. 

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=459962)
> 
> **Outdated Cakephp/authentication plugin dependency**

**Q:**
I have been following the Authentication tutorial in Gitlab, and it recommends installing the authentication plugin using the following composer install command:

`composer require "cakephp/authentication:^2.4"`

I believe this version is for cakephp v4.x, however cakephp has moved on to v5.x. It appears that installing "cakephp/authentication:^3.0" is the correct version for cakephp v5.x, is this correct? As a follow-up question, will this create any flow-on issues when following any other teaching materials? Thanks.

**A:**
I believe the latest walkthrough already removed the version designation from its composer command: 

[README.Authentication.md](https://git.infotech.monash.edu/UGIE/ugie-demo/cake_cms-auth/-/blob/main/docs/README.Authentication.md) (i.e. it will install the latest version of plugin based on framework compatibility)

The one you have linked is from a previous version. Use the latest version of documentation instead. Always review answers from GenAI before use. 

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=466760)
> 
> **naming conventions issue**

**Q:**
I have just realized that my project did not use plurals for database before baking. is there a way to fix this without having to restart the entire project?

**A:**
All bake does is code generation - Ms, Vs and Cs of MVC. That's it. 

Depends on how much work have put into the current codebase, you can do one of the following: 

- Alter the generated codes in `/src + /templates` folders and ensure they all matching the new, fixed schema (the "proper" way)
- Back up files in `/src + /templates` folders, remove any previously generated model (table and entity), controller and view template files, then using bake to re-generate MVC scaffold, and re-apply any changes you had in your original codebase (i.e. refactor)
- Use composer to generate another CakePHP project, connect to the same database in `/config/app_local.php`, using Bake to generate code scaffold, and using generated codes as referenes to fix your original project (i.e. learning by example)

Note - you MUST fix your schema on the naming conventions - it has deeper impact than you think. 

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=467658)
> 
> **i cant login it**

**Q:**
when i try to login the CakePHP The Building Blocks, it always show the Missing Database Connection, i have change the datasource in app_local

```
> E: > xampp > htdocs > cake_cms-auth > config > app_local.php\

'Datasources' =>
    'default' => [
        'host' => "localhost',
        /*
         * CakePHP will use the default DB port based on the driver selected
         * MySQL on MAMP uses port 8889, MAMP users will want to uncomment
         * the following line and set the port accordingly
         * /
        //'port' => 'non_standard_port_number",

        "username'=>"test@example.com',
        'password' => 'password',

        "database'=> "test@example.com,
        /*
         * If not using the default 'public' schema with the PostgresQl driver
         * set it here.
         * /
        //'schema' => 'myapp',

        /*
         * You can use a DSN string to set the entire configuration
         */
        "url' = env("DATABASE_URL", null),
    ]

```

**A:**
I'm not sure if you have [setup the database and required user in MySQL (with phpMyAdmin)](https://monash.au.panopto.com/Panopto/Pages/Viewer.aspx?id=4fb51249-df2d-4df1-bc8c-b123018116cf&start=953.084999). Just that usually database user names are not an email address. 

If you're deploying the building blocks demo, consider read the [README.md](https://git.infotech.monash.edu/UGIE/ugie-demo/cake_cms-auth/-/blob/main/README.md?ref_type=heads#installation) thoroughly as it does not come with a schema dump but rather using migration to seed the database schema and demo data. 

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=490739)
> 
> **i cant login my group member website**

**Q:**
when i clone my group member's website from git, it shows the different form, i am not sure why

**A:**
Reviewed this issue in studio. Turns out to be improper implementation of views and layout. Resources are not loading with helpers, everything is hardcoded in HTML, so the website would work in domain root, but not in a path.

This is a common beginners issue - remember to take advantage of the framework template engine.

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=491734)
> 
> **Error in the bottom left corner of screen??**

**Q:**
I am getting this error in the bottom right of the screen but i am unable to see the error. I tried to double click on it and find some part of it and it says 'Missing Route',
but why is it showing this way?

**A:**
I guess it's the same issue described [here](../cakephp/authentication_plugin_breaks_debugkit).

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=495833)
> 
> **invalid csrf token**

**Q:**
So I refreshed everything, the database, codebase, did configure app_local, composer and refreshed the fit3047 user, but I still keep getting this error.

```
> localhost:8080/team000-app_fit3047/users/login

Missing or invalid CSRF cookie
Cake\Http\Exception\InvalidCsrfTokenException
```

**A:**
Try your website in incognito mode. If the error is gone, then it's likely to do with the old cookie session keys in your browser session that conflicts with CakePHP.

Clear your site data using devtools of Chrome would resolve this issue. You can ask Copilot with prompt "chrome browser devtools application clear site data" for steps.

If the above does not resolve the issue, then there will be more possibilities -
- Reboot your computer - sometimes it's just PHP's session storage is acting up
- The form is broken. Check the form and see if CSRF token is added to the form as a hidden field (usually with the name of `_csrfToken` and type of `hidden`). Also check with Debug Toolkit bar (the cake icon to the bottom right corner)
- Something deeper that broken the security middleware of CakePHP. This will need to check everything case by case and see what's going on. Though you can always start with deleting `/vendor folder ` in cakeroot and reinstall packages with `composer install` to rule out potentially corrupted package files.

Also try turning your debug mode off, so that you dont see any deprecated warning, and try logging in again. It worked in some cases, as sometimes for some reason post request csrf gets broken due to error messages being printed on screen too early. 

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=528664)
> 
> **cPanel cannot find a file**

**Q:**
this error is showing up but only for one of our pages, every other page works. I have checked cPanel and `/templates/Pages/dashboard.php` exists:

```
> review.u00s0000.iedev.org/pages/dashboard

Not Found
Error: The requested address /pages/dashboard' was not found on this server.
```

I have also checked the case-sensitivity and the .htaccess files and everything seems to be fine so I don't know why the error is occuring. Below is the error.log output:

```
Request URL: /pages/dashboard
Referer URL: https://review.u00s0000.iedev.org/users/login?redirect=%2Fpages%2Fdashboard
Client IP: 175.33.38.215
2025-10-08 05:53:09 error: [Cake\Http\Exception\NotFoundException] Not Found in /srv/cpusers/u00s0000/review.u00s0000.iedev.org/src/Controller/PagesController.php on line 100
Request URL: /pages/dashboard
Referer URL: https://review.u00s0000.iedev.org/users/login?redirect=%2Fpages%2Fdashboard
```

**A:**
I checked the issue and found it's a case-sensitivity mismatch.

The reason it was not working is because, its trying to find the `admin_layout.php` file in `admin` folder, even though it says dashboard.

When going through the folder, you had `admin` folder named as `Admin`. So case-sensitive problem.

There are two ways to fix this, either rename the folder or change the code to have Admin.

----

{: .note-title }
> [Original forum post]()
> 
> **Load identifier error question?**

**Q:**
After I deploy the system to cPanel, I got this error.

```
Deprecated (16384): Since 3.3.0: loadIdentifier() usage is deprecated. Directly pass "identifier" config to the Authenticator. /srv/cpusers/u00s0000/project/src/Application.php, line: 131 You can disable all deprecation warnings by setting 'Error.errorLevel' to 'E_ALL & ~E_USER_DEPRECATED'. Adding 'src/Application.php' to 'Error.ignoredDeprecationPaths" in your your"config/app.php' config will mute deprecations from that file only. [in/srv/cpusers/u00s0000/project/vendor/cakephp/cakephp/src/Core/functions.php, line 373]
Deprecated (16384): Since 3.3.0: loadIdentifier usage is deprecated. Directly pass identifier" config to the Authenticator.
/srv/cpusers/u00s0000/project/vendor/cakephp/authentication/src/AuthenticationService.php, line: 144 You can disable all deprecation warnings by setting 'Error.errorLevel' to 'E_ALL & ~E_USER_DEPRECATED'.
Adding 'vendor/cakephp/authentication/src/AuthenticationService.php' to 'Error.ignoredDeprecationPaths" in your'config/app.php' config will mute deprecations from that file only. [in /srv/cpusers/u00s0000/project/vendor/cakephp/cakephp/src/Core/functions.php,line 373]
Warning (512): Unable to emit headers. Headers sent in file=/srv/cpusers/u00s0000/project/vendor/cakephp/cakephp/src/Error/Renderer/HtmlErrorRenderer.php line=37 [in /srv/epusers/u00s0000/project/vendor/cakephp/cakephp/src/Http/ResponseEmitter.php, line 65]
Warning(2): Cannot modify header information -headers already sent by (output started at/srv/cusers/u00s0000/project/vendor/cakephp/cakephp/src/Error/Renderer/HtmlErrorRenderer.php:37) [in /srv/cpusers/u00s0000/project/vendor/cakephp/cakephp/src/Http/ResponseEmitter.php, line 159]
Warning(2): Cannot modify header information-headers already sent by (output started at/srv/cpusers/u00s0000/project/vendor/cakephp/cakephp/src/Error/Renderer/HtmlErrorRenderer.php:37) [in /srv/cpusers/u00s0000/project/vendor/cakephp/cakephp/src/Http/ResponseEmitter.php,line 192]
Warning(2): Cannot modify header information-headers already sent by (output started at/srv/cpusers/u00s0000/project/vendor/cakephp/cakephp/src/Error/Renderer/HtmlErrorRenderer.php:37) [in /srv/cpusers/u00s0000/project/vendor/cakephp/cakephp/src/Http/ResponseEmitter.php, line 229]
```

**A:**
It seems it's because that function call is deprecated and generating a message that breaks certain part of CakePHP. 
I've updated codes in the IE Example repo to address this. 

Originally from: [https://book.cakephp.org/authentication/3/en/index.html#getting-started](https://book.cakephp.org/authentication/3/en/index.html#getting-started)

Updated Example repo: [https://git.infotech.monash.edu/UGIE/ugie-demo/cake_cms-auth/-/blob/main/src/Application.php#L114-152](https://git.infotech.monash.edu/UGIE/ugie-demo/cake_cms-auth/-/blob/main/src/Application.php#L114-152)

Also note you'll likely need to run "composer update" to ensure CakePHP base package is the latest version, otherwise you may not be able to login in (always show login fail)

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=534549)
> 
> **cPanel routing problems**

**Q:**
Hi, my team has noticed that some routing features seem to be broken on cPanel, despite working on our localhost. I have made sure all the git is updated, and I've even recloned on cPanel in case. Another suspicious thing is that the same feature broke on all 3 of our dev/review/prod, despite review and prod having older stable code that worked.

In our specific circumstance, from our `templates/Pages/home.php` page, we add an enquiry to the Enquiries table by making the form use the 'Enquiries' controller and the 'Add' action. The error that showed was "EnquiriesController could not be found", despite it being in the same location as how the error suggests to fix - we have never changed the name of this controller or any references to it.

**A:**
From `project/dev/logs/error.log`: (after I tried the enquiries form myself - originally it seems you have wiped all related logs previously)

```
2025-10-13 07:43:41 error: [Cake\Http\Exception\MissingControllerException] Controller class `Enquiries` could not be found. in /srv/cpusers/u25s1012/cohenveshut/dev/vendor/cakephp/cakephp/src/Controller/ControllerFactory.php on line 335
Exception Attributes: array (
  'controller' => 'Enquiries',
  'plugin' => NULL,
  'prefix' => 'admin',
  '_ext' => NULL,
)
```

It says - prefix is "admin". However given your prefix routing requires "Admin", I guess that may be the issue. On Linux host, the filesystem is sensitive and CakePHP may be affected by such behaviour change. 

Also I have noticed that the "add" action in Admin/Enquiries is behind authentication. If that's the case, users who does not login won't be able to submit the form anyway. Given you're using prefix routing, and the isolation you're looking for, you really should have a dedicated "EnquiriesController" outside of the Admin prefix routing to process related POST/GET queries. 

**Q:**
The problems are fixed. If anyone else has similar problems:
- I also found out that the case of the prefix and controller matters on a Linux system, which is why it worked on my windows system but not on cPanel.
- Also, I forgot I had an extra authentication check in AppController which was blocking the enquiries/add action, even though I had set `addUnauthenticatedActions(['add'])` present in EnquiriesController.

**A:**
Yes. case sensitivity is a very common oversight. And it also impact MySQL database as well since databases and tables, depends on the engine being used, may be stored on the filesystem. So it's very important to follow all conventions mentioned in the CakePHP Cookbook.

---

# moodle resources Important Basic Concepts

---
title: Watch me FIRST!
layout: default
nav_order: 1
parent: CakePHP
---

PHP is a widely available and adopted (memed "the best programming language in the world" - don't take it seriously :) server-side programming language that is HTML-embedded. If you're new to web development or PHP development, here are some resources on how to start develop in PHP. 

Please remember that FIT3047/FIT3048 are capstone units - we use those technology as a tool to implement your idea for your product owners/clients therefore it's time to use your experience in self-learning and do the best you can to quickly get to know web development. 

1. [Basic Web Development Concepts](https://www.youtube.com/watch?t=0&v=FXqTHsPaY0A) - What is web development: in 10 minutes! 
2. [PHP in 100 Seconds](https://www.youtube.com/watch?t=0&v=a7_WFUlFS94) - Get to know what role PHP plays, very quickly
3. [Learn PHP in Under 15 Minutes](https://www.youtube.com/watch?t=0&v=vt_GSfKqQ7I) - This covers some basic concepts of PHP, and with your knowledge in other languages, you should be able to start PHP development rather quickly
4. Now, start reading - best tutorials are from the official channels or good, well known consolidated learning sites, such as: 
    1. [PHP: a simple tutorial](https://www.php.net/manual/en/tutorial.php) from PHP official documentation
    2. [PHP tutorial](https://www.w3schools.com/php/) from W3School

{: .warning }
Please note that these are reading materials for you to familiar with the CONCEPT of the language and setup. For the standard development environment please move on to next article in this series as there are detailed video walkthrough.

---

# moodle resources Mixed CakePHP Start Guide

Got it — here's the same glossary-style summary, rewritten consistently as "The user …".

---

# **CakePHP Project Setup, Database Integration, Git, and Deployment (Glossary Points)**

## **Context**

* The user has already set up a local development environment on Windows or Mac (Apache, PHP, Composer, database tools).  
* This workflow focuses on:  
  * Creating a new CakePHP app  
  * Creating and connecting a database  
  * Generating code with Cake Bake  
  * Uploading to Git  
  * Deploying to a cPanel server under a subdomain

---

# **Part 1 — Windows-only setup adjustments (Mac users skip)**

## **Cleaning up the XAMPP web root**

* The user navigates to the XAMPP web root:  
  * `C:\xampp\htdocs`  
* The user removes or renames the default XAMPP landing `index` file (or deletes most default dashboard files).  
* The user keeps only what is still needed (e.g., phpinfo if they want it).  
* The user benefits from this because:  
  * Visiting `http://localhost` will show a directory listing of projects in `htdocs`, making navigation easier.

## **Enabling required PHP extensions (CakePHP prerequisites)**

* The user finds and opens the PHP configuration file:  
  * `C:\xampp\php\php.ini`  
* The user enables extensions by removing the leading semicolon `;` in the extensions section.  
* The user enables at least:  
  * `intl`  
  * `sqlite3`  
* The user may also enable database-related extensions as needed (depending on the course setup):  
  * `mysqli`  
  * `pdo_mysql`  
* The user saves `php.ini`.

## **Restarting Apache to apply php.ini changes**

* The user opens XAMPP Control Panel as Administrator.  
* The user stops Apache.  
* The user starts Apache again.

## **Verifying extensions using phpinfo**

* The user creates a simple phpinfo file (or recreates it if deleted) containing:  
  * `phpinfo()`  
* The user loads that file in the browser.  
* The user searches the phpinfo output for:  
  * `intl`  
  * `sqlite3`  
* The user confirms the extensions are enabled by seeing dedicated sections for them.

---

# **Part 2 — CakePHP documentation reference**

## **CakePHP Cookbook**

* The user uses CakePHP's official documentation:  
  * `book.cakephp.org`  
* The user uses "CakePHP at a Glance" to understand:  
  * CakePHP conventions  
  * MVC structure  
  * Request cycle  
* The user uses the Tutorials/Guides section for:  
  * quick-start walkthroughs  
  * reference examples

## **CakePHP conventions emphasis**

* The user treats naming conventions as critical, because:  
  * CakePHP and Bake rely heavily on correct naming to generate code cleanly.

---

# **Part 3 — Creating a new CakePHP project with Composer**

## **Choosing the project location (web root)**

* The user works inside the local web root so Apache can serve the project.  
  * Windows: `C:\xampp\htdocs`  
  * Mac: Homebrew Apache DocumentRoot (commonly `/opt/homebrew/var/www`)

## **Running Composer create-project**

* The user runs a Composer command to create a CakePHP skeleton project.  
* The user specifies:  
  * CakePHP skeleton source (`cakephp/app`)  
  * a project folder name (example used: property management system)  
* The user waits while Composer:  
  * downloads dependencies  
  * builds the project folder structure

## **Installation success indicators**

* The user confirms the CakePHP install by checking for:  
  * `vendor/` (dependencies)  
  * `tmp/` (runtime cache/temp)  
  * `logs/`  
  * `config/app_local.php`  
* The user notes that `app_local.php` is:  
  * generated locally  
  * excluded from Git because it contains credentials / sensitive config

## **Checking the CakePHP welcome page**

* The user visits the project URL under localhost.  
* The user expects:  
  * CakePHP welcome page loads  
  * styling/themes load properly (broken styling often suggests missing vendor assets or install issues)

---

# **Part 4 — Creating a database and connecting it to CakePHP**

## **Creating database \+ user in phpMyAdmin**

* The user opens phpMyAdmin.  
* The user creates a database user first (instead of only creating a database), so permissions are set correctly.  
* The user creates:  
  * a username tied to the project (example: property management)  
  * host restricted to `localhost`  
  * a chosen password  
* The user enables the options:  
  * create a database with the same name  
  * grant all privileges  
* The user verifies in privileges/user accounts that:  
  * the new user exists  
  * the user is linked to the database

## **Editing CakePHP database configuration**

* The user opens:  
  * `config/app_local.php`  
* The user edits the default datasource settings:  
  * `username`  
  * `password`  
  * `database`  
  * host remains localhost for local dev  
* The user saves the file.

## **Confirming CakePHP can connect to the database**

* The user refreshes the CakePHP homepage.  
* The user checks the database status area to confirm:  
  * CakePHP can connect successfully.

---

# **Part 5 — Schema design rules relevant to Bake**

## **Primary key convention**

* The user ensures every table has a primary key named:  
  * `id`  
* The user uses a UUID-style key in the example:  
  * `CHAR(36)` (not varchar), to support UUIDs  
* The user notes that integer keys can work, but UUIDs are more globally unique.

## **Foreign key naming convention**

* The user uses CakePHP naming conventions for foreign keys (critical for Bake):  
  * `user_id`, `property_id`, etc.  
* The user is directed to the CakePHP conventions section in the Cookbook (database conventions) for correct naming.

---

# **Part 6 — Generating MVC CRUD using Cake Bake**

## **Accessing Cake console commands**

* The user runs CakePHP console commands from the project root.  
* The user uses the Cake console entrypoint:  
  * `bin/cake` (Mac/Linux)  
  * `bin\cake` (Windows path style)

## **Verifying the command works**

* The user runs `bin/cake` to view available commands.

## **Baking generated code**

* The user runs Bake to generate full CRUD for each table:  
  * `bake all Users`  
  * `bake all Properties`  
* The user notes Bake generates:  
  * Model layer (Table \+ Entity)  
  * Controller  
  * Templates (index/view/add/edit)  
  * additional supporting files (fixtures may appear but aren't required here)

## **Testing generated CRUD in browser**

* The user visits:  
  * `/users` to see the user CRUD interface  
  * `/properties` to see property CRUD interface  
* The user adds a record to confirm:  
  * create works  
  * records display in index  
  * view/edit/delete work  
* The user observes relationship behavior:  
  * foreign-key-linked dropdowns appear automatically  
  * related data appears across pages when relationships are correctly defined

---

# **Part 7 — Uploading the project to Git**

## **Cloning an empty repository**

* The user clones a provided Git repository into the web root directory.

## **Ensuring CakePHP project is repo root**

* The user avoids nesting the CakePHP folder incorrectly.  
* The user moves the CakePHP project files into the cloned repo folder so that:  
  * the repository root becomes the CakePHP project root  
* The user ensures hidden files (dotfiles) are included:  
  * `.gitignore`, `.editorconfig`, etc.

## **Committing and pushing**

* The user runs:  
  * `git add .`  
  * `git commit -m "Initial commit"`  
  * `git push`  
* If Git identity isn't set, the user sets per-repo config:  
  * `git config user.name ...`  
  * `git config user.email ...`  
* The user refreshes the remote repository and confirms files appear.  
* The user optionally replaces the default CakePHP README with project-specific details for clarity.

---

# **Part 8 — Deploying to cPanel (subdomain hosting)**

## **Logging into cPanel**

* The user logs into the Monash cPanel portal.  
* The user identifies key tools inside cPanel:  
  * File Manager  
  * MySQL Databases (database \+ users)  
  * Domains (subdomains)  
  * Terminal (for git/composer)

## **Creating database \+ user on cPanel**

* The user creates:  
  * a database  
  * a database user  
  * a password (stored temporarily for later configuration)  
* The user manually assigns the user to the database and grants privileges (this is not automatic like local phpMyAdmin setups).

## **Moving local database schema/data to server**

* The user exports the local database from phpMyAdmin.  
* The user imports that SQL dump into the server database using phpMyAdmin on cPanel.  
* The user confirms the tables and data exist after import.

## **Cloning project on the server (order matters)**

* The user clones the Git repo in the cPanel home directory (not public\_html).  
* The user does this before creating the subdomain to avoid cPanel auto-generating files/folders that interfere with cloning.

## **Restoring missing dependencies and folders on server**

* The user notices expected folders are missing on the server clone because they are intentionally ignored:  
  * `vendor/`  
  * `tmp/`  
  * `logs/`  
  * `config/app_local.php`  
* The user runs:  
  * `composer install`  
  * if required due to PHP version constraints: `composer update`, then `composer install` again  
* The user confirms the missing runtime folders now exist.

## **Creating server-side app\_local.php**

* The user creates or copies `config/app_local.php` on the server.  
* The user updates it with:  
  * cPanel database name  
  * cPanel database username  
  * cPanel database password

## **Creating a subdomain and pointing it at the project**

* The user creates a subdomain under the existing account domain:  
  * `something.account.monash...` (subdomain pattern)  
* The user sets the document root to the cloned project folder (the correct one already containing code).  
* The user enables "show hidden files" in File Manager to avoid missing dotfiles.

## **Testing the live site**

* The user visits the new subdomain.  
* The user confirms the CakePHP app loads publicly.  
* The user notices debug mode behavior:  
  * debug home page is a local/dev feature  
  * on public hosting, debug is a security risk and may be disabled automatically or should be set to false

## **Debug setting recommendation**

* The user sets:  
  * `debug => false`  
* The user treats debug as:  
  * true only for local dev  
  * false for review/production/demo environments

---

# **End result**

* The user has completed a full CakePHP workflow:  
  * local setup (plus Windows-only extension tweaks)  
  * CakePHP project creation via Composer  
  * database \+ user creation  
  * connecting CakePHP to database via `app_local.php`  
  * schema creation using conventions  
  * CRUD generation via Bake  
  * Git upload for collaboration  
  * deployment to cPanel with database import  
  * composer install on server to restore ignored dependencies  
  * subdomain configuration pointing to the deployed project

---

---

# moodle resources Mac Setup Guide

## **MacOS Local Development Setup (Industry Experience Resource Repository) — Glossary Points**

### **Scope / which video**

* You're following the **MacOS setup** walkthrough.  
* If you're on **Windows**, you should switch to the Windows-specific video instead.

---

## **Quick concept recap: how websites work**

* **Native apps** (PC/tablet/phone) run code directly on your device and usually need to be compiled \+ installed.  
* **Web apps** are different:  
  * You interact with a **browser**, not the raw website code.  
  * The browser **renders** the site and handles user interaction.  
  * Most application logic runs on a **server** (a computer running 24/7 somewhere).  
* A typical server stack includes:  
  * **Web server (Apache / httpd):** "gatekeeper" that receives browser requests and serves responses/files.  
  * **PHP interpreter:** runs your backend PHP code (PHP is interpreted like Python).  
  * **Database (MariaDB/MySQL):** stores and serves your data.  
  * **phpMyAdmin:** GUI to manage the database.

---

## **Why you set up a local environment**

* You run the "server stack" on **your own Mac**, not in the cloud.  
* Benefits:  
  * Faster feedback loop (no network/cloud latency)  
  * Costs nothing (no cloud server fees)  
  * Uses your Mac's resources (CPU/RAM/storage)  
  * Lets you keep the stack running as background services so it "just works" when you open the browser

---

# **What you install (high level sequence)**

1. **Chrome**  
2. **Homebrew** (and Xcode command-line tools)  
3. **Apache (httpd)**  
4. **PHP**  
5. **Composer**  
6. **MariaDB \+ phpMyAdmin**  
7. Configure Apache for **PHP \+ phpMyAdmin**  
8. Test everything end-to-end

---

## **1\) Browser: Chrome**

* You install **Chrome** (even though Safari exists) because:  
  * Chrome has a larger user base → better compatibility confidence  
  * Chrome DevTools are generally more convenient for debugging than Safari's tooling  
* Install method (Mac standard):  
  * Download `.dmg`  
  * Drag Chrome into **Applications**  
  * Optionally pin to Dock  
  * You can stop using Safari for this workflow afterward

---

## **2\) Package manager: Homebrew \+ Xcode CLI tools**

* You install **Homebrew** (developer "app store" / package manager).  
* Installing Homebrew also installs **Xcode command-line tools**, which includes **git**.  
* Apple Silicon vs Intel note:  
  * Apple Silicon typically uses `/opt/homebrew/...`  
  * Intel Macs often use `/usr/local/...`  
  * Follow **your screen/paths**, not someone else's, when editing config files.

Useful commands (from your reference):

brew update  
brew upgrade \<package\_name\>  
brew doctor

* `brew doctor` is your quick "is Homebrew healthy?" check.

---

## **3\) Git check (comes with Xcode CLI tools)**

* After Homebrew/Xcode CLI tools, you verify Git works:

git \--version  
\# or just:  
git

* If Git isn't found, you likely need to reinstall / repair Xcode command line tools or search the specific error you got.

---

## **4\) Install Apache via Homebrew (package name: httpd)**

* In Homebrew, Apache is installed as **httpd** (Apache HTTP Server).  
* You check package info first:

brew info httpd

* Then install:

brew install httpd

Key concepts from the install notes:

* **DocumentRoot** (your web root / where served files live)  
  * Common Homebrew path on Apple Silicon:  
    * `/opt/homebrew/var/www`  
* **Port**  
  * Homebrew's Apache commonly uses **8080** to avoid conflicts with macOS services.

Start and verify the service:

brew services start httpd  
brew services list

Confirm what ports it's listening on:

sudo lsof \-i \-P \-n | grep LISTEN | grep httpd

Test in browser:

* You load:  
  * `http://localhost:8080`  
* If you see a basic "It works" page, Apache is serving content.

---

## **5\) Understand DocumentRoot (web root)**

* The "DocumentRoot" is the folder your web server serves files from.  
* If DocumentRoot is `/opt/homebrew/var/www`, then:  
  * `http://localhost:8080/` maps to files in that folder.  
* You can add the `www` folder to Finder sidebar for quick access later.

---

## **6\) Install PHP via Homebrew**

* You install PHP so Apache can serve dynamic `.php` files (not just HTML).  
* You can see available versions:

brew search php

* Install PHP (latest by default):

brew install php

* Version alignment note:  
  * Your team should ideally use the same **major/minor PHP version** to avoid weird compatibility issues.

---

## **7\) Configure Apache to run PHP**

* After installing PHP, you need to update Apache config so it:  
  1. Loads the PHP module  
  2. Treats `.php` files as PHP  
  3. Uses `index.php` as a default homepage (like `index.html`)  
  4. (Recommended) enables `mod_rewrite` for common frameworks

### **Find Apache config location**

Use:

brew info httpd

Common path on Apple Silicon:

* `/opt/homebrew/etc/httpd/httpd.conf`

### **Edit the config (example uses nano)**

nano /opt/homebrew/etc/httpd/httpd.conf

### **Recommended edits (from your reference)**

Uncomment `mod_rewrite` (remove the `#`):

LoadModule rewrite\_module lib/httpd/modules/mod\_rewrite.so

Load PHP module (path may vary by your PHP install):

LoadModule php\_module /opt/homebrew/opt/php/lib/httpd/modules/libphp.so

Add PHP file handler:

\<FilesMatch \\.php$\>  
    SetHandler application/x-httpd-php  
\</FilesMatch\>

Ensure DirectoryIndex prefers `index.php`:

DirectoryIndex index.php index.html

Restart Apache after config changes:

brew services restart httpd  
\# or if needed:  
brew services stop httpd  
brew services start httpd

### **Test PHP is working**

* In your DocumentRoot, you create something like `test.php`:

\<?php phpinfo();

* Then visit:  
  * `http://localhost:8080/test.php`  
* If you see the rendered PHP info page (not raw code), PHP is correctly integrated.

---

## **8\) Install Composer (PHP dependency manager)**

* Instead of downloading from the web, you install with Homebrew:

brew install composer

Test:

composer  
\# or:  
composer \--version

---

## **9\) Install MariaDB (MySQL-compatible) \+ phpMyAdmin**

* MariaDB is an open-source fork / drop-in replacement for MySQL.  
* You install both:

brew install mariadb phpmyadmin

Start MariaDB:

brew services start mariadb  
brew services list

Database shell access:

mysql

---

## **10\) Enable phpMyAdmin in Apache (Alias \+ Directory block)**

* phpMyAdmin isn't automatically visible at `/phpmyadmin` until you add an Apache config section.

In `httpd.conf`, add (as shown in the screenshot and typical Homebrew caveats):

Alias /phpmyadmin /opt/homebrew/share/phpmyadmin

\<Directory /opt/homebrew/share/phpmyadmin/\>  
    Options Indexes FollowSymLinks MultiViews  
    AllowOverride All  
    \<IfModule mod\_authz\_core.c\>  
        Require all granted  
    \</IfModule\>  
    \<IfModule \!mod\_authz\_core.c\>  
        Order allow,deny  
        Allow from all  
    \</IfModule\>  
\</Directory\>

Then restart Apache:

brew services restart httpd

Test:

* `http://localhost:8080/phpmyadmin`

Important note:

* phpMyAdmin is served via an **Alias**, so it won't necessarily appear as a physical folder inside your DocumentRoot.

---

## **11\) phpMyAdmin login restriction (root login change)**

* Modern MySQL/MariaDB setups often restrict **root** login via web UI for security.  
* You may find:  
  * root works only in terminal, or  
  * login differs based on macOS auth methods.  
* The video's approach:  
  * Create a separate "super user" that you use in phpMyAdmin.

---

## **12\) Create a "super user" in MariaDB/MySQL (for phpMyAdmin)**

You:

1. Log into mysql in Terminal:

mysql

2. Create a user and password (example pattern from the video)

CREATE USER 'superuser'@'localhost' IDENTIFIED BY 'superuserpassword';

3. Grant privileges (full privileges for local development):

GRANT ALL PRIVILEGES ON \*.\* TO 'superuser'@'localhost' WITH GRANT OPTION;

4. Apply changes:

FLUSH PRIVILEGES;

5. Exit:

quit;

Then you log into phpMyAdmin using:

* Username: `superuser`  
* Password: `superuserpassword`

(Reference link you provided for this idea: TablePlus article on creating a MySQL superuser.)

---

## **13\) Optional "bonus" convenience: auto-login in phpMyAdmin**

* You can configure phpMyAdmin to store credentials so you don't type them every time.  
* You edit phpMyAdmin's config file (Homebrew caveats usually show the exact path).  
* The video's key changes:  
  * Change auth type from `cookie` to `config`  
  * Add stored user/password lines for the first server entry  
* Security note:  
  * This is convenient for local dev but it stores credentials in a config file.

---

# **Final checklist: verify everything works**

You confirm each component:

### **Homebrew**

brew doctor

Expect: "Your system is ready to brew."

### **Git**

git \--version

### **Apache**

* Visit:  
  * `http://localhost:8080`

### **PHP in browser**

* Visit:  
  * `http://localhost:8080/test.php` (phpinfo page)

### **PHP in terminal**

php \-v  
which php

You want the terminal PHP and Apache PHP to match (both Homebrew-based).

### **Composer**

composer

### **MariaDB**

mysql

Expect: MariaDB/MySQL monitor opens without errors.

### **phpMyAdmin**

* Visit:  
  * `http://localhost:8080/phpmyadmin`  
* Confirm you can:  
  * log in with your created super user  
  * see "User accounts" / admin capabilities (basic privilege check)

---

## **Outcome**

* You've installed and configured:  
  * Chrome  
  * Homebrew \+ Xcode CLI tools (includes git)  
  * Apache (httpd) running as a background service on port 8080  
  * PHP integrated with Apache  
  * Composer installed via Homebrew  
  * MariaDB running as a background service  
  * phpMyAdmin enabled via Apache Alias  
  * A dedicated super user for database administration in phpMyAdmin

If you want, I can condense this into a one-page "commands-only" checklist while keeping the important paths (DocumentRoot, config paths, ports) visible.

---

# moodle resources Windows Setup Guide

Here is the revised glossary-style summary written in a more direct instructional perspective (e.g., "You have installed…" rather than third person).

---

# **Overview: Setting Up a Local Web Development Environment (Windows)**

## **Platform Note**

* This setup is for Windows only (demonstrated on Windows 11).  
* If you are using macOS, you must follow the Mac-specific setup guide instead.

---

# **How Web Development Works (Concept Recap)**

## **Client–Server Model**

Unlike desktop or mobile applications (which run entirely on your device), web systems are split into two parts:

### **1\. Client Side (Your Browser)**

* You use a web browser.  
* The browser sends requests to a server.  
* The browser displays the server's response.

### **2\. Server Side**

* Runs on another computer (normally in the cloud).  
* Processes logic and backend code.  
* Communicates with a database.  
* Sends results back to the browser.

---

## **Server Components You Are Installing**

Your server environment includes:

### **Web Server (Apache)**

* Acts as a gatekeeper.  
* Receives and handles incoming HTTP requests.  
* Serves static files.  
* Forwards dynamic requests to PHP.

### **PHP Interpreter**

* PHP is an interpreted language (like Python).  
* The interpreter executes backend PHP code.

### **Database (MySQL / MariaDB)**

* Stores application data.  
* MariaDB is an open-source fork of MySQL.

### **phpMyAdmin**

* A graphical interface for managing your database.

---

# **What a Local Development Environment Means**

Normally:

* Browser \= your computer  
* Server \= cloud computer

In local development:

* Both the browser and server run on your own machine.

## **Why You Do This**

* Faster (no network delay).  
* More flexible (you control installed software).  
* No hosting costs.  
* Full control over software versions.  
* Better use of your computer's CPU and memory.

---

# **Software You Have Installed**

You have installed:

1. Google Chrome  
2. Visual C++ Redistributables  
3. Git for Windows  
4. XAMPP (Apache \+ MariaDB \+ PHP)  
5. Composer (PHP package manager)

---

# **1\. Google Chrome**

You installed Chrome because:

* Although Edge is Chromium-based, it behaves slightly differently.  
* Chrome has the largest market share.  
* If your site works in Chrome, it will most likely work elsewhere.  
* Development tools are most consistent in Chrome.

---

# **2\. Visual C++ Redistributables**

You installed the required Visual C++ runtime libraries because:

* XAMPP and related tools are compiled using Visual C++.  
* Without these libraries, executables may fail.

Installation steps included:

* Downloading the all-in-one package.  
* Extracting it.  
* Running the installer as Administrator.

---

# **3\. Git for Windows**

You installed Git to manage source code.

Key points:

* Installed to the default location.  
* Added to system PATH automatically.  
* Git Credential Manager optionally stores passwords securely.

If you change your Git password later:

* You may need to manually update stored credentials.

## **You Verified Git by Running:**

git

If installed correctly:

* The command runs successfully.  
* No "command not found" error appears.

---

# **4\. XAMPP (Your Local Web Server Stack)**

You installed XAMPP, which includes:

* Apache (web server)  
* MariaDB (database)  
* PHP  
* phpMyAdmin

---

## **Version Choice**

* You selected PHP 8.1 (not necessarily the latest version).  
* Slightly older versions may be more stable.  
* Your team should use the same major PHP version.

---

## **Installation Location**

You installed XAMPP to:

C:\\xampp

Important:

* Do not change the default location.  
* XAMPP contains hardcoded paths.  
* Installing elsewhere can cause errors.

---

## **Firewall Warning**

When prompted by Windows Firewall:

* You clicked Cancel.  
* You did not allow public or private network access.

Reason:

* This is a local development server.  
* You do not want others on your network accessing it.

---

# **Configuring XAMPP**

## **You Ran the Control Panel as Administrator**

If not run as Administrator:

* You may encounter "Access Denied" errors.

---

## **You Installed Apache and MySQL as Windows Services**

You:

* Clicked the red X icons.  
* Installed Apache and MySQL as services.  
* Turned them into green ticks.

This means:

* They start automatically when Windows starts.  
* You do not need to manually start them each time.

---

# **Default Ports in Use**

* Apache runs on Port 80\.  
* MySQL runs on Port 3306\.

---

# **You Tested XAMPP**

In your browser, you visited:

http://localhost

You confirmed:

* XAMPP dashboard loads.

You also tested:

### **PHP Info**

http://localhost/dashboard/phpinfo.php

* Verified PHP version (e.g., 8.1.x).

### **phpMyAdmin**

http://localhost/phpmyadmin

* Verified database interface loads.  
* Confirmed default databases are visible.

---

# **5\. Composer (PHP Package Manager)**

You installed Composer from:

* getcomposer.org

During installation:

* You selected "Install for All Users."  
* You selected the correct PHP interpreter:

C:\\xampp\\php\\php.exe

Reason:

* Composer must use the same PHP version as your web server.  
* Prevents compatibility issues.

You also:

* Added PHP to PATH.

---

# **You Restarted Your Computer**

This ensured:

* System PATH variables updated.  
* Windows services initialized properly.

---

# **Final Verification**

After restarting:

## **Browser Test**

You visited:

http://localhost

Confirmed:

* Apache started automatically.  
* Database started automatically.

---

## **Command Line Tests**

You opened Command Prompt and ran:

### **Git**

git

### **Composer**

composer

### **PHP Version**

php \-v

You confirmed:

* PHP version displayed correctly.  
* No "command not found" errors.

---

# **What You Now Have**

You now have a complete local PHP development stack:

| Component | Purpose |
| ----- | ----- |
| Chrome | Browser testing |
| Git | Version control |
| Apache | Web server |
| PHP | Backend language |
| MariaDB | Database |
| phpMyAdmin | Database GUI |
| Composer | PHP dependency management |

---

# **Result**

You have successfully:

* Set up a fully functional local web server.  
* Installed PHP and database support.  
* Configured version control.  
* Installed a package manager for PHP.

You are now ready to install and develop a PHP-based project (e.g., CakePHP) and begin backend development.
