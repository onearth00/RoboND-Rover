## Project: Search and Sample Return
Ray Tang

04/28/2018

---

**The goals / steps of this project are the following:**  

The goals of the project is to navigate a rover in autonomous mode, and search and retrieve as many golden rocks (up to 6 in each run) as possible.

**Training / Calibration**  

* Download the simulator and take data in "Training Mode"
* Test out the functions in the Jupyter Notebook provided
* Add functions to detect obstacles and samples of interest (golden rocks)
* Fill in the `process_image()` function with the appropriate image processing steps (perspective transform, color threshold etc.) to get from raw images to a map.  The `output_image`  in this step demonstrate that the mapping pipeline works.
* Use `moviepy` to process the images in the saved dataset with the `process_image()` function.  Include the output video as part of the submission.

**Autonomous Navigation / Mapping**

* Fill in the `perception_step()` function within the `perception.py` script with the appropriate image processing functions to create a map and update `Rover()` data (similar to what you did with `process_image()` in the notebook). 

* Fill in the `decision_step()` function within the `decision.py` script with conditional statements that take into consideration the outputs of the `perception_step()` in deciding how to issue throttle, brake and steering commands. 

* Iterate on the perception and decision function until the rover does a reasonable (need to define metric) job of navigating and mapping.  

[//]: # (Image References)

[image1]: ./output/pickrock.png
[image2]: ./calibration_images/example_grid1.jpg
[image3]: ./calibration_images/example_rock1.jpg 


## [Rubric](https://review.udacity.com/#!/rubrics/916/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how I addressed each one. I submit my writeup as markdown.  

### Notebook Analysis
#### 1. Run the functions provided in the notebook on test images (first with the test data provided, next on data I have recorded). Add/modify functions to allow for color selection of obstacles and rock samples.

I added two functions to select obstacles (`color_thresh_obstacles`) and rock samples (`color_thresh_rock`). 

The selected pixels for obstacles is simply an inversion of navigable terrain, assuming the two are mutually exclusive and collectively exhaustive, which is largely true here. Accordingly, I define threshold as: 

above_thresh = (img[:,:,0] < rgb_thresh[0]) | (img[:,:,1] < rgb_thresh[1]) | (img[:,:,2] < rgb_thresh[2])  

THe selected pixels for golden rocks were determined by examining RGB values of a rock at `.\calibration_images/example_rock2.jpg` in an interactive matplotlib session. From there, threshold was defined as:

above_thresh = (img[:,:,0] < 210) & (img[:,:,0] > 140) & (img[:,:,1] < 180) & (img[:,:,1] > 110) & (img[:,:,2] < 30)

#### 2. Populate the `process_image()` function with the appropriate analysis steps to map pixels identifying navigable terrain, obstacles and rock samples into a worldmap.  Run `process_image()` on my test data using the `moviepy` functions provided to create video output of my result. The output video is included in the `output` folder of this submission.

### Autonomous Navigation and Mapping

#### 1. Fill in the `perception_step()` (at the bottom of the `perception.py` script) and `decision_step()` (in `decision.py`) 

`perception_step()` is a faithful replica of `process_imag()` , except that proper `Rover`properties are called (such as `Rover.img`, `Rover.worldmap`, and `Rover.vision_image`)  

In `perception_step()`, I also introduced two additional functions, `color_thresh_obstacles` and `color_thresh_rock`, to perceive non-navigable terrain and rock samples.

In `decision_step()`, I added in an addition If statement for picking up rock samples:

if Rover.near_sample and not Rover.picking_up:
        Rover.brake = 1
        Rover.send_pickup = True
          
![alt text][image1]

#### 2. Launching in autonomous mode my rover can navigate and map autonomously. It can also pick up samples when in the proximity of a rock. 


**Note: In the autonomous mode, I used screen resolution of `1024 by 768`, and graphgics quality `good`.


