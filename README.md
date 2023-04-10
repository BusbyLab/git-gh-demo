Git and GitHub: Laying a Foundation for Reproducible Data Analysis
================
Kyle A. Gervers
2023-04-10

> This repo contains work that is unpublished. All results shown are
> preliminary and are only included here to simulate a realistic use
> case.

> The content and organization of this demonstration is heavily inspired
> by
> [walkthroughs](https://riffomonas.org/reproducible_research/version_control/#1)
> designed by [Pat Schloss](https://www.schlosslab.org/). Please explore
> all of his work, perhaps starting
> [here](https://www.youtube.com/watch?v=olu821RTQA8&list=PLmNrK_nkqBpKtCHqSSHXZdK-C4Zg89Rrv)
> and/or [here](https://doi.org/10.1128/mra.01310-22)!

# Sections

- [Introduction](#introduction)
- [Git and GitHub](#git-and-github)
- [Getting started](#getting-started)
- [Using Git](#using-git)
- [Using GitHub CLI](#using-github-cli)
- [Reproducibility](#reproducibility)

## Introduction

[Return](#sections)

There are a lot of moving parts involved in most microbiome data
analysis projects. Demultiplexing, trimming, and quality filtering can
each yield dozens to hundreds of files, parameters need to be tuned to
the quirks of each data set, and entirely novel scripts are often needed
to examine properties of particular interest. Sometimes older versions
of the code are needed, and sometimes code needs to be shared with
advisors and collaborators.

It’s certainly possible to manually keep track of all these moving
parts. Every time I change a file, I could type those changes into a
log, indicating which lines or which elements I changed. But it can be
hard for me to remember and log all of the changes I made to a single
file, let alone every file in my project. Further, in the case of
research projects, getting confused by my code can have disastrous
consequences. Add a collaborator to a project, and the room for
confusion doubles. This is where version control software like Git and
platforms like GitHub lend a helping hand. These two solutions allow me
to (1) easily download code, (2) add and track the edits I make, (3)
undo those edits to a previous state, (4) store the edited code on a
remote web platform, (5) share this code with a co-author, who can then
(6) independently edit my code. I can then (7) merge these edits with
the edits I had been making in the meantime, and then, once the project
matures (8) publish a release of our code with the manuscripts I submit
for publication in a journal. Other researchers can then use my code
which (9) builds trust in my research and (10) helps other researchers
improve on my approaches, improving the field overall.

In short, using Git and GitHub improves makes practically every step of
the analysis, collaboration, and publication workflow easier. As with
all tools, using them consistently and comfortably requires patience and
practice. Thankfully, over 100 million people use Git and GitHub, so a
lot can be learned from this huge user base.

## Git and GitHub

[Return](#sections)

Before we get too far along: *Git is not the same as GitHub*. These are
two separate tools that have evolved to play very nicely with each
other. Linus Torvalds authored [Git](https://en.wikipedia.org/wiki/Git)
in 2005 as way to coordinate the development of the Linux kernel among
several developers, while
[GitHub.com](https://en.wikipedia.org/wiki/GitHub) was launched in 2008
by GitHub, Inc. (owned by Microsoft) to host software development
projects that used Git to organize workflows. While it used to be the
case that the two could be separated by the fact that Git was a program
and GitHub was a platform, GitHub has since developed a suite of handy
protocols and programs to improve developer workflows, notably [GitHub
CLI](https://cli.github.com/), which we will use here.

## Getting started

[Return](#sections)

- [Make a GitHub account](#make-a-github-account)
- [Install and configure Git](#install-and-configure-git)
- [Install and configure GitHub CLI](#install-and-configure-github-cli)

### Make a GitHub account

[Return](#getting-started)

Click [here](https://github.com/join) to make a GitHub account, and [set
up two-factor
authentication](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa)
while you’re at it.

I recommend using a non-OSU email to set up this account, as you might
not always have access to your OSU email address. In addition to your
email address, record the username you provided for this GitHub account.

### Install and configure Git

[Return](#getting-started)

As indicated in the Git installation
[guide](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git),
installation depends on operating system (Linux, macOS, or Windows), so
follow the directions specific to your OS.

Once installed, Git needs to be configured to make it a little easier to
use. Replace my information with your information on the command line.

    git config --global user.name "Kyle Austin Gervers"
    git config --global user.email "kyle@gervers.com"
    git config --global github.user "gerverska"
    git config --global color.ui "auto"
    git config --global core.editor "nano -w"
    git config --global credential.helper 'cache --timeout=3600'
    git config --global push.default simple

For Linux + macOS:

    git config --global core.autocrlf input

For Windows:

    git config --global core.autocrlf true

Feel free to change any of these [Git
configurations](https://git-scm.com/docs/git-config), but I have found
these settings to work fine for most of my projects.

I can always see what I’ve configured by running the following:

    git config --list

### Install and configure GitHub CLI

[Return](#getting-started)

This program is not entirely necessary for using Git with GitHub, but I
think it simplifies account security operations and project repository
creation. It also seems like this program simplifies higher-level
operations that I won’t cover here (and don’t full understand at the
time of this writing), and I’ll probably “grow into” this package over
time. Again, follow the [installation
directions](https://github.com/cli/cli#installation) relevant to your
operating system.

There’s minimal configuration to do, but consider changing the editor to
nano (my preference).

    gh config set editor nano

To check the configuration state, run:

    gh config list

## Using Git

[Return](#sections)

The place where project code is kept is called a repository. In the
language of Git and GitHub, this is often shortened to `repo`. If I
follow this link, I will be taken to my repo named `git-gh-demo`. From
here, anyone can download the code by running the following `git`
command:

    git clone https://github.com/gerverska/git-gh-demo.git

### 

Run this, and then `cd` into the directory associated with this repo.

    cd git-gh-demo
    ls -a

    .   01-demultiplex  03-denoise  05-rarefy   .adapters  config.yml  env           .git        notes.txt     README.md
    ..  02-trim         04-compile  06-analyze  code       data        fun-gi.Rproj  .gitignore  README_files  README.rmd

The output of `ls -a` shows all the files and folders associated with
the repo. See a folder named `.git`? This is the folder `git` uses to
keep track of the all the changes I’ve made to the `git-gh-demo` repo. I
never usually look in this folder. To get experience working with your
own repos, you’ll need to remove this directory.

    rm .git -fr

After `.git` has been removed, run `git init .` to initialize a repo
inside the `git-gh-demo` folder. It’s as easy as that.

    git init .

    Initialized empty Git repository in /path/to/git-gh-demo/.git/

I now have a bouncing bay repo.

However, there’s nothing in my repo. Whenever I run `git status` while
I’m in the `git-gh-demo` folder, I get this:

    On branch main

    No commits yet

    Untracked files:
      (use "git add <file>..." to include in what will be committed)
        .adapters
        .gitignore
        01-demultiplex/
        02-trim/
        03-denoise/
        04-compile/
        05-rarefy/
        06-analyze/
        README.rmd
        README_files/
        code/
        config.yml
        data/
        fun-gi.Rproj
        notes.txt

    nothing added to commit but untracked files present (use "git add" to track)

This shows me that all the files in `git-gh-demo` are untracked. To help
me only track the files I care about, `git` does track any files or
directories after I run `git init .`. Instead I have to `add` the files
I want tracked. The output I got from `git status` above suggests:

    use "git add <file>..." to include in what will be committed

I’ll do just that!

    git add .

## Using GitHub CLI

[Return](#sections)

Run the code below to sign in to GitHub from the command line.

    gh auth login

GitHub CLI will start an interactive session, asking you a series of
questions.

    ? What account do you want to log into?  [Use arrows to move, type to filter]
    > GitHub.com
      GitHub Enterprise Server

Select `GitHub.com`

    ? What is your preferred protocol for Git operations?  [Use arrows to move, type to filter]
    > HTTPS
      SSH

For now, select `HTTPS`. `SSH` is technically safer, but requires some
setup.

    ? Authenticate Git with your GitHub credentials? (Y/n)

Type `Y`. We’re just trying this out for now, but later this will allow
you to push your project to GitHub using Git.

    ? How would you like to authenticate GitHub CLI?  [Use arrows to move, type to filter]
    > Login with a web browser
      Paste an authentication token

For now, select `Login with a web browser`. You can choose to paste an
authentication token (fine-grained or classic) if you [set that
up](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token),
but just make sure that you keep the token somewhere memorable and safe.
However, in my opinion, if you want to go the token route, you might as
well set up an SSH sign in, which we’ll cover later.

    ! First copy your one-time code: 19C2-CD83
    Press Enter to open github.com in your browser...

After copying the code, press `Enter`. A GitHub tab in your default
browser window should pop up with a place for you to paste your code.

    ✓ Authentication complete.
    - gh config set -h github.com git_protocol https
    ✓ Configured git protocol
    ✓ Logged in as gerverska

And I’m logged in! I’ll need to run `gh auth login` whenever I’m ready
to push my project to GitHub. Once I’m logged in, I should be able to
push within an hour or so.

## Reproducibility

[Return](#sections)

All packages were installed and managed with `conda`. These packages
aren’t too relevant for this demo, but they’re listed here for
completeness.

    conda 23.3.1
    name: /home/gerverska/projects/git-gh-demo/env
    channels:
      - conda-forge
      - bioconda
      - defaults
    dependencies:
      - atropos=1.1.31
      - bioconductor-dada2=1.26.0
      - cutadapt=4.2
      - fastqc=0.11.9
      - itsx=1.1.3
      - multiqc=1.13
      - pheniqs=2.1.0
      - r-base=4.2.2
      - r-caret=6.0_93
      - r-dplyr=1.0.10
      - r-markdown=1.4
      - r-remotes=2.4.2
      - r-rmarkdown=2.20
      - r-vegan=2.6_4
      - vsearch=2.22.1
    prefix: /home/gerverska/projects/git-gh-demo/env

Install the above bioinformatic environment from `config.yml` using the
script `00-build.sh`

    bash code/00-build.sh
