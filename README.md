# pylaunch [![Build Status](https://magnum.travis-ci.com/mayfieldrobotics/pylaunch.svg?token=qHBoPmgQbWPVxfoYZkz1&branch=master)](https://magnum.travis-ci.com/mayfieldrobotics/pylaunch)
Library for writing and calling roslaunch files in Python.

### Example
```python
import pylaunch as pl

#pass in package_name, executable name, and name of node
configs = [pl.Node("rospy_tutorials", "talker", "talker2",
                # passing in node parameters (optional)
                params={'calibrate_time': False},
                # passing in namespaces (optional)
                namespace="/",
                # passing in args for executable (optional)
                args="--my-arg",
                # remapping topics (optional)
                remaps=[('chatter', 'hello_topic')]),
           # Including external launch files (params are where <arg> tags used to go)
           pl.Include('pylaunch', 'listener.launch', params={'some_arg': '21'})]

pl.launch(configs)
```
Or, for more control over roslaunch, use:

```python
p = pl.PyRosLaunch(config_list)
p.start()
raw_input("press enter to stop") # or p.spin(), p.spinOnce(), etc.
#Call this to kill all nodes launched.
#Note: you can't reuse p (i.e. call start again) after calling shutdown.
p.shutdown()  
```

Else, if you just want to call a single launch file:

```python
p = pl.LaunchFileRunner('my_package', 'my_launch_file.launch')
p.start()
raw_input("press enter to stop")
p.shutdown()
```
