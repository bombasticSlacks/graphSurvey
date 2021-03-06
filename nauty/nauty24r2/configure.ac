dnl Process this file with autoconf to produce a configure script.
AC_INIT(dreadnaut.c)

dnl AC_TRY_CFLAGS (CFLAGS, [ACTION-IF-WORKS], [ACTION-IF-FAILS])
dnl Check if $CC supports a given set of cflags
AC_DEFUN([AC_TRY_CFLAGS],
    [AC_MSG_CHECKING([if $CC supports $1 flags])
    SAVE_CFLAGS="$CFLAGS"
    CFLAGS="$1"
    AC_TRY_RUN([main(){exit(0);}],[ac_cv_try_cflags_ok=yes],
	[ac_cv_try_cflags_ok=no],[ac_cv_try_cflags_ok=no])
    CFLAGS="$SAVE_CFLAGS"
    AC_MSG_RESULT([$ac_cv_try_cflags_ok])
    if test x"$ac_cv_try_cflags_ok" = x"yes"; then
        ifelse([$2],[],[:],[$2])
    else
        ifelse([$3],[],[:],[$3])
    fi])

dnl CVT_YESNO([YES-NO VARIABLE],[0-1 VARIABLE])
dnl Make a 0-1 output variable from a yes/no shell variable
AC_DEFUN(CVT_YESNO,[if test x"$$1" = x"yes"; then
$2=1
else
$2=0
fi
AC_SUBST($2)])

dnl Checks for system features
AC_CANONICAL_HOST

dnl Checks for C compiler and sets CFLAGS if not set by user
user_cflags="$CFLAGS"
AC_PROG_CC
CFLAGS=$user_cflags
MORECFLAGS=""

if test x"$user_cflags" = x"" ; then
  AC_TRY_CFLAGS([-O4],[CFLAGS="$CFLAGS -O4"]);
  if test x"$ac_cv_try_cflags_ok" = x"no"; then
     AC_TRY_CFLAGS([-O3],[CFLAGS="$CFLAGS -O3"]);
     if test x"$ac_cv_try_cflags_ok" = x"no"; then
         AC_TRY_CFLAGS([-O2],[CFLAGS="$CFLAGS -O2"],[CFLAGS="$CFLAGS -O"]);
     fi
  fi
 
  if test "$CC" = "gcc" ; then
      gccver=`gcc --version 2>/dev/null || echo 0.0.0`
  else
      gccver=0.0.0
  fi

  case "$host" in
   *pentium*solaris*)
	;;
   *pentium4*)
	AC_TRY_CFLAGS([-march=pentium4],[CFLAGS="$CFLAGS -march=pentium4"]);;
   *pentium3*|*i686*|*athlon*|x86*)
	AC_TRY_CFLAGS([-march=i686],[CFLAGS="$CFLAGS -march=i686"]);;
   *pentium2*|*i586*)
	AC_TRY_CFLAGS([-march=i586],[CFLAGS="$CFLAGS -march=i586"]);;
   *powerpc*)
        machtype=`/usr/bin/machine 2>/dev/null || echo unknown`
        case "$machtype" in
	  ppc740?)
	     AC_TRY_CFLAGS([-mcpu=7400],[CFLAGS="$CFLAGS -mcpu=7400"])
	     AC_TRY_CFLAGS([-mtune=7400],[CFLAGS="$CFLAGS -mtune=7400"]);;
	  ppc745?)
	     AC_TRY_CFLAGS([-mcpu=7450],[CFLAGS="$CFLAGS -mcpu=7450"])
	     AC_TRY_CFLAGS([-mtune=7450],[CFLAGS="$CFLAGS -mtune=7450"])
             case "$gccver" in
               *\ 4.[[0-9]].[[0-9]]\ *)
	           AC_TRY_CFLAGS([$CFLAGS -fast],
			[MORECFLAGS="$MORECFLAGS -fast"]);;
             esac;;
	  ppc970)
	     AC_TRY_CFLAGS([-mcpu=g5],[CFLAGS="$CFLAGS -mcpu=g5"])
	     AC_TRY_CFLAGS([-mtune=g5],[CFLAGS="$CFLAGS -mtune=g5"])
	     AC_TRY_CFLAGS([-fast],[CFLAGS="$MORECFLAGS -fast"]);;
          *)
	     AC_TRY_CFLAGS([-mpowerpc],[CFLAGS="$CFLAGS -mpowerpc"]);;
        esac;;
   *osf*)
	AC_TRY_CFLAGS([-fast],[CFLAGS="$CFLAGS -fast"]);;
   *sparcv8*|*sparcv9*)
	AC_TRY_CFLAGS([-msupersparc],[CFLAGS="$CFLAGS -msupersparc"])
	if test x"$ac_cv_try_cflags_ok" = x"no"; then
	    AC_TRY_CFLAGS([-cg92],[CFLAGS="$CFLAGS -cg92"])
	    if test x"$ac_cv_try_cflags_ok" = x"no"; then
		AC_TRY_CFLAGS([-xcg92],[CFLAGS="$CFLAGS -xcg92"])
	    fi
	fi;;
  esac
