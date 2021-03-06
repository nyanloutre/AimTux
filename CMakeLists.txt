cmake_minimum_required(VERSION 2.8)
project(AimTux)

if("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
    if (CMAKE_CXX_COMPILER_VERSION VERSION_LESS 6.0)
        message(FATAL_ERROR "Your GCC seems outdated. Go to https://github.com/AimTuxOfficial/AimTux/wiki/Updating-your-compiler for further instructions. If you have done so and still see this message, running \'git clean -fdx\' and try again")
    endif()
else()
    message(FATAL_ERROR "You must have GCC >= 6.0 installed in order to compile AimTux. Go to https://github.com/AimTuxOfficial/AimTux/wiki/Updating-your-compiler for further instructions.")
endif()

FUNCTION (EXCLUDE_FILES_FROM_DIR_IN_LIST _InFileList _excludeDirName _verbose)
    foreach (ITR ${_InFileList})
        if ("${_verbose}")
            message(STATUS "ITR=${ITR}")
        endif ("${_verbose}")

        if ("${ITR}" MATCHES "(.*)${_excludeDirName}(.*)") # Check if the item matches the directory name in _excludeDirName
            message(STATUS "Remove Item from List:${ITR}")
            list (REMOVE_ITEM _InFileList ${ITR}) # Remove the item from the list
        endif ("${ITR}" MATCHES "(.*)${_excludeDirName}(.*)")

    endforeach(ITR)
    set(SOURCE_FILES ${_InFileList} PARENT_SCOPE) # Return the SOURCE_FILES variable to the calling parent
ENDFUNCTION (EXCLUDE_FILES_FROM_DIR_IN_LIST)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++14 -Wall -Wno-maybe-uninitialized -Wno-unused-result -O3")
set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -static-libstdc++")
set(CMAKE_BUILD_TYPE Debug)

file(GLOB_RECURSE SOURCE_FILES src/*.cpp)
EXCLUDE_FILES_FROM_DIR_IN_LIST("${SOURCE_FILES}" "src/ImGUI/examples/" FALSE)
EXCLUDE_FILES_FROM_DIR_IN_LIST("${SOURCE_FILES}" "src/ImGUI/extra_fonts" FALSE)

add_library(AimTux SHARED ${SOURCE_FILES})
target_link_libraries(AimTux SDL2 GL dl)
