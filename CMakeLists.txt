# Copyright (c) 2023, ETH Zurich and UNC Chapel Hill.
# All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions are met:
#
#     * Redistributions of source code must retain the above copyright
#       notice, this list of conditions and the following disclaimer.
#
#     * Redistributions in binary form must reproduce the above copyright
#       notice, this list of conditions and the following disclaimer in the
#       documentation and/or other materials provided with the distribution.
#
#     * Neither the name of ETH Zurich and UNC Chapel Hill nor the names of
#       its contributors may be used to endorse or promote products derived
#       from this software without specific prior written permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
# AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
# IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
# ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDERS OR CONTRIBUTORS BE
# LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
# CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF
# SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
# INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN
# CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)
# ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
# POSSIBILITY OF SUCH DAMAGE.


cmake_minimum_required(VERSION 3.10)

################################################################################
# Options
################################################################################

option(SIMD_ENABLED "Whether to enable SIMD optimizations" ON)
option(OPENMP_ENABLED "Whether to enable OpenMP parallelization" ON)
option(IPO_ENABLED "Whether to enable interprocedural optimization" ON)
option(CUDA_ENABLED "Whether to enable CUDA, if available" ON)
option(GUI_ENABLED "Whether to enable the graphical UI" OFF)
option(OPENGL_ENABLED "Whether to enable OpenGL, if available" ON)
option(TESTS_ENABLED "Whether to build test binaries" OFF)
option(ASAN_ENABLED "Whether to enable AddressSanitizer flags" OFF)
option(PROFILING_ENABLED "Whether to enable google-perftools linker flags" OFF)
option(CCACHE_ENABLED "Whether to enable compiler caching, if available" ON)
option(CGAL_ENABLED "Whether to enable the CGAL library" ON)
option(LSD_ENABLED "Whether to enable the LSD library" ON)

# Propagate options to vcpkg manifest.
if(TESTS_ENABLED)
  enable_testing()
  list(APPEND VCPKG_MANIFEST_FEATURES "tests")
endif()
if(CUDA_ENABLED)
    list(APPEND VCPKG_MANIFEST_FEATURES "cuda")
endif()
if(GUI_ENABLED)
    list(APPEND VCPKG_MANIFEST_FEATURES "gui")
endif()
if(CGAL_ENABLED)
    list(APPEND VCPKG_MANIFEST_FEATURES "cgal")
endif()

if(LSD_ENABLED)
    message(STATUS "Enabling LSD support")
    add_definitions("-DCOLMAP_LSD_ENABLED")
else()
    message(STATUS "Disabling LSD support")
endif()

project(COLMAP LANGUAGES C CXX)

set(COLMAP_VERSION "3.10-dev")

set(VCPKG_ROOT_DIR "E:/lib/vcpkg")
set(CMAKE_TOOLCHAIN_FILE "${VCPKG_ROOT_DIR}/scripts/buildsystems/vcpkg.cmake")
include(${CMAKE_TOOLCHAIN_FILE})

set(CMAKE_INSTALL_PREFIX "E:/privateSpace/workSpace/504colmap/install")

