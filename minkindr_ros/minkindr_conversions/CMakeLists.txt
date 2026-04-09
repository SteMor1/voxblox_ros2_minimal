cmake_minimum_required(VERSION 3.5)
project(minkindr_conversions)

find_package(ament_cmake REQUIRED)
find_package(minkindr REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(glog REQUIRED)

add_library(${PROJECT_NAME} INTERFACE)
target_include_directories(${PROJECT_NAME} INTERFACE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>)
target_link_libraries(${PROJECT_NAME} INTERFACE
  Eigen3::Eigen
  glog::glog)

if(BUILD_TESTING)
  find_package(ament_cmake_gtest REQUIRED)
  ament_add_gtest(kindr_tf_test test/kindr_tf_test.cc)
  target_link_libraries(kindr_tf_test ${PROJECT_NAME})
  ament_add_gtest(kindr_msg_test test/kindr_msg_test.cc)
  target_link_libraries(kindr_msg_test ${PROJECT_NAME})
endif()

install(TARGETS ${PROJECT_NAME}
  EXPORT export_${PROJECT_NAME})
install(DIRECTORY include/ DESTINATION include)

ament_export_targets(export_${PROJECT_NAME})
ament_export_dependencies(minkindr Eigen3 glog)
ament_package()