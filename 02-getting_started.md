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

## Prerequisites  {.tabset}

To install from the source code you will need the source code and the dependencies of R.

Retrieve R source code via into `TOP_SRCDIR`, note that we retrieve the r-devel source code:

      export TOP_SRCDIR="$HOME/Downloads/R"
      svn checkout https://svn.r-project.org/R/trunk/ $TOP_SRCDIR

If you need to install svn you can use your distribution's package manager to install it.

### Ubuntu

In Ubuntu you can use :

      sudo apt-get install -y \
          bash-completion \
          bison \
          debhelper-compat (= 11) \
          default-jdk \
          g++ (>= 4:4.9.2-2) \
          gcc (>= 4:4.9.2-2) \
          gfortran (>= 4:4.9.2-2) \
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
          tcl8.6-dev \
          texinfo (>= 4.1-2) \
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


### Windows

Install MSYS and rtools42

### macOS


## Building R in Linux

Here are the basic steps intended as a checklist.
For complete instructions please see the section in [R-admin](https://cran.r-project.org/doc/manuals/r-devel/R-admin.html#Installing-R-under-Unix_002dalikes).

1.  Download the latest recommended packages[^02-getting_started-2]:

        $TOP_SRCDIR/tools/rsync-recommended

3.  Create the build directory in the `BUILDDIR`:

        export BUILDDIR="$HOME/bin/R"
        mkdir -p $BUILDDIR
        cd $BUILDDIR
        
4.  Configure the R installation (with `--enable-R-shlib` so that RStudio IDE can use it):
        
        $TOP_SRCDIR/configure --enable-R-shlib
        
4.  Build R :

        make 


5.  Check that R works as expected:

        make check

    There are other checks you can run:

        make check-devel
        make check-recommended

[^02-getting_started-2]: Recommended packages are not in the subversion repository.

If we don't want to build R in a different directory than the source code we can simply use:

    cd $TOP_SRCDIR 
    svn update
    tools/rsync-recommended
    $TOP_SRCDIR/configure  --enable-R-shlib
    make 
    make check

Once you successfully build R from source you can modify R source code to fix an issue: Prepare a patch (See [this guide](https://www.r-project.org/bugs.html#how-to-submit-patches)) and after checking that R works as intended (`make check-devel`) submit the patch for the R core consideration.
(See the [lifecycle of a patch](#FixBug) chapter).


To use the r-devel version in RStudio to you can do the following:

    export RSTUIDO_WHICH_R="$BUILDDIR/bin/R"
    cd $TOP_SRCDIR
    rstudio


## Windows

### Installing R {#installR}

1.  The binary builds of R for Windows can be downloaded and installed from [here](https://cran.r-project.org/bin/windows/base/).
    Along with the link to the latest stable release, this page also contains links to the binary builds of r-patched and r-devel.

2.  Click on the download links to download an executable installer.

3.  Select the language while installing, read the GNU general public license information, and select the destination location to start the installation.
    You will be prompted to select components at this stage: `User installation`, `32-bit User installation`, `64-bit User installation`, or `Custom installation`.
    The default option may be chosen for the questions from this step onwards to complete the installation.

### Building R and R packages

#### What tools do you need to build R from source on Windows?

1.  [RTools](https://github.com/r-windows/docs/blob/master/faq.md#what-is-rtools) is the toolchain bundle that you can use to build R base and R packages containing compiled code, on Windows.

2.  You also need a distribution of LaTeX installed for building R and checking packages.
    The MiKTeX distribution of LaTeX that is used on CRAN can be downloaded from <https://miktex.org>.

#### How to setup `RTools`?

1.  The latest version of `RTools` can be downloaded from <https://cran.r-project.org/bin/windows/Rtools/> and run in the Windows-style installer.
    You will need to know if you have a 32-bit or 64-bit Windows machine (right-click `This PC` in Windows Explorer and check the properties if you are unsure).

2.  Don't forget to add `RTools` to the path as documented on the download page.

#### How to build R?

To build R for Windows using `RTools` follow the instructions in this [README](https://github.com/r-windows/r-base#readme) file.
There are two options available to build R.
One is the quick development build and the other option is the full installer build.

For development and testing, you need only the quick development build.
The quick build avoids building the manuals, which are generally not needed for development and testing.

However, even for the quick build there are some [default requirements](https://github.com/r-windows/r-base/blob/master/quick-build.sh).
For instance, MikTeX is to be installed in `C:/Program Files` and you have 64-bit R.
If necessary, these defaults can be customised.
The installation path of MikTex can be customised [here](https://github.com/r-windows/r-base/blob/50a229fc76c50a5fb42c0daa367466aaf2318171/quick-build.sh#L13) whereas the Windows bit can be customised [here](https://github.com/r-windows/r-base/blob/50a229fc76c50a5fb42c0daa367466aaf2318171/quick-build.sh#L6).

If you are a maintainer of the Windows CRAN releases then, the full installer build is available for building the complete installer as it appears on CRAN.
It will build both the 32-bit and 64-bit R, the pdf manuals, and the installer program.
You will use this to create the binary builds and not when building R from the source yourself.

### How to download the R sources directly or from the svn repository?

-   To download the R sources on Windows, you can use `tar` from the RStudio terminal.

-   If you want to checkout the sources from svn, it is probably best to install an SVN client.
    Either TortoiseSVN (<https://tortoisesvn.net/>, command line tool, and Windows Explorer integration) or SlikSVN (<https://sliksvn.com/download/>, just the command line tool) is recommended.
    They have simple Windows installers and you can use svn straight-away from Windows cmd or RStudio terminal.

## See also

1.  [CRAN official website](https://cran.r-project.org)

2.  [R installation and administration manual](https://cran.r-project.org/doc/manuals/r-patched/R-admin.html)

3.  [R for Windows FAQ](https://cran.r-project.org/bin/windows/base/rw-FAQ.html)

4.  [Rtools40 manual for Windows](https://cran.r-project.org/bin/windows/Rtools/)

5.  [R FAQ](https://cran.r-project.org/doc/FAQ/R-FAQ.html)
