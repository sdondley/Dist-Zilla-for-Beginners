# A More Useful `Dist::Zilla` Blueprint

You can create an entire library of blueprints customized for the different
kinds of modules and distributions you wish to create. For example, you might
want set up a blueprint that generates boilerplate appropirate for a Moose
module or one for distributing a standalone perl script. You can also set up
blueprints that create a git repository for your module as well as generate a
remote repository for you on GitHub and that will automatically push commits out
to it (we'll cover this in our next tutorial). There's lots of latent power
underlying the `new` command so let's get a better taste for how to tap into it.

Our last blueprint was a trivial one for the purposes of showing you the basics.
This next one might be one you'll want to add to your library, so you may want
to follow along closely.

## Drafting a New Blueprint

Setting up additional blueprints is easy. Let's say we want to create a new
blueprint appropriate for distributions with a simple command. Like before, we
first have to create the directory for storing our blueprint. We'll copy an
existing blueprint that is hopefully close to the new one we want to create. In
our case, we only have a `default` blueprint so we're forced to start with that
one:

```

`cd ~/.dzil/profiles`
`cp -r default command`

```

And now we can go to work making the necessary modifications to our new
`command` blueprint. The first thing we'll do is create the executable command
which, by convention, is usually stored in the `bin` directory in a of your
distribution.

### Modifying Your Blueprint Copy

How do we design a blueprint that adds a `bin` directory and places a command
inside of it? Good questions. You'll get to see some of `Dist::Zilla`'s more
powerful templating features as we reveal the answer.

To answer these questions, we'll start by taking a look inside the `profile.ini`
file.  The last section of the file has the following lines:

```

[GatherDir::Template]
root = skel

```

The first line in brackets is the name of a plugin, though it's a little
different than what we've seen previously because of the double colon sandwiched
in the name. This syntax mean we are using a plugin called `Template` that's a
sublcass of the `[GatherDir]` plugin.

So what exactly does this plugin do?

#### The `[GatherDir]` and `[GatherDir::Template]` Plugins

You might recall seeing the `[Gatherdir]` plugin when we manually added all the
individual plugins used by the `[@Basic]` bundle to our `dist.ini` file. This
plugin is going to be in all `dist.ini` files because it plays such a crucial
role in the distribution generation process.

When we issue the `dzil build` command, the job of the `[GatherDir]` plugin is
to gather files from your work area and place them on the assembly line. It does
a similar job when we issue the `dzil new` command except it adds files from
directory on your hard drive, usually from your profile, and adds them to your
work area. Before they get there, though, `[GatherDir]` stores files in your
computer's memory for in case they need more processing.

The `Template` subclass tells `Dist::Zilla` to treat the collected file like
templates and, if any variables are found inside of our files, to replace them
with the appropriate string. You'll see this in action shortly.

#### Adding a Skeleton Directory to Your Module

The second line in the `[GatherDir::Tempalte]` tells the plugin where to gather
the files from by passing a value to the `root` parameter. In this case, the
value we are passing is the `skel` directory inside of our blueprint directory.
There is nothing special about the "skel" name which is short for "skeleton." We
could call the directory anything we want.

But the `skel` directory doesn't exist yet so let's fix that. Making sure you
are inside the `command` blueprint directory, issue this command:

`mkdir skel`

Now we are going to add our `bin` directory inside of our `skel` directory. The
`bin` directory is where we are going to put our module's command.

`mkdir skel/bin`

Next, we are going to create the template file for our command. Open a new text file
add the following lines to a new file:

```

#!/usr/bin/perl

use {{$dist->name}};
{{$dist->name}}->run;

=head1 NAME

{{$dist->name}} - Add the command abstract here

```

Let's take a moment to study this file. First, you'll notice we've got what
looks like variables inside a double set curly braces. This is the syntax used
by the templating system `Dist::Zilla` uses to identify variables. Both the
curly braces and the `$dist->name` variable will be replaced by the name of the
distibution name we supply to the `dzil new` command when we set up our work
area. As mentioned, these substitutions are performed by our
`[GatherDir::Template]` plugin. In case you're wondering, `$dist` is the
`Dist::Zilla` object overseeing everything and `name`, of course, is the method
that generates the name of our distribution.

We will also note that the modle we will soon create makes use of the
`App::Cmd::Simple` module which supplies the `run` method we see in the command
file. If you are interested in how the `App::Cmd::Simple` command works, you
should refer to its documentation.

OK, now save the command file to `skel/bin/the_command`. `the_command` file name
is arbitrary and is just acting as a placeholder for us in our blueprint. When
it comes time to process our blueprint, we want to change the name of this file,
to the name of our actual command. To make that happen, we will use of the the
`rename` parameter that `[GatherDir::Template]` provides.

#### Changing the Command Name

Open up your `profile.ini` command and add the following line to the end of the
`[GatherDir::Template]` section:

`rename.the_command = $dist->name`

This snippet tells the plugin to change the name of any file called
`the_command` to the distribution name that gets passed to our `dzil new`
command which we will soon do.

### Modifying the Module Template

OK, just one task left and that's to modify the blueprint's module template
file. Replace your existing `Module.pm` file in the `command` blueprint
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
a new work area with our blueprint using the following command:

`dzil new sayhi -p command`

This tells `dzil` to set up a new module with the distribution name `sayhi`.
The `-p command` option tells it to use our `command` blueprint, the one we just
wrote, to generate our new work area.

With any luck, you'll see the `sayhi` work area set up in your directory and no
errors. If not, take a close look at any errors `Dist::Zilla` generated and
try to figure out what you have to do to get things working.

## Getting `sayhi` to Say "Hi"

`Dist::Zilla` has generated our template and skeleton files, now we just need
to make a few trivial changes to our module to get it to actually do something.
Fire up your text editor to edit the `lib/sayhi.pm` module and add/modify the
following lines (or just cut and paste the entire code listing that follows):

* add `use Greetings;` near the top
* replace all instances of `option1` with `shout`
* replace `do option 1` to `shout`
* change `$opt->{option1}` to `$opt->{shout}`
* replace `#do option 1 stuff` with `&Greetings::shout_hw;`
* replace `#do regular stuff` with `&Greetings::hw;`
* change `Add the module abstract here` to `Backend interface for "sayhi"
  command`

And here is module with the changes noted above:

```

package sayhi;
use strict;
use warnings;
use Greetings;
use base qw(App::Cmd::Simple);

sub opt_spec {
  return (
    [ "shout",  "shout" ],
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

That's it!  You can now build your new module:

`dzil build`

Hopefully you installed the `Greetings` module earlier so you can actually put
your command to good use printing "Hello, World!" to your heart's content.
