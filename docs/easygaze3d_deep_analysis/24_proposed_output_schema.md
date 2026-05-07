# Proposed Output Schema (Implemented)

The `extend_dict` has been updated to strictly include:

| Field Name | Meaning | Shape | Coordinates | Normalized? | Raw? |
|---|---|---|---|---|---|
| `LeftGaze_W` | Subject left eye-in-head gaze vector | (3,) | World (W) | Yes | Yes |
| `RightGaze_W` | Subject right eye-in-head gaze vector | (3,) | World (W) | Yes | Yes |
| `LeftGaze_C` | Subject left absolute gaze vector | (3,) | Camera (C) | Yes | Yes |
| `RightGaze_C` | Subject right absolute gaze vector | (3,) | Camera (C) | Yes | Yes |
| `Pupil_W` | 3D pupil locations (Left, Right) | (2, 3) | World (W) | N/A | Yes |
| `Pupil_I` | 2D pupil pixel coords | (2, 2) | Image (I) | N/A | Yes |
| `frame_valid` | Boolean indicating successful face tracking and finite vectors | Scalar | N/A | N/A | N/A |
| `gaze_coordinate_note` | Metadata string explaining W/C coordinates | String | N/A | N/A | N/A |
| `gaze_vector_export_version` | Export schema version ('1.0') | String | N/A | N/A | N/A |

*(Note: `gaze_W` and `gaze_C` are returned via the function signature, not `extend_dict`)*
