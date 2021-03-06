# -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.
AC_PREREQ(2.59)

AC_INIT([gst-plugins-fsl-vpu], [2012.03.1], [bugs@pengutronix.de])
AC_CONFIG_SRCDIR([src/mfw_gst_vpu_encoder.c])
AC_CONFIG_MACRO_DIR([config/m4])
AC_CONFIG_AUX_DIR([config/autoconf])
AC_CANONICAL_BUILD
AC_CANONICAL_HOST

AM_MAINTAINER_MODE

#
# libtool library versioning stuff
#
# Library code modified:	    REVISION++
# Interfaces changed/added/removed: CURRENT++	REVISION=0
# Interfaces added:		    AGE++
# Interfaces removed:		    AGE=0
LT_CURRENT=0
LT_REVISION=0
LT_AGE=0
AC_SUBST(LT_CURRENT)
AC_SUBST(LT_REVISION)
AC_SUBST(LT_AGE)


#
# Checks for programs.
#
AC_PROG_CC
#AM_MISSING_PROG(PERL, perl, $missing_dir)
# libtool, old:
AC_LIBTOOL_WIN32_DLL
#AC_LIBTOOL_TAGS([])
AC_PROG_LIBTOOL
# libtool, new:
# LT_INIT(win32-dll)

AM_INIT_AUTOMAKE([foreign no-exeext dist-bzip2])


#
# Checks for libraries.
#

##
# librn
##
# REQUIRES_LIBRN="librn >= 0.4.2"
# AC_SUBST(REQUIRES_LIBRN)
# PKG_CHECK_MODULES([librn],
# 	[${REQUIRES_LIBRN}],
# 	[],
# 	[AC_MSG_ERROR([*** ${REQUIRES_LIBRN} not found by pkg-config on your system])]
# )
# AC_SUBST(librn_CFLAGS)
# AC_SUBST(librn_LIBS)

#
# Checks for header files.
#
AC_HEADER_DIRENT
AC_HEADER_STDC
AC_HEADER_SYS_WAIT
AC_CHECK_HEADERS([	\
	arpa/inet.h	\
	limits.h	\
	netdb.h		\
	netinet/in.h	\
	stddef.h	\
	stdlib.h	\
	string.h	\
	sys/param.h	\
	sys/socket.h	\
	sys/time.h	\
	sys/un.h	\
	unistd.h	\
	utime.h		\
	])


#
# Checks for typedefs, structures, and compiler characteristics.
#
AC_C_CONST
AC_C_INLINE
AC_TYPE_SIZE_T
AC_HEADER_TIME


#
# Checks for library functions.
#
AC_FUNC_MEMCMP
AC_TYPE_SIGNAL
AC_FUNC_STAT
AC_FUNC_UTIME_NULL
AC_CHECK_FUNCS([gethostbyaddr gethostbyname gethostname gettimeofday memset mkdir socket utime])


#
# Debugging
#
AC_MSG_CHECKING([whether to enable debugging])
AC_ARG_ENABLE(debug,
    AS_HELP_STRING([--enable-debug], [enable debugging @<:@default=yes@:>@]),
	[case "$enableval" in
	y | yes) CONFIG_DEBUG=yes ;;
        *) CONFIG_DEBUG=no ;;
    esac],
    [CONFIG_DEBUG=yes])
AC_MSG_RESULT([${CONFIG_DEBUG}])
if test "${CONFIG_DEBUG}" = "yes"; then
    CFLAGS="${CFLAGS} -Werror -Wfloat-equal -Wformat -Wformat-security -g -O1"
    AC_DEFINE(DEBUG, 1, [debugging])
else
    CFLAGS="${CFLAGS} -O2"
fi

dnl Check for pkgconfig first
AC_CHECK_PROG(HAVE_PKGCONFIG, pkg-config, yes, no)

dnl Give error and exit if we don't have pkgconfig
if test "x$HAVE_PKGCONFIG" = "xno"; then
  AC_MSG_ERROR(you need to have pkgconfig installed !)
fi

dnl Now we're ready to ask for gstreamer libs and cflags
dnl And we can also ask for the right version of gstreamer

GST_REQUIRED=0.10

GST_MAJORMINOR=0.10
PKG_CHECK_MODULES(GST, \
  gstreamer-$GST_MAJORMINOR >= $GST_REQUIRED)

dnl make GST_CFLAGS and GST_LIBS available
AC_SUBST(GST_CFLAGS)
AC_SUBST(GST_LIBS)

dnl make GST_MAJORMINOR available in Makefile.am
AC_SUBST(GST_MAJORMINOR)

dnl If we need them, we can also use the plugin libraries
PKG_CHECK_MODULES(GST_PLUGINS_BASE, \
  gstreamer-plugins-base-$GST_MAJORMINOR >= $GST_REQUIRED)

dnl make GST_LIBS_CFLAGS and GST_LIBS_LIBS available
AC_SUBST(GST_PLUGINS_BASE_CFLAGS)
AC_SUBST(GST_PLUGINS_BASE_LIBS)

dnl set the plugindir where plugins should be installed
plugindir="\$(libdir)/gstreamer-$GST_MAJORMINOR"
AC_SUBST(plugindir)

dnl set proper LDFLAGS for plugins
GST_PLUGIN_LDFLAGS='-module -avoid-version -export-symbols-regex [_]*\(gst_\|Gst\|GST_\).*'
AC_SUBST(GST_PLUGIN_LDFLAGS)

AC_CONFIG_FILES([
	GNUmakefile
	config/gst-plugins-fsl-vpu.pc
	config/GNUmakefile
	include/GNUmakefile
	src/GNUmakefile
	])
AC_OUTPUT

