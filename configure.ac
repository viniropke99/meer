#                                               -*- Autoconf -*-
# Process this file with autoconf to produce a configure script.

VERSION=`cat src/version.h | grep "#define VERSION " | sed -e 's/^.*\"\(.*\)\"/\1/'`

AC_INIT([meer], m4_esyscmd_s([cat src/version.h | cut -d\" -f2]))
AC_CONFIG_SRCDIR([src])
AC_CANONICAL_SYSTEM
AC_GNU_SOURCE
AM_MAINTAINER_MODE([disable])

# Check OS

AC_MSG_CHECKING([host os])

# If no host OS,  try uname

if test -z "$host" ; then
        host="`uname`"
fi

echo -n "installation for $host OS... "


    case "$host" in
        *-*-*freebsd*)
            CFLAGS="${CFLAGS} -D__FreeBSD__"
            CPPFLAGS="${CPPFLAGS} -I/usr/local/include -I/usr/local/include/libnet11"
            LDFLAGS="${LDFLAGS} -L/usr/local/lib -L/usr/local/lib/libnet11"
            ;;
        *-*-openbsd*)
            CFLAGS="${CFLAGS} -D__OpenBSD__"
            CPPFLAGS="${CPPFLAGS} -I/usr/local/include -I/usr/local/include/libnet-1.1"
            LDFLAGS="${LDFLAGS} -L/usr/local/lib -I/usr/local/lib/libnet-1.1"
            ;;
        *darwin*|*Darwin*)
            CFLAGS="${CFLAGS} -D__Darwin__"
            CPPFLAGS="${CPPFLAGS} -I/opt/local/include"
            LDFLAGS="${LDFLAGS} -L/opt/local/lib"
            ;;
        *-*-linux*)
	    CFLAGS="${CFLAGS} -D__Linux__"
            #for now do nothing
            ;;
#        *-*-mingw32*)
#            CFLAGS="${CFLAGS} -DOS_WIN32"
#            LDFLAGS="${LDFLAGS} -lws2_32"
#            WINDOWS_PATH="yes"
#            ;;
#        *-*-cygwin)
#            WINDOWS_PATH="yes"
#            ;;
#        *-*-solaris*)
#            AC_MSG_WARN([support for Solaris/Illumos/SunOS is experimental])
#            LDFLAGS="${LDFLAGS} -lsocket -lnsl"
#            ;;
        *)
            AC_MSG_WARN([unsupported OS this may or may not work])
            ;;
    esac
    AC_MSG_RESULT(ok)



AC_C_BIGENDIAN(AC_SUBST([ENDIAN],[big]),AC_SUBST([ENDIAN],[little]))

AC_ARG_ENABLE(postgresql,
  [  --enable-postgresql     Enable PostgreSQL support.],
  [ POSTGRES="$enableval"],
  [ POSTGRES="no" ]
)

AC_ARG_WITH(libpostgresql_includes,
        [  --with-postgresql-includes=DIR    PostgreSQL include directory],
        [with_postgresql_includes="$withval"],[with_postgresql_includes="no"])

AC_ARG_WITH(postgresql_libraries,
        [  --with-postgresql-libraries=DIR   PostgreSQL library directory],
        [with_postgresql_libraries="$withval"],[with_postgresql_libraries="no"])

if test "x$with_postgresql_includes" != "xno"; then
   CPPFLAGS="${CPPFLAGS} -I${with_postgresql_includes}"
   fi

if test "x$with_postgresql_libraries" != "xno"; then
   LDFLAGS="${LDFLAGS}  -L${with_postgresql_libraries}"
fi


AC_ARG_ENABLE(mysql,
  [  --disable-mysql         Disable MySQL support.],
  [ MYSQL="$enableval"],
  [ MYSQL="yes" ]
)

AC_ARG_WITH(libmysql_includes,
        [  --with-mysql-includes=DIR    MySQL include directory],
        [with_mysql_includes="$withval"],[with_mysql_includes="no"])

AC_ARG_WITH(mysql_libraries,
        [  --with-mysql-libraries=DIR   MySQL library directory],
        [with_mysql_libraries="$withval"],[with_mysql_libraries="no"])

if test "x$with_mysql_includes" != "xno"; then
   CPPFLAGS="${CPPFLAGS} -I${with_mysql_includes}"
   fi

if test "x$with_mysql_libraries" != "xno"; then
   LDFLAGS="${LDFLAGS}  -L${with_mysql_libraries}"
fi

AC_ARG_ENABLE(redis,
  [  --enable-redis          Enable Redis support.],
  [ REDIS="$enableval"],
  [ REDIS="no" ]
)

#AC_ARG_ENABLE(libfastjson,
#  [  --disable-libfastjson         Disable libfastjson support.],
#  [ FASTJSON="$enableval"],
#  [ FASTJSON="yes" ]
#)

