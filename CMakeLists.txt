# Copyright (c) 2018, Ruslan Baratov
# All rights reserved.

cmake_minimum_required(VERSION 3.3)

include("cmake/HunterGate.cmake")
HunterGate(
    URL "https://github.com/ruslo/hunter/archive/v0.20.40.tar.gz"
    SHA1 "a500979296b4b6b1542a2881151ceb9c984f3e36"
)

project(android_google_apis_packer)

hunter_add_package(android_sdk_packer)

# android_sdk_packer_unified_install
# android_sdk_packer_parse_version_revision
find_package(android_sdk_packer CONFIG REQUIRED)

hunter_add_package(Android-Google-APIs)

android_sdk_packer_parse_version_revision(
    FULL "${HUNTER_Android-Google-APIs_VERSION}"
    VERSION version
    REVISION revision
)

set(props "${CMAKE_CURRENT_BINARY_DIR}/gen/Android-Google-APIs/source.properties")

# Used variables:
# * version
# * revision
configure_file("Android-Google-APIs.source.properties" "${props}" @ONLY)

file(GLOB x "${ANDROID-GOOGLE-APIS_ROOT}/*")

android_sdk_packer_unified_install(
    OBJECTS ${x} ${props}
    DESTINATION "add-ons/addon-google_apis-google-${version}"
)
