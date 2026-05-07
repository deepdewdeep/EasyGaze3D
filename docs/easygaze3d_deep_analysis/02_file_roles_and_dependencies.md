# File Roles and Dependencies

| Path | Category | Key Functions/Classes | Imports | Imported By | Expected Inputs | Expected Outputs | Role | Confidence | Notes |
|---|---|---|---|---|---|---|---|---|---|
| `gaze_estimation.py` | Primary gaze estimation logic | `gaze_vector` | `helpers.*`, `landmark.*`, `facial_shape.*` | `demo_webcam.py` | `image`, `intrinsic_params`, config | `gaze_C`, `gaze_W`, `extend_dict` | Central | High | Bottleneck for left/right gaze averaging is here. |
| `helpers.py` | Helper math and geometry logic | `head_pose`, `W_2_I`, `W_2_C`, `average_gaze`, `pupil_center`, `eyeball_center` | `cv2`, `numpy`, `scipy.io` | `gaze_estimation.py`, `calibrate_features.py` | Landmarks, camera matrices | Vectors, transforms | Central | High | Contains the math for PnP, projection, and the `average_gaze` loss. |
| `calibrate_features.py` | Subject specific calibration | `main` | `helpers.*`, `landmark.*`, `facial_shape.*` | User | Images | 3D facial shape, eyeball centers | Supporting | High | Computes eyeball centers by intersecting gaze lines. |
| `demo_webcam.py` | Primary gaze estimation logic | `main` | `gaze_estimation.*`, `helpers.kalman_filter` | User | Webcam frames | Visualized Gaze | Central | High | Runs the real-time pipeline. |
| `facial_shape.py` | 3D face reconstruction logic | `recon_facial_shape` | `TDDFA_V2` | `gaze_estimation.py` | Image | `fs_3d` | Supporting | High | Relies on third-party 3DDFA_V2. |
| `landmark.py` | Landmark detection logic | `detect_lmk_2d`, `detect_lmk_3d` | `mediapipe` | `gaze_estimation.py` | Image | 2D/3D landmarks | Supporting | High | |
| `config.py` | Configuration | `cfg` | `yacs` | All | None | Configuration object | Central | High | |
