# Configuring your Ubuntu-based PC for Quantitative Social Science Work
This file will help you get your Ubuntu-based Linux machine up and
running to do quantitative social science data work. It was inspirited
by the similarly-titled
[SocialScienceMacconfig](https://github.com/jwbowers/SocialScienceMacConfig)
put together by Jake Bowers and Jeff Gill.

Steps should be similar or identical in other Debian-based Linux
distros. For other Linux systems, you'll probably have access to
everything I describe here, but may need to change the commands a
bit.

Most (all?) programs here are also available on Windows and Mac. Just
google around to find out how to get them for your operating system. 

# Step 0: Install Ubuntu
You can install Ubuntu on pretty much any computer. I use the 6-month
rolling releases instead of the LTS. You can get more information
about how to install Ubuntu from
[Ubuntu's website](http://www.ubuntu.com/). A quick Google search will
also pull up helpful instructions. Be sure to test it out from a
bootable USB or other disc before installing it. This'll make sure
that it works well on your computer. 

# Step 1: Install programs from Ubuntu's repositories
You'll want Emacs (at least 24) for LaTeX, markdown, R, and git. Yes,
it really does all of that:
- [ ] `sudo apt-get install emacs24 ess`

You'll want git for version control:
- [ ] `sudo apt-get install git`

## Step 1a: Configure git
You'll need to tell git your name and email. Instructions for how to
do so are [here](). 

# Step 2: Install R
Installing R on an Ubuntu type Linux distribution is easy. You can
simply follow the steps
[here](https://cran.r-project.org/bin/linux/ubuntu/README). Be sure to
read about the secure APT part

## Step 2a: Configure CRAN mirror
This step isn't totally necessary, but it may save you some time in
the future. In your `.Rprofile` file (usually located at ~/), you can
set the CRAN mirror so that R doesn't ask you. I use RStudio's mirror,
since it should work fairly quickly anywhere in the world. If you want
to use a different mirror, replace `"https://cran.rstudio.com/"` with
your favorite CRAN mirror. You can find the full list of mirrors [here]().

    local({
      r <- getOption("repos")
          r["CRAN"] <- "https://cran.rstudio.com/"
              options(repos = r)
    })


# Step 3: Install TeXLive
This is a bit trickier than R. Ubuntu actually has TeXLive in its
repositories, but the version there tends to be a bit out of date. If
you want, you can install it via `sudo apt-get texlive-full`. Be
warned - it's a huge file, so it will take a lot of data (and time) to
download and install.

I really don't recommend doing that, though. You can install what's
referred to as "vanilla" TeXLive fairly easily. There's a stackoverflow
answer [here]() that gives step-by-step instructions for how to get
vanilla TeXLive set up on your system.

If you usually have regular access to the internet, I'd recommend
skipping the ______________________ files. They add a lot to the
installation and are totally unnecessary if you have internet
connectivity. 

If you're on a Mac, you can install MaCTeX and Windows users probably
want MiKTeX. 

