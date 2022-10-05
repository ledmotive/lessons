### This repo contains the class lessons of the Ironhack - EY Data Analytics Bootcamp 
For each lesson there's a lesson notebook. Labs will also be done here. 
Remember to upgrade this repo periodically by adding an remote upstream (only once):


git remote add upstream git@github.com:ledmotive/lessons.git   (only the first time)

And then upgrate the repo with the commands:

git fetch upstream
git merge -X ours upstream/master

And to push the master branch to your GitHub repo:

git push origin master
git push origin master --force


Other usefull commands:

git status  (status of the local repository(
git log --all  (show all commints in local and remote repositories)


To revert any uncompleted merge or rebase:
Git merge --abort 
Git rebase --abort

To discard a successful merge go to the previous commit with:
Git reset --hard previous_commit_number
Or
Git reset --hard HEAD~1

To merge from origin preserving local changes if there’s a conflict
Git fetch
Git merge -Xours origin/master

OR equivalently:

Git pull -Xours 

Use -Xtheirs to preserve changes in the remote 


