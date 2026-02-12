---
title: Code Generation w/ Bake
layout: default
nav_order: 4
parent: Past Forum Posts
---

Following questions are coming from Technical Issues forum of FIT3047 and FIT3048. 

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=527262)
> 
> **About the Database update**

**Q:**
If I change and update my Database on the "phpMyAdmin", and should I use bake to generate the skeleton codes again like the video "[https://monash.au.panopto.com/Panopto/Pages/Viewer.aspx?id=4fb51249-df2d-4df1-bc8c-b123018116cf](https://monash.au.panopto.com/Panopto/Pages/Viewer.aspx?id=4fb51249-df2d-4df1-bc8c-b123018116cf)" talking?

**A:**
I think you mean you have made changes to the schema (i.e. adding/removing columns in a table, creating new tables, etc.) and what to do next? 

In general, there will be two things to consider: 

1. Modify your CakePHP codes to reflect the changes. 

This includes update the Model (table and entity) files so they're matching the changes you've made to the schema. Also if there are new or removed columns, or change of data types, you may also need to change the corresponding controller files and/or view template files so these changes are adapted. 

Also don't forget to clear the schema cache in CakePHP deployments so the ORM knows the new schema for query generation. This can be achieved by deleting everything in /tmp folder. 

Of course, if a new table is added to the database, you can use Bake to generate skeleton codes (MVC) to kickstart that specific part of development. However the code generator will not update files already generated (unless you force it to overwrite existing files) so you may still need to manually make those changes if new relationships are added in your schema. 

2. Sharing the changes with your team

Now that the codes are compatible with your changes to the schema, you'll need to ensure others in the team also use this new version of schema. You can dump (export) the entire schema, with or without data, to a place that you can share. In your PGP, or add the file to your git repo. 

**Q:**
But if I just add or remove the columns instead of creating or deleting the table, does that mean I didn't need to generate the code with Bake command?

**A:**
Bake command will always re-generate all related codes and potentially overwriting your existing changes to skeleton. If you're just making minor changes, update your existing code files is always better.

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=436333)
> 
> **Running Bake command on Mac**

**Q:**
So I missed out on how to bake with CakePHP as I was setting up the environment. as I'm on MacBook, the example on ie repo for windows is really different from Mac.

Just wondering is there any other ways to bake.


**A:**
Bake is just generating code with CakePHP's command line interface and the bake plugin of CakePHP. 

Read related cookbook articles: 

- [https://book.cakephp.org/5/en/console-commands.html](https://book.cakephp.org/5/en/console-commands.html)
- [https://book.cakephp.org/bake/3/en/usage.html](https://book.cakephp.org/bake/3/en/usage.html)

Instructions were demonstrated on Windows because the usage is OS-agnostic. It's practically the same. 

In reality you're not missing out anything. At the current stage feel free to read as much about CakePHP as you can, ask GenAI, use the sample database schema (or come up with your own) to play around, get familiar with the framework. That's all. And of course, use the forum if you have any questions in particular. 

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=436687)
> 
> **CakePHP Bake**

**Q:**
I just had a quick question, in the tutorial I missed the last part where we were shown the CakePHP bake as I was resolving other issues. When I use the bake command in my cmd I get this. I'm not sure what it means or what has to be done?

```
C: \xampp\htdocs\cms>bin\cake bake all
Bake All
---------------------------------------
2025-07-31 08:50:20 error: [RuntimeException] Your database does not have any tables. in C:\xampp\htdocs\cms\vendor\cake php\bake\src\Utility\TableScanner.php on line 68
Stack Trace:
- ROOT\vendor\cakephp\bake\src\Utility\TableScanner.php:82
- ROOT \vendor\cakephp\bake\src\Command\AllCommand.php: 87
- CORE\src\Console\BaseCommand.php:201
- CORE\src\Console\CommandRunner.php:338
- CORE\src\Console\CommandRunner•php: 168
- ROOT\bin\cake.php:10
- [main]:

[RuntimeException] Your database does not have any tables. in C: \xampp\htdocs\cms\vendor\cakephp\bake\src\Utility\TableScanner.php on line 68

Stack Trace:

Bake\Utility\TableScanner→>listAll() - ROOT\vendor\cakephp\bake\src\Utility\TableScanner.php, line 82
Bake\Utility\TableScanner->listUnskipped() - ROOT\vendor\cakephp\bake\src\Command\AllCommand.php, line 87
Cake\Console\BaseCommand->run() - CORE\src\Console\CommandRunner.php, |
Bake\Command\AllCommand->executed() - CORE sre Console BaseCommend,php Line 3381
Cake\Console\CommandRunner->runCommand() - CORE\src\Console\CommandRunner.php, line 168
Cake \Console \CommandRunner->run - ROOT \bin\cake.php, line 10
[main] - [main], line O
```

**A:**
It seems the error message is quite self-explanatory. There's no tables in your database. 

Read this previous answer above. 

Bake plugin isn't magic. All it does is to generate boilerplate codes (M, V and Cs) based on tables in your database. Means you have either: 

- Didn't create tables (with CakePHP database conventions in mind) or import an existing, compatible schema in your database, OR
- Your CakePHP application is connecting to the wrong database in /config/app_local.php

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=440381)
> 
> **Warnings when using composer in MAMP (possibly causing incorrect composition)**

