cmake_minimum_required (VERSION 3.2.0)
project (DiscordRPC)

include(GNUInstallDirs)

option(BUILD_EXAMPLES "Build example apps" ON)

# format
file(GLOB_RECURSE ALL_SOURCE_FILES
    examples/*.cpp examples/*.h examples/*.c
    include/*.h
    src/*.cpp src/*.h src/*.c
)

# Set CLANG_FORMAT_SUFFIX if you are using custom clang-format, e.g. clang-format-5.0
find_program(CLANG_FORMAT_CMD clang-format${CLANG_FORMAT_SUFFIX})

if (CLANG_FORMAT_CMD)
    add_custom_target(
        clangformat
        COMMAND ${CLANG_FORMAT_CMD}
        -i -style=file -fallback-style=none
        ${ALL_SOURCE_FILES}
        DEPENDS
        ${ALL_SOURCE_FILES}
    )
endif(CLANG_FORMAT_CMD)

# thirdparty stuff
include(FetchContent)

FetchContent_Declare(
    rapidjson
    URL      https://github.com/Tencent/rapidjson/archive/refs/heads/master.zip
)

FetchContent_MakeAvailable(rapidjson)

# add subdirs
add_subdirectory(src)
if (BUILD_EXAMPLES)
    add_subdirectory(examples/send-presence)
endif(BUILD_EXAMPLES)
