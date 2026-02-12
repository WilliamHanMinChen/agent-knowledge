---
title: CakePHP Plugins Usage
layout: default
nav_order: 5
parent: Past Forum Posts
---

Following questions are coming from Technical Issues forum of FIT3047 and FIT3048. 

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=507864)
> 
> **Why does the Content Management System plugin replace the link prefix with `content-blocks` (e.g., makes URL read `/content-blocks/content-blocks` instead of `/dashboard/content-blocks`?**

**Q:**
When implementing the CMS provided in the IE resource repo, I've encountered an issue with how it constructs URLs. Placing the link to access the CMS within the admin dashboard provides a way for admins to access it, but it changes the URL in a manner outlined in the title. This becomes a problem when trying to navigate from this page displaying the content blocks (`http://xxxx.xxxx/xxxx/content-blocks/content-blocks`) to anywhere else on the website, as the URL now has a `/content-blocks` prefix and won't be able to find the proper controllers for the page. For example, when I try to navigate from to the Users page from this Content Blocks page by clicking the link on the sidebar, the URL becomes `http://xxxx.xxxx/xxxx/content-blocks/users`, which of course unfortunately doesn't exist (should just be`http://xxxx.xxxx/xxxx/users`). My tutor says there is a possible known solution for this issue, but he wasn't sure off the top of his head what the solution was. Can anyone provide any assistance with rectifying this issue? Just would like a way to navigate from the Content Blocks page to other pages on the admin dashboard via the sidebar. Any help is much appreciated.

**A:**
This is a known issue: [https://github.com/ugie-cake/cakephp-content-blocks/issues/6](https://github.com/ugie-cake/cakephp-content-blocks/issues/6)

First of course is not to hard-code any links in your layout or view templates; When using helpers or Router:: to create links that does not belong to the plugin, ensure the option plugin is set to null or false. For example, instead of:

```php
<?php echo $this->Html->link(
    'Foobar',
    ['controller' => 'foos', 'action' => 'bar']
); ?>
```

You should have:

```php
<?php echo $this->Html->link(
    'Foobar',
    ['plugin' => null, 'controller' => 'foos', 'action' => 'bar']
); ?>
```
(notice the `plugin` option added) This will force the router to generate route that does not belong to the plugin, but rather a global route.

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=528040)
> 
> **Error for reCAPTCHA**

**Q:**
You see, I am adding some functions in project which relates with inquiry, and I send one inquiry as dummy to test out if functions works. But when I click "submit" button, I'm saw this issue:

```
Localhost is not in the list of supported domains for this site key.
```
This error message was pretty weird -- I didn't change or modify anything about reCAPTCHA key and functions, but the error still happening. I am no ideas what the roots for cause issue and, I want to know how to fix it. Please help me out.

**A:**
For local testing, you may want to consider using [a test key](https://developers.google.com/recaptcha/docs/faq#id-like-to-run-automated-tests-with-recaptcha.-what-should-i-do) instead. Though if you're using reCAPTCHA v3, then you'll likely need to [setup the key properly](https://developers.google.com/recaptcha/docs/domain_validation#:~:text=If%20you%20would%20like%20to%20use%20%22localhost%22%20for%20development%2C%20you%20must%20add%20it%20to%20the%20list%20of%20domains.) for local testing. 

You may also want to consult your teammate who implemented this feature first. 

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=529256)
> 
> **Errors when try to create CMS**

**Q:**
I run into this error when I try to run `bin/cake migrations migrate --plugin=ContentBlocks` in my composer when following the tutorial in github:

`Connection to Mysql could not be established: SQLSTATE[HY000] [1045] Access denied for user 'brewhub'@'localhost' (using password: YES)`

The username and password are not the ones I set up in `app_local.php`

**A:**
The error message is very clear - your database username and password doesn't have privileges to whatever the database you're connecting to. 

Either the username/password combination isn't correct, or you did not grant privileges to the specific database to such user. 

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=497752)
> 
> **ContentBlocks CKSource editor removing HTML elements**

**Q:**
Hi, I understand that the CKSource HTML editor given to us in `contentblocks` has a whitelist for particular elements. But the problem comes when I try and view a `contentblock` without editing, it deletes some elements without me touching any HTML. Would appreciate it if anyone has any workarounds - solutions on the internet seem to refer to a case where CKEditor has been properly installed into a project, but I don't think that's the case in the contentblocks plugin, so they don't seem applicable.

For example, in my database, one of the values of the contentblock contains an `<i>` element. But when I open it in the CKSource editor, the `<i>` elements are gone, without me changing anything.

**A:**
The ContentBlocks plugin uses HTMLPurifier to sanitise input. Means anything that is considered dangerous are filtered out. This apparently including HTML form components. 

It is indeed quite dangerous to allow end users making their own HTML form, as you have no idea what they'll create, and if your controller can handle the form that is totally out of the scope of your design. 

If this is such a necessary feature in your design, you can implement the plugin as a local plugin and make changes, then relax the HTMLPurifier to allow the tags you wanted and customise CKEditor to allow WYSIWYG editing on form tags. If you're adding these tags but the editor isn't aware, you're hacking it and kind of defeat the purpose. 

**Q:**
The same thing is happening to me for json formats.

regarding your recommendation to how to solve this, is creating a local plugin the best way to solve this? so does that mean it won't use contentblock anymore? sorry I'm having trouble visualizing the changes we need to make to allow for this. Or do you have any other recommendations for how people usually deal with this?

**A:**
It's not that you're not using the plugin, but rather instead of installing the plugin from upstream, you're installing the plugin as a part of your own codebase (still as a plugin) so you can change its behaviour as you wish. Currently the only thing you can override in any plugin is its view templates, which won't be helpful in your use case.

Once the plugin become local and a part of your codebase, you can change whatever you'd like. You should do this especially when you're trying to alter its Controller's behaviour. 

It should be as simple as copy the plugin folder from `/vendors` to `/plugins`, and uninstall the upstream plugin using Composer to avoid conflict. Then you'll be able to change the codes in plugin (likely one of the controller files) to achieve your goals.
