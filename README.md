# CarND-Controls-PID
Self-Driving Car Engineer Nanodegree Program

---
## PID Controller Reflection

[//]: # (Image References)
[image1]: ./result/pid.png


Two PID controllers are used in this project: one for steering control and the other for speed control. 

For steering PID controller it contains three parts:

1. Proportional (P): The steering angle is proportional to the CTE, however, if only P is used, the car can easily ended up with oscillation. The larger P value, the quicker vehicle can return back to center but it's also more likely to overshoot. I tried different value and found 0.1 can be a good value. 

2. Integral (I): The sum of all CTE is stored and feed back to system to compensate the system offset. In this project there is no system offset (like wheel angle offset), so this value should set to very very small, even 0 can be accepted. Here I used 0.0001. 

3. Differential (D): The difference of previous CTE and new CTE is calculated and feed back to system. This can help to dampen the oscillation when move back to center. I tried 1.0, 2.0, 3.0, 4.0 and found they do not make so much difference. And finally I chose 3.0. 

```
  // Final hyperparameters are chosen complete manually after lots of tries 
  double Kp = 0.1;
  double Ki = 0.0001;
  double Kd = 3.0;
```

For speed PID I only used Proportional part since we don't need to control the speed so accurate. When the vehicle is turing at sharp turn and CTE is too large, we need to slow down to make it turn successfully. I set the speed to 100mph when CTE is less than 0.5, set to 80mph when CTE is less than 1.0, set to 50mph when CTE is less than 2.0 and set to 20mph when CTE is larger than 2.0. 

The car can drive at 60-80mph for most of the time, and slow down to 30-50mph at sharp turn. The max speed can reach to 91.42mph as shown in below picture. 

![alt text][image1]




## Dependencies

* cmake >= 3.5
 * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
  * Run either `./install-mac.sh` or `./install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets 
    cd uWebSockets
    git checkout e94b6e1
    ```
    Some function signatures have changed in v0.14.x. See [this PR](https://github.com/udacity/CarND-MPC-Project/pull/3) for more details.
* Simulator. You can download these from the [project intro page](https://github.com/udacity/self-driving-car-sim/releases) in the classroom.

There's an experimental patch for windows in this [PR](https://github.com/udacity/CarND-PID-Control-Project/pull/3)

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./pid`. 

## Editor Settings

We've purposefully kept editor configuration files out of this repo in order to
keep it as simple and environment agnostic as possible. However, we recommend
using the following settings:

* indent using spaces
* set tab width to 2 spaces (keeps the matrices in source code aligned)

## Code Style

Please (do your best to) stick to [Google's C++ style guide](https://google.github.io/styleguide/cppguide.html).

## Project Instructions and Rubric

Note: regardless of the changes you make, your project must be buildable using
cmake and make!

More information is only accessible by people who are already enrolled in Term 2
of CarND. If you are enrolled, see [the project page](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/f1820894-8322-4bb3-81aa-b26b3c6dcbaf/lessons/e8235395-22dd-4b87-88e0-d108c5e5bbf4/concepts/6a4d8d42-6a04-4aa6-b284-1697c0fd6562)
for instructions and the project rubric.

## Hints!

* You don't have to follow this directory structure, but if you do, your work
  will span all of the .cpp files here. Keep an eye out for TODOs.

## Call for IDE Profiles Pull Requests

Help your fellow students!

We decided to create Makefiles with cmake to keep this project as platform
agnostic as possible. Similarly, we omitted IDE profiles in order to we ensure
that students don't feel pressured to use one IDE or another.

However! I'd love to help people get up and running with their IDEs of choice.
If you've created a profile for an IDE that you think other students would
appreciate, we'd love to have you add the requisite profile files and
instructions to ide_profiles/. For example if you wanted to add a VS Code
profile, you'd add:

* /ide_profiles/vscode/.vscode
* /ide_profiles/vscode/README.md

The README should explain what the profile does, how to take advantage of it,
and how to install it.

Frankly, I've never been involved in a project with multiple IDE profiles
before. I believe the best way to handle this would be to keep them out of the
repo root to avoid clutter. My expectation is that most profiles will include
instructions to copy files to a new location to get picked up by the IDE, but
that's just a guess.

One last note here: regardless of the IDE used, every submitted project must
still be compilable with cmake and make./
