---
title: Git
layout: default
nav_order: 6
parent: Past Forum Posts
---

Following questions are coming from Technical Issues forum of FIT3047 and FIT3048. 

----

{: .note-title }
> [Original forum post](https://learning.monash.edu/mod/forum/discuss.php?d=528092)
> 
> **Merging Branch issue**

**Q:**
I’m having persistent issues with merging my branches and was hoping to get your guidance.

I created a rescue branch, which has the most recent and correct version of my project (it contains all the latest updates and fixes).The main branch, however, still has an older, outdated version, missing several of the updates from the rescue branch. When I try to merge or rebase the rescue branch onto main, Git says there’s nothing to merge/rebase because the branches are already up to date, even though visually they’re not. The main branch still lacks many of the new changes.

I also tried reverting commits and repeating the merge/rebase, but I get the same result each time. How do I properly sync main so it matches the rescue branch’s version (the one with all my updates).

**A:**
It seems that after several merges, your main branch is actually "newer" than the rescue branch. That's why it shows already up to date - git believes all changes in rescue is already the foundation of the current main branch. 

If you're so sure rescue branch is the "truth" and want to give up whatever is already on the main branch, you may need to checkout the rescue branch, then rebase it to main branch: 

- (you probably want to) git pull (or `git fetch --all` to ensure your local repo is up to date)
- `git checkout rescue`
- `git rebase main`

Though note you'll lose some commit history in main branch (since they're newer than rescue branch) and you may encounter conflicts that you must resolve. Use the git tools in PHPStorm may be easier to deal with conflicts as it has a great diff tool that help you to decide what piece of codes you really want. 

**Q:**
I rebased the rescue file onto main, which worked great. But then git said there was 6 new incoming commits and when I clicked update, it removed the new edits, files, with new errors popping up. How do I update and delete the conflicting commits?

**A:**
Technically speaking you can't. That's how version control works. It's not how new the files or changes are, but rather if new/existing comments have children and ancestors.

I had another look at your repo. It seems what I said above is happened to be the case. So the situation is that:
- Since main is newer (due to all those merges of other branches), updating rescue branch from main would likely override your changes in rescue branch
- While rebasing main with rescue branch, there will be conflicts. Upon resolving those conflicts to continue the rebase (and commit the changes), you won't be able to push the changes because the remote is "newer" than local and would override the local on pull

I know this is very confusing. This is confusing to me as well.

I have manually synced everything in rescue branch to main and created a commit. I have also rebased rescue branch from main so now both branches are in sync. If you noticed anything missing from all these operations, just check commit history and everything should still be there.
