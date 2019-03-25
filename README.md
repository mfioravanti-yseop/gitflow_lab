# Git Lab for Yseop Users

## Introduction

This workshop is for Yseopers that would like to refresh their knowledge about Git and understand the functioning of GitFlow. It is progressive, so for an advance Git user, the first parts can be processed very fast to focus on the harder ones. Each one at his own pace :-)

It is based on [a very fancy documentation](https://eborel-yseop.github.io/git-guide/) written by Etienne Borel, and adapted to be a practical exercise.

There is also a [version](https://mfioravanti-yseop.github.io/git_lab/) for stand-alone users that have just arrived in the company, or want to improve their skills, and want to do an auto-training. It is more centered on Git, though, and doesn't deal  much with GitFlow (Pull Requests can't be handled alone!).


## Prerequistes

This lab won't explain to you what is Git, it takes for granted that you have an overall knowing of the subject! Also, you OS does not matter, but you should have installed the following items:

- **Git**, obviously! And having a personal account on Github, related to the workspace Yseop (generally: *yourlogin*-yseop)
- **Visual Studio Code** (recommended, not mandatory)
- a graphical Git Client, like **GitExtension** or **Gitkraken** (recommended, not mandatory)

Almost all the instructions will be provided for both the command line and a Git Client, feel free to use whatever you want!

## Let's Get Started!

When you want to start working on a project versioned with Git, there are two ways to proceed:
* either you **clone** an existing project, meaning you create a copy on your computer to work on it
* or you create a project from scrach on your computer, and you **init** it, so you could later save it on Git

Here, we will see how to clone, which is the most common use. The first thing is to get the Git address where to find the project. You can retrieve it on Github, as you can see on the following image:

![Get Repository Address](/doc/images/clone_address.PNG)

Once you copied it: 

Command Line | Gitkraken
------------ | -------------
```git clone git@github.com:mfioravanti-yseop/gitflow_workshop.git``` | Click on *Open a Project*, *Clone*, and then: ![Clone](/doc/images/clone.PNG)


You can now open the project in Visual Studio Code. You will see that it's a very simple knowledge base.


## An overall about Gitflow

At Yseop, we use the workflow named **Gitflow** to manage our repositories. You can find more information [here](https://datasift.github.io/gitflow/IntroducingGitFlow.html).


Let's have a look on this working tree as an example:

![Gitflow](/doc/images/gitflow.PNG)

As you can see here:
* For each feature, a branch **feature/** is created from *develop*, and after being completed (and validated by a PR), is merged into *develop*.
* To release a version, a branch **release/** is created from *develop*, and then integrated to *master*. If wanted, a tag with the version number is created. Then *master* in merged into *develop*.
* The branch master represents then the last released version at each moment. If we want to fix a bug, you can create a branch **hotfix/** from *master*, fix the bug, and then merge it back to *master*. A new tag is created with a new version number, and *master* is merged again in *develop*.
* There are other branch types like **refactor/**, **bugfix/**, but please respect the [conventions](https://www.conventionalcommits.org/en/v1.0.0-beta.3/) and don't invent your own!


## Exercise 1: Complete feature integration without conflict

Let's imagine a very easy and idealistic case, where you are working alone on a project, or the *develop* branch does not change while you are doing your feature. That will be our first use case, and we will do it together.

In this exercise, you will complete all the following tasks:

* create a branch
* make modifications
* see the list of your changes
* do commits
* push to the remote
* integrate to *develop*

For this first exercise, we don't use Pull Requests, because we really want to apprehend the deep functioning of Git first. That's an important step to understand what Github is really doing in the background.


### Create a Branch

If you are in command line, you need to do this step first:

```
git checkout develop
```

Now, let's start by creating a branch named **feature/client_desc** and switch to it. 


Command Line | Gitkraken
------------ | -------------
```git checkout -b feature/client_desc``` | Right-click on *develop*, select *Create branch here*, and type *feature/client_desc*.



You will now go to the project folder, in the file *src/_texts/ClientInformations.ytextfunction*, and add some code in the xml tag ```<yseop-yml:text>```.


### See the State

Once this is done, you can see your current modifications like that:

Command Line | Gitkraken
------------ | -------------
```git status``` | ![See changes](/doc/images/see_changes.PNG)


### Create a Commit

The next step is to create a **commit**. This means you have to list the modifications you want to add to the project. 

In command line:
```
git add src/_texts/ClientInformations.ytextfunction
```
or to put all the modified files:
```
git add .
```
You have to name well the commit, so in the history, it will be easy for the others developers to understand in what the modifications consist.
```
git commit -m "feat: change the client description" 
```

In Gitkraken, you have the user interface to help you! In the changes section (on thr right), you can *Stage* modifications, meaning include them in the commit. After that, you can set a name to the commit. 

![Commit](/doc/images/commit.PNG)

NB: beware to respect the commit name convention!!

### Push to the Repository

After those operations, your modifications are still only on your computer! You will now need to put them on the remote repository. To do so:

Command Line | Gitkraken
------------ | -------------
```git push feature/client_desc <remote_name>``` | Click on *Push* on the top.

### Integrate to *develop*

As this Lab is really centered on Git, we won't use Pull Requests for integration. At Yseop, we always use PRs and integrate them with Github, so obviously you will do all the following steps via Github. But this lab will help you understand what's really going on in the background!

To integrate your work, you need to merge it in *develop*. To do so, you will first set *develop* as your current branch:

Command Line | Gitkraken
------------ | -------------
```git checkout develop``` | Right click on the branch *develop*, and click "Checkout <remote_name>/develop".


Now, you need to get the current state of *develop* on the remote repository.

Command Line | Gitkraken
------------ | -------------
```git pull develop``` | On top of the screen, click on *Pull*.

Now it's time to properly integrate! The principle is that you will indicate a branch to integrate in your current working branch, so here:

Command Line | Gitkraken
------------ | -------------
```git merge feature/client_desc``` | Right-click on *feature/client_desc*, and click on *Merge into develop*: ![Merge](/doc/images/merge_into.PNG)

Note that you can choose the option **fast-forward**: if you do so, the history will be a little different: 

With Fast-forward | Without Fast-forward
------------ | -------------
![Merge with Fast Forward](/doc/images/merge_with_ff.PNG) | ![Merge without Fast-Forward](/doc/images/merge_without_ff.PNG)


And now finally, you need to push *develop* on the remote repository:

Command Line | Gitkraken
------------ | -------------
```git push <remote_name> develop``` | Click on the button *Push* on the top.


NB: Note that with Gitkraken, the all *merge* operation can be made very easily: you just have to drag and drop your branch on *develop*, and choose *merge in develop*!

## Exercise 2: Integrate through a Pull Request

This time, we will reproduce the behaviour we are really supposed to have with *Gitflow*.

### Feature Creation

The first steps are identical to the previous exercise's, so you can:
* create a branch feature: *feature/complete_client_desc*
* modify the text of *src/_texts/ClientInformations.ytextfunction* to add more details
* create a commit
* push to the repository

Just so you know, Git provides a tool to help you with the git flow features. You can find all the informations [here](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow).

### Create a Pull Request

Now, you have to create a Pull Request on Github. When, you arrive on the main page of your project, you will see this header:

![Start PR](/doc/images/pr_start.PNG)

You can click on the button *Compare and Pull Request*, and find a wizard for the PR creation:


![Create PR](/doc/images/pr_create.PNG)

The important informations here are:

* **base**: the branch in which you want to merge your feature. It's most of the time *develop*, unless you want to merge it into another feature branch! But it can never be *master*!!
* **compare**: your current feature branch
* the title: must have a very explicit name, and start with a selector that follow the commit name convention (*feat*, *fix*, *chore*, etc.).
* the description: you can here put informations that can help the reviewers understand what is in your feature, or to know how to use or test the feature.
* **reviewers**: you can set a list of persons that will be asked to put a review, so they will receive a notification and "follow" the PR. Note that a user can do a review even if not explicitely asked, as long as he does have the rights to.
* **assignees**: you should assign here yourself and the other contributors to the PR. They will follow the subject too.
* **labels**: if they exist in your project, they are informations about the current state of the PR. For example, a label *ready for review*, or *on hold* can be useful.
* **projects** and **milestones**: once again, it depends on your project configuration. If for example, you know you are working on a POC and also a v1 version, you can create projects with those names, and know easily in which project a feature will be added.

Once you have filled everything that is interesting for your PR, you can click on *Create pull request*.

### Comment a Pull Request

Once the Pull Request has been created, it's up to the reviewer! Reviewers can be developers, testers, or practically anyone that can give a point of view on the feature!

A review consist in reading the code, adding some comments, asking questions, suggesting changes, ...  The reviewer also has to test the feature. It is obviously not prohibited to discuss it out of PR if the PR owner is physically near to you ;-) There can be several reviewers, and they can agree to split the tasks (one reading code, one testing, for eg.).

Reviewing the code, the reviewer can go in the tab *Files changed* and add comments, by clicking the little "+" on the left of a code line. After commenting, he can choose to continue the review (the comment will stay in *pending* state) and add more comments, or finish the review.

![Comment code PR](/doc/images/pr_comment.PNG)

When all the code reviewing and testing tasks are completed, the reviewer has 3 options:
* **Comment**: the reviewer doesn't feel ready for taking his decision, he just want to ask questions.
* **Approve**: the reviewer is ok with the feature, and give its approval.
* **Request Changes**: the reviewer decides that something must be fixed or changed before merging the feature. He previously detailed it in its comments in the code, or add a comment to his review to explain exactly which changes he expects.

![Request changes PR](/doc/images/pr_request_changes.PNG)

The owner of the PR will then be notified of the reviewer's decision. If changes are requested, he will have to fix it, and push another commit. And so on, until he finally receive an approbation!


### Merge it

After receiving the approbation, the owner is now ready to merge the Pull Request! Or, almost: he must unsure that all the checks have passed. For example, the branch can be not up-to-date with the base branch if it has changed in the meantime. In this case, the owner must proceed, in local, to a **merge** or a **rebase**. Note hat a rebase is cleaner, but can sometimes cancel the reviews (if a *force push* was necessary)! You will then have to ask for the reviewers to accept it again.

When you are finally ready, let's merge it! It is very easy, you just have to click on the button **Merge Pull Request**. Note that if you prefer not keeping the history of all the commits, you can click on the arrow on the right on the button, and choose **Squash and merge**: all your commits will then be gathered in a single one, and the comment will list the commits before squashing. There is also a third option, **Rebase and merge**. This one must be used very carefully, because the new code in the base branch can interact with yours, even though Github didn't detect conflicts.

![PR ready for merge](/doc/images/pr_approved.PNG)

Please unsure that the message of the squashing commit also respect the naming convention!

After merging, Github confirm you that the merge was successful and that you can delete the branch safely. You should really do it, because the feature is over, and the branch is now useless!

![PR merged](/doc/images/pr_merged.PNG)

Then, on your local computer, you can go in Git and process to a pull/fetch: you will see that the base branch (here, *develop*) now contains a new commit! You can also delete the feature branch locally.

![after PR](/doc/images/after_pr.PNG)

NB: there is a feature **git flow** in the last versions of Git that allows you to handle features easily in command line. Feel free to use it or not! You can find a full documentation [here](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow).


## Exercise 3: Create features in parallel

Generally, there are several developers doing features on a project, and we will test that in this exercise! The first three steps will repeat the same steps as for *Exercise 1*, everytime creating your branch from *develop*, and ***not doing yet the integration of the feature in develop***.

### User 1: Add a feature

Create a branch *feature/delivery*, and in the file *src/_texts/DeliveryProcess.ytextfunction*, modify the content of the text.

### User 2: add another independent feature

Create a branch named *feature/product_desc*, and in the file *src/_texts/ProductDescription.ytextfunction*, change the content of the text to describe the product.

### User 3: modify the same file as another feature

You'll create a third feature, but this time, we will willingly provoke a conflict! So create a branch *feature/related_products*, still on *develop*, and modify again *src/_texts/ProductDescription.ytextfunction*. Don't touch the current text, but add just after a list of related products.


### User 1: integrate the first feature

This part is the easy one, and you could integrate *feature/delivery* in *develop* via a PR, exactly how you did in Exercise 2.


### User 2: integrate a feature after the base branch has changed

You can create a Pull Request for the branch *feature/product_desc*.

The branch *feature/product_desc* has no conflict with the base branch *develop*. But at this state, it is based on an old version of *develop*. Github will indeed tell you that your branch is not up-to-date. So we have two ways to proceed.

* either you do directly a **merge**: this means you reunite your branch with the actual state of *develop*
* or you do a **rebase** first: this means you take the actual state of *develop* and put your work after that

Even without doing the rebase, the result is exactly the same in the project's sources! The difference is in the history of Git. Here, you can see by yourself:

Directly merge | Rebase first
------------ | -------------
![Merge](/doc/images/merge.PNG) | ![Rebase](/doc/images/rebase.PNG)


You can see that the history for the rebase is clearer! So that's the method we will use here. We can also do a fast-forward, as indicated in the previous exercise.

First, you will checkout the branck *feature/product_desc*.

Command Line | Gitkraken
------------ | -------------
```git checkout feature/product_desc``` | Right-click on the branch *feature/product_desc*, choose *Checkout feature/product_desc*.

Then, you will get the last version of *develop* from the remote:

Command Line | Gitkraken
------------ | -------------
```git pull develop``` | On top of the screen, click on *Pull*.

Now, you will rebase your current branch on the current state of *develop*:

Command Line | Gitkraken
------------ | -------------
```git rebase develop``` | Right-click on *develop*, and choose *Rebase on develop*: ![Rebase](/doc/images/rebase_on.PNG)

NB: Note that with Gitkraken, the all *rebase* operation can be made very easily: you just have to drag and drop your branch on *develop*, and choose *rebase on develop*!

After doing that, you can continue the PR process.

### Handle conflicts

We are now going to address the harder part of Git: conflicts! It will occur very often that your work will enter in conflict with the work of someone else. Sadly, the first arrived wins, and the other one has to handle the conflicts. You can sometimes negociate with the first commiter to do the painful merge together, but in case you are alone, the first rule is: don't panic!

Like for the previous part, you will checkout the branch (*feature/related_products*), and get the last version of *develop* from the remote. Now, you'll launch an operation **rebase**, and this time, it won't go well!

#### General Behaviour

You will have to do the following steps, that will be detailed for both the command line and Gitkraken:

* manually fix the conflicts
* add the modified files to the commit
* continue rebase

Every commit being applied can provoke its own conflicts. So you will have to repeat those three operations since they have all been applied. Here in our case, that will not be long, since there is only one commit!


#### Know the files in conflict

The files in conflict are what they call the *Unmerged paths*:

Command Line | Gitkraken
------------ | -------------
```git status``` | Click on the top-right corner on *View Conflicts*: ![Conflicts](/doc/images/view_conflicts.PNG)

Once you have the list, you can try and fix them.

#### Fix the conflicts

Conflicts look like that in the conflicted file:

```xml
<<<<<<< develop
<yseop-yml:text>The product is very fancy, shiny, and modern. You will love it!</yseop-yml:text>
=======
<yseop-yml:text>This is the product description. If you like it, you can also buy the Shiny Thing or the Bright Stuff.</yseop-yml:text>
>>>>>>> feat: add information about the related products
```

The part between the brackets and the equal signs is what was on the base branch. The second part, after the equal signs and before the last brackets, is your current modification. You need to choose if you want to:
* keep the previous version
* keep your modifications
* keep both
* do a mix of them

You can edit manually the file, or use a merge tool. I personally suggest you to use Visual Studio Code that will very facilitate your work, since you'll only have to click on the button above to choose what to do:

![Conflicts](/doc/images/conflict.PNG)

In this situation, the best way will be to copy/paste the new test of *coming change* in the *current change*, and then *accept the current change*.

#### Add the files

After you fixed all conflicts for the current commit, you will add the files to the commit:


Command Line | Gitkraken
------------ | -------------
```git add .``` | On each file on the right, click on *Mark resolved*, or do all in once with *Mark all resolved*.

#### Continue the rebase


Command Line | Gitkraken
------------ | -------------
```git rebase --continue``` | Click on the button *Continue Rebase*


#### Then what?

If there is no more commits and/or conflicts, the rebase will proceed to the end. After that, you can safely merge your branch, as we did in the previous parts!


#### What if I did wrong?

If can happen that you realize while doing the rebase that you have mistaken, or maybe you want to do this rebase later. In that case, you can abort the operation, and all the steps already completed will be cancelled!

Command Line | Gitkraken
------------ | -------------
```git rebase --abort``` | Click on the button *Abort Rebase*.


## Exercise 4: use the stash

Let's do a pause in complex features and learn an easy one :-) We will now talk about the **stash**.
It is a tool that allows you to put apart your modifications for a moment, so you can proceed to other operations. For example, it is not possible to do a rebase if you have current modifications not commited. 

There are three features:

Command Line | Result
------------ | -------------
```git stash``` or ```git stash save``` | Put apart your current modifications
```git stash pop``` | Put back your modifications in the project
```git stash clear``` | Cancel the modifications that were put apart


Let's now test that! 


### User 1: create a working branch

* create a new branch named *feature/signature*

### User 2: change the current state of develop

* create a new branch *feature/brief_company* and switch to it
* add the file *src/_texts/BriefCompany.ytextfunction*, that describes the vendor company
* commit and push your modifications on the repository
* merge your branch in *develop*
(the use of PR is not necessary in this exercise)

### User 1: use the new feature of develop

* open the file *src/_texts/finalDocument.ytextfunction*, and call the text function \BriefCompany.

Don't commit!

You are now in this situation :

* you know there is a new textfunction that do a brief of the company in *develop* (thanks to the commit name), and you'll like to use it. 
* You don't want to commit and push your work for now because it won't compile.

That's the right time to use the stash!

Command Line | Gitkraken
------------ | -------------
```git stash save``` | Click on the button **Stash** on the top of the window.

Now you can rebase your work on *develop*, so you'll get the textfunction *BriefCompany*. This done, you can put back your modifications in the working tree:

Command Line | Gitkraken
------------ | -------------
```git stash pop``` | Click on the button **Pop** on the top of the window.


Now you can commit, push and merge your modifications in *develop*.



## That's it!

This workshop is finished! You can find a lot more of advanced usages [here](https://mfioravanti-yseop.github.io/git_lab/), like changing history or recovering lost data.

Don't hesitate too to dig in the subject with the [GitFlow Feature Documentation](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow).

On a day-to-day basis, I suggest you use [this very useful documentation](https://eborel-yseop.github.io/git-guide/). You can also find a Git cheat sheet at the end of this documentation.

Thanks for your time, and don't hesitate to make some feedbacks to improve this workshop!


