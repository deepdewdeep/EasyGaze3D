# Coordinate System Verification

| Coordinate system | Meaning from code | Evidence | Main variables | Transform functions | Usefulness for biomarker | Risks | Confidence |
|---|---|---|---|---|---|---|---|
| World (W) | Head-relative model space | `recon_facial_shape` returns `fs_3d` prior to PnP transformation. It is standard across the 3DDFA model. | `fs_3d`, `LeftPupil_W` | `W_2_C`, `W_2_I` | **High.** Since it ignores head orientation relative to camera, gaze_W is purely head-relative (eye-in-head). | Assuming it matches absolute world scale. | High |
| Camera (C) | 3D space relative to camera lens | `W_2_C` applies `rotation_matrix` and `translation_vector` from PnP solver using camera intrinsic parameters. Z-axis faces subject. | `LeftPupil_C`, `gaze_C` | `C_2_I` | **High.** Needed for analyzing true ray intersections in physical space. | Confusing subject movement with eye movement. | High |
| Image (I) | 2D pixel space | Output of `projectPoints` | `lmk_2d`, `gaze_I` | `I_2_W` | Low. Primarily for visualization. | Not a 3D geometry. | High |

**Answers to Core Questions:**
1. **What does W mean?** It is a head-relative model space defined by the canonical 3DDFA_V2 face model.
2. **Is W truly world space?** No, it is **Head Relative Space**. PnP transforms W (head) to C (camera).
3. **What does C mean?** Camera relative 3D space.
4. **Where are rotation and translation vectors used?** In `W_2_C` and `W_2_I` to map the head-relative points into the camera's frame of reference.
5. **Are W vectors stable enough to be treated as face relative?** Yes. By definition, they exist prior to applying the head pose transformation.
6. **Which coordinate system should be exported first?** Both W and C. W isolates eye-in-head movement; C provides real-world gaze rays.