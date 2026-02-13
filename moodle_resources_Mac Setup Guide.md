## **MacOS Local Development Setup (Industry Experience Resource Repository) — Glossary Points**

### **Scope / which video**

* You’re following the **MacOS setup** walkthrough.  
* If you’re on **Windows**, you should switch to the Windows-specific video instead.

---

## **Quick concept recap: how websites work**

* **Native apps** (PC/tablet/phone) run code directly on your device and usually need to be compiled \+ installed.  
* **Web apps** are different:  
  * You interact with a **browser**, not the raw website code.  
  * The browser **renders** the site and handles user interaction.  
  * Most application logic runs on a **server** (a computer running 24/7 somewhere).  
* A typical server stack includes:  
  * **Web server (Apache / httpd):** “gatekeeper” that receives browser requests and serves responses/files.  
  * **PHP interpreter:** runs your backend PHP code (PHP is interpreted like Python).  
  * **Database (MariaDB/MySQL):** stores and serves your data.  
  * **phpMyAdmin:** GUI to manage the database.

---

## **Why you set up a local environment**

* You run the “server stack” on **your own Mac**, not in the cloud.  
* Benefits:  
  * Faster feedback loop (no network/cloud latency)  
  * Costs nothing (no cloud server fees)  
  * Uses your Mac’s resources (CPU/RAM/storage)  
  * Lets you keep the stack running as background services so it “just works” when you open the browser

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
  * Chrome DevTools are generally more convenient for debugging than Safari’s tooling  
* Install method (Mac standard):  
  * Download `.dmg`  
  * Drag Chrome into **Applications**  
  * Optionally pin to Dock  
  * You can stop using Safari for this workflow afterward

---

## **2\) Package manager: Homebrew \+ Xcode CLI tools**

* You install **Homebrew** (developer “app store” / package manager).  
* Installing Homebrew also installs **Xcode command-line tools**, which includes **git**.  
* Apple Silicon vs Intel note:  
  * Apple Silicon typically uses `/opt/homebrew/...`  
  * Intel Macs often use `/usr/local/...`  
  * Follow **your screen/paths**, not someone else’s, when editing config files.

Useful commands (from your reference):

brew update  
brew upgrade \<package\_name\>  
brew doctor

* `brew doctor` is your quick “is Homebrew healthy?” check.

---

## **3\) Git check (comes with Xcode CLI tools)**

* After Homebrew/Xcode CLI tools, you verify Git works:

git \--version  
\# or just:  
git

* If Git isn’t found, you likely need to reinstall / repair Xcode command line tools or search the specific error you got.

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
  * Homebrew’s Apache commonly uses **8080** to avoid conflicts with macOS services.

Start and verify the service:

brew services start httpd  
brew services list

Confirm what ports it’s listening on:

sudo lsof \-i \-P \-n | grep LISTEN | grep httpd

Test in browser:

* You load:  
  * `http://localhost:8080`  
* If you see a basic “It works” page, Apache is serving content.

---

## **5\) Understand DocumentRoot (web root)**

* The “DocumentRoot” is the folder your web server serves files from.  
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

* phpMyAdmin isn’t automatically visible at `/phpmyadmin` until you add an Apache config section.

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

* phpMyAdmin is served via an **Alias**, so it won’t necessarily appear as a physical folder inside your DocumentRoot.

---

## **11\) phpMyAdmin login restriction (root login change)**

* Modern MySQL/MariaDB setups often restrict **root** login via web UI for security.  
* You may find:  
  * root works only in terminal, or  
  * login differs based on macOS auth methods.  
* The video’s approach:  
  * Create a separate “super user” that you use in phpMyAdmin.

---

## **12\) Create a “super user” in MariaDB/MySQL (for phpMyAdmin)**

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

## **13\) Optional “bonus” convenience: auto-login in phpMyAdmin**

* You can configure phpMyAdmin to store credentials so you don’t type them every time.  
* You edit phpMyAdmin’s config file (Homebrew caveats usually show the exact path).  
* The video’s key changes:  
  * Change auth type from `cookie` to `config`  
  * Add stored user/password lines for the first server entry  
* Security note:  
  * This is convenient for local dev but it stores credentials in a config file.

---

# **Final checklist: verify everything works**

You confirm each component:

### **Homebrew**

brew doctor

Expect: “Your system is ready to brew.”

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
  * see “User accounts” / admin capabilities (basic privilege check)

---

## **Outcome**

* You’ve installed and configured:  
  * Chrome  
  * Homebrew \+ Xcode CLI tools (includes git)  
  * Apache (httpd) running as a background service on port 8080  
  * PHP integrated with Apache  
  * Composer installed via Homebrew  
  * MariaDB running as a background service  
  * phpMyAdmin enabled via Apache Alias  
  * A dedicated super user for database administration in phpMyAdmin

If you want, I can condense this into a one-page “commands-only” checklist while keeping the important paths (DocumentRoot, config paths, ports) visible.

