# Evidence Lock Pass

This document verifies the claims from Pass 1 using exact source code evidence.

**Claim 1: EasyGaze3D detects or derives left and right pupil information separately.**
- **Status:** Confirmed
- **Evidence files:** `gaze_estimation.py`, `helpers.py`
- **Evidence functions:** `pupil_center()` in `helpers.py`
- **Relevant variable names:** `LeftPupil_W`, `RightPupil_W`, `LeftPupil_C`, `RightPupil_C`
- **Source code reasoning:** `helpers.py` (lines 142-154) explicitly defines `pupil_center` which calculates `LeftPupil_W` and `RightPupil_W` separately using distinct `LeftEye_mask_W` and `RightEye_mask_W`.
- **Uncertainty:** None.
- **Implication for implementation:** The required raw inputs for separate vectors already exist in the pipeline.

**Claim 2: EasyGaze3D estimates left and right eyeball centers separately.**
- **Status:** Confirmed
- **Evidence files:** `helpers.py`, `calibrate_features.py`
- **Evidence functions:** `eyeball_center()` in `helpers.py`, line 155
- **Relevant variable names:** `LeftEyeballCenter_W`, `RightEyeballCenter_W`
- **Source code reasoning:** `eyeball_center` calls `spherical_fitting` separately on `LeftEye_mask_W` and `RightEye_mask_W`.
- **Uncertainty:** None.
- **Implication for implementation:** Both centers are natively distinct.

**Claim 3: EasyGaze3D computes enough information to form left and right gaze vectors before averaging.**
- **Status:** Confirmed
- **Evidence files:** `gaze_estimation.py`
- **Evidence functions:** `gaze_vector` (lines ~84-85)
- **Relevant variable names:** `LeftPupil_W`, `LeftEyeballCenter_W`, `LeftPupil_C`, `LeftEyeballCenter_C`
- **Source code reasoning:** The code creates these specific variables in W and C coordinate systems right before calling `average_gaze`. Vector subtraction (Pupil - Center) yields the gaze vector.
- **Uncertainty:** None.
- **Implication for implementation:** We have all components ready.

**Claim 4: The averaging bottleneck is inside helpers.py in average_gaze.**
- **Status:** Confirmed
- **Evidence files:** `helpers.py`
- **Evidence functions:** `average_gaze` (lines 160-168)
- **Relevant variable names:** `avg_gaze`
- **Source code reasoning:** `avg_gaze = (left_gaze_norm + right_gaze_norm) / 2`.
- **Uncertainty:** None.
- **Implication for implementation:** This function intentionally drops independent data.

**Claim 5: gaze_estimation.py calls average_gaze for gaze_W and gaze_C.**
- **Status:** Confirmed
- **Evidence files:** `gaze_estimation.py`
- **Evidence functions:** `gaze_vector`
- **Relevant variable names:** `gaze_W`, `gaze_C`
- **Source code reasoning:** Lines 88-89 explicitly assign `gaze_W = average_gaze(...)` and `gaze_C = average_gaze(...)`.
- **Uncertainty:** None.
- **Implication for implementation:** Modifying `extend_dict` here is the best place to intercept the independent information.

**Claim 6: separate left and right gaze vectors are not currently exported as first class outputs.**
- **Status:** Confirmed
- **Evidence files:** `gaze_estimation.py`
- **Evidence functions:** `gaze_vector`
- **Relevant variable names:** `extend_dict`
- **Source code reasoning:** The return signature is `gaze_C, gaze_W, extend_dict, True`. Inside `extend_dict` (lines 140-145), the pipeline exports `Pupil_I` and `Pupil_W`, but it does *not* export Eyeball centers or raw separate vectors.
- **Uncertainty:** None.
- **Implication for implementation:** We must explicitly add the new derived vectors to `extend_dict`.

**Claim 7: extend_dict does or does not contain enough information to reconstruct separate vectors later.**
- **Status:** Confirmed (does NOT contain enough)
- **Evidence files:** `gaze_estimation.py`
- **Evidence functions:** `gaze_vector`
- **Relevant variable names:** `extend_dict`
- **Source code reasoning:** `extend_dict` only includes `Pupil_W` and `distance_to_camera`. It lacks the `EyeballCenter` entirely, making offline reconstruction of the gaze vectors impossible without rerunning the core logic.
- **Uncertainty:** None.
- **Implication for implementation:** We cannot just use the existing output; code modification is strictly required.

**Claim 8: average_gaze normalizes individual vectors before averaging.**
- **Status:** Confirmed
- **Evidence files:** `helpers.py`
- **Evidence functions:** `average_gaze`
- **Relevant variable names:** `left_gaze_norm`, `right_gaze_norm`
- **Source code reasoning:** Line 164: `left_gaze_norm = left_gaze / (np.linalg.norm(left_gaze) + 1e-10)`
- **Uncertainty:** None.
- **Implication for implementation:** We must copy this exact normalization logic when exporting separate vectors.

**Claim 9: the averaged vector is or is not renormalized.**
- **Status:** Confirmed (it is NOT renormalized)
- **Evidence files:** `helpers.py`
- **Evidence functions:** `average_gaze`
- **Relevant variable names:** `avg_gaze`
- **Source code reasoning:** Line 167: `avg_gaze = (left_gaze_norm + right_gaze_norm) / 2`. Line 168: `return avg_gaze`. It is not divided by its own norm.
- **Uncertainty:** Mathematically, the average of two unit vectors is shorter than 1 unless they are parallel.
- **Implication for implementation:** The `gaze_W` output has a length < 1. Separate left/right vectors will have length 1.

**Claim 10: independent left and right information is lost after averaging.**
- **Status:** Confirmed
- **Evidence files:** `helpers.py`
- **Evidence functions:** `average_gaze`
- **Relevant variable names:** `avg_gaze`
- **Source code reasoning:** Once summed, the algebraic combination is irreversible since there are infinite pairs of vectors that sum to the same average.
- **Uncertainty:** None.
- **Implication for implementation:** The independent vectors must be preserved *before* this function call.