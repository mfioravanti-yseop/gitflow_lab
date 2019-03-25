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


You can see that the history for the rebase is clearer! So that's the method we will use here.

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

### 2.6 Handle conflicts

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

```js
<<<<<<< develop
export function divide(a,b) {
	return a / b;
=======
export function multiply(a, b) {
	return a * b;
>>>>>>> feat: add multiply function
}
```

The part between the brackets and the equal signs is what was on the base branch. The second part, after the equal signs and before the last brackets, is your current modification. You need to choose if you want to:
* keep the previous version
* keep your modifications
* keep both
* do a mix of them

You can edit manually the file, or use a merge tool. I personally suggest you to use Visual Studio Code that will very facilitate your work, since you'll only have to click on the button above to choose what to do:

![Conflicts](/doc/images/conflict.PNG)

Here, you will choose *Accept both Changes*, but also modify the file after, because a '}' might be missing!

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

Here's what you are supposed to obtain:

![After Conflict](/doc/images/after_conflict.PNG)


#### What if I did wrong?

If can happen that you realize while doing the rebase that you have mistaken, or maybe you want to do this rebase later. In that case, you can abort the operation, and all the steps already completed will be cancelled!

Command Line | Gitkraken
------------ | -------------
```git rebase --abort``` | Click on the button *Abort Rebase*.


## Exercise 3: use the stash

Let's do a pause in complex features and learn an easy one :-) We will now talk about the **stash**.
It is a tool that allows you to put apart your modifications for a moment, so you can proceed to other operations. For example, it is not possible to do a rebase if you have current modifications not commited. 

There are three features:

Command Line | Result
------------ | -------------
```git stash``` or ```git stash save``` | Put apart your current modifications
```git stash pop``` | Put back your modifications in the project
```git stash clear``` | Cancel the modifications that were put apart


Let's now test that! 

NB: As for the previous exercise, this one is only for testing the feature, but in general, the modifications come from another developer ;-)


### Step 1: create a working branch

* create a new branch named *feature/display_substract*

### Step 2: change the current state of develop

* create a new branch *feature/substract* and switch to it
* modify the file *file3.js* and add a function *substract*
* commit and push your modifications on the repository
* merge your branch in *develop*

### Step 3: go back to your current working branch

* switch back to your branch *feature/display_substract*
* modify the file *main.js*: add in the main function a line that display the result of an operation *substract*, even if it's not defined in the current branch:

```js
console.log('result of the substraction:', substract(8, 5));
```

Don't commit!

You are now in this situation :

* you know there is a new function to substract numbers in *develop* (thanks to the commit name), and you'll like to use it. 
* You don't want to commit and push your work for now because it won't compile.

That's the right time to use the stash!

Command Line | Gitkraken
------------ | -------------
```git stash save``` | Click on the button **Stash** on the top of the window.

Now you can rebase your work on *develop*, so you'll get the function *substract*. This done, you can put back your modifications in the working tree:

Command Line | Gitkraken
------------ | -------------
```git stash pop``` | Click on the button **Pop** on the top of the window.


Now you can commit, push and merge your modifications in *develop*.

![After Stash](/doc/images/after_stash.PNG)


## Exercise 4: Rearrange commits

Sometimes, when you are doing a lot of features in the same branch, you commit your modifications incrementally, and the history becomes ugly! So It would be nice to rearrange it afterward.

NB: For this part, I will only show you with Gitkraken: the command line version is very tricky, and unless you are an Ops on a server without access to the UI, you won't need it.

### Step 1: Create one commit with a lot of code

You will first create a branch *feature/string_op*. In the file *file2.js*, you will write:
* a function to concat two strings: *myConcat(string,string)*
* a function that returns the size of a string: *mySize(string)*
* a function that returns the position of a given character in a string: *myPosition(string,char)*

Since this is not the purpose of the exercise, if you don't know how to do that, fill your function with wathever you want!
Then, put all of them in a single commit (the name is not important), and push it on the repository.

### Step 2: Go backward

Now, you've just realized that it would have been better to separate the functions in three different commits! That's what we are going to do with the instruction **reset**.

First, let me explain how this work. There are 3 options (actually, 5, but some are unused) that are very impactful on the result!

Command Line | Result
------------ | -------------
```git reset --soft``` | Reset your branch to a specified commit, but the modifications since this commit are still here
```git reset --mixed``` | Same, but the modifications are not marked for commit (meaning the **add** has already been done)
```git reset --hard``` | Really reset to this commit, so the modifications after that are lost! (CAUTION WITH THIS ONE!)

