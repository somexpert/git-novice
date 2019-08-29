---
title: Collaborating
teaching: 45
exercises: 0
questions:
- "How can I use version control to collaborate with other people?"
objectives:
- "Fork an existing repository."
- "Clone a remote repository."
- "Collaborate pushing to a common repository."
- "Merge in changes from an upstream repository."
keypoints:
- "To work on someone else's code, you will typically need to fork their repository."
- "Collaboration on GitHub is driven by Pull Requests."
- "`git clone` copies a remote repository to create a local repository with a remote called `origin` automatically set up."
---

## Finding Repositories on GitHub

Now, we will see how GitHub can be used as a powerful collaboration tool. To begin, we need to navigate to the countries_201909 repository the instructor created during the set up for this lesson. In the search box on the top left, search for the name of the repository.

GitHub allows users to search on different levels. If you are already it in a repository, it will automatically search within that repo. As an indicator, "this repository" will appear in the search bar. Since we want to search all GitHub repositories, click the GitHub icon to reset the search settings.


![Searching all repos](../fig/01-fig_01.png)

![Searching within a specific repo](../fig/01-fig_02.png)


Once we have located the repository, clicking the title will take us to the repo. There is one folder and a README.md file within the directory. We can explore the files within the GitHub interface. Click on countries_201909 and then any of the markdown(.md) files.


>## README.md
>Remember that it is best practice to always have a README.md file within your repository. It should be descriptive so others (or you in a few months) will understand the purpose/structure of your code.
{: .callout}

Each of these files contains spaces for information about the population, capital, offical language, and interesting trivia for a specific country. Our goal today is to update a few of the files with information about the designated country.

Unlike our earlier experience with GitHub, we do not own this repository. We can quickly identify the owner by looking at the repostiory name located directly beneath the search bar. The first part of the name is the GitHub user who owns the repository followed by a slash and the repository's name. The owner could be an individual or organizaiton.

![The owner of this repo is oulib-swc](../fig/01-fig_03.png)

## Forking a Project on GitHub

Since we do not own this project, we don't want to make changes directly to this code. Instead, we should make a copy of the code, make our changes, and submit it to the owner. If the owner likes our changes, they can then incorporate it into the original repo.

In GitHub, making a copy of a repository is called forking. In the top right corner of the page, under your profile picture, there are three buttons. Click on Fork to create a forked copy of the repo. The number beside Fork indicates the number of times the original repo has been forked.

![Fork the Repo](../fig/01-fig_04.png)

After you fork the repo, GitHub will automatically redirect you to your forked verison. The forked repository is identical to the original repo. The forked verison includes all of the commits and contributors as the original.

>## Selecting where to fork
> If you are a member of organizations on GitHub, you will need to select where you want to fork the repository. For this exercise, please select your personal GitHub account.
{: .callout}

If we look at the title, we can see that the owner has changed. Now, our account name is listed before the name of the repository. There is also a link to the original repo.

![The Forked Repo](../fig/01-fig_05.png)

## Cloning your fork

Now that we have our own fork, we are ready to start adding information. When collaborating on projects in GitHub, it is
a good idea to plan with the group who will work on what.

Everyone should choose a country you want to work on, and add your name and the country to the etherpad. Try to get a
country that no one else is working on.

First, we'll clone our fork so that we can modify it.

~~~
$ cd ~/Desktop
$ git clone https://github.com/YOUR-ACCOUNT/countries_201909.git
$ cd countries_201909
~~~
{: .bash}

## Working Locally On a Branch

Let's check the status of our fork.

~~~
$ git status
~~~
{: .bash}
~~~

On branch master
Your branch is up-to-date with 'origin/master'.
nothing to commit, working directory clean
~~~
{: .output}

Notice it says we are "On branch master". Whenever you are working on a git
repo, you are working on a branch. Unless you or someone else changes it, by
default, you are working on a branch called "master". Using branches allows us
to do things like work on new additions/features for our project that we aren't
sure we want to keep yet, or maybe it is a change we want to propose as an addition
to someone else's project.

There is another command we can use to check what branch we are on.
~~~
$ git branch
~~~
{: .bash}

~~~
* master
~~~
{: .output}

You can also create new branches with this command and call them whatever you
like--though it may help you and your collaborators if it is a relevant name.
Let's create a branch called `update-country`.

~~~
$ git branch update-country
~~~
{: .bash}

This created a branch, but we are still on `master` branch. We use `git checkout`
to change to working on our new branch.

~~~
$ git checkout update-country
~~~
{: .bash}

~~~
Switched to branch 'update-country'
~~~
{: .output}

Now if you use `git branch` you see a list of your branches and which is active.
~~~
$ git branch
~~~
{: .bash}

~~~
  master
* update-country
~~~
{: .output}

In your locally cloned repository, edit the file in the `countries` folder to make your changes to the country you claimed.

~~~
$ cd countries
$ nano Spain.md
~~~
{: .bash}

Take a minute to look up a few facts about the country and enter them in the file.

~~~
## Spain
## population
46,733,038


## capital
Madrid


## official language
Spanish


## interesting trivia
The quill pen was invented in Spain, and the first novel was written there as well.
~~~

Once you're happy with your changes, commit and push them to your forked repository as normal:

~~~
$ git add Spain.md
$ git commit -m "Updating facts about Spain."
$ git push origin update-country
~~~
{: .bash}


## Sharing Your Work on GitHub

With your new changes pushed to GitHub, you can use the GitHub web interface to create a Pull
Request to share your changes with owners of the original repository. In your web browser, go
to your account's fork of the countries_201909 repo. If you already have it open, refresh the page.
When you do this you will see a yellow bar with a green `Compare and pull request` button.
Let's click the button, fill in some helpful information about the changes we're proposing, and
submit the pull request. Normally, a Pull Request just merges in changes from one branch of your
repository into another branch (normally the master branch) of the same repository, but since
GitHub recognized that this is a fork of someone else's repository, it by default will create the 
Pull Request in a way that will merge the changes from our branch into the ORIGINAL repository
that we forked from.

Opening this kind of pull request is how we let the original repo owners, who we want to collaborate
with, know that we have a contribution to the project that we would like them to consider including.
At this point, it's completely up to them whether to accept the changes or not, and the pull request
provides a good place for discussion about the changes you want to contribute.


## Updating Your Fork

Back in our terminal, we are ready to do more work on the project while we await a
response regarding our pull request. Let's check our status.

~~~
$ git status
~~~
{: .bash}

~~~
On branch update-country
nothing to commit, working directory clean
~~~
{: .output}

Since we have completed what we wanted to finish on our country, we want to go back
to using our master branch.

~~~
$ git checkout master
~~~
{: .bash}

~~~
Switched to branch 'master'
Your branch is up-to-date with 'origin/master'.
~~~
{: .output}

~~~
$ git branch
~~~
{: .bash}

~~~
* master
  update-country
~~~
{: .output}

When collaborating on a project, it is important to keep your repo up-to-date with
the changes your collaborators are incorporating into the project.
In order to keep your fork updated and get any new changes made in the upstream repository,
you'll need to merge in upstream changes manually.

Begin by adding the upstream repository

~~~
$ git remote add upstream https://github.com/unt-carpentries/countries_201909.git
$ git remote -v
$ git pull upstream master
$ git push origin master
$ git status
~~~
{: .bash}
