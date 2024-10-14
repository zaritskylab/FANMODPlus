========================
Compiling ``fanmod-cmd``
========================

``fanmod`` relied on the `nauty <https://www3.cs.stonybrook.edu/~algorith/implement/nauty/implement.shtml>`_ library which has moved on from the version initially used for `fanmod` (2.2). Later
versions of `nauty` followed the autoconf/automake GNU practice resulting in finer control over the final build. ``fanmod`` also relied on a simple bash script that directed the whole compilation
procedure.

This mini guide provides more details around some additions to `fanmod_cmd` to make the whole compilation process more convenient, especially if you plan to make use of the code in `fanmod` and 
would therefore require to compile it, possible more than once.

Quickstart
==========
 
The whole process of compiling `fanmod` is split into the following basic steps:


1. Fetch and compile ``nauty``
    * ``> make nauty`` from your ``fanmod_cmd`` root folder.
    
2. Edit ``fanmod_cmd/nautyinit.h``
    * Edit ``nautyinit.h`` so that ``fanmod_cmd`` makes use of a specific version's ``nauty.h, nautyinv.h`` files.

3. Compile ``fanmod_cmd``
    * ``> make fanmod_cmd`` from your ``fanmod_cmd`` root folder


* More details for each step* are provided in the following sections.

.. note::
    To compile ``fanmod_cmd`` with ``nauty`` version **26r12**, just run ``> make all`` from the root folder of ``fanmod_cmd``.



1. Fetch and compile ``nauty``
==============================

The initial version of ``fanmod`` redistributed ``nauty`` 2.2 along with its code. Since ``nauty`` now uses ``autoconf/automake`` it is possible to 
make the whole process of obtaining and compiling it automatic. Furthermore, later versions of `nauty` produced libraries with different optimisation 
settings as part of the basic build, which makes experimentation easier.

Downloading the latest version of ``fanmod_cmd`` and simply ``> make nauty`` will fetch version ``nauty27r3`` which is the latest at the time of writing.

Assuming that ``nauty`` is available over a URL and uses ``autoconf/automake``, it is possible to fully customise which version is downloaded and linked 
into ``fanmod_cmd`` from the ``makefile`` at the project's root folder.

* The URL to obtain ``nauty`` from is determined by variable ``NAUTY_URL_BASE``
* The archive file **name** to download is determined by variable ``NAUTY_ARCHIVE_BASE``

The makefile contains inline comments that assist in customising it further, but these are the two variables that determine which archive to download.
Once the archive has been downloaded and extracted (again, without any further intervention), the usual ``autoconf/automake`` process is triggered.



Details
-------

``fanmod_cmd`` triggers the ``checks`` makefile rule of ``nauty`` which builds all the software provided by ``nauty`` and also runs its tests. The result
of this process is (amongst other executables) six "flavours" of ``nauty``, each one optimised for analysis of networks of different orders of magnitude in terms of size.

1. ``nauty.a  : WORDSIZE = default, unlimited MAXN``
2. ``nauty1.a : WORDSIZE = default, MAXN = WORDSIZE``
3. ``nautyW.a : WORDSIZE = 32, unlimited MAXN``
4. ``nautyW1.a: WORDISZE = MAXN = 32``
5. ``nautyL.a : WORDSIZE = 64, unlimited MAXN``
6. ``nautyL1.a: WORDSIZE = MAXN = 64``

``WORDSIZE`` and ``MAXN`` determine the sizes of basic data structures that ``nauty`` uses to operate. For more 
information about those please see section 16 of the manual (``nauty/nauty[version]/nug[version].pdf`` that is included along with your release of the ``nauty`` library.

.. note::
    By default, ``fanmod_cmd`` links against ``nautyL1.a`` but this can be changed at line 36 of the makefile.


2. Edit ``fanmod_cmd/nautyinit.h``
==================================

This is a standard step in including `nauty` in another piece of software. ``nautyinit.h`` looks like:

.. code:: C
    #ifndef NAUTY_H
    #define NAUTY_H

    #define MAXN 64

    extern "C" {
        #include "nauty/nauty27r3/nauty.h"
        #include "nauty/nauty27r3/nautinv.h"
    }

    #endif


It is probably self-explanatory what to change here to link against a specific version of ``nauty``. The latest version of ``fanmod_cmd`` should link 
against the latest version of ``nauty`` anyway, but this is easy to change.

Also, notice the definition of ``MAXN`` in this file. It should match the ``MAXN`` of the ``nauty`` "flavour" you link against.


3. Compile ``fanmod_cmd``
=========================

Either ``> make fanmod`` to compile just ``fanmod`` and link it against ``nauty`` or ``> make all`` to compile everything in one go.

.. note::
    A "debug" version is built by default and then the debug symbols are striped on line 37 of the makefile. Comment that line out to be able to use a debugger on the executable.

Details
-------

The `fanmod_cmd` makefile defines the following targets:

1. ``clean`` to clean up both ``nauty`` and ``fanmod_cmd``.
2. ``clean_fanmod`` and ``clean_nauty`` to only clean the compilation artifacts of one of the components.
3. ``fanmod`` and ``nauty`` to only build one of the components.
4. ``all`` to build everything.



Other resources
===============

1. `The fanmod manual <resources/fanmod-manual.pdf>`_
    * This manual used to be available separately from the project's page.

2. S. Wernicke and F. Rasche, ‘FANMOD: a tool for fast network motif detection’, Bioinformatics, vol. 22, no. 9, pp. 1152–1153, May 2006, doi: 10.1093/bioinformatics/btl038.
    * This paper is openly available from `the publisher <https://academic.oup.com/bioinformatics/article/22/9/1152/199945>`_.

3. S. Wernicke, ‘A Faster Algorithm for Detecting Network Motifs’, in Algorithms in Bioinformatics, vol. 3692, R. Casadio and G. Myers, Eds. Berlin, Heidelberg: Springer Berlin Heidelberg, 2005, pp. 165–177. doi: 10.1007/11557067_14.
    * A cached version of this paper is available `via CiteSeer <https://citeseerx.ist.psu.edu/viewdoc/similar?doi=10.1.1.118.636&type=cc>`_
    * A copy of that paper is also available in this repository at `doc/resources/network-motifs-wabi05.pdf <resources/network-motifs-wabi05.pdf>`_

4. `The nauty and traces homepage <https://pallini.di.uniroma1.it/>`_
