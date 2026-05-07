# Coordinate Systems, Units, and Conventions

1. **World Coordinate System (W)**:
   - Origin and scale defined by the 3DDFA_V2 facial model.
   - It represents a face-relative coordinate space (head coordinates), where the face is generally centered and standard.
   - Extracting vectors in the W system effectively provides head-pose-compensated gaze vectors.
2. **Camera Coordinate System (C)**:
   - 3D spatial coordinates relative to the optical center of the camera.
   - The z-axis is pointing outward from the camera.
   - Used for computing true gaze rays interacting with the world.
3. **Image Coordinate System (I)**:
   - 2D pixel coordinates (x, y) on the image plane.

**Units**: The units of W are arbitrary based on the 3DDFA model, but `lines_intersection` and camera matrix define the scaling to real-world units (likely mm) in C.
