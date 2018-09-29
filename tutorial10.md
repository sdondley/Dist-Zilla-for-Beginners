# Testing Tutorial Part II: Writing and Running Tests with `Dist::Zilla`

Before proceeding make sure all the `sayhi` module tests pass. If not, do you
what you can to get them to pass. Let's also declutter our source tree so we can
focus on the files we care about.

`dzil clean`

Off to the races, we go.

## Fixing Failed Tests

To see what a failed test looks like, we are going to purposeflly introduce some
bad code. Edit the lib/sayhi.pm module and add the following lines to the end of
the file:

```

=hea1 SYNOPSIS

From the command line:

    sayhi          # Prints "Hello, World!" followed by a newline
    sayhi --shout  # Shouts it

```

From the source tree directory, run `dzil tests` and look toward the bottom of
the output for the following:

```

...

xt/author/00-compile.t .. ok
xt/author/pod-syntax.t .. 1/2
#   Failed test 'POD test for blib/lib/sayhi.pm'
#   at /usr/local/share/perl/5.20.2/Test/Pod.pm line 187.
# blib/lib/sayhi.pm (36): Unknown directive: =hea1
# Looks like you failed 1 test of 2.
xt/author/pod-syntax.t .. Dubious, test returned 1 (wstat 256, 0x100)
Failed 1/2 subtests

Test Summary Report
-------------------
xt/author/pod-syntax.t (Wstat: 256 Tests: 2 Failed: 1)
  Failed test:  1
  Non-zero exit status: 1
Files=2, Tests=5,  0 wallclock secs ( 0.02 usr  0.00 sys +  0.15 cusr  0.03 csys =  0.20 CPU)
Result: FAIL
[@Starter/RunExtraTests] Fatal errors in xt tests
[@Starter/RunExtraTests] Fatal errors in xt tests at inline delegation in Dist::Zilla::Plugin::RunExtraTests for logger->log_fatal (attribute declared in /home/steve/perl5/lib/perl5/Dist/Zilla/Role/Plugin.pm at line 59) line 18.

```

The test report says one of the `pod-syntax.t` tests failed and tells us exactly
what went wrong:

`blib/lib/sayhi.pm (36): Unknown directive: =hea1`

Silly us, looks like we made a typo in our pod on line 36 of the
`blib/lib/sayhi.pm` file. Note that your line number might be different from the
line reported here.

But wait, what is the `blib/lib` directory and what is our `sayhi.pm` file doing
in there? The tldr; version is you can ignore the `blib` portion of the
filepath and pretend it's not there.

But if you are curious to know, think back to last tutorial when we mentioned that
the `test` subcommand created a temporary build. That build is located in a
hidden `.build` directory in our source tree. Look inside of it with:

`cd .build; ls`.

In here, you'll see one or more randomly named directories along with a `latest`
and possibly a `previous` symbolic link. These are symbolic links to the
`latest` and  `previous` failed builds located in one of the randomly named
directories. Drop into the `latest` build and you'll see our the mysterious
`blib` directory containing our `lib/sayhi.pm` file. So this is why it says the
error is in the `blib` directory.

Let's jump out of the `.build` directory and back to the root of the source
tree. Oopen up the `lib/sayhi.pm` file and change `hea1` to `head1`. Run your
tests again to make sure everything is kosher:

`dzil test`

Looking good again? Great. We also have a new `SYNOPSIS` section in our
documentation to boot. Now that our toes are wet, let's dive all in and write
some new tests for our module. Where do we begin?

## Writing Your Own Tests

Follow these basic steps to write your tests.

### Step 1: Determine What Test to Write

Figuring out what tests to write can be daunting when you are new to testing.
Our task is harder because, contrary to good **test-driven development**
practices, which we'll talk more about soon, we've already written some code we
want to test. Fortunately our module is very simple and we can easily make up
our lost ground.

