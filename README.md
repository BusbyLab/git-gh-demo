Git and GitHub: Laying a Foundation for Reproducible Data Analysis
================
Kyle A. Gervers
2024-10-07

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
- [Git](#git)
- [GitHub CLI](#github-cli)
- [Subsequent pushes](#subsequent-pushes)
- [The virtuous circle](#the-virtuous-circle)
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
lot can be learned from this huge user base by googling.

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
- [A note on reproducibility](#a-note-on-reproducibility)

### Make a GitHub account

Click [here](https://github.com/join) to make a GitHub account, and [set
up two-factor
authentication](https://docs.github.com/en/authentication/securing-your-account-with-two-factor-authentication-2fa)
while you’re at it.

I recommend using a non-OSU email to set up this account, as you might
not always have access to your OSU email address. In addition to your
email address, record the username you provided for this GitHub account.

### Install and configure Git

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

This program is not entirely necessary for using Git with GitHub, but I
think it simplifies account security operations and project repository
creation. It also seems like this program simplifies higher-level
operations that I won’t cover here (and don’t full understand at the
time of this writing), and I’ll probably “grow into” this package over
time. Again, follow the [installation
directions](https://github.com/cli/cli#installation) relevant to your
operating system.

There’s minimal configuration to do, but consider changing the editor to
`nano` (my preference).

    gh config set editor nano

To check the configuration state, run:

    gh config list

### A note on reproducibility

If you are a Windows user, it might be easier to use Git and GitHub CLI
on the CQLS HPC (i.e., “the cluster”) or Posy’s lab computer. The
system-wide installs can be used, but both of these programs can also be
installed and managed to your liking using `conda`.

A guide to setting up your own `conda` install on the CQLS HPC can be
found
[here](https://software.cqls.oregonstate.edu/tips/posts/conda-tutorial/).
While not covered in this demo, using `conda` stands to help all users
keep their workflows reproducible and customizable. Refer to [videos on
this
topic](https://youtube.com/playlist?list=PLmNrK_nkqBpJtNdQBPhPWjIFRYeFOGfJ1&si=ICgjd2Pag0Z6sOp2)
by Pat Schloss on his Riffomonas Project YouTube channel.

## Git

[Return](#sections)

The place where project code is kept is called a repository. In the
language of Git and GitHub, this is often shortened to `repo`. If I
follow this [link](https://github.com/gerverska/git-gh-demo), I will be
taken to my repo named `git-gh-demo`. From here, anyone can download the
code by running the following `git` command:

    git clone https://github.com/gerverska/git-gh-demo.git

<br> Where did I get the `https://github.com/gerverska/git-gh-demo.git`
link? After following the link to the `git-gh-demo` repo, I clicked the
green `<> Code` button and copied and pasted the HTTPS link onto the
command line.

After running `git clone`, **(1)** use `mv` to change the name of
`git-gh-demo` as shown below (**but don’t use my name!**), **(2)** use
`cd` to access the renamed directory, and then **(3)** use `rm` to
remove the `.git` folder that came with the repo.

    mv git-gh-demo/ git-gh-demo_gerverska/
    cd git-gh-demo_gerverska/
    rm -fr .git

    ls -a

<br> The output of `ls -a` shows all the files and folders associated
with the repo.

    .               02-trim     05-rarefy   config.yml         .gitignore    README.rmd
    ..              03-denoise  06-analyze  data               README_files
    01-demultiplex  04-compile  code        git-gh-demo.Rproj  README.md

<br>

Run `git init .` to initialize a repo inside the renamed `git-gh-demo`
folder. It’s as easy as that.

    git init .

    Initialized empty Git repository in /path/to/git-gh-demo_gerverska/.git/

<br> I now have a bouncing baby repo. Specifically, after running
`git init .`, I now have a *local* repo (i.e., stored locally on my
computer). The link that I cloned `git-gh-demo` from is a *remote* repo
(i.e., stored on something that’s *not* my local repo). **The goal now
for folks following along is to create a remote repo from this local
repo.**

However, there’s nothing in my repo. Whenever I run `git status` while
I’m in the renamed `git-gh-demo` folder, I get this:

    On branch main

    No commits yet

    Untracked files:
      (use "git add <file>..." to include in what will be committed)
        .gitignore
        01-demultiplex/
        02-trim/
        03-denoise/
        04-compile/
        05-rarefy/
        06-analyze/
        README.md
        README.rmd
        README_files/
        code/
        config.yml
        data/
        git-gh-demo.Rproj

    nothing added to commit but untracked files present (use "git add" to track)

<br> This shows me that all the files in `git-gh-demo` are untracked. To
help me only track the files I care about, `git` doesn’t track any files
or directories after I run `git init .`. Instead I have to `add` the
files I want tracked. The output I got from `git status` above suggests:

    use "git add <file>..." to include in what will be committed

I’ll do just that!

    git add .

<br> Instead of specifying a file, I specified `.`. This is the same as
adding all of the files and folders to this repo. Now when I run
`git status` I get:

    On branch main

    No commits yet

    Changes to be committed:
      (use "git rm --cached <file>..." to unstage)
        new file:   .gitignore
        new file:   01-demultiplex/README.md
        new file:   01-demultiplex/logs/R1.html
        new file:   01-demultiplex/logs/R1_data/multiqc.log
        new file:   01-demultiplex/logs/R1_data/multiqc_citations.txt
        new file:   01-demultiplex/logs/R1_data/multiqc_data.json
        new file:   01-demultiplex/logs/R1_data/multiqc_fastqc.txt
        new file:   01-demultiplex/logs/R1_data/multiqc_general_stats.txt
        new file:   01-demultiplex/logs/R1_data/multiqc_sources.txt

<br> Several other files are listed too. If I wanted to undo this `add`,
`git status` says I could run `git rm .` to unstage, which is good to
know. I try not to add files that I think might present security
vulnerabilities, or contain speculation or important information that I
don’t think other GitHub users should see. If I’m happy with adding all
these files, the next step is to commit those changes.

    git commit -a

<br> This command commits all of the changes we just added and opens up
an instance of `nano`:

    GNU nano 6.2

    # Please enter the commit message for your changes. Lines starting
    # with '#' will be ignored, and an empty message aborts the commit.
    #
    # On branch main
    #
    # Initial commit
    #
    # Changes to be committed:
    #       new file:   .gitignore
    #       new file:   01-demultiplex/README.md
    #       new file:   01-demultiplex/logs/R1.html
    #       new file:   01-demultiplex/logs/R1_data/multiqc.log
    #       new file:   01-demultiplex/logs/R1_data/multiqc_citations.txt
    #       new file:   01-demultiplex/logs/R1_data/multiqc_data.json
    #       new file:   01-demultiplex/logs/R1_data/multiqc_fastqc.txt
    #       new file:   01-demultiplex/logs/R1_data/multiqc_general_stats.txt
    #       new file:   01-demultiplex/logs/R1_data/multiqc_sources.txt
    #       new file:   01-demultiplex/logs/R2.html

<br> There’s a note in the `nano` window that asks me to
`enter the commit message for your changes`. I try to make these commit
messages short and sweet (10 words or so), while also accurately
communicating what was changed. Since this is my initial commit, let’s
just write `Initial commit` and write `nano` output.

    [main (root-commit) #######] Initial commit
     192 files changed, 3217058 insertions(+)
     create mode 100644 .gitignore
     create mode 100644 01-demultiplex/README.md
     create mode 100644 01-demultiplex/logs/R1.html
     create mode 100644 01-demultiplex/logs/R1_data/multiqc.log
     create mode 100644 01-demultiplex/logs/R1_data/multiqc_citations.txt
     create mode 100644 01-demultiplex/logs/R1_data/multiqc_data.json
     create mode 100644 01-demultiplex/logs/R1_data/multiqc_fastqc.txt
     create mode 100644 01-demultiplex/logs/R1_data/multiqc_general_stats.txt
     create mode 100644 01-demultiplex/logs/R1_data/multiqc_sources.txt
     create mode 100644 01-demultiplex/logs/R2.html

<br> When I run `git status` again, I now have this update:

     On branch main
     nothing to commit, working tree clean

## GitHub CLI

[Return](#sections)

I’m very close to pushing this local repo to GitHub (the remote)!
Because this is my first push to GitHub (pretend for this example!),
I’ll actually use `gh` from GitHub CLI to do this. You can also create
an empty repo through the GitHub website that you use `git` to push to,
but I think it’s just easier to use `gh`.

    gh repo create

<br> This starts an interactive session with `gh`. Answer all the
prompts as shown below (follow the `>` arrow). If you’re in your renamed
`git-gh-demo` folder (which you should be if you’re following along),
just press `Enter` for each step.

    ? What would you like to do?  [Use arrows to move, type to filter]
      Create a new repository on GitHub from scratch
    > Push an existing local repository to GitHub

Press `Enter` <br> <br>

    ? Path to local repository (.)

Press `Enter` <br> <br>

    ? Repository name (git-gh-demo_gerverska)

Press `Enter` <br> <br>

    ? Repository owner  [Use arrows to move, type to filter]
    > BusbyTest
      gerverska

I’ll select `BusbyTest` for this example so my PI can associate this
repo with their organization. If you don’t have the option to select
`BusbyTest`, choose yourself. <br> <br>

    ? Description (git-gh-demo) Learning git + gh

Provide an informative description of the repo. <br> <br>

    ? Visibility  [Use arrows to move, type to filter]
      Public
    > Private
      Internal

I’ll select `Private` for this exercise, but I could select `Public` if
my PI gave me the go-ahead. **Always ask your PI before making a repo
public.** <br> <br>

    ✓ Created repository BusbyTest/git-gh-demo_gerverska on GitHub

    ? What should the new remote be called? (origin)

Press `Enter` <br> <br>

    ? Would you like to push commits from the current branch to "origin"? Yes

    Enumerating objects: 207, done.
    Counting objects: 100% (207/207), done.
    Delta compression using up to 12 threads
    Compressing objects: 100% (202/202), done.
    Writing objects: 100% (207/207), 46.31 MiB | 12.47 MiB/s, done.
    Total 207 (delta 73), reused 0 (delta 0), pack-reused 0
    remote: Resolving deltas: 100% (73/73), done.
    To https://github.com/BusbyTest/git-gh-demo_gerverska.git
     * [new branch]      HEAD -> main
    branch 'main' set up to track 'origin/main'.
    ✓ Pushed commits to https://github.com/BusbyTest/git-gh-demo_gerverska.git

Excellent! I now have a remote repo I can view at
<https://github.com/BusbyTest/git-gh-demo_gerverska> that matches my
local repo!

## Subsequent pushes

[Return](#sections)

Perhaps I make more changes to my code after the intial repo push. After
running `git add .`, `git commit -a`, and `git status`, I can see that
my *local* repo (on my computer) is ahead of my *remote* repo (on
GitHub).

    On branch main
    Your branch is ahead of 'origin/main' by 1 commit.
      (use "git push" to publish your local commits)

    nothing to commit, working tree clean

<br> Before I can run `git push` (which is what I’ll use for subsequent
pushes), I need to refresh my login.

    gh auth login

<br> GitHub CLI will start an interactive session, asking you a series
of questions.

    ? What account do you want to log into?  [Use arrows to move, type to filter]
    > GitHub.com
      GitHub Enterprise Server

Select `GitHub.com` <br> <br>

    ? What is your preferred protocol for Git operations?  [Use arrows to move, type to filter]
    > HTTPS
      SSH

For now, select `HTTPS`. `SSH` is technically safer, but requires some
setup. <br> <br>

    ? Authenticate Git with your GitHub credentials? (Y/n)

Type `Y`. <br> <br>

    ? How would you like to authenticate GitHub CLI?  [Use arrows to move, type to filter]
    > Login with a web browser
      Paste an authentication token

For now, select `Login with a web browser`. You can choose to paste an
authentication token (fine-grained or classic) if you [set that
up](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token),
but just make sure that you keep the token somewhere memorable and safe.
However, in my opinion, if you want to go the token route, you might as
well set up an SSH sign in. <br> <br>

    ! First copy your one-time code: 19C2-CD83
    Press Enter to open github.com in your browser...

After copying the code, press `Enter`. A GitHub tab in your default
browser window should pop up with a place for you to paste your code.
<br> <br>

    ✓ Authentication complete.
    - gh config set -h github.com git_protocol https
    ✓ Configured git protocol
    ✓ Logged in as gerverska

<br> And I’m signed in! Now that I’m signed in, I’ll go ahead and run
`git push`. If I don’t push soon, I’ll need to run `gh auth login`
again.

    Enumerating objects: 7, done.
    Counting objects: 100% (7/7), done.
    Delta compression using up to 12 threads
    Compressing objects: 100% (4/4), done.
    Writing objects: 100% (4/4), 5.20 KiB | 1.04 MiB/s, done.
    Total 4 (delta 3), reused 0 (delta 0), pack-reused 0
    remote: Resolving deltas: 100% (3/3), completed with 2 local objects.
    To https://github.com/BusbyTest/git-gh-demo_gerverska.git
       #######..#######  main -> main

Success! The remote repo on GitHub now reflects the changes made to the
local repo on my computer.

## The virtuous circle

[Return](#sections)

90% of my experience with Git and GitHub looks like this.

    git status
    git add .
    git status
    git commit -a
    git status
    gh auth login
    git push
    git status

As long as you’re running `git status` (and reading the output), you
shouldn’t get too surprised! When in doubt, take it slowly.

Usually, it’s best practice to make several commits in series while
you’re working on different sections of your code. By choosing to work
on specific sections of your code, you can write more specific `commit`
messages. You can treat these commit messages as notes to yourself or
your collaborators describing what you did. If you’re happy with your
changes after making a series of commits, go ahead and `push`.

I’ll be updating this repo with more information on how to use other
`git` and `gh` commands!

## Reproducibility

[Return](#sections)

All packages were installed and managed with `conda`.

    conda 24.4.0
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

    # Clone the repo (using the GitHub CLI tool) ####
    gh repo clone gerverska/git-gh-demo

    # Run the build script ####
    bash code/00-build.sh
