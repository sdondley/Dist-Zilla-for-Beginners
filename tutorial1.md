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
advantage of its goodness. In other words, you want to create a distribution
for your module. But where do you start?

## Tools for Helping You Get Your Distribution Started

Well, there is the hard way and then there are far easier ways. The hard way
involves manually creating all the directories and manually typing in all the
files, tests, installers, documentation, meta files, etc. that go into creating
a distribution from scratch. No one but a complete masochist will enjoy creating
a module like this.

Fortunately, there are already many tools available to automate the creation of
distributions. For example, you can break out the `h2xs` command line tool that
ships with Perl to help you generate these files. Go into an empty directory on
your hard drive and run the following command:

`h2xs -AX -n Greetings;`

Take a quick peek inside the resultant `Greetings` directory and you'll see the
command has generated many of necessary features of a proper software
distributions for you. It set up a directory structure, added documentation,
created build tools for you, and so forth. In short, the command has set up a
bare-bones or "skeleton" distribution for us that we can edit to add more more
tests, code, documentation, etc.

There are other options out there for starting your distribution. Another
popular command line tools is `Module::Starter` which is essentially a more
sophsticated version of `h2xs` and has more advanced options. We will leave it
as an exercise for the reader to play with these automation tools. But you should
definitely take some time to get familiar with them and take a look at the files
they generate to help enhance your understanding of `Dist::Zilla`'s
method for helping you generate your distribution.

## Distribution Building with `Dist::Zilla's` `dzil` Commands

So let's take `Dist::Zilla`'s for a spin now and see how it's differs from these
other tools. Make sure you have `Dist::Zilla` installed on your machine and run
this command if you don't already have `Dist::Zilla` configured:

`dzil setup`

`Dist::Zilla` will prompt you for your name, email address and ask some basic
questions about how your software will be released. Finally, it will ask you for
your credentials for your PAUSE account. If you don't have a PAUSE account or
don't know what one is, answer "no" and move on. You can always configure this
later. As we'll see, `Dist::Zilla` uses the configuration information you enter
and adds it to the appropriate files in your distribution's files.

OK, now we are ready to start a distribution similar to the way we created one
with `h2xs`, by issuing a command:

`dzil new Greetings`

`dzil` will dutifully keep us informed of its progress:

```
[DZ] making target dir /home/Greetings
[DZ] writing files to /home/Greetings
[DZ] dist minted in ./Greetings
```

Much like `h2xs`, `dzil` generated a directory for us and some files inside of it.
`Dist::Zilla` also reported that it has "minted" a "dist" for us. This is rather
cryptic but we'll come back to this later. For now, let's open up our new
`Greetings` directory and see what magic `dzil` has pulled off for us:

`cd Greetings`

Ouch! There's barely anything in here. Just a mysterious `dist.ini` file and a
`lib` directory with a minimal `Greetings.pm` file inside of that. Not too
impressive compared to what even a crude tool like `h2xs` did for us, is it? We
still have a long way to go to get our distribution in shape.

Ah, but `Dist::Zilla` works quite a bit differently than `h2xs`. The `new`
subcommand we issued doesn't actually build our distribution, it simply set up a
directory that will eventually hold all of our distribution builds. But before
we get ahead of ourselves, let's make our module useful by editing the
`lib/Greetings.pm` module file that `dzil` generated for us:

```
sub hw {
  print "Hello, World!\n";
}
```

And for reasons we don't need to worry about now, we need to add the following
line somewhere in the module so `Dist::Zilla` can build our module:

`# ABSTRACT: Quick Greetings for the world`

Your `Greetings.pm` file should look like this:

```
use strict;
warnings;
package Greetings;

sub hw {
  print "Hello, World!\n";
}

1;
# ABSTRACT: Quick Greetings for the world
```

Now we are ready to actually build our distribution by issuing `dzil`'s `build`
command from the top level of our `Greetings` distribution like so:

`dzil build`

OK! It looks like we are getting somewhere now. The `dist` command has reported
that is has created a new tarball and a directory, `Greeting-0.001` for us. These
files are functional distributions that can actually be installed. Take a look
inside the `Greetings-0.001` and now you'll see something that looks much closer
to what we generated with the `h2xs` command.

The tarball now sitting in the directory, `Greeting-0.001.tar.gz`, is the
compressed version of our `Greetings-0.001` directory which saves us the step of
having to create it ourselves.  We can immediately install our new module to our
local perl library with `cpanm` by running:

`cpanm Greetings-0.001.tar.gz`

...and you should see something like this output to the terminal:

```
Fetching file:///home/Greetings/Greetings-0.001.tar.gz ... OK
Configuring Greetings-0.001 ... OK
Building and testing Greetings-0.001 ... OK
Successfully installed Greetings-0.001
1 distribution installed
```

Nice, now our module is available to use anywhere on our system. So congrats,
you've successfully written your very first distribution with `Dist::Zilla` and
distributed it successfully, even if only to yourself.

There is certainly a lot more to learn but you now have the basic understanding
that you use `dzil`, along with its subcommands, to help you automate the
process of generating a distribution.
