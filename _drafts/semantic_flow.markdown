---
layout: post
---

I cannot find a way to manage deployments and stable releases with any git
branching model. The *git-flow* adds to many branches and too many merge
commits. Those merges makes easy to bring fixed bugs back, re-introduce code
and add noise in the log that makes harder to find the commit that broke
something.

The *github-flow* does not fit environments where you need to manage stable
releases. As there is only a master branch new features witch introduces new
bugs get in the middle of a release making it impossible to get something
throughly tested. Also it doesn't help managing deploys and then you need
some external tool to manage that.

Another pattern is to use a `production` and `staging` branches to manage
deployments to those environments, like Codeship does. The issue with this
approach is that as the git-flow it introduces to many merges and it makes
difficult to manage bugs in production.

## The tags flow

The recommended approach to manage stable versions is to use tags, but most of
the times the tags are added to the flow but are not really part of it, then it
becomes a bureaucratic part of tagging the release, but as nothing break if
someone forget that no one can really trust those tags. They are like a test
that no one runs.

So my idea is use tags to manage deployments and use semantic versioning to
manage the different environments. Then stable version tags deploy to
production, beta versions deploy to staging (or a beta testing environment).
Alfa releases could be for QA, etc.

As with github flow there is only one main *master* branch for development,
that, but stable releases are deployed with tags.

Those tags should follow the semantic versioning scheme as it already known by
many developers.

## Release flow

The flow will obviously depend on the requirements, but in general, the idea is
to to manage different environments with different pre-release identifiers. For
example, lets suppose first there are some QA team that test the release in
some kind of staging environment, then you tag some version as 1.0.0-alpha,
they discover some bugs, the development team fix those with some commits,
those commits are done in an ephemeral branch (1.0.0) and they release (tag)
1.0.0-alpha.1 and 1.0.0-alpha.2 until QA approves it.

They can tag it as (1.0.0-beta) so it is deployed for the beta testers (those
users that test new features before the release). Once it is ready someone tags
this version as 1.0.0 and it is deployed to production. 

The branch 1.0.0 can now be deleted. If there is a bug found that requires a
hotfix a new branch 1.0 can be created and a new release could be tagged 1.0.1
that can go to QA with 1.0.1-alpha first.

## No merges

One central point is that those branches are not merged-back to master, only
usefull commits are cherry-picked. This makes history clear and easy to follow,
makes bugs easier to find and makes developers happy.

## Signed

As tags can be digitally signed it's easier to manage permissions, so only some
people can release to production.

## The missing part

The only issue, is that there is no support on the continuous deployment
solutions that will trigger deploys with tags.

Do you like this approach? Any idea is welcomed on twitter.
