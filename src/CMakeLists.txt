# Note that headers are optional, and do not affect add_library, but they will not
# show up in IDEs unless they are listed in add_library.

# Optionally glob, but only for CMake 3.12 or later:
file(GLOB HEADER_LIST CONFIGURE_DEPENDS
        "${LRCalc_SOURCE_DIR}/include/lrcalc/*.h"
        "${LRCalc_SOURCE_DIR}/include/lrcalc/*.hpp")

file(GLOB SOURCE_LIST CONFIGURE_DEPENDS
        "${LRCalc_SOURCE_DIR}/src/*.h"
        "${LRCalc_SOURCE_DIR}/src/*.hpp"
        "${LRCalc_SOURCE_DIR}/src/*.cc"
        "${LRCalc_SOURCE_DIR}/src/*.cpp"
        "${LRCalc_SOURCE_DIR}/src/*.c")

# Make an automatic library - will be static or dynamic based on user setting
add_library(lrcalc ${SOURCE_LIST} ${HEADER_LIST})

# We need this directory, and users of our library will need it too
target_include_directories(lrcalc PUBLIC ../include)

# All users of this library will need at least C++11
target_compile_features(lrcalc PUBLIC cxx_std_11)

# Cross-platform compiler lints
if (${CMAKE_CXX_COMPILER_ID} STREQUAL "Clang"
        OR ${CMAKE_CXX_COMPILER_ID} STREQUAL "GNU")
    target_compile_options(lrcalc PRIVATE
            -Wall
            -Wextra
            -Wswitch
            -Wconversion
            -Wparentheses
            -Wfloat-equal
            -Wzero-as-null-pointer-constant
            -Wpedantic
            -pedantic
            -pedantic-errors)
elseif (${CMAKE_CXX_COMPILER_ID} STREQUAL "MSVC")
    target_compile_options(lrcalc PRIVATE
            /W3)
endif ()

# IDEs should put the headers in a nice place
source_group(TREE "${PROJECT_SOURCE_DIR}/include" PREFIX "Header Files" FILES ${HEADER_LIST})
