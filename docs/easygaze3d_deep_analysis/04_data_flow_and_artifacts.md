# Data Flow and Artifacts

| Data Item | Shape/Type | Coordinate System | Producer | Consumer | Saved | Exported | Useful for Biomarkers | Lost after Averaging |
|---|---|---|---|---|---|---|---|---|
| `fs_3d` | (38365, 3) | World (W) | `recon_facial_shape` | `detect_lmk_3d`, `gaze_vector` | Yes | No | No | No |
| `lmk_2d` | (468, 2) | Image (I) | `detect_lmk_2d` | `head_pose` | No | No | No | No |
| `lmk_3d` | (468, 3) | World (W) | `detect_lmk_3d` | `head_pose` | Yes | No | No | No |
| `rot_mat` | (3, 3) | Transformation | `head_pose` | Coordinate converters | No | No | Yes (head tracking) | No |
| `trans_v` | (3, 1) | Transformation | `head_pose` | Coordinate converters | No | No | Yes (head tracking) | No |
| `LeftPupil_W` | (3,) | World (W) | `pupil_center` | `average_gaze` | No | No | Yes | **Yes** |
| `RightPupil_W` | (3,) | World (W) | `pupil_center` | `average_gaze` | No | No | Yes | **Yes** |
| `LeftEyeballCenter_W` | (3,) | World (W) | `eyeball_center` | `average_gaze` | Yes | No | Yes | No |
| `RightEyeballCenter_W` | (3,) | World (W) | `eyeball_center` | `average_gaze` | Yes | No | Yes | No |
| Left Gaze Vector | (3,) | W / C | Internal to `average_gaze` | `average_gaze` | No | No | **Yes** | **Yes** |
| Right Gaze Vector | (3,) | W / C | Internal to `average_gaze` | `average_gaze` | No | No | **Yes** | **Yes** |
| Averaged Gaze | (3,) | W / C / I | `average_gaze` | Rendering | No | Yes | Limited | N/A |
