cmake_minimum_required(VERSION 3.2)
project(mojoshader VERSION 0.0.1)

set(TARGET_SRCS
  mojoshader.c
  mojoshader_assembler.c
  mojoshader_common.c
  mojoshader_compiler.c
  mojoshader_effects.c
  mojoshader_lexer.c
  mojoshader_preprocessor.c
  )

if(MSVC)
  add_definitions (-D_CRT_SECURE_NO_WARNINGS=1 -TP)
endif()

add_library(mojoshader "${TARGET_SRCS}")

#### install

set(config_install_dir "lib/cmake/${PROJECT_NAME}")
set(include_install_dir "include")

set(generated_dir "${CMAKE_CURRENT_BINARY_DIR}/generated")

# Configuration
set(version_config "${generated_dir}/${PROJECT_NAME}ConfigVersion.cmake")
set(project_config "${generated_dir}/${PROJECT_NAME}Config.cmake")
set(TARGETS_EXPORT_NAME "${PROJECT_NAME}Targets")
set(namespace "${PROJECT_NAME}::")

include(CMakePackageConfigHelpers)

write_basic_package_version_file(
  "${version_config}" COMPATIBILITY SameMajorVersion
  )

configure_package_config_file(
  "cmake/Config.cmake.in"
  "${project_config}"
  INSTALL_DESTINATION "${config_install_dir}"
  )

install(
  TARGETS mojoshader
  EXPORT "${TARGETS_EXPORT_NAME}"
  LIBRARY DESTINATION "lib"
  ARCHIVE DESTINATION "lib"
  RUNTIME DESTINATION "bin"
  INCLUDES DESTINATION "${include_install_dir}"
  )

install(
  FILES mojoshader.h
  DESTINATION "${include_install_dir}/${PROJECT_NAME}"
)

install(
  FILES "${project_config}" "${version_config}"
  DESTINATION "${config_install_dir}"
  )

install(
  EXPORT "${TARGETS_EXPORT_NAME}"
  NAMESPACE "${namespace}"
  DESTINATION "${config_install_dir}"
  )
