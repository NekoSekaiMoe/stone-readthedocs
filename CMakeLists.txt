cmake_minimum_required(VERSION 3.10)

project(SphinxDocumentation)

# Set source and build directories
set(SPHINX_SOURCE ${CMAKE_SOURCE_DIR}/source)
set(SPHINX_BUILD ${CMAKE_SOURCE_DIR}/build)

# Find Sphinx
find_program(SPHINX_EXECUTABLE
  NAMES sphinx-build
  DOC "Sphinx documentation generator")

# Define custom target to generate documentation
add_custom_target(SphinxDocs ALL
  COMMAND ${SPHINX_EXECUTABLE} -b html ${SPHINX_SOURCE} ${SPHINX_BUILD}
  WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
  COMMENT "Generating documentation with Sphinx")

# Install dependencies
add_custom_target(install-dep
  COMMAND ${CMAKE_COMMAND} -E chdir ${CMAKE_SOURCE_DIR} pip install -r requirements.txt
  COMMENT "Installing Python Dependencies")

# Preview documentation
find_program(SPHINX_AUTOBUILD_EXECUTABLE
  NAMES sphinx-autobuild
  DOC "Sphinx auto-build for live previews")
if(SPHINX_AUTOBUILD_EXECUTABLE)
  add_custom_target(preview
    COMMAND ${SPHINX_AUTOBUILD_EXECUTABLE} ${SPHINX_SOURCE} ${SPHINX_BUILD}/html
    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}
    COMMENT "Live previewing with Sphinx")
endif()

# Clean build directory
add_custom_target(clean
  COMMAND ${CMAKE_COMMAND} -E remove_directory ${SPHINX_BUILD}
  COMMENT "Cleaning build directory")

