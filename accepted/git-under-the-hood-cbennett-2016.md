| Proposer      | Year | Outcome  | Video   |
| ------------- | ---- | -------- | ------- |
| Clara Bennett | 2016 | Accepted | TBA     |


# Proposal

## Title
Git: A Peek Under the Hood

## Category
Source Control

## Duration
30 or 45 minutes: proposal is written for a 30-minute talk, but I think I could easily extend to 45 if there was interest and availability

## Description
Git is a powerful source control tool, but the learning curve can be steep. This talk introduces the underpinnings of git, to provide a foundation for more confident and effective git use. My hypothesis is that having a solid mental model of what git is actually doing under the hood helps you more easily learn to use advanced git features.

## Audience
Programmers familiar with git basics who want to be more effective, or who want to understand what happened when they made an error.

## Level
Intermediate

## Objectives
Attendees will learn how git stores files and histories, how checkouts work, and how to track their interactions with the git tree.

## Detailed Abstract
Thanks in part to the popularity of GitHub, git is a widely used version-control system. It is powerful and can do great things to facilitate collaboration on programming projects, but, due to a combination of git's idiosyncracies and the relative dearth of git resources beyond the ubiquitous Intro Tutorial, it can be difficult to get past the basics.

It is possible to be productive in git by memorizing a few sequences of steps: how to commit, how to push, and how to pull. However, this kind of workflow can be messy and is forwards-only, leaving the developer to handle reversions manually and without recourse when errors happen. A better command of git can make it easier to experiment, keep only the changes you want, and share work with others.

On the hypothesis that tools are easier to use effectively when you understand the underlying mechanics, this talk aims to provide a foundation from which audience-members more easily and effectively learn to leverage the full power of git.

## Outline
1. Intro (3 minutes)
1. Why should you care about git internals? (2 minutes)
    * build a better understanding of how git works
    * become a more confident and effective git user
    * know how to get yourself out of trouble
1. Before we start: a brief interlude in defense of git GUIs (2 minutes)
    * command line tools require you to not only already know what you're doing, but be able call up the precise commands on the spot
    * a good GUI will help you visualize the repo, see your options for action, and understand how the repo has changed in response to your actions
1. What gets saved when you commit? (2 minutes)
    * snapshot, meta-data
1. Detecting changes: the commit SHA (2 minutes)
1. Ancestry and branches (5 minutes)
    * commits know about their parents
    * a branch is just a pointer
    * a merge commit is one that has multiple parents
    * together, the branches form a tree
1. Traversing the tree (HEAD) (5 minutes)
    * what happens when you check out a commit? a branch?
    * accessing ancestors
    * the unattached HEAD state
    * example: undoing a commit without losing your work
1. Retracing your steps (the reflog) (2 minutes)
1. Next steps (1 minute)


## Additional Notes
### Addressing potential talk similarity
I realized that there was a talk at PyCon 2015 that may have some overlap with this proposal (David Baumgold's [Advanced Git](https://us.pycon.org/2015/schedule/presentation/343/) talk). I do think that this proposal is significantly different in that the focus is on understanding the underpinnings of git, not on learning the fun and fancy stuff that goes beyond "commit and push". That said, I know ya'll like to keep up the variety from year-to-year, so any suggestions on refocusing in a more differentiating (and interesting, hopefully) direction would be much appreciated.

### Speaking experience
I gave a [lightning talk at PyCon 2015](https://t.co/avRWdOM8qU), but otherwise, I've not spoken at any software conferences. I have twice given a [class in Pandas](https://github.com/picwell/intro_pandas) with my local PyLadies chapter, and I've collaborated on a company [tech talk for good git practices](https://github.com/themotleyfool/advance-git-tech-talk/blob/master/advance-git-tech-talk.md).

# Feedback
None given. (I submitted ~3 weeks before the deadline.)
