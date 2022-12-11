![Logo](https://savvycoders.com/wp-content/uploads/2018/08/SavvyCoders-Horizontal-Color-Logo_1@3x.png)

---

<br>

# _<p align="center"><u>Contributing to the SavvyCoder's Repositories</u></p>_

<br>

---

<br>

# <p align="center"><u>Forking a Repository</u></p>

<br>

1. On GitHub, navigate to the [`savvy-coders/sc-curriculum`](https://github.com/savvy-coders/sc-curriculum) repository.

<br>

2. In the top-right corner of the page, click `Fork`.

<br>

![Fork Button](https://docs.github.com/assets/cb-23088/images/help/repository/fork_button.png)

<br>

3. Select an owner for the forked repository which click dropdown and select your name to be the owner of your forked [`sc-curriculum`](https://github.com/savvy-coders/sc-curriculum) Repository. As shown below in this example image :

<br>

![Create a New Fork Page with Owner Dropdown Emphasized](https://docs.github.com/assets/cb-151543/images/help/repository/fork-choose-owner.png)

<br>

4. By default, forks are named the same as their upstream repositories. You can change the name of the fork to distinguish it further if you so choose to make it easier to differentiate between the [`savvy-coders/sc-curriculum`](https://github.com/savvy-coders/sc-curriculum) and your forked copy of the repository.

<br>

![Create a New Fork Page with Repository Name Field Emphasized](https://docs.github.com/assets/cb-151542/images/help/repository/fork-choose-repo-name.png)

<br>

_Optionally, you can add a description of your fork :_

<br>

![Create a New Fork Page with Description Field Emphasized](https://docs.github.com/assets/cb-177452/images/help/repository/fork-description.png)

<br>

# <p align="center"><u>Creating a Branch</u></p>

<br>

- After forking the repository, you will need to create a new branch to hold your changes. This is important because it allows you to isolate your changes from the main codebase, making it easier to review and merge your changes.

- To create a new branch, in your Terminal, you first got to change directory into your forked sc-curriculum repository e.g.
  `cd Code/SavvyCoders/your-forked-sc-curriculum-repository`

- Enter `git checkout -b ${"yourownbranchname"}`

<br>

**This will create a new branch with the specified name, and switch you to that branch.**

<br>

![Creating a Branch](https://paper-attachments.dropbox.com/s_E7E3F14C4905C4CE20AE3FDC33EFE78C3CAFED59288B605B89A9E40497700515_1657112003074_branch.gif)

---

_This point on, you will perform code fixes and updates on your forked repository which when done, we'll do a PR request on the issue you attempt to fix/update on the main[`sc-curriculum`](https://github.com/savvy-coders/sc-curriculum) Repository you forked from._

---

<br>

# <p align="center"><u>Creating a pull request from a fork</u></p>

You can create a pull request to propose changes you've made to a fork of an upstream repository.

Anyone with write access to a repository can create a pull request from a user-owned fork.

If your pull request compares your topic branch with a branch in the upstream repository as the base branch, then your topic branch is also called the compare branch of the pull request. For more information about pull request branches, including examples, see "[Creating a pull request](https://docs.github.com/en/articles/creating-a-pull-request/#changing-the-branch-range-and-destination-repository)."

### Note:

To open a pull request in a public repository, you must have write access to the head or the source branch or, for organization-owned repositories, you must be a member of the organization that owns the repository to open a pull request.

<br>

## Initiating a Pull Request

<br>

1.  Navigate to the original repository where you created your fork.

<br>

2.  Above the list of files, click the Pull request tab.

<br>

!["Pull request" link above list of files](https://docs.github.com/assets/cb-26570/images/help/pull_requests/pull-request-start-review-button.png)

<br>

3.  On the Compare page, click compare across forks.

<br>

![Compare across forks link](https://docs.github.com/assets/cb-10913/images/help/pull_requests/compare-across-forks-link.png)

<br>

4.  In the "base branch" drop-down menu, select the development branch which is the upstream [`sc-curriculum`](https://github.com/savvy-coders/sc-curriculum) repository you'd like to merge changes into.

<br>

![Drop-down menus for choosing the base fork and branch](https://docs.github.com/assets/cb-44606/images/help/pull_requests/choose-base-fork-and-branch.png)

<br>

5.  In the "head fork" drop-down menu, select your [`sc-curriculum`](https://github.com/savvy-coders/sc-curriculum) forked repository, then use the "compare branch" drop-down menu to select the branch you made your changes in e.g. `${yourdevelopmentbranchname}`.

<br>

![current working branch](https://i.imgur.com/1VS3gsW.png)

![Drop-down menus for choosing the head fork and compare branch](https://docs.github.com/assets/cb-43627/images/help/pull_requests/choose-head-fork-compare-branch.png)

<br>

6.  Type a title of of your issue you're fixing and description of what was changed for your pull request.

<br>

![Pull request title and description fields](https://docs.github.com/assets/cb-28826/images/help/pull_requests/pullrequest-description.png)

**You can specify which branch you'd like to merge your changes into when you create your pull request. Pull requests can only be opened between two branches that are different.**

### Note:

- To open a pull request in a public repository, you must have write access to the head or the source branch or, for organization-owned repositories, you must be a member of the organization that owns the repository to open a pull request.

<br>

_You can link a pull request to an issue to show that a fix is in progress and to automatically close the issue when someone merges the pull request. For more information, see_ ["Linking a pull request to an issue"](https://docs.github.com/en/issues/tracking-your-work-with-issues/linking-a-pull-request-to-an-issue).

<br>

# <p align="center"><u>Pull Request Editing Branches</u></p>

<br>

When thinking about branches, remember that the base branch is where changes should be applied _sc-curriculum development branch_, the head branch contains what you would like to be applied _which is the unique branch you created after you forked the sc-curriculum repository_.

When you change the base repository, you also change notifications for the pull request. Everyone that can push to the base repository will receive an email notification and see the new pull request in their dashboard the next time they sign in.

- When you change any of the information in the branch range, the Commit and Files changed preview areas will update to show your new range.

### Quick tip:

Using the compare view, you can set up comparisons across any timeframe. For more information, see [Comparing Commitments](https://docs.github.com/en/pull-requests/committing-changes-to-your-project/viewing-and-comparing-commits/comparing-commits)

<br>

# <p align="center"><u>Review and Merge</u></p>

Once you have created the pull request, the maintainers / Members of the SavvyCoders team, of the sc-curriculum will review your changes and either merge them into the main codebase or provide feedback on what needs to be changed. If the maintainers decide to merge your changes, they will merge the changes from your branch into the main codebase, and your changes will become part of the project.

<br>

# <p align="center"><u>Responding to Feedback</u></p>

If the maintainers of the [sc-curriculum](https://github.com/savvy-coders/sc-curriculum) have any questions or concerns about your changes, they may provide feedback in the pull request, and you can use the built-in editor on GitHub to make any necessary changes. Once you have addressed the feedback, you can update your pull request, and the maintainers will review it again.

<br>

# <p align="center"><u>Continuing to Contribute</u></p>

Once your changes have been merged, you can continue contributing to the [sc-curriculum](https://github.com/savvy-coders/sc-curriculum) by creating additional new branches and creating issues along with pull requests for any additional changes you would like to make.

<br>

---

<br>

### <p align="center">View the GitHub Documentation that was referenced</p>

<br>

### - [GitHub Documentation](https://docs.github.com/en/get-started/quickstart/contributing-to-projects)
