pkg_check_modules(RESOURCEQT "libresourceqt5" REQUIRED)
pkg_check_modules(NEMODEVICELOCK "nemodevicelock" REQUIRED)

include_directories(${RESOURCEQT_INCLUDE_DIRS} ${NEMODEVICELOCK_INCLUDE_DIRS} ${QT5BOOSTABLE_INCLUDE_DIRS} ${LIBVOICECALL_DIR})

add_definitions(-DVOICECALL_PLUGIN_DIRECTORY="/usr/lib/voicecall/plugins")


add_executable(voicecall-manager
    main.cpp
    audiocallpolicyproxy.cpp
    basicvoicecallconfigurator.cpp
    voicecallmanager.cpp
    dbus/voicecallmanagerdbusservice.cpp
)
add_dependencies(voicecall-manager voicecall)

target_link_libraries(voicecall-manager PUBLIC
    Qt5::Core
    Qt5::DBus
    Qt5::Multimedia
    ${RESOURCEQT_LIBRARIES}
    ${NEMODEVICELOCK_LIBRARIES}
    voicecall
)

set_property(TARGET voicecall-manager PROPERTY POSITION_INDEPENDENT_CODE TRUE)


install(TARGETS voicecall-manager RUNTIME
    DESTINATION ${CMAKE_INSTALL_BINDIR})

install(FILES voicecall-manager.service
    DESTINATION ${CMAKE_INSTALL_PREFIX}/lib/systemd/user/)