#设置cmake的索引路径
# set(CMAKE_PREFIX_PATH
# ${CMAKE_PREFIX_PATH}
# E:/lib/vcpkg/packages/blas_x64-windows/share/blas
# E:/lib/vcpkg/packages/boost-accumulators_x64-windows/share/boost_accumulators
# E:/lib/vcpkg/packages/boost-algorithm_x64-windows/share/boost_algorithm
# E:/lib/vcpkg/packages/boost-align_x64-windows/share/boost_align
# E:/lib/vcpkg/packages/boost-any_x64-windows/share/boost_any
# E:/lib/vcpkg/packages/boost-array_x64-windows/share/boost_array
# E:/lib/vcpkg/packages/boost-assert_x64-windows/share/boost_assert
# E:/lib/vcpkg/packages/boost-atomic_x64-windows/share/boost_atomic
# E:/lib/vcpkg/packages/boost-bimap_x64-windows/share/boost_bimap
# E:/lib/vcpkg/packages/boost-bind_x64-windows/share/boost_bind
# E:/lib/vcpkg/packages/boost-callable-traits_x64-windows/share/boost_callable_traits
# E:/lib/vcpkg/packages/boost-chrono_x64-windows/share/boost_chrono
# E:/lib/vcpkg/packages/boost-circular-buffer_x64-windows/share/boost_circular_buffer
# E:/lib/vcpkg/packages/boost-cmake_x64-windows/share/boost
# E:/lib/vcpkg/packages/boost-cmake_x64-windows/share/boost-cmake
# E:/lib/vcpkg/packages/boost-cmake_x64-windows/share/boost/cmake-build
# E:/lib/vcpkg/packages/boost-compute_x64-windows/share/boost_compute
# E:/lib/vcpkg/packages/boost-concept-check_x64-windows/share/boost_concept_check
# E:/lib/vcpkg/packages/boost-config_x64-windows/share/boost_config
# E:/lib/vcpkg/packages/boost-container-hash_x64-windows/share/boost_container_hash
# E:/lib/vcpkg/packages/boost-container_x64-windows/share/boost_container
# E:/lib/vcpkg/packages/boost-conversion_x64-windows/share/boost_conversion
# E:/lib/vcpkg/packages/boost-core_x64-windows/share/boost_core
# E:/lib/vcpkg/packages/boost-date-time_x64-windows/share/boost_date_time
# E:/lib/vcpkg/packages/boost-describe_x64-windows/share/boost_describe
# E:/lib/vcpkg/packages/boost-detail_x64-windows/share/boost_detail
# E:/lib/vcpkg/packages/boost-dynamic-bitset_x64-windows/share/boost_dynamic_bitset
# E:/lib/vcpkg/packages/boost-endian_x64-windows/share/boost_endian
# E:/lib/vcpkg/packages/boost-exception_x64-windows/share/boost_exception
# E:/lib/vcpkg/packages/boost-filesystem_x64-windows/share/boost_filesystem
# E:/lib/vcpkg/packages/boost-foreach_x64-windows/share/boost_foreach
# E:/lib/vcpkg/packages/boost-format_x64-windows/share/boost_format
# E:/lib/vcpkg/packages/boost-function-types_x64-windows/share/boost_function_types
# E:/lib/vcpkg/packages/boost-function_x64-windows/share/boost_function
# E:/lib/vcpkg/packages/boost-functional_x64-windows/share/boost_functional
# E:/lib/vcpkg/packages/boost-fusion_x64-windows/share/boost_fusion
# E:/lib/vcpkg/packages/boost-geometry_x64-windows/share/boost_geometry
# E:/lib/vcpkg/packages/boost-graph_x64-windows/share/boost_graph
# E:/lib/vcpkg/packages/boost-headers_x64-windows/share/boost_headers
# E:/lib/vcpkg/packages/boost-heap_x64-windows/share/boost_heap
# E:/lib/vcpkg/packages/boost-integer_x64-windows/share/boost_integer
# E:/lib/vcpkg/packages/boost-interval_x64-windows/share/boost_numeric_interval
# E:/lib/vcpkg/packages/boost-intrusive_x64-windows/share/boost_intrusive
# E:/lib/vcpkg/packages/boost-io_x64-windows/share/boost_io
# E:/lib/vcpkg/packages/boost-iostreams_x64-windows/share/boost_iostreams
# E:/lib/vcpkg/packages/boost-iterator_x64-windows/share/boost_iterator
# E:/lib/vcpkg/packages/boost-lambda_x64-windows/share/boost_lambda
# E:/lib/vcpkg/packages/boost-lexical-cast_x64-windows/share/boost_lexical_cast
# E:/lib/vcpkg/packages/boost-logic_x64-windows/share/boost_logic
# E:/lib/vcpkg/packages/boost-math_x64-windows/share/boost_math
# E:/lib/vcpkg/packages/boost-move_x64-windows/share/boost_move
# E:/lib/vcpkg/packages/boost-mp11_x64-windows/share/boost_mp11
# E:/lib/vcpkg/packages/boost-mpl_x64-windows/share/boost_mpl
# E:/lib/vcpkg/packages/boost-multi-array_x64-windows/share/boost_multi_array
# E:/lib/vcpkg/packages/boost-multi-index_x64-windows/share/boost_multi_index
# E:/lib/vcpkg/packages/boost-multiprecision_x64-windows/share/boost_multiprecision
# E:/lib/vcpkg/packages/boost-numeric-conversion_x64-windows/share/boost_numeric_conversion
# E:/lib/vcpkg/packages/boost-optional_x64-windows/share/boost_optional
# E:/lib/vcpkg/packages/boost-parameter_x64-windows/share/boost_parameter
# E:/lib/vcpkg/packages/boost-phoenix_x64-windows/share/boost_phoenix
# E:/lib/vcpkg/packages/boost-polygon_x64-windows/share/boost_polygon
# E:/lib/vcpkg/packages/boost-pool_x64-windows/share/boost_pool
# E:/lib/vcpkg/packages/boost-predef_x64-windows/share/boost_predef
# E:/lib/vcpkg/packages/boost-preprocessor_x64-windows/share/boost_preprocessor
# E:/lib/vcpkg/packages/boost-program-options_x64-windows/share/boost_program_options
# E:/lib/vcpkg/packages/boost-property-map_x64-windows/share/boost_property_map
# E:/lib/vcpkg/packages/boost-property-tree_x64-windows/share/boost_property_tree
# E:/lib/vcpkg/packages/boost-proto_x64-windows/share/boost_proto
# E:/lib/vcpkg/packages/boost-ptr-container_x64-windows/share/boost_ptr_container
# E:/lib/vcpkg/packages/boost-qvm_x64-windows/share/boost_qvm
# E:/lib/vcpkg/packages/boost-random_x64-windows/share/boost_random
# E:/lib/vcpkg/packages/boost-range_x64-windows/share/boost_range
# E:/lib/vcpkg/packages/boost-ratio_x64-windows/share/boost_ratio
# E:/lib/vcpkg/packages/boost-rational_x64-windows/share/boost_rational
# E:/lib/vcpkg/packages/boost-regex_x64-windows/share/boost_regex
# E:/lib/vcpkg/packages/boost-scope_x64-windows/share/boost_scope
# E:/lib/vcpkg/packages/boost-serialization_x64-windows/share/boost_serialization
# E:/lib/vcpkg/packages/boost-serialization_x64-windows/share/boost_wserialization
# E:/lib/vcpkg/packages/boost-smart-ptr_x64-windows/share/boost_smart_ptr
# E:/lib/vcpkg/packages/boost-spirit_x64-windows/share/boost_spirit
# E:/lib/vcpkg/packages/boost-static-assert_x64-windows/share/boost_static_assert
# E:/lib/vcpkg/packages/boost-system_x64-windows/share/boost_system
# E:/lib/vcpkg/packages/boost-test_x64-windows/share/boost_included_prg_exec_monitor
# E:/lib/vcpkg/packages/boost-test_x64-windows/share/boost_included_test_exec_monitor
# E:/lib/vcpkg/packages/boost-test_x64-windows/share/boost_included_unit_test_framework
# E:/lib/vcpkg/packages/boost-test_x64-windows/share/boost_prg_exec_monitor
# E:/lib/vcpkg/packages/boost-test_x64-windows/share/boost_test_exec_monitor
# E:/lib/vcpkg/packages/boost-test_x64-windows/share/boost_unit_test_framework
# E:/lib/vcpkg/packages/boost-thread_x64-windows/share/boost_thread
# E:/lib/vcpkg/packages/boost-throw-exception_x64-windows/share/boost_throw_exception
# E:/lib/vcpkg/packages/boost-tokenizer_x64-windows/share/boost_tokenizer
# E:/lib/vcpkg/packages/boost-tti_x64-windows/share/boost_tti
# E:/lib/vcpkg/packages/boost-tuple_x64-windows/share/boost_tuple
# E:/lib/vcpkg/packages/boost-type-index_x64-windows/share/boost_type_index
# E:/lib/vcpkg/packages/boost-type-traits_x64-windows/share/boost_type_traits
# E:/lib/vcpkg/packages/boost-typeof_x64-windows/share/boost_typeof
# E:/lib/vcpkg/packages/boost-ublas_x64-windows/share/boost_numeric_ublas
# E:/lib/vcpkg/packages/boost-uninstall_x64-windows/share/boost
# E:/lib/vcpkg/packages/boost-units_x64-windows/share/boost_units
# E:/lib/vcpkg/packages/boost-unordered_x64-windows/share/boost_unordered
# E:/lib/vcpkg/packages/boost-utility_x64-windows/share/boost_utility
# E:/lib/vcpkg/packages/boost-uuid_x64-windows/share/boost_uuid
# E:/lib/vcpkg/packages/boost-variant2_x64-windows/share/boost_variant2
# E:/lib/vcpkg/packages/boost-variant_x64-windows/share/boost_variant
# E:/lib/vcpkg/packages/boost-winapi_x64-windows/share/boost_winapi
# E:/lib/vcpkg/packages/boost-xpressive_x64-windows/share/boost_xpressive
# E:/lib/vcpkg/packages/brotli_x64-windows/share/unofficial-brotli
# E:/lib/vcpkg/packages/ceres_x64-windows/share/ceres
# E:/lib/vcpkg/packages/cgal_x64-windows/include/CGAL/license
# E:/lib/vcpkg/packages/cgal_x64-windows/share/cgal
# E:/lib/vcpkg/packages/colmap_x64-windows/share/colmap
# E:/lib/vcpkg/packages/colmap_x64-windows/share/colmap/cmake
# E:/lib/vcpkg/packages/cuda_x64-windows/share/cuda
# E:/lib/vcpkg/packages/double-conversion_x64-windows/share/double-conversion
# E:/lib/vcpkg/packages/eigen3_x64-windows/share/eigen3
# E:/lib/vcpkg/packages/flann_x64-windows/share/flann
# E:/lib/vcpkg/packages/freeglut_x64-windows/share/freeglut
# E:/lib/vcpkg/packages/freeglut_x64-windows/share/glut
# E:/lib/vcpkg/packages/freeimage_x64-windows/share/freeimage
# E:/lib/vcpkg/packages/freetype_x64-windows/share/freetype
# E:/lib/vcpkg/packages/gflags_x64-windows/share/gflags
# E:/lib/vcpkg/packages/gklib_x64-windows/share/gklib
# E:/lib/vcpkg/packages/glew_x64-windows/share/glew
# E:/lib/vcpkg/packages/glog_x64-windows/share/glog
# E:/lib/vcpkg/packages/gtest_x64-windows/share/gtest
# E:/lib/vcpkg/packages/harfbuzz_x64-windows/share/harfbuzz
# E:/lib/vcpkg/packages/imath_x64-windows/share/imath
# E:/lib/vcpkg/packages/jxrlib_x64-windows/share/jxr
# E:/lib/vcpkg/packages/lapack-reference_x64-windows/share/lapack-reference
# E:/lib/vcpkg/packages/lapack-reference_x64-windows/share/lapack-reference/wrapper
# E:/lib/vcpkg/packages/lapack_x64-windows/share/lapack
# E:/lib/vcpkg/packages/lcms_x64-windows/share/lcms
# E:/lib/vcpkg/packages/lcms_x64-windows/share/lcms2
# E:/lib/vcpkg/packages/libdeflate_x64-windows/share/libdeflate
# E:/lib/vcpkg/packages/libjpeg-turbo_x64-windows/share/jpeg
# E:/lib/vcpkg/packages/libjpeg-turbo_x64-windows/share/libjpeg-turbo
# E:/lib/vcpkg/packages/liblzma_x64-windows/share/liblzma
# E:/lib/vcpkg/packages/libpng_x64-windows/share/libpng
# E:/lib/vcpkg/packages/libpng_x64-windows/share/png
# E:/lib/vcpkg/packages/libraw_x64-windows/share/libraw
# E:/lib/vcpkg/packages/libwebp_x64-windows/share/WebP
# E:/lib/vcpkg/packages/lz4_x64-windows/share/lz4
# E:/lib/vcpkg/packages/metis_x64-windows/share/metis
# E:/lib/vcpkg/packages/openblas_x64-windows/share/openblas
# E:/lib/vcpkg/packages/openexr_x64-windows/share/openexr
# E:/lib/vcpkg/packages/openjpeg_x64-windows/share/openjpeg
# E:/lib/vcpkg/packages/openssl_x64-windows/share/openssl
# E:/lib/vcpkg/packages/pcre2_x64-windows/share/pcre2
# E:/lib/vcpkg/packages/qt5-base_x64-windows/share/cmake/Qt5
# E:/lib/vcpkg/packages/qt5-base_x64-windows/share/cmake/Qt5AccessibilitySupport
# E:/lib/vcpkg/packages/qt5-base_x64-windows/share/cmake/Qt5Concurrent
# E:/lib/vcpkg/packages/qt5-base_x64-windows/share/cmake/Qt5Core
# E:/lib/vcpkg/packages/qt5-base_x64-windows/share/cmake/Qt5DBus
# E:/lib/vcpkg/packages/qt5-base_x64-windows/share/cmake/Qt5DeviceDiscoverySupport
# E:/lib/vcpkg/packages/qt5-base_x64-windows/share/cmake/Qt5EdidSupport
# E:/lib/vcpkg/packages/qt5-base_x64-windows/share/cmake/Qt5EventDispatcherSupport
# E:/lib/vcpkg/packages/qt5-base_x64-windows/share/cmake/Qt5FbSupport
# E:/lib/vcpkg/packages/qt5-base_x64-windows/share/cmake/Qt5FontDatabaseSupport
# E:/lib/vcpkg/packages/qt5-base_x64-windows/share/cmake/Qt5Gui
# E:/lib/vcpkg/packages/qt5-base_x64-windows/share/cmake/Qt5Network
# E:/lib/vcpkg/packages/qt5-base_x64-windows/share/cmake/Qt5OpenGL
# E:/lib/vcpkg/packages/qt5-base_x64-windows/share/cmake/Qt5OpenGLExtensions
# E:/lib/vcpkg/packages/qt5-base_x64-windows/share/cmake/Qt5PlatformCompositorSupport
# E:/lib/vcpkg/packages/qt5-base_x64-windows/share/cmake/Qt5PrintSupport
# E:/lib/vcpkg/packages/qt5-base_x64-windows/share/cmake/Qt5Sql
# E:/lib/vcpkg/packages/qt5-base_x64-windows/share/cmake/Qt5Test
# E:/lib/vcpkg/packages/qt5-base_x64-windows/share/cmake/Qt5ThemeSupport
# E:/lib/vcpkg/packages/qt5-base_x64-windows/share/cmake/Qt5Widgets
# E:/lib/vcpkg/packages/qt5-base_x64-windows/share/cmake/Qt5WindowsUIAutomationSupport
# E:/lib/vcpkg/packages/qt5-base_x64-windows/share/cmake/Qt5Xml
# E:/lib/vcpkg/packages/qt5-base_x64-windows/share/qt5
# E:/lib/vcpkg/packages/sqlite3_x64-windows/share/unofficial-sqlite3
# E:/lib/vcpkg/packages/suitesparse_x64-windows/share/cxsparse
# E:/lib/vcpkg/packages/suitesparse_x64-windows/share/suitesparse
# E:/lib/vcpkg/packages/tiff_x64-windows/share/tiff
# E:/lib/vcpkg/packages/vcpkg-boost_x64-windows/share/vcpkg-boost
# E:/lib/vcpkg/packages/vcpkg-cmake-config_x64-windows/share/vcpkg-cmake-config
# E:/lib/vcpkg/packages/vcpkg-cmake-get-vars_x64-windows/share/vcpkg-cmake-get-vars
# E:/lib/vcpkg/packages/vcpkg-cmake_x64-windows/share/vcpkg-cmake
# E:/lib/vcpkg/packages/vcpkg-pkgconfig-get-modules_x64-windows/share/vcpkg-pkgconfig-get-modules
# E:/lib/vcpkg/packages/vcpkg-tool-meson_x64-windows/share/vcpkg-tool-meson
# E:/lib/vcpkg/packages/zlib_x64-windows/share/zlib
# E:/lib/vcpkg/packages/zstd_x64-windows/share/zstd
# )

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CUDA_STANDARD 14)
set(CMAKE_CUDA_STANDARD_REQUIRED ON)

