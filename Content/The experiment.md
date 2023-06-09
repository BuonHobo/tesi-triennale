## Objective
In order to assess the effectiveness of our model and its implementation, we decided to carry out an experiment. The experiment we're about to describe is inspired from the ShareWork mosaic [[Faroni2020]] and is meant to showcase the most relevant features of our framework.

The first relevant feature is our planner's ability to sensibly assess the risk of a situation using our model. This would prove the effectiveness of our model and enable us and other researchers to build on these foundations and come up with new experiments and implementations that could further expand the possibilities of HRC.

Then, the experiment should show that our implementation is effective at using its risk awareness to strike a good balance between risk and efficiency. As we said earlier, efficiency plays a crucial role in HRC and no progress can truly be made without taking it into account.

A different property that the experiment is meant to show is our planner's ability to make good choices, and to come up with better plans when it is given more freedom. It would also be interesting to see what impact this freedom has on the planning time and measure the planner's scalability.

Finally, we want to see how the robot reacts to different human expertise levels. A smart collaborative robot should be aware of who it is collaborating with and act accordingly.

### The rules of the experiment
The experiment is based on a series of Pick And Place operations, which is the most common kind of operation in HRC manufacturing. The robot and the human must collaborate to sort a set of cubes from a common working area into separated heaps, based on the cube's material.

There are three kinds of cube: foam, wood and hot metal. Each of these materials have different intrinsic risks associated with them.
Foam is safe for the robot to move because it is soft, so the possibility of accidentally hitting the human using a foam cube has a low severity.
Wood is firm and it is a riskier to move, since hitting the human with a wooden cube might hurt them, especially if there is no personal protective equipment.
Hot metal cubes are very risky for the robot to move, because hitting the human with it would almost probably cause injuries.

Since there are only Pick And Place operations, and the same robot is doing them, this means that the intrinsic risk of a given task only depends on what kind of cube is being moved.
Each Pick And Place task can be carried out with one of three different trajectories and one of three different speeds.
The shortest trajectory is one that moves the cube over the working area and it could collide with the operator's head. This trajectory is the fastest one but it is also the riskiest one.
There is also a trajectory that could collide with the human's chest, so it is safer but also slower.
The last trajectory moves the cube under the working area, where it is slower and safer as it could only collide with the operator's feet.

Each of these trajectories can be followed at a medium or high speed, thus making it faster but also riskier.

In the experiment there is only one human with one of two levels of experienced that are the same for the whole duration of the plan. The expertise level can be either high or low.

All these rules are meant to make the planner face hard decisions where it has to choose between prioritizing safety or efficiency, so we can see what kind of balance it finds.

## Setting up the experiment
The first thing we did was defining a sensible set of precompiled data which would be fed into the algorithm. A good set of values is crucial in getting the best results out of a planner.
![[Pasted image 20230603123336.png]]
The image above shows what the precompiled data looks like, and how it reflects the risk estimation that we intuitively and implicitly do as humans.

The strategies we tested for this experiment are RiskAssessmentSearchStrategy, RiskSearchStrategy, MakespanStrategy, and the blind depth first one, which PLATINUm calls PlannerSearchStrategy.

In order to have an accurate depiction of the quality of our tools, we decided to make many variations of the experiment with varying properties and see how the final result changes. We decided that each experiment should have 6 cubes of each kind, because having less would reduce the amount of choices that the planner has to take and having more would make the planner too slow.

In the different variations, there is a varying amount of shared tasks between the human and the robot. There are variations where 0 tasks are shared and both the human and the robot have been allocated 3 cubes of each kind. This is the lowest decision freedom that the planner had to work with, we went from 0 up to 6, in steps of 2.

For each of the variations with different amounts of shared cubes, there are 2 cases: one where the human is experienced and one where the human is unexperienced. This allows us to see how the planners react to every combination of parameters.

There are 2 expertise values and 4 shared amounts, so there are 8 plan variations. Each of the 4 tested strategies will run 5 times per variation, for a total of 160 runs.

The best way to know which strategy behaves best is to evaluate their plans using the NodeRiskEvaluator class mentioned earlier. The final dataset will contain all the data points calculated by the evaluator for each run.

We also tried to do the experiment multiple times, with different sets of precompiled data, we will elaborate further on this when discussing the results.

The benchmark took almost 4 hours to complete on a computer with a Ryzen 3600 CPU and 16 GB of RAM.

## Results

### Graphs
We will now show and explain the graphs made by our python tool.

![[Resources/graphs/all_data5.csv/timetoplan_strategy.png]]
This graph shows how the time to find a suitable plan changes for each strategy, and how increasing the shared amount impacts the planning time.
Planner is consistently the fastest one because it's only concerned with plan suitability and is not aware of safety or efficiency. The other strategies behave similarly, with RiskPlanner being the one suffering the most from a higher amount of shared cubes.
As we said earlier, a higher shared amount means that PLATINUm has to work with a much higher branching factor, thus taking more time to find a solution.

