## Project: Search and Sample Return

[//]: # (Image References)

[imagerock]: ./calibration_images/gold_rock.png
[imageBeforeThresh]: ./calibration_images/before_threshold.png




## [Rubric Points](https://review.udacity.com/#!/rubrics/916/view) 

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  

You're reading it!

### Notebook Analysis
#### 1. Run the functions provided in the notebook on test images (first with the test data provided, next on data you have recorded). Add/modify functions to allow for color selection of obstacles and rock samples.


*   Obstacles were determined by defining a color_threshold function to determine light areas as  navagable terrain and dark areas are considered obstacles.   The rgb threshold used were (160, 160, 160) to determine light areas.

figure shows before and after application of the color threshold.

![alt text][imageBeforeThresh]

* Rock Samples were indentified using the find_rocks function.  Rocks are very bright in yellow and the function employed loing for R levels above 110, B levels above 110 and G Levels below 50.

![alt text][imagerock]


#### 1. Populate the `process_image()` function with the appropriate analysis steps to map pixels identifying navigable terrain, obstacles and rock samples into a worldmap.  Run `process_image()` on your test data using the `moviepy` functions provided to create video output of your result. 


The `process_image()` function followed the following steps:
    1. `perspect_transform()`: the image from the rover camera are warped to produce a top down grid map of the terrain.  The calibration was taken from the lessons.
    2. `color_thresh()`: A color threshold is applied to the terrain image to determine navagable terrain and obstacles as discuessed above.
    3. The pixel coordinates are converted into rover coordinates and the finally world coordinates.
    4. The angle with the most areas of navegable terrain is then offered to the rover.
    5. The world coordinates of the the obstacles are colored in Red, the Navagable in Blue and the Rocks are given white dots.

    ```python    
    data.worldmap[y_world, x_world, 2] = 255  #blue channel == navagable terrain
    data.worldmap[obs_y_world, obs_x_world, 0] =255    #red channel == obstacle
    ```



Please refer to the \output\test_mapping.mp4 provided


### Autonomous Navigation and Mapping

#### 1. Fill in the `perception_step()` (at the bottom of the `perception.py` script) and `decision_step()` (in `decision.py`) functions in the autonomous mapping scripts and an explanation is provided in the writeup of how and why these functions were modified as they were.

The `perception.py` 


  The perpect_transform() as defined above converted the source image from Rover.vision_image and produced the destination coordinates
  
  The step applies a color threshold to determine navigable terrain color_theshed(warped) and rock samples employing find_rocks().

  The image pixels are converted to rover centric pixel values and then to wolrd coordinates.

  The Rover.worldmap is updated to refect the map coordinates of the obstacles, navagable terrain and rock samples.

  The decision.py implements conditionals based on the perception data.  The rover will move forward if the number of navigation angles are above or equal to the predetermined threshold, in my submission case it was set to 50 as defined by Rover.stop_forward.   The rover will steer at a clipped angle toward the mean of the navagle angles provdied.  If the number of navigable angles are below 50, the rover will stop and then turn until the number of navigable angles again reach 50 or over.



#### 2. Launching in autonomous mode your rover can navigate and map autonomously.  Explain your results and how you might improve them in your writeup.  

**Note: running the simulator with different choices of resolution and graphics quality may produce different results, particularly on different machines!  Make a note of your simulator settings (resolution and graphics quality set on launch) and frames per second (FPS output to terminal by `drive_rover.py`) in your writeup when you submit the project so your reviewer can reproduce your results.**


The simlator was run 5x and produced results with fidelity between 50 to 70 percent.  The map usually hit 80% within 200 seconds.

The simulator was ran on a windows computer at 1280x720. 

Ways to improve the results:
    The Fidelity could be improved with a better decision tree that allowed the rover to more smoothly stop and throttle without jarring the camera too much.

    A better decision tree that will plot areas to potentially hug the walls better and not cross areas that have already been mapped.

    The rover could slow as it neared samples and steer toward them rather than move at a fair clip.

    I would like to add a form of state control rather than the if statements currently used in the decision.py.

    Add functionality to stop and pick up samples.


![alt text][image3]


