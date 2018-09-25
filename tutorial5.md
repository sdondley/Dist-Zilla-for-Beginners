# `[@Starter]` Me Up

A dirty secret in the `Dist::Zilla` community that no one likes to talk about
[(we kid)](http://blogs.perl.org/users/grinnz/2016/07/distzilla---why-you-should-use-starter-instead-of-basic.html)
is that the `[@Basic]` bundle is pretty badly outdated. A better, more modern
bundle to use is the `[@Starter]` bundle. So let's modify our `dist.ini` file by
deleting all the individual plugins we created in the last tutorial and replace
them with:

`[@Starter]`

A big reason we are having you use `[@Starter]` is that it uses plugins that are
much more configurable than the plugins that ship with `[@Basic]`. And, at the
end of the day, configurability is what `Dist::Zilla` is all about.

But the `[@Starter]` plugin bundle is not part of the `Dist::Zilla` distribution
so we have to install it on our system from CPAN. If you use `cpanm`, simply
run:

`cpanm Dist::Zilla::PluginBundle::Starter`

Once installed, build your module again with the standard command:

`dzil build`

After the command finishes, take a peek inside your distribution's directory and
you'll probably notice some differences compared to what the `[Basic]` plugins
generated.

First, we now have a `META.json` file which CPAN will use to help tell the world
about your module. And if you open the `README` file, you'll now see that it's
completely blank. Did we break something? Not at all. We'll tackle this problem
in a moment. `[@Starter]` also makes a lot of other technical changes to the
process that we aren't necessary to cover here. You should refer to
`[@Starter]`'s documentation for the nitty gritty details.

Suffice it to say here that changing the bundle can drastically change how your
module gets packaged by using different machines (plugins) on the assembly line.
As a result, even though we started with the same raw product coming from our
workbench area, our product was wrapped differently. Makes sense, right?

## Supplying a `README` File Using `[@Starter]`

No one except true, professional, detail-oriented software developers will read
it, but just like a `LICENSE` file, it's important to include a `README` file
with your distribution so you'll look like a true, professional,
detail-oriented software developer.

The `[@Basic]` bundle employed the `[Readme]` plugin to generate a typically
useless `README` file for your distribution. The `[@Starter]` bundle gives you
two different options for helping your create a much more useful `README`
document using either the `[ReadmeAnyFromPod]` plugin or the `[Pod2Readme]`
plugin. Both of these plugins were installed for you when you installed the
`[@Starter]` bundle on your system. By default, `[@Starter]` uses the
`[ReadmeAnyFromPod]` plugin so we'll start with that one.

But what will the `[Starter]` bundle put into your `README` file? Is there some
powerful AI underlying `ReadmeAnyFromPod` to write it for us? Unfortunately,
no. We still have to do the hard work of documenting our module using Perl's
Plain Old Documentation (POD) because that is what the `[@Starer]` bundle uses
to generate your `README` file.

## Starting Your Module's Documentation

We haven't yet written any documentation for your module yet so let's fix that.
We don't want you to have a reputation as a lazy developer that doesn't document
their work. Add the following documetnation to your `Greetings.pm` by adding the
following lines to the bottom:

```

=head1 NAME

Greetings - Quick Greetings for the world

More documentation coming soon, we promise.

```

Interestingly, now that we have added the `NAME` section to our documentation,
`Dist::Zilla` can successfully build our module without the `# ABSTRACT` comment
that we had you create in the first tutorial. Feel free to delete that comment
if you are so inclined.

Alright, now issue the `dzil build` command and check out the README file now in
your distribution and you should see that your module's POD was inserted into
the README file. Nice. Go bake yourself a well-deserved cookie.

## Double Your Fun and Pleasure with Two `README` Files

So now you've got a plain old text file for reading your module's plain old
documentation. Kind of boring. All the cool kids are using GitHub these days and
the preferred format for `REAMDE` files there is markdown. We don't want you
looking like a stick in the mud, so let's hook you up with a fancy markdown
version of your `README` file by adding the following to the end of your
`dist.ini` file:

```

[ReadmeAnyFromPod]
type = markdown
filename = README.md

```

Run the `build` command:

`dzil build`

Now take a look inside your distribution. Awesome, you now have a plain text
`README` file and a fancier, markdown version `README.md` automatically
generated for you without having to know a lick of markdown syntax.

Let's take a moment to reflect on what we added to our `dist.ini` file. The
first line in brackets is, of course, the name of the plugin. In `.ini` file
parlance, bracketed text starts a new **section** in our `dist.ini` file.

Below and within this section we supplied two **parameters,** using the standard
key/value pair `.ini` syntax. Because they are in the section our plugin is
named after, they got passed to our plugin. Think of the parameters as custom
commands we are giving to our `README` insertion robot on the assembly line. As
you might have assumed by looking at the parameters, we are instructing the
`[@ReadmeAnyFromPod]` plugin to generate a `README.md` file using the `markdown`
syntax.

As we saw earlier, the `[@Starter]` automatically generated the plain text
`README` file using the `[ReadmeAnyFromPod]` plugins. So what we are doing here
is telling `dist.ini` to run the `[ReadmeAnyFromPod]` plugin a **second time**
to generate the markdown version of our `README.md` file.

But what if you don't want to ship the `README.md` file to CPAN? No problem! You
can direct `[ReadmeAnyFromPod]` to save the markdown version to the top level of
your `Dist::Zilla` directory instead of inside your distribution by adding the
following line to the `[ReadmeAnyFromPod]` section of your `dist.ini` file:

`location = root`

Try it out and run the `build` command and you'll see your `README.md` file
output at alongside your distribution's directory instead of inside it:

`dist.ini  Greetings-0.002  Greetings-0.002.tar.gz  lib  README.md`

Alright, we have now given you a very tiny taste for how to gain more control
over how your plugins work. We'll show you many more powerful and useful tricks
later. Now it's time to take a break from the world of plugins and start
talking about another fundamental area of `Dist::Zilla` knowledge called
"profiles."
