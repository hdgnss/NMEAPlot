cmake_minimum_required(VERSION 3.5)

project(NMEAPlot VERSION 0.0.1 LANGUAGES CXX)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(QT NAMES Qt6 Qt5 REQUIRED COMPONENTS Widgets LinguistTools Charts Sql)
find_package(Qt${QT_VERSION_MAJOR} REQUIRED COMPONENTS Widgets LinguistTools Charts Sql)

set(TS_FILES NMEAPlot_zh_CN.ts)

if (WIN32)
    set(APP_ICONS "${CMAKE_CURRENT_SOURCE_DIR}/resources/icon.rc")
elseif (APPLE)
    set(MACOSX_BUNDLE_ICON_FILE icon.icns)

    set(APP_ICONS "${CMAKE_CURRENT_SOURCE_DIR}/resources/icon.icns")
    set_source_files_properties(${APP_ICONS} PROPERTIES
           MACOSX_PACKAGE_LOCATION "Resources")
endif()

set(PROJECT_SOURCES
        main.cpp
        mainwindow.cpp
        mainwindow.h
        mainwindow.ui
        ${TS_FILES}
)

if(${QT_VERSION_MAJOR} GREATER_EQUAL 6)
    qt_add_executable(NMEAPlot
        MANUAL_FINALIZATION
        ${PROJECT_SOURCES}
        ${APP_ICONS}
    )

    qt_create_translation(QM_FILES ${CMAKE_SOURCE_DIR} ${TS_FILES})
else()
    add_executable(NMEAPlot
         ${PROJECT_SOURCES}
    )

    qt5_create_translation(QM_FILES ${CMAKE_SOURCE_DIR} ${TS_FILES})
endif()

target_link_libraries(NMEAPlot PRIVATE Qt${QT_VERSION_MAJOR}::Widgets Qt${QT_VERSION_MAJOR}::Charts Qt${QT_VERSION_MAJOR}::Sql)

set_target_properties(NMEAPlot PROPERTIES
    MACOSX_BUNDLE_GUI_IDENTIFIER my.example.com
    MACOSX_BUNDLE_BUNDLE_VERSION ${PROJECT_VERSION}
    MACOSX_BUNDLE_SHORT_VERSION_STRING ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}
    MACOSX_BUNDLE TRUE
    WIN32_EXECUTABLE TRUE
)

if(QT_VERSION_MAJOR EQUAL 6)
    qt_finalize_executable(NMEAPlot)
endif()
