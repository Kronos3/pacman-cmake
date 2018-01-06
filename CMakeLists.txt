cmake_minimum_required(VERSION 3.10)
project(pacman C)

set(CMAKE_C_STANDARD 99)

file(GLOB PACMAN_HEADERS "src/pacman/*.h")
file(GLOB PACMAN_SOURCES "src/pacman/*.c")


file(GLOB LIB_ALPM_HEADERS "lib/libalpm/*.h")
file(GLOB LIB_ALPM_SOURCES "lib/libalpm/*.c")

add_definitions (-DHAVE_LIBCURL)
add_definitions (-DSYSHOOKDIR=\"${CMAKE_SOURCE_DIR}/libalpm/hooks/\")
add_definitions (-DLIB_VERSION="10.0.2")
add_definitions (-DPACKAGE_VERSION="5.0.2")

add_definitions (-DFSSTATSTYPE="struct statvfs")
add_definitions (-DHAVE_STRNLEN)
add_definitions (-DSCRIPTLET_SHELL="/bin/sh")
add_definitions (-DLDCONFIG="/sbin/ldconfig")

set (LOCALSTATEDIR "/var")
set (SYSCONFDIR "/etc")

add_definitions (-D_XOPEN_SOURCE)
add_definitions (-DHAVE_LIBSSL)
add_definitions (-DHAVE_LIBGPGME)
add_definitions (-D_GNU_SOURCE)
add_definitions (-DCONFFILE="${SYSCONFDIR}/pacman.conf")
add_definitions (-DGPGDIR="${SYSCONFDIR}/pacman.d/gnupg/")
add_definitions (-DHOOKDIR="${SYSCONFDIR}/pacman.d/hooks/")
add_definitions (-DDBPATH="${LOCALSTATEDIR}/lib/pacman")
add_definitions (-DCACHEDIR="${LOCALSTATEDIR}/cache/pacman/pkg")
add_definitions (-DLOGFILE="${LOCALSTATEDIR}/log/pacman.log")
add_definitions (-DROOTDIR="/")
add_definitions (-DHAVE_GETMNTENT)
add_definitions (-DHAVE_MNTENT_H)

add_library(LIB_alpm SHARED ${LIB_ALPM_HEADERS} ${LIB_ALPM_SOURCES})
add_executable(pacman ${PACMAN_HEADERS} ${PACMAN_SOURCES})
target_link_libraries (pacman LIB_alpm archive curl ssl gpgme)