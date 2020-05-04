# git tutorial

## What is git?

### Why git?

How to track the modification history of a file?

you may use different renamed files to track different version of a file. For example:
```
work.doc
Work_2019_3_8.doc
Work_2019_9_5.doc
Work_2020_1_23_john.doc
Work_2020_2_1_smith.doc
```
If there are many modofication,there wiil be many files and you hard to know difference between them.

###  What git can do?

Git is a free and open source distributed **version control system(VCS)**. Git usae repositories to record all changes of all files of a project and can roll back to any verskion at any time. Every team member can clone a local copy called snapshot of a git repository and do work on local repository. Local repository can be pushed to remote repository on other place (shch as another machine or other place in his machine).

- Track changes of all files of a project
- Coordinates works among multiple developers
- Who made  what changes and when
- Roll back to the version of any time
- Local snapshot and remote repository


### A Short History of Git

Linus Trovaldus, the creator of linux kernel, invent the git.

![](https://upload.wikimedia.org/wikipedia/commons/thumb/0/01/LinuxCon_Europe_Linus_Torvalds_03_%28cropped%29.jpg/330px-LinuxCon_Europe_Linus_Torvalds_03_%28cropped%29.jpg)

For most of the lifetime of the Linux kernel maintenance (1991–2002), changes to the software were passed around as patches and archived files. In 2002, the Linux kernel project began using a proprietary DVCS called BitKeeper.

in 2005, the relationship between the community that developed the Linux kernel and the commercial company that developed BitKeeper broke down, and the tool’s free-of-charge status was revoked. This prompted the Linux development community (and in particular Linus Torvalds, the creator of Linux) to develop their own tool based on some of the lessons they learned while using BitKeeper. 

The goal of git is :

- Speed

- Simple design

- Strong support for non-linear development (thousands of parallel branches)

- Fully distributed

- Able to handle large projects like the Linux kernel efficiently (speed and data size)



## install git


Linux(Debian, Ubuntu)
```
     sudo apt-get install git
```
Linux(Fedora, CentOS)
```
     sudo dnf install git-all
```
Mac
```
     https://git-scm.com/download/mac
```
Windows
```
     https://desktop.github.com/
```


## [git config](https://www.vogella.com/tutorials/Git/article.html)

The **git config** command allows you to configure your Git settings. These settings can be *system wide*, *user* or *repository specific*.

+ **git config --system** store system setting in file "/etc/gitconfig"
+ **git config --global** store user settings in the .gitconfig file located in the user home directory. This is also called the global Git configuration. Git stores the committer and author of a change in each commit.
+ **git config --local**  store repository specific settings in the .git/config file of a repository


You have to configure at least your user and email address to be able to commit to a Git repository because this information is stored in each commit.

```
git config --global user.name “your name" 
git config --global user.email "your.email”
```
For example:
```python
git config --global user.email "john@example.com"
git config --global user.name "john"
```

unset
```
# Remove repository configuration
git config --unset [key]
# Remove global configuration
git config --global --unset [key]
# Remove system configuration
git config --system --unset [key]
```

### Query Git settings

```python
$git config --list
$git config --global --list
```

## local Git workflow 

![](https://hwdong-net.github.io/imgs/git/git_local.png)

**add** adds your modified files to the queue to be committed later.
**commit** commits the files in the index to the repository 

**reset** resets the index without touching the working tree,
while **checkout** changes the working tree without touching the index.

### An example of local Git workflow

1. Create a directory and enter the directory
```python
cd ../..
mkdir myproject
cd myproject
```

2. Create a Git repository in the current directory: **git init**
```python
git init
```

3. Create new file or modify content of some file
```python
ls
touch A.txt
ls
```


4. Add change to Stage(Index):  **git add . **
```python
git add . 
```

5. Commit staged changes to the repository ： **git commit**
```python
git commit -m"create A.txt"
```

6. See the current status of your repository: **git status**

modify the A.txt file
```python
vim A.txt
```

you can type i in keyboard to enter **insert** or **edit** mode and type something such as "first" in the file.

to save youe modification, type ":" colon and "wq" .

run the **git status**
```
git status
```
you will see status message :   Changes **not staged** for commit:

run 
```
git add .
git status
```
you will see status message : "Changes to be committed:".

run:
```python
git commit -m"first modification"
git status
```

7. **git show**

```python
git show
```

8. Now we add  another file
```
touch B.txt
git status
```
you will seee Status: untracked,

```
git add B.txt
git status
git commit - m"create B.txt"
git status
```
   
9. **git log** to see all version history
```python
git log
```
you will seee different version Id.

10. **git reset** to roll back the  version of any time

![](https://hwdong-net.github.io/imgs/git/git_reset.png)

```python
git reset --hard HEAD^2
```
to check if we roll back to the first version
```
cat A.txt
git log
```


## working with branchs


![](https://hwdong-net.github.io/imgs/gitgit_branch_checkout.png)


```python
# list all branches
git branch
# breate a new branch
git branch <branch>
#delete a branch
git branch -d <branch>
git branch -D <branch>
#switch between branches in a repository
git checkout <branchname> 
# you create and switch to a new branch
git checkout -b <branchname>  
```
For example:
```python
git branch
git branch another
git checkout another
vim A.txt
#do some modify ,such as add a new line
git add .
git commit - m"add a line:hwdong"
git check master
git branch -d another  # error: The branch 'another' is not fully merged.
git branch -D another  #Deleted branch another (was b98579f).
```
now breate and checkout a new branch
```python
git checkout -b another
vim A.txt
#do some modification
git add .
git commit -m"add: hwdong"
git checkout master
git merge another
git branch -d branch #Deleted branch another (was 254366f).
```

continue:
```python
git checkout -b branch
vim A.txt
# do some modification
git add .
git commit -m"add: hwdong.net"
git checkout master
vim A.txt
#do some modification
git add .
git commit -m"modified A"
git merge another #CONFLICT (content): Merge conflict in A.txt
```

you need to modifed the conflicted file in the master branch and the merge
```python
git add .
git commit -m"coflict solved"
git merge another
git branch -d another
```



[Git Checkout](atlassian.com/git/tutorials/using-branches/git-checkout):
![](https://wac-cdn.atlassian.com/dam/jcr:389059a7-214c-46a3-bc52-7781b4730301/hero.svg?cdnVersion=989)

```python
git branch
git checkout -b new_breanch
git branch
```
The branch name with the asterisk next to it indicates which branch you're pointed to at that given time. 

Now, if you switch back to the master branch and make some more commits, your new branch won't see any of those changes until you merge those changes onto your new branch.

```python
git checkout master
git branch
```

The -b option is a convenience flag that tells Git to run git branch <new-branch> before running git checkout <new-branch>.
   
12. Merge one branch to another branch: **git merge**

![](https://wac-cdn.atlassian.com/dam/jcr:b87df050-2a3a-4f17-bb80-43c5217b4947/07%20(1).svg?cdnVersion=989)

```python
# Start a new feature
git checkout -b new-feature master
# Edit some files
git add <file>
git commit -m "Start a feature"
# Edit some files
git add <file>
git commit -m "Finish a feature"
# Merge in the new-feature branch
git checkout master
git merge new-feature
# delete branch new-feature
git branch -d new-feature
```




##  working with github (Remote repositories)

![](https://hwdong-net.github.io/imgs/git/git_add_remote.png)

### 1. git clone
creates a new git repository by copying an existing one located at the URI you specify.
```python
git clone https://github.com/hwdong-net/test.git
cd test
ls
```

### 2. do some work in local repository
```python
touch A.txt
git status
git add . 
git status
git commit - m"create A.txt"
git status
```

### 3. push changes to this repository

You can push changes to this repository via **git push** as Git uses origin as default.

```
git push <repo name> <branch name>
```
for example:

```python
git push
```
Of course, pushing to a remote repository requires write access to this repository.

If there is a conflict related  with use account,you can delete github user account on windows.

```
start menu -> windows -> control panel -> User Accounts->Credential Manager ->Windows Credential ,delete the github account
```

you can do modify the repository directly in github, for example modify the A.txt file,for example all a line "hello world" to the file.

click the **submit** button. 

Then you can use **git pull** in your local machine to pull the modification from the github repository.
```python
git pull
cat A.txt
```


 
### 1. create a bare repository

```python
git init --bare
```

### 2. switch to a new directory
```python
mkdir ~/online
cd ~/online
```

### 2. clone online repository

The **git clone** command copies an existing Git repository. This copy is a working Git repository with the complete history of the cloned repository. It can be used completely isolated from the original repository.

```python
# the following will clone via HTTP
git clone http://github.com/hongwei-net/myproject.git
```
If you clone a repository, Git implicitly creates a remote named **origin** by default. The origin remote links back to the cloned repository.



### Adding remote repositories

Use  the **git remote add** command to add a new remote, in the directory your repository is stored at.

The git remote add command takes two arguments:

- A unique remote name, for example, “my_remote_repo”
- A remote URL, which you can find on the Source sub-tab of your Git repo

For example:
```python
$ git remote add origin https://github.com/user/repo.git
# Set a new remote

$ git remote -v
# Verify new remote
> origin  https://github.com/user/repo.git (fetch)
> origin  https://github.com/user/repo.git (push)
```

The **git push** command allows you to send (or push) the commits from your local branch in your local Git repository to the remote repository.

```
git push <repo name> <branch name>
```
    
```python
git remote add origin https://github.com/hwdong-net/cplusplus17.git
git remote -v
git push -u origin master
```


To push your changes into your remote repo execute the **git push <remote> <branch>** command:

```python
git push <your_remote_name>
```

[What is Git and why should I use it?](https://www.quora.com/What-is-Git-and-why-should-I-use-it)

[An Intro to Git and GitHub for Beginners (Tutorial)](https://product.hubspot.com/blog/git-and-github-tutorial-for-beginners)

[The Best Git Tutorials](https://www.freecodecamp.org/news/best-git-tutorial/)

[The Git Push Command Explained](https://www.freecodecamp.org/news/the-git-push-command-explained/)

[https://www.atlassian.com/git/tutorials/](https://www.atlassian.com/git/tutorials/)

[How to remove git account from local machine and add new account](https://stackoverflow.com/questions/43573790/how-to-remove-git-account-from-local-machine-and-add-new-account)

[git的reset和checkout的区别](https://segmentfault.com/a/1190000006185954)

[Git & GitHub Crash Course For Beginners](https://www.youtube.com/watch?v=SWYqp7iY_Tc)

[键盘上所有特殊符号的英文读法](https://www.douban.com/group/topic/12410327/)
