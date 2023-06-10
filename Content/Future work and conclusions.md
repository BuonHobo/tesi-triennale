- shows that it is possible to improve this thing
- Promising results
- Actual simulations of the plan could be run with ROS in order to show that avoiding collision is powerful
- Other ideas sprinkled in the article
- Work could be done to make the planner more reactive to the experience level of the human
- Improve the SearchStrategy so that it takes advantage of PLATINUm's temporal uncertainty capabilities
- Experiments could be tried with more than one human, maybe with different experience level
- Experiments where the risk factors are weighted according the hierarchy of severity, exposure, and avoidance
- Experiment with the possibility of idling for more than a unit of time
    - Idling as a way to avoid risk

The experiment results show that this model is a promising step towards making HRC safer and more efficient. There is still room for more extensive testing and plenty of improvements, that us or other researchers might want to work on in the future.
The simplicity and flexibility of the model allows it to be extended and applied to very specific scenarios, with all kinds of sophisticated techniques and fine tuning.

One of the most intuitive next steps is carrying out a real simulation of the experiment with ROS, in order to see what the robot actually behaves like in real life situation and how the reduced amount of safety stops impacts its efficiency.

Other ideas were sprinkled in earlier parts of the paper, and we will now list other relevant steps that could be taken to greatly improve this model.

The results show that the RiskAssessmentPlanner is not as reactive to the human expertise level as we would have liked. This could be due to the data we collected, which might not be detailed enough to let us appreciate the difference between plans with varying levels of human expertise. What first comes to mind is that we calculate the absolute number of collisions, while we could actually be measuring the amount of time where human and robot are at risk of colliding.
This could also be due to flaws in our implementation, or in the choice of our parameters and precompiled data.
Simulations could be carried out with multiple humans with different experience levels, in order to see if the planner actually makes sensible distinctions between them.

A different matter is the one related to fine-tuning of the parameters. For the scope of this project, it wasn't relevant to carry out a thorough analysis of the impact that small changes in the parameters might have. This is challenging because of the long time it takes to collect data about the experiment and more sophisticated tooling is needed. A possible next step in this regard would be weighing the risk factors according to their position in the hierarchy of severity, exposure and avoidance that we showed earlier.

A final topic worth mentioning is the level of integration with PLATINUm. Our implementation builds on top of PLATINUm but it is not as tightly integrated with it as it could be. More work could be done in exploring and documenting how PLATINUm works in order to improve our implementation's synergy with it. The first thought that comes to mind is refactoring our DDL plan generator so that it gives PLATINUm more information about the tasks, and changing the NodeRiskEvaluator so that it takes advantage of PLATINUm's innovative support of uncertainty.
Another step that might be taken towards improving our implementation is making a better use of the Idle task. As of now it is just an intermediate step between different tasks and it only lasts a unit of time. A smarter approach would be letting the robot or the human Idle for a variable amount of time in order to avoid collisions and dangerous situation. 
As an example, the robot could wait for the human to finish moving a metal cube before moving one itself, this would improve safety and avoid collisions and safety stops.

As we said, this model is promisi