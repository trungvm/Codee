cmake_minimum_required(VERSION 2.8.12)

project(offboard)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/CMakeModules/")

find_package(OpenCV REQUIRED)
find_package(LibRealSense QUIET)


if(NOT MSVC)

    add_definitions("-std=c++11 -Wall -Wextra -Werror")
    # Line below required if /usr/local/include is not in your default includes
    include_directories(/usr/local/include)
    # Line below required if /usr/local/lib is not in your default linker path
    link_directories(/usr/local/lib)
else()
    add_definitions("-std=c++11 -WX -W2")
    add_definitions("-D_USE_MATH_DEFINES") # For M_PI
    include_directories(${CMAKE_SOURCE_DIR}/../../install/include)
    link_directories(${CMAKE_SOURCE_DIR}/../../install/lib)
endif()

add_executable(offboard
    offboardcontrol.cpp rsCam.h rsCam.cpp
)

target_link_libraries(offboard
    dronecode_sdk
    dronecode_sdk_action
    dronecode_sdk_offboard
    dronecode_sdk_telemetry
	${OpenCV_LIBS} 
	${LIBREALSENSE_LIBRARY} 
)
