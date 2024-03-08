AC_PREREQ([2.69])

AC_INIT(hpl, 2.3, hpl@icl.utk.edu)
AC_CONFIG_SRCDIR([include/hpl.h])
AC_CONFIG_HEADERS([include/hplconfig.h])

AX_PROG_CC_MPI

AC_PROG_RANLIB

AC_PROG_INSTALL

AM_INIT_AUTOMAKE([subdir-objects])

AM_PROG_CC_C_O

dnl
dnl AX_BLAS requires Fortran compiler and detects fortran libraries in $FLIBS
dnl
dnl AX_BLAS(LIBS="$BLAS_LIBS $LIBS $FLIBS")
dnl

HPL_BLAS(LIBS="$BLAS_LIBS $LIBS",AC_MSG_ERROR([BLAS not found]))

dnl FIXME: test for CBLAS: Atlas, MKL, OpenBLAS, ESSL, ...
dnl FIXME: test for GSL CBLAS

AC_CHECK_FUNCS([dgemm_])

AC_CHECK_HEADERS([mpi.h])

AC_MSG_CHECKING(--with-adiak)
AC_ARG_WITH(adiak,
    [AS_HELP_STRING([--with-adiak=<path>], [Enable metadata collection with adiak])],
    [with_adiak=$withval],[with_adiak=no])
AC_MSG_RESULT($with_adiak)

if test "x$with_adiak" != xno; then
    adiak_root=$with_adiak
    CPPFLAGS+=" -I$adiak_root/include"
    LIBS+=" -L$adiak_root/lib -ladiak"
    AC_LINK_IFELSE([AC_LANG_SOURCE([#include <adiak.h>
        int main() { adiak_init(NULL); return 0; }])],
        AC_MSG_RESULT([Found Adiak]),
        AC_MSG_FAILURE([Could not link with adiak]))
    AC_DEFINE(HAVE_ADIAK)
fi

AC_MSG_CHECKING(--with-caliper)
AC_ARG_WITH(caliper,
    [AS_HELP_STRING([--with-caliper=<path>], [Enable profiling with caliper])],
    [with_caliper=$withval],[with_caliper=no])
AC_MSG_RESULT($with_caliper)

if test "x$with_caliper" != xno; then
    caliper_root=$with_caliper
    CPPFLAGS+=" -I$caliper_root/include"
    LIBS+=" -L$caliper_root/lib64 -lcaliper"
    AC_LINK_IFELSE([AC_LANG_SOURCE([#include <caliper/cali.h>
        int main() { cali_begin_region("main"); cali_end_region("main"); return 0; }])],
        AC_MSG_RESULT([Found Caliper]),
        AC_MSG_FAILURE([Could not link with caliper]))
    AC_DEFINE(HAVE_CALIPER)
fi

AC_CONFIG_FILES([Makefile src/Makefile testing/Makefile])

AC_OUTPUT
