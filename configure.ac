AC_PREREQ(2.64)
AC_COPYRIGHT([Copyright (c) 2011-2016 Varnish Software])
AC_INIT([libvmod-curl], [1.0.4])
AC_CONFIG_MACRO_DIR([m4])
AC_CONFIG_AUX_DIR([build-aux])
AC_CONFIG_HEADERS(config.h)

AM_INIT_AUTOMAKE([foreign color-tests parallel-tests])
AM_SILENT_RULES([yes])

AC_ARG_VAR([RST2MAN], [the program to build manuals from reStructuredText])
AC_CHECK_PROGS(RST2MAN, [rst2man rst2man.py])

PKG_CHECK_MODULES([CURL], [libcurl])

AC_CHECK_HEADERS([sys/stdlib.h])

# Check for python
AC_CHECK_PROGS(PYTHON, [python3 python3.1 python3.2 python2.7 python2.6 python2.5 python2 python], "no")
if test "x$PYTHON" = "xno"; then
	AC_MSG_ERROR([Python is needed to build this vmod, please install python.])
fi

VARNISH_PREREQ([7.5.0])
VARNISH_VMODS([curl])

AM_PROG_AR

LT_PREREQ([2.2.6])
LT_INIT([dlopen disable-static])

AC_CONFIG_FILES([
	Makefile
	src/Makefile
])

AC_OUTPUT

AS_ECHO("
	==== $PACKAGE_STRING ====

	varnish:	$VARNISH_VERSION
	prefix:		$prefix
	vmoddir:	$vmoddir
	vcldir:		$vcldir
	pkgvcldir:	$pkgvcldir

	compiler:	$CC
	cflags:		$CFLAGS
	ldflags:	$LDFLAGS
")
