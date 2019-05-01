AC_INIT([Janus CloudRoom Plugin],[0.1.0],[https://github.com/jamken/janus-mixroom],[janus-mixroom],[https://www.qulubo.net])
AC_LANG(C)

AC_CONFIG_AUX_DIR([autoconf])
AC_CONFIG_MACRO_DIR([m4])

AC_ENABLE_SHARED(yes)
AC_ENABLE_STATIC(no)

AM_INIT_AUTOMAKE([foreign subdir-objects])
AM_SILENT_RULES([no])

#AC_GNU_SOURCE has been deprecated (https://www.gnu.org/software/autoconf/manual/autoconf-2.69/html_node/Obsolete-Macros.html)
AC_USE_SYSTEM_EXTENSIONS

AC_PROG_CC

LT_PREREQ([2.2])
LT_INIT

# Common CFLAGS
CFLAGS="$CFLAGS \
	-fPIC \
	-fstack-protector-all \
	-fstrict-aliasing \
	-ggdb3 \
	-pthread \
	-Wall \
	-Warray-bounds \
	-Wextra \
	-Wformat-nonliteral \
	-Wformat-security \
	-Wformat=2 \
	-Winit-self \
	-Winline \
	-Wlarger-than=65537 \
	-Wmissing-declarations \
	-Wmissing-format-attribute \
	-Wmissing-include-dirs \
	-Wmissing-noreturn \
	-Wmissing-prototypes \
	-Wnested-externs \
	-Wno-aggregate-return \
	-Wno-missing-field-initializers \
	-Wno-redundant-decls \
	-Wno-undef \
	-Wno-unused-parameter \
	-Wold-style-definition \
	-Wpacked \
	-Wpointer-arith \
	-Wsign-compare \
	-Wstrict-prototypes \
	-Wswitch-default \
	-Wunused \
	-Wwrite-strings"

AM_CONDITIONAL([USE_CLANG], [test "x$CC" = "xclang"])
AM_COND_IF([USE_CLANG],
	# Specific clang flags
	[CFLAGS="$CFLAGS \
		-Wno-cast-align \
		-Wno-initializer-overrides"
	],
	# Specific gcc flags
	[CFLAGS="$CFLAGS \
		-Wcast-align \
		-Wno-override-init \
		-Wunsafe-loop-optimizations \
		-Wunused-but-set-variable"	
	]
)

case "$host_os" in
darwin*)
	CFLAGS="$CFLAGS -I/usr/local/opt/openssl/include -I/usr/local/include"
	# add rdynamic to LDFLAGS
	LDFLAGS="$LDFLAGS -Wl,-export_dynamic"
	LDFLAGS="$LDFLAGS -L/usr/local/lib -L/usr/local/opt/openssl/lib -L/opt/local/lib -L/usr/local/libsrtp/lib"
	AM_CONDITIONAL([DARWIN_OS], true)
	AM_CONDITIONAL([HAS_DTLS_WINDOW_SIZE], false)
;;
*)
	LDFLAGS="$LDFLAGS -Wl,--export-dynamic"
	AM_CONDITIONAL([DARWIN_OS], false)
	AM_CONDITIONAL([HAS_DTLS_WINDOW_SIZE], true)
esac

glib_version=2.34
ssl_version=1.0.1
jansson_version=2.5



PKG_CHECK_MODULES([JANUS],
                  [
                    glib-2.0 >= $glib_version
					libconfig
                    nice
                    jansson >= $jansson_version
                    libssl >= $ssl_version
                    libcrypto
                  ])
JANUS_MANUAL_LIBS+=" -lm"
AC_SUBST(JANUS_MANUAL_LIBS)



AC_SEARCH_LIBS([tls_config_set_ca_mem],[tls],
             [AM_CONDITIONAL([LIBRESSL_DETECTED], true)],
             [AM_CONDITIONAL([LIBRESSL_DETECTED], false)]
             )


AC_CHECK_LIB([dl],
             [dlopen],
             [JANUS_MANUAL_LIBS+=" -ldl"],
             [AC_MSG_ERROR([libdl not found.])])





##
# Plugins
##

PKG_CHECK_MODULES([PLUGINS],
                  [
                    glib-2.0 >= $glib_version
                    jansson >= $jansson_version
                  ])




AC_ARG_ENABLE([plugin-cloudroom],
              [AS_HELP_STRING([--disable-plugin-cloudroom],
                              [Disable cloudroom plugin])],
              [AS_IF([test "x$enable_plugin_cloudroom" != "xyes"],
                     [enable_plugin_cloudroom=no])],
              [enable_plugin_cloudroom=yes])

AM_CONDITIONAL([ENABLE_PLUGIN_CLOUDROOM], [test "x$enable_plugin_cloudroom" = "xyes"])


JANUS_MANUAL_LIBS+=" -pthread"




AC_CONFIG_FILES([
  Makefile
  html/Makefile
])

AC_OUTPUT

##
# Summary
##
echo
echo "Compiler:                  $CC"
echo "Plugins:"
AM_COND_IF([ENABLE_PLUGIN_CLOUDROOM],
	[echo "    Cloud Room:            yes"],
	[echo "    Cloud Room:            no"])
echo 


AC_MSG_NOTICE([Configure path:  
 
janus-cloudroom version .................... : $PACKAGE_VERSION
janus-cloudroom Install prefix ............. : $prefix
janus-cloudroom exec prefix ................ : $exec_prefix
janus-cloudroom library path ............... : $libdir
janus-cloudroom include path ............... : $includedir
janus-cloudroom program path ............... : $bindir
janus-cloudroom data path .................. : $datarootdir
janus-cloudroom conf path .................. : $sysconfdir
])    
    
echo
echo "If this configuration is ok for you, do a 'make' to start building Janus-cloudroom. A 'make install' will install janus-cloudroom plugin to the specified prefix(must be the same with Janus）. Finally, a 'make configs' will install some sample configuration files too (something you'll only want to do the first time, though)."
echo
