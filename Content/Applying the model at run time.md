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
- Hard to use sensor data. The technology might not even be there yet
- Movement override when runtime risk is too high

## Interpreting the parameters

The five risk factors from the proposed method are fairly easy to apply to run time implementations of the model. All it takes is some further interpretation of what each parameter means by defining ways to calculate it and ways to influence it to reduce risk.

Just like we discussed earlier, the intrinsic risk value depends on the task that is being carried out at a given time and it does not depend on how it is being carried out. This means that we can derive this risk factor from our knowledge of the task at hand. Intrinsic risk values for each task could be precalculated in cases where we already know exactly which tasks the robot is going to accomplish. It is also possible to use the robot's dynamic, run time knowledge to assess the intrinsic risk of the current situation. This would be very useful for HRC scenarios where it is not possible to know the exact set of tasks that the robot is going to do beforehand. It would allow for very flexible and smart robots to exist and collaborate with humans.
The intrinsic risk of a given situation can be curbed by the robot by changing the current task in some way. Maybe it is possible to accomplish the same goal with a different task, or it could be useful to allocate risky tasks later, when they are less likely to cause damage.

The geometry risk value can be calculated at run time using the knowledge of the chosen trajectory for a given task and the sensor data about the human that the robot collected. The geometry risk value calculated can be low at the beginning of a task, but it could increase as the human gets too close to the working area. A way to reduce the geometry risk value at run time is increasing the distance between robot and human if possible. A different way could be following a different, safer trajectory that doesn't intersect vulnerable body parts. It is probably safer for the robot to follow a trajectory that only risks hitting the human's foot (which may be protected with PPE) than a trajectory that could hit the eyes (even if the operator is wearing safety glasses).

Movement speed is very intuitively applied here, it might not be required to calculate it since most robots probably know what their target speed is at run time, and they have a current speed value that they can use. It is also possible to derive this value from sensor data, since some robots might not be fully able to control their moving speed. An example of this kind of robot is one that flies or drives around, which could easily be pushed by the wind or any surrounding agent. Movement speed is usually an adjustable value in HRC cells.

Impact probability is also intuitive. It represents the general distance between robot and human and the probability of them getting in each other's way. This can be evaluated using known data about what the allocated task for the human is, or it could be estimated based on the physical distance between the working areas of human and robot. This risk factor can be made safer by reallocating the human's task so that the working areas are not close together. It could also be reduced by asking the human to move away with light or sound signals (more on this later).

Human uncertainty can be estimated using known data about the operator like experience level, age, movement range, etc. Even though the run time application of this model probably benefits far more from evaluating this factor using sensor data during run time because it equips the robot with the capability of responding to nuanced, real life situations. Run time human uncertainty can be calculated by detecting how quickly or how unpredictably the human has been moving in a recent time interval. If the human has been moving really quickly for the past minute then it's probably better to account for a higher human uncertainty value. If the human has been standin

## Qualities




