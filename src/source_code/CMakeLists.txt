cmake_minimum_required(VERSION 3.5)

project(3dviewer10 VERSION 0.1 LANGUAGES C CXX)

#set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

#find_package(QT NAMES Qt6 Qt5 COMPONENTS Widgets REQUIRED)
find_package(QT NAMES Qt6 Qt5 COMPONENTS Widgets OpenGL OpenGLWidgets REQUIRED)
find_package(Qt${QT_VERSION_MAJOR} COMPONENTS Widgets OpenGL OpenGLWidgets REQUIRED)

add_subdirectory(QtGifImage)

#set_source_files_properties(
#    aff_transformation.c
#    s21_viewer.c
#    PROPERTIES LANGUAGE C
#)

include_directories(.)

set(PROJECT_SOURCES
        main.cpp
        viewer.cpp
        viewer.h
        viewer.ui
        scene.cpp scene.h
        s21_viewer.c s21_viewer.h
        aff_transformation.c aff_transformation.h
)

if(${QT_VERSION_MAJOR} GREATER_EQUAL 6)
    qt_add_executable(3dviewer10
        MANUAL_FINALIZATION
        ${PROJECT_SOURCES}
    )
# Define target properties for Android with Qt 6 as:
#    set_property(TARGET 3dviewer10 APPEND PROPERTY QT_ANDROID_PACKAGE_SOURCE_DIR
#                 ${CMAKE_CURRENT_SOURCE_DIR}/android)
# For more information, see https://doc.qt.io/qt-6/qt-add-executable.html#target-creation
else()
    if(ANDROID)
        add_library(3dviewer10 SHARED
            ${PROJECT_SOURCES}
        )
# Define properties for Android with Qt 5 after find_package() calls as:
#    set(ANDROID_PACKAGE_SOURCE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/android")
    else()
        add_executable(3dviewer10
            ${PROJECT_SOURCES}
        )
    endif()
endif()

target_link_libraries(3dviewer10 PRIVATE Qt${QT_VERSION_MAJOR}::Widgets Qt${QT_VERSION_MAJOR}::OpenGL Qt${QT_VERSION_MAJOR}::OpenGLWidgets QtGifImage::QtGifImage)

#target_link_libraries(3dviewer10 PRIVATE Qt${QT_VERSION_MAJOR}::Widgets)

set_target_properties(3dviewer10 PROPERTIES
    MACOSX_BUNDLE_GUI_IDENTIFIER my.example.com
    MACOSX_BUNDLE_BUNDLE_VERSION ${PROJECT_VERSION}
    MACOSX_BUNDLE_SHORT_VERSION_STRING ${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}
    MACOSX_BUNDLE TRUE
    WIN32_EXECUTABLE TRUE
)

if(QT_VERSION_MAJOR EQUAL 6)
    qt_finalize_executable(3dviewer10)
endif()
