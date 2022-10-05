# dscov-github-workshop
Demonstration repository for the DScoV Lunch Workshop: Collaborative Coding for Science using Git, GitHub and Pull Requests

## Aim for the Workshop

> 🎯 We're going to collectively format a copy of the [Wikipedia page about Git](https://en.wikipedia.org/wiki/Git) using "[Markdown](https://daringfireball.net/projects/markdown/)," one of the text formats which GitHub uses for README files, comments on code, etc. 


## Approach 

- **Start** We'll start from an incorrectly formatted version of the [Wikipedia page about Git](https://en.wikipedia.org/wiki/Git), which you can find here: https://github.com/brown-ccv/dscov-github-workshop/blob/main/git-text-content.md. 
- **Issue**: Each person will choose an issue.
- **Pull Request**: Each person will make a "pull request" (PR) to improve the part of the document referenced by the issue. 
  - The author of each PR will request a review from their peers.
  - Everyone will have the chance to look at the PR and suggest changes and improvements, or to say "this is great, let's merge it!"
  - The author will merge the approved PR.
- **Update Pull Requests**: All the other PRs can be updated to incorporate the new changes, before they too are merged. 
- **Merge Conflicts**: If people work on the same part of the document, then the authors will resolve the conflicting changes.

## Getting Started

### Prerequisites
- You have a GitHub account (get one here: [https://github.com/](https://github.com/)).
- You have a way to clone and modify the repository on your machine:
  * either the [GitHub Desktop app](https://docs.github.com/en/desktop/installing-and-configuring-github-desktop/installing-and-authenticating-to-github-desktop/installing-github-desktop) (recommended) or
  * the [GitHub Command Line Tool `gh`](https://github.com/cli/cli#installation) *and* the [Git command line tool `git`](https://git-scm.com/downloads)
- You have a text editor ([VSCode](https://code.visualstudio.com) is commonly used and a pretty good general text editor.)

### Preparation
- Download the repository,
- Open the repository in your text editor.

## How to make a PR

### Choose your Issue
- Look at the [list of issues](https://github.com/brown-ccv/dscov-github-workshop/issues/) 
- Pick one you like. 
- Assign yourself to the PR. <img width="1110" alt="Screen Shot 2022-10-05 at 15 35 27" src="https://user-images.githubusercontent.com/2803227/194147465-3e1cf130-4695-42c0-a758-8caf21124010.png">

### Make a Branch based on "main" where you'll solve the issue
There are several ways you can make a new branch.

#### On the Issue Page on GitHub (the easiest option) 
Click "Create a branch" on the issue page.
<img width="1110" alt="Screen Shot 2022-10-05 at 15 46 03" src="https://user-images.githubusercontent.com/2803227/194149229-899e9a1c-a97d-47f4-adbc-a12dbd47d670.png">

A branch name will be suggested, and you can specify how you want to get access to the new branch. 
<img width="1110" alt="Screen Shot 2022-10-05 at 15 49 52" src="https://user-images.githubusercontent.com/2803227/194149861-71146984-1872-4e38-9391-a8e77b401e8a.png">

If you click "Open branch with GitHub Desktop", you should automatically be prompted to open the GitHub Desktop App. If you've not already downloaded the Repository, the Desktop app will handle that for you.

If you click "Checkout locally" you'll be prompted to run some commands in your local copy of the repository.<img width="1110" alt="Screen Shot 2022-10-05 at 15 53 02" src="https://user-images.githubusercontent.com/2803227/194150561-819ec255-6b2b-4d01-b30b-264df4f3c1a5.png">

#### Using GitHub Desktop (the second easiest option)

Click on "Current Branch" and then "New Branch" <img width="1068" alt="Screen Shot 2022-10-05 at 15 57 53" src="https://user-images.githubusercontent.com/2803227/194151554-1dbc056b-bb92-4bf3-8104-73f4de8ac31a.png">

If you currently have the main branch checked out, you'll be asked to name your new branch.
<img width="1112" alt="Screen Shot 2022-10-05 at 16 02 01" src="https://user-images.githubusercontent.com/2803227/194152194-feb675e5-e41b-4604-89c2-ca060c28742c.png">

Choose a descriptive name, like `34-feat-update-history-section-formatting` or `42-fix-broken-links-in-introduction`. 
Use lower-case words, separated by dashes.

If you currently have a different branch checked out, you'll be asked to name your branch *and* choose to "Create branch based on..."
<img width="1112" alt="Screen Shot 2022-10-05 at 15 59 45" src="https://user-images.githubusercontent.com/2803227/194152020-b049819a-cc5a-475d-8571-16f088fd2b07.png">

Here you should choose the "main" branch to base your new branch on.

#### Using the Git command line (the most flexible option)

### Solve the issue
Make a series of commits to solve the issue.
- Making small changes to the text, formatting 


Once you're done, repeat from step 1).


You can do this in one of three ways:

- You'll start a draft PR, with 

  - a high-level description and 
  - a link to the issue you're fixing. 
- Once you think you're done, please mark the PR as "ready for review" and request two or three reviewers.
## Definition of Done
A part of the document is done if it is:
- content-complete,
- free of factual errors,
- free of repetition,
- free of grammatical errors,
- correctly formatted when viewed in the preview.

- improve the formatting

## Tools


## Resources
- [GitHub flavoured Markdown](https://github.github.com/gfm/) specifies precisely how to write Markdown text for GitHub


## Examples

