add_library(KF5WindowSystemX11Plugin MODULE)

target_sources(KF5WindowSystemX11Plugin PRIVATE
    kwindoweffects.cpp
    kwindowinfo.cpp
    kwindowshadow.cpp
    kwindowsystem.cpp
    kxerrorhandler.cpp
    kxutils.cpp
    plugin.cpp
)
ecm_qt_declare_logging_category(KF5WindowSystemX11Plugin
    HEADER kwindowsystem_xcb_debug.h
    IDENTIFIER LOG_KKEYSERVER_X11
    CATEGORY_NAME kf.windowsystem.keyserver.x11
    DEFAULT_SEVERITY Warning
)

if (TARGET Qt5::X11Extras)
    set(_qt_x11_libs Qt5::X11Extras)
elseif (TARGET Qt6::Gui)
    set(_qt_x11_libs Qt6::GuiPrivate) # qtx11extras_p.h
endif()

target_link_libraries(KF5WindowSystemX11Plugin
    PRIVATE
        KF5WindowSystem
        XCB::XCB
        XCB::RES
        ${X11_LIBRARIES}
        ${X11_Xfixes_LIB}
        ${_qt_x11_libs}
)

ecm_generate_headers(KWindowSystemX11_HEADERS
    HEADER_NAMES
        KSelectionOwner
        KSelectionWatcher
        KXMessages
        NETWM # does not match the classnames in that file...

    REQUIRED_HEADERS
        KWindowSystemX11_HEADERS
)

install(
    FILES
        ${KWindowSystemX11_HEADERS}
        kkeyserver_x11.h
        kmanagerselection.h
        fixx11h.h
    DESTINATION
        ${KDE_INSTALL_INCLUDEDIR_KF}/KWindowSystem
    COMPONENT
        Devel
)

set_target_properties(
    KF5WindowSystemX11Plugin
    PROPERTIES LIBRARY_OUTPUT_DIRECTORY "${CMAKE_LIBRARY_OUTPUT_DIRECTORY}/kf5/kwindowsystem"
)

install(
    TARGETS
        KF5WindowSystemX11Plugin
    DESTINATION
        ${KDE_INSTALL_PLUGINDIR}/kf5/kwindowsystem/
)