To determine which test to write, it helps to keep in mind that a test
determines if your code has certain characteristics or does what you expect it
to. If your code agrees with the tests expectations, the test passes.  If not,
the test fails. Your tests should answer very basic questions. For example:

* Does the module throw the proper error if we give it bad input?
* Does the module give us the output we expect when we input X? How about if we
  input Y? How about Z?
* Does our module not throw any warnings?

With that in mind, let's ask: "What do we expect our sayhi module to do?" Well,
when we type `sayhi` on the command line, we want it to output "Hello, World!\n"
and when we type `sayhi --shout` we want the same thing but in upper case.

OK, so now how do we actually write the tests that will do that?

### Step 2: Determining How to Write the Test

Fortuantely, we don't have to put much effort into writing our tests beyond
finding and installing any of the hundreds of Perl modules that have done the
hard work of writing off-the-shelf test functions for us.

In this case, we are going to bust out the `Test::Command` module available on
CPAN. It has a function called `stdout_is_eq` does exactly what we need.

### Step 3: Determining Where to Put the Test

Next we need to figure out where to put our tests by first asking: "Is this a
test end users should run?" The answer is not always obvious but in this case it
is. The end users needs to be ssure they will get correct output when running
the `sayhi` command. Therefore, our test is a standard test and should go in the
`t` directory.

But which file should the tests go in? It's up to the developer to figure out
how to best organize the test files in the `t` directory. Typically, a test
file's name starts with a double digits and that count upward.  Those digits are
followed by a dash, followed by a name. The test functions inside each
file are usually related somehow. For example, you might have a group of tests
for making sure a new feature works.

Following this convention we will name our test file `01-stdout_tests.t`. Tests
within the same directory are run in alphabetical order so the
`00-report-prereqs.t` test file, which begins with `00`, will run before the
`01-stdout_tests.t` file.

### Step 4: Writing the Tests

OK, with all that thinking and planning out of the way, we can write our tests.
First, install the `Test::Command` module:

`cpanm Test::Command`

Next, since our source tree doesn't yet have a `t` directory, let's create it:

`mkdir t`

Any tests we place in the source tree will be automatically moved into the
build's `t` directory.

Edit a new file `t/01-stdout_tests.t` and add the following test code to it:

```

use Test::Command tests => 2;

stdout_is_eq  "bin/sayhi",  "Hello, World!\n";
stdout_is_eq  "bin/sayhi --shout",  "HELLO, WORLD!\n";

```

The `tests => 2` bit is called the **plan** and tells the `Test::Harness` module
how many tests it should expect to run in this file. Including a plan helps
improve the clarity of the `Test::Harness` reports and quiets pesky warnings
from `Test::Harness`.

The two tests we run are straightforward. They both call a function called
`stdout_is_eq` that compares output from the command in the first argument to
the strign in the second argument. Now make sure our new tests pass:

`dzil test`

If you see errors, look closely at your test code and make sure it has no
mistakes. If that looks good, study the error messages closely and try to
pinoint where things went wrong and try to fix the issue.

Even if we have success with out new tests, we still have a big problem. Our
`lib/sayhi.pm` module relies on the `Greetings` module to generate the output.
What if the `Greetings` module isn't generating the proper output? Our tests
will fail.

