- Explanation of the five risk values
    - What they mean
        - Intrinsic risk
            - what tool/payload?
                - hot
                - powered
                - sharp
                - heavy
            - what kind of robot?
                - compliant
                - pointy parts
                - fine grained control
            - Is there any protective equipment?
                - gloves, glasses, etc
            - Force or pressure?
        - Geometry risk
            - Could the robot hit a human by following this trajectory?
            - Which part?
        - Movement speed
            - How fast is the robot moving?
        - Impact probability
            - How close is the robot to the human?
            - What is the human doing?
        - Human uncertainty
            - How experienced is the human?
            - How much is he moving?
            - How experienced is he?
            - Is he aware of the robot?
    - (0,1] interval, but could be changed
    - Heuristics can be done when we don’t know impact probability or human uncertainty
    - Each one of these values can be modified in some way
        - We have agency over the risk of the situation
- Qualities
    - It can be applied anywhere
    - Parameters can be reinterpreted to better fit a given scenario
    - Can reduce the amount of safety stops and slow downs
    - The robot can be more careful when working with unexperienced people
    - Can be fine tuned, although not very easily
        - Guidelines or automated tools for fine tuning could be worked on

## The 5 risk factors
We propose a model consisting of 5 risk factors that describe the safety characteristics of an HRC cell, thus giving collaborative robots a sophisticated risk awareness

### What they are
The intrinsic risk value is the direct translation of the severity concept. It is a value that represents how risky a given task is. The intrinsic risk value depends exclusively on properties that are only associated to the task itself and that don't change. This value does not depend on how the task is carried out, but just on which task is being carried out.
Some factors that are included in this value are:
Is the tool/payload hot, powered, sharp or heavy?
Is the robot compliant? Does it have any sharp or pointy parts?
How fine-grained is the control of the robot?
Is the human operator wearing any protective equipment such as glasses, gloves, etc.?
How much force and pressure are being applied?

The geometry risk depends on which trajectory the robot is following to carry out its task. The risk value of a trajectory is assigned based on how likely it is for that trajectory to intersect a human body part and which part it is likely to intersect. For example, a path that is likely to intersect the operator's head has a higher geometric risk than a path that might intersect the operator's foot with low probability.

Movement speed is also a risk value, and it intuitively depends on how fast the robot is carrying out its task.

Impact probability is a risk value that depends on how likely it is for the human and robot to be in each other's way. It depends on things like distance, but most importantly it depends on which tasks are being carried out at the same time. This is the value that most closely translates to exposure. Impact probability is high not only if human and robot are close together, but most importantly, it's high if they're accomplishing a strictly collaborative task.

The last risk value is human uncertainty. This value represents the concept of avoidance (along with geometry risk and movement speed) and it is higher when the system is less certain about the human's movement or intention. As an example, human uncertainty is higher when the operator is walking rather than when it's standing still. More importantly, human uncertainty can also encapsulate finer concepts such as human awareness of human expertise. If the operator is known to be aware of the robot moving next to them, then we can assume the uncertainty to be lower. Likewise, if the operator is experienced, we can assume their uncertainty to be lower.

### How they are used
As stated above, the impact probability and human uncertainty values depend heavily on what we know about the operator, like what is he doing or how experienced he is. It is very easy, though, to apply heuristics to these parameters by assuming values such as worst case, best case or other more sophisticated logics.

By default, these values are included in the (0,1] interval, so that each of them has equal weight in calculating a final risk value for a given task.

The final risk value is calculated by multiplying all the values together. To be more specific:

A scenario has a set of tasks $A$ that the robot might carry out. The $i$-th task in $A$ can be executed with a set of distinct trajectories $T_i$ (each of them with its own risk factor $t_{i,j}$).
Each of the trajectories can be followed with one of the speed values $v_{i,j,k}$ in the set $V_{i,j}$.
Each of the tasks in $A$ has a set of tasks $P_i$ that can be carried out concurrently by the human (each of those with its own risk factor $p_{i,n}$)
Each of the tasks in $A$ also has its own level of intrinsic risk $a_i$.
The operator that is being considered in the given scenario can have one of many uncertainty values inside $U$, we can assume for simplicity that in the scenario at hand the operator always has the uncertainty value $u$ which depends on their expertise level.

After deciding all of these values we know our intrinsic risk, geometry risk, movement speed, impact probability and human uncertainty. 
We can multiply them together to find this exact situation's risk value.
$$R_{i,j,k,n}=a_i\cdot t_{i,j}\cdot v_{i,j,k}\cdot p_{i,n}\cdot u$$
If we don't know what tasks the human is carrying out in the meantime, we can just assume $p_{i,n}=\max(P_i)$ or a similar heuristic value.

## Qualities of the model
Each one of these values depends on its own specific environmental properties. A nice perk of this representation is that we can act on each of the values in some way. We can reduce the intrinsic risk by swapping the current task with another; we can change the trajectory and speed to reduce geometry risk and movement speed respectively; we can reallocate the tasks to reduce impact probability; we can communicate with the operator to reduce the human uncertainty. This will be elaborated further later on in the paper.

The values are also all between 0 (excluded) and 1 (included), but they're very easy to fine tune according to one's preference in order to ensure a behavior that is closer to the desired one. Admittedly, it might not be easy to fine tune the ranges without a reference and without experimental data. We think that further work could be done in defining a set of guidelines to help users of this model tweak the parameter's values in order to obtain desired qualities. The experiment we talk about later on in the paper could be the first step towards this.

A great new capability of this model is the capability 

This model uses parameters that are very general and can be applied to almost any scenario, since they are derived from the first principles of the standards they're based on. These parameters satisfy our requirements of flexibility and generality quite well. The parameters themselves can be easily reinterpreted in specific situations to better fit a given scenario
As a proof of the ease with which this model can be applied, we will be showing how we implemented this model in a PLATINUm based planner.