set_property(GLOBAL PROPERTY GLOBAL_DEPENDS_NO_CYCLES ON)

################################################################################
# Include CMake dependencies
################################################################################

set(CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

include(CheckCXXCompilerFlag)

# Include helper macros and commands, and allow the included file to override
# the CMake policies in this file
include(${CMAKE_CURRENT_SOURCE_DIR}/cmake/CMakeHelper.cmake NO_POLICY_SCOPE)

# Build position-independent code, so that shared libraries can link against
# COLMAP's static libraries.
set(CMAKE_POSITION_INDEPENDENT_CODE ON)

################################################################################
# Dependency configuration
################################################################################

set(COLMAP_FIND_QUIETLY FALSE)
include(cmake/FindDependencies.cmake)

################################################################################
# Compiler specific configuration
################################################################################

if(CMAKE_BUILD_TYPE)
    message(STATUS "Build type specified as ${CMAKE_BUILD_TYPE}")
else()
    message(STATUS "Build type not specified, using Release")
    set(CMAKE_BUILD_TYPE Release)
    set(IS_DEBUG OFF)
endif()

if("${CMAKE_BUILD_TYPE}" STREQUAL "ClangTidy")
    find_program(CLANG_TIDY_EXE NAMES clang-tidy)
    if(NOT CLANG_TIDY_EXE)
        message(FATAL_ERROR "Could not find the clang-tidy executable, please set CLANG_TIDY_EXE")
    endif()
else()
    unset(CLANG_TIDY_EXE)
endif()

if(IS_MSVC)
    # Some fixes for the Glog library.
    add_definitions("-DGLOG_NO_ABBREVIATED_SEVERITIES")
    add_definitions("-DGL_GLEXT_PROTOTYPES")
    add_definitions("-DNOMINMAX")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /EHsc")
    # Disable warning: 'initializing': conversion from 'X' to 'Y', possible loss of data
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /wd4244 /wd4267 /wd4305")
    # Enable object level parallel builds in Visual Studio.
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} /MP")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /MP")
    if("${CMAKE_BUILD_TYPE}" STREQUAL "Debug" OR "${CMAKE_BUILD_TYPE}" STREQUAL "RelWithDebInfo")
        set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} /bigobj")
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /bigobj")
    endif()
