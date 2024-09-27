# raylib_nuklear shared library

include(FetchContent)
FetchContent_Declare(
    raylib
    GIT_REPOSITORY https://github.com/raysan5/raylib.git
    GIT_TAG 5.0
    GIT_SHALLOW 1
)
FetchContent_GetProperties(raylib)
if (NOT raylib_POPULATED) # Have we downloaded raylib yet?
    set(FETCHCONTENT_QUIET NO)
    FetchContent_Populate(raylib)
    set(BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
    set(BUILD_GAMES    OFF CACHE BOOL "" FORCE)
    set(BUILD_SHARED_LIBS ON CACHE BOOL "" FORCE)
    add_subdirectory(${raylib_SOURCE_DIR} ${raylib_BINARY_DIR})
endif()

add_library(raylib_nuklear_shared SHARED raylib-nuklear.c)
target_include_directories(raylib_nuklear_shared INTERFACE raylib_nuklear)
target_include_directories(raylib_nuklear_shared INTERFACE raylib)

message("Raylib shared dir: " ${raylib_BINARY_DIR})
if(CMAKE_CONFIGURATION_TYPES)
    foreach(config ${CMAKE_CONFIGURATION_TYPES})
        string(TOUPPER ${config} CONFIG_UPPER)
        if(${CONFIG_UPPER} STREQUAL "DEBUG")
            target_link_directories(raylib_nuklear_shared PUBLIC ${raylib_BINARY_DIR}"/raylib/Debug")
        elseif(${CONFIG_UPPER} STREQUAL "RELEASE")
            target_link_directories(raylib_nuklear_shared PRIVATE ${raylib_BINARY_DIR}"/raylib/Release/")
        endif()
    endforeach()
else()
    if(CMAKE_BUILD_TYPE STREQUAL "Release")
        target_link_directories(raylib_nuklear_shared PRIVATE ${raylib_BINARY_DIR}"/raylib/Release/")
    else()
        target_link_directories(raylib_nuklear_shared PUBLIC ${raylib_BINARY_DIR}"/raylib/Debug")
endif()
endif()
if (MSVC)
    target_link_libraries(raylib_nuklear_shared raylib raylib.lib)
else()
    target_link_libraries(raylib_nuklear_shared raylib)
endif()
