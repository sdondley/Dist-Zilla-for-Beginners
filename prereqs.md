# Who This Tutorial is For

This series of `Dist::Zilla` tutorials is aimed at beginning developers
interested in making contributions to CPAN but have limited or no experience
creating a distribution and are interested in learning how `Dist::Zilla` can
help them do it. Some will argue that beginners have no business using
`Dist::Zilla` but we disagree. We think the major problem with `Dist::Zilla` is
not so much with it's complexity but with a lack of documentation aimed at
beginners. Like a sharp pair of scissors, you can injure yourself badly but with
the proper guidance, a grade schooler has what it takes to learn to use the tool
responsibly.

The tutorial is also for developers who may have some experience releasing
distributions using the many other simpler options available for creating
distributions but who may feel handcuffed by their limitations.

Finally, these tutorials are also great for existing `Dist::Zilla` users who may
not completely understand all the moving parts and are looking for some
enlightenment.

## Software Prerequisites for this Tutorial

You should have a relatively modern release of Perl installed on your machine.
Anything newer than 5.20 will certainly be adequate and you'll probably be fine
with installs as old 5.10.

You will need to install modules from CPAN and the examples in this tutorial
make frequent use of the `cpanm` module so take a moment to install that with
`cpan App::cpanminus` if it's not already installed.

Finally, you'll also need a copy of `Dist::Zilla` installed. If it's not
installed, you know what to do.
