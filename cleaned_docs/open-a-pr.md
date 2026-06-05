{{< note >}}
Code developers: If you are documenting a new feature for an
upcoming Kubernetes release, see
Document a new feature.
{{< /note >}}
To contribute new content pages or improve existing content pages, open a pull request (PR).
Make sure you follow all the requirements in the
Before you begin section.
If your change is small, or you're unfamiliar with git, read
Changes using GitHub to learn how to edit a page.
If your changes are large, read Work from a local fork to learn how to make
changes locally on your computer.

Changes using GitHub
If you're less experienced with git workflows, here's an easier method of
opening a pull request. Figure 1 outlines the steps and the details follow.

{{< mermaid >}}
flowchart LR
A([fa:fa-user NewContributor]) --- id1[(kubernetes/websiteGitHub)]
subgraph tasks[Changes using GitHub]
direction TB
    0[ ] -.-
    1[1. Edit this page] --> 2[2. Use GitHub markdowneditor to make changes]
    2 --> 3[3. Select Commit changes...]
end
subgraph tasks2[ ]
direction TB
4[4. Select Propose changes] --> 5[5. Select Create pull request] --> 6[6. Fill in Open a pull request]
6 --> 7[7. Select Create pull request] 
end
id1 --> tasks --> tasks2
classDef grey fill:#dddddd,stroke:#ffffff,stroke-width:px,color:#000000, font-size:15px;
classDef white fill:#ffffff,stroke:#000,stroke-width:px,color:#000,font-weight:bold
classDef k8s fill:#326ce5,stroke:#fff,stroke-width:1px,color:#fff;
classDef spacewhite fill:#ffffff,stroke:#fff,stroke-width:0px,color:#000
class A,1,2,3,4,5,6,7 grey
class 0 spacewhite
class tasks,tasks2 white
class id1 k8s
{{</ mermaid >}}
Figure 1. Steps for opening a PR using GitHub.

On the page where you see the issue, select the Edit this page option in the right-hand side navigation panel.

Make your changes in the GitHub markdown editor.

On the right above the editor, Select Commit changes.
   In the first field, give your commit message a title.
   In the second field, provide a description.

{{< note >}}
   Do not use any GitHub Keywords
   in your commit message. You can add those to the pull request description later.
   {{< /note >}}

Select Propose changes.

Select Create pull request.

The Open a pull request screen appears. Fill in the form:

The Add a title field of the pull request defaults to the commit summary.
     You can change it if needed.

The Add a description field contains your extended commit message, if you have one,
     and some template text. Add the
     details the template text asks for, then delete the extra template text.
Leave the Allow edits from maintainers checkbox selected.

{{< note >}}
   PR descriptions are a great way to help reviewers understand your change.
   For more information, see Opening a PR.
   {{</ note >}}

Select Create pull request.

Addressing feedback in GitHub
Before merging a pull request, Kubernetes community members review and
approve it. The k8s-ci-robot suggests reviewers based on the nearest
owner mentioned in the pages. If you have someone specific in mind,
leave a comment with their GitHub username in it.
If a reviewer asks you to make changes:

Go to the Files changed tab.
Select the pencil (edit) icon on any files changed by the pull request.
Make the changes requested.
Commit the changes.

