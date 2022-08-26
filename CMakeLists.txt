cmake_minimum_required(VERSION 3.5)
project(latlong2closestlanelet)

list(APPEND CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/cmake")


# Default to C99
if(NOT CMAKE_C_STANDARD)
  set(CMAKE_C_STANDARD 99)
endif()

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
# uncomment the following section in order to fill in
# further dependencies manually.
# find_package(<dependency> REQUIRED)
find_package(rclcpp REQUIRED)
find_package(std_msgs REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(nav_msgs REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(GeographicLib REQUIRED)

install(DIRECTORY launch DESTINATION share/${PROJECT_NAME}/)

include_directories(include/)
include_directories(include
        ${GeographicLib_INCLUDE_DIRS})

ament_export_include_directories(include)

add_executable(${PROJECT_NAME} src/latlong2closestlanelet.cpp)


ament_target_dependencies(${PROJECT_NAME} rclcpp std_msgs geometry_msgs sensor_msgs GeographicLib)


install(TARGETS
        ${PROJECT_NAME}
        DESTINATION lib/${PROJECT_NAME}
        )

set(CLIENT_LIBRARIES
        "GeographicLib"
        )
set(AMENT_DEPENDENCIES
        "tf2"
        "tf2_ros"
        "geometry_msgs"
        "tf2_geometry_msgs"
        "sensor_msgs"
        )
        

target_include_directories(${PROJECT_NAME} PUBLIC
  $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
  $<INSTALL_INTERFACE:include>)

install(TARGETS ${PROJECT_NAME}
  DESTINATION lib/${PROJECT_NAME})


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