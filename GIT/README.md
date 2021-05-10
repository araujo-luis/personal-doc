# GIT Docs

## When you commit changes in a wrong branch

First, go to the branch where you committed your changes and run 

    git reset --soft HEAD^
    
 Then create your branch and commit your changes
 
    git checkout -b your-branch
    git add .
    git commit -m "Your commit"
    git push origin your-branch
    
 If you push a single commit an a wrong branch, you can run reset --soft or --hard, depending if you want (or not) your changes, then run

    git push --force
