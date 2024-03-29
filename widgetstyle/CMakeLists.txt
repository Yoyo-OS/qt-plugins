cmake_minimum_required(VERSION 3.5)
project(yoyostyle)

set(TARGET yoyostyle)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_AUTOMOC ON)
set(CMAKE_INCLUDE_CURRENT_DIR ON)

find_package(ECM REQUIRED NO_MODULE)
set(CMAKE_MODULE_PATH ${ECM_MODULE_PATH} ${ECM_KDE_MODULE_DIR} ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

set(QT Core Gui Widgets DBus X11Extras)
find_package(Qt5 REQUIRED ${QT})
find_package(PkgConfig REQUIRED)
find_package(KF5 REQUIRED WindowSystem)

if (${KF5_VERSION} VERSION_LESS 5.93.0)
    include(ECMQueryQmake)
else()
    include(ECMQueryQt)
endif()

set (SRCS
    blurhelper.cpp
    blurhelper.h
    pstyleplugin.cpp
    pstyleplugin.h
    basestyle.h
    basestyle.cpp
    phantomcolor.h
    phantomcolor.cpp
    shadowhelper.h
    shadowhelper.cpp
    tileset.h
    tileset.cpp
    boxshadowrenderer.h
    boxshadowrenderer.cpp
)

add_library(${TARGET} MODULE ${SRCS})
target_link_libraries(${TARGET}
    Qt5::GuiPrivate
    Qt5::Core
    Qt5::Gui
    Qt5::Widgets
    Qt5::DBus
    Qt5::X11Extras
    KF5::WindowSystem
    )

if (${KF5_VERSION} VERSION_LESS 5.93.0)
    query_qmake(CMAKE_INSTALL_QTPLUGINDIR QT_INSTALL_PLUGINS)
else()
    ecm_query_qt(CMAKE_INSTALL_QTPLUGINDIR QT_INSTALL_PLUGINS)
endif()

install(TARGETS ${TARGET} DESTINATION ${CMAKE_INSTALL_QTPLUGINDIR}/styles/)
