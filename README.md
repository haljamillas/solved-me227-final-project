Download Link: https://assignmentchef.com/product/solved-me227-final-project
<br>
We have spent the past few weeks developing models and looking at controllers for vehicle motion in the plane. Now the time has come to see how all of this works on a real vehicle.  Or at least a more complete simulation for now.  This project, to be performed in your project teams, consists of developing a speed controller and two different steering controllers and evaluating the performance on simulators of varying fidelity.

You will find on Canvas a description of the path that defines 2 laps in the Searsville parking lot. The information consists of a vector of distances along the path, s, the path curvature κ at each distance, the location of each of these points on the path in terms of their East and North positions, and the local heading. The path consists of two laps around an ‘oval’ track. The straights are 24.1 m long, and each of the two turns (with a radius of 8.7 m) are 15.6 m long. The straights and arcs are joined by clothoids with a length of 11.6 m. For your convenience, the Fresnel integrals were evaluated to find the path parameters every 0.25 m along the path.

We have also included a speed profile for this path, determined by integrating backwards from the constant radius curves as we did in class.  The speed profile was determined according to the following constraints:

<ul>

 <li>The desired speed profile should start at zero speed at s = 0 and return to zero speed 3 meters before the end of the map (i.e., the end of the second lap).</li>

 <li>The longitudinal acceleration, a<sub>xdes</sub>, should not be larger than 3 m/s<sup>2</sup> or less than -4 m/s<sup>2</sup>.</li>

 <li>The lateral acceleration, a<sub>y</sub>, (calculated from V<sub>des</sub> and κ) should not exceed 4 m/s<sup>2</sup>.</li>

 <li>The combined acceleration magnitude should not exceed 4 m/s<sup>2</sup>.</li>

</ul>

Your mission is to implement controllers for the longitudinal speed and lateral dynamics, simulate these controllers and, based on your simulation results, refine your controllers so that they can run on the actual car.

All requests to simulate in this assignment refer to the bicycle model in Assignment 4 involving all six states (Ux, Uy, r, e, s, ∆ψ). <strong><em>The longitudinal equation should be updated to include drag, grade and rolling resistance terms.  </em></strong>The grade is initially zero; the other terms are:




frr = 0.015                                            C<sub>D</sub>A = 0.594 m<sup>2</sup> (at ρ = 1.225 kg/m<sup>3</sup>)

<h1>Controller Design</h1>

We developed a basic cruise control in the last assignment. Now that the car has drag and grade terms and our desired speed is changing, let’s implement something a little better to control longitudinal speed. Update your controller to include the feedforward/feedback speed controller we derived in class.  Assume grade is zero.  Interpret the speed profile provided as a desired value of U<sub>x</sub> and simulate your controller tracking this profile on a straight road to make sure your controller works as expected.  You will adjust the proportional gain in this controller to meet the project performance specifications.

Next you need to combine the speed profile and longitudinal controller with a lateral control law to fully control the vehicle around the path. Your goal is to choose controller types and gains that will produce no more than a 20 cm lateral error at the center of gravity and track the speed profile within 0.75 m/s, starting with the first turn (don’t worry about any initial speed mismatch at the start of the simulation).

Your team should choose two controllers to compare both in simulation and on the car. One of these should be a version of the lookahead controller we designed in class and simulated in Assignment 4. Keep in mind that the gains we used in that assignment may not be what you need to meet the tracking specification. The second controller can be of any type you choose. If you do not have much experience in control beyond this class and E105, a PID controller is perfectly fine and can lead to some interesting comparisons.

You will need to be sure that the controller you are developing can run on the car.  This is analogous to designing a controller to run on a particular choice of hardware or ECU, as it would in a production vehicle.  To ensure your code can run on the car, you need to keep the interface between the controller and the car exactly the same as it is in the MATLAB code provided.  The header file for your controller is given by:

[ delta, Fx ] = groupnum_controller( s, e, dpsi, Ux, Uy, r, control_mode, path)

Do not alter this header in any way.  The car will expect that it can send the values stated to your control code in exactly the form provided and receive steering and longitudinal force commands in return.  Similarly, the auto code generation function in MATLAB is limited in the functions it supports.  It will not accept functions from toolboxes (sorry, no lqr command for you) and is generally limited to functions in the following list:

