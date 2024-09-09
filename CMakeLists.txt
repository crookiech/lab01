cmake_minimum_required(VERSION 3.10) # Устанавливает минимальную версию CMake, необходимую для этого проекта (в данном случае 3.10).
set(PATCH_VERSION "1" CACHE INTERNAL "Patch version")
set(PROJECT_VESRION 0.0.${PATCH_VERSION})
project(helloworld VERSION ${PROJECT_VESRION}) # Определяет имя проекта helloworld и использует переменную PROJECT_VESRION для указания версии проекта.
configure_file(version.h.in version.h) # Создает файл version.h из шаблона version.h.in, заменяя в нем специальные переменные, например, имя проекта и версию.
add_executable(helloworld_cli main.cpp)
add_library(helloworld lib.cpp)
# set_target_properties(helloworld_cli helloworld test_version PROPERTIES
#     CXX_STANDARD 14
#     CXX_STANDARD_REQUIRED ON
# )
target_include_directories(helloworld
    PRIVATE "${CMAKE_BINARY_DIR}"
)
target_link_libraries(helloworld_cli PRIVATE
    helloworld
)
if (MSVC) # Устанавливает флаги компиляции в зависимости от компилятора
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
    # target_compile_options(test_version PRIVATE
    #     -Wall -Wextra -pedantic -Werror
    # )
endif()
install(TARGETS helloworld_cli RUNTIME DESTINATION bin) # Устанавливает исполняемые файлы в директорию bin после успешной сборки.
set(CPACK_GENERATOR DEB) # Настраивает CPack для создания пакетов DEB (Linux).
set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")
set(CPACK_PACKAGE_CONTACT example@example.com)
include(CPack)
enable_testing() # Включает тестирование
add_test(test_version test_version) # Добавляет тест test_version
