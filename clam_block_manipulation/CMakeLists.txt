cmake_minimum_required(VERSION 2.8.3)
project(clam_block_manipulation)

find_package(Eigen REQUIRED)
find_package(Boost REQUIRED thread date_time system filesystem program_options python)

# Load catkin and all dependencies required for this package
find_package(catkin REQUIRED
  roscpp
  std_msgs
  actionlib 
  visualization_msgs 
  clam_controller 
  clam_msgs
  pcl_ros
  cv_bridge
  eigen_conversions 
  moveit_ros_planning 
  moveit_ros_planning_interface
  moveit_core
  tf
  tf_conversions
  block_grasp_generator
)

find_package(OpenCV REQUIRED)


catkin_package(
  CATKIN_DEPENDS
    roscpp
    std_msgs
    actionlib 
    visualization_msgs 
    clam_controller 
    clam_msgs
    pcl_ros
    cv_bridge
    eigen_conversions 
    moveit_ros_planning 
    moveit_ros_planning_interface
    moveit_core
    tf
    tf_conversions
    block_grasp_generator
#  INCLUDE_DIRS include
)

# Enable gdb to show line numbers
#set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -g")
#set(CMAKE_BUILD_TYPE Debug)

## Build 
include_directories(
  include 
  ${catkin_INCLUDE_DIRS}
)
link_directories(${catkin_LIBRARY_DIRS})

## Action Servers  -------------------------------------------

# Perception
add_executable(block_perception_server src/block_perception_server.cpp)
add_dependencies(block_perception_server clam_msgs_gencpp ${PROJECT_NAME}_gencfg) # wait for msgs to be buil
target_link_libraries(block_perception_server ${catkin_LIBRARIES})

# Pick Place - Custom
#add_executable(block_pick_place_server src/block_pick_place_server.cpp)
#target_link_libraries(block_pick_place_server ${catkin_LIBRARIES})

# Pick Place - MoveIt
add_executable(block_pick_place_moveit_server src/block_pick_place_moveit_server.cpp)
add_dependencies(block_pick_place_moveit_server clam_msgs_gencpp ${PROJECT_NAME}_gencfg) # wait for msgs to be buil
target_link_libraries(block_pick_place_moveit_server ${catkin_LIBRARIES})

# Logic
add_executable(block_manipulation_logic src/block_manipulation_logic.cpp)
add_dependencies(block_manipulation_logic clam_msgs_gencpp ${PROJECT_NAME}_gencfg) # wait for msgs to be buil
target_link_libraries(block_manipulation_logic ${catkin_LIBRARIES})


