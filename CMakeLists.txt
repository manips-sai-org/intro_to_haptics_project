
cmake_minimum_required (VERSION 2.8.12)
project (SHORT_PROJECT)
SET(CMAKE_CXX_FLAGS "-std=c++11 -I/usr/include -I/usr/local/include")
if(${CMAKE_SYSTEM_NAME} MATCHES Darwin)
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -I/opt/homebrew/include")
endif()

# set common source
set (COMMON_SOURCE
	${PROJECT_SOURCE_DIR}/src/timer/LoopTimer.h
	${PROJECT_SOURCE_DIR}/src/force_sensor/ForceSensorSim.h
	${PROJECT_SOURCE_DIR}/src/force_sensor/ForceSensorDisplay.h)
include_directories(${PROJECT_SOURCE_DIR}/src)

# # set common dependencies
# # - eigen3 # must be loaded before chai to avoid a conflict between the system Eigen and the Chai eigen.
find_package(Eigen3 REQUIRED)
include_directories(${EIGEN3_INCLUDE_DIR})

# - CHAI3D
find_package(CHAI3D REQUIRED)
include_directories(${CHAI3D_INCLUDE_DIRS})
add_definitions(${CHAI3D_DEFINITIONS})
if (CMAKE_SYSTEM_NAME MATCHES Darwin)
  add_definitions(-D_MACOSX -DMACOSX)
elseif (CMAKE_SYSTEM_NAME MATCHES Linux)
  add_definitions(-D_LINUX -DLINUX)
endif () #NOTE: Windows is currently unsupported
link_directories(${CHAI3D_LIBRARY_DIRS})

# - SAI2-SIMULATION. TODO: this should not be needed. something fishy.
find_package(SAI2-SIMULATION REQUIRED)
include_directories(${SAI2-SIMULATION_INCLUDE_DIRS})
add_definitions(${SAI2-SIMULATION_DEFINITIONS})

# - SAI2-URDF. TODO: this should not be needed. something fishy.
find_package(SAI2-URDF REQUIRED)
include_directories(${SAI2-URDF_INCLUDE_DIRS})
add_definitions(${SAI2-URDF_DEFINITIONS})

# - SAI2-MODEL. TODO: this should not be needed. something fishy.
find_package(SAI2-MODEL REQUIRED)
include_directories(${SAI2-MODEL_INCLUDE_DIRS})
add_definitions(${SAI2-MODEL_DEFINITIONS})

# - SAI2-GRAPHICS. TODO: this should not be needed. something fishy.
find_package(SAI2-GRAPHICS REQUIRED)
include_directories(${SAI2-GRAPHICS_INCLUDE_DIRS})
add_definitions(${SAI2-GRAPHICS_DEFINITIONS})

# - SAI2-COMMON
find_package(SAI2-COMMON REQUIRED)
include_directories(${SAI2-COMMON_INCLUDE_DIRS})
add_definitions(${SAI2-COMMON_DEFINITIONS})

# SAI2-PRIMITIVES
find_package(SAI2-PRIMITIVES REQUIRED)
include_directories(${SAI2-PRIMITIVES_INCLUDE_DIRS})
add_definitions(${SAI2-PRIMITIVES_DEFINITIONS})

# SAI2-INTERFACES
find_package(SAI2-INTERFACES REQUIRED)
include_directories(${SAI2-INTERFACES_INCLUDE_DIRS})

# - glfw3
find_package(glfw3 QUIET)
find_library(GLFW_LIBRARY glfw)

# - rbdl
find_package(RBDL QUIET)
include_directories(${RBDL_INCLUDE_DIR})

set(COMMON_LIBRARIES
    ${CHAI3D_LIBRARIES}
    ${SAI2-URDF_LIBRARIES}
    ${SAI2-SIMULATION_LIBRARIES}
    ${SAI2-MODEL_LIBRARIES}
    ${SAI2-GRAPHICS_LIBRARIES}
    ${SAI2-INTERFACES_LIBRARIES}
    ${GLFW_LIBRARY}
)

# add apps
set (CMAKE_RUNTIME_OUTPUT_DIRECTORY                ${PROJECT_SOURCE_DIR}/bin)
set (CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG          ${PROJECT_SOURCE_DIR}/bin)
set (CMAKE_RUNTIME_OUTPUT_DIRECTORY_MINSIZEREL     ${PROJECT_SOURCE_DIR}/bin)
set (CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE        ${PROJECT_SOURCE_DIR}/bin)
set (CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELWITHDEBINFO ${PROJECT_SOURCE_DIR}/bin)
set (BINARY_DIR ${PROJECT_SOURCE_DIR}/bin)
SET (RESOURCE_DIR ${BINARY_DIR}/resources)
FILE(MAKE_DIRECTORY ${RESOURCE_DIR})

add_subdirectory(task1)
add_subdirectory(task2)
add_subdirectory(task3)
#add_subdirectory(task4)
