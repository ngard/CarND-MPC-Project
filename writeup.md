# MPC project
## Reflection
### The model details

I used kinematic model.
It has 4 states (x,y,psi&v), 2 actuators (delta&a).

The state of next timestep (t+1) can be approximately described as below based on states and actuators of previous timestep (t).

> x[t+1] = x[t] + v[t] * cos(psi[t]) * dt
> y[t+1] = y[t] + v[t] * sin(psi[t]) * dt
> psi[t+1] = psi[t] + v[t] / Lf * delta[t] * dt
> v[t+1] = v[t] + a[t] * dt

### Timestep Length and Elapsed Duration

N (timestep length) can be from small integer to infinite and
dt (duration between timestep) should be usually smaller than one.
Generally speaking, smaller N and bigger dt is preferred because larger N and smaller dt result in long computation time and make no benefit.
On the other hand, too small N and too big dt result in prediction failure.
The product N times dt is also important because it is the length of total prediction and that should be longer than motion planning range but should not exceed the range of available map.

This time I set dt to 0.1 which is equal to the latency of the vehicle and set N to 20 because N*dt will be 2.0 seconds which is sufficient for moderate driving.

### Preprocesses on waypoints

Available coordinates of waypoints are global coordinate in this project.
However, it is not intuitive and inconvenient for displaying the dots.
Therefore, I applied homogeneous transformation to transform into vehicle coordinate system.

### Latency

I simply offset latency by looking ahead of incoming control variables.
As my MPC output N sequential control variables, I just used the one with 100ms (1dt) delay.