cmake_minimum_required(VERSION 3.10)
set(PATCH_VERSION "1" CACHE INTERNAL "Patch version")
set(PROJECT_VESRION 0.0.${PATCH_VERSION})
project(helloworld VERSION ${PROJECT_VESRION})
find_package(Boost COMPONENTS unit_test_framework REQUIRED)
configure_file(version.h.in version.h)

add_executable(helloworld main.cpp)
add_library(helloworld_lib lib.cpp) 
add_executable(test_version test_version.cpp)

set_target_properties(helloworld helloworld test_version PROPERTIES
    CXX_STANDARD 14
    CXX_STANDARD_REQUIRED ON
)

target_include_directories(helloworld
    PRIVATE "${CMAKE_BINARY_DIR}"
)

set_target_properties(test_version PROPERTIES
    COMPILE_DEFINITIONS BOOST_TEST_DYN_LINK
    INCLUDE_DIRECTORIES ${Boost_INCLUDE_DIR}
)

# **Убедитесь, что target_link_libraries находится после add_executable**
add_executable(helloworld_cli main.cpp)
target_link_libraries(helloworld_cli PRIVATE
    helloworld_lib  
)

target_link_libraries(test_version
    ${Boost_LIBRARIES}
    helloworld_lib
)

if (MSVC)
    target_compile_options(helloworld_cli PRIVATE
        /W4
    )
    target_compile_options(helloworld PRIVATE
        /W4
    )
    target_compile_options(test_version PRIVATE
        /W4
    )
else ()
    target_compile_options(helloworld_cli PRIVATE
        -Wall -Wextra -pedantic -Werror
    )
    target_compile_options(helloworld PRIVATE
        -Wall -Wextra -pedantic -Werror
    )
    target_compile_options(test_version PRIVATE
        -Wall -Wextra -pedantic -Werror
    )
endif()

install(TARGETS helloworld_cli RUNTIME DESTINATION bin)

# Упаковка в ZIP-архив для macOS
if (APPLE)
    set(CPACK_GENERATOR ZIP)
endif()

set(CPACK_PACKAGE_NAME "helloworld")
set(CPACK_PACKAGE_VERSION ${PROJECT_VESRION})

# Создание пакета
add_custom_command(TARGET helloworld POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy  
    "${CMAKE_BINARY_DIR}/helloworld" 
    "${CMAKE_BINARY_DIR}/helloworld-${PROJECT_VESRION}-Windows.exe"
)

# CPack
include(CPack)
enable_testing()
add_test(test_version test_version)

# cmake_minimum_required(VERSION 3.10)
# set(PATCH_VERSION "1" CACHE INTERNAL "Patch version")
# set(PROJECT_VESRION 0.0.${PATCH_VERSION})
# project(helloworld VERSION ${PROJECT_VESRION})
# find_package(Boost COMPONENTS unit_test_framework REQUIRED)
# configure_file(version.h.in version.h)

# # Изменяем имя исполняемого файла
# add_executable(helloworld main.cpp)
# add_library(helloworld lib.cpp)
# add_executable(test_version test_version.cpp)

# set_target_properties(helloworld helloworld test_version PROPERTIES
#     CXX_STANDARD 14
#     CXX_STANDARD_REQUIRED ON
# )

# target_include_directories(helloworld
#     PRIVATE "${CMAKE_BINARY_DIR}"
# )

# set_target_properties(test_version PROPERTIES
#     COMPILE_DEFINITIONS BOOST_TEST_DYN_LINK
#     INCLUDE_DIRECTORIES ${Boost_INCLUDE_DIR}
# )

# target_link_libraries(helloworld_cli PRIVATE
#     helloworld
# )
# target_link_libraries(test_version
#     ${Boost_LIBRARIES}
#     helloworld
# )

# if (MSVC)
#     target_compile_options(helloworld_cli PRIVATE
#         /W4
#     )
#     target_compile_options(helloworld PRIVATE
#         /W4
#     )
#     target_compile_options(test_version PRIVATE
#         /W4
#     )
# else ()
#     target_compile_options(helloworld_cli PRIVATE
#         -Wall -Wextra -pedantic -Werror
#     )
#     target_compile_options(helloworld PRIVATE
#         -Wall -Wextra -pedantic -Werror
#     )
#     target_compile_options(test_version PRIVATE
#         -Wall -Wextra -pedantic -Werror
#     )
# endif()

# install(TARGETS helloworld_cli RUNTIME DESTINATION bin)

# # Упаковка в ZIP-архив для macOS
# if (APPLE)
#     set(CPACK_GENERATOR ZIP)
# endif()

# set(CPACK_PACKAGE_NAME "helloworld")
# set(CPACK_PACKAGE_VERSION ${PROJECT_VESRION})

# # Создание пакета
# add_custom_command(TARGET helloworld POST_BUILD
#     COMMAND ${CMAKE_COMMAND} -E copy  
#     "${CMAKE_BINARY_DIR}/helloworld" 
#     "${CMAKE_BINARY_DIR}/helloworld-${PROJECT_VESRION}-Windows.exe"
# )

# # CPack
# include(CPack)
# enable_testing()
# add_test(test_version test_version)
