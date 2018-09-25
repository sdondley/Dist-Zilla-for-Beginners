# Distributing Your First Perl Module with Dist::Zilla

## Sharing Your Work

Imagining beginning programmers the world over are going out of their minds
having to type out `print "Hello, World!\n"`, you create the following module
for them to ease their burdens:

```

# Greetings.pm file
package Greetings;

sub hw {
  print "Hello, World!\n";
}

```

You are eager to share your module with others on CPAN so they can take
advantage of its goodness. In other words, you want to create a distribution for
your module. But where do you start?

## Tools for Helping You Get Your Distribution Started

The hard way involves creating all the directories, files, tests, installers,
documentation, meta files, etc. that go into creating a distribution from
scratch. If you are a masochist, this is the recommended approach.

For non-masochists, existing tools are available to help automate the creation
of distributions. For example, you can use the `h2xs` command line tool that
ships with Perl to help you get your distribution started. Drop into any empty
directory on your system and run the following command:

`h2xs -AX -n Greetings;`

Take a peek inside the resultant `Greetings` directory and you'll see the
command generated a minimal distribution for your `Greetings` module. Now you
can go in and edit this "skeleton" or "bare-bones" distribution and add some flesh
to it with custom code, tests, documentation, etc. But what `h2xs` generated for
you without any modifications could technically be installed to your local
machine as a distribution, albeit a rather useless one.

Another widely used tool for starting distributions is the more straightfowardly
named, `Module::Starter` which provides more command line options than `h2xs`
and the convenience of using a config file. We will leave it as an exercise for
the reader to find and tinker with these other tools. But it would be worthwhile
to take some time to get familiar with them and examine the files they generate
to enhance your appreciation of what `Dist::Zilla` does for you.

## Distribution Building with `Dist::Zilla's` `dzil` Commands

Now let's take `Dist::Zilla` for a spin now and see how it differs from `h2xs`.
Make sure you have `Dist::Zilla` installed on your machine and run this command
if you don't already have `Dist::Zilla` configured:

`dzil setup`

`Dist::Zilla` will prompt you for your name, email address and ask some basic
questions about how your software will be released. Finally, it will ask you for
your credentials for your PAUSE account. If you don't have a PAUSE account or
don't know what one is, answer "no" and move on. You can always configure this
later. As we'll see, `Dist::Zilla` uses the configuration information you enter
and adds it to the appropriate files in your distribution's files.

### The `dzil new` Command

OK, now we are ready to start our distribution similar to the way we created one
with `h2xs`, by issuing a command:

`dzil new Greetings`

`dzil` will dutifully keep us informed of its progress:

```

[DZ] making target dir /home/Greetings
[DZ] writing files to /home/Greetings
[DZ] dist minted in ./Greetings

```

Much like `h2xs`, `dzil` generated a directory for us and some files inside of
it.  `Dist::Zilla` also reported that it has "minted" a "dist" for us. We'll
come back to this crypticism later. For now, let's plow ahead and open our new
`Greetings` directory and see what magic `dzil` has pulled off for us:

`cd Greetings`

Ouch! There's barely anything in here. Just a mysterious `dist.ini` file and a
`lib` directory with a minimal `Greetings.pm` file inside of that. This doesn't
seem very impressive compared to the `h2xs` tool.

But `Dist::Zilla` works a bit differently than `h2xs`. Its `new` subcommand
isn't designed to immediately build our distribution, it simply sets up a
directory that will eventually store our code and distribution builds. But
before we get ahead of ourselves, let's make our module useful by editing the
`lib/Greetings.pm` module file that `dzil` generated for us and add in our
function:

```

sub hw {
  print "Hello, World!\n";
}

```

And for reasons we don't need to worry about now, we have to add a brief
abstract so `Dist::Zilla` can build our module with this line:

`# ABSTRACT: Quick Greetings for the world`

So your `Greetings.pm` file should look like this:

```

use strict;
use warnings;
package Greetings;

sub hw {
  print "Hello, World!\n";
}

1;
# ABSTRACT: Quick Greetings for the world

```

### The `dzil build` Command

Now we are ready to actually build our distribution by issuing `dzil`'s `build`
command from the top level of our `Greetings` distribution:

`dzil build`

OK! It looks like we are getting somewhere now. The `dist` command has reported
that is has created a new tarball and a directory, `Greetings-0.001` for us.  The
files in this directory are a fully functional build of your module that can
actually be released, distributed, installed on other machines. Take a look
inside the `Greetings-0.001` and now you'll see something that looks much closer
to what we generated with the `h2xs` command.

### Distributing Your Module to Yourself with the `install` Command

The tarball in our directory, `Greetings-0.001.tar.gz`, is the
compressed version of our `Greetings-0.001` conveniently saving us the step of
having to create it ourselves. We can immediately install our new module to our
local perl library with `dzil install` command:

`dzil install`

...and you should see something like this output to the terminal:

```

[DZ] building distribution under .build/NG8bhY4qL6 for installation
[DZ] beginning to build Greetings
[DZ] guessing dist's main_module is lib/Greetings.pm
[DZ] writing Greetings in .build/NG8bhY4qL6
--> Working on .
Configuring Greetings-0.001 ... OK
Building and testing Greetings-0.001 ... OK
Successfully installed Greetings-0.001
1 distribution installed
[DZ] all's well; removing .build/NG8bhY4qL6

```

Nice, now our module is available to use anywhere on our system. So congrats,
you've successfully built your very first distribution with `Dist::Zilla` and
distributed it successfully, even if only to yourself. But feel free to
email the tarball to your friends and astonish them with what your new
module can do. Later in the tutorial, we will show you how to upload your work
to CPAN so you can find an even wider audience for your modules.

You now have a rudimentary understanding of how to use `dzil`, along
with its subcommands, to help you automate the process of generating a
distribution.
