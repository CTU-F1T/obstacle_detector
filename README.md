#Package for obstacles detection and tracking
Main purpose of this package is to detect obstacles from PCL and/or Laser Scan and track them. Obstacles are represented as line segments and circles. 

#Original package from - https://github.com/tysik/obstacle_detector

#Dependencies
cmake
libopenblas-dev 
liblapack-dev 
libarpack-dev
armadillo 

#Installation
sudo apt-get install cmake
sudo apt-get install libopenblas-dev
sudo apt-get install liblapack-dev
sudo apt-get install libarpack-dev

#Build
#Needed library for rviz vizualization objects - OGRE3d
sudo apt-get install libogre-1.8-dev #do not install 1.9, does not work!

##Armadillo installation
wget https://sourceforge.net/projects/arma/files/armadillo-8.300.3.tar.xz/download -O armadillo-8.300.3.tar.xz
tar xf armadillo-8.300.3.tar.xz

cd armadillo-8.300.3
cmake .
make

sudo make install


#Obstacles detection
obstacle_extractor - node responsible for obstacles detection.

Listens to the /scan topic for the Laser Scan messages.
Publishes /raw_obstacles with the list of obstacles in custom message Obstacles:

-Obstacles
--Header header
--obstacle_detector/SegmentObstacle[] segments
--obstacle_detector/CircleObstacle[] circles

-CircleObstacle
--geometry_msgs/Point center - center of circular obstacle,
--geometry_msgs/Vector3 velocity - linear velocity of circular obstacle,
--float64 radius - radius of circular obstacle with added safety margin,
--float64 true_radius - measured radius of obstacle without the safety margin.

-SegmentObstacle
--geometry_msgs/Point first_point - first point of the segment (in counter-clockwise direction),
--geometry_msgs/Point last_point - end point of the segment.

The principal of detection is just grouping points together and if the cirle (created with heuristic approach) containing the points is smaller than maximal user defined radius, than the group is represented as circle obstacle. Else the line segment is created.


##Obstacles detection usage
- launch detector with default parameters configuration
rosrun obstacle_detector obstacle_extractor 

- change parameters 
-- from parameters server  - NOT TESTED
-- from rviz panels imported from folder /panels - NOT WORKING AS EXPECTED
-- in code - WORKING, but not practical

- visualize output in rviz
-- add Obstacles layer with topic /raw_obstacles


#Obstacles tracking
##TODO
