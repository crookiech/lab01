cmake_minimum_required(VERSION 3.10)
project(MyProject)

# Set compiler flags
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -g")

# Include directories for the project's own headers
include_directories(include)

# Define the source files
set(SOURCE_FILES 
  main.cpp
  lib.cpp
)

# Add the executable target
add_executable(MyProject ${SOURCE_FILES})

# Define the version string
execute_process(COMMAND git describe --tags --always OUTPUT_VARIABLE GIT_VERSION)
string(REPLACE "-" "_" GIT_VERSION ${GIT_VERSION})
file(WRITE version.h.in "#define PROJECT_VERSION \"${GIT_VERSION}\"")

# Generate version.h from version.h.in
configure_file(version.h.in version.h)

# Link with necessary libraries
# This will depend on the specific libraries your project requires
# For example:
# target_link_libraries(MyProject ${CMAKE_DL_LIBS})
# target_link_libraries(MyProject ${CMAKE_THREAD_LIBS})

# Set output directory for binaries
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY "bin")

# Install target for the executable
install(TARGETS MyProject RUNTIME DESTINATION bin)


# cmake_minimum_required(VERSION 3.10)
# set(PATCH_VERSION "1" CACHE INTERNAL "Patch version")
# set(PROJECT_VESRION 0.0.${PATCH_VERSION})
# project(helloworld VERSION ${PROJECT_VESRION})
# configure_file(version.h.in version.h)
# add_executable(helloworld_cli main.cpp)
# add_library(helloworld lib.cpp)
# target_include_directories(helloworld
#     PRIVATE "${CMAKE_BINARY_DIR}"
# )
# target_link_libraries(helloworld_cli PRIVATE
#     helloworld
# )
# if (MSVC)
#     target_compile_options(helloworld_cli PRIVATE
#         /W4
#     )
#     target_compile_options(helloworld PRIVATE
#         /W4
#     )
# else ()
#     target_compile_options(helloworld_cli PRIVATE
#         -Wall -Wextra -pedantic -Werror
#     )
#     target_compile_options(helloworld PRIVATE
#         -Wall -Wextra -pedantic -Werror
#     )
# endif()
# install(TARGETS helloworld_cli RUNTIME DESTINATION bin)
# if(MSVC)
#     set(CPACK_GENERATOR ZIP)
#     set(CPACK_PACKAGE_NAME helloworld)
#     set(CPACK_PACKAGE_VERSION "${PROJECT_VESRION}")
#     set(CPACK_PACKAGE_VENDOR "Example")
#     set(CPACK_PACKAGE_DESCRIPTION "Hello World example")
#     set(CPACK_PACKAGE_FILE_NAME "helloworld-0.0.${{ github.run_number }}-Windows")
#     include(CPack)
# else()
#     set(CPACK_GENERATOR DEB)
#     set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
#     set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
#     set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")
#     set(CPACK_PACKAGE_CONTACT example@example.com)
#     include(CPack)
# endif()
# enable_testing()
