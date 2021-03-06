project(libconfig)

include(HiveDefaults)
include(ExternalProject)

set(CONFIGURE_CMD "./configure")
set(CONFIGURE_ARGS
    "--prefix=${HIVE_INT_DIST_DIR}"
    "--enable-static"
    "--disable-shared"
    "--disable-cxx"
    "--disable-examples")

set(BUILD_CMD "make")

set(INSTALL_CMD "make")
set(INSTALL_ARGS "install")

ExternalProject_Add(
    libconfig

    PREFIX ${HIVE_DEPS_BUILD_PREFIX}
    URL "https://hyperrealm.github.io/libconfig/dist/libconfig-1.7.2.tar.gz"
    URL_HASH SHA256=7c3c7a9c73ff3302084386e96f903eb62ce06953bb1666235fac74363a16fad9
    DOWNLOAD_NAME "libconfig-1.7.2.tar.gz"
    DOWNLOAD_DIR ${HIVE_DEPS_TARBALL_DIR}
    DOWNLOAD_NO_PROGRESS 1

    BUILD_IN_SOURCE 1

    PATCH_COMMAND ${PATCH_EXE} -s -p1 < ${CMAKE_CURRENT_LIST_DIR}/libconfig.patch

    CONFIGURE_COMMAND ${CONFIGURE_CMD} ${CONFIGURE_ARGS}
    BUILD_COMMAND ${BUILD_CMD} ${BUILD_ARGS}
    INSTALL_COMMAND ${INSTALL_CMD} ${INSTALL_ARGS}
)

add_library(lconfig INTERFACE)
target_include_directories(lconfig INTERFACE "${HIVE_INT_DIST_DIR}/include")
target_link_libraries(lconfig INTERFACE config)
