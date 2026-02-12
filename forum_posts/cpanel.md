---
title: cPanel
layout: default
nav_order: 2
parent: Past Forum Posts
---

Following questions are coming from Technical Issues forum of FIT3047 and FIT3048. 

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=468692)
> 
> **cPanel: "you have reached the maximum number of domains for this account"**

**Q:**
When trying to create a subdomain using cPanel for the onboarding system it shows the error above. should i just use the main domain instead?

```
Domain: Enter the domain that you would like to create:
    _ onboarding.system _

Error:
You have reached the maximum number of domains for this account.
Please contact your hosting provider for more information, or create a subdomain on an existing domain on this account.
```

**A:**
You have missed a very important article in the IE Resource Repository before using your development resources: 

[Workaround on creating subdomains in cPanel version 106 and up](../cpanel/subdomain_creation_workaround)

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=472035)
> 
> **Structure of CPanel folders and git**

**Q:**
At the beginning of the semester it was mentioned that we would have a development, review and approved folder for our system. In CPanel, is it better to structure it so that there are three different folders all of the repository but we just choose when to pull for each one? Or should we instead use separate branches for the reviewing and development? The video provided on launching our project on CPanel mentions we would have different folders, but copying code from review to approved seems tedious and errorprone, not to mention that in that case there would be three instances of the repository within our files which is a lot of wasted space.

**A:**
You can (and should) have 3 different and separate pulls of your git repo on your IE cPanel server for each of the suggested subdomains (dev, review and prod). They should also use a different database as you may have different schema between those versions.

As of which git upstream each folders/subdomains have (all from main branch or different branches) it's really up to you. In general, you have:

- dev: you can do whatever you want. It's volatile
- review: it shouldn't be updated until you're submitting the iteration for integrity testing
- prod: it shouldn't be updated until your iteration has passed integrity testing and approved for UAT

where all above were explained in earlier studios (including the tech mini workshop on week 1). If you're unsure, it's always a good idea to consult your tech studio mentors.

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=472986)
> 
> **Could not start the session**

**Q:**
Hello,

When i try to open my website through cpanel i am getting the following error

```
Could not start the session
Cake\Core\Exception\CakeException
```

I have checked the error log and it says

`[Sun Aug 24 17:00:02.968955 2025] [authz_core:error] [pid 975493:tid 975493] [client 0.0.0.0:17292] AH01630: client denied by server configuration: /srv/cpusers/u00s0000/public_html/php.ini, referer: https://gamma.iedev.org:2083/`

> fixed it, happened due to cpanel creating user.ini file in public_html which had two session path

**A:**
It’s not recommended to customise session path from the system default due to potential permission issues. Also could not start session error can be caused by code error, for example, the visitor is using an old cookie session key encrypted by a different secret in `/config/app_local.php`, or unexpected, premature prints/outputs in Model or Controller (usually speaking in MVC the only place to generate output is View).

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=472700)
> 
> **Not found the requested URL**

**Q:**
When i am opening my project in cpanel I am getting the following error

`Not Found The requested URL was not found on this server. Additionally, a 404 Not Found error was encountered while trying to use an ErrorDocument to handle the request.`

I have stored the files for the website in the following directory, u23s1999.iedev.org/dev,

**A:**
I don’t think the domain u23s1999.iedev.org belongs to your team. Would you like to double check?

**Q:**
yeah how can i check it?
And Also i am using gd extension for my captcha how to i enable it in cpanel

**A:**
root domain of your account is listed on the dashboard sidebar.

PHP extensions and parameters can be set using Multi-PHP manager. Documentation can be found using cPanel’s help or on cPanel Support docs website. Check current PHP settings with phpinfo() first, maybe the extension you want is already a part of the current configuration. You can also force Composer to check required PHP extension by adding them to your composer manifest.

Note when configuring PHP with MultiPHP manager it will generate several `.ini` files (that overrides system wide `php.ini`) in your documentroot. You will need to exclude them in your git (if you ever choose to make any comment from IE cPanel, which you shouldn’t, but just in case) as those files could conflict with any local Apache/PHP setup or most commercial cPanel deployments that uses CloudLinux PHP Selector, which switches PHP version with container instead.

**Q:**
the mitliphp manager doesnt have an option to set or enable extension, I have tried to put extension = gd in multiphp editor but that didnt do anything

