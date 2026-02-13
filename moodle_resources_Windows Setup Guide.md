Here is the revised glossary-style summary written in a more direct instructional perspective (e.g., “You have installed…” rather than third person).

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
* The browser displays the server’s response.

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
* Better use of your computer’s CPU and memory.

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
* No “command not found” error appears.

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

* You may encounter “Access Denied” errors.

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

* You selected “Install for All Users.”  
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
* No “command not found” errors.

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

