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
    PATHS ${LIBFFI_DIR}/include
)

find_library(FFI_LIB
    NAMES ffi
    PATHS ${LIBFFI_DIR}/lib
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

if(BUILD_GIO)
    add_library(gio SHARED
        gio/gaction.c
        gio/gactiongroup.c
        gio/gactiongroupexporter.c
        gio/gactionmap.c
        gio/gappinfo.c
        gio/gapplication.c
        gio/gapplicationcommandline.c
        gio/gapplicationimpl-dbus.c
        gio/gasynchelper.c
        gio/gasyncinitable.c
        gio/gasyncresult.c
        gio/gbufferedinputstream.c
        gio/gbufferedoutputstream.c
        gio/gbytesicon.c
        gio/gcancellable.c
        gio/gcharsetconverter.c
        gio/gcontenttype-win32.c
        gio/gcontextspecificgroup.c
        gio/gconverter.c
        gio/gconverterinputstream.c
        gio/gconverteroutputstream.c
        gio/gcredentials.c
        gio/gdatainputstream.c
        gio/gdataoutputstream.c
        gio/gdbusactiongroup.c
        gio/gdbusaddress.c
        gio/gdbusauth.c
        gio/gdbusauthmechanismanon.c
        gio/gdbusauthmechanism.c
        gio/gdbusauthmechanismexternal.c
        gio/gdbusauthmechanismsha1.c
        gio/gdbusauthobserver.c
        gio/gdbusconnection.c
        gio/gdbusdaemon.c
        gio/gdbus-daemon-generated.c
        gio/gdbuserror.c
        gio/gdbusinterface.c
        gio/gdbusinterfaceskeleton.c
        gio/gdbusintrospection.c
        gio/gdbusmenumodel.c
        gio/gdbusmessage.c
        gio/gdbusmethodinvocation.c
        gio/gdbusnameowning.c
        gio/gdbusnamewatching.c
        gio/gdbusobject.c
        gio/gdbusobjectmanager.c
        gio/gdbusobjectmanagerclient.c
        gio/gdbusobjectmanagerserver.c
        gio/gdbusobjectproxy.c
        gio/gdbusobjectskeleton.c
        gio/gdbusprivate.c
        gio/gdbusproxy.c
        gio/gdbusserver.c
        gio/gdbusutils.c
        gio/gdelayedsettingsbackend.c
        gio/gdrive.c
        gio/gdummyfile.c
        gio/gdummyproxyresolver.c
        gio/gdummytlsbackend.c
        gio/gemblem.c
        gio/gemblemedicon.c
        gio/gfdonotificationbackend.c
        gio/gfileattribute.c
        gio/gfile.c
        gio/gfiledescriptorbased.c
        gio/gfileenumerator.c
        gio/gfileicon.c
        gio/gfileinfo.c
        gio/gfileinputstream.c
        gio/gfileiostream.c
        gio/gfilemonitor.c
        gio/gfilenamecompleter.c
        gio/gfileoutputstream.c
        gio/gfilterinputstream.c
        gio/gfilteroutputstream.c
        gio/ggtknotificationbackend.c
        gio/ghttpproxy.c
        gio/gicon.c
        gio/ginetaddress.c
        gio/ginetaddressmask.c
        gio/ginetsocketaddress.c
        gio/ginitable.c
        gio/ginputstream.c
        gio/gioenumtypes.c
        gio/gioerror.c
        gio/giomodule.c
        gio/gioscheduler.c
        gio/giostream.c
        gio/gkeyfilesettingsbackend.c
        gio/glistmodel.c
        gio/gliststore.c
        gio/gloadableicon.c
        gio/glocalfile.c
        gio/glocalfileenumerator.c
        gio/glocalfileinfo.c
        gio/glocalfileinputstream.c
        gio/glocalfileiostream.c
        gio/glocalfilemonitor.c
        gio/glocalfileoutputstream.c
        gio/glocalvfs.c
        gio/gmemoryinputstream.c
        gio/gmemoryoutputstream.c
        gio/gmemorysettingsbackend.c
        gio/gmenu.c
        gio/gmenuexporter.c
        gio/gmenumodel.c
        gio/gmount.c
        gio/gmountoperation.c
        gio/gnativevolumemonitor.c
        gio/gnetworkaddress.c
        gio/gnetworking.c
        gio/gnetworkmonitorbase.c
        gio/gnetworkmonitor.c
        gio/gnetworkservice.c
        gio/gnotificationbackend.c
        gio/gnotification.c
        gio/gnullsettingsbackend.c
        gio/goutputstream.c
        gio/gpermission.c
        gio/gpollableinputstream.c
        gio/gpollableoutputstream.c
        gio/gpollableutils.c
        gio/gpollfilemonitor.c
        gio/gpropertyaction.c
        gio/gproxyaddress.c
        gio/gproxyaddressenumerator.c
        gio/gproxy.c
        gio/gproxyresolver.c
        gio/gregistrysettingsbackend.c
        gio/gremoteactiongroup.c
        gio/gresolver.c
        gio/gresource.c
        gio/gresourcefile.c
        gio/gseekable.c
        gio/gsettingsbackend.c
        gio/gsettings.c
        gio/gsettings-mapping.c
        gio/gsettingsschema.c
        gio/gsimpleaction.c
        gio/gsimpleactiongroup.c
        gio/gsimpleasyncresult.c
        gio/gsimpleiostream.c
        gio/gsimplepermission.c
        gio/gsimpleproxyresolver.c
        gio/gsocketaddress.c
        gio/gsocketaddressenumerator.c
        gio/gsocket.c
        gio/gsocketclient.c
        gio/gsocketconnectable.c
        gio/gsocketconnection.c
        gio/gsocketcontrolmessage.c
        gio/gsocketinputstream.c
        gio/gsocketlistener.c
        gio/gsocketoutputstream.c
        gio/gsocketservice.c
        gio/gsocks4aproxy.c
        gio/gsocks4proxy.c
        gio/gsocks5proxy.c
        gio/gsrvtarget.c
        gio/gsubprocess.c
        gio/gsubprocesslauncher.c
        gio/gtask.c
        gio/gtcpconnection.c
        gio/gtcpwrapperconnection.c
        gio/gtestdbus.c
        gio/gthemedicon.c
        gio/gthreadedresolver.c
        gio/gthreadedsocketservice.c
        gio/gtlsbackend.c
        gio/gtlscertificate.c
        gio/gtlsclientconnection.c
        gio/gtlsconnection.c
        gio/gtlsdatabase.c
        gio/gtlsfiledatabase.c
        gio/gtlsinteraction.c
        gio/gtlspassword.c
        gio/gtlsserverconnection.c
        gio/gunionvolumemonitor.c
        gio/gvdb/gvdb-reader.c
        gio/gvfs.c
        gio/gvolume.c
        gio/gvolumemonitor.c
        gio/gwin32appinfo.c
        gio/gwin32inputstream.c
        gio/gwin32mount.c
        gio/gwin32outputstream.c
        gio/gwin32volumemonitor.c
        gio/gzlibcompressor.c
        gio/gzlibdecompressor.c
        gio/thumbnail-verify.c
        gio/win32/gwin32filemonitor.c
        gio/win32/gwin32fsmonitorutils.c
        gio/win32/gwinhttpfile.c
        gio/win32/gwinhttpfileinputstream.c
        gio/win32/gwinhttpfileoutputstream.c
        gio/win32/gwinhttpvfs.c
    )

    target_link_libraries(gio PUBLIC
        glib
    )

    target_compile_definitions(gio PRIVATE
        -DGIO_COMPILATION
    )

    target_include_directories(gio PRIVATE
        gio
        gmodule
    )

    configure_file(gio/gnetworking.h.win32 ${CMAKE_BINARY_DIR}/gio/gnetworking.h)
    configure_file(gio/gnetworking.h.win32 ${CMAKE_BINARY_DIR}/gnetworking.h)
endif(BUILD_GIO)

install(TARGETS glib gobject gmodule
    RUNTIME DESTINATION bin
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
)
install(FILES glib/glib.h ${CMAKE_BINARY_DIR}/glibconfig.h
    DESTINATION include
)
