# CarND-Controls-PID

## Dependencies

* cmake >= 3.5
 * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1(mac, linux), 3.81(Windows)
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

Fellow students have put together a guide to Windows set-up for the project [here](https://s3-us-west-1.amazonaws.com/udacity-selfdrivingcar/files/Kidnapped_Vehicle_Windows_Setup.pdf) if the environment you have set up for the Sensor Fusion projects does not work for this project. There's also an experimental patch for windows in this [PR](https://github.com/udacity/CarND-PID-Control-Project/pull/3).

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./pid`. 

Tips for setting up your environment can be found [here](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/0949fca6-b379-42af-a919-ee50aa304e6a/lessons/f758c44c-5e40-4e01-93b5-1a82aa4e044f/concepts/23d376c7-0195-4276-bdf0-e02f1f3c665d)

## Project implementation

Each “To Do” task within the code was completed. The PID class was completed to store the proportional, integral, and differential gains, as well as the corresponding error values. Initially, this sort of PID controller was implemented to control only the steering input angle as a function of the cross-track error. Initially, the gains 𝐾𝐾𝑝𝑝 and 𝐾𝐾𝑑𝑑 were manually tuned to provide a decent transient response. These two properties were initially tuned to get the car around the first curve in the track. Next, the integral gain was implemented and tune, however I found that adding this in and continuing to tune the other two gains didn’t readily impact the ability of the car to navigate around the curve after the bridge.
I suspect this was likely due to the preset constant throttle. I then implement a basic proportional-differential controller and began tuning its parameters as well. The thought was to have it drive similarly to a human: accelerate with shallow correction angles on more straight sections and decelerate with steeper correction angles around sharp turns. This approach was largely successful, but I found that over time, there would be ever increasing oscillations. From my debugging console output, it became apparent that the integral error was growing excessively over time and forcing the controller to overcorrect.
I then experimented with only PD controllers for steering and throttle. After a few attempts with manual tuning, I was able to get the car to drive itself easily around the course. By having a comparatively high proportional gain and lower differential gain the car was able to quickly slow down and correct for sudden sharp turns. I found that a ratio of 2:1 worked well and settled on gains of 𝐾𝑝=0.15 and 𝐾𝑑=0.075 for the steering controller, and 𝐾𝑝=0.3 and 𝐾𝑑=0.15 for the throttle controller.
