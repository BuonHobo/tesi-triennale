## The state of the art

### Standards
The [[ANSI_RIA R15.06-2012]] standard is an adoption of pre-existing robot safety standards such as [[ISO 10218-1.2011]] and [[ISO 10218-2.2011]], which focuses on providing HRC cell designers with appropriate guidelines to help them improve the safety of collaborative cells.

Part 5.10 provides some of the safety measures that should be used to ensure a safe collaborative environment, such as safety stop, hand guiding, speed and separation monitoring, power and force limiting.

The monitored safety stop is meant to keep the collaborative robot from hurting the human operator by stopping whenever it detects obstructions (like human body parts). The standard recommends keeping a high collaboration level by avoiding powering off the robot in case of safety stops, so that it can resume working without any external inputs. It is worth mentioning that, while being one of the most useful techniques, safety stops can heavily impact efficiency if they happen very often.

Hand guiding consists in letting the human operator move the robot manually after the robot comes to a safety stops. This can help the robot resume operation with fewer impediments, and it might prove useful in a number of applications.

Speed and Separation Monitoring is the concept of keeping track of the distance between the robot and surrounding obstructions (i.e. a human operator) and using that data to moderate the robot's speed. Different safety zones could be defined and predefined speed values can be assigned to each of them, so that the robot slows down as the human gets dangerously close. This can be seen as an extension of the aforementioned Safety Stop, since the robot is not only able to stop itself, but also slow down dynamically.

Power and Force limiting means designing robots that are incapable of exerting high amounts of force that could hurt human operators. Pressure is also considered in this section, which suggest designing an HRC cell without pinch points, sharp edges and so on. This is very useful in scenarios where human and robot work closely and contact cannot be easily avoided.

### Risk factors
The main risk factors that were highlighted in the standard are, in order of importance, severity, exposure and avoidance.

This allows HRC cell designers to have a more detailed representation of risk than the one based on severity and probability, which is very flat and often prevents us from appreciating the nuances hidden in a scenario and hides them behind a simple value.

Severity is the most relevant factor and the standard assigns it values such as minor, moderate and serious. Although it is intuitive, we specify that severity is meant as an indicator for the kind of impact that an accident in a given situation might have, regardless of its likelihood.
Exposure can be either high or low, and it is an indicator of the physical characteristics of the HRC cell that help prevent accidents. It measures how easy it is for the human operator to physically get in the way of the robot and risk getting hurt by it.
Avoidance can be likely, not likely, or not possible. It measures how likely it is for the human operator to avoid accidents if it's in the robot's way. Avoidance can be likely when the human has a broad range of motion and the robot is slow and predictable. It can be not possible when the robot is fast and the operator cannot physically get out of its way. 
The reason why avoidance is the last of these factors is that it would be unfair to rely on the human getting out of the robot's way. Humans can often be distracted, or they can be affected from other issues that don't allow them to avoid danger easily.

After considering all of these factors, a risk level can be determined, and it can assume values such as: negligible, low, medium, high and very high.

### Risk reduction
The standard suggests using these factors during the design of an HRC cell. The proposed approach is one that involves an iterative process of repeated risk reduction.

First, the use limits of a robot are defined so that designers know the full range of situations that the system might be involved in. Then designers are invited to identify tasks and associated hazards and estimate how risky they are using the aforementioned risk factors. 
After this is done, the procedure involves determining ways to reduce the risk and implementing those measures.
When these measures are verified, the designer will start identifying and reducing new risks until the desired safety level is achieved.

The standard provides a general approach to risk reduction, which follows a hierarchy of actions that the designer can take to make the system safer.

The immediate course of action is modifying the robot to improve its safety. This can be done by eliminating its risky features as much as possible. 
An example of this could be changing the robot's responsibilities in an HRC scenario so that it is not required to move heavy payloads or handle dangerous tools. The robot shouldn't be doing anything that can pose a risk on the humans that work with it, unless it really has to.
If the robot really has to accomplish dangerous tasks, then the standard suggests substituting risky feature with safer ones.
An example is the adoption of some level of compliance. A compliant collaborative robot is much less dangerous for the human that works with it, since it does not overpower the operator. Substitution can also be applied by trying to accomplish the same task by using a different method, like pushing a heavy payload on the ground instead of lifting it over the operator's head.
A last resort when modifying the robot is changing its moving speed, which is a trivial way to improve safety.

Next up on the list is the use of safeguards inside the HRC cell, in order to physically prevent the human from getting close to high danger spots inside the system. It is worth mentioning that an excessive level of safeguarding could defeat the point of a collaborative system, since completely separating the robot and the human removes all risks at the cost of leaving us with a traditional robot and no degree of collaboration.

The remaining steps in reducing HRC risk involve acting on the human operator that is gonna work in that system. The most important of these is raising awareness by properly training the operators that get near the machine. Awareness could be also achieved by using light and sound indicators in the HRC cell, which can be highly sophisticated.
If awareness has already been raised, then it's always a good practice to provide operators with personal protective equipment whenever needed. This kind of solution might not be useful in all scenarios, but it can go a long way in improving HRC safety in constrained systems where other solutions are hard or impossible to achieve.

