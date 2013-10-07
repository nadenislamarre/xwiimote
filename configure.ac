#
# XWiimote - configure
# Written by David Herrmann, 2011
# Dedicated to the Public Domain
#

AC_PREREQ(2.68)

AC_INIT([xwiimote],
        [2],
        [http://github.com/dvdhrm/xwiimote/issues],
        [xwiimote],
        [http://dvdhrm.github.io/xwiimote])
AC_CONFIG_SRCDIR([lib/xwiimote.h])
AC_CONFIG_AUX_DIR([build-aux])
AC_CONFIG_MACRO_DIR([m4])
AC_CONFIG_HEADER(config.h)
AC_USE_SYSTEM_EXTENSIONS
AC_SYS_LARGEFILE

AM_INIT_AUTOMAKE([foreign 1.11 subdir-objects dist-xz no-dist-gzip tar-pax -Wall -Werror -Wno-portability])
AM_SILENT_RULES([yes])

# Don't add a default "-g -O2" if CFLAGS wasn't specified
: ${CFLAGS=""}

AC_PROG_CC
AC_PROG_CC_C99
AM_PROG_CC_C_O
m4_ifdef([AM_PROG_AR], [AM_PROG_AR])

LT_PREREQ(2.2)
LT_INIT

PKG_CHECK_MODULES([UDEV], [libudev])
AC_SUBST(UDEV_CFLAGS)
AC_SUBST(UDEV_LIBS)

PKG_CHECK_MODULES([NCURSES], [ncurses])
AC_SUBST(NCURSES_CFLAGS)
AC_SUBST(NCURSES_LIBS)

AC_MSG_CHECKING([whether to build with debugging on])
AC_ARG_ENABLE([debug],
              [AS_HELP_STRING([--enable-debug], [whether to build with debugging on)])],
              [debug="$enableval"],
              [debug=no; AC_DEFINE([NDEBUG], [1], [No Debug])])
AM_CONDITIONAL([DEBUG], [test x$debug = xyes])
AC_MSG_RESULT([$debug])

#
# Check for swig / python
#
swig_enabled=yes
swig_explain=
swig_python_enabled=yes
swig_python_explain=
swig_perl_enabled=yes
swig_perl_explain=
AC_ARG_ENABLE(swig,
   [--disable-swig don't build the SWIG bindings.],
   swig_enabled=$enableval)

if test x$swig_enabled = xyes; then
   AC_CHECK_PROG([SWIG], [swig], [swig])
   if test x$SWIG = x; then
     swig_enabled=no
     swig_explain="(swig not found)"
     swig_python_enabled=no
     swig_perl_enabled=no
   else
     AM_PATH_PYTHON(, [have_python="yes"], [have_python="no"])
     if test x$have_python = xyes
     then
       AC_CHECK_PROGS([PYTHON_CONFIG], [python-config python$PYTHON_VERSION-config], [no])
       if test x$PYTHON_CONFIG != xno
       then
               BCK_CPPFLAGS=$CPPFLAGS
               PYTHON_INCLUDE=$($PYTHON_CONFIG --includes)
               CPPFLAGS=$PYTHON_INCLUDE
               AC_CHECK_HEADER(Python.h, [have_python_headers="yes"], [have_python_headers="no"])
               CPPFLAGS=$BCK_CFLAGS
       fi
     fi
     if test x$have_python_headers != xyes; then
       swig_python_enabled=no
       swig_python_explain="(python headers not found)"
     fi
     if test x$have_python != xyes; then
       swig_python_enabled=no
       swig_python_explain="(python not found)"
     fi

     AC_PATH_PROG([PERL], [perl], [])
     if test x$PERL = x; then
       swig_perl_enabled=no
       swig_perl_explain="(perl not found)"
     fi
   fi
else
  swig_enabled=no
  swig_explain="(swig disabled)"
  swig_python_enabled=no
  swig_perl_enabled=no
fi
AM_CONDITIONAL(HAVE_SWIG, test "x$swig_enabled" = xyes)
AM_CONDITIONAL(HAVE_PYTHON, test "x$swig_python_enabled" = xyes)
AM_CONDITIONAL(HAVE_PERL, test "x$swig_perl_enabled" = xyes)

#
# Check for doxygen
#

AC_ARG_VAR([DOXYGEN], [Path to doxygen command])
AC_ARG_WITH(doxygen,
            AS_HELP_STRING([--with-doxygen],
                           [Use doxygen to generate documentation]),
            [use_doxygen=$withval],
            [use_doxygen=auto])
have_doxygen=no
if test ! "x$use_doxygen" = "xno" ; then
        AC_PATH_PROG([DOXYGEN], [doxygen])
        if test "x$DOXYGEN" = "x" ; then
                if test "x$use_doxygen" = "xyes" ; then
                        AC_MSG_ERROR([--with-doxygen=yes specified but doxygen not found in PATH])
                fi
        else
                have_doxygen=yes
        fi
fi
AM_CONDITIONAL([HAVE_DOXYGEN], [test "$have_doxygen" = yes])

#
# Write configuration
#

AC_CONFIG_FILES([Makefile libxwiimote.pc doc/Doxyfile swig/python/Makefile swig/python/setup.py swig/perl/Makefile swig/perl/Makefile.PL])
AC_OUTPUT

AC_MSG_NOTICE([Build configuration:

               prefix: $prefix
          exec-prefix: $exec_prefix
               libdir: $libdir
           includedir: $includedir

                debug: $debug
              doxygen: $have_doxygen
       python binding: $swig_python_enabled $swig_explain$swig_python_explain
         perl binding: $swig_perl_enabled $swig_explain$swig_perl_explain

        Run "${MAKE-make}" to start compilation process])
