# `[@Starter]` Me Up

A dirty secret in the `Dist::Zilla` community that no one likes to talk about
[(we kid)](http://blogs.perl.org/users/grinnz/2016/07/distzilla---why-you-should-use-starter-instead-of-basic.html)
is that the `[@Basic]` bundle is pretty badly outdated. A better, more modern
bundle to use is the `[@Starter]` bundle. So let's modify our `dist.ini` file by
deleting all the individual plugins we created in the last tutorial and replace
them with:

`[@Starter]`

A big reasons we are having you use `[@Starter]` is that it uses plugins
that are much more configurable than the plugins that ship with `[@Basic]`. And,
at the end of the day, configurability is what `Dist::Zilla` is all about.

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
detail-oriented, software developer.

The `[@Basic]` bundle employed the `[Readme]` plugin to generate and typically
useless `README` file for your distribution. The `[@Starter]` bundle gives you
two different options for helping your create a much more useful `README`
document using either the `[ReadmeAnyFromPod]` plugin or the `[Pod2Readme]`
plugin. Both of these plugins were installed for you when you installed the
`[@Starter]` bundle on your system. By default, `[@Starter]` uses the
`[ReadmeAnyFromPod]` plugin so we'll start with that one.

Go ahead and add the following lines to you `dist.ini` file below your
`[@Starter]` command:

```

[ReadmeAnyFromPod]
type = markdown
filename = README.md

```

Let's take a moment to explain what's going on here. Our first line in brackets
is just like the bracketed text we've already been using for our module
listings. In `.ini` file parlance, it starts a new **section** in our `dist.ini`
file. Below the section are **parameters,** using the standard key/value pair
`.ini` syntax, that will get passed to our plugin. Think of these as commands we
give our `README` insertion robot on the assembly line. As you might have
assumed by the parameters, we are instructing the `[@ReadmeAnyFromPod]` plugin
to generate a `README.md` file with the `markdown` syntax.

But what will it put into our `README` file? Is there some powerful AI that
`ReadmeAnyFromPod` will write it for us? Unfortunately, no. We still have to do
the hard work of documenting our module using Perl's Plain Old Documentation
(POD) because that is what the `[ReadmeAnyFromPod]` will use to generate your
`README` file.

## Starting Your Module's Documentation

We haven't yet written any documentation for our module yet so let's fix that.
We don't want you to have a reputation as a developer too lazy to provide
documentaiton for their work. Add the following documetnation to your
`Greetings.pm` by adding the following lines to the bottom:

```

=pod

=head1 Real Documentation Coming Soon!

We promise!

```

Issue the `dzil build` command and check out the README file now in your
distribution and you should see that your module's POD was inserted into the
README file. Nice. Go bake yourself a well-deserved cookie.

Alright, we have now given you a very tiny taste for gaining more control over
how your plugins work. We'll show you many more powerful and useful tricks
later. For now it's time to take a break from the world of plugins and start
talking about another fundamental area of `Dist::Zilla` knowledge, minting
profiles.
