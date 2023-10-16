# edu_robot_control

Provides nodes, robot description for RViz and other tools for remote control of EduArt's robots for an easy monitoring and controlling.

# Controlling the Robot Using Remote Control Node

With the provided ROS remote control node a gamepad or joystick can be used for controlling the robot. The default assignment for the controller is:

| Axis  | DS5                       | Idle position | Value range | function          | 
|-------|---------------------------|---------------|-------------|-------------------|
| [0]   | Joystick L: left & right  | 0.0           | 1.0 to -1.0 | Steering
| [1]   | Joystick L: up & down     | 0.0           | 1.0 to -1.0 | not in use
| [2]   | L2                        | 1.0           | 1.0 to -1.0 | not in use
| [3]   | Joystick R: left & right  | 0.0           | 1.0 to -1.0 | not in use
| [4]   | Joystick R: up & down     | 0.0           | 1.0 to -1.0 | Throttle
| [5]   | R2                        | 1.0           | 1.0 to -1.0 | not in use
| [6]   | D-Pad: left & right       | 0.0           | 1.0 to -1.0 | not in use
| [7]   | D-Pad: up & down          | 0.0           | 1.0 to -1.0 | not in use

| Button    | DS5           | Idle position | Value range   | function          | 
|-----------|---------------|---------------|---------------|-------------------|
| [0]       | Square        | 0             | 0 or 1        | Switch to Skid Drive Kinematic
| [1]       | Cross         | 0             | 0 or 1        | Light pattern: Operation
| [2]       | Circle        | 0             | 0 or 1        | Switch to Mecanum Drive Kinematic
| [3]       | Triangle      | 0             | 0 or 1        | Light pattern: Operation
| [4]       | L1            | 0             | 0 or 1        | Light pattern: Turning left
| [5]       | R1            | 0             | 0 or 1        | Light pattern: Turning right
| [6]       | L2            | 0             | 0 or 1        | Switch into mode AUTONOMOUS (drives will be enabled and robot will subscribe to topic "autonomous/cmd_vel")
| [7]       | R2            | 0             | 0 or 1        | Override collision avoidance
| [8]       | SHARE         | 0             | 0 or 1        | Disable driving
| [9]       | OPTIONS       | 0             | 0 or 1        | Switch into mode REMOTE_CONTROLLED (drives will be enabled and robot will subscribe to topic "cmd_vel")
| [10]      | PS            | 0             | 0 or 1        | Connect Controller to Eduard
| [11]      | L3            | 0             | 0 or 1        | not in use
| [12]      | R3            | 0             | 0 or 1        | not in use
| [13]      | Map           | 0             | 0 or 1        | Light pattern: Warning light

and is defined in the parameter file "remote_control.yaml" located in the folder "parameter". Of course it can be reassigned to your needs.

## Deploying

A Docker container is used to run this software on our robots. Normally, all our robots are shipped with a Docker container registered to start after a reboot. If you want to deploy a newer version, or whatever the reason, make sure to remove the previously deployed container. To check which containers are running, use the following command:

```bash
docker container ls 
```
A typically print out looks like:

```bash
CONTAINER ID   IMAGE                      COMMAND                  CREATED      STATUS          PORTS     NAMES
46c8590424c0   eduard-iotbot:0.2.1-beta   "/ros_entrypoint.sh …"   6 days ago   Up 21 minutes             eduard-iotbot-0.2.1-beta
```

To stop and remove the container, use the following command with the container ID displayed by the above command:

```bash
docker stop <container id>
docker rm <container id>
```

## Deploying on IoT2050

