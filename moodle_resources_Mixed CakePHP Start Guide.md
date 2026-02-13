Got it — here’s the same glossary-style summary, rewritten consistently as “The user …”.

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

* The user uses CakePHP’s official documentation:  
  * `book.cakephp.org`  
* The user uses “CakePHP at a Glance” to understand:  
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
  * additional supporting files (fixtures may appear but aren’t required here)

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
* If Git identity isn’t set, the user sets per-repo config:  
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
* The user enables “show hidden files” in File Manager to avoid missing dotfiles.

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

