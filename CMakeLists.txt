cmake_minimum_required(VERSION 3.14)

if (WIN32)
    project(qtTest LANGUAGES CXX)
elseif(UNIX)
    project(qtTest)
endif()

set(CMAKE_CONFIGURATION_TYPES "Release;RelWithDebInfo" CACHE STRING "" FORCE)

#======================= INCLUSION OF Qt =======================#
set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_PREFIX_PATH $ENV{Qt6_DIR})
find_package(Qt6 REQUIRED COMPONENTS Widgets Core Gui)

#=================== INCLUSION OF Project Files ====================#
set(FORMS_DIR "${CMAKE_SOURCE_DIR}/forms")
set(INCLUDE_DIR "${CMAKE_SOURCE_DIR}/include")
set(SOURCE_DIR "${CMAKE_SOURCE_DIR}/src")

include_directories(${FORMS_DIR})
include_directories(${INCLUDE_DIR})
include_directories(${SOURCE_DIR})

file(GLOB_RECURSE SOURCES
        "${FORMS_DIR}/*.ui"
        "${FORMS_DIR}/*.qrc"
        "${INCLUDE_DIR}/*.h"
        "${SOURCE_DIR}/*.cpp"
)

#=================== SETUP EXECTUABLE ====================#


# Add the forms directory to the AUTOUIC search paths
set(CMAKE_AUTOUIC_SEARCH_PATHS ${CMAKE_AUTOUIC_SEARCH_PATHS} ${FORMS_DIR})

# Add the executable
if (WIN32)
    add_executable(qtTest WIN32 ${SOURCES}
            src/main.cpp
            src/mainwindow.cpp
            include/mainwindow.h
            forms/mainwindow.ui)
elseif(UNIX)
    add_executable(qtTest ${SOURCES}
            src/main.cpp
            src/mainwindow.cpp
            include/mainwindow.h
            forms/mainwindow.ui)
endif()

# Add the target includes for MY_PROJECT
target_include_directories(qtTest PRIVATE ${FORMS_DIR})
target_include_directories(qtTest PRIVATE ${INCLUDE_DIR})
target_include_directories(qtTest PRIVATE ${SOURCE_DIR})

#===================== LINKING LIBRARIES =======================#
target_link_libraries(qtTest PRIVATE
        Qt6::Widgets
        Qt6::Gui
        Qt6::Core)