#Build a Better Hat Rack - All Contributions Welcome

## Audience
Open source contributors, open source project maintainers, or anyone looking to learn how they can help in the open source ecosystem 
## Objectives
Acknowledgement and respect for the vast amounts of unseen work in open source Tips and tools to assist project maintainers to identify and acknowledge all contributions from all contributors 
## Duration
I prefer a 30 minute slot
## Description
By default, we idolise a 'open source contributor' as a person that contributes code. But what about all the other things in the software development lifecycle - documentation, code review, marketing, support - so much work happens without proper acknowledgement. Learn just how every little bit helps, and how to find and acknowledge these contributions. 
## Abstract
We have many ways of reporting and recognising our code contributions in open source projects, but often it is the work we do outside of code commits themselves that get forgotten and unattributed. Hours of code review, documentation, testing; organising of meetups and volunteering at conferences; even just brainstorming and talking about things -- just how much of this is done without any kind of accreditation?

During this session, we will discuss what it means to contribute to open source projects, what constitutes a non-code contribution, steps we can take to recognise the work of our peers, and how projects can better encourage non-code participation through recognition and acknowledgement.

The talk centres on the LABHR movement, started by Leslie Hawthorn. LABHR describes a five-step process to identify and thank, both privately and publicly, non-code contributions.

This talk also provides useful, actionable people can take to acknowledge non-code contributions in open source, and introduces octohatrack, a simple Python command-line tool that utilises the GitHub API to those who contribute to a GitHub project that aren't normally displayed as 'contributors'.

For open source contributors: come see just how the smallest thing you do is deeply appreciated.

For project maintainers: come learn how to find these contributors, and help retain them by giving them the gratitude they deserve. 

## Outline
* Introduction of space - Open Source Software Development (2 minutes)
 - speaker intro
 - projects that pay, eg: docker (monetary), OpenStack (free tickets to summit)
* Open Source code contribution analytics (1 minute)
 - GitHub, etc, have many graphs and charts to track contributions
 - only track specific subsets of the code work
 - does not track any of the other elements of open source software - documentation, testing, marketing, bug review, marketing, etcetcetc
* Leslie Hawthorn and LABHR (5 minutes)
 - introduction of Leslie, formerly Elastic and Google Summer of Code
 - run down of her "Lets all Build a Hat Rack" (LABHR) project
 - 5 step explanation - write it down, send it to them; shout about it on twitter, write a LinkedIn recommendation, and do it for someone not like you (Woman of Colour, non-code contributions, etc)
* Demo: Live "hat-racking" (2 minutes)
 - demonstration of how to be nice and give someone praise
 - volunteer accolade recipient is given a chance to respond.
 - Note: the volunteer from the audience knows they will be called out in the talk
* How the demo has failed before (1 minute)
 - Every time I've given this talk, I've gotten 'jokingly negative' twitter feedback. Examples: [1](https://twitter.com/chrisjrn/status/627363232370421760) [2](https://twitter.com/tveastman/status/627363893522890752) [3](https://twitter.com/chrisjrn/status/638192883200167936) [4](https://twitter.com/chrisjrn/status/638193757343510528)
* Why people can't accept thanks (2 minutes)
 - going from the example feedback, a riff on minimisation, tall poppy syndrome and imposter syndrome
* Recognise our own achievements (1 minute)
 - riff on the [Donut Manifesto](http://larahogan.me/donuts/)
* The achievements of people in large numbers (1 minute)
 - e.g. StackOverflow, wikipedia pages - many small edits to create a large library of information
* The benefits of thanking (2 minutes)
 - thanking and retaining contributors
 - making people feel good about helping 
 - 'foot in the door' technique - little things making people more likely to do bigger things
 - recommend their work as a take away from their efforts
* Recommendations of work (2 minutes)
 - how to 'properly' use LinkedIn to give written recommendations (as opposed to the hit-and-miss of LinkedIn endorsements)
* The problem with GitHub Profiles (2 minutes)
 - no ability to edit, algorithm generated 'popular repos', 'contribution listings'
 - The green user graph: incorrectly used by recruiters to assess competency, linked to burn out from people trying to fill it, easily alterable by use of backdated git commits
* The problem with GitHub Project pages (2 minutes)
 - the number of contributors listed is based on an extremely limited set of requirements
 - the click through of the contributor count doesn't display all contributors for projects with more than 100 (only the top 100 ordered by lines of code committed works)
* How profiles can be fixed (1 minute)
 - add the ability to edit
 - add the ability for projects to tag someone as a contributor, regardless of if they have code in master
 - option to remove the graph all together
* Showing all the contributors to projects, code and non-code (3 minutes)
 - intro to `octohatrack`, Python app that deep dives a github repo to show all the contributions
 - statistics of popular python repos, e.g. requests, showing 3x non-code contribs as code
* Summary and Questions (2 minutes) 

## Notes
This was originally presented as a lightning talk highlighting Leslie Hawthorn's #LABHR project back in early 2015

Since then, I've presented this talk at multiple meetup groups, and a number of Python conferences, including PyCon AU and Kiwi PyCon. I've also written about this for OpenSource.com

Octohatrack is a python application that uses the GitHub API to show: * all the code contributors to a project, not just the top 100 by lines in master * all the non-code contributors, which includes anyone who has commented on an issue or PR, submitted an issue or PR, or commented on a commit.

The talk keeps evolving as the project keeps evolving, but the outliine submitted is per my last telling of the story.

I would absolutely love to share this continuing evolving story with a wider international audience.

Please note: the octohatrack project was previously known as 'octohat', but the name was changed due to the similarity between the GitHub trademark 'octocat'. I've written publicly about this, but the name change will not be mentioned in my talk.

