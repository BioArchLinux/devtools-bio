project(devtools-bio)

## Setup
cmake_minimum_required(VERSION 3.0)
set(CMAKE_MODULE_PATH "${CMAKE_MODULE_PATH}"
  "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

include(GNUInstallDirs)

function(install_build_script cmd)
  add_custom_command(OUTPUT "${CMAKE_CURRENT_BINARY_DIR}/cmds/${cmd}"
    COMMAND "${CMAKE_COMMAND}" -E make_directory
    "${CMAKE_CURRENT_BINARY_DIR}/cmds"
    COMMAND "${CMAKE_COMMAND}" -E create_symlink archbuild
    "${CMAKE_CURRENT_BINARY_DIR}/cmds/${cmd}")
  add_custom_target(${cmd}.target ALL
    DEPENDS "${CMAKE_CURRENT_BINARY_DIR}/cmds/${cmd}")
endfunction()

install_build_script(bioarchlinux-i686-build)
install_build_script(bioarchlinux-x86_64-build)
install_build_script(bioarchlinux-testing-x86_64-build)
install_build_script(bioarchlinux-staging-x86_64-build)
install_build_script(bioarchlinux-kde-unstable-x86_64-build)
install_build_script(multilib-bioarchlinux-build)

foreach(CARCH arm armv6h armv7h aarch64)
  install_build_script("bioarchlinux-${CARCH}-build")
  configure_file(pacman-bioarchlinux-arm.conf.in
    "pacman-bioarchlinux-${CARCH}.conf" @ONLY)
  install(FILES
    "${CMAKE_CURRENT_BINARY_DIR}/pacman-bioarchlinux-${CARCH}.conf"
    DESTINATION "${CMAKE_INSTALL_DATAROOTDIR}/devtools/")
endforeach()

install(DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/cmds/"
  DESTINATION "${CMAKE_INSTALL_BINDIR}")
install(FILES
  "pacman-bioarchlinux.conf"
  "pacman-bioarchlinux-testing.conf"
  "pacman-bioarchlinux-staging.conf"
  "pacman-bioarchlinux-kde-unstable.conf"
  "pacman-multilib-bioarchlinux.conf"
  DESTINATION "${CMAKE_INSTALL_DATAROOTDIR}/devtools/")
