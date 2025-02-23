cmake_minimum_required(VERSION 3.12)
project(plugin)
add_compile_options(-Wno-attributes -std=c++11)

find_package(catkin REQUIRED COMPONENTS
    pybind11_catkin)

catkin_python_setup()

catkin_package(
    LIBRARIES
    CATKIN_DEPENDS pybind11_catkin
)

include_directories(${PROJECT_SOURCE_DIR}/include)

add_library(base_plugin SHARED src/base_plugin.cpp)
add_library(plugin_wrapper SHARED src/plugin_wrapper.cpp)
target_link_libraries(plugin_wrapper base_plugin pybind11::embed)

pybind_add_module(py_base_plugin MODULE src/py_base_plugin.cpp)

add_executable(plugin_runner src/plugin_runner.cpp)
target_link_libraries(plugin_runner base_plugin plugin_wrapper)

install(TARGETS plugin_runner
                base_plugin
                plugin_wrapper
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(TARGETS py_base_plugin
  LIBRARY DESTINATION ${PYTHON_INSTALL_DIR}
)
