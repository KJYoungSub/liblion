cmake_minimum_required(VERSION 3.15.0 FATAL_ERROR)

#set(CMAKE_SYSTEM_VERSION 10.0.17763.0 CACHE STRING "" FORCE)

project(liblion CXX)

################################################################################
# Set target arch type if empty. Visual studio solution generator provides it.
################################################################################
if(NOT CMAKE_VS_PLATFORM_NAME)
    set(CMAKE_VS_PLATFORM_NAME "x64")
endif()
message("${CMAKE_VS_PLATFORM_NAME} architecture in use")

if(NOT ("${CMAKE_VS_PLATFORM_NAME}" STREQUAL "x64"))
    message(FATAL_ERROR "${CMAKE_VS_PLATFORM_NAME} arch is not supported!")
endif()

################################################################################
# Global configuration types
################################################################################
set(CMAKE_CONFIGURATION_TYPES
    "Release"
    CACHE STRING "" FORCE
)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fPIC -mavx")

################################################################################
# Common utils
################################################################################
include(CMake/Utils.cmake)

################################################################################
# Additional Global Settings(add specific info there)
################################################################################
include(CMake/GlobalSettingsInclude.cmake OPTIONAL)

################################################################################
# Use solution folders feature
################################################################################
set_property(GLOBAL PROPERTY USE_FOLDERS ON)

################################################################################
# Sub-projects
################################################################################

set(PROJECT_NAME lion)

################################################################################
# Source groups
################################################################################
set(Header_Files
    "liblion.h"
    "src/avx_helper.h"
    "src/backprojector.h"
    "src/complex.h"
    "src/ctf.h"
    "src/error.h"
    "src/euler.h"
    "src/fftw.h"
    "src/filename.h"
    "src/funcs.h"
    "src/healpix_sampling.h"
    "src/image.h"
    "src/macros.h"
    "src/mask.h"
    "src/matrix1d.h"
    "src/matrix2d.h"
    "src/memory.h"
    "src/metadata_container.h"
    "src/metadata_label.h"
    "src/metadata_table.h"
    "src/multidim_array.h"
    "src/numerical_recipes.h"
    "src/projector.h"
    "src/rwMRC.h"
    "src/strings.h"
    "src/symmetries.h"
    "src/tabfuncs.h"
    "src/transformations.h"
)
source_group("Header Files" FILES ${Header_Files})

set(Healpix
    "src/Healpix_2.15a/arr.h"
    "src/Healpix_2.15a/cxxutils.cc"
    "src/Healpix_2.15a/cxxutils.h"
    "src/Healpix_2.15a/datatypes.h"
    "src/Healpix_2.15a/geom_utils.h"
    "src/Healpix_2.15a/healpix_base.cc"
    "src/Healpix_2.15a/healpix_base.h"
    "src/Healpix_2.15a/lsconstants.h"
    "src/Healpix_2.15a/message_error.h"
    "src/Healpix_2.15a/openmp_support.h"
    "src/Healpix_2.15a/pointing.h"
    "src/Healpix_2.15a/vec3.h"
)
source_group("Healpix" FILES ${Healpix})

set(Source_Files
    "src/backprojector.cpp"
    "src/complex.cpp"
    "src/ctf.cpp"
    "src/error.cpp"
    "src/euler.cpp"
    "src/fftw.cpp"
    "src/filename.cpp"
    "src/funcs.cpp"
    "src/healpix_sampling.cpp"
    "src/image.cpp"
    "src/mask.cpp"
    "src/matrix1d.cpp"
    "src/matrix2d.cpp"
    "src/memory.cpp"
    "src/metadata_container.cpp"
    "src/metadata_label.cpp"
    "src/metadata_table.cpp"
    "src/multidim_array.cpp"
    "src/numerical_recipes.cpp"
    "src/projector.cpp"
    "src/strings.cpp"
    "src/symmetries.cpp"
    "src/tabfuncs.cpp"
    "src/transformations.cpp"
)
source_group("Source Files" FILES ${Source_Files})

set(ALL_FILES
    ${Header_Files}
    ${Healpix}
    ${Source_Files}
)


################################################################################
# Target
################################################################################
add_library(${PROJECT_NAME} STATIC ${ALL_FILES})

################################################################################
# Compile definitions
################################################################################
target_compile_definitions(${PROJECT_NAME} PRIVATE "FLOAT_PRECISION")

target_include_directories(${PROJECT_NAME} PRIVATE "${ROOT_SOURCE_DIR}")