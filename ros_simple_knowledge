'ros wiki'에서 찾아서 ros에 필요한 패키지 다운받기

http://wiki.ros.org/noetic/Installation/Ubuntu



현재 우분투는 20.04
python 관련은 꼭 python3로 설치 할 것



everything in this document is from [http://wiki.ros.org/Documentation](http://wiki.ros.org/Documentation)



## Create a ROS Workspace

```shell
$ mkdir -p ~/catkin_ws/src
$ cd ~/catkin_ws/
$ catkin_make
```

The catkin_make command is a convenience tool for working with catkin workspaces. Running it the first time in your workspace, it will create a CMakeLists.txt link in your 'src' folder.



## Creating a catkin Package

This tutorial will demonstrate how to use the [catkin_create_pkg](http://wiki.ros.org/catkin/commands/catkin_create_pkg) script to create a new catkin package, and what you can do with it after it has been created. 

First change to the source space directory of the catkin workspace you created in the [Creating a Workspace for catkin tutorial](http://wiki.ros.org/catkin/Tutorials/create_a_workspace): 

```shell
# You should have created this in the Creating a Workspace Tutorial
$ cd ~/catkin_ws/src
```

Now use the `catkin_create_pkg` script to create a new package called 'beginner_tutorials' which depends on std_msgs, roscpp, and rospy: 

```shell
$ catkin_create_pkg beginner_tutorials std_msgs rospy roscpp
```

This will create a `beginner_tutorials` folder which contains a [package.xml](http://wiki.ros.org/catkin/package.xml) and a [CMakeLists.txt](http://wiki.ros.org/catkin/CMakeLists.txt), which have been partially filled out with the information you gave `catkin_create_pkg`. 

`catkin_create_pkg` requires that you give it a `package_name` and optionally a list of dependencies on which that package depends: 

```shell
# This is an example, do not try to run this
# catkin_create_pkg <package_name> [depend1] [depend2] [depend3]
```

`catkin_create_pkg` also has more advanced functionalities which are described in [catkin/commands/catkin_create_pkg](http://wiki.ros.org/catkin/commands/catkin_create_pkg). 



## ROSPY

ospy is a pure Python client library for ROS. The rospy client    API enables Python programmers to quickly interface with ROS [Topics](http://ros.org/wiki/Topics), [Services](http://ros.org/wiki/Services), and [Parameters](http://ros.org/wiki/Parameter Server).

how to install rospy

```shell
$ sudo apt-get update -y
$ sudo apt-get install -y python-rospy
```



## simple publisher and subscriber (pub, sub)

[http://wiki.ros.org/rospy_tutorials/Tutorials/WritingPublisherSubscriber](http://wiki.ros.org/rospy_tutorials/Tutorials/WritingPublisherSubscriber)

making pub, sub package by terminal

```shell
# You should have created this in the Creating a Workspace Tutorial
$ roscd beginner_tutorials

$ mkdir scripts
$ cd scripts

# download pre-coded talker.py and modify property of the file.
# talker.py is the publisher
$ wget https://raw.github.com/ros/ros_tutorials/kinetic-devel/rospy_tutorials/001_talker_listener/talker.py
$ chmod +x talker.py

# download pre-coded listener.py and modify property of the file.
# listner.py is the subscriber
$ roscd beginner_tutorials/scripts/
$ wget https://raw.github.com/ros/ros_tutorials/kinetic-devel/rospy_tutorials/001_talker_listener/listener.py
$ chmod +x listener.py (Package)
```

modify cmake file

```shell
# add
catkin_install_python(PROGRAMS scripts/talker.py scripts/listener.py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
```

catkin_make (build package)

```shell
$ cd ~/catkin_ws
$ catkin_make
```

run package in terminal

```shell
$ roscore
# new window
$ rosrun beginner_tutorials talker.py
# new window
$ rosrun beginner_tutorials listener.py
# new window
$ rqt_graph
```

talker.py

```python
#!/usr/bin/env python
import rospy
from std_msgs.msg import String

def talker():
    pub = rospy.Publisher('chatter', String, queue_size=10)
    rospy.init_node('talker', anonymous=True)
    rate = rospy.Rate(10) # 10hz
    while not rospy.is_shutdown():
        hello_str = "hello world %s" % rospy.get_time()
        rospy.loginfo(hello_str)
        pub.publish(hello_str)
        rate.sleep()

if __name__ == '__main__':
    try:
        talker()
    except rospy.ROSInterruptException:
        pass
```

listener.py

```python
#!/usr/bin/env python
import rospy
from std_msgs.msg import String

def callback(data):
    rospy.loginfo(rospy.get_caller_id() + "I heard %s", data.data)
    
def listener():

    # In ROS, nodes are uniquely named. If two nodes with the same
    # name are launched, the previous one is kicked off. The
    # anonymous=True flag means that rospy will choose a unique
    # name for our 'listener' node so that multiple listeners can
    # run simultaneously.
    rospy.init_node('listener', anonymous=True)

    rospy.Subscriber("chatter", String, callback)

    # spin() simply keeps python from exiting until this node is stopped
    rospy.spin()

if __name__ == '__main__':
    listener()
```