**A:**
MultiPHP Manager doesn't support disable/enable extensions. However GD, and many other common PHP extensions, should have already been enabled on the server. 

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=497320)
> 
> **cPanel transferring error**

**Q:**
when I try to open a link from the home page of our website it comes up with the error below. also a lot of the styling on the homepage doesn't seem to be working like it does on localhost

```
Not Found
The requested URL was not found on this server.
Additionally, a 404 Not Found error was encountered while trying to use an ErrorDocument to handle the request.
```

**A:**
There’s not much to work with, so here are my guesses:

- links are hard-coded instead of generated with helpers, means the links would behave differently when the website is running from a subdirectory vs domain root
- htaccess files, either or both the one in cake root and webroot may be altered or damaged which somehow not crashing the web server, but prevents rewriting from working properly
- can’t think of a point three at this moment

Given the look of this 404 error page (Apache looking, so you’re not using the Cake’s built-in dev server that is on top of PHP websever, but CakePHP error pages are not being rendered, means the request is not routed through CakePHP pipeline, means Apache thinks you’re requesting something outside of cake root), I think the first possibility is the most likely case.

**Q:**
it was the second one. when i tried moving files in cpanel, the hidden files didn't get moved so I had to change the setting to see hidden files, then move them so that `.htaccess` would also move. It works now, thank for the help.

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=499816)
> 
> **cPanel email setup issue**

**Q:**
I was using Mailtrap’s sandbox locally, and after moving to cPanel I changed the account, password, and settings to cPanel’s. However, it still doesn’t work. I’ve tried multiple times but couldn’t fix it, even though the network side shows a 200 response.

**A:**
What domain are you trying to email to? cPanel only supports sending to IE domains. What we have done, and you can do too if you need an account for testing, is on your cPanel dashboard, under email, you will find an option to create email accounts. Your server will be able to send emails to these, since they will be on your domain, and you can open the inboxes through that same menu.

**Q:**
The “forgot password” feature needs to send a link to the user to reset their password. Our administrators can directly reply to users’ contacts in the dashboard. I tried changing the email settings in app_local to use the cPanel email connection, but it still doesn’t work.

**A:**
You can only send emails to the email accounts on your cPanel, as Emma said. So if your user is a different domain, like gmail.com, or monash.edu, it won't send it, it will instead end up "trapped" and go to the default server email address on your cPanel.

Get used to using the mail app on your cPanel to ready emails you are sending (i usually have an extra tab open to check emails while I am testing)

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=453664)
> 
> **'Could not start the session, headers already sent in file' when deploy to cpanel**

**Q:**
I'm experiencing deployment issues like below when trying to login. It works perfectly on localhost but fails when deployed to cPanel hosting.

```
Could not start the session, headers already sent in file
```

Could you please advise on what might be causing this and how I can resolve it?

**A:**
One common cause of this error message is if there is any white space or raw html before the <?php or after the closing ?>. The full text of your error message should include the source file that's causing the problem. If this isn't the problem, check the top answer to this stack exchange question for other possible causes/solutions: [https://stackoverflow.com/questions/8028957/how-to-fix-headers-already-sent-error-in-php](https://stackoverflow.com/questions/8028957/how-to-fix-headers-already-sent-error-in-php)

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=465844)
> 
> **Setting Up cPanel with git**

**Q:**
From the error it seems our ssh connection is not working. We have tried creating new ssh keys and followed instructors to put the public key into our git profiles but we cant seem to get it working. Is there a video or some resources we can access to help fix theese issues?

**A:**
SSH connection on FIT GitLab is restricted. For students only HTTPS is available for use. 

For IE cPanel (gamma.iedev.org) server the SSH is generally jailed with limited features. Also there's no git server hosted on it so you're not supposed to add cPanel as your git remote. 

Did you try the demo/steps in the "Begin with CakePHP" video? (i.e. clone git repo to cPanel account root, connect subdomain to the folder, setup, etc.)

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=471943)
> 
> **Web Hosting Service - Clarification**

**Q:**
My client currently has a web hosting service but is not too tech savvy so he doesnt know the exact information to provide to us. He has asked us to prepare list/message of what we need so that he can send it directly to his developer. I was wondering if we could get some clarification as well on what information exactly that we need regarding the web hosting service. Thanks!

**A:**
A few things:

- Figure out which ISP they're using
- Figure out what technology their website is based on
- Find out if the current hosting can be reused (either replace the current system or hosted side-by-side with the new system)

