# Left Right Identity Check

1. **MediaPipe landmark indices:** `config.py` defines:
   - `cfg.LeftPupil_2d_idx = 473`
   - `cfg.RightPupil_2d_idx = 468`
2. **MediaPipe Documentation Check (external knowledge):** In MediaPipe Iris/Face Mesh, index 468 represents the right eye (subject's right, appearing on the left side of a non-mirrored image), and 473 represents the left eye (subject's left, appearing on the right side of a non-mirrored image).
3. **Mirroring Behavior:** `demo_webcam.py` line 125 does `image = cv2.flip(image, 1)` **after** all processing and drawing is complete, purely for user comfort. This means the pipeline processes the unmirrored, native webcam frame.

**Conclusion:**
The naming "Left" and "Right" in the code explicitly maps to **Subject Left** and **Subject Right**, because MediaPipe's indices natively refer to the subject's anatomy.
- **Risk of left right swap:** Low, as long as the user doesn't physically flip the camera feed *before* passing it into `gaze_vector`.
- **Recommended schema names:** `subject_left_gaze_W`, `subject_right_gaze_W`. To maintain symmetry with the repo's internal names, `LeftGaze_W` is acceptable but must be explicitly documented as "Subject Left".