endif()

if(IS_GNU)
    if(CMAKE_CXX_COMPILER_VERSION VERSION_LESS 4.9)
        message(FATAL_ERROR "GCC version 4.8 or older not supported")
    endif()

    # Hide incorrect warnings for uninitialized Eigen variables under GCC.
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wno-maybe-uninitialized")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-maybe-uninitialized")
endif()

if(IS_DEBUG)
    add_definitions("-DEIGEN_INITIALIZE_MATRICES_BY_NAN")
endif()

if(SIMD_ENABLED)
    message(STATUS "Enabling SIMD support")
else()
    message(STATUS "Disabling SIMD support")
endif()

if(IPO_ENABLED AND NOT IS_DEBUG AND NOT IS_GNU)
    message(STATUS "Enabling interprocedural optimization")
    set_property(DIRECTORY PROPERTY INTERPROCEDURAL_OPTIMIZATION 1)
else()
    message(STATUS "Disabling interprocedural optimization")
endif()

if(ASAN_ENABLED)
    message(STATUS "Enabling ASan support")
    if(IS_CLANG OR IS_GNU)
        add_compile_options(-fsanitize=address -fno-omit-frame-pointer -fsanitize-address-use-after-scope)
        add_link_options(-fsanitize=address)
    else()
        message(FATAL_ERROR "Unsupported compiler for ASan mode")
    endif()
