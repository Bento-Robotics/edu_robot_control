FROM eduartrobotik/eduart-robot-control:0.3.1

# using bash
SHELL ["/bin/bash", "-c"]
ENV ROS_DISTRO humble
ENV USER user
ENV DEBIAN_FRONTEND=noninteractive

# installing of linux joy node
USER root
RUN apt-get update \
    && apt-get install -y ros-$ROS_DISTRO-joy-linux

USER $USER

# in case cyclone is used this profile is needed
COPY ./ipc_cyclone_dds_profile.xml /home/$USER/
COPY ./ie_app_cyclone_dds_profile.xml /home/$USER/
ENV CYCLONEDDS_URI=/home/$USER/ipc_cyclone_dds_profile.xml

# add entry point
COPY --chown=$USER ./entrypoint /home/$USER/
RUN sudo chmod a+x /home/$USER/entrypoint
ENTRYPOINT [ "/home/user/entrypoint" ]

# launch control node and joy interpreter
CMD [ "ros2", "launch", "edu_robot_control", "robot_remote_control_ipc127e.launch.py" ]
