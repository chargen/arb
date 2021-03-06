.. _setup:

Setup
===============================================================================

Package managers
-------------------------------------------------------------------------------

The easiest way to install Arb including all dependencies is via ready-made
packages available for various distributions.
Note that some packages may not be up to date.

* Debian / Ubuntu / Linux Mint

  - https://packages.debian.org/source/sid/flint-arb

* Fedora

  - https://admin.fedoraproject.org/pkgdb/package/rpms/arb/

* Arch Linux

  - https://www.archlinux.org/packages/community/x86_64/arb/

  - https://www.archlinux.org/packages/community/i686/arb/

* Guix

  - https://www.gnu.org/software/guix/packages/

Download
-------------------------------------------------------------------------------

Tarballs of released versions can be downloaded from https://github.com/fredrik-johansson/arb/releases

Alternatively, you can simply install Arb from a git checkout of https://github.com/fredrik-johansson/arb/.
The master branch is generally safe to use (the test suite should pass at all
times), and recommended for
keeping up with the latest improvements and bug fixes.

Dependencies
-------------------------------------------------------------------------------

Arb has the following dependencies:

* Either MPIR (http://www.mpir.org) 2.6.0 or later, or GMP (http://www.gmplib.org) 5.1.0 or later.
  If MPIR is used instead of GMP, it must be compiled with the ``--enable-gmpcompat`` option.
* MPFR (http://www.mpfr.org) 3.0.0 or later.
* FLINT (http://www.flintlib.org) version 2.4 or later. You may also
  use a git checkout of https://github.com/fredrik-johansson/flint2


Standalone installation
-------------------------------------------------------------------------------

To compile, test and install Arb from source as a standalone library,
first install FLINT. Then go to the Arb source directory and run::

    ./configure <options>
    make
    make check       (optional)
    make install

If GMP/MPIR, MPFR or FLINT is installed in some other location than
the default path ``/usr/local``, pass
``--with-gmp=...``, ``--with-mpfr=...`` or ``--with-flint=...`` with
the correct path to configure (type ``./configure --help`` to show
more options).

After the installation, you may have to run ``ldconfig``.

Installation as part of FLINT (deprecated)
-------------------------------------------------------------------------------

WARNING: this feature is being deprecated. Please install Arb as a separate
library, as detailed above.

With some versions of FLINT, Arb can be compiled as a FLINT
extension package.

Simply put the Arb source directory somewhere, say ``/path/to/arb``.
Then go to the FLINT source directory and build FLINT using::

    ./configure --extensions=/path/to/arb <other options>
    make
    make check       (optional)
    make install

This is convenient, as Arb does not need to be
configured or linked separately. Arb becomes part of the compiled FLINT
library, and the Arb header files will be installed along with the other
FLINT header files.

Building with MSVC
-------------------------------------------------------------------------------

To compile arb with MSVC, compile MPIR, MPFR, pthreads-win32 and FLINT using
MSVC. Install CMake >=2.8.12 and make sure it is in the path. Then go to the Arb
source directory and run::

    mkdir build
    cd build
    cmake ..                                            # configure
    cmake --build . --config Release                    # build
    cmake --build . --config Release --target install   # install

To build a Debug build, create a new build directory and pass
``-DCMAKE_BUILD_TYPE=Debug`` to ``cmake``. To create a dll library, pass
``-DBUILD_SHARED_LIBS=yes`` to ``cmake``. Note that creating a dll library
requires CMake >= 3.5.0

If the dependencies are not found, pass ``-DCMAKE_PREFIX_PATH=/path/to/deps``
to ``cmake`` to find the dependencies.

To build tests add, pass ``-DBUILD_TESTS=yes`` to ``cmake`` and run `ctest`
to run the tests.

Running code
-------------------------------------------------------------------------------

Here is an example program to get started using Arb:

.. code-block:: c

    #include "arb.h"

    int main()
    {
        arb_t x;
        arb_init(x);
        arb_const_pi(x, 50 * 3.33);
        arb_printn(x, 50, 0); flint_printf("\n");
        flint_printf("Computed with arb-%s\n", arb_version);
        arb_clear(x);
    }

Compile it with::

    gcc test.c -larb

Depending on the environment, you may also have to pass
the flags ``-lflint``, ``-lmpfr``, ``-lgmp`` to the compiler.

If the Arb/FLINT header and library files are not in a standard location
(``/usr/local`` on most systems), you may also have to provide flags such as::

    -I/path/to/arb -I/path/to/flint -L/path/to/flint -L/path/to/arb

Finally, to run the program, make sure that the linker
can find the FLINT (and Arb) libraries. If they are installed in a
nonstandard location, you can for example add this path to the
``LD_LIBRARY_PATH`` environment variable.

The output of the example program should be something like the following::

    [3.1415926535897932384626433832795028841971693993751 +/- 6.28e-50]
    Computed with arb-2.4.0