endif()

if(CCACHE_ENABLED)
    find_program(CCACHE ccache)
    if(CCACHE)
        message(STATUS "Enabling ccache support")
        set(CMAKE_C_COMPILER_LAUNCHER ${CCACHE})
        set(CMAKE_CXX_COMPILER_LAUNCHER ${CCACHE})
    else()
        message(STATUS "Disabling ccache support")
    endif()
else()
    message(STATUS "Disabling ccache support")
endif()

if(PROFILING_ENABLED)
    message(STATUS "Enabling profiling support")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -lprofiler -ltcmalloc")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -lprofiler -ltcmalloc")
else()
    message(STATUS "Disabling profiling support")
endif()

################################################################################
# Add sources
################################################################################

# Generate source file with version definitions.
include(GenerateVersionDefinitions)

include_directories(src)
link_directories(${COLMAP_LINK_DIRS})

#colmap和third_party的顺序原本是颠倒的，
add_subdirectory(src/thirdparty)
add_subdirectory(src/colmap)

################################################################################
# Generate source groups for Visual Studio, XCode, etc.
################################################################################

COLMAP_ADD_SOURCE_DIR(src/colmap/controllers CONTROLLERS_SRCS *.h *.cc)
COLMAP_ADD_SOURCE_DIR(src/colmap/estimators ESTIMATORS_SRCS *.h *.cc)
COLMAP_ADD_SOURCE_DIR(src/colmap/exe EXE_SRCS *.h *.cc)
COLMAP_ADD_SOURCE_DIR(src/colmap/feature FEATURE_SRCS *.h *.cc)
COLMAP_ADD_SOURCE_DIR(src/colmap/geometry GEOMETRY_SRCS *.h *.cc)
COLMAP_ADD_SOURCE_DIR(src/colmap/image IMAGE_SRCS *.h *.cc)
COLMAP_ADD_SOURCE_DIR(src/colmap/math MATH_SRCS *.h *.cc)
COLMAP_ADD_SOURCE_DIR(src/colmap/mvs MVS_SRCS *.h *.cc *.cu)
COLMAP_ADD_SOURCE_DIR(src/colmap/optim OPTIM_SRCS *.h *.cc)
COLMAP_ADD_SOURCE_DIR(src/colmap/retrieval RETRIEVAL_SRCS *.h *.cc)
COLMAP_ADD_SOURCE_DIR(src/colmap/scene SCENE_SRCS *.h *.cc)
COLMAP_ADD_SOURCE_DIR(src/colmap/sensor SENSOR_SRCS *.h *.cc)
COLMAP_ADD_SOURCE_DIR(src/colmap/sfm SFM_SRCS *.h *.cc)
COLMAP_ADD_SOURCE_DIR(src/colmap/tools TOOLS_SRCS *.h *.cc)
COLMAP_ADD_SOURCE_DIR(src/colmap/ui UI_SRCS *.h *.cc)
COLMAP_ADD_SOURCE_DIR(src/colmap/util UTIL_SRCS *.h *.cc)

