# Mathematical Modeling

### `head_pose`
- **Math**: Uses `cv2.solvePnP` to map 3D landmarks (`lmk_3d` in W) to 2D image plane (`lmk_2d` in I). Outputs rotation `rot_v` and translation `trans_v` mapping W to C.

### `W_2_C`
- **Math**: `vers_C = np.matmul(rotation_matrix, vers_W) + translation_vector`. Explicitly transforms World points to Camera points. Note the sign adjustment: `if (vers_C[:, 2] < 0).all(): vers_C = - vers_C` to ensure points are in front of the camera.

### `W_2_I`
- **Math**: Uses `cv2.projectPoints` to project World 3D coordinates into 2D Image pixels via intrinsics.

### `average_gaze`
- **Math**:
  ```python
  left_gaze = LeftPupil - LeftEyeballCenter
  right_gaze = RightPupil - RightEyeballCenter
  left_gaze_norm = left_gaze / np.linalg.norm(left_gaze)
  right_gaze_norm = right_gaze / np.linalg.norm(right_gaze)
  avg_gaze = (left_gaze_norm + right_gaze_norm) / 2
  ```
- **Analysis**: This linearly averages two 3D direction vectors. This is the exact bottleneck where binocular vergence and left/right differences are destroyed. The resultant vector is NOT renormalized to length 1.

### `lines_intersection`
- **Math**: Computes the least-squares intersection of multiple lines in 3D. Used during calibration to find the eyeball center from multiple gaze directions.
