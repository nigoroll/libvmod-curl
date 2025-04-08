..
.. NB:  This file is machine generated, DO NOT EDIT!
..
.. Edit ./vmod_curl.vcc and run make instead
..

.. role:: ref(emphasis)

=========
vmod_curl
=========

-------------------------
cURL bindings for Varnish
-------------------------

:Manual section: 3

SYNOPSIS
========

.. parsed-literal::

  import curl [as name] [from "path"]
  
  VOID get(STRING)
  
  VOID head(STRING)
  
  VOID fetch(STRING)
  
  VOID post(STRING, STRING)
  
  STRING header(STRING)
  
  VOID free()
  
  INT status()
  
  STRING error()
  
  STRING body()
  
  VOID set_timeout(INT)
  
  VOID set_connect_timeout(INT)
  
  VOID set_ssl_verify_peer(INT)
  
  VOID set_ssl_verify_host(INT)
  
  VOID set_ssl_cafile(STRING)
  
  VOID set_ssl_capath(STRING)
  
  STRING escape(STRING)
  
  STRING unescape(STRING)
  
  VOID header_add(STRING)
  
  VOID header_remove(STRING)
  
  VOID header_add_all()
  
  VOID proxy(STRING)
  
  VOID set_proxy(STRING)
  
  VOID set_method(STRING)
  
  VOID set_unix_path(STRING)
  
  VOID set_debug(ENUM)
  

.. image:: https://travis-ci.org/varnish/libvmod-curl.svg?branch=master
   :alt: Travis CI badge
   :target: https://travis-ci.org/varnish/libvmod-curl/

This vmod provides cURL bindings for Varnish so you can use Varnish
as an HTTP client and fetch headers and bodies from backends.

WARNING: Using vmod-curl to connect to HTTPS sites is currently unsupported
and may lead to segmentation faults on VCL load/unload. (openssl library
intricacies)

Installation
============

Source releases can be downloaded from:

    https://download.varnish-software.com/libvmod-curl/

Installation requires an installed version of Varnish Cache, including the
development files. Requirements can be found in the `Varnish documentation`_.

.. _`Varnish documentation`: https://www.varnish-cache.org/docs/4.1/installation/install.html#compiling-varnish-from-source
.. _`Varnish Project packages`: https://www.varnish-cache.org/releases/index.html

Source code is built with autotools, you need to install the correct
development packages first.
If you are using the official `Varnish Project packages`_::

    sudo apt install varnish-dev || sudo yum install varnish-devel

If you are using the distro provided packages::

    sudo apt install libvarnishapi-dev || sudo yum install varnish-libs-devel

In both cases, you also need the libcurl development package::

    sudo apt install libcurl4-openssl-dev || sudo yum install libcurl-devel

Then proceed to the configure and build::

    ./configure
    make
    make check   # optional
    sudo make install

The resulting loadable modules (``libvmod_*.so`` files) will be installed to
the Varnish module directory. (default `/usr/lib/varnish/vmods/`)

Usage
=====

To use the vmod do something along the lines of::

    import curl;

    sub vcl_recv {
        curl.get("http://example.com/test");
        if (curl.header("X-Foo") == "bar") {
        ...
        }

        curl.free();
    }

# GET the URL in the first parameter

.. _curl.get():

VOID get(STRING)
----------------

# HEAD the URL in the first parameter

.. _curl.head():

VOID head(STRING)
-----------------

# Compatibility name for get

.. _curl.fetch():

VOID fetch(STRING)
------------------

# POST the URL in the first parameter with the body fields given in
# the second

.. _curl.post():

VOID post(STRING, STRING)
-------------------------

# Return the header named in the first argument

.. _curl.header():

STRING header(STRING)
---------------------

# Free the memory used by headers. Not needed, will be handled
# automatically if it's not called.

.. _curl.free():

VOID free()
-----------

# The HTTP status code

.. _curl.status():

INT status()
------------



.. _curl.error():

STRING error()
--------------

# A response body can contain chars that are not allowed into headers,
# e.g. CRLF. If the response body is a binary and/or it contains any
# special chars, then this funtion MUST be used via synthetic:
# synthetic(curl.body()). Otherwise it can be assigned to a header
# resp.http.x-body = curl.body();
# Test 12 for a complete example.

.. _curl.body():

STRING body()
-------------

# set_timeout and set_connect_timeout are not
# global, but per request functions, therefore
# they can't be used in vcl_init. 

.. _curl.set_timeout():

VOID set_timeout(INT)
---------------------



.. _curl.set_connect_timeout():

VOID set_connect_timeout(INT)
-----------------------------



.. _curl.set_ssl_verify_peer():

VOID set_ssl_verify_peer(INT)
-----------------------------



.. _curl.set_ssl_verify_host():

VOID set_ssl_verify_host(INT)
-----------------------------



.. _curl.set_ssl_cafile():

VOID set_ssl_cafile(STRING)
---------------------------



.. _curl.set_ssl_capath():

VOID set_ssl_capath(STRING)
---------------------------



.. _curl.escape():

STRING escape(STRING)
---------------------



.. _curl.unescape():

STRING unescape(STRING)
-----------------------

# Add / Remove request headers

.. _curl.header_add():

VOID header_add(STRING)
-----------------------



.. _curl.header_remove():

VOID header_remove(STRING)
--------------------------

# Add all request headers from the req (or bereq) object

.. _curl.header_add_all():

VOID header_add_all()
---------------------



.. _curl.proxy():

VOID proxy(STRING)
------------------



.. _curl.set_proxy():

VOID set_proxy(STRING)
----------------------



.. _curl.set_method():

VOID set_method(STRING)
-----------------------



.. _curl.set_unix_path():

VOID set_unix_path(STRING)
--------------------------



.. _curl.set_debug():

VOID set_debug(ENUM)
--------------------

::

   VOID set_debug(
      ENUM {none, text, header_in, header_out, data_in, data_out}
   )

Development
===========

The source git tree lives on Github: https://github.com/varnish/libvmod-curl

All source code is placed in the master git branch. Pull requests and issue
reporting are appreciated.

Unlike building from releases, you need to first bootstrap the build system
when you work from git. In addition to the dependencies mentioned in the
installation section, you also need to install the build tools::

    sudo apt-get automake autotools-dev python-docutils

Then build the vmod::

    ./autogen.sh
    ./configure
    make
    make check # recommended

If the ``configure`` step succeeds but the ``make`` step fails, check for
warnings in the ``./configure`` output or the ``config.log`` file. You may be
missing bootstrap dependencies not required by release archives.

If you have installed Varnish to a non-standard directory, call ``autogen.sh``
and ``configure`` with ``PKG_CONFIG_PATH`` and ``ACLOCAL_PATH`` pointing to
the appropriate path. For instance, when varnishd configure was called with
``--prefix=$PREFIX``, use::

    export PKG_CONFIG_PATH=$PREFIX/lib/pkgconfig
    export ACLOCAL_PATH=$PREFIX/share/aclocal

--

Development of this VMOD has been sponsored by the Norwegian company
Aspiro Music AS for usage on their WiMP music streaming service.

.. _`Varnish Project packages`: https://www.varnish-cache.org/releases/index.html
