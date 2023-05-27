- Parameters interpretation
    - Values are pre-computed or estimated
    - Impact probability is the closeness of tasks
    - Human uncertainty is the expertise level of the human
- Making it efficient
    - Deriving Makespan
        - Deriving makespan from geometry, speed and assigned task
        - Makespan can also use heuristics such as human expertise and collision probability
    - Using Pareto optimization
    - If we don’t know what the human is doing yet, we can use heuristics
- Advantages of this solution
    - It can be tightly integrated with PLATINUm’s planning process
    - It provides PLATINUm with a form of risk awareness that can be used along with metrics
    - The robot can take the human’s experience level into account
- Disadvantages
    - The plan-time version can require a lot of pre-compiled data about the tasks
- Other work that can be done
    - Robot-Human communication when plan time risk is too high
    - Guidelines or automated tools for compiling data could be worked on

## Interpreting the parameters
Just like with the run time application, we're gonna start reinterpreting the risk factors in a way that makes the most sense in the plan time domain.

Intrinsic risk still depends on the task that is being carried out. Since we're using this data during the planning phase we are already aware of which tasks we're scheduling, so we can assume this value to be known at all times. Since this value should be included in the \[0,1] interval, we assign a risk value to the specific tasks based on how risky they are relative to each other, with the riskiest one having a value that is closest to 1.

Geometric risk depends on which of the possible trajectories the robot is following to carry out a given task. This is also data that the planner has to be aware of in order to find a suitable schedule. The planner knows what trajectory the robot will follow, but how will the associated risk factor be estimated? Our proposed interpretation involves using a table of precompiled data about the set of tasks, trajectories, and movement speeds available to the planner.
Specifically, the geometric risk associated to a given trajectory depends on a compile-time estimation of the operator's general working area. Once the general positioning of the human inside the HRC cell has been defined, it is possible to determine whether the trajectory is likely to intersect with a body part. It is also possible to determine which body part is subject to risk. We assigned this value based on how close the trajectory gets to the general position of the operator's head, but different strategies can be used.

Movement speed is known by the planner, since it is tasked with determining the target movement speed for each task. This value could be determined by considering how fast the robot is moving compared to its maximum allowed velocity. The final value will represent what percentage of the maximum speed the robot is currently moving at.

It is worth mentioning that geometric risk and movement speed are decided at plan time, but complete implementations of this model are supposed to implement run time safety measures to ensure that accidents still do not happen. The run time solution could be the application mentioned earlier, but it could also be a less sophisticated system that only engages in speed moderation and safety stops. In any case, the plan time estimates of geometric risk and movement speed are just indicative values and the 

## Accounting for efficiency

## Qualities

### Advantages

### Disadvantages

### Other work


