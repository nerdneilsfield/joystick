cmake_minimum_required(VERSION 2.8.3)
project(joystick)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_BUILD_TYPE Release)
# Add shared library

add_library(libjoystick SHARED joystick/joystick.cc joystick/joystick.h)
target_include_directories(libjoystick PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
set_property(TARGET libjoystick PROPERTY CXX_STANDARD 11)
set_target_properties(libjoystick
    PROPERTIES
    PUBLIC_HEADER "joystick/joystick.h"
)

# Add static library

add_library(libjoystick-static STATIC joystick/joystick.cc joystick/joystick.h)
target_include_directories(libjoystick-static PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
set_property(TARGET libjoystick-static PROPERTY CXX_STANDARD 11)
set_target_properties(libjoystick-static
    PROPERTIES
    PUBLIC_HEADER "joystick/joystick.h"
)


# Add a test

add_executable(joystick_test test/test.cc)
target_include_directories(joystick_test PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
target_link_libraries(joystick_test libjoystick-static)
set_property(TARGET joystick_test PROPERTY CXX_STANDARD 11)


# Installation
install(TARGETS joystick_test libjoystick libjoystick-static
        RUNTIME DESTINATION bin
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib/static
        PUBLIC_HEADER DESTINATION include/
)