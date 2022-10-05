[⇦ Back to: Issue](how-to-issue.md) | [⇧ Overview](README.md) | [**⇨ Next: Commit**](how-to-commit.md)

# Make a Branch based on "main" where you'll solve the issue

- **What is a branch, anyway?** 
  > 💡 A branch is a movable label attached to a particular commit. If you have a branch "checked-out", then when you make a new commit "on the branch", the branch is updated to point to the new commit. You'll make a series of commits, and then merge the final commit in that series (which *is* the "branch") back into "main". 
- **What is the main branch?**
  > 💡  The default branch is called "main". The current most up-to-date version of the code, excluding any not-yet-merged pull requests, is the commit which the main branch points to.
- **Some of the documentation says that "master" is the default branch. Why the difference?**
  > 💡  The default was formerly called "master," a term which has been replaced to its association with slavery. Some Git documentation still uses the old term.

[More info on branches in the Git Book.](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)

There are several ways you can make a new branch.
- On the GitHub issue page (the easiest option) 
- In the GitHub Desktop App (the second easiest option)
- Using the Git command line tool (the most flexible option)

For all of the methods, you should follow these rules of branch naming:
- `use-lower-case-with-dashes-separating-the-words`
- Choose a descriptive name, like `34-feat-update-history-section-formatting` or `42-fix-broken-links-in-introduction` to help others navigate the branches. 
 > ⚠️ Projects involving a few people might have tens of concurrent branches. Non-descriptive names like `test-branch1` or `final-update`, `final-update-2` make the task of remembering which branch is which overwhelming or impossible.


## On the GitHub issue page
Click "Create a branch" on the issue page.
<img width="1110" alt="Screen Shot 2022-10-05 at 15 46 03" src="https://user-images.githubusercontent.com/2803227/194149229-899e9a1c-a97d-47f4-adbc-a12dbd47d670.png">

A branch name will be suggested, and you can specify how you want to get access to the new branch. 
<img width="1110" alt="Screen Shot 2022-10-05 at 15 49 52" src="https://user-images.githubusercontent.com/2803227/194149861-71146984-1872-4e38-9391-a8e77b401e8a.png">

If you click "Open branch with GitHub Desktop", you should automatically be prompted to open the GitHub Desktop App. (If you've not already downloaded the Repository, the Desktop app will handle that for you at this stage.)

If you click "Checkout locally" you'll be prompted to run some commands in your local copy of the repository.<img width="1110" alt="Screen Shot 2022-10-05 at 15 53 02" src="https://user-images.githubusercontent.com/2803227/194150561-819ec255-6b2b-4d01-b30b-264df4f3c1a5.png">

The new branch will be created and you can start to make changes in your text editor.

## Using GitHub Desktop

Click on "Current Branch" and then "New Branch" <img width="1068" alt="Screen Shot 2022-10-05 at 15 57 53" src="https://user-images.githubusercontent.com/2803227/194151554-1dbc056b-bb92-4bf3-8104-73f4de8ac31a.png">

If you currently have the main branch checked out, you'll be asked to name your new branch.
<img width="1112" alt="Screen Shot 2022-10-05 at 16 02 01" src="https://user-images.githubusercontent.com/2803227/194152194-feb675e5-e41b-4604-89c2-ca060c28742c.png">

If you currently have a different branch checked out, you'll be asked to name your branch *and* choose to "Create branch based on..."
<img width="1112" alt="Screen Shot 2022-10-05 at 15 59 45" src="https://user-images.githubusercontent.com/2803227/194152020-b049819a-cc5a-475d-8571-16f088fd2b07.png">

Here you should choose the "main" branch to base your new branch on.

The new branch will be created and you can start to make changes.

## Using `git` on the command line

If you are already on the branch where your changes need to be included – in our example always the "main" branch, then you can run this command to switch to a new branch you **c**reate
```shell
git switch -c 34-fix-formatting
```
If you have a different branch currently checked out, then you can do the following:
```shell
git switch -c 34-fix-formatting main
```
... where `main` is the name of the "start-point," that is the commit where your new branch will begin.