#AC_ARG_WITH(libfastjson_includes,
#        [  --with-libfastjson-includes=DIR    libfastjson include directory],
#        [with_libfastjson_includes="$withval"],[with_libfastjson_includes="no"])

#AC_ARG_WITH(libfastjson_c_libraries,
#        [  --with-libfastjson-libraries=DIR   libfastjson library directory],
#        [with_libfastjson_libraries="$withval"],[with_libfastjson_libraries="no"])
#
#if test "x$with_libfastjson_includes" != "xno"; then
#   CPPFLAGS="${CPPFLAGS} -I${with_libfastjson_includes}"
#   fi

#if test "x$with_libfastjson_libraries" != "xno"; then
#   LDFLAGS="${LDFLAGS}  -L${with_libfastjson_libraries}"
#fi

AC_SUBST(VERSION)
AM_INIT_AUTOMAKE([subdir-objects])
AC_CONFIG_HEADER(config.h)

# Checks for programs.
AC_PROG_CC
AC_PROG_INSTALL
AC_LANG_C
AC_PROG_MAKE_SET

AM_PROG_CC_C_O

PKG_PROG_PKG_CONFIG

# Checks for header files.
AC_HEADER_STDC
AC_HEADER_SYS_WAIT

AC_CHECK_HEADERS([stdio.h stdlib.h sys/types.h unistd.h stdint.h inttypes.h ctype.h errno.h fcntl.h sys/stat.h string.h getopt.h time.h stdarg.h stdbool.h arpa/inet.h netinet/in.h sys/time.h sys/socket.h sys/mmap.h sys/mman.h])

AC_CHECK_SIZEOF([size_t])

# F_SETPIPE_SZ in fcntl.h
AC_MSG_CHECKING([for F_SETPIPE_SZ in fcntl.h])
AC_EGREP_CPP([yes_have_f_setpipe_sz], [
  #define _GNU_SOURCE
  #include <fcntl.h>
  #ifdef F_SETPIPE_SZ
  yes_have_f_setpipe_sz
  #endif
], [
  AC_MSG_RESULT([yes])
  AC_DEFINE([HAVE_SETPIPE_SZ], [], [F_SETPIPE_SZ is supported])
], [
  AC_MSG_RESULT([no])
  AC_MSG_NOTICE([F_SETPIPE_SZ not found, cannot alter FIFO])
])

# F_GETPIPE_SZ in fcntl.h
AC_MSG_CHECKING([for F_GETPIPE_SZ in fcntl.h])
AC_EGREP_CPP([yes_have_f_getpipe_sz], [
  #define _GNU_SOURCE
  #include <fcntl.h>
  #ifdef F_GETPIPE_SZ
  yes_have_f_getpipe_sz
  #endif
], [
  AC_MSG_RESULT([yes])
  AC_DEFINE([HAVE_GETPIPE_SZ], [], [F_GETPIPE_SZ is supported])
], [
  AC_MSG_RESULT([no])
  AC_MSG_NOTICE([F_GETPIPE_SZ not found, cannot alter FIFO])
])

# Checks for typedefs, structures, and compiler characteristics.
AC_C_CONST
AC_HEADER_TIME

# Checks for library functions.
AC_PROG_GCC_TRADITIONAL
AC_FUNC_MALLOC
AC_FUNC_SELECT_ARGTYPES
AC_FUNC_STAT
AC_TYPE_SIGNAL
AC_FUNC_STRFTIME
AC_FUNC_FORK
AC_FUNC_REALLOC
AC_TYPE_PID_T
AC_TYPE_SIZE_T
AC_STRUCT_TM 

#AX_EXT
AM_PROG_AS

AC_CHECK_FUNCS([select strstr strchr strcmp strlen sizeof write snprintf strncat strlcat strlcpy getopt_long gethostbyname socket htons connect send recv strspn memset access ftruncate strerror mmap shm_open gettimeofday])

AC_ARG_ENABLE(quadrant,                                                                               
[  --enable-quadrant       Enable Quadrant Information Security specific support],
       enable_quadrant="$enableval", enable_quadrant="no")

if test "x$enable_quadrant" = "xyes"; then
    CPPFLAGS="$CPPFLAGS -DQUADRANT"
fi

# json-c

AC_ARG_WITH(libjsonc_includes,
        [  --with-libjsonc-includes=DIR  libjson-c include directory],
        [with_libjsonc_includes="$withval"],[with_libjsonc_includes=no])
AC_ARG_WITH(libjsonc_libraries,
        [  --with-libjsonc-libraries=DIR    libjson-c library directory],
        [with_libjsonc_libraries="$withval"],[with_libjsonc_libraries="no"])

if test "$with_libjsonc_includes" != "no"; then
    CPPFLAGS="${CPPFLAGS} -I${with_libjsonc_includes}"
fi

