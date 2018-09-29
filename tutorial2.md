# Major Dist::Zilla Commands for Getting the Job Done

Now that we've gotten our hands good and grimy operating some of `Dist::Zilla`'s
machinery, let's zoom out a bit and take a factory floor level look at
`Dist::Zilla` with a quick overview of the major commands `Dist::Zilla`
provides.

## Understanding the `dzil new` Command

After installing and configuring `Dist::Zilla`, we issued the `dzil new` command
to get our process started. If we think of `Dist::Zilla` as a module
distribution production factory, then our `new` command is like establishing a
new work area on the factory floor where we assemble the raw product (our module)
before packaging and shipping it.

The work area we set up for our simple `Greetings` module used `Dist::Zilla`'s
default profile that, as we saw, was very sparse and bare-bones. We will learn
how to teach `Dist::Zilla` to establish highly customized work areas by, in the
jargon of `Dist::Zilla`, "minting a custom profile." More on this later.

## Understanding the `dzil build` Command

After finishing our simple, one-function module, we issued the `dzil build`
command. As we saw in the last tutorial, the `build` command generates a copy of
your distribution and places a copy of it in your work area along with a
tarballed version of it.

A good way to think about the `build` command is to imagine it as an assembly
line. Your raw product, the module, is loaded onto the beginning of the assembly
line. As your module moves down the line, an ordered series for robots, or
**plugins,** work their magic to transform your module into a finished, fully
packaged distribution at the end of the line that's ready for shipping.

Many plugins come pre-packaged with `Dist::Zilla` but there are hundreds more
available on CPAN. You can also write your own plugins to help build your
distribution in highly speciaized ways. Plugins are an important topic and we
will be exploring them in more detail soon.

## The `dzil release` Command

We didn't issue this command in the first tutorial but this is the command that
will "box" and "ship" your finished distribution to whatever destination you
want to deliver it to. Common destinations include a GitHub repository and CPAN.
As we'll see, you can customize this process just like you can the `build`
process. And similar to the `build` process, the `release` process relies on a
series of discrete plugins to help us get our product out the door. We'll cover
this command much later in our tutorial series.

## The `dzil test` Command

This is another command we didn't cover in the first tutorial but as you might
guess, it's used to run the tests on your module. This command will come in
handy as as you develop your module to see if it passes the tests. We will
discuss this command as well as automated testing in future tutorials.

## The `dzil install` Command

As we saw in the previous tutorial, we can use this command to install our
distribution on to our local machine to make it available to our other modules.

## The `dzil clean` Command

No one likes working in a dirty environment so it's a good idea to sweep away
all the debris that accumulates on your shop floor while you work. Go ahead and
issue this command now to see what happens:

`dzil clean`

Both the tarball file and the distribution directory are now
gone and only the files we had after issuing the `new` command are left behind.
But don't worry, we can easily get them back if we need them.

## Other `dzil` Commands

There are other, minor `dzil` subcommands but since this is a tutorial and not a
manual, we will encourage you to take some time and explore the other commands
with `dzil --help` and `dzil <subcommand> --help`. We will cover some of these
other command later in the tutorial.

We now turn our attention to a very immportant topic, the `dist.ini` file.
