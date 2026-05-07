# Exact Gaze Vector Trace

1. **input image or frame**: Captured in `demo_webcam.py` (line 74), standard BGR numpy array.
2. **2D facial landmarks**: `lmk_2d` in `landmark.py`, `detect_lmk_2d`. Coordinates in Image space (I). 468x2 array.
3. **3D face model or facial shape**: `fs_3d` in `facial_shape.py`, `recon_facial_shape`. World space (W). 38365x3 array.
4. **head pose**: `rot_v`, `rot_mat`, `trans_v` in `helpers.py`, `head_pose`. Uses PnP.
5. **pupil points (2D)**: `Pupil_2d` extracted from `lmk_2d`. Image space (I).
6. **eye masks**: `LeftEye_mask_W`, `RightEye_mask_W` in `helpers.py`, `eye_mask`. World space (W).
7. **left pupil in W**: `LeftPupil_W` in `helpers.py`, `pupil_center`. World space (W). (3,) array. Not currently saved/exported independently.
8. **right pupil in W**: `RightPupil_W` in `helpers.py`, `pupil_center`. World space (W). (3,) array. Not currently saved/exported independently.
9. **left pupil in C**: `LeftPupil_C` in `helpers.py`, `W_2_C`. Camera space (C). (3,) array. Not saved/exported.
10. **right pupil in C**: `RightPupil_C` in `helpers.py`, `W_2_C`. Camera space (C). (3,) array. Not saved/exported.
11. **left eyeball center in W**: `LeftEyeballCenter_W` from `calibration/results/.../eyeball_center.npy`. World space (W). Saved during calibration. Not exported at runtime.
12. **right eyeball center in W**: `RightEyeballCenter_W` from same file.
13. **left eyeball center in C**: `LeftEyeballCenter_C` converted via `W_2_C` in `gaze_estimation.py`. Camera space (C).
14. **right eyeball center in C**: `RightEyeballCenter_C` converted via `W_2_C` in `gaze_estimation.py`. Camera space (C).
15. **left gaze vector in W**: Calculated inside `average_gaze` but discarded. **Needed for final goal.**
16. **right gaze vector in W**: Calculated inside `average_gaze` but discarded. **Needed for final goal.**
17. **averaged gaze vector in W**: `gaze_W` returned by `gaze_vector`. Exported.
18. **left gaze vector in C**: Calculated inside `average_gaze` but discarded. **Needed for final goal.**
19. **right gaze vector in C**: Calculated inside `average_gaze` but discarded. **Needed for final goal.**
20. **averaged gaze vector in C**: `gaze_C` returned by `gaze_vector`. Exported.
21. **final dictionary or returned output**: `extend_dict` containing 2D/W pupil info and camera distance.
22. **visualization output**: Processed in `demo_webcam.py` using `kalman_filter` to smooth `gaze_I` and draw arrows on screen.