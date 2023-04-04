cmake_minimum_required(VERSION 3.8)
project(lane_detection)

if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
 endif()
 
if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(ament_index_cpp REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(rosidl_default_generators REQUIRED)
find_package(scale_truck_msgs REQUIRED)
find_package(OpenCV REQUIRED)
find_package(usb_cam REQUIRED)
#find_package(lane_detection REQUIRED)
find_package(cv_bridge REQUIRED)

 
set(PROJECT_SOURCES_DEPENDENCIES
  rclcpp
  std_msgs
  lane_detection
  ament_index_cpp
  cv_bridge
  )


set(msg_files
  "msg/Lane.msg"
  )
  
 rosidl_generate_interfaces(${PROJECT_NAME}
 ${msg_files}
 DEPENDENCIES geometry_msgs
 )

include_directories(
  include
  ${OpenCV_INCLUDE_DIRS}
 )
 
 
 # Build
add_executable(lanedetection src/lane_detect.cpp)
ament_target_dependencies(lanedetection rclcpp std_msgs sensor_msgs cv_bridge)

#target_link_libraries(lanedetection
# ${OpenCV_LIBS}
# )

#  install(TARGETS
#  lanedetection
#  DESTINATION lib/${PROJECT_NAME}
#  )

install(TARGETS
  lanedetection
  DESTINATION lib/${PROJECT_NAME}
  )

install(DIRECTORY
  include/
  DESTINATION include
  )

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # uncomment the line when a copyright and license is not present in all source files
  #set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # uncomment the line when this package is not in a git repo
  #set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
