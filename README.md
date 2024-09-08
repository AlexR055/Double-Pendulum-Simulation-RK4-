# Double-Pendulum-Simulation-RK4-

Contains a simulation for a double pendulum using the Runge-Kutta 4th order method.

This simulation works by first gathering user input for constants such as rod length and pendulum mass. Next, the derivatives function calculates the equations of motion based on the current state of the system. The motion of the double pendulum is governed by non-linear differential equations. These equations describe how the angles and angular velocities of the rods evolve over time. The Runge-Kutta 4th order method is a numerical technique that that integrates differential equations over small, discrete time steps. The rk4 function iterates over the equations of motion using this method to plot the motion of the pendulum over time.

Feel free to use this simulation in order to learn and visualize a double pendulum, but please contact me if you intend to use it any other way.
