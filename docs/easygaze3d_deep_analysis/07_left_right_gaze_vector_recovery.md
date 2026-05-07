# Left and Right Gaze Vector Recovery

## Can we recover them?
Yes. The vectors are independently derived in 3D (both W and C systems) immediately before the `average_gaze()` call.

## Source Evidence
In `gaze_estimation.py`:
```python
LeftPupil_C, RightPupil_C = W_2_C(LeftPupil_W, rot_mat, trans_v), W_2_C(RightPupil_W, rot_mat, trans_v)
LeftEyeballCenter_C, RightEyeballCenter_C = W_2_C(LeftEyeballCenter_W, rot_mat, trans_v), W_2_C(RightEyeballCenter_W, rot_mat, trans_v)

gaze_W = average_gaze(LeftPupil_W, RightPupil_W, LeftEyeballCenter_W, RightEyeballCenter_W)
```

## What must be changed?
We do not need to retrain any models. We only need to expose `LeftPupil_C - LeftEyeballCenter_C` (and normalized) and attach it to the `extend_dict` returned by `gaze_vector()`.
