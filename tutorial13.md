# Ch-Ch Changes

A couple of tutorials back, one of the Kwalitee tests failed because our
distribution lacked a `Changes` file. Let's address this deficiency and show you
how to use `Dist::Zilla` to generate this file and add it to the distribution.

The `Changes` file is designed for end users to help them do a risk analysis of
new versions of your module. It helps them answer the question, "Does the new
version of this module have new features or bug fixes that are worth the risk of
upgrading?" If all your new version does is offer some document changes, they
might want to pass. But if the module can now brush their teeth for them,
they'll probably want to download and install it.

A `Changes` file should be a simple log with each new release of your
distribution constituting three components:

* the version number of the new release
* the date of the release
* a summary of changes of interest to end users

The version and date of the release can be easily automated. The summary of
changes **can** be automated with a simple dump of your git commit log (assuming
you are using git). Whether this is something you **should** do is up to you. If
your typical end user is tecnically inclined, this may be fine. Or if you write
your commit messages with the end user in mind, you might be able to get away
with this though your `Change` file might be a lot to wade through. Ideally,
however, you will craft each summary with all the love and attention you can
muster.
