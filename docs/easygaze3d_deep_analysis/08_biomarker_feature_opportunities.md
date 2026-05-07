# Biomarker Feature Opportunities

By preserving the left and right vectors, we can extract:
1. **Vergence Angle**: The angle between the left and right gaze vectors in the C system. Indicates convergence or divergence.
2. **Left/Right Angular Disagreement**: Difference in elevation or azimuth between eyes. Highlights strabismus or fatigue.
3. **Temporal Synchrony**: Delay in saccade onset between the left and right eyes.
4. **Per-eye Gaze Stability**: Fixation noise mapped independently for the left and right eyes.
5. **Head-Pose Compensated Features**: By using the W coordinate system, we inherently obtain face-relative gaze, separating eye movement from head movement.
