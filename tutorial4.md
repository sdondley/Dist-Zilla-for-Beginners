# Controlling Your Distribution Production Factory with the `dist.ini` File

OK, let's crack open the `dist.ini` file in your favorite text editor and see
what we can break in the name of science and understanding.

First, as much as we appreciate the convenience of bundles, we are control
freaks, so the first thing we are going to do is drop the `[@Basic]` command and
replace it with individual plugin names so we can more precisely layout our
distribution factory's floor plan.

Adding an individual plugin to `dist.ini` is simple. Just take the last part of
the plugin's package name and surround it with square brackets and add it on its
own line in the file. So if the plugin's package full name is
`Dist::Zilla::Plugin::License`, you are going to add `[License]` to a line in
your `dist.ini` file. The order of the plugins is important so make sure you
list the plugins in the same order as the listing in the previous tutorial.

Once you do this for each plugin implemented by the `Basic` bundle, your new
`dist.ini` file will look like this:

```

name  = Greetings
author = Your Name <your@email.com>
license = Perl_5
copyright_holder = Your Name
copyright_year  = 2018

version = 0.001

[GatherDir]
[PruneCruft]
[ManifestSkip]
[MetaYAML]
[License]
[Readme]
[ExtraTests]
[ExecDir]
[ShareDir]
[MakeMaker]
[Manifest]
[TestRelease]
[ConfirmRelease]
[UploadToCPAN]

```

Now let's make a trivial change to our `dist.ini` by throwing a semicolon in
front of our `[License]` plugin listing, like so:

;\[License]

With `.ini` files, this is how you comment out a line. And so we have
effectively taken our robot in charge of slipping license agreements into our
distribution offline. Can you guess what will happen? Let's build the
distribution and find out if you were right. Save the `dist.ini` file and run:

`dzil build`

And now:

`ls Greetings-0.001`

Which should show:

`dist.ini  lib  Makefile.PL  MANIFEST  META.yml  README`

Sure enough, our module lacks a `LICENSE` file containing our license agreement.
Good thing no one reads those things anyway. Of course, some fancy pants,
high-powered lawyer might find a way to bring us to financial ruin over our
careless omission so let's go back in and uncomment our our `[License]` plugin
and then run:

`dzil build`

Now double check just to make sure with `ls Greetings-0.001`:

`dist.ini  lib  LICENSE  Makefile.PL  MANIFEST  META.yml  README`

Awesome. We're back in good legal standing with the software gods.

You'll notice we didn't run `dzil clean` after each build. `Dist::Zilla` will
overwrite existing versions with new distributions so long as they have the same
version number. But as we add new versions, our older distributions will
accumulate in our factory and we will want to sweep them out from time to time.

Speaking of versions, how do we add a new version of our module? Glad you asked!

Let's add an incredible new feature to our `Greetings.pm` module by adding the
following function:

```

sub shout_hw {
  return uc hw();
}

```

And now edit the `dist.ini` file to update the `version` value from `0.001` to
`0.002` and then again run:

`dzil build`

Cool! Now you have a new version of your module to really annoy friends and
family by shouting "HELLO, WORLD!" at them. You'll also notice that version
0.001 is still laying around. You can quickly clean things up with `dzil clean`
and then run `dzil build` again to get version 0.002 back. Then, if you want,
run `dzil install` so the other modules on your machine can take advantage of
your new should funciton.

Are you impressed with `Dist::Zilla`, yet? Maybe if you are brand new
to distribution building you are excited but the truth is this is all pretty
ho-hum stuff to more experienced developers. Don't worry, some more impressive
tricks will soon be covered. We have to ensure everyone can walk before we teach
them to run.

## A Word on Taming the `Dist::Zilla` Beast

At this point, we want to mention that even though `Dist::Zilla` is designed to
automate things for you, it makes few demands on how to do the actual
automation. For example, changing the `dist.ini` file isn't the only way to
change your module's version number. You can use the `[VersionFromModule]`
plugin to get the the version number from your module instead of from
`dist.ini`. Or, you can use the `[AutoVersion]` plugin to help you automatically
generate a version number based on the current date. If you use git to manage
your module's releases, you can use `[Git::NextVersion]` to automatically
generate the next version number in sequential order.

There are hundreds of `Dist::Zilla` plugins out there that can help you automate
your distribution's creation just the way you want. And if there isn't, you can
write your own plugins to satisfy your inner control freak.

But `Dist::Zilla`'s maze of plugins and flexibility is both a blessing and a
curse. It's a blessing for developers who demand precise control over their
distributions while avoiding a lot of tedious work. But it's a curse for
newcomers wrestling with `Dist::Zilla` to get it to do what they want.
`Dist::Zilla` was named after a monster for good reason.

The goal of these tutorials is to try to help ease the pain of learning your way
around `Dist::Zilla` as much as possible. So try to restrain you urge to go full
control freak for now. We still have a lot more basic stuff to cover before you
should unleash yourself.

OK, now that we know more about how to manage our `dist.ini` file, let's dive
down a level deeper and learn how to modify how to gain more control over
how the indvidual plugins do their jobs.