So if we are going to be thorough, we should go back and add tests to the
`Greetings` module. We'll assign that to you for homework. Hint: Use tests
found in the `Test::Simple` module. We also highly recommend checking out this
[basic
tutorial](https://metacpan.org/pod/release/EXODIST/Test-Simple-1.302140/lib/Test/Tutorial.pod)
on testing Perl code for many more basic test examples.

## Test-Driven Development

As mentioned, writing tests first and then the code to get the test to pass is a
recommended approach to testing. This practice is known as "test-driven
development." To show how that process works, create a new test to see if sayhi
command that will output "HELLO, WORLD!\n" if we issue a `sayhi --yell` command.
Add a third test to the `t/01-stdout_tests.t` file:

`stdout_is_eq  "bin/sayhi --yell",  "HELLO, WORLD!\n";`

Don't forget to increment the plan to "3" while you're editing the file. Save
the file and run the tests:

`dzil test`

You should see the following in your tests:

```

t/01-stdout_tests.t .... 1/3
#   Failed test 'stdout_is_eq: bin/sayhi --yell, HELLO, WORLD!
# '
#   at t/01-stdout_tests.t line 6.
#          got: ''
#     expected: 'HELLO, WORLD!
# '
# Looks like you failed 1 test of 3.
t/01-stdout_tests.t .... Dubious, test returned 1 (wstat 256, 0x100)
Failed 1/3 subtests

```

No surprise here, our test failed. The failed tests says it "expected" "HELLO,
WORLD!\n" but "got" an empty string instead. To fix the error we need to update
the if conditional in our module's `execute` command:

`if ($opt->{shout} || $opt->{yell}) {`

In the `opt_spec` function, we must add the following anonymous array to the
return value:

`[ "yell|y", "same as shout" ],`

Once the changes are made, you should see that all tests pass again. Rinse and
repeat until the `sayhi` command of your dreams is complete.

## Changing the Types of Tests `dzil test` Runs

By default, the `dzil test` command runs the standard tests and the `[@Starter]`
bundle also causes it to run the author tests, too. The `test` subcommand gives
you options for changing this behavior:

* `dzil --no-author` - skips author tests.
* `dzil --release` - runs all the tests that run during when the `dzil
  realease` command is given. More on this later.
* `dzil --extended` - this is an advanced option for running tests that run only
  when the $ENV{EXTENDED_TESTING} is set to true. Extended tests typically take
  a long time to run and so developers code these tests to run only when the
  EXTENDED_TESTING flag is set to help cut development time down.
* `dzil --automated` - used to run "smoke tests" which we won't cover here.
* `dzil --all` - runs all the different kinds of tests

### Running Test Files More Selctively with the `prove` Command

As your project gets more complex, you'll accumulate more and more tests which
can slow things down a great deal. To speed up testing, you can selectively run
which test files to run with the `prove` command supplied by the Perl core. To
run just the output tests, we can issue the following `prove` command:

`prove -l t/01-stdout_tests.t`

The `-l` option tells the prove command to look in the `lib` directory for the
module. You should read over `prove`'s documentation to get more familiar with
it and its other options.

Note that if we are doing some advanced usage of `Dist::Zilla` and need it to
generate some code that your test needs to pass, you will have to do a `dist
build` first and run the `prove` command from inside the build tree. We don't
need to do that in this case since we have all the code we need is in our source
tree. And if you are running `XS` modules with c code in them, you'll need to
seek another tutorial out for how to get around that with the `prove` command.

## Useful Plugins for Improving the Quality of Your Code

As we saw, the `[@Starter]` bundle includes test plugins to find potential
problems with your code. For example, the `[PodSyntaxTests]` warns you when your
documenation has an error.

You can add similar kinds of tests to your distirubiton to "coach" you and make
recommendations for code improvement. This is a quick overview of some of the
more common, basic ones that you may want to explore further or replace with
more sophisticated test plugins.

### POD Coverage Tests

Hop over to the `Greetings` work area and modify the `dist.ini` file and add the
following plugin:

`[PodCoverageTests]`

Now run `dzil test` and you'll see a failed test for two `naked subroutines`,
`hw` and `hw_shout`. This means that your documentation does not properly
document how these functions work. Update the inline documentation in your
module to get those tests to pass.

### Kwalitee Tests

A Kwalitee Test judges the overall quality of your distribution. Instead of
describing what it does, let's set it in action. Add the plugin to your
`dist.ini` file:

`[Test::Kwalitee]`

We are going to teach you a new trick for downloading plugins mentioned in your
`dist.ini` file. Run the following command:

`dzil authordeps --missing | cpanm`

This will detect any missing plugins and install them for you.

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
