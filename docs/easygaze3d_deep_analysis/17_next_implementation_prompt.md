# Next Implementation Prompt

```text
@Jules,

Please implement the "Minimal Dictionary Export" strategy for the EasyGaze3D repository.
The implementation must preserve independent left eye and right eye gaze vectors and must not only expose an averaged gaze vector.

Requirements:
1. Target File: Inspect and modify `gaze_estimation.py` inside the `gaze_vector()` function.
2. Target Function: Do NOT modify the `average_gaze()` function in `helpers.py`.
3. Computation: Calculate `left_gaze_W`, `right_gaze_W`, `left_gaze_C`, and `right_gaze_C` independently using vector subtraction (`Pupil - EyeballCenter`).
4. Normalization: You MUST manually normalize these four vectors using `/ (np.linalg.norm(vec) + 1e-10)` before exporting them.
5. Export: Append `LeftGaze_W`, `RightGaze_W`, `LeftGaze_C`, and `RightGaze_C` to `extend_dict`.
6. Coordinate Metadata: Add `extend_dict['coord_metadata'] = 'W is head-relative, C is camera-relative'`.
7. Validity Flags: Append `extend_dict['frame_valid'] = True` in the success block. Ensure failure blocks return an empty dict `{}` or handle flags appropriately so the user knows `np.zeros` outputs are invalid.
8. Backward Compatibility: Preserve the existing return signature exactly: `gaze_C, gaze_W, extend_dict, exist_face`.
9. Constraints: Do not retrain models. Do not broadly refactor the codebase. Keep raw and smoothed vectors separate (do not add smoothing to `gaze_estimation.py`).
10. Testing: Create a minimal test script `test_export.py` to ensure the dictionary keys exist, vectors are length 1.0, and `average_gaze` output remains unchanged.
11. Await human approval before executing this implementation.
```