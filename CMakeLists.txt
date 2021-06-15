cmake_minimum_required(VERSION 3.19)

set(LibUSB_FIND_STATIC ON)
set(BUILD_EXAMPLES OFF)
set(BUILD_UNIT_TESTS OFF)
set(CHECK_FOR_UPDATES OFF)
set(BUILD_SHARED_LIBS OFF CACHE "" INTERNAL)
set(CMAKE_POLICY_DEFAULT_CMP0077 NEW)

add_subdirectory(librealsense)

include(${CMAKE_CURRENT_SOURCE_DIR}/max-sdk/script/max-package.cmake)

set(CMAKE_LIBRARY_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/jit.realsense/externals")
include(${CMAKE_CURRENT_SOURCE_DIR}/max-sdk/script/max-pretarget.cmake)

include_directories( 
	"${MAX_SDK_INCLUDES}"
	"${MAX_SDK_MSP_INCLUDES}"
	"${MAX_SDK_JIT_INCLUDES}"
)

add_library(${PROJECT_NAME} MODULE 
  jit.realsense.cpp 
  max.jit.realsense.cpp  
  ${MAX_SDK_INCLUDES}/common/commonsyms.c
)
target_link_libraries(${PROJECT_NAME} PRIVATE realsense2)

# comment out if only building for intel, or change path to location of your arm64 libusb
if(APPLE)
  target_link_libraries(${PROJECT_NAME} PRIVATE "/opt/homebrew/lib/libusb-1.0.dylib")
endif()

include(${CMAKE_CURRENT_SOURCE_DIR}/max-sdk/script/max-posttarget.cmake)
