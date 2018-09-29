# Testing Tutorial Part III: Useful Test Plugins

As we saw, the `[@Starter]` bundle includes test plugins to find potential
problems with your code. For example, the `[PodSyntaxTests]` warns you when your
documenation has an error.

You can add additional plugins to your distirubiton that similarly "coach" you
and make recommendations for code improvement. This is a quick overview of some
the more common ones that you may want to include in your `dist.ini` file.

Note that many of the code quality tests below can be integrated with advanced
text editors like vim. Instead of tests, you may prefer to lean on your
your text editor instead.

## Installing Plugins

Before you can use the test plugins, you have to first install them. It's an
easy process.

First, drop the plugin into your `dist.ini` file using the usual bracketed
notation we've seen before. Next, add any module dependencies for
that plugin with the following two commands:

```

dzil authordeps --missing | cpanm
dzil listdeps --author --missing | cpanm

```

Obviously, you'll need `cpanm` installed to take advantage of these shortcuts.

## POD Coverage Tests

[Official documenation](https://metacpan.org/pod/Dist::Zilla::Plugin::PodCoverageTests)

Hop over to the `Greetings` work area and modify the `dist.ini` file and add the
following plugin:

`[PodCoverageTests]`

This plugin ships with `Dist::Zilla` so you don't need to run the module
installation commands above.

Now run `dzil test` and you'll see a failed test for two `naked subroutines`,
`hw` and `hw_shout`. This means that your documentation does not properly
document how these functions work. Update the inline documentation in your
module to get those tests to pass.

## Kwalitee Tests

[Official documenation](https://metacpan.org/pod/Dist::Zilla::Plugin::Test::Kwalitee)

A Kwalitee Test judges the overall quality of your distribution. Instead of
describing what it does, let's see it in action. Install the `[Test::Kwalitee]`
plugin to your `dist.ini` file:

The `[Test::Kwalitee]` test is run only when the `release` subcommand is issued.
So to run its tests, you must run:

`dzil test --release`

You should see the following errors within your test output:

```

xt/release/kwalitee.t ..... 1/?
#   Failed test 'has_changelog'
#   at xt/release/kwalitee.t line 7.
# Error: The distribution hasn't got a Changelog (named something like m/^chang(es?|log)|history$/i. A Changelog helps people decide if they want to upgrade to a new version.
# Details:
# Any Changelog file was not found.
# Remedy: Add a Changelog (best named 'Changes') to the distribution. It should list at least major changes implemented in newer versions.

#   Failed test 'has_license_in_source_file'
#   at xt/release/kwalitee.t line 7.
# Error: Does not have license information in any of its source files
# Details:
# LICENSE section was not found in the pod.
# Remedy: Add =head1 LICENSE and the text of the license to the main module in your code.
# Looks like you failed 2 tests of 17.
xt/release/kwalitee.t ..... Dubious, test returned 2 (wstat 512, 0x200)
Failed 2/17 subtests

Test Summary Report
-------------------
xt/release/kwalitee.t   (Wstat: 512 Tests: 17 Failed: 2)
  Failed tests:  6, 15
  Non-zero exit status: 2
Files=4, Tests=23,  0 wallclock secs ( 0.02 usr  0.02 sys +  0.48 cusr  0.02 csys =  0.54 CPU)
Result: FAIL

```

`Test::Harness` says we failed two tests: `has_changelog` and
`has_license_in_source_file`. We will address these issues in another tutorial.

## Mojibake Tests

[Official documenation](https://metacpan.org/pod/Dist::Zilla::Plugin::MojibakeTests)

If you work a lot with improving older CPAN modules, the `[MojibakeTests]` module may can help
you spot UTF-8 encoding problems in your code. Add the following to your
`dist.ini` file:

## Perl Critic Tests

If you want to see if your code is following coding best practices, you can use
`[Test::Perl::Critic]`:

After installing this module and running `dzil test`, we'll see a new error:

```

xt/author/critic.t ........ 2/?
#   Failed test 'Test::Perl::Critic for "blib/script/sayhi"'
#   at /usr/local/share/perl/5.20.2/Test/Perl/Critic.pm line 104.
#
#   Code before strictures are enabled at line 4, column 1.  See page 429 of PBP.  (Severity: 5)
xt/author/critic.t ........ Dubious, test returned 1 (wstat 256, 0x100)
Failed 1/2 subtests

```

This cryptic error means there is no `use strict` pragma in our `sayhi` command.
Slap that in and you'll be good to go.

Note that Perl Critic has a reputation for being quite opinionated. You can
change the behavior of Perl Critic to your liking by supplying configuratoin
file parameters to the plugin with something like:

`critic_config = perlcritic.rc`

The plugin will look for the configuration file in the default location, the
root of your source tree.

There is a lot to the Perl Critic tests and you should definitely [read the
documentation](https://metacpan.org/pod/Test::Perl::Critic) to get the most out
of it.

## Trailing Whitespace Tests

[Official documenation](https://metacpan.org/pod/Dist::Zilla::Plugin::Test::EOL)

If you want to be able to bounce a coin off your code, you'll be interested in
the `[Test::EOL]` plugin which will find trailing whitespace at the
end of lines or files.

## More Tests

Needless to say, there are many, many more plugins out there for helping you
improve your code. Hopefully the small sampling we've offered here whets your
appetite for exploring other useful test plugins on CPAN.

The easiest way to find them is with a [CPAN
search](https://metacpan.org/search?q=dist%3A%3AZilla%3A%3ATest)
