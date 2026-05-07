# Final Strategy Plan (Refined)

The verified strategy is strictly conservative:

1. **Target File**: `gaze_estimation.py`
2. **Action**: In `gaze_vector()`, calculate the raw normalized vectors exactly as `average_gaze` does, but DO NOT modify `average_gaze`:
   ```python
   # W Coordinates
   left_gaze_W = LeftPupil_W - LeftEyeballCenter_W
   left_gaze_W = left_gaze_W / (np.linalg.norm(left_gaze_W) + 1e-10)
   right_gaze_W = RightPupil_W - RightEyeballCenter_W
   right_gaze_W = right_gaze_W / (np.linalg.norm(right_gaze_W) + 1e-10)

   # C Coordinates
   left_gaze_C = LeftPupil_C - LeftEyeballCenter_C
   left_gaze_C = left_gaze_C / (np.linalg.norm(left_gaze_C) + 1e-10)
   right_gaze_C = RightPupil_C - RightEyeballCenter_C
   right_gaze_C = right_gaze_C / (np.linalg.norm(right_gaze_C) + 1e-10)
   ```
3. **Action**: Attach these to `extend_dict`:
   ```python
   extend_dict['LeftGaze_W'] = left_gaze_W
   extend_dict['RightGaze_W'] = right_gaze_W
   extend_dict['LeftGaze_C'] = left_gaze_C
   extend_dict['RightGaze_C'] = right_gaze_C
   extend_dict['frame_valid'] = True
   ```
4. **Action**: Ensure failure blocks return `extend_dict['frame_valid'] = False` or return `{}` so the caller knows the returned `np.zeros(3)` is invalid.