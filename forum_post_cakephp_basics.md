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
I checked the issue and found it’s a case-sensitivity mismatch.

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