>Note: If you are using [edu_robot](https://github.com/EduArt-Robotik/edu_robot), it is not necessary to install **edu_robot_control**. It is already supplied with **edu_robot**.

### Use Prebuilt Docker Images

The easiest way, and one that is usually quite sufficient, is to use a prebuilt Docker image. All released versions of edu_robot software are usually available. 

The following command deploys and starts the image. Note: please make sure that the robot has internet connection. It is considered that the official ["Example Image V1.3.1"](https://support.industry.siemens.com/cs/document/109741799/downloads-f%C3%BCr-simatic-iot20x0?dti=0&lc=de-DE) provided by Siemens will be used for the IoT2050. If this is not the case it cloud lead in a misinterpretation of the game pad.

We provide a docker compose file for the IoT2050. Either [download the file](docker/iot2050/docker-compose.yaml) on IoT2050 or clone the repository and navigate to the file:

```bash
git clone https://github.com/EduArt-Robotik/edu_robot.git
cd edu_robot_control/docker/iot2050
```

Then execute following command inside the folder where the ["docker-compose.yaml"](docker/iot2050/docker-compose.yaml) is located:

```bash
docker compose up
```

Inside the docker compose file a namespace is defined. This namespace can freely be modified. We recommend to reflect the robot color with this namespace. But in general do it like you want.

The docker container can be removed by the command:

```bash
docker compose down
```

executed at the location of the docker compose file. This has to be done if a newer version should be deployed.

## Deploying on IPC127e

>Note: If you are using [edu_robot](https://github.com/EduArt-Robotik/edu_robot), it is not necessary to install **edu_robot_control**. It is already supplied with **edu_robot**.

The easiest way, and one that is usually quite sufficient, is to use a prebuilt Docker image. All released versions of edu_robot software are usually available. 

```bash
git clone https://github.com/EduArt-Robotik/edu_robot.git
cd edu_robot_control/docker/ipc127e
```

Then execute following command inside the folder where the ["docker-compose.yaml"](docker/ipc127e/docker-compose.yaml) is located:

```bash
docker compose up
```

Inside the docker compose file a namespace is defined. This namespace can freely be modified. We recommend to reflect the robot color with this namespace. But in general do it like you want.

The docker container can be removed by the command:

```bash
docker compose down
```

executed at the location of the docker compose file. This has to be done if a newer version should be deployed.

## Setting up your Joystick

A joystick can be used to operate Eduard. For this purpose, the robot must be extended by a Debian-compatible Bluetooth stick. PlayStation&reg; 4 and PlayStation&reg; 5 controllers were used, which are interpreted identically in their operating interface. The initial start-up of a Bluetooth controller follows.

Start the Bluetooth controller in the operating system:

```console
$ bluetoothctl
```

Set up the controller and prepare for scanning:

```console
$ agent on 
$ default-agent 
$ power on 
$ discoverable on 
$ pairable on
```

Put the PlayStation&reg; Controller into connection mode by pressing the Share and PS buttons simultaneously. 
Rapid flashing indicates the status.

<!-- <img src="documentation/images/controller_pairing.jpg" width="500" /> <br> -->

Now start the scanning process:

```console
$ scan on
```

The connection process so far should look like this. Your joystick is now recognised as a wireless controller. Copy the MAC address of the device for the rest of the procedure.

Connect the controller using the following commands and its MAC address. If needed, press the PlayStation button again when the light signals stop flashing.

```console
$ pair XX:XX:XX:XX:XX:XX 
$ trust XX:XX:XX:XX:XX:XX 
$ connect XX:XX:XX:XX:XX:XX
$ exit 
```

NOTE: These steps are only required once at the very beginning. From now on, when the PS button is pressed, the joystick should automatically connect to the IOT2050 once it has successfully booted up. These operations are only then necessary again if the controller has been connected to another device in the meantime.


# Monitoring Eduard using RViz

For visualization of Eduard's sensors and actors a RViz setup is provided including a robot description. Since Eduard ROS control node publish all of Eduard's states via TF and ROS topic/services it is easy to access them.

The best way to monitor Eduard's states is using RViz. A launch file is provided including a RViz configuration that allows an easy and fast start. Two ways are supported. Also the color and the wheel type can be selected, which then is respected by RViz.

## Native ROS2 Installation

If ROS is natively installed the "edu_robot_control" package can be installed into an ROS workspace. As first step clone the package into the workspace by:

```bash
git clone https://github.com/EduArt-Robotik/edu_robot_control.git
```

Please make sure the package will be cloned into the "src" folder in the workspace. If no knowledge about ROS is present please see [docs.ros.org](https://docs.ros.org/en/galactic/Tutorials/Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.html) for further information. 

After the package was cloned it needs to be installed via:

```bash
colcon build --packages-select edu_robot_control 
```

Now RViz with the correct configuration can be launched by:

```bash
ros2 launch edu_robot_control eduard_monitor.launch.py
```

EDU_ROBOT_NAMESPACE and EDU_ROBOT_WHEEL_TYPE environment variable is read by this launch file. The namespace must fit to the used one of the robot. The wheel type can be "mecanum" or "skid". RViz will load the corresponding meshes. If RViz comes up properly it will be shown following:

![Eduard visualized using RViz](documentation/image/eduard-monitoring-using-rviz.png)

## Installed into a Docker Container

Another way is to build a Docker image using the provided docker file. First navigate into the correct folder:

```bash
cd edu_robot/docker/host
```

Next step is to build the Docker image by executing following command:

```bash
make all
make clean
```

After the Docker image was built it can be started using following command:

```bash
docker run --rm --user=user --net=host --pid=host --ipc=host --env=DISPLAY --volume=/tmp/.X11-unix:/tmp/.X11-unix:rw --env EDU_ROBOT_NAMESPACE=eduard/red --env EDU_ROBOT_WHEEL_TYPE=mecanum eduard-robot-monitoring:0.2.0
```