Finally:

- If to reuse the current hosting or ISP, get access to your client's hosting account (either get delegated access or credentials)
- If need a new hosting, discuss this with your studio mentors

If not sure about anything, discuss with your studio mentors. 

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=497234)
> 
> **Clarification on Hosting Setup**

**Q:**
I have a question regarding a client’s setup. My client is using Crazy Domains, and since they didn’t already have hosting, we had them purchase a basic Linux web hosting plan. However, when I logged into the Crazy Domains cPanel, I noticed that it looks quite different from the IE cPanel I’m familiar with.

For example:
- There’s no Terminal option available, so I’m unsure if we’re supposed to use SSH for access.
- When I click Manage under Domains, it says “no config option exists for the XXX domain.”
- The document root is `public_html`. should I deleted the existing files in that folder and uploaded the project files directly without using `git clone` and created a new directory.
- I’m also not sure how best to handle `composer install` for dependencies like `app_local.php` on this environment.
- Also I am not sure why it is saying WordPress and when I try to click **edit site** it show error 404 so I could not explore that part.

If this were a testing or IE cPanel, I’d normally experiment and see what breaks, but since this is for a client, I don’t want to take unnecessary risks.

Is there an official guide (or Crazy Domains–specific documentation) on how to properly deploy a PHP application in this environment? Or is it more a case of researching and piecing things together based on the hosting limitations and taking risky action.

**A:**
To answer your questions: 

- It's quite normal that there's no web terminal on commercial cPanel for security purposes. In some cases you don't even have SSH access/shell access at all. 

If a shell access through SSH is available, there's likely no Composer installed. You can ask Copilot with prompt "Use composer.phar with php command without installer". However if there's no shell access at all, you'll need to clone your repo locally first, use Composer to complete the puzzle locally, then zip and upload all files to the hosting using file manager instead. cPanel shell is NOT a requirement for CakePHP project to work. 

-  You cannot change the document root of the main domain name. It has to be `~/public_html`. IE cPanel server has the same behaviour. 

- You can clean up the `public_html` folder and ensure there's no file (including hidden files) in there, which enables you to clone directly into `public_html` folder if you define git clone destination to current folder (notice the dot), such as `git clone <your repo uri> .`

- Answered above

For the last question and screenshot, I'm not sure what's the WordPress is about. Maybe the hosting think there's a WP site on that domain? From what I see, it seems the domain name isn't really pointing to the hosting plan your client have purchased. Domain name and hosting aren't naturally bonded together, especially when there's already a website on the domain. Worth some investigation. Such as checking if default subdomains (@ and www) have A records pointing to a different IP address other than your hosting account (you can ask Copilot on how to check these using `dig` command or tools like [mxtoolbox.com](https://mxtoolbox.com/SuperTool.aspx)). 

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=505425)
> 
> **exec_shell() command permission limitation on cPanel**

**Q:**
It is unfortunate for us to discover that we were barred from running `exec_shell()` on cPanel from our cakePHP project. 

This command is imperative to run for our success in building a doable system for the clients. Our clients require a functionality that involves interacting with a sizeable XLS spreadsheet database; and the necessity to work with docxtpl for word templating rendered it necessary to create python scripts as to avoid the limitation of functionalities that PHP can provide.

On our local system, `exec_shell()` was successfully called to execute the python script for the generation of reports based on a word document template and the uploaded csv database. 

We would like to know if there is any workaround for the situation, and if it is at all possible for you to elevate our authorisation/permission level on cPanel.

If the proposed solution could not be fulfilled, we are glad to use our own Azure server. 

**A:**
I assume you mean `exec()` and its derivatives including `shell_exec()`? You'll find these functions disabled on most commercial cPanel services due to obvious security implications **by default**. However if you're having them on shell-triggered PHP instances, such as in a cronjob, you may selectively alter `disable_functions` when needed. Or its behaviour can be altered with MultiPHP ini Editor or `.htaccess` directives. 

Though we have teams in the past that heavily rely on `exec()` to call other commands which **practically all of them** encountered interoperability issues on implementation.  

As of your reasons - Python is as inefficient as PHP as both are dynamically interpreted. 

**Also I should remind you that hosting your client's project on an external, public server that does not belong to Monash or your client is violating the deed of confidentiality and client legal agreements.**
