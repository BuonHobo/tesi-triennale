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

It is worth mentioning that geometric risk and movement speed are decided at plan time, but complete implementations of this model are supposed to implement run time safety measures to ensure that accidents still do not happen. The run time solution could be the application mentioned earlier, but it could also be a less sophisticated system that only engages in speed moderation and safety stops. In any case, the plan time estimates of geometric risk and movement speed are just target values and the plan execution doesn't have to blindly follow these directives if it means posing a risk on the human operator.

Impact probability is one of the most important values, and it represents one of the most crucial properties of the plan time application. Our proposed interpretation of impact probability makes it depend completely on the planner's knowledge of which tasks are being carried out concurrently by the human and the robot. The planner knows what tasks the human is doing while the robot accomplishes a given one. If we define what general working area is associated to a task, then we can determine how close the working areas of two tasks are going to be. This is critical when determining impact probability. We decided to estimate impact probability based on how close the human and the robot will be working together, given the knowledge of which specific tasks they're going to be carrying out at the same time. Since a single human task might not completely overlap with a robot task, and a robot task might overlap with multiple human tasks, there can be different strategies for calculating the exact value. Our approach will be discussed in later on in the paper. The planner might not always know which human tasks are scheduled along with a robot one, this is due to advanced planners such as PLATINUm making heavy use of heuristics to find better solutions, or comparable solutions in less time. In that case, best case or worst case analysis can be used to approximate the safety of tasks.

Human uncertainty is hard to know for sure during plan time, so we need a sensible strategy to estimate it. This is where one of the other features of this model can come into play: responsiveness to the human operator's experience level. We propose that human uncertainty at plan time should represent the expertise level of the human operator. This allows the planner to take into account different skill levels when assessing risk, and potentially act in a more prudent way with inexperienced operators.

## Accounting for efficiency
A planner is the best suited component to come up with a tradeoff between safety and efficiency. This is because of all the prior knowledge that the planner has, since it is the component that makes the decision it has to be generally aware of every detail. This data could be used in powerful ways, as we'll show later on.

We will be referring to the time it takes to carry out a task (or a plan) with the term 'makespan'.
Makespan data can be derived at plan time from the trajectory and movement speed data that we are also using to assess the risk value. The trajectory data includes the expected distance that the robot has to travel to accomplish a task, and the movement speed tells us how much time the robot will take to follow the complete trajectory. This is the simplest way to make an estimate of the makespan of the robot's tasks. More sophisticated techniques could be used for this, an example could be taking into account the collisions that the robot might have with the human and penalizing those tasks. 
The makespan of human tasks, on the other hand, can be estimated with data such as task type and experience level. Simpler strategies could be used to evaluate this, as well as more sophisticated ones.

The estimated makespan can be used in a Pareto optimization when comparing different nodes during the plan time search for a feasible schedule. Different strategies can be used in the Pareto optimization, with widely varying degrees of complexity. We will show the one we used later on in the paper. As a rule of thumb, they all generally start by checking dominance conditions, which indicate when a plan is both safer and more efficient. If there is no clear superiority, then we can deploy all kinds of smart decision logic to find the final plan that better resembles what we want it to be. This represents one of the most critical components of a plan time implementation of this model. The Pareto logic in use can dramatically affect the schedules that the planner will come up with.

## Qualities


### Advantages


### Disadvantages


### Other work


