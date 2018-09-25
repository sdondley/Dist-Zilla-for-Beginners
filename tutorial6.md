# `Dist::Zilla` ~~Minting Profiles~~ Blueprints

We've all been there. We fire up a text editor with the dreaded blank screen staring
back at us and we start dutifully typing:

`!#/usr/bin/perl`

Oops. No wait. SHA-bang...

```

#!/usr/bin/perl
use strict;
use warnings;
use Carp;

# plus the other usual boilerplate

```

You've done it 100 times and 95 times you have throught to yourself "I really
need to automate this." If you consider yourself a decent Perl developer, you've
probably already using some kind of templating system for kicking off a project.
Some are good, many of them suck.

The good news is that `Dist::Zilla` has one of the most powerful, customizable
templating systems available. The bad news is they are called **minting
profiles.** We hate the name and think the term is confusing. Presumably the
term's aim is to get us thinking about "minting" coins. In any case, we will
avoid using the official terminology where possible in the hopes the analogy we
use to understand how `Dist::Zilla` works catches on.

## A Better Way to Think About the "Minting Profiles"

In an earlier tutorial, we used the analogy of a "workarea." To refresh your
memory, we imagined that the `dzil new` command set up a new workarea on our
factory floor where we would forge our module. Each time you issue the `new`
command, you set up a new workarea in a new directory on your computer for
creating and distributing a new module.

But it gets even better than that. You can control not just what your workarea
will initially look like but what manufacturing process will be used to pakcage
your module. In other words, you can control which plugins go into your
`dist.ini` file. This is a powerful feature of `Dist::Zilla` that takes you well
beyond what you can do with a more typical distribution generation systems using
plain templates.

None of this really sounds like "minting" to us.

So instead of "minting profiles," we encourage you to think of them instead as
"blueprints" as it better captures the essence of what the `new` command does:
sets up and configures your module and the processes that will work on it
according to your plan. And wherever we have to use the term `profile` below,
replace it **mentally** with the word `blueprint` instead. Of course, if we have
you type in `profile`, make sure you actually type in `profile.`

## Creating Our First Factory Blueprint

We first need to esablish a storage area for our default blueprint. The default
blueprint is what's used unless we specify a custom blueprint.

First, create a default directory:

`mkdir -p ~/.dzil/profiles/default`

Inside the default directory, you're going to create a `profile.ini` file (which
we cannot call `blueprint.ini`). The primary job of this file is to tell
`Dist::Zilla` how to establish our workarea. The file works very similarly to
how our `dist.ini` file works. But instead of processing our module, it helps us
establish the template for our main module, any other supporting files, and
helps assemble the `dist.ini` file.

Create the following `profile.ini` file in the `default` directory:

```

[TemplateModule/:DefaultModuleMaker]
template = Module.pm

[DistINI]
append_file = plugins.ini

[GatherDir::Template]
root = skel

```

We can see that we have three sections in our `profile.ini` file, one for each
plugin that we use and we pass in a parameters to each plugin. Don't worry
exactly what these mean just yet. We will explain it in more detail later.

We also need another `.ini` file, `plugins.ini` as part of our blueprint. Create
it and add the following lines to it, which should look familiar to you:

```

[@Starter]
[ReadmeAnyFromPod]
type = markdown
filename = README.md
location = root

```

If you guessed `plugins.ini` will end up inside our `dist.ini` file, you guessed
right.

Lastly, we need to add a file that will act as a template for our new module.
Create a file called `Module.pm` and add the following to it:

```

package {{$name}};
use strict;
use warnings;

# Module implementation goes here

1;

=head1 NAME

{{$name}} - Add the module abstract here

```

You shoud now have three files in the `default` directory. Together, they
comprise your first factory blueprint. Let's see if you cut and paste everything
accurately. Create a new, empty directory and try creating a new workarea:

`dzil new Super::Greetings`

If you see a new `Super-Greetings` directory and `Dist::Zilla` didn't throw any
errors at you, congratulations, you've successfully ~~minted~~ drafted and
processed your first ~~profile~~ blueprint. If something went wrong, go back and
inspect your three blueprint files for errors and try again.