In practice, the first option does not really make sense, and the third is not what we are interested in here, because we don't want to loose our functions. So let's try the second one.

To do that, you will right-click on the last commit of *develop*, and select *reset feature/string_op to this commit*, and then *Mixed*.

![Reset Mixed](/doc/images/reset_mixed.PNG)

So now, you can see your branch is at the same level of *develop*, and your modifications are still here and ready for commit (unstaged files).

### Step 3: Commit individually each function

You can click on the file *file2.js* on the right part, and you'll see all your modifications on the left. You now need to select only the line that you are interested in, and stage them. So first, take the first function.

![Stage Selected Lines](/doc/images/stage_lines.PNG)

Your file, on the right, will now be split in two:
* if you click on *file2.js* on the bottom part, you will see what you have just staged
* if you click on *file2.js* on the top part, you will see what is still not staged

So you can now create a commit named *feat: function to concat strings*.

That's it! Now do the same thing with the two other functions.

### Step 4: Force push

When you have rewritten history and the previous version has been pushed to the remote, Git won't allow you to push your modifications the traditional way. That's what the mode **push force** is aimed for.
I strongly recommend you to be very careful with this one!!

In Gitkraken, you have to use the button push as usual, but Git will detect a problem and suggest you two options:

![Force Push](/doc/images/force_push.PNG)

The first one is to be used when someone else has worked on the same branch, and you want to keep his modifications. That's actually the most common case, but here, we know that the only data we are going to ovewrite is our bad commits! So you can use the option *force push*.
Gitkraken will ask you again if you're sure, because, I insist, it's a dangerous feature because it can erase some code!

After that, your three commits will be neat, and you can merge all that in *develop*.

Note: *develop* and *master* are protected against the option *force push* in all our repositories! So you can only do that on your branches.


## Exercise 5: Interactive Rebase \[Advanced\]

This part is a hard one, especially because it is not possible with Gitkraken (but it is with GitExtension). But this feature is so nice to use! If you don't feel at ease, you can skip this part, but since you are in a sandbox, I suggest you to try anyway :-)

The previous exercise was really useful because we wanted to rewrite everything. But what if we have already made some commits right, and we don't want to touch them? You can do that with the **interactive rebase**.

### Step 1: preparation

For this exercise, we will create a branch named feature/misc and modify *file1.js* as follows:

* Create a function *mySecondFunction* that displays a text of your choice, and commit with *feat: my second function*
* Modify the message in the function and commit with: *feat: change the second function*
* Create a function *myThirdFunction* that displays another message, and commit with: *WIP*
* Create a function *myFourthFunction* that displays another message, and commit with: *feat: my fourth function*

![Before Interactive Rebase](/doc/images/before_interactive_rebase.PNG)

### Step 2: interactive rebase

So now, where are we? Here are the changes we should do on this history:
* The second commit is not useful, we would like to merge it into the previous one
* The commit message *WIP* is ugly, we would like to rename it

Let's open a console in the git folder, and lauch the following instruction:

```
git rebase -i develop
```

A file will open, with all the commits that have been added after develop (if you are on Linux, that will be directly in the terminal).

![Start Interactive Rebase](/doc/images/interactive_rebase_start.PNG)

Here's what we will do:

* We won't do nothing on the first commit
* The second one needs to be merged in the first one, so we will use the option **fixup** (f)
* The third one needs to be renamed, so use **reword** (r). Don't change the commit message here, you'll be ask for the it later!
* Leave the fourth commit untouched

We should obtain:

![End Interactive Rebase](/doc/images/interactive_rebase_end.PNG)

Now just save the file and close it. The rebase will proceed, every step being logged in the terminal. When the operation needs information for you (like the commit message), it will ask you. You just have to do your modification, save, and close the file.

![Reword Interactive Rebase](/doc/images/interactive_rebase_commit.PNG)

When it's over, you should have obtained that:

![Success Interactive Rebase](/doc/images/interactive_rebase_success.PNG)

As you can see in Gitkraken, your history is not very nice:

![Result Interactive Rebase](/doc/images/interactive_rebase_result.PNG)

You can now push and merge it, as usual.

NB: You could also have used **squash** instead of **fixup** to keep the old commit message in the description, but here, we didn't want to do that. Remind this operation anyway, because it is very common when you are doing PRs on Github.

NB2: You could also reorder the commits simply by inverting two lines in the file. Obviously, if the change is not possible, the abort will fail!

### Precautions

When you are doing tricky operations like this, I suggest you create a tag at the position *before* rewriting. Like this, if you decided to abort the operation, or if you have made some mistakes, you'll just have to do a *reset --hard* on the tag and restore your branch to its previous state.
Please note also that you can abort the interactive rebase, just as for the standard rebase, with the instruction:

