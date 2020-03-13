## Paper: 사용자 맞춤형 광고 제공을 위한 오픈소스 로봇 플랫폼(Open-source robot platform providing personalized advertisements)

#
## Robot Appearance
![fig 2 Robot appearance_copy](https://user-images.githubusercontent.com/62131182/76616839-482af380-6568-11ea-9436-bbef9b91e156.jpg)

## Robot Specification
* Main Board
   * Odroid H2
* Micro controller
   * Arduino Mega2580
* Distance measurement sensors
   * HC-SR04 Ultrasonic(5EA)
   * Infrared(1EA)
* Image sensor
   * OcamS-1CGN-U
* HRI media
   * Speaker: Britz BA-BR9 Soundbar
   * Monitor: ECO GD220LED HDMI SLIM
* Omni-wheel driving modules
   * 12V DC Coreless motor X3
   * DCMD-50-D Motor driver X3
   * 48mm omni wheel X3
* Communication
   * WiFi module
* Battery
   * DC12V/2.6Ah(3.7V/2600mA*3ea)X2
   * DC12V/5.2Ah(3.7V/2600mA*6ea) 


## How to Start main.py(Advertisement service) 
* Requirements
    * Register an account with MS Azure cognitive services 
    * Install MS Azure Face API
          
          $sudo pip install cognitive_face
          $sudo apt install imagemagick -y
            
            * Reference: https://blog.naver.com/ljy9378/221463790053
 * Modify Key and Base_url in main.py
 * Start main.py
       
       $python main.py
      
## ROS Libarary install on Arduino
* rosserial install for Arduino
   * http://wiki.ros.org/rosserial_arduino/Tutorials/Arduino%20IDE%20Setup
   * Arduino IDE Setup
            
            $sudo apt-get install ros-kinetic-rosserial-arduino
            $sudo apt-get install ros-kinetic-rosserial
    
   * Installing from Source onto the ROS workstation

          $cd ~/catkin_ws/src
          $git clone https://github.com/ros-drivers/rosserial.git
          $cd ~/catkin_ws && catkin_make
    
   * Install ros_lib into the Arduino Environment
    
         $cd ~/Arduino/libraries
         $rm -rf ros_lib
         $rosrun rosserial_arduino make_libraries.py .

## How to drive a robot

1. Omni_Robot.ino file upload on Arduino Mega

2. Start ROS Master
 
        $roscore

3. Connect Arduino Mega and Odroid H2(Master) to rosserial
   
        $rosrun rosserial_python serial_node.py _port:=/dev/serial_port_file(=ttyACM#,#:serial_port_number)
   * Example 
       
         $rosrun rosserial_python serial_node.py _port:=/dev/ttyACM1

3. robot control ros command

       $rostopic pub -r 15 /byu_control geometry_msgs/Transform ‘[translation: [translation(x),translation(y), translation(z)], rotation: [rotation(x), rotation(y), rotation(z), w]’

4. Robot driving Example
    * Open three terminals and type the following commands in order for each terminal.
        
          $roscore
          $rosrun rosserial_python serial_node.py _port:=/dev/ttyACM1
          $rostopic pub -r 15 /byu_control geometry_msgs/Transform '{translation: [150, 150, 0], rotation: [0, 0, 0.5, 0]}'
