# R Patched and Development Versions {#GetStart}

These instructions cover how to install R from source or from binaries.
Contributors will typically need to work with the patched or development versions of R.
This chapter describes where the source code for these versions can be found and how to install these versions from the source or the binary builds (where available).
The tools required to build R and R packages are also discussed.
For the most up to date and complete instructions you can check the [R installation and administration manual](https://cran.r-project.org/doc/manuals/r-devel/R-admin.html) .

## The R Source Code

R uses [svn](https://subversion.apache.org/ "SVN official page") as a version control tool hosted at <https://svn.r-project.org/R/> and uses a 'major.minor.patchlevel' version numbering scheme[^02-getting_started-1].

[^02-getting_started-1]: Also known as [semantic versioning](https://en.wikipedia.org/wiki/Software_versioning#Semantic_versioning "Wikipedia explanation of semantic versioning")

There are three releases of R available to install:

-   The latest official release (`r-release`), either major version x.0.0 or minor version x.y.0, for example: 3.0.0 or 3.2.0

-   The patched release (`r-patched`), for example 3.0.1 or 3.2.1 and

-   The development release (`r-devel`) : continually developed version moving from r-release to next major/minor version (x + 1).0.0 or x.(y + 1).0 a few weeks before release (at the start of the "GRAND FEATURE FREEZE").

The source code of released versions of R can be found at [R/tags](https://svn.r-project.org/R/tags/ "svn release source code folder"), the patched versions are at [R/branch](https://svn.r-project.org/R/branches/ "svn patched source code folder").


The `r-devel` at [R/trunk](https://svn.r-project.org/R/trunk "svn devel source code folder") is the next minor or eventual major release development version of R.
Bug fixes and new features are introduced in `r-devel` first. 
If the change meets the [development guidelines](https://developer.r-project.org/devel-guidelines.txt) R Core will also make the change in `r-patched`.

## Prerequisites

To install from the source code you will need the source code and the dependencies of R.

If you need to install svn you can use your distribution's package manager to install it.

### Ubuntu

In Ubuntu you can use :

<!-- # Command to obtain it: apt-rdepends --build-depends --follow=DEPENDS r-base | grep " B" | sed -e "s/ Build-Depends: //" | sort -->

```sh
sudo apt-get install -y \
      bash-completion \
      bison \
      debhelper-compat \
      default-jdk \
      g++ \
      gcc \
      gfortran \
      groff-base \
      libblas-dev \
      libbz2-dev \
      libcairo2-dev \
      libcurl4-dev \
      libcurl4-openssl-dev \
      libjpeg-dev \
      liblapack-dev \
      liblzma-dev \
      libncurses5-dev \
      libpango1.0-dev \
      libpcre2-dev \
      libpcre3-dev \
      libpng-dev \
      libreadline-dev \
      libtiff5-dev \
      libx11-dev \
      libxt-dev \
      mpack \
      rsync \
      tcl8.6-dev \
      texinfo \
      texlive-base \
      texlive-extra-utils \
      texlive-fonts-extra \
      texlive-fonts-recommended \
      texlive-latex-base \
      texlive-latex-extra \
      texlive-latex-recommended \
      texlive-plain-generic \
      tk8.6-dev \
      x11proto-core-dev \
      xauth \
      xdg-utils \
      xfonts-base \
      xvfb \
      zlib1g-dev 
```

### Fedora

<!-- # Command to obtain it: dnf rq -q --repo=fedora-source --requires R -->
<!-- # Plus the rsync package-->

In Fedora you can use:

```sh
sudo dnf install -y \
    autoconf \
    automake \
    bzip2-devel \
    cairo-devel \
    flexiblas-devel \
    gcc-c++ \
    gcc-gfortran \
    java-devel \
    less \
    libICE-devel \
    libSM-devel \
    libX11-devel \
    libXmu-devel \
    libXt-devel \
    libcurl-devel \
    libicu-devel \
    libjpeg-devel \
    libpng-devel \
    libtiff-devel \
    libtool \
    ncurses-devel \
    pango-devel \
    pcre2-devel \
    readline-devel \
    rsync \
    tcl-devel \
    'tex(inconsolata.sty)' \
    'tex(latex)' \
    'tex(upquote.sty)' \
    texinfo-tex \
    tk-devel \
    tre-devel \
    valgrind-devel \
    xz-devel \
    zlib-devel
```

## Building R 

### Linux

Here are the basic steps intended as a checklist.
For complete instructions please see the section in [R-admin](https://cran.r-project.org/doc/manuals/r-devel/R-admin.html#Installing-R-under-Unix_002dalikes).

0. Retrieve R source code via into `TOP_SRCDIR`, note that we retrieve the `r-devel` source code:

    ```sh
    export TOP_SRCDIR="$HOME/Downloads/R"
    svn checkout https://svn.r-project.org/R/trunk/ "$TOP_SRCDIR"
    ```

1.  Download the latest recommended packages[^02-getting_started-2]:

    ```sh
    "$TOP_SRCDIR/tools/rsync-recommended"
    ```

3.  Create the build directory in the `BUILDDIR`:

    ```sh
    export BUILDDIR="$HOME/bin/R"
    mkdir -p "$BUILDDIR"
    cd "$BUILDDIR"
    ```
        
4.  Configure the R installation (with `--enable-R-shlib` so that RStudio IDE can use it):
        
    ```sh
    "$TOP_SRCDIR/configure" --enable-R-shlib
    ```
        
4.  Build R :

    ```sh
    make
    ```


5.  Check that R works as expected:

    ```sh
    make check
    ```

    There are other checks you can run:

    ```sh
    make check-devel
    make check-recommended
    ```


[^02-getting_started-2]: Recommended packages are not in the subversion repository.


If we don't want to build R in a different directory than the source code we can simply use:

```sh
cd "$TOP_SRCDIR"
svn update
tools/rsync-recommended
"$TOP_SRCDIR/configure"  --enable-R-shlib
make 
make check
```

Once you successfully build R from source you can modify R source code to fix an issue: Prepare a patch (See [this guide](https://www.r-project.org/bugs.html#how-to-submit-patches)) and after checking that R works as intended (`make check-devel`) submit the patch for the R core consideration.
(See the [lifecycle of a patch](#FixBug) chapter).


To use the `r-devel` version in RStudio to you can do the following:

```sh
export RSTUIDO_WHICH_R="$BUILDDIR/bin/R"
cd "$TOP_SRCDIR"
rstudio
```


### Windows

#### Binaries

The binary builds of R for Windows can be downloaded and installed from [here](https://cran.r-project.org/bin/windows/base/).
Along with the link to the latest stable release, this page also contains links to the binary builds of `r-patched` and `r-devel`.

1.  Click on the download links to download an executable installer.

2.  Select the language while installing, read the GNU general public license information, and select the destination location to start the installation.
    You will be prompted to select components at this stage: `User installation`, `64-bit User installation`, or `Custom installation`.
    The default option may be chosen for the questions from this step onwards to complete the installation.
    
Daily binaries for `r-devel` are made available for [download and installation](https://cran.r-project.org/bin/windows/base/rdevel.html). 

#### From source

Before installing R from source, some additional programs are needed, as per the [latest documentation](https://cran.r-project.org/bin/windows/base/howto-R-4.2.html):

1.  [Rtools](https://cran.r-project.org/bin/windows/Rtools/) is the suggested toolchain bundle for building R base and R packages containing compiled code on Windows. 
  The latest [version of Rtools42](https://cran.r-project.org/bin/windows/Rtools/rtools42/rtools.html) can be installed using the [Rtools installer rtools42-XXXX-XXX.exe ](https://cran.r-project.org/bin/windows/Rtools/rtools42/files/).

2.  A LaTeX compiler is needed to install and build R, check packages and build  manuals. 
    On CRAN, MiKTeX is used, which can be downloaded from <https://miktex.org>. 
    Once installed open MiKTeX via the Windows start menu.
    It might ask to check for updates and more importantly, to make it available in PATH. You can accept both.

1.  Open the Rtools42 terminal to update and install subversion:

    ```sh
    pacman -Syuu
    pacman -Sy wget subversion
    ```
    
3. Retrieve the latest source code via subversion:

    ```sh
    export TOP_SRCDIR="$HOME/Downloads/R"
    svn checkout https://svn.r-project.org/R/trunk/ "$TOP_SRCDIR"
    ```

    If you already have the repository available you can update as:

    ```sh
    cd $TOP_SRCDIR
    svn update
    ```

    You can also use a SVN client such as TortoiseSVN (<https://tortoisesvn.net/>, command line tool, and Windows Explorer integration) or SlikSVN (<https://sliksvn.com/download/>, just the command line tool) so that it can be also found by other tools.

2. Download the latest tcl/tk and unzip it in `$TOP_SRCDIR`:

    ```sh
    cd "$TOP_SRCDIR"
    wget -np -nd -r -l1 -A 'tcltk-*.zip' https://cran.r-project.org/bin/windows/Rtools/rtools42/files/
    unzip "tcltk-*.zip"
    ```

3. Add gcc, MiKTeX and tar to the PATH and set one tar option:

    ```sh
    export PATH="/x86_64-w64-mingw32.static.posix/bin:$PATH"
    export PATH="/c/Program\ Files/MiKTeX/miktex/bin/x64:$PATH"
    export TAR="/usr/bin/tar"
    export TAR_OPTIONS="--force-local"
    ```

    If MiKTeX was installed just for your user you might need to run:

    ```sh
    export PATH="/c/Users/$USER/AppData/Local/Programs/MiKTeX/miktex/bin/x64:$PATH"
    ```
   
4. Check that all the programs can be found:

    ```sh
    which make gcc pdflatex tar
    ```

    If there is any error you'll need to find where the program is installed and add the corresponding path.
    
5.  Download the latest recommended packages[^02-getting_started-2]:

    ```sh
    cd "$TOP_SRCDIR/src/gnuwin32/"
    "$TOP_SRCDIR/tools/rsync-recommended"
    ```

6.  Build R and the recommended packages:

    ```sh
    make all recommended
    ```

    The recently compiled version of R will be at `$TOP_SRCDIR/bin/`.
    In RStudio you can select that folder and restart it to use the `r-devel` version.

7.  Check that R works as expected:

    ```sh
    make check
    ```

    There are other checks you can run for testing the successful installation of the recommended packages:

    ```sh
    make check-devel
    make check-recommended
    ```


### macOS


## See also

1.  [CRAN official website](https://cran.r-project.org)

2.  [R installation and administration manual](https://cran.r-project.org/doc/manuals/r-patched/R-admin.html)

3.  [R for macOS](https://mac.r-project.org/)

3.  [Tools for R in macOS](https://mac.r-project.org/tools/)

3.  [R for requirements in macOS](https://mac.r-project.org/src/)

3.  [R for Windows FAQ](https://cran.r-project.org/bin/windows/base/rw-FAQ.html)

4.  [Rtools40 manual for Windows](https://cran.r-project.org/bin/windows/Rtools/)

5.  [R FAQ](https://cran.r-project.org/doc/FAQ/R-FAQ.html)
