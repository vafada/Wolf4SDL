cmake_minimum_required(VERSION 3.10)

project(Wolf4sdl LANGUAGES C)

file(GLOB SOURCES CONFIGURE_DEPENDS "*.c")

# Find dependencies
find_package(SDL2 REQUIRED)
find_package(SDL2_mixer REQUIRED)

add_executable(Wolf4sdl ${SOURCES})

# Include directories
target_include_directories(Wolf4sdl PRIVATE
        ${CMAKE_CURRENT_SOURCE_DIR}
        ${SDL2_INCLUDE_DIRS}
        ${SDL2_MIXER_INCLUDE_DIRS}
)

# Link libraries
target_link_libraries(Wolf4sdl PRIVATE
        SDL2
        SDL2_mixer
)

# Link math library on Unix/Linux (needed for math.h functions)
if (UNIX AND NOT APPLE)
    target_link_libraries(Wolf4sdl PRIVATE m)
endif ()

# Compiler flags matching Makefile CFLAGS
if (CMAKE_CXX_COMPILER_ID MATCHES "GNU|Clang")
    target_compile_options(Wolf4sdl PRIVATE
            -Wpointer-arith
            -Wreturn-type
            -Wwrite-strings
            -Wcast-align
            -Wno-write-strings # Typically needed for legacy code handling string literals
    )
endif ()

# Basic install rule
install(TARGETS Wolf4sdl RUNTIME DESTINATION bin)

file(COPY "data/" DESTINATION ${CMAKE_BINARY_DIR})