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

Intrinsic risk still depends on the task that is being carried out. Since we're using this data during the planning phase we are already aware of which tasks we're scheduling, so we can assume that this value is known at all times. 

Geometric risk depends on which of the possible trajectories the robot is following to carry out a given task. This is also data that the planner has to be aware of in order to find a suitable schedule. The planner knows what trajectory the robot will follow, but how will the associated risk factor be estimated? Our proposed interpretation involves using a table of precompiled data about the set of tasks, trajectories, and movement speeds available to the planner.

## Accounting for efficiency

## Qualities

### Advantages

### Disadvantages

### Other work


