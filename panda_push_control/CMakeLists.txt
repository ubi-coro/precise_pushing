cmake_minimum_required(VERSION 3.4)
project(panda_push_control)

find_package(catkin REQUIRED
    controller_interface
    dynamic_reconfigure
    pluginlib
    franka_example_controllers)

find_package(Eigen3 REQUIRED)
find_package(Franka REQUIRED)

generate_dynamic_reconfigure_options(
  cfg/Controller.cfg
)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES ${PROJECT_NAME}
  CATKIN_DEPENDS
    controller_interface
    franka_example_controllers
    dynamic_reconfigure
  DEPENDS Franka
)

add_library(${PROJECT_NAME} 
  src/joint_velocity_push_controller.cpp
)

target_link_libraries(${PROJECT_NAME} PUBLIC
  ${Franka_LIBRARIES}
  ${catkin_LIBRARIES}
)

target_include_directories(${PROJECT_NAME} SYSTEM PUBLIC
  ${Franka_INCLUDE_DIRS}
  ${EIGEN3_INCLUDE_DIRS}
  ${catkin_INCLUDE_DIRS}
)
target_include_directories(${PROJECT_NAME} PUBLIC
  include
)

add_dependencies(${PROJECT_NAME} ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

install(TARGETS ${PROJECT_NAME}
 ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
 LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
 RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY include/${PROJECT_NAME}/
 DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
)

install(FILES panda_push_control_plugin.xml
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)

install(DIRECTORY cfg
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)