# Usage

## Installation

To install Git, follow along with [this installation tutorial](https://www.atlassian.com/git/tutorials/install-git). It's just a few steps. Please make sure not to skip the last step where you configure username and email -- these should match what you set up in GitHub.

```
$ git config --global user.name "username"
$ git config --global user.email "email"
```

To get started with the code, run

```
$ git clone https://github.com/IrvingtonRobotics/tower-takeover-A.git
```

then the directory should appear with the contents of the repository.

## Committing

After making some edits, you will need to commit and push changes. However, the Git-in-Atom user interface has considerably changed recently. I'm having trouble finding a good tutorial for it, but [this quick tutorial](https://youtu.be/mIj4p97K_VU?t=305) should do sufficiently, starting at 5:05. To open the side bar, move your mouse to the far right of the window or go to the top and click the actions `Packages > Github > Toggle Git Tab`. The tutorial works with HTML, but Git works just the same with C++. For your own good, press the <kbd>Fetch</kbd> and then <kbd>Pull</kbd> buttons if available before you start coding to apply the latest changes on Github.

## Proper Style

Unlike this tutorial, please don't name your commits like `"Commit 1"` -- this is undescriptive and useless. Follow proper [commit guidelines](https://chris.beams.io/posts/git-commit/) to make the messages useful. In addition, keep each commit [as small as possible](https://www.freshconsulting.com/atomic-commits/) to separate changes into different commits, but each commit should be a snapshot that is code that works and we could upload.

If you're planning to add several related changes that do not depend on each other, press <kbd>Master</kbd> then <kbd>New Branch</kbd> before committing to create a new branch. Once you're done with commits for a branch, go to the Github tab on the right (not the Git tab) and press the blue <kbd>Open new pull request</kbd> button. This should redirect you to a web browser where you can wrap up a pull request to bring code into `master`.

# Normal Workflow

About 90% of the time your work will proceed in the same way:

1. Download the current repository
2. Make some edits, bringing the code to an operational point
3. Commit the code to your local repository
4. Push the code to the GitHub repository

The way to do this in PROS/Atom and terminal/Git CMD is as follows:

1. Download

  - If you do not have the repository at all on your computer, you have to clone it from Github to your computer using `git clone <URL>` such as

          git clone https://github.com/IrvingtonRobotics/tower-takeover-A.git

  - If you already have the repository on your computer, run `git pull`

    - inside PROS/Atom, you could instead press the <kbd>Fetch</kbd> button. This checks if there were any changes on the remote Github repository. If there were any remote changes, you will have to then press <kbd>Pull</kbd> right after to bring in the changes into your local.

2. Edit

   - Make edits on files. You will mostly edit the `src/` directory. Be sure to test your code before proceeding: Each commit should have working code, so in case anything breaks, you just have to roll back.

3. Stage + Commit

   - Open the Git sidebar.

   - You need to stage the changed files before committing. Do this by pressing <kbd>Stage All</kbd> near the top-right. This moves all the files to the Staging Area below.

   - Write a commit message in the box. Remember to use the imperative mood and capitalize the first letter. Most importantly, the message should make it clear what the commit did.

   - Press the <kbd>Commit to Master</kbd> button.

4. Push to Github

   - At this point, the commit is just stored in the local repository on your computer.

   - Push it to github by pressing <kbd>Push</kbd>

# In Case of Confusion

Hopefully this brief introduction helped you get started with Git, Github, and Atom. If you are confused or have any questions:

1. Try it again

2. Google the problem

3. Feel free to reach out to me. I'll be glad to help

Good luck!
