cmake_minimum_required(VERSION 3.13)

message(STATUS "doing IDeclare")

project(PDFViewer)

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTOUIC ON)
set(CMAKE_AUTORCC ON)

if(QUICK_COMPILER)
	qtquick_compiler_add_resources(RCC_SOURCES "resources.qrc")
else()
	qt5_add_resources(RCC_SOURCES "resources.qrc")
endif()

qt5_wrap_ui(UI mainwindow.ui)
message("UI (${UI}")

if(ANDROID)
    add_library(${PROJECT_NAME} SHARED 
        main.cpp
        mainwindow.cpp
        mainwindow.h
        pageselector.cpp
        pageselector.h
        zoomselector.cpp
        zoomselector.h
        ${UI}
        ${RCC_SOURCES})
else()
    add_executable(${PROJECT_NAME} 
        main.cpp
        mainwindow.cpp
        mainwindow.h
        pageselector.cpp
        pageselector.h
        zoomselector.cpp
        zoomselector.h
        ${UI}
        ${RCC_SOURCES})
endif()

target_compile_definitions(${PROJECT_NAME} PRIVATE $<$<OR:$<CONFIG:Debug>,$<CONFIG:RelWithDebInfo>>:QT_QML_DEBUG>)

target_link_libraries(${PROJECT_NAME}
                        PRIVATE                              
							Qt5::Core 
                            Qt5::Quick 
                            Qt5::Gui
                            Qt5::Widgets 
                            pdf
                            pdfium
                            pdfwidgets                            
                        )

message(STATUS "end doing ${PROJECT_NAME}")
