cmake_minimum_required(VERSION 3.10)
project(nrlmsise-00 VERSION 2.0)
set(nrlmsise_00_version 2.0)

set(CMAKE_C_STANDARD 99)

add_compile_options(
    -O2
    -fPIC
)

#=== Shared Library ==================================================

add_library(nrlmsise-00 SHARED
    src/nrlmsise-00.c
    src/nrlmsise-00_data.c
)

target_include_directories(nrlmsise-00 PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include> 
)

target_link_libraries(nrlmsise-00 m)

#=== Test ============================================================

add_executable(nrlmsise-00-test test/nrlmsise-00_test.c)

target_link_libraries(nrlmsise-00-test nrlmsise-00)

enable_testing()

add_test(NAME nrlmsise-00-test COMMAND nrlmsise-00-test)

#=== Package =========================================================

install(TARGETS nrlmsise-00 EXPORT nrlmsise-00-targets
    LIBRARY DESTINATION lib COMPONENT Runtime
    INCLUDES DESTINATION include
)

install(FILES include/nrlmsise-00.h DESTINATION include)

install(EXPORT nrlmsise-00-targets
    FILE nrlmsise-00-targets.cmake
    DESTINATION lib/cmake/nrlmsise-00
)

include(CMakePackageConfigHelpers)

configure_package_config_file(${CMAKE_CURRENT_SOURCE_DIR}/nrlmsise-00-config.cmake.in
    "${CMAKE_CURRENT_BINARY_DIR}/nrlmsise-00-config.cmake"
    INSTALL_DESTINATION "lib/cmake/nrlmsise-00"
    NO_SET_AND_CHECK_MACRO
    NO_CHECK_REQUIRED_COMPONENTS_MACRO
)

write_basic_package_version_file(
    "${CMAKE_CURRENT_BINARY_DIR}/nrlmsise-00-config-version.cmake"
    VERSION "${nrlmsise_00_version}"
    COMPATIBILITY AnyNewerVersion
)

install(FILES
    ${CMAKE_CURRENT_BINARY_DIR}/nrlmsise-00-config.cmake
    ${CMAKE_CURRENT_BINARY_DIR}/nrlmsise-00-config-version.cmake
    DESTINATION lib/cmake/nrlmsise-00
)

export(EXPORT nrlmsise-00-targets
    FILE "${CMAKE_CURRENT_BINARY_DIR}/nrlmsise-00-targets.cmake"
)

