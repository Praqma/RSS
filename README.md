# Repository standards

In Praqma we automate things - yet I found we still craft every single repositories by hand.

This repository will evolve to automated repository standards, we little human work is used to create nice and standard repositories just they way we like them.

## The problem

Repositories looks very different, and some of them doesn't live up to what we would like of a bare minimum.

Some remember to create labels and milestones according to our [work flow](http://www.praqma.com/stories/a-pragmatic-workflow/)  or [milestone and office hours](http://www.praqma.com/stories/milestones-and-officehours/), some misspell the milestones, some do it half.

Beside that we have several other ongoing, half adopted standards to keep an eye on regarding our repositories.
One is our internal handbook write-up - oh well it was in our [noops repo](https://github.com/Praqma/noops/blob/master/github.md) - about how we handle permissions and collaborators in our Github org.
Another one [is our internal slack post about how we add CI automation by convention, in a CI-folder.](https://praqma.slack.com/archives/C363GC04T/p1491216765858420)
To all this we add the new front-matter stuff that is used to generate our [repository statistics and overview](http://code.praqma.com/repo-stat/)

## Solution

We must automate all this and implement it as part of this repository you read now. Below are the current ideas.

# Features and functionality

First focus in on the create functionality, as this will help us the most on daily basis right now.

## Create

The create repository feature will help you easily create a new repository that live up to our standards, and with as much automated content (probably templates) and configuration as possible:
* (repo config) Labels according to our flow and office hours.
* (repo config) The standard milestones according to our office hours.
* (repo config) Frontmatter information, either default or by input choices.
* (repo config) Configure permission and collaborators, as well as other repository settings (release and automation users maybe)
* (waffle.io) Create waffle.io board for the project, with our standard configuration.
* (waffle.io) Add project to common waffle.io boards.
* (templated content) CI folder and standard contents, or links to. This could be Jenkins pipeline or concourse setup.
* (templated content) Copy-in some default content (templates) for repo README, contribution and license file, as well a roadmap.

The create feature can either be used as a kind of script, or from a manual executed Jenkins job. Both allows for some input parameters.

It should be allowed to run without adding templated content, to allow to easily upgrade to newer version of repo configuration on existing project where we will not want to overwrite content.

Create repository feature will always use latest repository standards.

## Verify

It should be possible to run a verification of one or several repositories against our latest repository standard.
Verification will _include_ labels, milestones, frontmatter and CI, but we _exclude_ actual content, e.g. what is originally contributed by templates, as this should be allowed to evolve and be changed by the project.

## Monitor

Monitoring is basically reporting of continuously running verifications. That way we always know how many of our repositories follow our standards.
Reporting should probably be done to an ELK system, but could also contribute to the [repo stats we have](http://code.praqma.com/repo-stat/).

## Compliance verification and compliance request issues

When a verification is executed on an existing repository, a compliance check is executed.
If the repository do not follow standard, an issue in the repository is automatically created reporting the compliance check failed.
The reported issues should explain where compliance is missing, as well as point to resources explain how to fix the issues. E.g. run create script again.

The same issues should never be filed twice, thus running verification twice with the same result, should not create two issues, but detect the compliance problems is already issued.

## Release flow for our repository standards

Our repository standards always have a semantic version number, as well as a 'latest' pointer.

Minor releases will verify and update monitoring of all our existing repositories, reporting missing compliance.
Major will generate new compliance request issues in the repositories, for those configuration changes we can not automatically correct.
We use major releases to ensure new ways of working.
Patch releases are just small fixes, doesn't change compliance for any repository.

# Design

## Compliance verification using behavior driven test frameworks

Maybe all the verification can be implemented using behavior driven test frameworks, like cucumber or Rspec, so the tests themselves becomes the repository standards and are easy to read so they serve as documentation for our standards as well.
It will also improve the reporting of the compliance issues.

## API thoughts

Github API is just REST API so curl commands could do, but in the long run were better of with a programming language to support easy wrapping, parsing etc. and to use programming language features like objects etc.

[https://developer.github.com/v3/](https://developer.github.com/v3/)

So we should use a wrapper library:

[https://developer.github.com/libraries/](https://developer.github.com/libraries/)

We use a lot of groovy in Praqma, we use some Perl (most the old grumpy guys!) and a few projects are in Ruby. Recently we use Go as well.

In theory it doesn't matter what we chose, we can work with them all, but I think it weight that Ruby if officially supported by Github as wrapper library.
Our largest complain about Ruby is the maintance and handling of Ruby environment, gems etc. which is a non-problem if we use containers.


# Testing

Yes the scripts and repository standards should have test themselves.
Maybe we can simply test by using the create method on a dummy repo, and the run compliance check and if that doesn't find anything it seems okay.


# Use-cases

# Use-cases

## Create

As a Praqma employee I would like to run the following command to create a github repository called `myproject` in the `praqma` organisation with the description `This is a temporary project for me`:

	docker run --rm praqma/repository-standards create "praqma/myproject" "This is a temporary project for me"


## Update

I can update the repository with a certain configuration matching latest repository standards, running:

	docker run --rm praqma/repository-standards update --permissions "praqma/myproject"

or

	docker run --rm praqma/repository-standards update --topics "praqma/myproject" "groovy" "tool" "automation"

I can also make everything match standards:

	docker run --rm praqma/repository-standards update "praqma/myproject"

## Verify

I can easily verify my repository for compliance with latest standards:

	docker run --rm praqma/repository-standards verify "praqma/myproject"

## Versioned standards

The docker commands without version will run docker images with latest tags, which will match our latest repository standards.
I can pick and chose using version number for the docker image:

	docker run --rm praqma/repository-standards:1.7.9-345 verify "praqma/myproject"

There is a 1:1 relation between the docker image and the repository-standards release versions.

## Use-case limitations

* We will avoid implementing support for functionality, to start with, that seems easier to to in the UI. This could maybe be things like the above update example with "Add topics". You will just have to add topics to a repository for now using the Github UI.
