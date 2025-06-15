## Portfolio

---

### Pose estimation

[ArUco marker pose estimation](https://github.com/sergiogtorres/ArUco_tracker)
<img src="images/thumbnail-wide.png?raw=true"/>

Demo of pose estimation using OpenCV's implementation of the Perspective-n-Point (PnP) algortihm.
Since the absolute dimensions of the QR/ArUco/etc. marker are known, the pose of the marker (pose and orientation) can 
be solved for in world units (meters, or otherwise) accurately (assuming known camera intrinsics and an undistorted
frame).

---



[High Speed Dart 3D Tracking](https://github.com/sergiogtorres/dart_tracking_high_speed)
<video width="100%" autoplay loop muted playsinline>
  <source src="images/depth_tracking.webm" type="video/webm">
  Your browser does not support the video tag.
</video>

Detecting a NERF Dart using an Intel Realsense D435i stereo camera's 300 fps mode at a lowered resolution. Since the 
dart is moving much faster than any other object in the scene, background subtraction is an ideal detection method.
The pipeline detects all contours with OpenCV's cv2.findContours(), and filters for a dart using an oriented bounding 
box to check for a contour of a certain aspect ratio and size.

Once a detected contour is confirmed as a dart, it is used as a mask to extract the projectile depth. This is done with
a median operation to remove outliers (e.g. spurious depth pixels of the background inside the mask). 

The 3D coordinates of the dart are extracted through backprojection using the inverse of the camera's intrinsic matrix K. 

First we apply matrix multiplication with the inverse of K to get the 3D x, y, z point, up to scale:

XYZ = K_inv @ dart_uv

We divide by the z component to get normalized camera coordinates:

XYZ /= XYZ[-1]

Finally, multiply by the known z coordinate measured before to get the 3D position of the dart in the camera frame of 
reference:

XYZ *= (avg_z_coord_NERF_dart * 1e-3)

This (x, y, z) vector is tracked with a kalman filter also built from scratch.

---

[Stereo Visual Odometry](https://github.com/sergiogtorres/stereo_visual_odometry)

Pipeline for Stereo Visual Odometry. Uses keypoint detection between successive frames to estimate relative movement
in world units (meters, and orientation). At each frame, 3D points retrieved from a disparity map of the stereo pair of 
images is matched with a single image (from the left camera) from the previous frame to do 3D-2D estimation using the 
Perspective n Point (PnP) algorithm.

This kind of pipeline may be found in autonomous systems, together with other fast sensor data e.g. an IMU, with slow 
but drift-less GNSS measurements using sensor fusion, to track the vehicle's position accurately in real time.

Tested using images from the KITTI benchmark (https://www.cvlibs.net/datasets/kitti/)

---

[Occupancy Grid Demo](https://github.com/sergiogtorres/occupancy_grid_demo)

<video width="100%" autoplay loop muted playsinline>
  <source src="images/occupancy_grid_mapping_demo.webm" type="video/webm">
  Your browser does not support the video tag.
</video>

From-scratch simulation of the concept of occupancy grid mapping of obstacles.
Uses a belief map with bayesian updates to detect obstacles and create an occupancy grid in a simulated, LiDAR-like 
perception simulation.

The algorithm uses ray tracing to backtrack a detection ray and infer either the presence or absence, or no information,
of obstacles. For example, for a certain bearing, all ranges lower than the detected range can be assumed to be free of
obstacles, and the detected range can be assumed to contain an obstacle. This detected obstacle position is discretized
in polar coordinates with a pair of  δr and δφ that represents the minimum detection region for a single ray. Any points
not currently inferrable as obstacle or no-obstacle, i.e., points beyond the maximum detection range, and points at 
relative angles other than the current bearing, are considered as giving no information, so they don't affect the 
belief map.

---