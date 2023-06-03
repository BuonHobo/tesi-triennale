- Objective of the experiment
    - Based on the ShareWork mosaic experiment
    - Showcase the planner’s capability to find tradeoffs between risk and makespan
    - Show how the solutions improve as the planner has more freedom
- The rules of the experiment
    - The 3 cubes
    - 3 speeds
    - 3 trajectories
    - experienced/unexperienced human
    - both can handle each type of cube (with varying constraints)
    - cubes are in a common working area
    - each type of cube is stacked in a different place
- Example of pre-compiled data
- Results from the experiment
    - Gathering detailed data proved crucial in determining the quality of the model
        - What runs we did
            - how many of each kind
            - Changed shared value
            - Changed expertise value
            - only 6 cubes for each type (too slow)
        - Runs with different risk values
        - Tested strategies
            - Risk
            - Makespan
            - Blind
            - RiskAssessment
    - PC specs/overnight run
    - Final graphs
        - time to plan
            - Blind is the fastest because it only cares about feasibility
            - more shared, more time
        - risk-makespan (2)
            - shared value has great impact on plan quality
            - expertise value increases risk, and robot tries to go slower
            - the amount of data allows for finer tuning
        - We need a better metric to appreciate the plan quality, risk value is not enough
            - Let’s see tasks
        - task allocation
            - shared=6
            - Makespan assigns a lot of tasks to the robot because it’s faster
                - A lot of metal because the human is slower at moving metal
            - Planner blindly assigns almost everything to the robot
                - defeats the purpose
                - a robot working alone is not a threat
            - RiskAssessment avoids assigning too many woods and metals to the robot, while foam is safer
            - Risk tries to avoid metal and wood
        - The data allows us to do more, we can see collisions
        - possible collisions
            - Makespan has many collisions because it goes for parallelism
            - Planner is blind and it is not aware of collisions
            - RiskAssessment was able to avoid metal and wood collisions even when it had constraints
                - it’s more relaxed with foam
            - Risk avoids every collision that is not foam
        - collision score-makespan
            - We still have confirmation of how much better the RiskAssessment is
    - Increasing freedom gives better plans at the cost of planning time
    - Failed iterations of the experiment with bad graphs
    - Considerations about the expertise value
        - Human takes more time to do tasks, so there are more collisions
        - Planner struggles to avoid collisions with an incompetent human
    - Avoiding collisions is great for both safety and efficiency
	- The MakespanPlanner doesn’t account for collisions, but collisions increase makespan
	- Probably more than the RiskAssessmentPlanner

## Objective
In order to assess the effectiveness of our model and its implementation, we decided to carry out an experiment. The experiment we're about to describe is inspired from the ShareWork mosaic [[Faroni2020]] and is meant to showcase the most relevant features of our framework.

The first relevant feature is our planner's ability to sensibly assess the risk of a situation using our model. This would prove the effectiveness of our model and enable us and other researchers to build on these foundations and come up with new experiments and implementations that could further expand the possibilities of HRC.

Then, the experiment should show that our implementation is effective at using its risk awareness to strike a good balance between risk and efficiency. As we said earlier, efficiency plays a crucial role in HRC and no progress can truly be made without taking it into account.

A different property that the experiment is meant to show is our planner's ability to make good choices, and to come up with better plans when it is given more freedom. It would also be interesting to see what impact this freedom has on the planning time and measure the planner's scalability.

Finally, we want to see how the robot reacts to different human expertise levels. A smart collaborative robot should be aware of who it is collaborating with and act accordingly.

### The rules of the experiment
The experiment is based on a series of Pick And Place operations, which is the most common kind of operation in HRC manufacturing. 