# ros_karl
Karl's ROS work
Getting started with the ROS navigation stack (amcl, move_base)
Initially, my odom coordinate system would jump between the initial location and the logical location based on odometry from the Create 2 robot package. I finally discovered I had made a static tranform between odom and base_link in my move_base.launch file because I was initially thinking the create_autonomy package did not do this. So I had conflicting tf's for odom that were fighting each other. I also removed all of the Create 2 "Robot Model" links so I could keep it simple. Just map, odom, base_link, and laser. The rosrun tf view_frames was useful for debugging this.

After getting base_link to move properly in the odom coordinate system, I moved to debugging amcl localization. The localization would not update. Check the initial covariance matrix defined in the ca_driver folder of the create_autonomy package. I believe the key changes were some parameters in the /opt/ros/kinetic/share/amcl/examples/amcl_diff.launch

<param name="odom_model_type" value="diff-corrected"/>
<param name="min_particles" value="200"/>
  <param name="max_particles" value="1000"/>
<param name="odom_alpha1" value="1.0"/>
  <param name="odom_alpha2" value="1.0"/>
  <param name="odom_alpha3" value="0.8"/>
  <param name="odom_alpha4" value="0.8"/>
    <param name="laser_sigma_hit" value="0.1"/>
    <param name="update_min_d" value="0.0"/>
  <param name="update_min_a" value="0.0"/>
  <param name="transform_tolerance" value="0.3"/>
  <param name="recovery_alpha_slow" value="0.001"/>
  <param name="recovery_alpha_fast" value="0.1"/>
  <param name="use_map_topic" value="true"/>
  <param name="first_map_only" value="true"/>
  <remap from="scan" to="scan_filtered" />
  
  After testing planning, I concluded I needed the laser scan filter (see small_bot_2dnav/launch/)
  
  Also, in move_base.launch:
  <param name="controller_frequency" value="5.0" />
    <param name="clearing_rotation_allowed" value="false" />
