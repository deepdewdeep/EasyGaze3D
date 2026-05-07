# Five Intervention Options (Refined)

### Option 1: Minimal Dictionary Export (SELECTED)
Modify `gaze_vector` to calculate explicitly normalized left and right gaze vectors (in both W and C) and append them to `extend_dict`, alongside a `frame_valid` boolean flag.
- **Pros**: Zero-risk to legacy code, requires no refactoring of `average_gaze`, proven by evidence trace.
- **Cons**: None identified.

### Option 2: New Separate Gaze Helper
Create `separate_gaze()` in `helpers.py`.
- **Pros**: Clean code architecture.
- **Cons**: Requires touching `helpers.py`, slightly higher risk.

### Option 3: Coordinate Rich Export Mode
Add a boolean flag in `config.py` like `cfg.export_independent_gaze`.
- **Pros**: Very explicit.
- **Cons**: Unnecessary configuration overhead.

### Option 4: Offline Video Export Script
Create a new script entirely.
- **Pros**: Perfectly aligns with research goals.
- **Cons**: Beyond the scope of fixing the core API bottleneck.

### Option 5: Face Relative Binocular Pipeline
Create a wrapper class.
- **Pros**: High-level.
- **Cons**: Requires substantial new code.