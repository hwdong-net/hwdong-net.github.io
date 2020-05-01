## git tutorial

To add a new remote, use the **git remote add** command on the terminal, in the directory your repository is stored at.

The git remote add command takes two arguments:

- A unique remote name, for example, “my_remote_repo”
- A remote URL, which you can find on the Source sub-tab of your Git repo

![](https://hwdong-github.io/imgs/git/imgs/git_add_remote.png)

```python
git remote add my_remote_repo https://github.com/hwdong-net/cplusplus17.git
```

To push your changes into your remote repo execute the **git push <remote> <branch>** command:

```python
git push <your_remote_name>
```
