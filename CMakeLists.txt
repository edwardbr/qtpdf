cmake_minimum_required(VERSION 3.13)
if(NOT ANDROID)
    set(ANDROID_STL " ")
    set(ANDROID_ABI " ")
    set(ANDROID_PLATFORM " ")
endif()

set(CMAKE_VERBOSE_MAKEFILE ON)
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/cmake")

include(HunterGate)

HunterGate(
    URL "https://github.com/ruslo/hunter/archive/v0.23.214.tar.gz"
    SHA1 "e14bc153a7f16d6a5eeec845fb0283c8fad8c358"
    FILEPATH "${CMAKE_CURRENT_LIST_DIR}/cmake/Hunter/config.cmake"
)

set(HUNTER_KEEP_PACKAGE_SOURCES ON)

project(QTPDF)

message("Qt5")

if(EXISTS ${QT_CMAKE})

  set(Qt5Core_DIR "${QT_CMAKE}/Qt5Core")
  set(Qt5Gui_DIR "${QT_CMAKE}/Qt5Gui")
  set(Qt5_DIR "${QT_CMAKE}/Qt5")
  set(Qt5Qml_DIR "${QT_CMAKE}/Qt5Qml")
  set(Qt5Quick_DIR "${QT_CMAKE}/Qt5Quick")
  set(Qt5Widgets_DIR "${QT_CMAKE}/Qt5Widgets")
  set(Qt5QuickCompiler_DIR "${QT_CMAKE}/Qt5QuickCompiler")

  find_package(Qt5 COMPONENTS Core Gui Quick Widgets REQUIRED)
else()

  hunter_add_package(Qt)
  hunter_add_package(Qt COMPONENTS qtdeclarative)

  find_package(Qt5Core REQUIRED)
  find_package(Qt5Gui REQUIRED)
  find_package(Qt5Widgets REQUIRED)
  find_package(Qt5Quick REQUIRED)

endif()


add_subdirectory(src/3rdparty)
add_subdirectory(src/pdf)
add_subdirectory(src/pdfwidgets)
add_subdirectory(examples/pdf/pdfviewer)