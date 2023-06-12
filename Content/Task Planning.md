- AI Planning
    - What is AI planning
    - How it works
- Timeline based planning
    - How it works (model, problem)
    - Advantages
    - Disadvantages
- PLATINUm
    - Implementation of timeline is based planning
    - supports uncertainty
    - Used at CNR
    - Flexibility
    - Java

## AI Planning

In today's rapidly evolving world, the ability to automate decision-making processes and plan complex actions is of paramount importance. Artificial Intelligence (AI) planning has emerged as a subfield of AI that addresses this need. AI planning focuses on generating plans or sequences of actions to accomplish predefined objectives. It involves constructing a plan that transforms an initial state of a system into a desired goal state. AI planning algorithms utilize domain knowledge and search techniques to efficiently explore the space of possible plans and identify the most optimal or feasible solution.

The process of AI planning involves several key components. Firstly, the representation of states, actions, and goals provides a formal framework for describing the problem domain. States represent the different configurations of the system, actions capture the atomic units of change that transform one state to another, and goals specify the desired end-state to be achieved. Secondly, the planning algorithm utilizes search techniques to traverse the space of possible states and actions, aiming to find a sequence of actions that satisfies the specified goals. The algorithm may employ heuristic functions to guide the search process efficiently. Ultimately, AI planning produces a plan, which is a sequence of actions that, when executed, achieves the desired goals.

## Timeline-based planning

Timeline-based planning is a specific approach within AI planning that incorporates temporal constraints into the planning process. While traditional AI planning focuses on determining the sequence of actions without considering time, timeline-based planning considers the temporal aspects of actions and events. It captures the temporal dependencies and constraints among actions, enabling the generation of plans that satisfy both logical and temporal constraints. By explicitly modeling the temporal aspects, timeline-based planning enhances the quality and efficiency of the generated plans.

As shown in [[Mayer2015]], timeline-based planning models a domain as a set of features with an associated set of temporal functions on a finite set of values. The time-varying features are usually called multi-valued state variables. The evolution of the features is described by some causal laws and limited by domain constraints. These are specified in a domain specification. The task of a planner is to find a sequence of decisions that brings the timelines into a final desired set, satisfying the domain specification and special conditions called goals. Causal and temporal constraints specify which value transitions are allowed, the minimal and maximal duration of each valued interval and synchronization constraints between different state variables.

In real-world domains, once a temporally flexible plan is generated, it has to be executed by an executive system that manages controllable processes in the presence of exogenous events. In this scenario, the execution process is not completely under the control of the executive. Thus, a mandatory requirement for dealing with P&S in real-world contexts is to distinguish between controllable and uncontrollable tasks.

This approach has been shown to be successful in a number of concrete applications, such as autonomous space systems. It provides a general semantics for timeline-related planning concepts such as domains, goals, problems, constraints and flexible plans, taking into account the difference between controllable and uncontrollable activities. Some sources of uncertainty are also modeled in the proposed framework and taken into account in the characterization of valid plans that are assumed not to take decisions on components the planner cannot control. A formal definition of different forms of plan controllability is also proposed.

Timeline-based planning offers several advantages over traditional planning approaches. Firstly, it allows for the consideration of temporal constraints, such as deadlines, durations, and temporal ordering, enabling the generation of plans that adhere to strict temporal requirements. This is particularly beneficial in domains where timing is crucial, such as manufacturing, scheduling, and robotics. Secondly, timeline-based planning can improve plan quality by incorporating temporal reasoning, resulting in more efficient and effective plans. It can handle complex temporal constraints, optimize resource allocation, and synchronize actions, leading to improved performance in various real-world scenarios.

While timeline-based planning provides significant benefits, it also presents some challenges. One major disadvantage is the increased computational complexity due to the consideration of temporal constraints. The incorporation of time-related reasoning adds complexity to the planning algorithm, potentially resulting in longer planning times and increased resource requirements. Additionally, timeline-based planning may face challenges in dealing with uncertainty and dynamic environments, where temporal constraints may change dynamically. Scalability can also be an issue when applying timeline-based planning to large-scale problems, as the search space grows exponentially with the number of actions and time intervals.

However, the benefits of accurate temporal coordination in HRC scenarios often outweigh these challenges, as it improves safety, efficiency, and overall task performance. In HRC scenarios, where humans and robots work together, timeline-based planning can ensure that the robot's actions align with the temporal expectations of the human collaborator. This synchronization is crucial for smooth and effective coordination between humans and robots.

Overall, timeline-based planning provides a powerful framework for dealing with complex planning problems that involve temporal constraints and uncertainty. It offers a rigorous and intelligent approach to modeling and solving real-world problems.

## PLATINUm

PLATINUm [[umbrico2017platinum]] (Planning and Acting with TImeliNes under Uncertainty) is a new planning framework developed at CNR (Consiglio Nazionale delle Ricerche) that advances the state of the art with the ability to deal with temporal uncertainty both at the planning and plan execution level. It is a comprehensive planning system endowed with a new algorithm for temporal planning with uncertainty, heuristic search capabilities grounded on hierarchical modeling, and a robust plan execution module to address temporal uncertainty while executing plans .

PLATINUm has been successfully deployed in a manufacturing scenario to support Human-Robot Collaboration. The system has been developed and deployed within the FourByThree research project . The PLATINUm planning and acting capabilities have been integrated into a software environment that facilitates the adaptation of a new robotic arm in different HRC manufacturing scenarios. The proposed planning system has been completely deployed in a realistic case study, demonstrating its ability to support productive and safe collaboration between human and robot .

One of the key strengths of PLATINUm is its ability to handle temporal uncertainty both at planning and execution time. This allows the framework to address problems where not all features of a domain are under the control of the system. Moreover, the combination of temporal flexibility and temporal uncertainty allows P&S controllers to generate flexible and temporally robust plans that can be dynamically adapted at execution time without generating new plans from scratch .

PLATINUm is implemented in Java, a portable and robust programming language with a vast ecosystem of libraries and tools. Java's platform independence enables PLATINUm to run on various operating systems, while its object-oriented nature promotes modular and maintainable code for extensibility and long-term maintenance.

In conclusion, PLATINUm serves as a powerful implementation of timeline-based planning, enabling the handling of uncertainty and supporting complex temporal constraints. Its application in the FourByThree research project has demonstrated its effectiveness in human-robot collaboration scenarios, and its flexibility makes it a versatile framework for addressing various problem domains.