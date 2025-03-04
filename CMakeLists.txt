cmake_minimum_required(VERSION 2.8.3)
# Compile options so compiles with ROS Kinetic
# Future releases may want to 
add_compile_options(-std=c++11)
project(vectornav)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
        roscpp
        message_generation
        dynamic_reconfigure
        )

add_message_files(
        FILES
        Ins.msg
)

generate_messages(
        DEPENDENCIES
        std_msgs
)

generate_dynamic_reconfigure_options(
        config/vn100.cfg
)

###################################
## catkin specific configuration ##
###################################
catkin_package(
        #  INCLUDE_DIRS include
        #  LIBRARIES vectornav
        CATKIN_DEPENDS roscpp sensor_msgs
        #  DEPENDS system_lib
)

###########
## Build ##
###########

#include(vnproglib-1.1/cpp/CMakeLists.txt)
add_subdirectory(vnproglib-1.1.5.0/cpp)

## Specify additional locations of header files
## Your package locations should be listed before other locations
include_directories(vnproglib-1.1.5.0/cpp/include ${catkin_INCLUDE_DIRS})

## Declare a cpp library
## Declare a cpp executable
add_executable(vnpub src/main.cpp)
add_dependencies(vnpub ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS} ${PROJECT_NAME}_generate_messages ${PROJECT_NAME}_gencfg)

## Specify libraries to link a library or executable target against
target_link_libraries(vnpub
        libvncxx
        ${catkin_LIBRARIES}
        )

## Mark executables and/or libraries for installation
install(TARGETS vnpub
        ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
        RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
        )
