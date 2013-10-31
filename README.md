openstack-git-guide
===================

Openstack quick GIT guide, for beginners with Openstack and GIt. A git basics that should be enought to know, to be able to handle the whole openstack gerrit process.


Setup a gerrit
--------------

Please follow https://wiki.openstack.org/wiki/GerritWorkflow


Sending a new patch
===================

Go to project directory with your favourite project. e.g:

    cd /opt/stack/nova
    
Create a new branch where you will be doing the changes:

    git checkout master # goes to the master branch
    git fetch --all # fetches the remote repositories
    git pull origin master # updates the master with the latest version
    git checkout -b bug/xxx # creates a new branch from the actual branch

Make the changes of the files you need in your favourite project, you should see what files you have changed with

    git status
    
You should see what lines you have changed with

    git diff
    
Now you can add the changed files to repository, adding dirrectory name will add all changes files within the directory

    git add <file_name or directory name>
    
You should see that the files are added running git status again. You need to all files to be added or stashed. 
Commit won't be possible with files not added. You can: 

    # stash files
    git stash save # stash changed files and allows you to commit
    git stash pop # will return the stashed files back
    # revert the changes
    git checkout -- <file or directory>
    
Now you are ready to commit the changes. Please follow the https://wiki.openstack.org/wiki/GitCommitMessages#Summary_of_GIT_commit_message_structure

    git commit # will prompt you to add commit message
    
Now you should check that your commit has a change-id and is correctly connected with your blueprint or bug with:

    git log
    
If everything is fine, you can send the commit to gerrit by:

    git review
    
Editing a patch and sending it again
====================================

Due to several reasons, you will need to edit the changes in your commit and sent it again. It's important to edit
the existing commit and keep the change-id. Gerrit will create a new patchset for each new change, it reckognizes that 
it's the updated commit thanks to change-id. 

Easiest way to add files to commit is

    git commit --amend # adding new changes to latest commit
    
If you have created another commit and you want to merge it to the previous commit, you can do it via iteractive rebase.
Always backup your git when you rebasing. E.g. by creating another branch from branch you have right now with:

    git checkout -b testing_rebasing_branch
    # then merge the patches
    git rebase -i HEAD~2 # HEAD~2 means it takes 2 last commits. It opens a vim with iteractive rebase. Check the options
                         # the easiest merge is add letter "s" in front of commit you want to squash into the other commit. 
    
If you havent sent you commit to gerrit yet (via git review), you might decide just to cancel the commit and start again.
You will delete the commit with.

    git reset HEAD~1 # HEAD~1 means the last commit, it resets the commit, but keeps your files so you can 'git add'
                     # them again. There is also --hard option, that will also delete the changes.
                     

Rebasing with the latest master
===============================

Too keep your patch up to date you will often need to rebase. Rebasing is described here  https://wiki.openstack.org/wiki/GerritWorkflow
The easiest way to rebase is:

    git checkout master # go to branch master
    git fetch --all # fetches all the remote changes
    git pull origin master # updates the master with the latest changes
    git checkout <your branch> # go to branch that holds your patch
    git rebase master # rebases your branch with latests master commits
    git review # send rebased patch to gerrit and it's done :-)
    
You may have conflicts during rebasing, it will mark the conflicts in your files. You need to edit those files,
then 'git add' them then finish the rebase with 'git rebase --continue'
