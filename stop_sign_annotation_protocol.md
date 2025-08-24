# Stop Sign Annotation Protocol
The purpose of this document is to provide clear and comprehensive guidelines for annotating stop sign behavior videos collected by dashcam footage, ensuring that annotators understand key terms, consistently identify traffic events, and apply labels accurately across all scenarios.

---


## Common Terms
This section defines key terms used throughout the annotation guide to ensure consistent understanding and labeling across all annotators.

- **Ego Vehicle:** The car from which the video is being captured.

- **Stop Line:** The white painted line on the road where vehicles are expected to stop. If there is no line, it is the imaginary line just before the crosswalk or intersection.

- **Junction / Intersection:** The area where two or more roads meet and vehicles must navigate crossing or merging traffic.

- **Cross Traffic:** Vehicles moving on the road perpendicular to the ego vehicle at the junction.

- **Right of Way:** The legal right for a vehicle to proceed before other vehicles in a traffic scenario.

- **Occlusion:** Any obstruction (parked cars, trees, etc) that partially or fully blocks the view of the stop sign, junction, or relevant vehicles.

- **Vehicle Ahead:** A vehicle directly in front of the ego vehicle along the same  path.

- **Collision:** A collision is any physical contact between the ego vehicle and another object (vehicle, pedestrian, etc), causing physical contact or damage.

---
## General Notes for Annotators
1. When annotating, make sure to save all entries in a .csv file. Each row should represent one video.
2. Most of the videos potentially involve stop signs. If a video does not contain a stop sign, please record its ID in a separate file named `videos_without_stop_signs.csv`. No annotation is required for these videos.
3. Use visual judgment only - exact measurements are not required.  
4. Base your annotations only on what you can see in the video - do not assume events off-screen or behind occluded objects.
5. Refer to the “Common Terms” section to understand field definitions, such as Ego Vehicle, Stop Line, etc.


---
## Annotation Instructions
This section defines all the labels that are required for annotation and provides guidance on how they should be applied.

### 1. Video Id
- **Field:** `video_id`  
- **Definition:** The unique id of the video that is being annotated (could be extracted from the filename, e.g "893f22ef-668d-40c3-8715-7b92ab8d952a").

---

### 2. Time of Day
- **Field:** `time_of_day`  
- **Values:** `DAY` / `NIGHT` / `DUSK` / `DAWN`  
- **Definition:** The approximate time of day when the video was captured, as can be observed visually.
  - **DAY:** Sun is well above the horizon (daytime), regardless of weather or brightness.
  - **NIGHT:** Sun is below the horizon (nighttime), regardless of lighting conditions.
  - **DUSK:** The period just after sunset when light is fading.
  - **DAWN:** The period just before sunrise when light is increasing.

---

### 3. Occlusion Level
- **Field:** `occlusion_level`  
- **Values:** `CLEAR` / `PARTIAL` / `OCCLUDED`  
- **Definition:** Visibility of the junction, or relevant vehicles around a stop sign:  
  - **CLEAR:** All relevant vehicles and the junction are fully visible.  
  - **PARTIAL:** Some relevant vehicles or parts of the junction are partially blocked, so the ego vehicle can see them but not fully.
  - **OCCLUDED:** The ego vehicle cannot see the relevant vehicles or junction until it is nearly at the stop line.

---

### 4. Junction Density
- **Field:** `junction_density`  
- **Values:** `LOW` / `MEDIUM` / `HIGH`  
- **Definition:** Estimated number of vehicles near the junction.  
  - **LOW:** 0-1 vehicles  
  - **MEDIUM:** 2-3 vehicles  
  - **HIGH:** 4+ vehicles

---

### 5. Approach Speed
- **Field:** `approach_speed`  
- **Values:** `SLOW` / `MEDIUM` / `FAST`  
- **Definition:** Estimated speed of the ego vehicle as it approaches the stop sign.

---

### 6. Stop Status
- **Field:** `stop_status`  
- **Values:** `FULL_STOP` / `PARTIAL_STOP` / `NO_STOP`  
- **Definition:** How the vehicle stopped at the stop sign:  
  - **FULL_STOP:** Complete stop. 
  - **PARTIAL_STOP:** Slowed significantly but did not fully stop.  
  - **NO_STOP:** Passed without meaningful deceleration.

---

### 7. Stop Position
- **Field:** `stop_position`  
- **Values:** `AT_LINE` / `BEFORE_LINE` / `AFTER_LINE` / `OTHER`  
- **Definition:** Position where the vehicle stopped relative to the stop line:  
  - **AT_LINE:** Stopped approximately at the line  
  - **BEFORE_LINE:** Stopped before of the line  
  - **AFTER_LINE:** Stopped beyond the line  
  - **OTHER:** Any situation not captured above (e.g. `stop_status = NO_STOP`)

---

### 8. Vehicle Ahead Existence
- **Field:** `vehicle_ahead_existence`  
- **Values:** `YES` / `NO` 
- **Definition:** Indicates whether there is a vehicle directly ahead of the ego vehicle path, close to the stop sign. Only mark vehicles that are near enough to influence the ego vehicle’s stopping behavior. Vehicles far ahead or beyond the junction should not be labeled.

---

### 9. Right of Way Compliance
- **Field:** `right_of_way_compliance`  
- **Values:** `YES` / `NO` / `OTHER`  
- **Definition:** Indicates whether the ego vehicle correctly yielded to other road users while crossing the junction:  
  - **YES:** Vehicle yielded properly  
  - **NO:** Vehicle failed to yield  
  - **OTHER:** Ambiguous or complex scenario (e.g. the ego vehicle collided with the vehicle head before crossing the junction)

---

### 10. Collision Type
- **Field:** `collision_type`  
- **Values:** `COLLISION_VEHICLE_HEAD` / `COLLISION_CROSS_TRAFFIC` / `COLLISION_OTHER` / `COLLISION_AVOIDED` / `NO_COLLISION`  
- **Definition:** Type of collision or near collision event involving the ego vehicle:  
  - **COLLISION_VEHICLE_HEAD:** Impact with vehicle directly ahead  
  - **COLLISION_CROSS_TRAFFIC:** Impact with cross traffic vehicle  
  - **COLLISION_OTHER:** Impact with pedestrian, unspecified object
  - **COLLISION_AVOIDED:** Emergency maneuver prevented a collision
  - **NO_COLLISION:** No collision occurred  
  
---


# Assumptions / Simplifications
There are few assumptions and simplifications I made while creating this annotation protocol:

1. Some videos may not contain stop signs (as they are flagged by a detection algorithm).
2. Annotators have no special annotation tools and can only watch the videos.
3. Annotators are not able to annotate events at the frame level.
4. Each video contains only a single stop sign event.
5. At the beginning I considered including labels like "stop line duration" and "vehicle ahead mobility status" (static/dynamic) but omitted them since they could add some complexity without providing a substantial value.
6. Since I was asked to dedicate only a short amount of time to this task, I did not include images and edge case illustrations in the document. However this information is generally important and can help reduce annotation mistakes.


# Design Explanation
The fields I chose in the annotation schema are designed to capture key features of stop sign scenarios,
including scene complexity, vehicle behavior and interactions with other road users.
The combination of vehicle behavior (e.g approach_speed, stop_status, stop_position),
scene complexity (e.g. occlusion_level, time_of_day, junction_density), and special events indication like collision_type
can help the model predict various types of potential risks/collisions.
Few attributes such as right_of_way_compliance, stop_status can also help the model predict driver yielding behavior.
These annotations allow the model to learn how environmental factors and driver behavior are related,
improving its ability to detect risky scenarios and classify compliance.