# Red Map and Warning Map

## Red Map (Failure Modes)
1. **Left/Right Swap**: Confusion between image-left and subject-left.
2. **Kalman Filter Smoothing Destroying Synchrony**: The demo applies Kalman filtering to `gaze_I_seq`. Doing this individually to left/right vectors might introduce artificial phase delays if not careful.
3. **Invalid Frames Treated as Valid**: Blink frames or Mediapipe failures might yield default zeros or frozen vectors.
4. **Coordinate Ambiguity**: Exporting vectors without stating if they are in C or W coordinates.
5. **No Face Detected Silently Failing**: `exist_face` false branch returning `np.zeros` which could be confused with a valid vector.

## Warning Map
- **Warning**: Missing Normalization.
- **Why it matters**: `average_gaze` normalizes the individual vectors before averaging, but the average itself is not renormalized. If returning independent vectors, we must ensure they are properly normalized.