```
git rebase --abort
```

For further readings on the subject of rewriting history, here is [a very useful link](https://git-scm.com/book/fr/v2/Utilitaires-Git-R%C3%A9%C3%A9crire-l%E2%80%99historique)

## Exercise 6: oops! \[Advanced\]

Sometimes, it happens: you overwrite your work or someone else's by doing a destructive operation. The good news is that with Git, nothing is ever really lost. We are going to see how to do that.

My source here is this [amazing page](https://git-scm.com/book/fr/v2/Les-tripes-de-Git-Maintenance-et-r%C3%A9cup%C3%A9ration-de-donn%C3%A9es) that explains very well the different solutions.

### Step 1: overwrite the data

You will first create a branch named *feature/oops*, in which you will create a file *blibli.txt* at the root of the project, write a text inside, commit (*feat: blibli*) and push it.

Just for the exercise (because it is so unnatural!), you will do a **reset --hard** of your branch on *develop*. 

Now, you create a file *blublu.txt*, write a text inside, commit (*feat: blublu*) and **force push it**. 

### Step 2: recover data

Oops, so we have overwritten our commit *blibli*, and the file *blibli.txt* has been erased! Don't panic, we will save it, because Git keeps everything, and you just have to find it back! 
To do that, you will open a terminal and type:

```
git reflog
```

You will obtain a list of all the operations that have been realized on your repo:

![Reflog](/doc/images/reflog.PNG)

We can see here the commit *feat: blibli*, that's the one we wan to save. To do that, we will create a recover branch on this specific commit, by using its identifier.

```
git tag recover f65fa2d
```

NB: you can also do that with a branch, but I find it easier with a tag.

You can now see in Gitkraken that the commit has been retrieved and tagged:

![Recover](/doc/images/recover.PNG)

So now it is very easy, you just have to do a cherry pick on this commit!

![Recover Done](/doc/images/recover_done.PNG)

Now you can push your branch to the remote, and merge it in develop!


## Exercise 7: clean up

We arrive at the end of our lab: the clean up! Now that we have done all those operations, we have a lot of branches and tags that are not necessary anymore, so we will remove them (except *develop* and *master*).

Note that those operations should be done along the way, and not in the end! That is just for the lab that we waited ;-)

### Remove tags

To remove the tag *recovery*:

Command Line | Gitkraken
------------ | -------------
```git tag -d recover && git push --delete origin recover``` | Right click on the tag and select *delete recover locally*. You'll be asked if you want to remove also them from the remotes.

NB: for the tag *recover*, you don't have to remove it remotely, since it has not been pushed. Indeed, it needs a special command to push a tag to the remote. For the tags created with Gitraken, however, you will need to remove them remotely, because Gitkraken automatically push on all remotes the tags you create!

### Remove branches

To remove a branch locally:

Command Line | Gitkraken
------------ | -------------
```git branch -d feature/oops``` | Right click on the branch *feature/oops* and select *delete feature/oops*. You'll be asked if you want to remove also them from the remotes.

To remove a branch remotely:

Command Line | Gitkraken
------------ | -------------
```git push <remote_name> --delete feature/oops``` | Right click on the branch *feature/oops* and select *delete <remote_name>feature/oops*.

Do those operations with all the branches we have created in this exercise!

### Update the branches state

Sometimes, mostly when you have used PRs to integrate and remove the distant branches, you would like to inform your local repo that those branches don't exist anymore. Here is the command to use:

```
git remote prune <your_repo>
```


## That's it!

This lab is finished! On a day-to-day basis, I suggest you use [this very useful documentation](https://eborel-yseop.github.io/git-guide/). You can also find a Git cheat sheet at the end of this documentation.

Thanks for your time, and don't hesitate to make some feedbacks to improve this lab!

## Bibliography

Here is a list of the sources quoted in this lab:

Gitflow:
https://datasift.github.io/gitflow/IntroducingGitFlow.html

Rewriting history:
https://git-scm.com/book/fr/v2/Utilitaires-Git-R%C3%A9%C3%A9crire-l%E2%80%99historique)

Save lost data:
https://git-scm.com/book/fr/v2/Les-tripes-de-Git-Maintenance-et-r%C3%A9cup%C3%A9ration-de-donn%C3%A9es


And other useful links:

Understand the Fast Forward:
https://confluence.atlassian.com/bitbucket/git-fast-forwards-and-branch-management-329977726.html

Git Ignore:
https://fr.atlassian.com/git/tutorials/saving-changes/gitignore


Gitflow:
https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow

