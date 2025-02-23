cmake_minimum_required(VERSION 3.12)
project(cpp_object_attr)

find_package(catkin REQUIRED COMPONENTS
    pybind11_catkin)

catkin_python_setup()

catkin_package(
    LIBRARIES
    CATKIN_DEPENDS pybind11_catkin
)

pybind_add_module(cpp_object MODULE src/cpp_object.cpp)
pybind_add_module(example SHARED src/example.cpp)
add_executable(main src/main.cpp)
target_link_libraries(main example pybind11::embed)

install(TARGETS main
                cpp_object
  LIBRARY DESTINATION ${PYTHON_INSTALL_DIR}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
