# Implementation Report: Minimal Dictionary Export

## Files Modified
- `gaze_estimation.py`

## Functions Modified
- `gaze_vector()`

## New Fields Added to `extend_dict`
1. `LeftGaze_W`: Independent, subject-left eye gaze vector in head-relative World (W) space.
2. `RightGaze_W`: Independent, subject-right eye gaze vector in head-relative World (W) space.
3. `LeftGaze_C`: Independent, subject-left eye gaze vector in camera-relative spatial (C) space.
4. `RightGaze_C`: Independent, subject-right eye gaze vector in camera-relative spatial (C) space.
5. `frame_valid`: Boolean flag checking for successful facial tracking and vector calculations.
6. `gaze_vector_export_version`: String '1.0' to track the API behavior.
7. `gaze_coordinate_note`: Explicit warning about interpreting W and C systems correctly to avoid artificial vergence errors.

## Mathematical Formula Used
```python
vector = pupil_point - eyeball_center
normalized_vector = vector / (np.linalg.norm(vector) + 1e-10)
```
This strictly duplicates the logic found in `average_gaze()` inside `helpers.py` without modifying the original function.

## Coordinate System Warning
The output dictionary now contains `gaze_coordinate_note`, explicitly stating: "W vectors are head-relative model space. C vectors are camera-relative spatial vectors. Left/Right based on subject anatomy."

## Validity Flag Behavior
If `detect_lmk_2d` or `recon_facial_shape` or `eye_mask` fails, the function continues to return a fallback `np.zeros(3)`, but `extend_dict['frame_valid']` is now explicitly set to `False` (`return np.zeros(...), ..., {'frame_valid': False}, False`).
When successful, it also asserts that all computed gaze vectors are finite and their raw lengths exceed a `1e-6` safety threshold before confirming `frame_valid = True`.

## Backward Compatibility Statement
- `average_gaze()` is untouched.
- `gaze_W` and `gaze_C` (the averaged outputs) are still returned as the primary function outputs.
- `demo_webcam.py` visualization remains unchanged.

## Remaining Technical Risk
The implementation isolates independent gaze correctly, but downstream analysis code MUST check the `frame_valid` flag, otherwise `np.zeros` will be processed as valid clinical vectors.

## Remaining Scientific Risk
MediaPipe Iris detection under extreme horizontal and vertical eye rotations may be physically inaccurate. High-strabismus samples should still be validated with a Tobii/EyeLink device before declaring definitive biomarker validity.

## How to Inspect
Run `cat gaze_estimation.py` and inspect the bottom 30 lines of `gaze_vector()`, or run `python3 test_export.py` to trigger the static assertion tests.