fi

echo CFLAGS=$CFLAGS
echo MORECFLAGS=$MORECFLAGS

dnl Checks relevant to Cygwin and other Windows things
AC_EXEEXT

dnl Checks for header files.
dnl AC_HEADER_STDC
AC_HEADER_SYS_WAIT
AC_CHECK_HEADERS(stddef.h unistd.h sys/types.h sys/wait.h stdlib.h string.h errno.h)
CVT_YESNO(ac_cv_header_stddef_h,header_stddef_h)
CVT_YESNO(ac_cv_header_unistd_h,header_unistd_h)
CVT_YESNO(ac_cv_header_stdlib_h,header_stdlib_h)
CVT_YESNO(ac_cv_header_string_h,header_string_h)
CVT_YESNO(ac_cv_header_signal_h,header_signal_h)
CVT_YESNO(ac_cv_header_sys_wait_h,header_sys_wait_h)
CVT_YESNO(ac_cv_header_sys_types_h,header_sys_types_h)
CVT_YESNO(ac_cv_header_errno_h,header_errno_h)

AC_TYPE_PID_T
CVT_YESNO(ac_cv_type_pid_t,have_pid_t)

AC_MSG_CHECKING(if malloc is declared in stdlib.h or malloc.h)
AC_EGREP_HEADER([(^|[^a-zA-Z_0-9])malloc[^a-zA-Z_0-9]],stdlib.h,
  malloc_dec=1,malloc_dec=0)
if test $malloc_dec -eq 0; then
  AC_EGREP_HEADER([(^|[^a-zA-Z_0-9])malloc[^a-zA-Z_0-9]],malloc.h,
  malloc_dec=2,malloc_dec=0)
fi
AC_SUBST(malloc_dec)
AC_MSG_RESULT($malloc_dec)

AC_MSG_CHECKING(if ftell is declared in stdio.h)
AC_EGREP_HEADER([(^|[^a-zA-Z_0-9])ftell[^a-zA-Z_0-9]],stdio.h,
  ftell_dec=1,ftell_dec=0)
AC_SUBST(ftell_dec)
AC_MSG_RESULT($ftell_dec)

AC_MSG_CHECKING(if fdopen is declared in stdio.h)
AC_EGREP_HEADER([(^|[^a-zA-Z_0-9])fdopen[^a-zA-Z_0-9]],stdio.h,
  fdopen_dec=1,fdopen_dec=0)
AC_SUBST(fdopen_dec)
AC_MSG_RESULT($fdopen_dec)

AC_MSG_CHECKING(if popen is declared in stdio.h)
AC_EGREP_HEADER([(^|[^a-zA-Z_0-9])popen[^a-zA-Z_0-9]],stdio.h,
  popen_dec=1,popen_dec=0)
AC_SUBST(popen_dec)
AC_MSG_RESULT($popen_dec)

AC_MSG_CHECKING(if putenv is declared in stdlib.h)
AC_EGREP_HEADER([(^|[^a-zA-Z_0-9])putenv[^a-zA-Z_0-9]],stdlib.h,
  putenv_dec=1,putenv_dec=0)
AC_SUBST(putenv_dec)
AC_MSG_RESULT($putenv_dec)

