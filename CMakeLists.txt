cmake_minimum_required(VERSION 3.1)
project(GLib2)

add_library(glib SHARED
    glib/deprecated/gallocator.c
    glib/deprecated/gcache.c
    glib/deprecated/gcompletion.c
    glib/deprecated/grel.c
    glib/deprecated/gthread-deprecated.c
    glib/garray.c
    glib/gasyncqueue.c
    glib/gatomic.c
    glib/gbacktrace.c
    glib/gbase64.c
    glib/gbitlock.c
    glib/gbookmarkfile.c
    glib/gbytes.c
    glib/gcharset.c
    glib/gchecksum.c
    glib/gconvert.c
    glib/gdataset.c
    glib/gdate.c
    glib/gdatetime.c
    glib/gdir.c
    glib/genviron.c
    glib/gerror.c
    glib/gfileutils.c
#    glib/ggettext.c
    glib/ghash.c
    glib/ghmac.c
    glib/ghook.c
    glib/ghostutils.c
    glib/giochannel.c
    glib/gkeyfile.c
    glib/glib-init.c
    glib/glib-private.c
    glib/glist.c
    glib/gmain.c
    glib/gmappedfile.c
    glib/gmarkup.c
    glib/gmem.c
    glib/gmessages.c
    glib/gnode.c
    glib/goption.c
    glib/gpattern.c
    glib/gpoll.c
    glib/gprimes.c
    glib/gqsort.c
    glib/gquark.c
    glib/gqueue.c
    glib/grand.c
    glib/gregex.c
    glib/gscanner.c
    glib/gsequence.c
    glib/gshell.c
    glib/gslice.c
    glib/gslist.c
    glib/gstdio.c
    glib/gstrfuncs.c
    glib/gstring.c
    glib/gstringchunk.c
    glib/gtestutils.c
    glib/gthread.c
    glib/gthreadpool.c
    glib/gtimer.c
    glib/gtimezone.c
    glib/gtranslit.c
    glib/gtrashstack.c
    glib/gtree.c
    glib/guniprop.c
    glib/gutf8.c
    glib/gunibreak.c
    glib/gunicollate.c
    glib/gunidecomp.c
    glib/gurifuncs.c
    glib/gutils.c
    glib/gvariant.c
    glib/gvariant-core.c
    glib/gvariant-parser.c
    glib/gvariant-serialiser.c
    glib/gvarianttypeinfo.c
    glib/gvarianttype.c
    glib/gversion.c
    glib/gwakeup.c
    glib/gprintf.c
    glib/giowin32.c
    glib/gspawn-win32.c
    glib/gthread-win32.c
    glib/gwin32.c
    glib/libcharset/localcharset.c

    build/win32/dirent/dirent.c

    glib/gnulib/asnprintf.c
    glib/gnulib/printf-args.c
    glib/gnulib/printf-parse.c
    glib/gnulib/printf.c
    glib/gnulib/vasnprintf.c
    glib/pcre/pcre_byte_order.c
    glib/pcre/pcre_chartables.c
    glib/pcre/pcre_compile.c
    glib/pcre/pcre_config.c
    glib/pcre/pcre_dfa_exec.c
    glib/pcre/pcre_exec.c
    glib/pcre/pcre_fullinfo.c
    glib/pcre/pcre_get.c
    glib/pcre/pcre_globals.c
    glib/pcre/pcre_jit_compile.c
    glib/pcre/pcre_newline.c
    glib/pcre/pcre_ord2utf8.c
    glib/pcre/pcre_string_utils.c
    glib/pcre/pcre_study.c
    glib/pcre/pcre_tables.c
    glib/pcre/pcre_valid_utf8.c
    glib/pcre/pcre_xclass.c
)

find_path(LIBFFI_INCLUDE_DIR
    NAMES ffi.h
    PATHS ${LIBFFI_DIR}
)

find_library(FFI_LIB
    NAMES ffi
    PATHS ${LIBFFI_DIR}
)
message(${FFI_LIB})

