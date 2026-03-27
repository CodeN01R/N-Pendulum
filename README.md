# N-Pendulum Simulator

A real-time, interactive pendulum simulation built entirely in a single HTML file. It starts as a classic double pendulum and scales all the way up to a 100-segment chain — all driven by proper Lagrangian mechanics, not spring approximations or shortcuts.

Try it [here!](https://coden01r.github.io/N-Pendulum/)

**Author:** CodeN01R

---

## What is this?

If you've ever watched a double pendulum and wondered what happens when you add a third arm, or a tenth, or a hundredth — this is that. The simulation solves the full N-body coupled equations of motion at every frame using a generalized Lagrangian formulation and a 4th-order Runge-Kutta integrator. In plain terms: the physics are real, not faked.

At two segments you get the classic chaotic double pendulum. Crank it up to ten and it starts behaving like a whip. At fifty or more it looks like a living rope that can't decide where it wants to go. The whole thing runs in your browser with nothing to install.

## Features

### Adjustable segment count

The slider at the bottom of the screen controls how many segments the pendulum has, from 2 to 100 on an exponential scale. The title updates as you go — Double, Triple, Quadruple, and so on up to "N-Pendulum" for higher counts. Changing the count resets the chain with appropriately scaled starting angles so it doesn't immediately explode.

### Drag to position

Click and drag any bob to set the starting configuration by hand. The chain responds fluidly — segments below the one you're holding dangle and follow like a rope, while segments above get a subtle pull. This only happens during dragging; once you release and resume, the real physics take over with no residual weirdness.

### Pause and resume

The pause button freezes the simulation in place and switches to "Resume" so you know the state at a glance. Dragging a bob automatically pauses the sim so you can position things carefully. The simulation only unpauses when you explicitly hit Resume — no surprises.

### Time control

The time slider in the top-right scales simulation speed from 0.05× (near-frozen slow motion) up to 2× (double speed). This directly scales the integration timestep, so slow motion isn't just dropping frames — it's actually computing the physics at finer resolution, which means it's more accurate too.

### Fading trail

The tip of the pendulum leaves a trail as it moves, colored with a gradient that fades from dark at the tail to bright blue at the head. The canvas uses a semi-transparent overlay each frame instead of a hard clear, giving the trail a natural afterglow as it decays.

### Trail modes

Two trail modes are available via the "Trail: Tip / Trail: All" toggle:

- **Tip** traces only the last bob, which is the default and gives you the classic chaotic path visualization.
- **All** traces every joint in the chain simultaneously, each in its own color. This is where things get visually interesting — you can see exactly how the motion of each link differs from its neighbors and how the chaos propagates down the chain.

The trail can also be turned off entirely with the "Trail: On/Off" button. When trails are off, the canvas switches to a hard clear for a clean look with no ghosting.

### Live HUD

The bottom of the screen shows real-time readouts for the first and last segments: their angles (θ₁ and θₙ) in degrees and angular velocities (ω₁ and ωₙ). Useful if you want to get a quantitative sense of what's happening.

## How the physics work

The simulation uses the generalized Lagrangian for an N-link pendulum. For each pair of segments i and j, the mass matrix entry is:

```
M[i][j] = L² · cos(θᵢ − θⱼ) · μ(i,j)
```

where μ(i,j) is the total mass hanging from the lower of the two links. The force vector accounts for Coriolis/centripetal coupling between all pairs and gravitational torque on each segment. Every frame, this N×N system is solved via Gaussian elimination with partial pivoting to get the angular accelerations, which are then integrated using RK4.

For large N, the timestep shrinks adaptively and a light velocity damping prevents numerical energy drift from accumulating into a blowup. If the integrator ever produces a NaN (which can happen at very high N with fast time scales), it catches it and resets gracefully instead of crashing.

## Who is this for?

- **Students** learning about Lagrangian mechanics, chaos theory, or numerical methods. Being able to drag the pendulum into a configuration and watch it evolve makes the concepts tangible in a way that textbooks can't.
- **Teachers** who want a live classroom demo. The segment slider is great for showing how complexity emerges — start at two, slowly add more, and let the class watch the transition from simple oscillation to full chaos.
- **Anyone** who just thinks chaotic systems are mesmerizing to watch. Turn the trails on, crank it to as many segments as you like, and let it run. (note, performance might get worse with higher segments 😂)

## Running it

Open `index.html` in any modern browser. That's it. No build step, no server, no dependencies.

## License

This project is source-available under a custom license. You're free to read the code and learn from it, but you may not modify, redistribute, or use it in your own projects. See [LICENSE](LICENSE) for the full terms.

---

*Built by CodeN01R*
