- How are people making robots safe right now?
    - Hierarchy of risk reduction.
        - elimination.
        - substitution.
        - safeguarding.
        - personal protection equipment.
        - awareness.
    - Risk factors.
        - These are shown in the RIA pdf.
        - Mention the ISO, RIA and ANSI standards.
        - Avoidance,.
        - severity,.
        - exposure.
    - Changing shape of robot (or compliant robot).
    - Putting barriers.
    - Reallocating responsibilities.
    - Sensors
        - Reaction to sensor data
        - Slow down, stop (bad for efficiency)
- These are mostly design guidelines.
    - We can’t easily apply them to an existing HRC
        - they are about the shape, role and safety measures
    - Or if we can, it’s not sophisticated
        - It’s just a slowdown or a stop, or keeping distance
        - Safety stops can be frequent since there is no way to plan with safety in mind
        - Estimated safety parameters prior to execution
            - They make the designers be over conservative
    - Some of them decrease collaboration
        - In the name of safety, some task are never done collaboratively
        - might be because of the application of those design patterns
        - might be because of the application of the not sophisticated runtime techniques
    - We need a better plan-time and run-time solution
- We want a new risk awareness model
    - It should be general
    - It should be flexible
    - It should be based on a preexisting and tested standard
- How we morph the “design patterns” into a flexible, general model
    - Morph design time severity into plan time (or run time) severity
        - General risk of the scenario becomes a risk value for each task
        - It is intrinsic to the task itself, not how it is carried out
            - Which tool
                - Shape
                - Powered?
            - What power
            - What weight
            - impact type
            - impact strength
    - Morph design time avoidance into …
        - General avoidance considerations become values that describe the movement of robot and human
        - We can use the knowledge how robot and human position
        - robot and human speed
        - Uncertainty of human movement
    - Morph design time exposure …
        - This is about the morphology of the collaborative environment
            - barriers
            - protective equipment
        - Since it’s meant to reduce impact probability, we can just use that
        - This is where the awareness of which tasks are being executed comes into play
        - High exposure means that robot and human are working closely or in highly collaborative tasks
        - Tasks that are far away or less collaborative have less exposure

# The state of the art

## Standards
The [[ANSI_RIA R15.06-2012]] standard is an adoption of pre-existing robot safety standards such as [[ISO 10218-1.2011]] and [[ISO 10218-2.2011]], which focuses on providing HRC cell designers with appropriate guidelines to help them improve the safety of collaborative cells.

Part 5.10 provides some of the safety measures that should be used to ensure a safe collaborative environment, such as safety stop, hand guiding, speed and separation monitoring, power and force limiting.

The monitored safety stop is meant to keep the collaborative robot from hurting the human operator by stopping whenever it detects obstructions (like human body parts). The standard recommends keeping a high collaboration level by avoiding powering off the robot in case of safety stops, so that it can resume working without any external inputs. It is worth mentioning that, while being one of the most useful techniques, safety stops can heavily impact efficiency if they happen very often.

Hand guiding consists in letting the human operator move the robot manually after the robot comes to a safety stops. This can help the robot resume operation with fewer impediments and it might prove useful in a number of applications.

Speed and Separation Monitoring is the concept of keeping track of the distance between the robot and surrounding obstructions (i.e. a human operator) and using that data to moderate the robot's speed. Different safety zones could be defined and predefined speed values can be assigned to each of them, so that the robot slows down as the human gets dangerously close. This can be seen as an extension of the aforementioned Safety Stop, since the robot is not only able to stop itself, but also slow down dynamically.

Power and Force limiting means designing robots that are incapable of exerting high amounts of force that could hurt human operators. Pressure is also considered in this section, which suggest designing an HRC cell without pinch points, sharp edges and so on. This is very useful in scenarios where human and robot work closely and contact cannot be easily avoided.

## Risk factors
The main risk factors that were highlighted in the standard are, in order of importance, severity, exposure and avoidance.

This allows HRC cell designers to have a more detailed representation of risk than the one based on severity and probability, which is very flat and often prevents us from appreciating the nuances hidden in a scenario and hides them behind a simple value.

Severity is the most relevant factor and the standard assigns it values such as minor, moderate and serious. Although it is intuitive, we specify that severity is meant as an indicator for the kind of impact that an accident in a given situation might have, regardless of its likelihood.
Exposure can be either high or low and it is an indicator of the physical characteristics of the HRC cell that help prevent accidents. It measures how easy it is for the human operator to physically get in the way of the robot and risk getting hurt by it.
Avoidance can be likely, not likely or not possible. It measures how likely it is for the human operator to avoid accidents if it's in the robot's way. Avoidance can be likely when the human has a broad range of motion and the robot is slow and predictable. It can be not possible when the robot is fast and the operator cannot physically get out of its way. 
The reason why avoidance is the last of these factors is that it would be unfair to rely on the human getting out of the robot's way. Humans can often be distracted or they can be affected from other issues that don't allow them to avoid danger easily.

After considering all of these factors, a risk level can be determined and it can assume values such as: negligible, low, medium, high and very high.

## Risk reduction
The standard suggests using these factors during the design of an HRC cell. The proposed approach is one that involves an iterative process of repeated risk reduction.

First, the use limits of a robot are defined so that designers know the full range of situations that the system might be involved in. Then designers are invited to identify tasks and associated hazards and estimate how risky they are using the aforementioned risk factors. 
After this is done, the procedure involves determining ways to reduce the risk and implementing those measures.
When these measures are verified, the designer will start identifying and reducing new risks until the desired safety level is achieved.

The standard provides a general approach to risk reduction, which follows a hierarchy of actions that the designer can take to make the system safer.

The immediate course of action is modifying the robot to improve its safety. This can be done by eliminating its risky features as much as possible. 
An example of this could be changing the robot's responsibilities in an HRC scenario so that it is not required to move heavy payloads or handle dangerous tools. The robot shouldn't be doing anything that can pose a risk on the humans that work with it, unless it really has to.
If the robot really has to accomplish dangerous tasks, then the standard suggests to substitute risky feature with safer ones.
An example is the adoption of some level of compliance. A compliant collaborative robot is much less dangerous for the human that works with it, since it does not overpower the operator. Substitution can also be applied by trying to accomplish the same task by using a different method, like pushing a heavy payload on the ground instead of lifting it over the operator's head.
A last resort when modifying the robot is changing its moving speed, which is a trivial way to improve safety.

Next up on the list is the use of safeguards inside the HRC cell, in order to physically prevent the human from getting close to high danger spots inside the system. It is worth mentioning that an excessive level of safeguarding could defeat the point of a collaborative system, since completely separating the robot and the human removes all risks at the cost of leaving us with a traditional robot and no degree of collaboration.

The remaining steps in reducing HRC risk involve acting on the human operator that is gonna work in that system. The most important of these is raising awareness by properly training the operators that get near the machine. Awareness could be also achieved by using light and sound indicators in the HRC cell, which can be highly sophisticated.
If awareness has already been raised, then it's always a good practice to provide operators with personal protective equipment whenever needed. This kind of solution might not be useful in all scenarios, but it can go a long way in improving HRC safety in constrained systems where other solutions are hard or impossible to achieve.

It's easy to notice how each of these measures directly impacts on the risk factors that were mentioned above.
Things like reducing weight and power or using PPE greatly reduces the severity factor, while safeguarding and awareness decrease exposure and avoidance, respectively.

# Unsolved issues

## Applying design guidelines to HRC cells


# A new risk awareness model

## Requirements

# Morphing design patterns into a flexible, general model

## Severity

## Exposure

## Avoidance
