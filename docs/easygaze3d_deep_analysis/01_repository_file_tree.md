# Repository File Tree

```text
EasyGaze3D/
├── README.md
├── LICENSE
├── config.py
├── demo_webcam.py
├── calibrate_features.py
├── gaze_estimation.py
├── helpers.py
├── facial_shape.py
├── landmark.py
├── save_landmark_index.py
├── visualization.py
├── landmark_index.pkl
├── eye_contour_2d_index.mat
├── eye_mask_3d_index.mat
├── calibration/
│   └── (user-specific images and results)
└── TDDFA_V2/
    └── (third-party 3DDFA_V2 code)
```

## Classifications

1. **primary gaze estimation logic**: `gaze_estimation.py`, `demo_webcam.py`
2. **calibration capture logic**: `calibration/` scripts
3. **camera intrinsic logic**: `calibration/camera_intrinsic_params.py` (assumed based on README)
4. **subject specific calibration logic**: `calibrate_features.py`
5. **landmark detection logic**: `landmark.py`, `save_landmark_index.py`
6. **3D face reconstruction logic**: `facial_shape.py`
7. **helper math and geometry logic**: `helpers.py`
8. **visualization logic**: `visualization.py`
9. **configuration**: `config.py`
10. **third party or borrowed 3DDFA logic**: `TDDFA_V2/`
11. **binary index or model asset**: `landmark_index.pkl`, `eye_contour_2d_index.mat`, `eye_mask_3d_index.mat`
12. **generated data or expected runtime output**: `calibration/results/`
13. **notebook or demo**: None
14. **unclear**: None
