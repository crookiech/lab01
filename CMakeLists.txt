cmake_minimum_required(VERSION 3.10)
set(PATCH_VERSION "1" CACHE INTERNAL "Patch version")
set(PROJECT_VESRION 0.0.${PATCH_VERSION})
project(helloworld VERSION ${PROJECT_VESRION})
configure_file(version.h.in version.h)
add_executable(helloworld_cli main.cpp)
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

# CPack
include(CPack)
set(CPACK_GENERATOR ZIP) # Используем ZIP для обоих платформ
set(CPACK_PACKAGE_VERSION_MAJOR 0)
set(CPACK_PACKAGE_VERSION_MINOR 0)
set(CPACK_PACKAGE_VERSION_PATCH ${PROJECT_VERSION_PATCH})
set(CPACK_PACKAGE_CONTACT example@example.com)
set(CPACK_SOURCE_PACKAGE_NAME helloworld)
set(CPACK_PACKAGE_FILE_NAME helloworld)
set(CPACK_PACKAGE_VENDOR "Your Company") # Замените на ваше имя компании
set(CPACK_PACKAGE_DESCRIPTION "Description of your software") # Замените на описание