if(LSD_ENABLED)
    COLMAP_ADD_SOURCE_DIR(src/thirdparty/LSD THIRDPARTY_LSD_SRCS *.h *.c)
endif()
COLMAP_ADD_SOURCE_DIR(src/thirdparty/PoissonRecon THIRDPARTY_POISSON_RECON_SRCS *.h *.cpp *.inl)
COLMAP_ADD_SOURCE_DIR(src/thirdparty/SiftGPU THIRDPARTY_SIFT_GPU_SRCS *.h *.cpp *.cu)
COLMAP_ADD_SOURCE_DIR(src/thirdparty/VLFeat THIRDPARTY_VLFEAT_SRCS *.h *.c *.tc)

# Add all of the source files to a regular library target, as using a custom
# target does not allow us to set its C++ include directories (and thus
# intellisense can't find any of the included files).
set(ALL_SRCS
    ${CONTROLLERS_SRCS}
    ${ESTIMATORS_SRCS}
    ${EXE_SRCS}
    ${FEATURE_SRCS}
    ${GEOMETRY_SRCS}
    ${IMAGE_SRCS}
    ${MATH_SRCS}
    ${MVS_SRCS}
    ${OPTIM_SRCS}
    ${RETRIEVAL_SRCS}
    ${SCENE_SRCS}
    ${SENSOR_SRCS}
    ${SFM_SRCS}
    ${TOOLS_SRCS}
    ${UI_SRCS}
    ${UTIL_SRCS}
    ${THIRDPARTY_POISSON_RECON_SRCS}
    ${THIRDPARTY_SIFT_GPU_SRCS}
    ${THIRDPARTY_VLFEAT_SRCS}
)

