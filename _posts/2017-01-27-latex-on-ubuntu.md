---
layout: post
title: LaTeX on Ubuntu
description: Best way of installing and configuring LaTeX on Ubutnu
categories: LaTeX, Linux, Ubuntu
---

Installing and configuring LaTeX on Ubuntu

Although [LaTeX](https://www.latex-project.org/) is available via the usual Ubuntu repositories, an alternative method which utilises the [TeX Live Package Manager](http://www.tug.org/texlive/tlmgr.html) is available.

Source: [https://help.ubuntu.com/community/LaTeX](https://help.ubuntu.com/community/LaTeX)

# Installing

For install instructions see [https://www.tug.org/texlive/quickinstall.html](https://www.tug.org/texlive/quickinstall.html).

Steps I have used:

*   Download TeXLive installer from [http://tug.org/texlive/acquire-netinstall.html](http://tug.org/texlive/acquire-netinstall.html).
*   If a previous installation exists (likely to be in `/usr/local/texlive`) remove it first.
*   Run TeXLive installer `sudo ./install-tl`.
*   Select option `C` and deselect any unwanted packages - mainly languages.
*   Make sure to set the option for symbolic links, by selecting `O` for options, and `L` for create symlinks in standard directories.
*   Alternatively use an existing profile, see below for an example, `install-tl --profile=mytexlive.profile`.
*   Add directory of TeX Live binary files to path by creating a new file `/etc/profile.d/texlive2016-env.sh` and adding the following lines:

 ```
TEXLIVE2016_HOME=/usr/local/texlive/2016/
TEXLIVE2016_BIN=$TEXLIVE2016_HOME/bin/x86_64-linux
PATH=$PATH:$TEXLIVE2016_BIN
TEXLIVE2016_INFO=$TEXLIVE2016_HOME/texmf-dist/doc/info
INFOPATH=$INFOPATH:$TEXLIVE2016_INFO
TEXLIVE2016_MAN=$TEXLIVE2016_HOME/texmf-dist/doc/man
MANPATH=$MANPATH:$TEXLIVE2016_MAN
 ```

If there are problems try installing from a different mirror. Alternative mirrors can be found at [http://www.ctan.org/mirrors/mirmon](http://www.ctan.org/mirrors/mirmon).

For example `./install-tl --location http://ctan.math.washington.edu/tex-archive/systems/texlive/tlnet/`.

Example profile:

```
# texlive.profile written on Fri Jan 27 01:05:08 2017 UTC
# It will NOT be updated and reflects only the
# installation profile at installation time.
selected_scheme scheme-custom
TEXDIR /usr/local/texlive/2016
TEXMFCONFIG ~/.texlive2016/texmf-config
TEXMFHOME ~/texmf
TEXMFLOCAL /usr/local/texlive/texmf-local
TEXMFSYSCONFIG /usr/local/texlive/2016/texmf-config
TEXMFSYSVAR /usr/local/texlive/2016/texmf-var
TEXMFVAR ~/.texlive2016/texmf-var
binary_x86_64-linux 1
collection-basic 1
collection-bibtexextra 1
collection-binextra 1
collection-context 1
collection-fontsextra 1
collection-fontsrecommended 1
collection-fontutils 1
collection-formatsextra 1
collection-genericextra 1
collection-genericrecommended 1
collection-htmlxml 1
collection-humanities 1
collection-langenglish 1
collection-latex 1
collection-latexextra 1
collection-latexrecommended 1
collection-mathscience 1
collection-metapost 1
collection-pictures 1
collection-plainextra 1
collection-pstricks 1
collection-publishers 1
in_place 0
option_adjustrepo 1
option_autobackup 1
option_backupdir tlpkg/backups
option_desktop_integration 1
option_doc 1
option_file_assocs 1
option_fmt 1
option_letter 0
option_menu_integration 1
option_path 1
option_post_code 1
option_src 1
option_sys_bin /usr/local/bin
option_sys_info /usr/local/info
option_sys_man /usr/local/man
option_w32_multi_user 1
option_write18_restricted 1
portable 0
```

# Updating

For examples of how to the [TeX Live manager](https://www.tug.org/texlive/tlmgr.html) see [http://tug.org/texlive/doc/tlmgr.html#EXAMPLES](http://tug.org/texlive/doc/tlmgr.html#EXAMPLES).

*   To update the TeX Live manager use `tlmgr update --self`
*   To update all installed LaTeX packages use `tlmgr update --all`.

# GUI

The TeX Live manager supports a GUI interface. In order to use it [Perl/Tk](http://search.cpan.org/~ni-s/Tk/pod/UserGuide.pod) must first be installed using `sudo apt-get install perl-tk`.

To utilise the GUI, use the option `--gui` when running the manager, for example `tlmgr --gui`.
