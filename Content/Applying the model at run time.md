- Parameters interpretation
    - Intrinsic risk is the same
    - Geometry risk and Movement speed are real time
    - Human uncertainty and Impact probability is real time (sensor data)
- Hard to use real time data, but it allows great control over safety
    - Robot motion data (like geometry and speed) can be adjusted in real time
    - Human motion data can also be adjusted
        - Signaling danger to the human
            - lights, sound, voice
        - Changing the human task
- Hard limits to risk can be set
- A lot of configuration possibilities with presets
- Movement override when runtime risk is too high
- Hard to use sensor data. The technology might not even be there yet


## Interpreting the parameters
The five risk factors from the proposed method are fairly easy to apply to run time implementations of the model. All it takes is some further interpretation of what each parameter means by defining ways to calculate it and ways to influence it to reduce risk.

Just like we discussed earlier, the intrinsic risk value depends on the task that is being carried out at a given time, and it does not depend on how it is being carried out. This means that we can derive this risk factor from our knowledge of the task at hand. Intrinsic risk values for each task could be precalculated in cases where we already know exactly which tasks the robot is going to accomplish. It is also possible to use the robot's dynamic, run time knowledge to assess the intrinsic risk of the current situation. This would be very useful for HRC scenarios where it is not possible to know the exact set of tasks that the robot is going to do beforehand. It would allow for very flexible and smart robots to exist and collaborate with humans.
The intrinsic risk of a given situation can be curbed by the robot by changing the current task in some way. Maybe it is possible to accomplish the same goal with a different task, or it could be useful to allocate risky tasks later, when they are less likely to cause damage.

The geometry risk value can be calculated at run time using the knowledge of the chosen trajectory for a given task and the sensor data about the human that the robot collected. The geometry risk value calculated can be low at the beginning of a task, but it could increase as the human gets too close to the working area. A way to reduce the geometry risk value at run time is increasing the distance between robot and human if possible. A different way could be following a different, safer trajectory that doesn't intersect vulnerable body parts. It is probably safer for the robot to follow a trajectory that only risks hitting the human's foot (which may be protected with PPE) than a trajectory that could hit the eyes (even if the operator is wearing safety glasses).

Movement speed is very intuitively applied here, it might not be required to calculate it since most robots probably know what their target speed is at run time, and they have a current speed value that they can use. It is also possible to derive this value from sensor data, since some robots might not be fully able to control their moving speed. An example of this kind of robot is one that flies or drives around, which could easily be pushed by the wind or any surrounding agent. Movement speed is usually an adjustable value in HRC cells.

Impact probability is also intuitive. It represents the general distance between robot and human and the probability of them getting in each other's way. This can be evaluated using known data about what the allocated task for the human is, or it could be estimated based on the physical distance between the working areas of human and robot. This risk factor can be made safer by reallocating the human's task so that the working areas are not close together. It could also be reduced by asking the human to move away with light or sound signals (more on this later).

Human uncertainty can be estimated using known data about the operator like experience level, age, movement range, etc. Even though the run time application of this model probably benefits far more from evaluating this factor using sensor data during run time because it equips the robot with the capability of responding to nuanced, real life situations. Run time human uncertainty can be calculated by detecting how quickly or how unpredictably the human has been moving in a recent time interval. If the human has been moving really quickly for the past minute, then it's probably better to account for a higher human uncertainty value. A lower uncertainty value should be assigned when the operator has been standing still for five minutes.

## Qualities
With the risk values above, a robot has sophisticated awareness of how risky a given situation is. It is not only aware of a flat general risk assessment value, but it also knows what exactly is making the situation unsafe, and it knows how to act in order to solve the issue. The robot has a broad range of actions that it can perform to avoid dealing damage to the human operator working next to it. We suggest using the same hierarchy that the mentioned standard provides when trying to curb risk. Start from trying to reduce severity and then make your way towards reducing exposure and avoidance. This is the best way to reduce risk since you are acting on the most impactful factors first. The robot's capability to dynamically and intelligently respond to precise risk factors of a given situation is a strikingly useful feature of this application. It is crucial for an HRC robot to have agency over the risk of a situation, and this technique allows for this agency to be applied to any kind of HRC robot.

We feel the need to stress that agency does not have to be limited to curbing severity and exposure, but it is possible to come up with appropriate ways to also improve factors like avoidance in nontrivial ways. A trivial way to improve avoidance would be reducing the robot's speed, but it comes with drawback, and it is best to do only when necessary. 
Besides reallocating the tasks so that the human is not in the way of the robot, we emphasize the possibility of using sound and visual signals to have a proper communication with the operator. Nowadays, it is easier than ever for robots to communicate with humans thanks to the recent staggering progress in AI language models, and it would make sense to apply these advancements to HRC environments. An HRC robot should give a steady stream of information to the human operator about what it is about to do, this is also what makes human to human collaboration work so well.

If the risk value gets too high, a robot has the capability of performing the safety stops that we mentioned earlier. The robot can make an informed decision that allows it to avoid stopping when it is not necessary. Future work could go into implementing more sophisticated techniques of movement override, such as evasive maneuvers or the hand guiding feature that the standard mentions.

The run time application of this model is also well suited for the fine-tuning of the risk parameters, which can have a great impact on the efficiency and safety of an HRC cell. As we stated earlier, it could be hard to tweak the parameters, so a way to make it easier in this specific environment is offering preset values for different kinds of safety-efficiency tradeoffs for each HRC scenario.

An immediately apparent characteristic of this application is that it is heavily dependent on sensor data. This is what makes it so flexible and general, but it also means that it may be hard to implement. Sensor data is a tricky subject, accurate sensors are expensive and the state of the art is only just starting to come up with advanced situational awareness techniques based on sensor feeds.