AC_MSG_CHECKING(if setenv is declared in stdlib.h)
AC_EGREP_HEADER([(^|[^a-zA-Z_0-9])setenv[^a-zA-Z_0-9]],stdlib.h,
  setenv_dec=1,setenv_dec=0)
AC_SUBST(setenv_dec)
AC_MSG_RESULT($setenv_dec)

AC_MSG_CHECKING(if INFINITY is declared in math.h)
AC_EGREP_CPP(yes,
[#include <stdio.h>
#include <math.h>
#ifdef INFINITY
  yes
#endif
], has_math_inf=1, has_math_inf=0)
AC_SUBST(has_math_inf)
AC_MSG_RESULT($has_math_inf)

dnl Checks for sizes of integer types; avoid 64-bit if necessary
AC_CHECK_SIZEOF(int,4)
AC_SUBST(ac_cv_sizeof_int)
AC_CHECK_SIZEOF(long,4)
AC_SUBST(ac_cv_sizeof_long)
AC_CHECK_SIZEOF(long long,0)
AC_SUBST(ac_cv_sizeof_long_long)
lok=0
testprogs="dreadtest dreadtestS dreadtestS1 dreadtest4K"
testprogs="$testprogs dreadtest1 dreadtestW1"
AC_SUBST(MORECFLAGS)

if test $ac_cv_sizeof_long_long -eq 8; then
   lok=1
   testprogs="$testprogs dreadtestL1 dreadtestL"
else
   if test $ac_cv_sizeof_long -eq 8; then
      lok=1
   fi
fi
AC_SUBST(lok)
AC_SUBST(testprogs)

dnl Checks for typedefs, structures, and compiler characteristics.
AC_C_CONST
CVT_YESNO(ac_cv_c_const,have_const)

dnl Checks for library functions.
AC_CHECK_FUNC(isatty,have_isatty=1,have_isatty=0)
AC_SUBST(have_isatty)
AC_CHECK_FUNC(times,have_times=1,have_times=0)
AC_SUBST(have_times)
AC_CHECK_FUNC(time,have_time=1,have_time=0)
AC_SUBST(have_time)
AC_CHECK_FUNC(gettimeofday,have_gettimeofday=1,have_gettimeofday=0)
AC_SUBST(have_gettimeofday)
AC_CHECK_FUNC(times,have_times=1,have_times=0)
AC_SUBST(have_times)
AC_CHECK_FUNC(getrusage,have_getrusage=1,have_getrusage=0)
AC_SUBST(have_getrusage)
AC_CHECK_FUNC(perror,have_perror=1,have_perror=0)
AC_SUBST(have_perror)
AC_CHECK_FUNC(pipe,have_pipe=1,have_pipe=0)
AC_SUBST(have_pipe)
AC_CHECK_FUNC(wait,have_wait=1,have_wait=0)
AC_SUBST(have_wait)
AC_CHECK_FUNC(popen,have_popen=1,have_popen=0)
AC_SUBST(have_popen)
AC_CHECK_FUNC(putenv,have_putenv=1,have_putenv=0)
AC_SUBST(have_putenv)
AC_CHECK_FUNC(setenv,have_setenv=1,have_setenv=0)
AC_SUBST(have_setenv)

AC_CHECK_FUNC(getc_unlocked,have_getc_unlocked=1,have_getc_unlocked=0)
AC_CHECK_FUNC(flockfile,have_flockfile=1,have_flockfile=0)
if test $have_getc_unlocked -a $have_flockfile; then
    stdio_nolock=1
else
    stdio_nolock=0
fi
AC_SUBST(stdio_nolock)

AC_CHECK_PROGS(sort_prog,gsort sort,no_sort_found)
AC_MSG_CHECKING(if sort supports the -k switch)
if { $sort_prog -k 2,3 </dev/null >/dev/null 2>&1 ; } ; then
   sort_newparams_ok=1
else
   sort_newparams_ok=0
fi
AC_SUBST(sort_newparams_ok)
AC_MSG_RESULT($sort_newparams_ok)

AC_OUTPUT(makefile:makefile.in nauty.h:nauty-h.in
    naututil.h:naututil-h.in gtools.h:gtools-h.in)
