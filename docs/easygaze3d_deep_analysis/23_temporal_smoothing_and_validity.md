# Temporal Smoothing and Validity

1. **Where kalman_filter is used:** Only in `demo_webcam.py` (lines 94, 112) for visual arrow stabilization and camera distance smoothing.
2. **Is smoothing applied to averaged gaze only?** Yes, it smooths `gaze_I` and `dist_to_cam`.
3. **Does it affect core export?** No. `gaze_estimation.py` returns purely raw, unsmoothed data.
4. **What happens when no face is detected?**
   - `gaze_estimation.py` lines 40, 55, 71 return `np.zeros((1, 3)).squeeze(), np.zeros((1, 3)).squeeze(), {}, False`
   - **CRITICAL RISK:** `np.zeros(3)` is not a valid gaze vector. An offline export script must check the 4th return value (`exist_face == False`) and log `NaN` or `None`, not zeros.
5. **What happens during blinks?**
   - MediaPipe still forces landmark fits, but the eye mask might become very small. Lines 69-71 in `gaze_estimation.py` check if `LeftEye_mask_W.shape[0] <= 10`. If so, it returns the failure tuple (zeros).
6. **What validity flags should be added?**
   - `frame_valid`: Boolean. Derived directly from the `exist_face` variable in the return tuple.

**Conclusion:** The underlying API (`gaze_estimation.py`) provides purely raw, unsmoothed vectors. This is **excellent** for biomarker research. No decoupling of smoothing is needed at the API level, only at the downstream export script level.