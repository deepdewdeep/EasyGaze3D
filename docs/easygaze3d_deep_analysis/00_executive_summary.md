# Executive Summary

This document presents a deep, role-based analysis of the EasyGaze3D repository.
The analysis is focused on evaluating the repository as a research artifact rather than a simple software package.
The core objective of this analysis is to determine the feasibility of extracting and preserving independent left and right gaze vectors. This is critical to maintain binocular synchrony, vergence, asymmetry, and other high-value biomarker information that is currently lost due to artificial synchronization (averaging) of the gaze vectors.

## Key Findings
- **Independent Gaze Vectors:** The pipeline explicitly computes the left and right pupil centers and eyeball centers in 3D. However, the `average_gaze` function immediately normalizes and averages these two vectors, discarding the independent information.
- **Feasibility of Recovery:** Independent left and right gaze vectors can be easily salvaged without retraining models. They can be exposed by extending the existing API's output dictionary.
- **Coordinate Systems:** The pipeline uses three main coordinate systems: World (W), Camera (C), and Image (I). The World coordinate frame is tied to the 3D reconstructed facial shape.

## Strategy Selection
The recommended strategy involves appending the independent left and right gaze vectors (in both W and C coordinate systems) into the `extend_dict` returned by the `gaze_vector` function. This approach preserves the existing API, avoids refactoring the `average_gaze` logic, and perfectly aligns with the goal of preserving binocular biomarkers.
