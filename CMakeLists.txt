cmake_minimum_required(VERSION 3.15)

# Indica dónde está el vcpkg toolchain
set(CMAKE_TOOLCHAIN_FILE
    "${CMAKE_SOURCE_DIR}/vcpkg/scripts/buildsystems/vcpkg.cmake"
    CACHE STRING "Vcpkg toolchain file"
)

project(proyectochat
    VERSION 1.0
    DESCRIPTION "Cliente C++ para preguntas con ChatGPT y MySQL"
    LANGUAGES CXX
)

# Usa al menos C++17
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# Encuentra las librerías instaladas con vcpkg
find_package(CURL           REQUIRED)
find_package(nlohmann_json  CONFIG REQUIRED)

# Aquí listamos todos los .cpp que formen tu ejecutable
# Ajusta si cambias nombres o rutas
add_executable(${PROJECT_NAME}
    main/main.cpp
    main/preguntarChatGPT.cpp
    main/conMySql.cpp
    main/contxt.cpp
 "main/sqlite3.c" "main/sqlite3.h" "main/bd_utilidades.cpp")

# Si tienes headers propios en main/, puedes añadir:
target_include_directories(${PROJECT_NAME}
    PRIVATE
      ${CMAKE_SOURCE_DIR}/main
)

# Enlaza con libcurl y nlohmann_json
target_link_libraries(${PROJECT_NAME}
    PRIVATE
      CURL::libcurl
      nlohmann_json::nlohmann_json
)

# Opcional: coloca el ejecutable en build/bin
set_target_properties(${PROJECT_NAME} PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin"
)
