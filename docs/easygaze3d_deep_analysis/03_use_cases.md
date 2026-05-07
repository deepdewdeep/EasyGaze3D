# Use Cases

## Use Case 1: Subject Specific Calibration
**Flow**:
1. User captures calibration images while staring at the camera lens.
2. `calibrate_features.py` loops over calibration images.
3. Extracts 2D landmarks (`detect_lmk_2d`).
4. Reconstructs 3D facial shape (`recon_facial_shape`).
5. Averages facial shapes across all frames to get a stable `avg_fs_3d`.
6. Uses PnP to estimate head pose (`head_pose`) using 3D/2D landmarks.
7. Projects facial shape into image coordinates, identifying pupil and eye contours.
8. Constructs gaze lines by projecting from image plane into 3D world space (`I_2_W`).
9. Estimates eyeball centers by finding the intersection of these gaze lines (`lines_intersection`).
10. Saves `facial_shape_3d.npy` and `eyeball_center.npy`.

**Files**: `calibrate_features.py`, `helpers.py`, `facial_shape.py`, `landmark.py`
**Outputs**: Subject-specific parameters (eyeball center, 3D facial shape).

## Use Case 2: Real Time Gaze Estimation
**Flow**:
1. `demo_webcam.py` captures webcam frames.
2. Loads camera intrinsics and subject-specific parameters.
3. `gaze_vector()` is called per frame.
4. Detects 2D landmarks via MediaPipe.
5. Uses PnP (`head_pose`) to align the saved 3D facial shape with 2D landmarks.
6. Computes eye masks and pupil centers in World (W) and Camera (C) coordinates.
7. Uses the saved eyeball centers.
8. Computes left and right gaze vectors separately.
9. **Averages the left and right vectors** using `average_gaze()`.
10. Returns the averaged `gaze_W` and `gaze_C`.
11. Smoothes `gaze_I` via a Kalman filter in `demo_webcam.py` and draws arrows.

**Files**: `demo_webcam.py`, `gaze_estimation.py`, `helpers.py`
**Outputs**: Averaged gaze direction drawn on screen.

## Use Case 3: Offline Video Processing Adaptation
Offline processing is not directly implemented, but `demo_webcam.py` logic can be trivially adapted. A new script could read `cv2.VideoCapture('video.mp4')`, run `gaze_vector()`, and store the frame-by-frame outputs (especially the `extend_dict` containing left/right vectors) into a CSV or JSON file.
