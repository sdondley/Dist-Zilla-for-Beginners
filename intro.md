# A 35,000 Foot View of `Dist::Zilla` and Software Distributions

## Introduction

The `Dist::Zilla` module helps us create software distributions for perl
modules. What exactly does that mean, however? Most of us are generally familiar
with the concept of a “software distribution” but don’t give it much more than a
passing thought. So let’s briefly explore this concept to lay the groundwork for
a discussion specific to Perl module distributions to figure out where the
`Dist::Zilla` module fits into the universe from a jet plane's perspective.

## In the Beginning...

At the dawn of the information age, the best way to move computer code around
the physical world was with dead trees. You typed your code into a mechanical
keypunch machine to generate paper punch cards that you bundled into boxes and
walked down to the computer room to load them into the hulking machine that
magically translated the holes punched into the cards into executable,
electronic signals to perform your desired computing task. You were likely the
only person to ever run your code, the only person likely to install it, and
there was only one machine your code would probably ever run on. And so although
crude, dead trees helped get the job of software distribution done just fine
under the given circumstances.

## Modern Software Distribution Makes More Demands on Developers

As technology evolved, more efficient data storage mediums arrived in the form of
magnetic tapes and disks, making it much easier to copy, reproduce and
distribute computer code. Magnetic media made commercially available software
available to a mass audience for the first time. You could drive to your local
computer store, plunk down 40 bucks and receive a box with some 5 1/4” floppies
inside containing the text adventure game, Zork, and then trek back to
deliver the code to your awaiting microcomputer. If you disovered when you got
back home that you accidentally bought the disks for a Commodore 64 when you
owned an Apple ][e, you had to go back to the store and make an exchange.

Then, of course, the internet revolutionized how we distributed software.
Physical media has become invisible to us and exists only as an abstract notion
we refer to as “the cloud.” We can now easily download software with a few taps
on our device screens while resting in our easy chairs in the ugliest clothes
imaginable.

### Helping Users Install Your Software

Though technology has helped us overcome the physical challenges of distributing
software, that is only part of the battle. It usually isn’t enough to give
someone a copy of the computer code you have written. You'll want to make sure
it's easy for others to install and run your software.

Most users won't know the first thing about all the obscure commands required to
get their copy of your software loaded onto their machine so it can be executed.
And even if they knew the commands to run, it’s a boring task and one that can
be automated. And so almost all software today of any substantial complexity is
accompanied by “installers,” programs designed to make it easy–or at least as
easy as possible–to install and run your computer code on another machine.

So we have to be sure to accompany out software with other software necessary to
install, configuring and running the software on the end user’s machine.

### Getting Your Code to Run on Different Machines

Unlike in the age of punch cards, your perl module probably isn't intended to be
run just by you on your one computer. So your software distribution should also
supply a suite of tests to ensure that your module will work as advertised on
computers that could be much different from yours. You can program the installer
to take the appropriate action if there is a problem with any of the tests.
Hopefully, the problem can be entirely resolved by the installer automatically
or at least allow for partial installation of your software. The installer
should alert the users if any problems were encountered and help users fix them,
if possible.

### Helping Users Find and Use Your Software

Our more modern age of computing has also introduced less obvious jobs for
software distributions but that are just as important if you expect your
software to be widely adopted. First, you'll want to help users discover your
software. If it's sitting on some obscure server no one knows about, it won't
get used. Similarly, if your software has no manual and users have to guess at
how it's supposed to work, users will likely get frustrated and never run it
again.

So another big part of a software distribution’s job is to help users both
discover and use your creation.

### Helping Users Update Their Software

Finally, your distribution should have a way for users to update to the latest
version of your software so they can take advantage of bug fixes and new
features. All but the most trivial or extremely mature software programs need to
constantly improve to stay relevant and keep users happy. And developers have to
keep in mind that if it’s a pain for user to update the software, it will likely
get abandoned.

### Summary of Key Sofftware Distribution Functions

Now we have a clearer picture of what a software distribution is and does. It
should contain the following six key components and features:

* the actual software the end user will run
* a way for users to discover your software and get a copy of it
* an installer to load the software onto a user’s machine and make it easy to
 execute
* tests to ensure your software will actually run
* documentation for using the software
* a convenient way to update the software

Let’s turn now to a more specific discussion about perl module distributions and
the role `Dist::Zilla` plays in the creation of these distributions.

## Perl Gives You the Tools for Building Your Distribution

Let's say you’ve written a Perl module that can shave people's armpits. Although
writing your software was probably a herculean task, you are still missing 5 out
of the 6 essential elements for creating your distribution. Until you build the
software distribution, your software is probably destined for failure (unless
your module really can shave armpits).

Fortunately, the Perl community already provides many tools for helping you
build a complete software distribution that will save you tons of time, money
and aggravation.

First and foremost, there is the free-to-use CPAN (the Comprehensive Perl
Archive Network) website which developers can use to distribute the latest
releases of their Perl modules. CPAN helps you raise awareness about your module
by providing a well-known, publicly available and searchable interface for
reading your module’s documentation, source code and other useful information
about your module to help users determine if your module suits their needs.
Then, to help document your module, you can leverage the inline documentation
feature, know as Plain Old Documentation (POD) that the Perl language supplies,
to display your documentation on the CPAN website as well as offline to your
users on their local machines. Perl also has a suite of tools, called build
systems, to help you automate the process of creating installation software for
your module. And perl has another set of tools and modules for creating and
running tests to help ensure your module will actually work as designed on your
machine and others. Finally, the Perl community has a large team of volunteer
“smoke testers” that will automatically download your module from CPAN, install
it and test it on a wide variety of different kinds of machines and report their
findings to you. All of this is available to you for free. Amazing!

## Software Distributions Still Require Lots of Work

That’s not to say that finishing your distribution will be easy, especially if
you want to deliver high quality software. Writing clear, concise documentation
will always be hard work. Writing tests that ensure your software works well and
will run across many different platforms will always have some challenges. And
AI is nowhere close to being able to replace all the conscientious effort that
goes into  maintaining and updating your distribution with regular fixes and
improvements. But the infrastructure and tools Perl provides will at least make
these important tasks much more achievable by mere mortals.

But even with all the helpful tools the Perl community provides, creating a
software distribution still involves a lot of tedious work and will require you
to keep on top of important bookkeeping tasks. For example, there are many
requirements for building the files CPAN wants to see in your distribution. A
lot of your documentation will probably have boilerplate language that you’ll
want to copy from other sources and paste into your document. You’ll also need a
ensure you update your software version numbers. You’ll probably want to
maintain your module in a git repository and coordinate releases to the
repository with your copy of the module on CPAN. And before uploading your
modules to CPAN, you’ll want to be sure to remember to test your module. And
don’t forget to update your CHANGE log, documentation, and tests to keep up with
new features.

You get the idea. There are dozens, if not hundreds, of little chores that
you’ll need to perform if you wish to release a software distribution of good
quality. Though none of these tasks are particularly hard, few of them are
interesting. And collectively, they can be a real burden and intimidating for
newcomers to the world of perl module distributions. And if you've written
dozens of different modules instead of just your magical armpit shaver, you are
in for a world of pain.

## Dist::Zilla Makes Producing Perl Module Software Distributions Easier

With that understanding, we can begin to appreciate what the `Dist::Zilla`
module brings to the table. Its goal is to help developers make the creation of
perl module distributions much less tedious by automating the many existing
tools used to create, maintain and release a perl module distribution.

For example, though not an installer itself, `Dist::Zilla` can automate the
creation of installers that you can bundle with your distribution. `Dist::Zilla`
can also help automatically generate POD quickly and effortlessly. `Dist::Zilla`
does not market your app for you but can definitely ease the pain of generating
a distribution suitable for delivery on CPAN. And `Dist::Zilla` obviously can’t
write your module for you but it can help set up your module’s directory
structure and populate your modules with boilerplate code to get you started.
`Dist::Zilla` will not be able to write all of your tests but it will ensure
that they are run before you release your code. In short, `Dist::Zilla` can
eliminate a ton of repetitive, mindless tasks to make the distribution creation
process much smoother, faster and more worry-free by helping you automate the
other automation tools that help you build a perl module distribution.

It’s very difficult to catalogue everything `Dist::Zilla` can do for you. And
aside from all that the core `Dist::Zilla` module can do, there are hundreds of
`Dist::Zilla` plugins that you can install and configure to help you automate
any number of common as well as very obscure distribution chores. And what you
choose to automate is entirely up to you. You can use a small subset of
`Dist::Zilla`’s plugins to handle only the most rudimentary tasks or you can
create an almost totally automated beast that can create, generate, modify,
build, test, document, release, push, upload and update your distribution with a
a very small number of keystrokes.

`Dist::Zilla` is a very powerful, flexible tool. Like any advanced tool for
developers, it is best wielded by those who have a good understanding of how to
properly build a perl distribution using more traditional approaches. And so
`Dist::Zilla` does not eliminate the need to understand how build systems,
tests, and other, more basic concepts and practices that are employed to create a
distribution. Therefore, beginners who are completely new to perl distributions
should probably avoid diving into `Dist::Zilla` without a solid understanding of
what goes into the proper building of a perl module distribution.

And so this series of articles, designed for beginners new to the world of perl
software distribution, will help introduce them to many of the tools that
`Dist::Zilla` either incorporates directly or mimics using its various plugins
to help illustrate what `Dist::Zilla` is doing under the hood to make its
automated magic happen.