![[Resources/graphs/all_data5.csv/risk_makespan_shared.png]]
This scatter plot shows how each strategy finds its balance between risk and efficiency. The data points are grouped by expertise and shared values. The most relevant part of the graph is on the right, and it shows how RIskAssessmentPlanner is consistently safer than MakespanPlanner, and also faster than both Planner and RiskPlanner.
We can see that when the human is less experienced, the RiskAssessmentPlanner is aware of the increased risk and tries to make safer plans that take longer to execute as a response.
Some data points appear to be missing from the groups with fewer shared cubes, this is due to their risk value being too low to be represented in this logarithmic scale satisfyingly. To make them visible, we also included a linearly scaled version of this graph.

![[Resources/graphs/all_data5.csv/risk_makespan_shared_linear.png]]

Grouping these data points by the amount of shared cubes makes it easy to compare different strategies in similar conditions, but it is not the best to see how a strategy behaves as the amount of shared cubes changes.

![[Resources/graphs/all_data5.csv/risk_makespan_strategy.png]]
We included this version of the graph (and its respective linearly scaled version) to show how the RiskAssessmentPlanner gets better at trading safety for efficiency as it is gradually given more freedom of choice.

![[Resources/graphs/all_data5.csv/risk_makespan_strategy_linear.png]]

The Risk-Makespan based graphs do a good job at showing how the strategies behave and how RiskAssessmentPlanner find the best balance between safety and efficiency when it is given more freedom. However, a flat risk value might not be the best way to qualitatively analyze the plans that each strategy comes up with.
Fortunately, we also collected data about the allocation of tasks, so we can use that to better understand how our planners behave.

![[Resources/graphs/all_data2.csv/task_strategy.png]]
This graph shows runs where the amount of shared cubes is 6, when the planners are completely free to choose how to allocate tasks.
We can see how MakespanPlanner tries to optimize for speed and parallelism with foam and wood cubes, while it tries to move all the metal cubes. This is because the human is slower at moving metal cubes, so the planner wants to compensate by allocating most metal cubes to the robot.
The Planner makes no distinction between human, robot or different kinds of tasks. So it generally resorts to assigning most tasks to the robot, which removes any level of collaboration. This might be safer in some cases, but it defeats the purpose of HRC.
RiskAssessmentPlanner clearly tries to achieve some sort of parallelism and collaboration, but it still tries to allocate tasks in a way that makes it safer for the human. The robot is assigned less metal cubes because it is safer to have the human handle those, while foam cubes are safer and more often assigned to the robot.
RiskPlanner is extremely biased towards safety. It avoids allocating metal cubes to the robot, but it allows the robot to move most of the foam cubes.

The collected data allows us to do even more, since we know which collisions happen between robot and human.

![[Resources/graphs/all_data5.csv/collisions_strategy.png]]
This graph shows how many collisions of each kind happen with each strategy. This graph considers data with all amounts of shared cubes, and further demonstrates how RiskAssessmentPlanner is able to avoid collisions even when the allocation of tasks is heavily constrained.
MakespanPlanner generates many collisions because it tries to increase parallelism.
Planner is not aware of collisions, so it blindly generates a great deal of them.
RiskAssessmentPlanner allows foam collisions, which are the safest, but it manages to strongly avoid collisions with wood, and has almost no collisions with metal.
RiskPlanner allows foam collisions, and it managed to have exactly zero collisions of metal and wood cubes.

Now that we have this useful information about collision, we can plot this against makespan to see a different perspective on the safety-efficiency trade off.

![[Resources/graphs/all_data5.csv/collisionscore_makespan_shared.png]]
The collision score used in the y-axis is calculated by taking a weighted sum of the collisions from each run, according to how dangerous a given collision is.
We can easily see how RiskAssessmentPlanner is still consistently the best one at finding tradeoffs between risk and efficiency. It ensures a low amount of collisions, and they're mostly foam collisions, which are the safest ones.

It could be argued that, in a real life scenario, the RiskAssessmentPlanner schedules would execute faster than the MakespanPlanner ones. The reason being that the dramatically lower amount of collisions would translate to a much lower amount of safety stops, which are the main cause of HRC slow-downs. This means that risk aware planning is not only a great way to improve safety in the HRC working environment, but it can also be the best way to improve efficiency as well.

It is clear how a greater amount of freedom results in better plans at the cost of higher planning time, which is then paid back with potentially more efficient plans.

It can also be noted how the planners struggle a bit to avoid collisions with an inexperienced human. This could be due to the fact that, in these simulations, the inexperienced human takes longer to accomplish tasks and the robot is not able to wait for it to finish, so it could get in the way.

These results are also a testament of how powerful fine-tuning can be when looking for better plans, as three different datasets were collected with different parameters before this one. They generally showed the same properties, but fine-tuning allowed us to decrease the number of collisions. Even better results would probably be achieved with more fine-grained data and refined parameters. 