<a href="https://www.mathworks.com/help/releases/R2016b/fixedpoint/ug/functions-supported-for-code-acceleration-and-code-generation-from-matlab.html">Functions Supported for Code Acceleration or C Code Generation </a><a href="https://www.mathworks.com/help/releases/R2016b/fixedpoint/ug/functions-supported-for-code-acceleration-and-code-generation-from-matlab.html">– </a><a href="https://www.mathworks.com/help/releases/R2016b/fixedpoint/ug/functions-supported-for-code-acceleration-and-code-generation-from-matlab.html">MATLAB &amp;</a> <a href="https://www.mathworks.com/help/releases/R2016b/fixedpoint/ug/functions-supported-for-code-acceleration-and-code-generation-from-matlab.html">Simulink (mathworks.com)</a>




For your controllers, give some thought to gain choice and what values make physical sense.  While in simulation it is easy to make the gain extremely high (or effectively infinite), high gains can amplify sensor noise in the real system.  Use some of your understanding of control theory to set your controller gains at reasonable levels.  In general, the lower the gain, the better, assuming you can meet your performance objective and have some margin to account for model errors or unmodeled effects.  For the longitudinal gain, make sure your controller does not produce a longitudinal acceleration (neglecting drag and rolling resistance) greater than 0.25g in response to a speed error of 1 m/s.

Once you have chosen the gains for the lookahead controller, simulate the complete system with your desired speed profile. You can use the visualization to see what the car is doing as it goes around the path and get a sense for the performance. Plot the lateral error, speed tracking error, lateral acceleration, longitudinal acceleration and total acceleration magnitude as a function of distance along the path and ensure you meet the lateral error and speed specification in simulation. If you don’t, adjust your gains.  Repeat this simulation process with your second controller until you have two controllers that satisfy the specifications with the same speed profile.




<strong> </strong>

<h1>Detailed Simulation</h1>




At this point, you have developed controllers analytically and simulated them on a simple nonlinear model.  In industry, it is then common to test your controllers on a more detailed simulation.  Your next step is to simulate your controllers on a simulation with more realistic actuator dynamics and sensor noise and see if any of these factors pose an issue.  We will shortly post MATLAB files of a more detailed simulator that you can use to test your code.  This code includes three different test modes that are designed to highlight differences between the actual car and the simpler nonlinear simulation model.







<h1>Project Review</h1>




The design review next week is an important milestone in the project and will be part of your grade for this assignment.  You should come to the review next week ready to present the following information:




<ul>

 <li>Your choice of gains for speed control and how you determined these</li>

</ul>




<ul>

 <li>Your two steering controllers, your choice of gains for these controllers and your process for setting these initial gains (we want to see some knowledge of control theory here, not just a guess)</li>

</ul>




<ul>

 <li>Your basic simulation results running each of these steering controllers and your speed controller on the Assignment 4 simulator with drag and rolling resistance added (to show you meet the design specifications)</li>

</ul>




<ul>

 <li>Simulation results of the lookahead controller running on the more detailed simulation we provide and your explanation of what physical factors might produce the differences you see from the simple simulation</li>

</ul>




You will present this on a Zoom call with a member of the teaching team and can present this in whatever form you feel will be most effective to communicate your results.




We won’t require you to have simulated your second steering controller on the more detailed simulation by the time of the project reviews but, if you do have those results, we are more than happy to discuss them.  Since you will be refining your controllers over the course of the next week, this would put you ahead of schedule.




<strong> </strong>

<h1>Refinement and Final Report</h1>




Your assignment for the following week will be to refine your controllers based on the more detailed simulation until you get performance you like with both of them.  You also need to put together a short report on your results.




Your final report will consist of two comparisons. First you will compare the results you achieved with the simpler and more detailed simulations and discuss what differences you saw.  You should describe which aspects of the more detailed simulation had the most impact on your design.  Then you will compare the performance of your two controllers, explaining which one achieved better performance experimentally and why. Additional information on the report requirements will be handed out next week.




<h1>Vehicle Testing</h1>




We will test your code on the car on Saturday, May 15, starting at about 10am.  We will have a sign-up list for times posted on Piazza before the testing.  We will live stream this over Twitch so you, your family and your friends can watch your code drive the car around the lot.