# Configuring your Arch Linux-based PC for Quantitative Social Science Work
This file will help you get your Arch Linux-based Linux machine up and
running to do quantitative social science data work. It was inspirited
by the similarly-titled
[SocialScienceMacConfig](https://github.com/jwbowers/SocialScienceMacConfig)
put together by Jake Bowers and Jeff Gill.

Most (all?) programs included in this list are
[FOSS](https://en.wikipedia.org/wiki/Free_and_open-source_software). Thus,
they are also available on Windows and Mac. Just
google around to find out how to get them for your operating system. 

# Step 0: Install Arch Linux
This is not meant as a manual to help you install Arch. However,
you can install Arch on pretty much any computer. Arch is a rolling
release distro, so there aren't any "big" updates that you'll have to
do like with Windows, Mac, or other Linux distros (Ubuntu, for
example). 
[Arch's wiki](https://wiki.archlinux.org/index.php/Beginners'_guide)
has a great article that will get you started with Arch. Don't be
afraid - the installation process is pretty painless if you're good at
following directions. Make sure you go to the
[general recommendations](https://wiki.archlinux.org/index.php/General_recommendations)
at the end to set up a non-root user account and a working desktop
environemnt (I use gnome).

## Step 0a: Update Arch's Mirrorlist
Arch has a lot of mirrors. You can customize this list, sorting the
list so that you use the mirrors that are fastest for you with the
following commands. I've found that this makes a non-trivial
difference in the speed at which packages download. You'll need to
change "United States" to whatever
country you're in.

    sudo pacman -S reflector
    sudo reflector --sort rate --save /etc/pacman.d/mirrorlist -c "United States" -f 5 -l 5

# Step 1: Install programs from Arch's repositories
You'll want Emacs for LaTeX, markdown, R, and git. Yes,
it really does all of that. Git is good for all your version control
needs. R is everything you need for statistical analysis, and texlive
will provide you with a working TeX distribution. Pandoc converts between 
many different document formats (tex, pdf, markdown, Word, etc). The fortran
compiler is used by one of lintr's dependencies. Finally, aspell-en gives you 
dictionary files for spell checking. If you type in languages other than English, 
you'll need to find the proper dictionaries.

    sudo pacman -S emacs git r texlive-most pandoc gcc-fortran aspell-en

## Step 1a: Install packages from the AUR
There are a *ton* of useful packages in the Arch User's Repository
(AUR). You can either download and install them yourself or set up an
AUR helper like [yaourt](https://wiki.archlinux.org/index.php/Yaourt)
to manage this for you. If you're doing Bayesian analysis you want jags. 
Dropbox is also in there, so install that if you want. 

    yaourt -S jags dropbox

## Step 1b: Configure git
You'll need to tell git your name and email. Run these two commands in
the terminal, replacing the "John Doe" information with your name and
email:

    git config --global user.name "John Doe"
    git config --global user.email johndoe@example.com

The global option will tell git to remember those values, so you
shouldn't have to do this again. 

## Step 1c: Configure R
This step isn't totally necessary, but it may save you some time in
the future. In your `.Rprofile` file (usually located at ~/), you can
set the CRAN mirror so that R doesn't ask you. I use RStudio's mirror,
since it should work fairly quickly anywhere in the world. If you want
to use a different mirror, replace `"https://cran.rstudio.com/"` with
your favorite CRAN mirror. You can find the full list of mirrors
[here](https://cran.r-project.org/mirrors.html). 

The second part of this configuration file tells R never to save your workspace, 
and don't even ask about it. If you leave this out, R asks after you `q()` if 
you want the workspace saved and you have to answer `y` or `n`. This way, you never
have to deal with it. And you should really be saving things you want *before* trying
to quit R, anyway! 

    local({
      r <- getOption("repos")
          r["CRAN"] <- "https://cran.rstudio.com/"
              options(repos = r)
    })
    utils::assignInNamespace(
        "q",
        function(save = "no", status = 0, runLast = TRUE)
        {
        .Internal(quit(save, status, runLast))
        },
    "base"
    )


# Step 2: Install R packages
R can do a lot out of the box, but it's real strength lies in packages
that extend it. Here's a quick list of some of the packages that I
find myself using frequently:

    install.packages(c("lintr", "ggplot2", "dplyr", "zoo", "haven",
                     "rstan", "rjags", "MCMCpack", "rmarkdown",
                     "knitr", "reshape2", "servr"))

# Step 3: Configure Emacs
Emacs is infinitely configure-able. You could spend months (years?)
getting it *just* right. Feel free to do so. It's probably not worth
it, though. My advice is just to commit to using emacs, and develop
your specific configuration as you go. There are definitely a few
things that you'll want to set up beforehand, though.

I keep my .emacs file in
a [github repository](https://github.com/jabranham/emacs), so feel
free to check it out. If you drop the init.el file into ~/.emacs.d/,
it should work just fine on your setup. It adds the ability to connect
to [MELPA](https://melpa.org/) to your emacs setup, so you'll need an
internet connection the first time you use it.

I'm not sure I'd recommend just using my .emacs file, though (or
anyone else's). Setting up emacs yourself teaches you a bit about how
the program thinks. So I'll list some packages here that I think are
particularly invaluable:

* magit - for all your git needs, inside emacs
* auctex - for writing LaTeX
* polymode - for .Rnw (knitr + LaTeX) and .Rmd (rmarkdown) files
* ebib - for .bib file management
* markdown-mode - for working with markdown files
* reftex - this comes bundled with emacs in recent versions (so no
  need to install it separately), but makes
  working with .bib files a breeze 

Those are absolute must-haves for doing the sort of work we do. If you
work with languages other than R, LaTeX, and markdown, you'll want to
make sure you have the proper "major mode" for that language. ESS
(which you already installed above) makes emacs work with R, so no
need to do anything there. There are some other packages that just
make emacs work a little better, and I'll list some here:

* better-defaults - literally better defaults for emacs. Pretty much
everyone should use this
* smex - easier way to navigate commands (like `M-x`)
* There are several "ido" packages - go look them up. They'll make
your life easier.
* smartparens - easier parentheses management
* company - for autocompletion (there's also auto-complete, but I
prefer company)
* flyspell - for spell checking on the fly
* smooth-scrolling - for doing away with emacs' nonsense default
scrolling
* flycheck - checks for good style and syntax

There are tons more packages available, but those should get you
started.

## Step 3a: PDF files in emacs
`pdf-tools` is an emacs package to work with pdf files. The github
repo is [here](https://github.com/politza/pdf-tools). You'll need to
follow the installation instructions there. Note that you can't just
download it from MELPA, since you need to compile and install the C
part.  You probably already have these installed, but if not do:

    sudo pacman -S poppler poppler-glib

You'll then need to clone the git repo somewhere. Navigate somewhere
where you want, then do:

    git clone https://github.com/politza/pdf-tools.git
    cd pdf-tools/
    make -s
    make install-package

# Step 5: Profit!
You're now set up to use a combination of git, R, markdown, and LaTeX
in a sane working environment. You didn't have to pay a penny, either!
These programs (emacs and R in particular) have a bit of a learning
curve, but don't be dissuaded. There are tons of resources online that
can help you out. I also write about this kind of stuff on
[my blog](www.jabranham.com). 

# Step 6: Setting up your Machine to Serve Github Pages

If you run your personal website through
[github pages](https://pages.github.com/), then you probably want the
stuff in this section. Otherwise, skip it. You'll need several of
jekyll's dependencies, then jekyll itself (which is included in the
`github-pages` gem). 

    sudo pacman -S ruby
    gem install github-pages

Make sure the jekyll installed correctly:

    jekyll -v