if(LSD_ENABLED)
    list(APPEND ALL_SRCS
        ${THIRDPARTY_LSD_SRCS}
    )
endif()

add_library(
    ${COLMAP_SRC_ROOT_FOLDER}
    ${ALL_SRCS}
)

# Prevent the library from being compiled automatically.
set_target_properties(
    ${COLMAP_SRC_ROOT_FOLDER} PROPERTIES
    EXCLUDE_FROM_ALL 1
    EXCLUDE_FROM_DEFAULT_BUILD 1)


################################################################################
# Install and uninstall scripts
################################################################################

# Install batch scripts under Windows.
if(IS_MSVC)
    install(FILES "scripts/shell/COLMAP.bat" "scripts/shell/RUN_TESTS.bat"
            PERMISSIONS OWNER_READ OWNER_WRITE OWNER_EXECUTE
                        GROUP_READ GROUP_EXECUTE WORLD_READ WORLD_EXECUTE
            DESTINATION "/")
endif()

# Install application meny entry under Linux/Unix.
if(UNIX AND NOT APPLE)
    install(FILES "doc/COLMAP.desktop" DESTINATION "share/applications")
endif()

# Configure the uninstallation script.
configure_file("${CMAKE_CURRENT_SOURCE_DIR}/cmake/CMakeUninstall.cmake.in"
               "${CMAKE_CURRENT_BINARY_DIR}/CMakeUninstall.cmake"
               IMMEDIATE @ONLY)
