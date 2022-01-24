project(devtools-bio)

## Setup
cmake_minimum_required(VERSION 2.8)
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

install_build_script(BioArchLinux-i686-build)
install_build_script(BioArchLinux-x86_64-build)
install_build_script(BioArchLinux-testing-x86_64-build)
install_build_script(BioArchLinux-staging-x86_64-build)
install_build_script(BioArchLinux-kde-unstable-x86_64-build)
install_build_script(multilib-BioArchLinux-build)

install(DIRECTORY "${CMAKE_CURRENT_BINARY_DIR}/cmds/"
  DESTINATION "${CMAKE_INSTALL_BINDIR}")
install(FILES
  "pacman-BioArchLinux.conf"
  "pacman-BioArchLinux-testing.conf"
  "pacman-BioArchLinux-staging.conf"
  "pacman-BioArchLinux-kde-unstable.conf"
  "pacman-multilib-BioArchLinux.conf"
  DESTINATION "${CMAKE_INSTALL_DATAROOTDIR}/devtools/")
