---
layout: post
title: LaTeX on Ubuntu
description: Best way of installing and configuring LaTeX on Ubutnu
categories: LaTeX, Linux, Ubuntu
---

Installing and configuring LaTeX on Ubuntu

Although [LaTeX](https://www.latex-project.org/) is available via the usual Ubuntu repositories, an alternative method which utilises the [TeX Live Package Manager](http://www.tug.org/texlive/tlmgr.html) is available.

Source: [https://help.ubuntu.com/community/LaTeX](https://help.ubuntu.com/community/LaTeX)

For install instructions see [https://www.tug.org/texlive/quickinstall.html](https://www.tug.org/texlive/quickinstall.html).

Steps I have used:

*   Download TeXLive installer from [http://tug.org/texlive/acquire-netinstall.html](http://tug.org/texlive/acquire-netinstall.html).
*   To install without `sudo`, first create texlive directory and enable permissions, using `sudo mkdir /usr/local/texlive` and `sudo chmod -R a+rw /usr/local/texlive`.
*   Run TeXLive installer `./install-tl`.
*   Select option `C` and deselect any unwanted packages - mainly languages.
*   Make sure to set the option for symbolic links, by selecting `O` for options, and `L` for create symlinks in standard directories.
*   Add directory of TexLive binary files to path ``.

If there are problems try installing from a different mirror. Alternative mirrors can be found at [http://www.ctan.org/mirrors/mirmon](http://www.ctan.org/mirrors/mirmon).

For example `./install-tl --location http://ctan.math.washington.edu/tex-archive/systems/texlive/tlnet/`
