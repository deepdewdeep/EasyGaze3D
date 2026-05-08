# EasyGaze3D: Towards Effective and Flexible 3D Gaze Estimation from a Single RGB Camera 
## Usage

To use your own webcam for real-time gaze estimation:

1. Build NMS, Sim3DR and mesh render in 3DDFA_V2 (Thanks to this amazing work!)

   ---> cd ./TDDFA_V2/
   
   ---> sh ./build.sh

2. Calibrate subject-specific features

   ---> Set cfg.EasyCali.subj_idx
   
   ---> Set cfg.EasyCali.to_save to True (Recommend to False before getting ready to start)

   ---> Run calibration/capture_images.py (Should fixate at the camera lens center and move your head freely, then press "s" on keyboard to capture 50 images continuously)

   ---> Set camera intrinsic parameters in camera_intrinsic_params.py (Calibrate the webcam before)

   ---> Run calibration/camera_intrinsic_params.py

   ---> Run calibrate_features.py (Set cfg.EasyCali.visualize to True to check the gaze lines)

3. Real-time gaze estimation

   ---> Set cfg.eye_mask_type to simple for higher fps if needed

   ---> Set cfg.render to True for facial shape visualization if needed

   ---> Run demo_webcam.py

## Downstream upstream-sync setup (BrainGaze upstream)

This repository can be used as a **downstream** that tracks any chosen upstream repository (for example, a team-maintained upstream fork such as `BrainGazeUpstream`).

### Important GitHub platform constraint

GitHub does **not** allow changing a fork's built-in parent relationship after creation.

- If you need GitHub native **Sync fork**, create a fresh fork from your BrainGaze upstream repository.
- If you keep this repository, use the custom upstream remote + workflow approach below.

### 1) Configure local git upstream remote

Run these commands locally (replace placeholders):

```bash
git remote remove upstream 2>/dev/null || true
git remote add upstream https://github.com/<UPSTREAM_OWNER>/<UPSTREAM_REPO>.git
git fetch upstream --prune
git config remote.upstream.fetch "+refs/heads/*:refs/remotes/upstream/*"
git fetch upstream --prune
```

This tracks all upstream branches under `refs/remotes/upstream/*`.

### 2) Configure repository variables for automation

In this downstream repository, set these **Repository Variables** (`Settings -> Secrets and variables -> Actions -> Variables`):

- `UPSTREAM_OWNER` (GitHub org/user name, example: `ExampleUpstreamOrg`)
- `UPSTREAM_REPO` (your upstream repo name)
- `UPSTREAM_BRANCHES` (optional, comma-separated; leave empty to monitor all upstream branches)
- `UPSTREAM_AUTO_SYNC` (`true` or `false`, default recommended: `false`)
- `UPSTREAM_NOTIFY_ISSUE` (`true` or `false`, default recommended: `true`)

### 3) Enable notifications for your team

To ensure updates are visible in this repository:

- Watch this repository with **Custom** notifications enabled for:
  - Actions workflow runs
  - Issues
  - Pull requests
- Optionally add team notification routing (email/Slack) from your org notification settings.

### 4) Validate detection flow

1. Make a test commit in the configured upstream branch.
2. Run the `Monitor upstream changes` workflow manually (`workflow_dispatch`) in this downstream repository.
3. Confirm that:
   - an issue update is posted when branch movement is detected, and/or
   - sync PR(s) are created when auto-sync is enabled.

## Acknowledgement
The 3D face model reconstruction in this work is modified from [3DDFA_V2](https://github.com/cleardusk/3DDFA_V2).
