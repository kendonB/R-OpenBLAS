|Appveyor Build Status|

Overview
========

This project is used to compile R_ on Windows using the fast openblas_
BLAS library as a fast alternative to R's native BLAS.

Acknowledgement
===============

First, I am grateful to Avraham Adler for his excellent `blog post`_ covering
this topic. If you want to read his script--which this package mostly follows--
please do so.

Procedure
=========

Prerequisites:

The key difficultly in getting this to work is R is built using the GNU tools
which have sometimes spotty and confusing windows support. However, the key
elemenets are that you need to download the following:

- Rtools_ Used for the actual ``gcc`` and ``gfortran`` compilers.
- `MiKTeX Portable`_ R builds a ton of documentation as HTML and PDF files.
- Appveyor_ Is an online continuous integration system that essentially runs Windows buils.
- openblas_ is needed. A version specfically compiled for R for Windows can be found here: https://github.com/thequackdaddy/OpenBLASR

Strategy:

The GNU make tools can be configured using environment variables. For the most
part, that's the approach I have taken. The environment variables are configured
using the ``appveyor.yml`` file. In addition, one file must be edited/copied
because it explicitly uses driver names which are not used for Windows R. This
file is:

``src/extra/blas/Makefile.win``

The name of the BLAS library file--something like
``openblas_haswellp-r0.2.20``--needs to replace the currently existing
references. Note that the ``lib`` that starts the file and the ``.a`` extension
are expected by the compiler and so the makefile should only need all the
characters after the ``lib`` and before the ``.a``

LaTeX is required for the build to complete. I've used `MiKTeX Portable`_ only
because its easy to download and install. The LaTeX bin directory needs to be
added to the PATH. MiKTeX also must download smörgåsbord of extra packages--
and must do so without user interaction. There's a ``initexmf`` command
which can help set that up.

Additionally, QPDF_ is needed for processing some of the completed PDFs. The
location of the install can be specified in the environment variables
as part of the installation. QPDF is auto-downloaded from its GitHub releases
page.

A compatible openblas_ library is required. You can make your own--and
compile for a processor other than Intel's Haswell family  using the openblasr_
project. A Haswell library is provided in this build for simpolicity. If you
want to download the openblasr file, you may do so here:

https://ci.appveyor.com/project/thequackdaddy/openblasr

Pick the most recent build that succeeded and select the Artifact tab to
download it.

Please note that it is important to set the ``PATH`` so that the Rtools_ ``gcc``
compiler and ``make`` will be found before any other installation on the system.
Additionally, ``pdflatex.exe`` needs to be in the path. ``MiKTeX`` needs
a few additional packages to install everything, so be sure to either download
them or configure ``MiKTeX`` so that dependencies are auto-downloaded and
installed.

Download
========

A compiled 64-bit R, with OpenBLAS compiled for Intel Core i7 processors is
available here:

https://github.com/thequackdaddy/R-OpenBLAS/releases

Select the most recent successful build and download the executable artifact.

.. code-block::

   cd r-source
   git checkout -t origin/tags/<version number>
   cd ..
   git add r-source
   git commit -m "Updated OpenBLAS"
   git push origin <branchname>

.. _openblas: http://www.openblas.net/
.. _R: https://www.r-project.org/
.. _Rtools: https://cran.r-project.org/bin/windows/Rtools/
.. _MSYS2: http://www.msys2.org/
.. _QPDF: https://github.com/qpdf/qpdf
.. _`MiKTeX Portable`: https://miktex.org/
.. _Appveyor: http://appveyor.com/
.. _`blog post`: https://www.avrahamadler.com/r-tips/build-openblas-for-windows-r64/
.. _openblasr: https://github.com/thequackdaddy/openblasr
.. |Appveyor Build Status| image:: https://ci.appveyor.com/api/projects/status/fm8mj3hq6v053gul/branch/master?svg=true
   :target: https://ci.appveyor.com/project/thequackdaddy/r-openblas/
