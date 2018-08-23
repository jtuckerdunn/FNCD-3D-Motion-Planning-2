## Project: 3D Motion Planning
![Quad Image](./misc/enroute.png)

---


# Required Steps for a Passing Submission:
1. Load the 2.5D map in the colliders.csv file describing the environment.
2. Discretize the environment into a grid or graph representation.
3. Define the start and goal locations.
4. Perform a search using A* or other search algorithm.
5. Use a collinearity test or ray tracing method (like Bresenham) to remove unnecessary waypoints.
6. Return waypoints in local ECEF coordinates (format for `self.all_waypoints` is [N, E, altitude, heading], where the drone’s start location corresponds to [0, 0, 0, 0].
7. Write it up.
8. Congratulations!  Your Done!

## [Rubric](https://review.udacity.com/#!/rubrics/1534/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  

You're reading it! Below I describe how I addressed each rubric point and where in my code each point is handled.

### Explain the Starter Code

#### 1. Explain the functionality of what's provided in `motion_planning.py` and `planning_utils.py`
motion_planning.py is simialar to the backyard_flyer.py implementation with some added functionality. There is an added planning state and an added plan_path function which feeds waypoints to self.waypoints. planning_utils.py creates a 2d grid representation of the configuration space, creates an action class with all possible drone actions, and has a star and heuristic functions which search the grid.

And here's a lovely image of my results (ok this image has nothing to do with it, but it's a nice example of how to include images in your writeup!)
![Top Down View](./misc/high_up.png)

Here's | A | Snappy | Table
--- | --- | --- | ---
1 | `highlight` | **bold** | 7.41
2 | a | b | c
3 | *italic* | text | 403
4 | 2 | 3 | abcd

### Implementing Your Path Planning Algorithm

#### 1. Set your global home position
Here I read the first line of the csv file, extract lat0 and lon0 as floating point values and use the self.set_home_position() method to set global home. This was done with the following code:

        with open('colliders.csv') as colliders:
            latlon = colliders.readline().strip().split(',')
            start_lat, start_lon = float(latlon[0].strip().split(' ')[1]), float(latlon[1].strip().split(' ')[1])


        self.set_home_position(start_lon, start_lat, 0)

#### 2. Set your current local position
I found my current local position by first retrieving my global position and converting that using global_to_local relative to my home position.


#### 3. Set grid start position from local position
I set the grid start poition using the code:

grid_start = (int(current_local_position[0]-north_offset), int(current_local_position[1]-east_offset))

#### 4. Set grid goal position from geodetic coords
I used random lat and lon which were on the map to set my goal location using the code below.

        global_goal = np.array([-122.396214, 37.795150, 0])
        local_goal = global_to_local(global_goal, self.global_home)
        grid_goal = (int(local_goal[0]-north_offset), int(local_goal[1]-east_offset))


#### 5. Modify A* to include diagonal motion (or replace A* altogether)
I added diagonal motion to the A* implementation in planning_utils as it was implemented in the receding horizons exercise.

#### 6. Cull waypoints
I used the colinearity test which was implemented in the Putting it together exercise to cull the waypoints. This was done using the prune_path function which is implemented in planning_utils.py



### Execute the flight
#### 1. Does it work?
It works!
