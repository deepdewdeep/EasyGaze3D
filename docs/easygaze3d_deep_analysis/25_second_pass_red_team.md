# Second Pass Red Team Attack

1. **Risk: Zeros treated as valid gaze**
   - **Where:** `gaze_estimation.py` returns `np.zeros(3)` on failure.
   - **Mitigation:** The implementation must add `extend_dict['frame_valid'] = True` inside the success path, and the caller must explicitly check this before trusting any vectors.
2. **Risk: Coordinate Confusion (W vs C)**
   - **Where:** Users analyzing Vergence using W instead of C.
   - **Mitigation:** Explicit documentation in `README` or export schema stating: "W = Head-Relative, C = Camera-Relative. Use C for real-world raycasting, use W for strabismus/eye-movement-only analysis."
3. **Risk: Incorrect Normalization**
   - **Where:** Forgetting to add `+ 1e-10` during normalization, causing division by zero on corrupted frames.
   - **Mitigation:** Strict enforcement of `v / (np.linalg.norm(v) + 1e-10)` in the implementation.
4. **Risk: Implementation breaks `demo_webcam.py`**
   - **Where:** Changing `average_gaze` signature.
   - **Mitigation:** Do not touch `average_gaze()`. Duplicate the subtraction and normalization logic directly inside `gaze_vector()` before assigning to `extend_dict`.