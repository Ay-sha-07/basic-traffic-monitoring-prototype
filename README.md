# basic-traffic-monitoring-prototype
A basic traffic monitoring and adaptive signal simulation prototype using Python and  OpenCV. 

### Screenshots:
<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/e5f124c6-0e20-4749-921a-726c335a793b" />
<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/1190f2c5-4b31-4f08-898c-6694013ba3ea" />
<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/9e09589e-e8c8-42e5-abac-8f2ec1146d4d" />
<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/0f6d9717-cdd1-4f72-8093-3d407c29146d" />
<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/a01052ea-7cfe-48d4-98e7-e39ab98dbe65" />
<img width="400" height="200" alt="image" src="https://github.com/user-attachments/assets/2abce86b-e645-46f2-9d91-f3ba3730a733" />

# Implementation Methodology

## System Overview

The developed prototype implements an intelligent traffic monitoring and adaptive signal control system using Python, OpenCV, YOLOv8, and SORT tracking. The system processes a traffic intersection video to detect vehicles, estimate traffic density, generate congestion warnings, and dynamically allocate adaptive green signal timing.

## Methodology Workflow

```text id="cv9vl0"
Traffic Video Input
        ↓
YOLOv8 Vehicle Detection
        ↓
SORT Multi-Object Tracking
        ↓
Vehicle Counting
        ↓
Traffic Density Estimation
        ↓
Congestion Detection
        ↓
Adaptive Signal Timing
        ↓
Output Visualization
```

## 1. Traffic Video Input

* A traffic intersection video was provided as input.
* The video was processed frame-by-frame using OpenCV.
* Each frame was analyzed for vehicle detection and tracking.

### Technologies Used

* Python
* OpenCV

## 2. Vehicle Detection using YOLOv8

* YOLOv8 (You Only Look Once) was used for real-time object detection.
* The model detected:

  * cars,
  * motorcycles,
  * buses,
  * trucks.

### Detection Process

* Each frame was passed to the YOLOv8 model.
* Bounding boxes were generated around detected vehicles.
* Low-confidence detections were filtered using a confidence threshold.

### Advantages

* Fast real-time detection
* High detection accuracy
* Suitable for intelligent traffic systems

## 3. Vehicle Tracking using SORT

* SORT (Simple Online Realtime Tracking) was used for multi-object tracking.
* Unique IDs were assigned to vehicles across consecutive frames.

### Purpose of Tracking

* Prevent duplicate counting
* Maintain vehicle identity
* Improve traffic density estimation

### Tracking Output

* Each vehicle received a unique tracking ID.
* Vehicle movement was tracked continuously.

## 4. Vehicle Counting and Density Estimation

* Vehicle count was estimated using tracked vehicles.
* Traffic density levels were classified into:

  * Low Density
  * Medium Density
  * High Density

### Density Logic

| Vehicle Count | Density Level |
| ------------- | ------------- |
| < 5           | Low           |
| 5–9           | Medium        |
| ≥ 10          | High          |


## 5. Congestion Warning System

* A congestion warning was generated when vehicle count exceeded the predefined threshold.

### Congestion Threshold

```text id="zvl55i"
Vehicle Count > 10
```

### Output Warning

```text id="rrv8h4"
CONGESTION ALERT!
```

## 6. Adaptive Traffic Signal Timing

The system dynamically adjusted green signal timing according to traffic density.

### Signal Allocation Logic

```text id="j7d5k5"
Adaptive Green Time = MIN_GREEN + (Vehicle Count × 2)
```

### Signal Timing Constraints

* Minimum Green Time = 10 seconds
* Maximum Green Time = 60 seconds

### Comparison with Fixed-Time Signals

| Fixed-Time Signal               | Adaptive Signal        |
| ------------------------------- | ---------------------- |
| Constant green duration         | Dynamic green duration |
| Does not adapt to traffic       | Responds to congestion |
| Inefficient during peak traffic | Better traffic flow    |
| Higher waiting time             | Reduced waiting time   |


# Demo of the Developed Code

