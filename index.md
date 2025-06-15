## Portfolio

---

### Pose estimation

[ArUco marker pose estimation](https://github.com/sergiogtorres/ArUco_tracker)
<img src="images/thumbnail-wide.png?raw=true"/>

Demo of pose estimation using OpenCV's implementation of the Perspective-n-Point (PnP) algortihm.
Since the absolute dimensions of the QR/ArUco/etc. marker are known, the pose of the marker (pose and orientation) can 
be solved for in world units (meters, or otherwise)

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


---

[Stereo Visual Odometry](https://github.com/sergiogtorres/stereo_visual_odometry)



---

[Occupancy Grid Demo](https://github.com/sergiogtorres/occupancy_grid_demo)



---