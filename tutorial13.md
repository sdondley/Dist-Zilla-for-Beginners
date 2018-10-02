# Ch-Ch Changes

A couple of tutorials back, one of the Kwalitee tests failed because our
distribution lacked a `Changes` file. Let's address this deficiency and show you
how to use `Dist::Zilla` to generate this file and add it to the distribution.

The `Changes` file is designed for end users to help them do a risk analysis of
new versions of your module. It helps them answer the question, "Does the new
version of this module have new features or bug fixes that are worth the risk of
upgrading?" If your new version offers only minor document changes, they might
choose to pass. But if the module can now brush their teeth for them, they'll
probably want to download and install it.

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
with this though your `Changes` file might be a lot to wade through.

Ideally, however, you will craft each version summary with all the love and
attention you can muster to help you stand out as the conscientious developer
that you are. As such a developer, this tutorial will not bother showing you how
to generate your Change log from git. If you are truly that lazy as to not want
to write summaries for your end users, you can seek out another tutorial for
that. Rant over. At least you know where we stand on this matter.

## Automating a Distribution's Version Numbers

Before tackling how to create the Change log, we need to implement automated
versioning in our `App::sayhi` module.

### Upgrade the `[@Starter]` Bundle's Capabilities

We are going to tap an old friend, the `[@Starter]` bundle, to get our automated
versioning mechanism in place. Before we can do that, though, we need to imbue
it with more capabilities than it currently has. In the `dist.ini` file for the
`App::sayhi` distribution, add two lines directly after the section for the
`[@Starter]` bundle like so:

```

[@Starter] ;already in your dist.ini file
; add the next two lines below
revision = 3
managed_versions = 1

```

Up until now, we have been using revision 1 of the `[@Starter]` bundle. Revision
3, along with the `managed_versions` option, adds new plugins to the bundle that
we will use to automate our version numbers and help generate our Change log.
Namely, the `[RewriteVersion]`, `[NextRelease]` and `[BumpVersionAfterRelease]`
plugins. Let's put these plugins to work for us to make .

### Add `$VERSION` to the Module

Just as we did for the `Greetings` module, we need to add the `$VERSION`
variable to our `App::Sayhi` module which should be placed just under our
module's package delcaration so it's easy to find:

`our $VERSION = '0.001';`

Let's see if our distribution can build with that in place:

`dzil build`

Ugh. What's Zill monster angry about now? Let's look:

```

[DZ] attempted to set version twice
[DZ] attempted to set version twice at inline delegation in Dist::Zilla for logger->log_fatal (attribute declared in /home/steve/perl5/lib/perl5/Dist/Zilla.pm at line 768) line 18.

```

### Remove `version` from `dist.ini`

The problem is we are setting the version twice now, once in our module and once
in `dist.ini`. Let's fix this by deleting the following line from `dist.ini`:

`version = 0.001`

And let's see if that fixes things for us:

`dzil build`

```

[@Starter/NextRelease] failed to find Changes in the distribution
[@Starter/NextRelease] failed to find Changes in the distribution at inline delegation in Dist::Zilla::Plugin::NextRelease for logger->log_fatal (attribute declared in /home/steve/perl5/lib/perl5/Dist/Zilla/Role/Plugin.pm at line 59) line 18.

```

### Add the `Changes` File

Zilla monster has found something else to stomp his feet about. As we can see
from the error, the `[NextRelease]` plugin is complaining about not having a
`Changes` file. This is easy to fix. From the command line, issue:

```

touch Changes

```

This will create a blank Changes file for us. OK, third time's a charm, they say:

`dzil build`

And now Zilla monster is finally appeased. Obviously he doesn't care much about
blank change log files but our users do. So let's make the change log useful. Add the following
to the top of `Changes` file:

```

{{$NEXT}}

```

Let's see what this generates:

```

dzil build
cat lib/App/sayhi.pm

```

Cool, the `[NextRelease]` replaced `{{$NEXT}}` with the version number and a
timestamp. All that's missing is a summary of version changes. Let's add it
to the file below the `{{$NEXT}}` template variable:

```

  - Initial release
  - Greet the world with they `sayhi` command
  - See `sayhi -h` for available options

```

And now:

```

dzil build
cat lib/App/sayhi.com

```

We now have a simple Change log for our end users and an automated versioning
system in place to boot. The log format in our example is very basic. Notice we
added some basic markdown syntax with the backticks. If you are interested in
tricking it out more, consult the [`[NextRelease]`
documentation](https://metacpan.org/pod/Dist::Zilla::Plugin::NextRelease) for
additional options and template variables you can add to the `Changes` file. We
also recommend Neil Bowers' [blog post on Change log
conventions](http://blogs.perl.org/users/neilb/2013/09/a-convention-for-changes-files.html)
for inspiration. And before going too crazy, you should consult the [CPAN
spec](https://metacpan.org/pod/CPAN::Changes::Spec) for the Changes file.

There is a bit more to cover with the Change log with the
`[BumpVersionAfterRelease]` plugin which we will cover when the time comes for
discussing releasing our distribution to the world.
