# System Architecture & Algorithms

## 1. Vision Processing (`vision_features`)
The standard single-point Blob Analysis block was replaced with a custom raw-pixel matrix script. The camera output ($120 \times 160$) is processed to output 6 critical variables:
* `n (4x1 array)`: Pixel counts in the top, right, bottom, and left edge regions. Used to detect upcoming corners.
* `c (4x1 array)`: Cross-track error in each of the 4 directions.
* `area`: Total red pixel count.
* `border`: A 3-pixel thick perimeter ring. If `border == 0`, the target is fully isolated inside the camera frame.
* `ex, ey`: The $X$ and $Y$ centroid coordinates of the red blob relative to the image center.

## 2. Flight Logic & Control Workflow (`flight_logic`)
The flight controller operates without altering the drone's `Yaw`. Instead, it uses a directional toggle mode:

* **Line Following Mode:** The drone evaluates `n` (pixel counts). If a massive block of white pixels fills the left or right window, it snaps its travel direction to that side. It then uses a PI controller (`Kp = 0.002`, `Ki = 0.00005`) on the cross-track error `c` to apply lateral strafing speed, keeping the camera perfectly centered.
* **Gap Coasting Mode:** If `n` drops below 15 pixels in the direction of travel, the PI controller is bypassed. The drone maintains a forward coasting velocity of `0.08` to clear the track break safely.
* **Target Hunting & Landing:** If the drone detects an isolated circle (`area > 150` and `border < 3`), it immediately overrides the line-following logic. It switches to a secondary P-controller (`Kc = 0.004`) to hunt the `ex` and `ey` centroid coordinates. Once both `ex` and `ey` are under 8 pixels for 3 consecutive frames, it latches the landing state and commands `Z_cmd = 0`.
