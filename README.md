# People Counter
A computer vision project to count people moving up and down in a video feed using YOLOv8 and the SORT tracking algorithm.

## Description
This project uses YOLOv8 (`yolov8l.pt`) to detect and count people in a video feed as they cross two predefined lines (one for "up" movement, one for "down"). It uses the SORT (Simple Online and Realtime Tracking) algorithm to track people across frames. I built this to learn object detection and tracking as part of my computer vision journey.

## How It Works
- **Detection**: The YOLOv8 model (`yolov8l.pt`) detects people in each frame of the video.
- **Masking**: A mask (`mask.png`) is applied to focus detection on a specific region (e.g., a pathway). This reduces false positives by ignoring areas outside the region of interest.
- **Tracking**: The SORT algorithm tracks detected people across frames, assigning each person a unique ID.
- **Counting**: Two lines are drawn on the video:
  - "Up" line (defined by `limitsUp = [103, 161, 296, 161]`): Counts people moving upward.
  - "Down" line (defined by `limitsDown = [527, 489, 735, 489]`): Counts people moving downward.
  When a person’s center crosses one of these lines, they’re counted, and the ID ensures they’re not counted again for that direction.
- **Visualization**: The video feed shows:
  - Bounding boxes around detected people with their tracking IDs.
  - Two red lines where counting occurs (one for "up," one for "down").
  - The total count of people moving up (in green) and down (in red) displayed on the video.
  - An overlay image (`graphics.png`) for additional visualization (e.g., a background or counter display).

## Requirements
- Python 3.x
- Install dependencies: pip install -r requirements.txt
- Key libraries: `ultralytics`, `opencv-python`, `cvzone`, `numpy`.
- YOLOv8 weights: The script uses `yolov8l.pt`, which is not included due to its size. Download it from the [Ultralytics YOLOv8 repository](https://github.com/ultralytics/ultralytics) and place it in the `Yolo-weights` folder.
- Video file: Download the sample video [here](https://drive.google.com/file/d/1xU1xGgcy9VUeXx7a6uAcLJqCINY7b5ox/view?usp=sharing) and place it in the `videos` folder as `people.mp4`.

## How to Run
1. Clone this repo
2. Navigate to the folder:
3. Install the required dependencies:pip install -r requirements.txt
4. Download the sample video and YOLO weights (see Requirements) and place them in the correct folders.
5. Run the script:
- The script uses `videos/people.mp4` by default. If you use a different video, you’ll need to create a new mask (see below).

## About the Mask
- The `mask.png` file is a binary mask (white for the region of interest, black elsewhere) specific to `people.mp4`. It focuses detection on a specific pathway to improve accuracy.
- **To Use a Different Video**:
1. Replace `videos/people.mp4` with your video file.
2. Update the video path in `PeopleCounter.py` (line: `cap = cv2.VideoCapture("../videos/people.mp4")`).
3. Create a new mask:
  - Use OpenCV to display a frame of your video.
  - Draw a white region of interest (ROI) where people should be counted (e.g., a pathway) on a black background.
  - Save the mask as `mask.png` in the project folder.
- Alternatively, modify `PeopleCounter.py` to not use a mask by removing the `cv2.bitwise_and(img, mask)` line if you don’t need one.

## Example Output
Below is an example of the people counter in action using `people.mp4`:

![People Counter Example](graphics.png)

Watch a demo video of the people counter [here](https://drive.google.com/file/d/1jevZbW2ajGglYFEK8lskSAHmHVNkCVp1/view?usp=sharing).

You can download the sample video [here](https://drive.google.com/file/d/1xU1xGgcy9VUeXx7a6uAcLJqCINY7b5ox/view?usp=sharing) to see the exact setup.

- `graphics.png`: An overlay image used for visualization (e.g., a background or counter display).
- `mask.png`: The mask used to focus detection on a specific region.

## Notes
- This is a beginner project—expect some rough edges!
- The counting lines (`limitsUp = [103, 161, 296, 161]` and `limitsDown = [527, 489, 735, 489]`) and mask are tuned for `people.mp4`. You may need to adjust these for a different video.
- The script can be modified to use a webcam by uncommenting the webcam code (`cap = cv2.VideoCapture(1)`) and commenting out the video file line.