cmake_minimum_required(VERSION 3.10)
set(PATCH_VERSION "1" CACHE INTERNAL "Patch version")
set(PROJECT_VESRION 0.0.${PATCH_VERSION})
project(helloworld VERSION ${PROJECT_VESRION})
configure_file(version.h.in version.h)
add_executable(helloworld main.cpp)
add_library(helloworld lib.cpp)
target_include_directories(helloworld
    PRIVATE "${CMAKE_BINARY_DIR}"
)
target_link_libraries(helloworld_cli PRIVATE
    helloworld
)
if (MSVC)
    target_compile_options(helloworld_cli PRIVATE
        /W4
    )
    target_compile_options(helloworld PRIVATE
        /W4
    )
else ()
    target_compile_options(helloworld_cli PRIVATE
        -Wall -Wextra -pedantic -Werror
    )
    target_compile_options(helloworld PRIVATE
        -Wall -Wextra -pedantic -Werror
    )
endif()
install(TARGETS helloworld_cli RUNTIME DESTINATION bin)

# Упаковка в ZIP-архив
set(CPACK_GENERATOR ZIP)
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