It's easy to notice how each of these measures directly impacts on the risk factors that were mentioned above.
Things like reducing weight and power or using PPE greatly reduces the severity factor, while safeguarding and awareness decrease exposure and avoidance, respectively.

## Application to pre-existing HRC cells

Most of the guidelines and patterns mentioned above are meant to be applied during the design phases of an HRC cell. 
This is due to the fact that an HRC cell that has already been installed and started is very hard to change when it comes to affecting the shape of the robot, the morphology of the system, the allocation of responsibilities, etc. These solutions can be impractical or even impossible to apply on pre-existing hardware without radically changing the HRC cell into a new, different one.

Furthermore, as a side effect of being mostly meant for the design phases of a collaborative robot, these techniques have a limited reach on a cell's safety properties. They are of course crucial to ensuring a safe collaborative work environment, but they stop providing further safety once the HRC cell is installed and running. This also forces designers to be very conservative when determining the safety of an HRC environment, because they know that the robot will have to avoid harming people in a variety of possibly unforeseen situations. Over prioritization of safety could severely impact performance in HRC cells.

Of course, some of those safety measures can always be applied later, like providing better personal protective equipment or reducing the operating speed of the machine. Even some of the more complex solutions like safety stops or barriers can be added later, although it's rare to find a cell where those safety measures weren't already used where necessary. An example of a suboptimal posthumous application of safety measures could be the placement of a large barrier between the robot and the human. The barrier solves any risk issue, but it completely destroys any hope of collaboration, which in turn impacts flexibility and efficiency. 

Another very important point could be made against unnecessarily applying excessively strict safety measures in collaborative environments. As stated earlier in the paper, HRC environment are intended as a way to make production tool chains flexible and efficient at a lower cost compared to traditional robots. Unsophisticated safety measures can defeat the point of investing in an HRC cell, since they can dramatically impact both the efficiency and the flexibility that collaborative environments are known for.
As an example, safety stops are both a great way to ensure safety, but frequent safety interruptions can drastically decrease the throughput of an HRC cell. There should be a way to operate the HRC cell such that safety stops are reduced, without impacting the safety of the human.

It is apparent that the state of the art is underdeveloped when it comes to deploying sophisticated, flexible and general solutions to existing HRC cells that allow to strike a balance between safety and efficiency. This is going to be our focus.

## A new risk awareness model

### Requirements
In order to define a risk awareness model, we must first define what its scope and objectives will be.

As stated earlier, the state of the art is in need of more and more sophisticated solutions to improve safety and efficiency in domains that are out of the reach of standard design guidelines.
We identify those domains to be run time and planning time.

Our contribution focuses on providing a solution that is general enough to be applicable to a wide range of HRC scenarios and flexible enough to be used both at plan time and execution time. Flexibility is a very desirable property, since it is what makes HRC great, and no safety measure would make sense if it didn't preserve this defining feature of collaborative robots.

Another crucial requirement is that the model should be built on top of existing and well-worn standards and concepts that stood the test of time. We want our solution to be solid, and we can only achieve that if it's based on solid foundations. 
This is why we chose to base this model on the techniques that were previously explored. 

Our model is heavily based on the concepts of severity, exposure and avoidance, but since those risk factors are meant to be used during the design phases, we are adapting them to fit our previously specified requirements.

### Morphing design patterns into a flexible, general model

We will now be explaining the process that we used to morph the previously discussed design time guidelines into general and flexible risk factors that can be applied during plan time and execution time

The first factor we're discussing is the most important one: severity. Luckily, the concept of severity translates really well to our interested domain. 
Design time severity depends on the tasks that a robot is expected to accomplish, and it ends up being a value that generally measures the gravity of any possible accident that may happen with a given collaborative robot.
Our concept of severity is more precise, and it is associated to the risk value of each specific task. Just like the design guideline, our concept of safety depends on the task itself and not on how it is carried out, so we could say that it is an intrinsic property of the task at hand.
A task's severity depends on things like:
the tool being used (is it powered? Is it sharp?);
the power of the robot;
the weight of the robot and/or its carried payload;
the kind of impact it could have on the human limbs (pinch, press, tear, etc.);

Exposure is definitely harder to translate into our domain. It is a concept that inherently refers to static things like barriers, so it's not an immediate transition into plan time and run time.
If we go to the root of what the concept of exposure means in HRC cells, we can see that it is a metric intended to reduce impact probability by making it hard for human and robot to be in the same place at the same time.
It is definitely possible to reshape this concept so that we can use it in our model. Since we can't reduce the probability of impact with physical devices, we used software. We can interpret the exposure as the awareness of which tasks are being executed at the same time by the human and the robot. For example, the exposure is high if human and robot are both working on the same task or on two highly collaborative tasks that are close together.
Our risk aware model uses the knowledge of the tasks being carried out by the robot and the human to avoid the probability of being in the same place, which is exactly what exposure is meant to measure.

Design time avoidance heavily depends on the human properties, like position, range of movement and speed.
These general avoidance considerations become precise values that describe the movement of robot and human relative to each other. It is possible to calculate the avoidance using the knowledge of the position and speed of robot and human. 
Since this value highly depends on the human, we decided to make the avoidance value also dependent on how uncertain the human's movement is at a given time.

In the next chapters, we will explain exactly how the model works.