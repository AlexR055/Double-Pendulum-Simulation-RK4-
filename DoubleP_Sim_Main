import matplotlib
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.animation as animation
from time import sleep
matplotlib.use('TkAgg')

print("Welcome to the double pendulum simulation.")
sleep(2)
print("This simulation uses the Runge-Kutta 4th order method for numerical integration of the equations of motion.")
sleep(2)
print("Be cautious about putting extremely unbalanced masses on the bobs, as this can overload the sim.")
sleep(2)

# Gather constants
g = 9.81  
while True:
    try:
        L1 = float(input("What is the length of the first rod, in meters? "))  
        L2 = float(input("What is the length of the second rod, in meters? ")) 
        m1 = float(input("What is the mass of the first bob, in kg? "))  
        m2 = float(input("What is the mass of the second bob, in kg? ")) 
        damping = float(input("What is the damping coefficient? ")) 
        break
    except ValueError:
        pass

# Time settings
dt = 0.01  # time step (s)
t_max = 100  # maximum simulation time (s)

# Initial conditions (angles in radians, angular velocities in rad/s)
theta1 = np.pi / 2  # initial angle of the first pendulum
theta2 = np.pi / 2  # initial angle of the second pendulum
omega1 = 0.0  # initial angular velocity of the first pendulum
omega2 = 0.0  # initial angular velocity of the second pendulum


# Function to calculate the derivatives
def derivatives(state):
    theta1, omega1, theta2, omega2 = state

    delta_theta = theta2 - theta1
    denominator = (m1 + m2) * L1 - m2 * L1 * np.cos(delta_theta) ** 2

    # Angular accelerations
    domega1 = ((m2 * L1 * omega1 ** 2 * np.sin(delta_theta) * np.cos(delta_theta) +
                m2 * g * np.sin(theta2) * np.cos(delta_theta) +
                m2 * L2 * omega2 ** 2 * np.sin(delta_theta) -
                (m1 + m2) * g * np.sin(theta1)) /
               denominator)
    domega2 = ((-m2 * L2 * omega2 ** 2 * np.sin(delta_theta) * np.cos(delta_theta) +
                (m1 + m2) * g * np.sin(theta1) * np.cos(delta_theta) -
                (m1 + m2) * g * np.sin(theta2)) /
               (L2 / L1 * denominator))

    # Damping effect
    domega1 -= damping * omega1
    domega2 -= damping * omega2

    return np.array([omega1, domega1, omega2, domega2])


# Function to solve the equations of motion using the 4th-order Runge-Kutta method
def rk4(state, dt):
    k1 = derivatives(state)
    k2 = derivatives(state + 0.5 * dt * k1)
    k3 = derivatives(state + 0.5 * dt * k2)
    k4 = derivatives(state + dt * k3)
    return state + (dt / 6.0) * (k1 + 2 * k2 + 2 * k3 + k4)


# Animation code
fig, ax = plt.subplots()
ax.set_xlim((-L1-L2)*1.2, (L1+L2)*1.2)
ax.set_ylim((-L1-L2)*1.2, (L1+L2)*1.2)

line, = ax.plot([], [], 'o-', lw=2)

# Adds timer to the plot
time_text = ax.text(0.05, 0.9, '', transform=ax.transAxes, fontsize=12)
current_time = 0.0


def init():
    line.set_data([], [])
    time_text.set_text('')
    return line, time_text


def animate(i):
    global theta1, omega1, theta2, omega2, current_time
    state = np.array([theta1, omega1, theta2, omega2])

    # Advance by one time step
    state = rk4(state, dt)

    theta1, omega1, theta2, omega2 = state

    # Calculates the positions of the bobs
    x1 = L1 * np.sin(theta1)
    y1 = -L1 * np.cos(theta1)
    x2 = x1 + L2 * np.sin(theta2)
    y2 = y1 - L2 * np.cos(theta2)

    # Update the line data
    line.set_data([0, x1, x2], [0, y1, y2])

    # Update the current time and timer text
    current_time += dt
    time_text.set_text(f'Time: {current_time:.2f} s')

    return line, time_text


# Initializes the animation
ani = animation.FuncAnimation(fig, animate, frames=int(t_max / dt), interval=dt * 1000, init_func=init, blit=True)

plt.show()