target_include_directories(glib PUBLIC
    .
    ${CMAKE_BINARY_DIR}
    glib
    build/win32/dirent
    ${LIBFFI_INCLUDE_DIR}
)

target_compile_definitions(glib
PRIVATE
    -DHAVE_CONFIG_H
    -DGLIB_COMPILATION
    -DDLL_EXPORT
    -DLIBDIR
    -DLINK_SIZE=2
    -DNEWLINE=-1
    -DMAX_NAME_SIZE=32
    -DMAX_NAME_COUNT=10000
    -DNEWLINE=-1
    -DPOSIX_MALLOC_THRESHOLD=10
    -DMATCH_LIMIT=10000000
    -DMATCH_LIMIT_RECURSION=10000000
    -DSUPPORT_UCP
    -DSUPPORT_UTF
    -DSUPPORT_UTF8
)

target_compile_options(glib PUBLIC
    -FI msvc_recommended_pragmas.h
)

target_link_libraries(glib PUBLIC
    ws2_32
    winmm
 )

add_library(gmodule SHARED
    gmodule/gmodule.c
)

target_link_libraries(gmodule PUBLIC
    glib
)

add_library(gobject SHARED
    gobject/gatomicarray.c
    gobject/gbinding.c
    gobject/gboxed.c
    gobject/gclosure.c
    gobject/genums.c
    gobject/gmarshal.c
    gobject/gobject.c
    gobject/gparam.c
    gobject/gparamspecs.c
    gobject/gsignal.c
    gobject/gsourceclosure.c
    gobject/gtype.c
    gobject/gtypemodule.c
    gobject/gtypeplugin.c
    gobject/gvalue.c
    gobject/gvaluearray.c
    gobject/gvaluetransform.c
    gobject/gvaluetypes.c
)

target_link_libraries(gobject PUBLIC
    glib
    ${FFI_LIB}
)

target_compile_definitions(gobject PRIVATE
    -DGOBJECT_COMPILATION
    -DFFI_BUILDING
)

set(GLIB_MAJOR_VERSION 2)
set(GLIB_MINOR_VERSION 45)
set(GLIB_MICRO_VERSION 3)
set(GLIB_INTERFACE_AGE 0)
set(GLIB_BINARY_AGE 0)
configure_file(config.h.win32.in config.h @ONLY)
configure_file(glib/glibconfig.h.win32.in glibconfig.h)
configure_file(gmodule/gmoduleconf.h.win32 gmoduleconf.h)

add_executable(glib-genmarshal
    gobject/glib-genmarshal.c
)

target_link_libraries(glib-genmarshal PUBLIC
    glib
    gobject
)

file(STRINGS gobject/gmarshal.list Contents)
unset(ModifiedContents)
foreach(Line ${Contents})
  if("${Line}" MATCHES "^[A-Z]")
    message(${Line})
    string(REPLACE
        ":"
        "__"
        Line ${Line})
    string(REPLACE
        ","
        "_"
        Line ${Line})
    message(${Line})
    set(ModifiedContents "${ModifiedContents}\"g_cclosure_marshal_${Line}\",\n")
  endif()
endforeach()
file(WRITE ${CMAKE_BINARY_DIR}/gmarshal.strings ${ModifiedContents})

# gmarshal.strings : gmarshal.list
#       perl marshal-genstrings.pl > gmarshal.strings
# gmarshal.h : gmarshal.list glib-genmarshal.exe
#	echo #ifndef __G_MARSHAL_H__ > xgen-gmh
#	echo #define __G_MARSHAL_H__ >> xgen-gmh
#	glib-genmarshal --nostdinc --prefix=g_cclosure_marshal gmarshal.list --header >> xgen-gmh
#	echo #endif /* __G_MARSHAL_H__ */ >> xgen-gmh
#	copy xgen-gmh gmarshal.h
# gmarshal.c: gmarshal.list gmarshal.h glib-genmarshal.exe
#	glib-genmarshal --nostdinc --prefix=g_cclosure_marshal gmarshal.list --body > gmarshal.c
