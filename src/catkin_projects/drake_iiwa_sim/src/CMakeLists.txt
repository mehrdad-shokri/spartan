# -*- mode: cmake -*-
# vi: set ft=cmake :

# Copyright (c) 2017, Massachusetts Institute of Technology.
# All rights reserved.
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions are met:
#
# * Redistributions of source code must retain the above copyright notice, this
#   list of conditions and the following disclaimer.
#
# * Redistributions in binary form must reproduce the above copyright notice,
#   this list of conditions and the following disclaimer in the documentation
#   and/or other materials provided with the distribution.
#
# * Neither the name of the copyright holder nor the names of its contributors
#   may be used to endorse or promote products derived from this software
#   without specific prior written permission.
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
# AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
# IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
# ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE
# LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
# CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF
# SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
# INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN
# CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)
# ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
# POSSIBILITY OF SUCH DAMAGE.

set(PROJECT_INCLUDE_DIR ${PROJECT_SOURCE_DIR}/include)

add_library(kuka_schunk_station
        kuka_schunk_station.cc ${PROJECT_INCLUDE_DIR}/drake_iiwa_sim/kuka_schunk_station.h)
add_dependencies(kuka_schunk_station ${catkin_EXPORTED_TARGETS})
target_link_libraries(kuka_schunk_station
        drake::drake
        gflags_shared)

add_library(schunk_wsg_ros_actionserver
        schunk_wsg_ros_actionserver.cc
        ${PROJECT_INCLUDE_DIR}/drake_iiwa_sim/schunk_wsg_ros_actionserver.h)
add_dependencies(schunk_wsg_ros_actionserver ${catkin_EXPORTED_TARGETS})
target_link_libraries(schunk_wsg_ros_actionserver
        drake::drake
        ${catkin_LIBRARIES}
        gflags_shared)

add_library(ros_scene_graph_visualizer
        ros_scene_graph_visualizer.cc
        ${PROJECT_INCLUDE_DIR}/drake_iiwa_sim/ros_scene_graph_visualizer.h)
add_dependencies(ros_scene_graph_visualizer ${catkin_EXPORTED_TARGETS})
target_link_libraries(ros_scene_graph_visualizer
        drake::drake
        ${catkin_LIBRARIES}
        gflags_shared)

add_library(ros_rgbd_camera_publisher
        ros_rgbd_camera_publisher.cc
        ${PROJECT_INCLUDE_DIR}/drake_iiwa_sim/ros_rgbd_camera_publisher.h)
add_dependencies(ros_rgbd_camera_publisher ${catkin_EXPORTED_TARGETS})
target_link_libraries(ros_rgbd_camera_publisher
        drake::drake
        ${catkin_LIBRARIES}
        gflags_shared)

add_executable(kuka_schunk_station_simulation
               kuka_schunk_station_simulation.cc)
add_dependencies(kuka_schunk_station_simulation ${catkin_EXPORTED_TARGETS})
target_link_libraries(kuka_schunk_station_simulation
        kuka_schunk_station
        schunk_wsg_ros_actionserver
        ros_scene_graph_visualizer
        ros_rgbd_camera_publisher
        drake::drake
        gflags_shared
	yaml-cpp
        ${catkin_LIBRARIES})

# install library
install(TARGETS schunk_wsg_ros_actionserver kuka_schunk_station
    ros_scene_graph_visualizer ros_rgbd_camera_publisher
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

# install executable
install(TARGETS kuka_schunk_station_simulation
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

