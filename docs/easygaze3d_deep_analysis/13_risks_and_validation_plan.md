# Risks and Validation Plan

1. **Risk**: Asymmetric behavior during blinks.
   - **Validation**: Extract blink masks using MediaPipe eye contours to filter out bad frames in post-processing.
2. **Risk**: Overclaiming biomarker validity.
   - **Validation**: Cross-validate the extracted vergence angle against a high-end commercial eye tracker (e.g., Tobii) before relying on it medically.
