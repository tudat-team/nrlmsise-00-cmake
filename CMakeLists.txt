cmake_minimum_required(VERSION 3.21)
project(nrlmsise_00 VERSION 2.0 LANGUAGES C)

set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED ON)

include(GNUInstallDirs)

#=== Library =========================================================

add_library(nrlmsise_00 SHARED
    src/nrlmsise-00.c
    src/nrlmsise-00_data.c
)

add_library(nrlmsise_00::nrlmsise_00 ALIAS nrlmsise_00)

set_target_properties(nrlmsise_00 PROPERTIES
    C_STANDARD 99
    C_STANDARD_REQUIRED ON
)

target_compile_options(nrlmsise_00 PRIVATE -O2)

target_include_directories(nrlmsise_00 PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>
)

target_link_libraries(nrlmsise_00 PRIVATE m)

#=== Tests ===========================================================

if(PROJECT_IS_TOP_LEVEL)
    add_executable(nrlmsise_00_test test/nrlmsise-00_test.c)
    target_link_libraries(nrlmsise_00_test PRIVATE nrlmsise_00)

    include(CTest)
    add_test(NAME nrlmsise_00_test COMMAND nrlmsise_00_test)
endif()

#=== Install & Packaging =============================================

if(PROJECT_IS_TOP_LEVEL)
    install(TARGETS nrlmsise_00 EXPORT nrlmsise_00-targets
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR} COMPONENT Runtime
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR} COMPONENT Development
        RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR} COMPONENT Runtime
        INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
    )

    install(FILES include/nrlmsise-00.h DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})

    install(EXPORT nrlmsise_00-targets
        FILE nrlmsise_00-targets.cmake
        NAMESPACE nrlmsise_00::
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/nrlmsise_00
    )

    include(CMakePackageConfigHelpers)

    configure_package_config_file(
        ${CMAKE_CURRENT_SOURCE_DIR}/nrlmsise_00-config.cmake.in
        "${CMAKE_CURRENT_BINARY_DIR}/nrlmsise_00-config.cmake"
        INSTALL_DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/nrlmsise_00"
        NO_SET_AND_CHECK_MACRO
    )

    write_basic_package_version_file(
        "${CMAKE_CURRENT_BINARY_DIR}/nrlmsise_00-config-version.cmake"
        VERSION "${PROJECT_VERSION}"
        COMPATIBILITY AnyNewerVersion
    )

    install(FILES
        ${CMAKE_CURRENT_BINARY_DIR}/nrlmsise_00-config.cmake
        ${CMAKE_CURRENT_BINARY_DIR}/nrlmsise_00-config-version.cmake
        DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/nrlmsise_00
    )
endif()