## Demo Objectives

The demonstration shows:

* vehicle detection,
* vehicle tracking,
* density estimation,
* congestion detection,
* adaptive signal timing.

## Step 1 — Input Traffic Video

* A traffic intersection video was loaded into the system.
* The video contained multiple moving vehicles in urban traffic conditions.

## Step 2 — Vehicle Detection

* YOLOv8 detected vehicles in each frame.
* Bounding boxes were drawn around detected vehicles.

### Detected Vehicle Types

* Cars
* Motorcycles
* Buses
* Trucks

## Step 3 — Vehicle Tracking

* SORT tracking assigned unique IDs to detected vehicles.

### Example

```text id="i0f1nd"
ID: 1
ID: 2
ID: 3
```

* Tracking ensured consistent monitoring across frames.

## Step 4 — Vehicle Counting and Density Estimation

* Vehicle count was calculated for each frame.
* Traffic density was estimated based on the total number of vehicles.

### Example Output

```text id="z4p6aa"
Vehicles: 12
Density: High
```

## Step 5 — Congestion Warning Display

* When traffic density exceeded the threshold, the system displayed:

```text id="vh2lv6"
CONGESTION ALERT!
```

* This indicated heavy traffic congestion at the intersection.

## Step 6 — Adaptive Signal Timing

* Green signal timing was adjusted dynamically according to vehicle density.

### Example

```text id="gt2uh6"
Fixed Green: 20s
Adaptive Green: 34s
```

* Higher traffic density received longer green duration.

## Step 7 — Output Video Generation

* The processed frames were saved into an output video.
* The final video displayed:

  * vehicle detection,
  * tracking IDs,
  * density information,
  * congestion alerts,
  * adaptive signal timing.

# Output Observations and Findings

## 1. Vehicle Detection Performance

* YOLOv8 successfully detected multiple vehicle categories in real-time traffic video.
* Detection performance was accurate for most vehicles in normal lighting conditions.
* Bounding boxes clearly identified vehicles in each frame.

## 2. Vehicle Tracking Performance

* SORT tracking maintained unique IDs for vehicles across frames.
* Tracking reduced duplicate counting and improved traffic monitoring reliability.

## 3. Traffic Density Estimation

* The system successfully classified traffic conditions into low, medium, and high density.
* Traffic density increased significantly during crowded traffic scenes.

## 4. Congestion Detection

* Congestion alerts were generated whenever vehicle count exceeded the threshold.
* The warning system effectively identified highly congested traffic conditions.

## 5. Adaptive Signal Timing Performance

* Adaptive signal timing dynamically increased green signal duration during heavy traffic conditions.
* During low-density traffic, green signal duration remained shorter, reducing unnecessary waiting.

## 6. Fixed-Time vs Adaptive Signal Analysis

| Parameter               | Fixed-Time Signal | Adaptive Signal |
| ----------------------- | ----------------- | --------------- |
| Responsiveness          | Static            | Dynamic         |
| Traffic Adaptation      | Poor              | Better          |
| Waiting Time            | Higher            | Reduced         |
| Congestion Handling     | Inefficient       | Improved        |
| Traffic Flow Efficiency | Moderate          | Higher          |


## 7. Key Findings

* The prototype demonstrated the effectiveness of intelligent traffic monitoring using computer vision techniques.
* Adaptive traffic signal control improved responsiveness to changing traffic conditions.
* Vehicle tracking and density estimation enhanced congestion analysis accuracy.

## 8. Limitations

* The current implementation supports only a single traffic intersection.
* Reinforcement learning and swarm intelligence optimization are not yet implemented.
* Detection accuracy may decrease in highly crowded or low-visibility environments.

# Conclusion

The developed prototype successfully demonstrated:

* YOLOv8-based vehicle detection,
* SORT-based vehicle tracking,
* traffic density estimation,
* congestion warning generation,
* adaptive traffic signal timing.

The system showed the potential of intelligent traffic monitoring for improving urban traffic coordination and reducing congestion in smart transportation systems.


