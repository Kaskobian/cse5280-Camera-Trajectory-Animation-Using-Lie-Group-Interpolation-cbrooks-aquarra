# Camera Trajectory Animation Using Lie-Group Interpolation

## Overview
This project extends the ray tracer by replacing the fixed camera views with a moving camera trajectory. The camera pose is represented as a rigid transformation in SE(3), where the rotation stores the camera orientation and the translation stores the camera center. Several keyframe poses are placed around the scene, and the intermediate frames are generated with Lie-group interpolation instead of simple Euclidean interpolation.

## Trajectory Construction
The trajectory uses multiple camera keyframes around the reflective and refractive sphere scene. Each keyframe is first described with an eye position and look-at target. The code converts each eye/target pair into a 4x4 camera pose matrix. The matrix contains the camera right, up, and backward-viewing basis vectors, plus the camera center.

## Interpolation Method
For neighboring keyframes T0 and T1, the relative transform is computed as T0^-1 T1. The SE(3) logarithm map converts this relative rigid-body motion into a tangent-space twist. Intermediate poses are then generated using the exponential map:

```text
T(a) = T0 exp(a log(T0^-1 T1)), 0 <= a <= 1
```

This keeps every interpolated camera pose on the SE(3) manifold, so each frame has a valid rigid transformation and a consistent orientation.

## Euclidean Comparison
The project also computes a comparison trajectory where the camera centers are linearly interpolated and the rotations are interpolated separately. This simpler method is easier to understand, but it separates translation and rotation. The SE(3) method treats the pose as one geometric object, which is more consistent for rigid camera motion.

## Bonus Features
The implementation includes SO(3) rotation interpolation, full SE(3) interpolation, optional tangent-space perturbation, and a closed-loop style multi-keyframe path. The `--perturb` option applies a small tangent-space twist to study how perturbations affect the trajectory.

## Implementation Details
All assignment code is contained inside `raytracer.py`. The added code includes SO(3) exponential/log maps, SE(3) exponential/log maps, a camera trajectory builder, a trajectory visualization function, and animation rendering presets. Frames are saved as PNG images and the MP4 video is created automatically when `ffmpeg` is installed.

## Results and Observations
The Lie-group trajectory creates smooth camera motion around the scene while preserving valid camera poses. The trajectory plot shows the camera centers and coordinate frames. Compared with simple Euclidean interpolation, the SE(3) method is more mathematically consistent because it interpolates the full rigid-body pose on the manifold.

## How to Run
Run the project normally:

```bash
python raytracer.py
```

Then select option 5 for the recommended 1080p video output. Select option 6 for a very fast test, option 8 for CPU fallback, or option 12 for the bonus perturbed trajectory.
