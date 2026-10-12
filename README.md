# Vehicle-detection-and-turning-movement-counting-YOLO-
Vehicle detection and turning-movement counting from CCTV junction footage. YOLO11n fine-tuned on 200 hand-labelled frames (14 local vehicle classes) using Colab GPU, then ByteTrack tracking with clicked leg lines counts left/through/right/U-turn movements per vehicle type for each leg of a 4-way junction.
Primary Data: CCTV Footage frames of Siddhartha Chowk, Pokhara (for training 14 vehicle categories including pedestrian).
# Vehicle Detection, Tracking & Turning Movement Counts using YOLOv11

A complete computer vision pipeline for **automatic turning movement counts** at a 4-leg road junction using a custom-trained YOLOv11 model.

The system detects and tracks multiple vehicle categories from CCTV footage, determines entry and exit legs, and automatically classifies movements as **Left / Through / Right / U-turn** for each vehicle category.


##  Project Overview

| Feature | Description |
|---------|-------------|
| **Object Detection** | Custom-trained YOLOv11 model on local traffic data |
| **Multi-Object Tracking** | ByteTrack tracker for consistent vehicle IDs |
| **Turning Movement Counts** | Automatic Left / Through / Right / U-turn classification |
| **4-Leg Junction Support** | Counts movements for all 12 possible movements (4 legs × 3 main turns) |
| **Category-wise Counts** | Separate counts for each vehicle type |
| **Interactive Line Drawing** | Click to define the 4 counting lines on the video |
| **CSV Export** | Results saved in structured CSV format |



##  Detected Classes

The model was trained on the following 14 classes:

| ID | Class Name              | ID | Class Name              |
|----|-------------------------|----|-------------------------|
| 0  | motorcycle              | 7  | truck                   |
| 1  | bicycle                 | 8  | mini_truck              |
| 2  | person                  | 9  | bus                     |
| 3  | car                     | 10 | mini_bus                |
| 4  | van                     | 11 | microbus                |
| 5  | tractor                 | 12 | utility_vehicle         |
| 6  | tractor_with_trailer    | 13 | autorickshaw            |

YOLO-Turning-Counts/
├── best.pt                          # Custom trained YOLOv11 weights
├── yolo11n.pt                       # Pretrained YOLOv11n (baseline)
├── lines.json                       # Saved counting lines for the junction
├── turning_counts.csv               # Output counts
├── YOLO-1.ipynb                     # Basic detection + tracking
├── YOLO-1_turning_counts.ipynb      # Full turning movement counting system
├── yolo_train.ipynb                 # Local training notebook
├── Google_colab_training.ipynb      # Training on Google Colab (GPU)
└── README.md


---

##  How Turning Movement Counting Works

1. **Define 4 counting lines** (one across each leg of the junction) by clicking on a video frame.
2. When a vehicle **crosses a line into the junction** → that leg is recorded as the **Entry Leg**.
3. When the same vehicle **crosses a line out of the junction** → that leg is the **Exit Leg**.
4. Movement is calculated from the difference between Entry and Exit leg (legs numbered clockwise):

| Exit - Entry (mod 4) | Movement |
|----------------------|----------|
| 0                    | U-turn   |
| 1                    | Left     |
| 2                    | Through  |
| 3                    | Right    |

5. Counts are maintained separately for every vehicle category.



##  Installation
#Clone the repository
git clone https://github.com/your-username/YOLO-Turning-Counts.git
cd YOLO-Turning-Counts

# Create virtual environment (recommended)
python -m venv venv
source venv/bin/activate          # Linux/Mac
# venv\Scripts\activate           # Windows

# Install dependencies
pip install ultralytics opencv-python numpy pandas


Steps:
Run the cells one by one.
If lines.json does not exist (or REDRAW_LINES = True), a window will open.
Click START and END of each of the 4 leg lines in clockwise order.
Press ENTER to accept.
The system will process the video and show live counts.
Press Q to stop early.
Run the final cell to see detailed tables and save turning_counts.csv.

Sample Output
=== Entering from Leg 1 ===
Category         Left  Through    Right   U-turn    Total
motorcycle          0        2        0        0        2
person              0        0        0        0        0
car                 0        0        0        0        0
...
ALL                 0        2        0        0        2


Model Training
Training on Google Colab (Recommended)
Use Google_colab_training.ipynb:
Trained with YOLOv11n
100 epochs on free T4 GPU


Local Training
Use yolo_train.ipynb (slower on CPU).
Dataset structure expected:
vehicle_dataset/
├── images/
│   ├── frame_00000.jpg
│   ├── frame_00001.jpg
│   └── ...
├── labels/
│   ├── frame_00000.txt
│   └── ...
└── data.yaml

## 📁 Project Structure
