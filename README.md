# CSCE 274 002 Fall 2022 – Project 2 – swhidden

The name of my duckiebot is littleduck.

Here's my before image:
![rqt before image](https://github.com/LittlestCube/project-2/raw/master/rosgraph.png)

Here's my after image:
![rqt after image](https://github.com/LittlestCube/project-2/raw/master/rosgraph_after.png)

I can find eight nodes that have been directly initialized by roslaunch afterwards:

littleduck/fsm_node
littleduck/line_detector_node
littleduck/ground_projection_node
littleduck/line_segment_visualizer_node
littleduck/lane_filter_node
littleduck/lane_controller_node
littleduck/lane_pose_visualizer_node
littleduck/anti_instagram_node

Although it appears from the command window that there is a ninth, littleduck_to_map, but I simply cannot find it on the graph. It also mysteriously does not have /littleduck as a parent. As far as explanations for each node go...

The fsm_node keeps track of what state the robot is currently in. Part of its job is to defer control to either the joystick or the duckiebot, depending on what gets published to joystick_override by joy_mapper_node.

The anti_instagram_node takes the image from the camera and helps the line_detector_node figure out where the lines are by calculating thresholds from the given images and publishing them.

I'm going to take a **wild** guess and say that the line_detector_node detects lines. It receives the thresholds from anti_instagram_node, the image from the camera_node and publishes a segment list to ground_projection_node, presumably concerning the lines it has now detected.

It looks like ground_projection_node takes the segment list from line_detector_node and creates a better picture, or _projection_, of the ground for the robot to use. This takes the form of lineseglist which gets published to lane_filter_node and line_segment_visualizer_node.

The lane_filter_node takes the lineseglist and filters out the parts that actually contain the lane of the road. It calculates lane_pose and publishes it to both lane_pose_visualizer_node (which I believe, taking another unbelievable stab in the dark, visualizes lane_pose) and lane_controller_node. The name lane_pose sounds like lane position, which implies that the duckies can handle roads with multiple same-direction lanes. If that code works, that rules.

Finally, lane_controller_node receives lane_pose from lane_filter_node, presumably so that it can decide whether or not to change that value and move to another lane, and I suspect it reads wheels_cmd and car_cmd to do that.

Oh and the line segment visualizer visualizes line segments blah blah blah

Begin the awfulness: https://youtu.be/BxeWFaB7NeY

I have clocked over 30 hours in the lab trying to get this to work. I have tried replacing the rpi, the controller, the camera, the big wheels and the caster wheel. I don't know what that's worth, but I've replaced so many things my duckie is becoming the flippin' Ship of Theseus.