cmake_minimum_required(VERSION 3.10) 

project(pack-man-bot)

# "includes"
list(APPEND CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/accessibility/cmake ${rosserial_arduino_PACKAGE_PATH}/cmake_scripts/modules)
set(ARDUINO_TOOLCHAIN_FILE ${ROSSERIAL_ARDUINO_TOOLCHAIN})

include(notice_message)
include(setup_ros_alias)
# include(Arduino)
message(STATUS "rosserial stuff: ${ROSSERIAL_ARDUINO_TOOLCHAIN}")
#[[
ABOUT:
This project will be used to compile everything within 
the pac-man-bot project.
There are some options, see below:
If you would like to compile just the ros packages, set the option 'JUST_ROS'.
If you owuld like to compile just the arduino libraries, set the option 'JUST_ARDUINO'
]]

# default build ros, but not arduino
option(EXCLUDE_ROS "used to compile only the catkin packages" off)
option(INCLUDE_ARDUINO "used to only compile the arduino packages" off)

# set(ARDUINO_BINARY_DIR ${CMAKE_SOURCE_DIR}/Arduino/libraries)
# set(ARDUINO_TOOLCHAIN_FILE "${CMAKE_SOURCE_DIR}/accessibility/cmake/Arduino-CMake-Toolchain")
# set(ROS_ARDUINO_TOOLCHAIN "${rosserial_arduino_PACKAGE_PATH}/cmake_scripts/toolchains/Arduino.cmake")
# non-ros/non-arduino libraries
add_subdirectory(${PROJECT_SOURCE_DIR}/submodules/controllers)
add_subdirectory(submodules/noise-recognition)

# arduino libraries
if(NOT INCLUDE_ARDUINO)
    notice_message("excluding arduino libraires")
else()
    notice_message("adding arduino libraries")
    add_subdirectory(submodules/drivers)
    
    # rosserial libraries 
    add_subdirectory(${PROJECT_SOURCE_DIR}/catkin_ws/src/arduino-ros)
endif()

# catkin libraries and other libraries
if(EXCLUDE_ROS)
    notice_message("excluding ros packages")
else()
    notice_message("building ros packages")
    # ros packages
    add_subdirectory(${PROJECT_SOURCE_DIR}/catkin_ws/src/behaviors)
    add_subdirectory(${PROJECT_SOURCE_DIR}/catkin_ws/src/statemachine)
    add_subdirectory(${PROJECT_SOURCE_DIR}/catkin_ws/src/ros-wrappers)

    # creating rossrc command if not already exeisting 
    notice_message("setting ros variables")
    set(ros_path "${CMAKE_BINARY_DIR}/devel/setup.sh")
    setup_ros_alias(${ros_path})
endif()

