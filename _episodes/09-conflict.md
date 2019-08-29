---
title: Conflicts
teaching: 15
exercises: 0
questions:
- "What do I do when my changes conflict with someone else's?"
objectives:
- "Explain what conflicts are and when they can occur."
- "Resolve conflicts resulting from a merge."
keypoints:
- "Conflicts occur when two or more people change the same file(s) at the same time."
- "The version control system does not allow people to overwrite each other's changes blindly, but highlights conflicts so that they can be resolved."
---

> ## Set up a Conflict
> 
> Have one of the helpers create a PR to countries/Libya.md that will create
> a conflict:
> ~~~
> ##Libya
> ## population
> 6,293,253
> 
> ## capital
> Tripoli
> 
> ## official language
> Arabic
> 
> ## interesting trivia
> Libya is the fourth largest country in Africa
> ~~~
> {: .output}
> Make sure these changes get merged into upstream before having learners pull
> in the upstream changes (after they commit their local Libya.md changes).
> {: .callout}

As soon as people can work in parallel, they'll likely step on each other's
toes.  This will even happen with a single person: if we are working on
a piece of software on both our laptop and a server in the lab, we could make
different changes to each copy.  Version control helps us manage these
[conflicts]({{ page.root }}/reference/#conflicts) by giving us tools to
[resolve]({{ page.root }}/reference/#resolve) overlapping changes.

To see how we can resolve conflicts, we must first create one (This would be
a good time for someone to merge the helper's Libya.md PR).
The file `countries/Libya.md` currently looks like this in our repository:

~~~
$ cat countries/Libya.md
~~~
{: .bash}

~~~
##Libya
## population

## capital
 
## official language

## interesting trivia
~~~
{: .output}

Let's add details about Libya to our copy:

~~~
$ nano countries/Libya.md
~~~
{: .bash}

~~~
##Libya
## population
6,000,000

## capital
Tripoli

## official language
Arabic

## interesting trivia
Inhabited as early as 8000 BC.
~~~
{: .output}

We like our work, so we commit the changes and push to our GitHub repo:


~~~
$ git add countries/Libya.md
$ git commit -m "Adds details about Libya."
~~~
{: .bash}

~~~
[master 90d1076] Adds details about Libya.
 1 file changed, 4 insertions(+), 4 deletions(-)
~~~
{: .output}

~~~
$ git push origin master
~~~
{: .bash}

~~~
Counting objects: 4, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (4/4), 464 bytes | 0 bytes/s, done.
Total 4 (delta 1), reused 0 (delta 0)
remote: Resolving deltas: 100% (1/1), completed with 1 local object.
To https://github.com/ldko/2018-06-05-unt-swc-collaboration.git
   f15fb28..90d1076  master -> master
~~~
{: .output}

Later, we decdide we want to make sure our repository is up-to-date
with the upstream repository.

~~~
$ git pull upstream master
~~~
{: .bash}

~~~
remote: Counting objects: 4, done.
remote: Compressing objects: 100% (4/4), done.
remote: Total 4 (delta 1), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (4/4), done.
From https://github.com/unt-carpentries/2018-06-05-unt-swc-collaboration
 * branch            master     -> FETCH_HEAD
 * [new branch]      master     -> upstream/master
Auto-merging countries/Libya.md
CONFLICT (content): Merge conflict in countries/Libya.md
Automatic merge failed; fix conflicts and then commit the result.
~~~
{: .output}

We see a "CONFLICT" message because changes have been committed to the upstream
version of Libya.md, but we changed our local copy without first incorporating
those.

![The Conflicting Changes](../fig/conflict.svg)

Git isn't sure how to [merge]({{ page.root }}/reference/#merge) the changes
from upstream because they are different from the new updates we have made.

~~~
$ git status
~~~
{: .bash}

~~~
On branch master
Your branch is up-to-date with 'origin/master'.
You have unmerged paths.
  (fix conflicts and run "git commit")

Unmerged paths:
  (use "git add <file>..." to mark resolution)

	both modified:   countries/Libya.md

no changes added to commit (use "git add" and/or "git commit -a")
~~~
{: .output}


Notice the message give us hints about what to do:
"Fix Conflicts," "git commit," "git add"

We need to resolve the lines in the file that conflict, then add our changes
to the staging area, then commit the changes.

~~~
$ cat countries/Libya.md
~~~
{: .bash}

~~~
##Libya
## population
<<<<<<< HEAD
6,000,000
=======
6,293,253
>>>>>>> 68cf724e5dcb8adc858efc8adc64ff85f44419ab

## capital
Tripoli

## official language
Arabic

## interesting trivia
<<<<<<< HEAD
Inhabited as early as 8000 BC.
=======
Libya is the fourth largest country in Africa

>>>>>>> 68cf724e5dcb8adc858efc8adc64ff85f44419ab
~~~
{: .output}

`HEAD` shows us where our changes that differ from upstream start.
`=======` shows us where our changes end and the conflict begins with what is
in upstream.
`>>>>>>> 68cf724e5dcb8adc858efc8adc64ff85f44419ab` shows us where the
conflicting change ends. (The string of letters and digits after that marker
identifies the commit we've just downloaded.)

This looks intimidating, but all we need to do is modify the file to look
the way we want, then add and commit our changes.

The upstream population looks more precise than what we added, so let's use
that. There was no conflict with the capital and official language. We like
both pieces of trivia, so we will keep both. 

~~~
$ nano countries/Libya.md
~~~
{: .bash}

~~~
##Libya
## population
6,293,253

## capital
Tripoli
 
## official language
Arabic

## interesting trivia
Inhabited as early as 8000 BC.

Libya is the fourth largest country in Africa


~~~
{: .output}

Now that we have edited the file to look how we want, we can push the resolved
changes to GitHub:

~~~
$ git add countries/Libya.md
$ git status
~~~
{: .bash}

~~~
On branch master
All conflicts fixed but you are still merging.
  (use "git commit" to conclude merge)

Changes to be committed:

	modified:   countries/Libya.md

~~~
{: .output}

~~~
$ git commit -m "Resolve details about Libya."
$ git push origin master
~~~
{: .bash}

~~~
Counting objects: 8, done.
Delta compression using up to 8 threads.
Compressing objects: 100% (8/8), done.
Writing objects: 100% (8/8), 1.16 KiB | 0 bytes/s, done.
Total 8 (delta 4), reused 0 (delta 0)
remote: Resolving deltas: 100% (4/4), completed with 2 local objects.
To https://github.com/ldko/2018-06-05-unt-swc-collaboration.git
   02a0a2c..65825a0  master -> master
~~~
{: .output}

We can see on GitHub now, that everything is as we have edited.

Git's ability to resolve conflicts is very useful, but conflict resolution
costs time and effort, and can introduce errors if conflicts are not resolved
correctly. If you find yourself resolving a lot of conflicts in a project,
consider these technical approaches to reducing them:

- Pull from upstream more frequently, especially before starting new work
- Use topic branches to segregate work, merging to master when complete
- Make smaller more atomic commits
- Where logically appropriate, break large files into smaller ones so that it is
  less likely that two authors will alter the same file simultaneously

Conflicts can also be minimized with project management strategies:

- Clarify who is responsible for what areas with your collaborators
- Discuss what order tasks should be carried out in with your collaborators so
  that tasks expected to change the same lines won't be worked on simultaneously
- If the conflicts are stylistic churn (e.g. tabs vs. spaces), establish a
  project convention that is governing and use code style tools (e.g.
  `htmltidy`, `perltidy`, `rubocop`, etc.) to enforce, if necessary
