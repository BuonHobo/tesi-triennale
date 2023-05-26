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

Impact probability is a risk value that depends on how likely it is for the human and robot to be in each other's way. It depends on things like distance, but most importantly it depends on

### Considerations

## Qualities of the model