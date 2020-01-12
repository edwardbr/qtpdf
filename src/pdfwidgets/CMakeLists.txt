cmake_minimum_required(VERSION 3.13)

project(pdfwidgets)

message(STATUS "doing ${PROJECT_NAME}")

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
add_library(${PROJECT_NAME} 
            STATIC 
                qpdfview.cpp
                qpdfview.h
                qpdfview_p.h
                qtpdfwidgetsglobal.h
            )

target_include_directories(${PROJECT_NAME} PUBLIC 
    .
    ${Qt5Core_DIR}/../../../include/QtWidgets/5.12.5
    ${Qt5Core_DIR}/../../..//include/QtWidgets/5.12.5/QtWidgets
    ${Qt5Core_DIR}/../../..//include/QtGui/5.12.5
    ${Qt5Core_DIR}/../../..//mkspecs/common/uikit
    ${Qt5Core_DIR}/../../..//include/QtGui/5.12.5/QtGui)

if(WIN32)
	target_compile_definitions(${PROJECT_NAME} BC_STATIC)
	target_compile_definitions(${PROJECT_NAME} NOMINMAX)
	target_compile_definitions(${PROJECT_NAME} _WIN32_WINNT=0x0A00)
	target_compile_definitions(${PROJECT_NAME} BOOST_AUTO_LINK_TAGGED)
else()
	target_compile_options(${PROJECT_NAME} PRIVATE "-mfpmath=sse")
    target_compile_options(${PROJECT_NAME} PRIVATE "-fvisibility=hidden")
    target_compile_options(${PROJECT_NAME} PRIVATE "-fvisibility-inlines-hidden")
	target_compile_options(${PROJECT_NAME} PRIVATE "-fno-exceptions")
    target_compile_options(${PROJECT_NAME} PRIVATE "-fPIC")


    target_compile_definitions(${PROJECT_NAME} PRIVATE _REENTRANT)
    target_compile_definitions(${PROJECT_NAME} PRIVATE QT_NO_NARROWING_CONVERSIONS_IN_CONNECT)
    target_compile_definitions(${PROJECT_NAME} PRIVATE QT_BUILD_PDFWIDGETS_LIB)
    target_compile_definitions(${PROJECT_NAME} PRIVATE QT_BUILDING_QT)
    target_compile_definitions(${PROJECT_NAME} PRIVATE QT_NO_CAST_TO_ASCII)
    target_compile_definitions(${PROJECT_NAME} PRIVATE QT_ASCII_CAST_WARNINGS)
    target_compile_definitions(${PROJECT_NAME} PRIVATE QT_MOC_COMPAT)
    target_compile_definitions(${PROJECT_NAME} PRIVATE QT_USE_QSTRINGBUILDER)
    target_compile_definitions(${PROJECT_NAME} PRIVATE QT_DEPRECATED_WARNINGS)
    target_compile_definitions(${PROJECT_NAME} PRIVATE QT_DISABLE_DEPRECATED_BEFORE=0x050000)
    target_compile_definitions(${PROJECT_NAME} PRIVATE QT_NO_EXCEPTIONS)
    target_compile_definitions(${PROJECT_NAME} PRIVATE QT_NO_DEBUG)
    target_compile_definitions(${PROJECT_NAME} PRIVATE QT_PDF_LIB)
    target_compile_definitions(${PROJECT_NAME} PRIVATE QT_WIDGETS_LIB)
    target_compile_definitions(${PROJECT_NAME} PRIVATE QT_GUI_LIB)
    target_compile_definitions(${PROJECT_NAME} PRIVATE QT_CORE_LIB)
endif()

target_link_libraries(${PROJECT_NAME} 
                        PUBLIC 
                            Qt5::Core 
                            Qt5::Quick
                            Qt5::Gui
                            Qt5::Widgets
                            pdf
                            pdfium
                    )

message(STATUS "end doing ${PROJECT_NAME}")
