cmake_minimum_required(VERSION 3.10) 

project(pack-man-bot)

# "includes"
list(APPEND CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/accessibility/cmake)
set(ACCESSIBILITY_DIR ${CMAKE_SOURCE_DIR}/accessibility)
include(notice_message)

#[[
ABOUT:
This project will be used to compile everything within 
the pac-man-bot project.
There are some options, see below:
If you would like to compile just the ros packages, set the option 'JUST_ROS'.
If you owuld like to compile just the arduino libraries, set the option 'JUST_ARDUINO'
]]
option(JUST_ROS "used to compile only the catkin packages" off)
option(JUST_ARDUINO "used to only compile the arduino packages" off)

set(ARDUINO_BINARY_DIR ${CMAKE_SOURCE_DIR}/Arduino/libraries)

# catkin libraries and other libraries
if(JUST_ARDUINO)
    notice_message("excluding other packages")
else()
    notice_message("building non-arduino packages")
    # non-ros libraries
    add_subdirectory(${PROJECT_SOURCE_DIR}/submodules/controllers)
    add_subdirectory(submodules/noise-recognition)

    # ros packages
    add_subdirectory(${PROJECT_SOURCE_DIR}/catkin_ws/src/behaviors)
    add_subdirectory(${PROJECT_SOURCE_DIR}/catkin_ws/src/statemachine)

    # creating rossrc command if not already exeisting 
    notice_message("checking ros variables")
    set(ros_path "${CMAKE_SOURCE_DIR}/setup.sh")
    execute_process(COMMAND ${ACCESSIBILITY_DIR}/bash/add_ros_cmds.sh ${CMAKE_BINARY_DIR}/devel/setup.sh)
endif()

# arduino libraries
if(JUST_ROS)
    notice_message("excluding arduino libraires")
else()
    notice_message("adding arduino libraries")
    # add_subdirectory(submodules/drivers ARDUINO_BINARY_DIR)
endif()