##############################################################################
# CMake
##############################################################################

cmake_minimum_required(VERSION 2.8.0)
project(project_name)
set(CMAKE_INCLUDE_CURRENT_DIR ON)

##############################################################################
# Catkin
##############################################################################
# qt_build provides the qt cmake glue, roscpp the comms for a default talker
find_package(catkin REQUIRED COMPONENTS roscpp std_msgs)
include_directories(${catkin_INCLUDE_DIRS})
# Use this to define what the package will export (e.g. libs, headers).
# Since the default here is to produce only a binary, we don't worry about
# exporting anything. 
catkin_package()

##############################################################################
# Qt Environment
##############################################################################
set(Qt5_DIR "/home/omar/Qt/5.15.2/gcc_64/lib/cmake/Qt5" CACHE PATH "Initial cahe" FORCE)
find_package(Qt5 REQUIRED COMPONENTS Core Gui Widgets)
set(QT_LIBRARIES Qt5::Widgets Qt5::Core Qt5::Gui)

##############################################################################
# Sections
##############################################################################

file(GLOB QT_FORMS RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} Forms/*.ui)
file(GLOB QT_RESOURCES RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} Resources/*.qrc)
file(GLOB_RECURSE QT_MOC RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} FOLLOW_SYMLINKS Headers/*.h)

QT5_ADD_RESOURCES(QT_RESOURCES_CPP ${QT_RESOURCES})
QT5_WRAP_UI(QT_FORMS_HPP ${QT_FORMS})
QT5_WRAP_CPP(QT_MOC_HPP ${QT_MOC})



##############################################################################
# Sources
##############################################################################

file(GLOB_RECURSE QT_SOURCES RELATIVE ${CMAKE_CURRENT_SOURCE_DIR} FOLLOW_SYMLINKS Sources/*.cpp)

##############################################################################
# Binaries
##############################################################################

add_executable(project_name ${QT_SOURCES} ${QT_RESOURCES_CPP} ${QT_FORMS_HPP} ${QT_MOC_HPP})
target_link_libraries(project_name ${QT_LIBRARIES} ${catkin_LIBRARIES})
install(TARGETS project_name RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})

