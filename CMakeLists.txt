cmake_minimum_required(VERSION 3.14)
project(linger VERSION 0.0.1)

include(CTest)

add_library(linger INTERFACE)
target_include_directories(linger
    INTERFACE
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
)

target_sources(linger
    INTERFACE
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/linear_algebra.hpp>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/linear_algebra/addition_traits.hpp>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/linear_algebra/addition_traits_impl.hpp>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/linear_algebra/arithmetic_operators.hpp>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/linear_algebra/arithmetic_traits.hpp>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/linear_algebra/debug_helpers.hpp>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/linear_algebra/dynamic_engines.hpp>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/linear_algebra/engine_traits.hpp>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/linear_algebra/fixed_size_engines.hpp>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/linear_algebra/forward_declarations.hpp>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/linear_algebra/library_aliases.hpp>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/linear_algebra/matrix.hpp>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/linear_algebra/multiplication_traits.hpp>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/linear_algebra/multiplication_traits_impl.hpp>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/linear_algebra/negation_traits.hpp>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/linear_algebra/negation_traits_impl.hpp>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/linear_algebra/number_traits.hpp>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/linear_algebra/operation_traits.hpp>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/linear_algebra/row_column_views.hpp>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/linear_algebra/subtraction_traits.hpp>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/linear_algebra/subtraction_traits_impl.hpp>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/linear_algebra/transpose_views.hpp>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include/linear_algebra/vector.hpp>
        $<INSTALL_INTERFACE:include/linear_algebra.hpp>
        $<INSTALL_INTERFACE:include/linear_algebra/addition_traits.hpp>
        $<INSTALL_INTERFACE:include/linear_algebra/addition_traits_impl.hpp>
        $<INSTALL_INTERFACE:include/linear_algebra/arithmetic_operators.hpp>
        $<INSTALL_INTERFACE:include/linear_algebra/arithmetic_traits.hpp>
        $<INSTALL_INTERFACE:include/linear_algebra/debug_helpers.hpp>
        $<INSTALL_INTERFACE:include/linear_algebra/dynamic_engines.hpp>
        $<INSTALL_INTERFACE:include/linear_algebra/engine_traits.hpp>
        $<INSTALL_INTERFACE:include/linear_algebra/fixed_size_engines.hpp>
        $<INSTALL_INTERFACE:include/linear_algebra/forward_declarations.hpp>
        $<INSTALL_INTERFACE:include/linear_algebra/library_aliases.hpp>
        $<INSTALL_INTERFACE:include/linear_algebra/matrix.hpp>
        $<INSTALL_INTERFACE:include/linear_algebra/multiplication_traits.hpp>
        $<INSTALL_INTERFACE:include/linear_algebra/multiplication_traits_impl.hpp>
        $<INSTALL_INTERFACE:include/linear_algebra/negation_traits.hpp>
        $<INSTALL_INTERFACE:include/linear_algebra/negation_traits_impl.hpp>
        $<INSTALL_INTERFACE:include/linear_algebra/number_traits.hpp>
        $<INSTALL_INTERFACE:include/linear_algebra/operation_traits.hpp>
        $<INSTALL_INTERFACE:include/linear_algebra/row_column_views.hpp>
        $<INSTALL_INTERFACE:include/linear_algebra/subtraction_traits.hpp>
        $<INSTALL_INTERFACE:include/linear_algebra/subtraction_traits_impl.hpp>
        $<INSTALL_INTERFACE:include/linear_algebra/transpose_views.hpp>
        $<INSTALL_INTERFACE:include/linear_algebra/vector.hpp>
)

target_compile_features(linger
    INTERFACE
        cxx_std_17
)

add_library(linger::linger ALIAS linger)

add_executable(linger_test "")

target_include_directories(linger_test
    PRIVATE
        test
)

target_sources(linger_test
    PRIVATE
        test/test_new_arithmetic.hpp
        test/test_new_engine.hpp
        test/test_new_number.hpp
        test/test_obj_matrix.cpp
        test/test_op_add.cpp
        test/test_op_mul.cpp
        test/test_op_neg.cpp
        test/test_op_sub.cpp
 #       test/test_01.cpp
 #       test/test_02.cpp
        test/test_main.cpp
)

target_link_libraries(linger_test
    PRIVATE
        linger::linger
)

set_target_properties(linger_test PROPERTIES
    CXX_STANDARD 17
    CXX_STANDARD_REQUIRED YES
    CXX_EXTENSIONS NO
)

target_compile_options(linger_test
    PRIVATE
        $<$<OR:$<CXX_COMPILER_ID:GNU>>:-Wall -pedantic -Wextra  -Wno-unused-function -Wno-unused-variable -W -Wno-unused-but-set-variable -fmax-errors=10>
        $<$<OR:$<CXX_COMPILER_ID:AppleClang>,$<CXX_COMPILER_ID:Clang>>:-Wall -pedantic -Wextra -Wno-unused-parameter -Wno-unused-function>
)

# This should be passed in via the commmand line ("cmake --build ./ -v" or "cmake --build ./ -DCMAKE_VERBOSE_MAKEFILE=1")
#set(CMAKE_VERBOSE_MAKEFILE 1)

include(GNUInstallDirs)

# Hierarchically copy headers to the install dir
install (
    DIRECTORY
        "${CMAKE_CURRENT_SOURCE_DIR}/include/linear_algebra"
    DESTINATION
        ${CMAKE_INSTALL_INCLUDEDIR}
    FILES_MATCHING PATTERN
        "*.hpp"
)

install(
    TARGETS linger
    DESTINATION lib/cmake/linger
    EXPORT linger-target
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

install(
    EXPORT linger-target
    NAMESPACE linger::
    #FILE linger-target.cmake
    DESTINATION "${CMAKE_INSTALL_LIBDIR}/cmake/linger"
)

include(CMakePackageConfigHelpers)
configure_package_config_file(
    ${CMAKE_CURRENT_LIST_DIR}/linger-config.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/linger-config.cmake
    INSTALL_DESTINATION
        "${CMAKE_INSTALL_LIBDIR}/cmake/linger"
)

write_basic_package_version_file(
    ${CMAKE_CURRENT_BINARY_DIR}/linger-version.cmake
    VERSION ${PROPAGATE_CONST_VERSION}
    COMPATIBILITY SameMajorVersion
    ARCH_INDEPENDENT
)

install(
    FILES
        ${CMAKE_CURRENT_BINARY_DIR}/linger-config.cmake
        ${CMAKE_CURRENT_BINARY_DIR}/linger-version.cmake
    DESTINATION
        "${CMAKE_INSTALL_LIBDIR}/cmake/linger"
)
