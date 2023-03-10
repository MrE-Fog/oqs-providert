cmake_minimum_required(VERSION 3.0 FATAL_ERROR)
project(oqs-provider LANGUAGES C)
set(OQSPROVIDER_VERSION_TEXT "0.5.0-dev")
set(CMAKE_C_STANDARD 11)
set_property(GLOBAL PROPERTY FIND_LIBRARY_USE_LIB64_PATHS ON)
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    message(STATUS "Creating Debug build with OQS-Debug env vars enabled")
else()
    message(STATUS "Creating Release build")
    add_definitions( -DNDEBUG )
endif()

option(NOPUBKEY_IN_PRIVKEY "Do not include public keys in private key structures/PKCS#8 " OFF)
if(${NOPUBKEY_IN_PRIVKEY})
    message(STATUS "Build will not store public keys alongside private keys in PKCS#8 structures")
    add_compile_definitions( NOPUBKEY_IN_PRIVKEY )
else()
    message(STATUS "Build will store public keys in PKCS#8 structures")
endif()

include(CheckLibraryExists)
include(CheckFunctionExists)

find_package(OpenSSL 3.0 REQUIRED)
include_directories(${OPENSSL_INCLUDE_DIR})
find_package(liboqs REQUIRED)

# Provider module
add_subdirectory(oqsprov)

# Testing
enable_testing()
add_subdirectory(test)