**Q:**
Trying to bake results in an inability to find a CLI version of PHP and inability to load 'intl' and 'sqlite3' (see below).

```
localhost: property_mgmt samford$ bin/cake bake all
Failed to find a CLI version of PHP; falling back to system standard php executable
Warning: PHP Startup: Unable to load dynamic library 'intl' (tried: /Applications/MAMP/bin/php/php8.3.14/1ib/php/extensions/no-debug-non-zts-20230831/intl (dlopen/Applications/MAMP/bin/php/php8.3.14/11b/php/extensions/no-debug-non-zts-20230831/intl, 0x0009): tried: /Applications/MAMP/bin/php/php8.3.14/1ib/php/extensions/no-debug-non-zts-20230831/intl'(no such file)), /Applications/MAMP/bin/php/php8.3.14/1ib/php/extensions/no-debug-non-ts-20230831/intl.so (diopen/Applications/MAMP/bin/php/php8.3.14/lib/php/extensions/no-debug-non-zts-20230831/intl.so, 0x0009): tried: /Applications/MAMP/bin/php/php8.3.14/1ib/php/extensions/no-debug-non-zts-20230831/intl.so' (no such file) in Unknown on line 0
Warning: PHP Startup: Unable to load dynamic library 'sqlite3' (tried: /Applications/MAMP/bin/php/php8.3.14/11b/php/extensions/no-debug-non-zts-20230831/sqlite3 (dlopen/Applications/MAMP/bin/php/php8.3.14/1ib/php/extensions/no-debug-non-zts-20230831/sqlite3,0x0009): tried: /Applications/MAMP/bin/php/php8.3.14/1ib/php/extensions/no-debug-non-zts-20230831/sqlite3' (no such file), /Applications/MAMP/bin/php/php8.3.14/1ib/php/extensions/no-debug-non-ts-20230831/sqlite3.so (dlopen/Applications/MAMP/bin/php/php8.3.14/1ib/php/extensions/no-debug-non-zts-20230831/sqlite3.so, 0x0009): tried: '/Applicat ions/MAMP/bin/php/php8.3.14/lib/php/extensions/no-debug-non-zts-20230831/sqlite3.so' (no such file) in Unknown on line 0
```

Terminal screenshot of failure to find CLI PHP and inability to load intl + sqlite3 libs.

However, I've confirmed I'm using the right version of php (using `which php` to return `/Applications/MAMP/bin/php/php8.3.14/bin/php`), and running `Applications/MAMP/bin/php/php8.4.1/bin/php -m` confirms that the 'intl' and 'sqlite3' PHP modules are present. I've gone so far as to redefine the PATH and add a PHP alias to the /.bash_profile file pointing directly into MAMP, but to no avail. Does anyone know what is happening here?

I went ahead and tried to bake the tables anyway but get these errors when trying to access them in the server:

```Fatal error: Uncaught Error: Using $this when not in object context in /Applications/MAMP/htdocs/property_mgmt/templates/Properties/index.php:8 Stack trace: #0 {main} thrown in /Applications/MAMP/htdocs/property_mgmt/templates/Properties/index.php on line 8```

Which I assume is a result of the composer not working properly? (Not sure why it's trying to use `$this` in a static method, though)

Any help would be appreciated! (no biggie tho coz I'll be in class tonight)

**A:**
It seems because you have enabled PHP extensions that is already a part of your compilation of PHP. Essentially you're loading the same extension twice. PHP would panic if this is the case. 

Solution? Just remove or comment out these clause in `php.ini` and it should work. 

Normally you only need to do this on XAMPP for Windows. PHP on Homebrew also behaves similarly to your case. Generally there's no need to change parameters in `php.ini` on macOS. 
