cmake_minimum_required(VERSION 3.5.1)
cmake_policy(SET CMP0048 NEW)
project(sbpl_lattice_planner)

##############################################################################
# Find dependencies
##############################################################################

set(THIS_PACKAGE_ROS_DEPS roscpp costmap_2d nav_core pluginlib tf tf2
  geometry_msgs nav_msgs)
find_package(catkin REQUIRED COMPONENTS
  ${THIS_PACKAGE_ROS_DEPS} message_generation)

# Find SBPL
find_package(PkgConfig REQUIRED)
pkg_check_modules(SBPL REQUIRED sbpl)
find_library(SBPL_LIBRARY NAMES sbpl PATHS ${SBPL_LIBRARY_DIRS} NO_DEFAULT_PATH)
set(SBPL_LIBRARIES ${SBPL_LIBRARY})

include_directories(include ${catkin_INCLUDE_DIRS} ${SBPL_INCLUDE_DIRS})

##############################################################################
# Generate messages
##############################################################################

add_message_files(FILES SBPLLatticePlannerStats.msg)
generate_messages(DEPENDENCIES geometry_msgs)

##############################################################################
# Define package
##############################################################################

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES ${PROJECT_NAME}
  CATKIN_DEPENDS ${THIS_PACKAGE_ROS_DEPS} message_runtime
  DEPENDS SBPL
)

##############################################################################
# Build
##############################################################################

add_library(${PROJECT_NAME} src/sbpl_lattice_planner.cpp)
target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES} ${SBPL_LIBRARIES})
add_dependencies(${PROJECT_NAME} ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})
target_compile_options(${PROJECT_NAME} PUBLIC "-Wno-terminate")  # suppress warning from included SBPL header

##############################################################################
# Install
##############################################################################

install(TARGETS ${PROJECT_NAME}
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.h"
  PATTERN ".svn" EXCLUDE
)

install(FILES bgp_plugin.xml
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)

install(DIRECTORY
  launch
  matlab
  rviz
  worlds
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)
