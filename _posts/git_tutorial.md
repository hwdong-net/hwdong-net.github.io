## git tutorial

### [git config](https://www.vogella.com/tutorials/Git/article.html)

The **git config** command allows you to configure your Git settings. These settings can be *system wide*, *user* or *repository specific*.

+ **git config --system** store system setting in file "/etc/gitconfig"
+ **git config --global** store user settings in the .gitconfig file located in the user home directory. This is also called the global Git configuration. Git stores the committer and author of a change in each commit.
+ **git config --local**  store repository specific settings in the .git/config file of a repository


You have to configure at least your user and email address to be able to commit to a Git repository because this information is stored in each commit.

```python
$git config --global user.name “your name" 
$git config --global user.email "your.email”

```


To add a new remote, use the **git remote add** command on the terminal, in the directory your repository is stored at.

The git remote add command takes two arguments:

- A unique remote name, for example, “my_remote_repo”
- A remote URL, which you can find on the Source sub-tab of your Git repo

![](https://hwdong-net.github.io/imgs/git/git_add_remote.png)

```python
git remote add my_remote_repo https://github.com/hwdong-net/cplusplus17.git
```

To push your changes into your remote repo execute the **git push <remote> <branch>** command:

```python
git push <your_remote_name>
```

[How to remove git account from local machine and add new account](https://stackoverflow.com/questions/43573790/how-to-remove-git-account-from-local-machine-and-add-new-account)


[Git & GitHub Crash Course For Beginners](https://www.youtube.com/watch?v=SWYqp7iY_Tc)
