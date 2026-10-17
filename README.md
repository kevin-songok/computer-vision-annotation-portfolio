# Computer Vision Data Annotation & QA Portfolio

## 📌 Project Overview
This repository serves as a professional portfolio showcasing my methodology for high-quality **Video and Image Data Annotation**. It simulates real-world dataset preparation workflows for training Autonomous Vehicles, Action Recognition, and Object Detection models. 

This case study demonstrates my capacity to apply complex grading rubrics, structure annotation data formats, and resolve highly ambiguous edge cases to maximize machine learning model accuracy.

---

## 🗺️ Framework 1: Video Action & Object Labeling Schema
This framework outlines the exact taxonomy and rules applied when evaluating continuous multi-frame video datasets.

### 1. Object Classes & Attributes Defined
*   **Class: `Pedestrian`** 
    *   *Attributes:* `Is_Moving` (Boolean), `Direction` (North/South/East/West), `Occlusion_Percentage` (0-100%).
*   **Class: `Active_Vehicle`**
    *   *Attributes:* `Vehicle_Type` (Sedan/SUV/Truck/Bike), `Brake_Lights_On` (True/False).

### 2. Strict Boundary Rules
*   **Tight Bounding Boxes:** All bounding boxes must be drawn within 2 pixels of the object’s true outer margin.
*   **Handling Occlusion:** If an object is occluded by more than 85% across 5 consecutive frames, the track ID must be flagged for supervisor audit rather than guessed.

---

## 📝 Framework 2: Structured Labeling Output (JSON Simulation)
Below is a structured representation of a fully validated metadata output tracking an object through a 3-frame video clip sequence:

```json
{
  "dataset_id": "CV-VIDEO-TRAFFIC-094",
  "total_frames": 3,
  "annotator_id": "Songokkevin18",
  "quality_status": "PASSED_QA",
  "annotations": [
    {
      "frame_id": 1,
      "timestamp_ms": 0,
      "objects": [
        {
          "track_id": "ped_track_001",
          "label": "Pedestrian",
          "bounding_box_2d": {"xmin": 120, "ymin": 340, "xmax": 180, "ymax": 480},
          "attributes": {"Is_Moving": true, "Occlusion_Percentage": 0}
        }
      ]
    },
    {
      "frame_id": 2,
      "timestamp_ms": 100,
      "objects": [
        {
          "track_id": "ped_track_001",
          "label": "Pedestrian",
          "bounding_box_2d": {"xmin": 125, "ymin": 340, "xmax": 185, "ymax": 480},
          "attributes": {"Is_Moving": true, "Occlusion_Percentage": 10}
        }
      ]
    },
    {
      "frame_id": 3,
      "timestamp_ms": 200,
      "objects": [
        {
          "track_id": "ped_track_001",
          "label": "Pedestrian",
          "bounding_box_2d": {"xmin": 130, "ymin": 340, "xmax": 190, "ymax": 480},
          "attributes": {"Is_Moving": true, "Occlusion_Percentage": 15}
        }
      ]
    }
  ]
}
```

---

## ⚠️ Framework 3: Ambiguous Edge Case Resolution Log
A primary component of my roles at **Telus International** and **Mindrift** is auditing and documenting guideline deviations. Below is my systematic approach to resolving complex dataset scenarios:

| Ambiguous Scenario Event | Immediate Correct Action Protocol | Impact on Model Training |
| :--- | :--- | :--- |
| **Partial Occlusion** (e.g., A pedestrian walks halfway behind a static post). | Continue the bounding box to encapsulate the *entire implied body* if less than 50% hidden. Over 50%, split the track. | Prevents bounding box jitter and stabilizes temporal tracking. |
| **Low Light / Heavy Rain** (Visual blur prevents clear object identification). | Label the object under parent class `Obstacle_Unidentified`, append an uncertainty tag, and escalate to the project train lead. | Prevents the model from learning incorrect class features from corrupted frames. |
| **Reflective Surfaces** (A store window projects a clear reflection of a car). | Do **not** label reflections as active objects. Apply an `Environment_Reflection` mask if required by project guidelines. | Prevents dangerous ghost-object detection false positives in deployment. |