If you are waiting on a reviewer, reach out once every 7 days. You can also post a message in the
#sig-docs Slack channel.
When your review is complete, a reviewer merges your PR and your changes go live a few minutes later.
Work from a local fork {#fork-the-repo}
If you're more experienced with git, or if your changes are larger than a few lines,
work from a local fork.
Make sure you have git installed
on your computer. You can also use a git UI application.
Figure 2 shows the steps to follow when you work from a local fork. The details for each step follow.

{{< mermaid >}}
flowchart LR
1[Fork the kubernetes/websiterepository] --> 2[Create local cloneand set upstream]
subgraph changes[Your changes]
direction TB
S[ ] -.-
3[Create a branchexample: my_new_branch] --> 3a[Make changes usingtext editor] --> 4["Preview your changeslocally using Hugo(localhost:1313)or build container image"]
end
subgraph changes2[Commit / Push]
direction TB
T[ ] -.-
5[Commit your changes] --> 6[Push commit toorigin/my_new_branch]
end
2 --> changes --> changes2
classDef grey fill:#dddddd,stroke:#ffffff,stroke-width:px,color:#000000, font-size:15px;
classDef white fill:#ffffff,stroke:#000,stroke-width:px,color:#000,font-weight:bold
classDef k8s fill:#326ce5,stroke:#fff,stroke-width:1px,color:#fff;
classDef spacewhite fill:#ffffff,stroke:#fff,stroke-width:0px,color:#000
class 1,2,3,3a,4,5,6 grey
class S,T spacewhite
class changes,changes2 white
{{</ mermaid >}}
Figure 2. Working from a local fork to make your changes.
Fork the kubernetes/website repository

Navigate to the kubernetes/website repository.
Select Fork.

Create a local clone and set the upstream

In a terminal window, clone your fork and update the Docsy Hugo theme:

shell
   git clone git@github.com:<github_username>/website
   cd website

Navigate to the new website directory. Set the kubernetes/website repository as the upstream remote:

```shell
   cd website
git remote add upstream https://github.com/kubernetes/website.git
   ```

Confirm your origin and upstream repositories:

shell
   git remote -v
Output is similar to:
none
   origin   git@github.com:<github_username>/website.git (fetch)
   origin   git@github.com:<github_username>/website.git (push)
   upstream https://github.com/kubernetes/website.git (fetch)
   upstream https://github.com/kubernetes/website.git (push)

Fetch commits from your fork's origin/main and kubernetes/website's upstream/main:

shell
   git fetch origin
   git fetch upstream
This makes sure your local repository is up to date before you start making changes.
{{< note >}}
   This workflow is different than the
   Kubernetes Community GitHub Workflow.
   You do not need to merge your local copy of main with upstream/main before pushing updates
   to your fork.
   {{< /note >}}
Create a branch

Decide which branch to base your work on:

For improvements to existing content, use upstream/main.

For new content about existing features, use upstream/main.
For localized content, use the localization's conventions. For more information, see
     localizing Kubernetes documentation.
For new features in an upcoming Kubernetes release, use the feature branch. For more
     information, see documenting for a release.
For long-running efforts that multiple SIG Docs contributors collaborate on,
     like content reorganization, use a specific feature branch created for that effort.

If you need help choosing a branch, ask in the #sig-docs Slack channel.

Create a new branch based on the branch identified in step 1. This example assumes the base
   branch is upstream/main:

shell
   git checkout -b <my_new_branch> upstream/main

Make your changes using a text editor.

At any time, use the git status command to see what files you've changed.
Commit your changes
When you are ready to submit a pull request, commit your changes.

In your local repository, check which files you need to commit:

shell
   git status
Output is similar to:
```none
   On branch 
   Your branch is up to date with 'origin/'.
Changes not staged for commit:
   (use "git add ..." to update what will be committed)
   (use "git checkout -- ..." to discard changes in working directory)
modified:   content/en/docs/contribute/new-content/contributing-content.md
no changes added to commit (use "git add" and/or "git commit -a")
   ```

Add the files listed under Changes not staged for commit to the commit:

shell
   git add <your_file_name>
Repeat this for each file.

After adding all the files, create a commit:

shell
   git commit -m "Your commit message"
{{< note >}}
   Do not use any GitHub Keywords
   in your commit message. You can add those to the pull request
   description later.
   {{< /note >}}

Push your local branch and its new commit to your remote fork:

shell
   git push origin <my_new_branch>
Preview your changes locally {#preview-locally}
It's a good idea to preview your changes locally before pushing them or
opening a pull request. The Previewing locally
article explains how you can run a website locally and preview
the suggested changes.
Open a pull request from your fork to kubernetes/website {#open-a-pr}
Figure 3 shows the steps to open a PR from your fork to the kubernetes/website. The details follow.
Please, note that contributors can mention kubernetes/website as k/website.

{{< mermaid >}}
flowchart LR
subgraph first[ ]
direction TB
1[1. Go to kubernetes/website repository] --> 2[2. Select New Pull Request]
2 --> 3[3. Select compare across forks]
3 --> 4[4. Select your fork fromhead repository drop-down menu]
end
subgraph second [ ]
direction TB
5[5. Select your branch fromthe compare drop-down menu] --> 6[6. Select Create Pull Request]
6 --> 7[7. Add a descriptionto your PR]
7 --> 8[8. Select Create pull request]
end
first --> second
classDef grey fill:#dddddd,stroke:#ffffff,stroke-width:px,color:#000000, font-size:15px;
classDef white fill:#ffffff,stroke:#000,stroke-width:px,color:#000,font-weight:bold
class 1,2,3,4,5,6,7,8 grey
class first,second white
{{</ mermaid >}}
Figure 3. Steps to open a PR from your fork to the kubernetes/website.

In a web browser, go to the kubernetes/website repository.
Select New Pull Request.
Select compare across forks.
From the head repository drop-down menu, select your fork.
From the compare drop-down menu, select your branch.
Select Create Pull Request.

Add a description for your pull request:

Title (50 characters or less): Summarize the intent of the change.

Description: Describe the change in more detail.

If there is a related GitHub issue, include Fixes #12345 or Closes #12345 in the
    description. GitHub's automation closes the mentioned issue after merging the PR if used.
    If there are other related PRs, link those as well.

If you want advice on something specific, include any questions you'd like reviewers to
    think about in your description.

Select the Create pull request button.

Congratulations! Your pull request is available in Pull requests.
After opening a PR, GitHub runs automated tests and tries to deploy a preview using
Netlify.

If the Netlify build fails, select Details for more information.
If the Netlify build succeeds, select Details opens a staged version of the Kubernetes
  website with your changes applied. This is how reviewers check your changes.

GitHub also automatically assigns labels to a PR, to help reviewers. You can add them too, if
needed. For more information, see Adding and removing issue labels.
Addressing feedback locally

After making your changes, amend your previous commit:

shell
   git commit -a --amend

-a: commits all changes

--amend: amends the previous commit, rather than creating a new one

Update your commit message if needed.

Use git push origin <my_new_branch> to push your changes and re-run the Netlify tests.

{{< note >}}
   If you use git commit -m instead of amending, you must squash your commits
   before merging.
   {{< /note >}}
Changes from reviewers
Sometimes reviewers commit to your pull request. Before making any other changes, fetch those commits.

Fetch commits from your remote fork and rebase your working branch:

shell
   git fetch origin
   git rebase origin/<your-branch-name>

After rebasing, force-push new changes to your fork:

shell
   git push --force-with-lease origin <your-branch-name>
Merge conflicts and rebasing
{{< note >}}
For more information, see Git Branching - Basic Branching and Merging,
Advanced Merging, or ask in the
#sig-docs Slack channel for help.
{{< /note >}}
If another contributor commits changes to the same file in another PR, it can create a merge
conflict. You must resolve all merge conflicts in your PR.

Update your fork and rebase your local branch:

shell
   git fetch origin
   git rebase origin/<your-branch-name>
Then force-push the changes to your fork:
shell
   git push --force-with-lease origin <your-branch-name>

Fetch changes from kubernetes/website's upstream/main and rebase your branch:

shell
   git fetch upstream
   git rebase upstream/main

Inspect the results of the rebase:

shell
   git status
This results in a number of files marked as conflicted.

Open each conflicted file and look for the conflict markers: >>>, <<<, and ===.
   Resolve the conflict and delete the conflict marker.

{{< note >}}
   For more information, see How conflicts are presented.
   {{< /note >}}

Add the files to the changeset:

shell
   git add <filename>

Continue the rebase:

shell
   git rebase --continue

Repeat steps 2 to 5 as needed.

After applying all commits, the git status command shows that the rebase is complete.

Force-push the branch to your fork:

shell
   git push --force-with-lease origin <your-branch-name>
The pull request no longer shows any conflicts.
Squashing commits
{{< note >}}
For more information, see Git Tools - Rewriting History,
or ask in the #sig-docs Slack channel for help.
{{< /note >}}
If your PR has multiple commits, you must squash them into a single commit before merging your PR.
You can check the number of commits on your PR's Commits tab or by running the git log
command locally.
{{< note >}}
This topic assumes vim as the command line text editor.
{{< /note >}}

Start an interactive rebase:

shell
   git rebase -i HEAD~<number_of_commits_in_branch>
Squashing commits is a form of rebasing. The -i switch tells git you want to rebase interactively.
   HEAD~<number_of_commits_in_branch indicates how many commits to look at for the rebase.
Output is similar to:
```none
   pick d875112ca Original commit
   pick 4fa167b80 Address feedback 1
   pick 7d54e15ee Address feedback 2
# Rebase 3d18sf680..7d54e15ee onto 3d183f680 (3 commands)
...
# These lines can be re-ordered; they are executed from top to bottom.
   ```
The first section of the output lists the commits in the rebase. The second section lists the
   options for each commit. Changing the word pick changes the status of the commit once the rebase
   is complete.
For the purposes of rebasing, focus on squash and pick.
{{< note >}}
   For more information, see Interactive Mode.
   {{< /note >}}

Start editing the file.

Change the original text:
none
   pick d875112ca Original commit
   pick 4fa167b80 Address feedback 1
   pick 7d54e15ee Address feedback 2
To:
none
   pick d875112ca Original commit
   squash 4fa167b80 Address feedback 1
   squash 7d54e15ee Address feedback 2
This squashes commits 4fa167b80 Address feedback 1 and 7d54e15ee Address feedback 2 into
   d875112ca Original commit, leaving only d875112ca Original commit as a part of the timeline.

Save and exit your file.

Push your squashed commit:

shell
   git push --force-with-lease origin <branch_name>
Contribute to other repos
The Kubernetes project contains 50+ repositories. Many of these
repositories contain documentation: user-facing help text, error messages, API references or code
comments.
If you see text you'd like to improve, use GitHub to search all repositories in the Kubernetes
organization. This can help you figure out where to submit your issue or PR.
Each repository has its own processes and procedures. Before you file an issue or submit a PR,
read that repository's README.md, CONTRIBUTING.md, and code-of-conduct.md, if they exist.
Most repositories use issue and PR templates. Have a look through some open issues and PRs to get
a feel for that team's processes. Make sure to fill out the templates with as much detail as
possible when you file issues or PRs.
{{% heading "whatsnext" %}}

Read Reviewing to learn more about the review process.