# A More Useful `Dist::Zilla` Blueprint

You can create an entire library of blueprints customized for the different
kinds of modules and distributions you wish to create. For example, you might
want set up a blueprint that generates boilerplate for Moose modules and another
for standalone perl scripts. You can also set up blueprints that create a git
repository for your module as well as generate a remote repository for you on
GitHub and that will automatically push commits out to it (we'll cover this in
our next tutorial). There's lots of latent power underlying the `new` command so
let's get a better taste for how to tap into it.

Our last blueprint was a trivial one for the purposes of showing you the basics.
This next one might be one you'll want to add to your blueprint library, so you
may want to follow along closely and do all the steps below.

## Drafting a New Blueprint

Our blueprint will be for a distribution containing a simple command. To get
started, we'll copy an existing blueprint that is the most similar to the new
one we want to create. In our case, we only have one other blueprint so we'll
copy that:

```

`cd ~/.dzil/profiles`
`cp -r default command`

```

And now we can go to work making the necessary modifications to our new
`command` blueprint. The first thing we'll do is create the executable command
which, by convention, is usually stored in the `bin` directory of a
distribution.

### Modifying Your Blueprint Copy

How do we design a blueprint that adds a `bin` directory and places a command
inside of it? Good questions. And as we answer them, you'll get to see some of
`Dist::Zilla`'s more powerful templating features.

Open the `profile.ini` file note the study the last section with the following
two lines:

```

[GatherDir::Template]
root = skel

```

The first line in brackets is the name of a plugin, though it's a little
different than what we've seen previously because of the double colon sandwiched
in the name. This syntax means we are using a plugin called `Template` which is
a sublcass of the `[GatherDir]` plugin.

So what exactly does this plugin do?

#### The `[GatherDir]` and `[GatherDir::Template]` Plugins

You might recall the `[Gatherdir]` plugin from when you manually added the
individual plugins used by the `[@Basic]` bundle to the `dist.ini` file. The
`[GatherDir]` plugin is in all `dist.ini` files because it plays a critical role
in the distribution generation process.

When we issue the `dzil build` command, the `[GatherDir]` gathers the files from
your work area and place them on the assembly line. It does a similar job when
we issue the `dzil new` command except it adds files from a directory on your
hard drive–usually wihtin your blueprint directory–and adds them to your work
area. Before saving them there, though, `[GatherDir]` stores the files in your
computer's memory in case they need more processing.

The `Template` subclass tells `Dist::Zilla` to treat the collected file like
templates and, if any variables are found inside of our files, to replace them
with the appropriate string. You'll see this in action shortly.

#### Adding a Skeleton Directory to Your Module

The `root` parameter in the `[GatherDir::Template]` tells the plugin which
directory to gather the files from. In this case, the `skel` directory inside
our blueprint directory. There is nothing special about the "skel" name which
is short for "skeleton." We could call the directory anything we want.

But the `skel` directory doesn't exist yet so let's fix that. Making sure you
are inside the `command` blueprint directory, issue this command:

`mkdir skel`

Now we are going to add our `bin` directory inside of our `skel` directory. As
mentioned, the `bin` directory is where our module's command will go.

`mkdir skel/bin`

#### Adding the Command Template

Next, we create the command's template file. Open a new file add the following
lines to it:

```

#!/usr/bin/perl

use {{$dist->name}};
{{$dist->name}}->run;

=head1 NAME

{{$dist->name}} - Add the command abstract here

```

Take a moment to study this code. First, you'll notice we've got some variables
inside a double set curly braces. This is the syntax used by the templating
system `Dist::Zilla` uses to identify string substitutions. Both the curly
braces and the `$dist->name` variable will be replaced with the distibution name
argument passed to the `dzil new` command. As mentioned, these substitutions are
performed by our `[GatherDir::Template]` plugin. In case you're wondering,
`$dist` is the `Dist::Zilla` object overseeing everything and `name`, of course,
is ad method for generating the name of our distribution.

