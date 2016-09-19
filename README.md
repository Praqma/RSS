# Repository structure standards
We need a common set of standards that each and every repository in praqma possess.
The standards should be followed by an opt-out process; you need to comply with everything, unless you have a reason not to.
In this way we can have both small scripts and company products under the same standard.


## Checklist
- [ ] Does it have a README.md file containing:
	- [ ] Reason for making the script
	- [ ] Maintainer/Technical owner (contact information)
	- [ ] Example of a valid usage pattern
	- [ ] Roadmap
	- [ ] Contribution instructions including how to build and test, how to submit new change
- [ ] A LICENSE file with MIT license
- [ ] A pipeline. See below
- [ ] Subpage on http://code.praqma.com/
- [ ] Metrics e.g. [Findbugs for java](http://findbugs.sourceforge.net/)
- [ ] Own site like: http://www.2git.io/
- [ ] Logo
- [ ]

Remember this is a working resource. Some things become obsolete, some things arise as new requirements. Make issues and pull requests whenever needed.

## A pipeline

Projects that are mature enough, such as our websites or our Jenkins plug-ins, should
have a pipeline set up at http://code.praqma.net/ci/

The pipeline should implement our praqmatic workflow as described in http://www.praqma.com/stories/a-pragmatic-workflow/

For other projects, such as POCs, it might be easier to start with just an automated
build pipeline, triggered on commits. As long as the repository is public, there are free
services for this, for example https://travis-ci.org/
