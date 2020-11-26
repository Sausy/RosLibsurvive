cmake_minimum_required(VERSION 3.15)
project(camerasterio)

message("PROJECT_SOURCE_DIR: ${PROJECT_SOURCE_DIR}")
#SET(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)
#SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -g")
#set(CMAKE_CXX_STANDARD 14)
#SET(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fPIC -std=gnu99 -rdynamic -Werror=incompatible-pointer-types -Wall -Wno-unused-variable -Wno-switch -Wno-parentheses -Wno-missing-braces -Werror=implicit-function-declaration -Wpedantic -Werror=return-type -fvisibility=hidden -Werror=vla")

#SET(CMAKE_BUILD_TYPE Debug)

#==============================================================================
# ROS Dependencies
#==============================================================================
find_package(catkin REQUIRED COMPONENTS
				roscpp
				std_msgs
				message_generation
				tf
				tf_conversions
				)

catkin_package(CATKIN_DEPENDS
        message_runtime
        )

include_directories(
        ${catkin_INCLUDE_DIRS}
        )

#==== LIBSURVIVE ====
#LIST(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/
include_directories(
${PROJECT_SOURCE_DIR}/../../../libsurvive/include/libsurvive
${PROJECT_SOURCE_DIR}/../../../libsurvive/redist
)

set(LIBS_SURVIVE
${PROJECT_SOURCE_DIR}/../../../libsurvive/bin/*.so
)
#==============================================================================
# Main Project files
#==============================================================================
FILE(GLOB SRC_FILES src/*.cpp)
FILE(GLOB INCLUDE_FILES src/*.hpp)


#==============================================================================
# Project specific defines
#==============================================================================
#preprocessor definition for absolut path to project
add_definitions(-DSEARCH_PATH="${CMAKE_SOURCE_DIR}")

#==============================================================================
# Dependencies
#==============================================================================
include_directories(
        src/
        include/
      )

#==============================================================================
# Application
#==============================================================================
add_executable( ${PROJECT_NAME}
        ${SRC_FILES}
        ${INCLUDE_FILES}
        main.cpp
        #main_registration.cpp
        )


add_dependencies(${PROJECT_NAME}
  ${catkin_EXPORTED_TARGETS}
)

target_link_libraries( ${PROJECT_NAME} -lpthread -lrt ${catkin_LIBRARIES} ${LIBS_SURVIVE})
