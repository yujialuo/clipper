# Define library. Only source files here!
cmake_minimum_required(VERSION 3.2 FATAL_ERROR)
project(libclipper VERSION 0.1.0 LANGUAGES CXX)


add_library(clipper
    src/query_processor.cpp
    src/datatypes.cpp
    src/metrics.cpp
    src/selection_policies.cpp
    src/persistent_state.cpp
    src/task_executor.cpp
    src/rpc_service.cpp
    src/timers.cpp
    src/containers.cpp
    src/redis.cpp
    )


# Define headers for this library. PUBLIC headers are used for
# compiling the library, and will be added to consumers' build
# paths.
target_include_directories(clipper PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>
    PRIVATE src)


target_link_libraries(clipper boost zmqcpp redox pthread)

# This makes the project importable from the build directory
export(TARGETS clipper FILE ClipperConfig.cmake)



add_executable(libclippertests EXCLUDE_FROM_ALL
    test/test_main.cpp
    test/metrics_test.cpp
    test/rpc_service_test.cpp
    test/timers_test.cpp
    test/input_test.cpp
    test/persistent_state_test.cpp
    test/redis_test.cpp
    test/config_test.cpp
    test/selection_policies_test.cpp
    )
target_link_libraries(libclippertests clipper gtest gmock_main)
add_dependencies(unittests libclippertests)

