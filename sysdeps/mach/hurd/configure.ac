GLIBC_PROVIDES dnl See aclocal.m4 in the top level source directory.

dnl We need this setting because of the need for PLT calls in ld.so.
AC_DEFINE([NO_HIDDEN])

# Don't bother trying to generate any glue code to be compatible with the
# existing system library, because we are the only system library.
inhibit_glue=yes

case "$machine" in
  i386*)
    # The default oldest ABI is 2.2.6.
    # We only need a "yes" here if the oldest ABI supported will be < 2.2.6.
    if test "$oldest_abi" != default && test "$oldest_abi" \< "2.2.6"; then
      libc_cv_gcc_unwind_find_fde=yes
    fi
    ;;
esac

if test -n "$sysheaders"; then
  OLD_CPPFLAGS=$CPPFLAGS
  CPPFLAGS="$CPPFLAGS $SYSINCLUDES"
fi

AC_CACHE_CHECK(Hurd header version, libc_cv_hurd_version, [dnl
AC_TRY_COMPILE(dnl
[#include <hurd/version.h>], [
#define NEED_VERSION 20020609
#if HURD_INTERFACE_VERSION < NEED_VERSION
# error Hurd version too old: HURD_INTERFACE_VERSION < NEED_VERSION
#endif],
	       libc_cv_hurd_version=ok,
	       libc_cv_hurd_version=bad)])
if test "x$libc_cv_hurd_version" != xok; then
  AC_MSG_ERROR(Hurd headers not installed or too old)
fi

if test -n "$sysheaders"; then
  CPPFLAGS=$OLD_CPPFLAGS
fi