AC_CHECK_HEADER(json-c.h,,LIBJSONC="no")

if test "$with_libjsonc_libraries" != "no"; then
    LDFLAGS="${LDFLAGS} -L${with_libjsonc_libraries}"
fi

LIBJSONC=""
AC_CHECK_LIB(json-c,main,,LIBJSON="no")

if test "$LIBJSONC" = "no"; then
    echo
    echo "   ERROR! Required libjson-c library not found, go get it."
    echo
    echo "   Debian/Ubuntu: apt-get install libjson-c-dev"
    echo
    exit 1
fi

AC_CHECK_LIB(json-c, main,,AC_MSG_ERROR(Meer needs libjson-c!))

# libyaml

AC_ARG_WITH(libyaml_includes,
        [  --with-libyaml-includes=DIR  libyaml include directory],
        [with_libyaml_includes="$withval"],[with_libyaml_includes=no])
AC_ARG_WITH(libyaml_libraries,
        [  --with-libyaml-libraries=DIR    libyaml library directory],
        [with_libyaml_libraries="$withval"],[with_libyaml_libraries="no"])

if test "$with_libyaml_includes" != "no"; then
    CPPFLAGS="${CPPFLAGS} -I${with_libyaml_includes}"
fi

AC_CHECK_HEADER(yaml.h,,LIBYAML="no")

if test "$with_libyaml_libraries" != "no"; then
    LDFLAGS="${LDFLAGS} -L${with_libyaml_libraries}"
fi

LIBYAML=""
AC_CHECK_LIB(yaml,yaml_parser_initialize,,LIBYAML="no")

if test "$LIBYAML" = "no"; then
    echo
    echo "   ERROR!  libyaml library not found, go get it"
    echo "   from http://pyyaml.org/wiki/LibYAML "
    echo "   or your distribution:"
    echo
    echo "   Debian/Ubuntu: apt-get install libyaml-dev"
    echo "   Fedora: yum install libyaml-devel"
    echo
    exit 1
fi

if test "$MYSQL" = "yes"; then
        AC_MSG_RESULT([------- MySQL Support is enabled -------])
        AC_CHECK_HEADER([mysql/mysql.h])
        AC_CHECK_LIB(mysqlclient, main,,AC_MSG_ERROR(The MySQL library libmysqlclient is missing!
        If you're not interested in MySQL support use the --disable-mysql flag.))
        fi

if test "$POSTGRES" = "yes"; then
       AC_MSG_RESULT([------- PostgreSQL support is enabled -------])
       AC_CHECK_HEADER([libpq-fe.h])
       AC_CHECK_LIB(pq, main,,AC_MSG_ERROR(The PostgreSQL library cannot be found.
       If you're not interested in PostgreSQL support use the --disable-postgresql flag.))
       fi

if test "$REDIS" = "yes"; then
       AC_MSG_RESULT([------- Redis support is enabled -------])
       AC_CHECK_HEADER([hiredis/hiredis.h])
       AC_CHECK_LIB(hiredis, main,,AC_MSG_ERROR(The Hiredis (Redis) library cannot be found.
       If you're not interested in Redis support use the --disable-redis flag.))
       fi


test "x$prefix" = x. || test "x$prefix" = xNONE && prefix=/usr/local
AC_DEFINE_UNQUOTED(CONFIG_FILE_PATH, "`eval echo "${sysconfdir}/meer.yaml"`", [Meer configuration file]) 

AC_DEFINE_UNQUOTED(PACKAGE_NAME, "meer" )
AC_DEFINE_UNQUOTED(PACKAGE_STRING, "Meer $VERSION")
AC_DEFINE_UNQUOTED(PACKAGE_BUGREPORT, "cclark@quadrantsec.com" )
AC_DEFINE_UNQUOTED(PACKAGE_TARNAME, "meer" )
AC_DEFINE_UNQUOTED(PACKAGE_VERSION, "$VERSION" )

AC_CONFIG_FILES([ \
Makefile \
src/Makefile])

# AC_OUTPUT(Makefile)
AC_OUTPUT

AC_MSG_RESULT([])
AC_MSG_RESULT([@@@@@@@@@@  @@@@@@@@ @@@@@@@@ @@@@@@@])
AC_MSG_RESULT([@@! @@! @@! @@!      @@!      @@!  @@@   Quadrant Information Security])
AC_MSG_RESULT([@!! !!@ @!@ @!!!:!   @!!!:!   @!@!!@a    https://quadrantsec.com])
AC_MSG_RESULT([!!:     !!: !!:      !!:      !!: :!a    Copyright (C) 2018])
AC_MSG_RESULT([ :      :   : :: ::  : :: ::   :   : :   By Champ Clark III])  
AC_MSG_RESULT([])
AC_MSG_RESULT([Meer is configured! Now type 'make'])
AC_MSG_RESULT([])