add_custom_target(uninstall COMMAND ${CMAKE_COMMAND} -P ${CMAKE_CURRENT_BINARY_DIR}/CMakeUninstall.cmake)
set_target_properties(uninstall PROPERTIES FOLDER ${CMAKE_TARGETS_ROOT_FOLDER})

set(COLMAP_EXPORT_LIBS
    # Internal.
    colmap_controllers
    colmap_estimators
    colmap_exe
    colmap_feature_types
    colmap_feature
    colmap_geometry
    colmap_image
    colmap_math
    colmap_mvs
    colmap_optim
    colmap_retrieval
    colmap_scene
    colmap_sensor
    colmap_sfm
    colmap_util
    # Third-party.
    colmap_poisson_recon
    colmap_vlfeat
)
if(LSD_ENABLED)
    list(APPEND COLMAP_EXPORT_LIBS
         # Third-party.
         colmap_lsd
    )
endif()
if(GUI_ENABLED)
    list(APPEND COLMAP_EXPORT_LIBS
         colmap_ui
    )
endif()
if(CUDA_ENABLED)
    list(APPEND COLMAP_EXPORT_LIBS
         colmap_util_cuda
         colmap_mvs_cuda
    )
endif()
if(GPU_ENABLED)
    list(APPEND COLMAP_EXPORT_LIBS
         colmap_sift_gpu
    )
endif()

# Add unified interface library target to export.
add_library(colmap INTERFACE)
target_link_libraries(colmap INTERFACE ${COLMAP_EXPORT_LIBS})
set(INSTALL_INCLUDE_DIR "${CMAKE_INSTALL_PREFIX}/include")
target_include_directories(
    colmap INTERFACE $<INSTALL_INTERFACE:${INSTALL_INCLUDE_DIR}>)

install(
    TARGETS colmap ${COLMAP_EXPORT_LIBS}
    EXPORT colmap-targets
    LIBRARY DESTINATION thirdparty/)

# Generate config and version.
include(CMakePackageConfigHelpers)
set(PACKAGE_CONFIG_FILE "${CMAKE_CURRENT_BINARY_DIR}/colmap-config.cmake")
set(INSTALL_CONFIG_DIR "share/colmap")
configure_package_config_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/cmake/colmap-config.cmake.in ${PACKAGE_CONFIG_FILE}
    INSTALL_DESTINATION ${INSTALL_CONFIG_DIR})
install(FILES ${PACKAGE_CONFIG_FILE} DESTINATION ${INSTALL_CONFIG_DIR})

configure_file("${CMAKE_CURRENT_SOURCE_DIR}/cmake/colmap-config-version.cmake.in"
                "${CMAKE_CURRENT_BINARY_DIR}/colmap-config-version.cmake" @ONLY)
install(FILES "${CMAKE_CURRENT_BINARY_DIR}/colmap-config-version.cmake"
        DESTINATION "share/colmap")

# Install targets.
install(
    EXPORT colmap-targets
    FILE colmap-targets.cmake
    NAMESPACE colmap::
    DESTINATION ${INSTALL_CONFIG_DIR})

# Install header files.
install(
    DIRECTORY src/colmap
    DESTINATION include
    FILES_MATCHING PATTERN "*.h")
install(
    DIRECTORY src/thirdparty
    DESTINATION include/colmap
    FILES_MATCHING REGEX ".*[.]h|.*[.]hpp|.*[.]inl")

# Install find_package scripts for dependencies.
install(
    DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/cmake
    DESTINATION share/colmap
    FILES_MATCHING PATTERN "Find*.cmake")
