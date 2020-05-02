# git tutorial

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
run the **git status**
```
git status
```
Changes **not staged** for commit:

```
touch B.txt
git status
```
**untracked**,

```
git commit - m"Add B.txt"
git status
```
staged, but notcommited

   
7. Commit staged changes to the repository: 
```python
git log
```

8. Viewing the changes of a commit:  **git show**

```python
git show
```
9. Revert changes in files in the working tree: **git checkout**

```python
git branch
git checkout -b new_breanch
git branch
```




##  Remote repositories

![](https://hwdong-net.github.io/imgs/git/git_add_remote.png)

### git clone
creates a new git repository by copying an existing one located at the URI you specify.
```python
   git clone git://github.com/user/test.git
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

[An Intro to Git and GitHub for Beginners (Tutorial)](https://product.hubspot.com/blog/git-and-github-tutorial-for-beginners)

[The Best Git Tutorials](https://www.freecodecamp.org/news/best-git-tutorial/)

[The Git Push Command Explained](https://www.freecodecamp.org/news/the-git-push-command-explained/)

[How to remove git account from local machine and add new account](https://stackoverflow.com/questions/43573790/how-to-remove-git-account-from-local-machine-and-add-new-account)


[Git & GitHub Crash Course For Beginners](https://www.youtube.com/watch?v=SWYqp7iY_Tc)
