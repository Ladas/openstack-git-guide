openstack-git-guide
===================

Openstack quick GIT guide. A git basics that should be enought to know, to be able to handle the whole openstack gerrit process.


Setup a gerrit
--------------

Please follow https://wiki.openstack.org/wiki/GerritWorkflow


Sending a new patch
===================

Make the changes of the files you need in your favourite project, and then go to the directory with the project, e.g.:

    cd /opt/stack/nova

You should see what files you have changed with

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
    

  
