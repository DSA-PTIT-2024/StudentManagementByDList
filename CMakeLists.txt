cmake_minimum_required(VERSION 3.21.1)

option(LINK_INSIGHT "Link Qt Insight Tracker library" ON)
option(BUILD_QDS_COMPONENTS "Build design studio components" ON)

project(StudentManagementByDListApp LANGUAGES CXX)

set(CMAKE_AUTOMOC ON)
set(CMAKE_OBJECT_PATH_MAX 512)
find_package(Qt6 6.2 REQUIRED COMPONENTS Core Gui Qml Quick)

if (Qt6_VERSION VERSION_GREATER_EQUAL 6.3)
    qt_standard_project_setup()
endif()

qt_add_executable(StudentManagementByDListApp src/main.cpp
    src/Containers/DList/DList.hpp src/Containers/DList/DListExceptions.hpp src/Containers/DList/DListIterator.hpp src/Containers/DList/DListNode.hpp src/Utilities/Allocator.hpp src/Utilities/Exception.hpp src/DSALibraries.hpp
    tablestudentcontroller.h tablestudentcontroller.cpp
    student.h student.cpp)

qt_add_resources(StudentManagementByDListApp "configuration"
    PREFIX "/"
    FILES
        qtquickcontrols2.conf
)

target_link_libraries(StudentManagementByDListApp PRIVATE
    Qt6::Core
    Qt6::Gui
    Qt6::Qml
    Qt6::Quick
)

if (BUILD_QDS_COMPONENTS)
    include(${CMAKE_CURRENT_SOURCE_DIR}/qmlcomponents)
endif()

include(${CMAKE_CURRENT_SOURCE_DIR}/qmlmodules)

if (LINK_INSIGHT)
    include(${CMAKE_CURRENT_SOURCE_DIR}/insight)
endif ()

include(GNUInstallDirs)
install(TARGETS StudentManagementByDListApp
    BUNDLE DESTINATION .
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

# make IDEs aware of the QML import path
set(QML_IMPORT_PATH ${PROJECT_BINARY_DIR}/qml CACHE PATH
    "Path to the custom QML components defined by the project")
