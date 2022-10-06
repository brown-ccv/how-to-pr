[⇦ Back to: Commit](how-to-commit.md) | [⇧ Overview](README.md) | [**⇨ Next: Review Pull Request**](how-to-pr-review.md)

# Make a Pull Request (PR)

> ⛔️TODO⛔️ include link to Notion "How to PR" detailed content for the interested. Finish that up and publish it before Friday.

The process of making a PR is:
- Push your commit(s) to a GitHub repository.
- Create a new pull request for that branch on the  [GitHub Pull Requests page](https://github.com/brown-ccv/dscov-github-workshop/pulls).

## Push the branch

### Using GitHub Desktop

### Using `git` on the command line


## Make a Draft Pull Request

- Open the [GitHub Pull Requests page for the shared repository](https://github.com/brown-ccv/dscov-github-workshop/pulls).
- Click on "New pull request"
- Choose the branches to compare:
    - Select base: main (this should be the default)
    - Select compare: your-new-branch
- Add the title and description:
  - a high-level description and 
  - a reference to the issue you're fixing. 
    - The issue has a number which looks like "#12". You can find it on the issue page.
    You should add to the pull request description something which looks like:
    ```gfm
    fixes: #12
    ```

On the "create pull request" button, there is an arrow. Click this, and select "Draft pull request."
⛔️TODO⛔️ Image of Draft PR

## Check everything

At this stage, you can check that everything is in place.

If you realise you need to do more work on the code, then [make some more commits on the same branch](how-to-commit.md) and push them using GitHub Desktop or `git push`. The PR will update automatically.

## Mark the Pull Request as "Ready for review"

When you click "ready for review" at the bottom of the pull request conversation, the reviewers will be informed that you've requested their review.

At this stage, it's best to not make any more updates until the reviewers are finished and have given their feedback. It can be frustrating for the reviewer if the code changes during their review.

If you realise that the code needs updating more, then re-mark the PR as a draft. Your reviewers will be able to see that the PR is no longer ready for review. 
⛔️TODO⛔️ Image of Switch back to Draft PR

## Wait for feedback
Wait until you have the amount of feedback you need.