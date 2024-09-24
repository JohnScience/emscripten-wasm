cmake_minimum_required(VERSION 3.8)

project(test)

set (CMAKE_CXX_STANDARD 17)

add_library(calc SHARED calc.cpp)

if(WIN32)
    set_target_properties(calc PROPERTIES WINDOWS_EXPORT_ALL_SYMBOLS TRUE)

    # Set output directories for Windows
    set_target_properties(calc PROPERTIES
        ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
        LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/lib"
        RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin")
endif()

add_executable(test test.cpp)
target_link_libraries(test PRIVATE calc) 

set_target_properties(test PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin"
)

add_custom_target(run
    COMMAND test
    DEPENDS test
    WORKING_DIRECTORY ${CMAKE_RUNTIME_OUTPUT_DIRECTORY}
    COMMENT "Running the test program..."
)