The module we will soon create makes use of the `App::Cmd::Simple` module which
supplies the `run` method we see in the command file. If you are interested in
how the `App::Cmd::Simple` command works, you should refer to its documentation.

OK, now save the command file to `skel/bin/the_command`. `the_command` file name
is arbitrary and acts as a placeholder in our blueprint. When it comes time to
process our blueprint, we want the name of this file to change to the name of
our command name.

#### Changing the Command Name

To make that happen, we will use of the the `rename` parameter that
`[GatherDir::Template]` provides. Reopen your `profile.ini` command and add the
following line to the end of the `[GatherDir::Template]` section and save the
file:

`rename.the_command = $dist->name`

This snippet tells the plugin to change the name of any file called
`the_command` to the distribution name that gets passed to our `dzil new`
command which we will soon do.

### Modifying the Module Template

OK, just one task left and that's to modify the blueprint's module template
file. Replace the existing `Module.pm` file in the `command` blueprint
directory with the following code:

```

package {{$dist->name}};
use strict;
use warnings;
use base qw(App::Cmd::Simple);

sub opt_spec {
  return (
    [ "option1|a",  "do option 1" ],
  );
}

sub validate_args {
  my ($self, $opt, $args) = @_;

  # no args allowed but options
  $self->usage_error("No args allowed") if @$args;
}

sub execute {
  my ($self, $opt, $args) = @_;

  if ($opt->{option1}) {
      # do option 1 stuff
  } else {
      # do regular stuff
  }
}

1;

=head1 NAME

{{$dist->name}} - Add the module abstract here

```

Notice, again, the use of the `{{$dist->name}}` in our template. As for how the
rest of the code works, we encourage you to take a look at the
`App::Cmd::Simple` documentation.

## Set Up Your Work Area with the `new` Command and the `-p` Argument

It's time to see if you accurately followed instructions. Let's try to generate
a new work area with our blueprint. Jump to a new empty directory and issue the
following command:

`dzil new sayhi -p command`

This tells `dzil` to set up a new module work area with the distribution name
"sayhi".  The `-p command` option tells it to use our `command` blueprint, the
one we just created, to generate our new work area.

After running this command, you'll see the `sayhi` work area set up in your
directory and no errors. If not, take a close look at any errors `Dist::Zilla`
generated and try to figure out what you have to do to get things working.

## Getting `sayhi` to Say "Hi"

`Dist::Zilla` has generated our template and skeleton files, now we just need to
make some trivial changes to our module to get it functional. Fire up your text
editor to edit the `lib/sayhi.pm` module and add/modify the following lines (or
just cut and paste the entire code listing that follows):

* add `use Greetings;` near the top
* in the `opt_spec` function, replace all instances of `option1|a` with `shout|s`
* replace `do option 1` to `shout it`
* change `$opt->{option1}` to `$opt->{shout}`
* replace `#do option 1 stuff` with `&Greetings::shout_hw;`
* replace `#do regular stuff` with `&Greetings::hw;`
* change `Add the module abstract here` to `Backend interface for "sayhi"
  command`

And here is the entire finished module:

```

package sayhi;
use strict;
use warnings;
use Greetings;
use base qw(App::Cmd::Simple);

sub opt_spec {
  return (
    [ "shout|s",  "shout it" ],
  );
}

sub validate_args {
  my ($self, $opt, $args) = @_;

  # no args allowed but options!
  $self->usage_error("No args allowed") if @$args;
}

sub execute {
  my ($self, $opt, $args) = @_;

  if ($opt->{shout}) {
    &Greetings::shout_hw;
  } else {
    &Greetings::hw;
  }
}

1;

=head1 NAME

sayhi - Add the module abstract here

```

That's it! You can now build your new module:

`dzil build`

If that went well, install your distribution with:

`dzil install`

Hopefully you also installed the `Greetings` module earlier so you can actually
put your command to good use printing "Hello, World!" to your heart's content
right from the command line. To print a standard greeting, do `sayhi` from the
command line. To shout it, do `sayhi --shout` or `sayhi -s`.
