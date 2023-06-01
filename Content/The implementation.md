- Extending Planner
    - decorators
    - configurations
- Extending SearchStrategy
    - enqueue
    - compare
    - Domain specific object
- The RiskAssessmentPlanner Class
    - Platinum’s flexibility allows to make a planner just by adding a class
- The RiskAssessmentSearchStrategy Class
    - A strategy affects the way a planner gets to the solution
    - The trivial safe solution is to avoid collaboration entirely
    - Pareto logic
- RiskSearchStrategy Class
    - Risk only
- MakespanSearchStrategy Class
    - dumb makespan only
- PlannerSearchStrategy Class
    - no risk awareness
    - already implemented
    - depth first
- Risk parameters as Java Enums
    - PLATINUm's representation of tasks
    - FromString constructor
    - Easy interface with platinum
    - makespan
    - show every enum in detail
    - show humantaskrisk
    - show robottaskrisk
    - idle
- The NodeRiskEvaluator Class
    - Can be used by any planning strategy
    - Parses the whole plan into sequences of risk aware tasks
    - The risk factor of each robot task uses the weighted average of the parallel human tasks
    - implementation details
    - how every parameter is calculated
    - RiskAssessmentSearchStrategy's compare function
    - All the data it produces:
        - average risk
        - max risk
        - possible collisions
        - best makespan
        - human makespan
        - best robot makespan
        - estimated robot makespan
        - robot tasks
        - human tasks
    - Evaluates Risk
    - Evaluates Makespan
- The python ddl generator
    - Flexible and easy to configure
        - more kinds of tasks
        - tweak amount of tasks
        - tweak allocation constraints
        - tweak trajectories, speeds and risk values
    - Modular
    - DDL and PDL
	- Domain components
       - Constraints
       - Transition between state values
       - parameters
	- goal component
- The DataCollector Class
    - Start it and leave it
    - Get a detailed css
        - it collects time to plan
- The python chart generator
    - Define a set of functions, each one makes a graph
    - Just upload the css file and it automatically generates graphs

## Integrating with PLATINUm
### PLATINUm's architecture
As stated earlier, and in [[Timeline-based planning and execution with uncertainty. Theory, modeling methodologies and practice]], PLATINUm is a general-purpose framework for planning and execution with timelines under uncertainty, which implements a hierarchical solving procedure.
The PLATINUm framework has a layered architecture, which consists of two main components: the Representation Framework and the Problem Solving component.

The Representation Framework is responsible for representing the planning domain and problem in a structured manner. It consists of a plan database that stores information about the planning domain, such as state variables, decomposition rules, and synchronization rules. The plan database is structured in such a way that it can support hierarchical modeling of timeline-based domains.

The Problem Solving component is responsible for generating plans that satisfy the given planning goals. It consists of a planner that uses search heuristics to leverage the hierarchical structure of the planning domain during plan generation. The planner is capable of dealing with temporal uncertainty during plan generation by leveraging information about the temporal uncertainty of the planning domain. This layer works in conjunction with the Representation Framework to support hierarchical modeling and solving of timeline-based planning problems under uncertainty. 

The planner uses the information stored in the plan database, such as state variables, decomposition rules, and synchronization rules. It's aim is to synthesize a set of operations that, given an initial state, allow the system to reach a desired goal state. The reasoning process relies on a model which represents a general description of the problem to solve. The model provides a representation of the environment in terms of the possible states of the world and the actions the system can perform to interact with the environment.

Suitable plans are found through the use of a Search Strategy class, which determines how the planner explores the space of possible solutions in order to find a plan that achieves the desired goal state. The choice of search strategy can have a significant impact on the efficiency and effectiveness of the planning process. There are many search strategies that can be used by planners, each with its own strengths and weaknesses. Some common search strategies include depth-first search, breadth-first search, best-first search, and iterative deepening search. The choice of search strategy depends on factors such as the size and complexity of the problem space, the available computational resources, and the desired trade-off between speed and optimality.

### Building on top of PLATINUm
In order to expand PLATINUm's capabilities with the ones provided by our model, we had to extend the two very important classes that were mentioned above: namely the Planner class and the SearchStrategy class.

PLATINUm's high degree of flexibility allowed us to make a new RiskAssessmentPlanner class very easily. The steps to make it work were configuring the Planner's modules using decorators. To be more specific, we configured the RiskAssessmentPlanner to have no planning timeout. This allows the Planner to keep looking for solutions for an unlimited amount of time. This was necessary because, as we'll explain later in the paper, the time to find a suitable plan can be quite high.
The other significant configuration step in RiskAssessmentPlanner was setting its search strategy to be our own RiskAssessmentSearchStrategy.

The RiskAssessmentSearchStrategy is one of the most essential parts of this project, since it is where some of the most crucial algorithmic parts of the implementation reside. The RiskAssessmentSearchStrategy expands the SearchStrategy class, and in order for it to work as we intend it to, we had to override some methods.

The first method we had to override was the `void enqueue(SearchSpaceNode node)` function. This function is responsible for receiving a partial plan and use it to make a wide variety of estimations about it. These estimations can be used later by the `int compare(SearchSpaceNode searchSpaceNode1, SearchSpaceNode searchSpaceNode2)` function to establish which partial plan the Problem Solving Component should consider to be better. The better node will be further refined and compared again with other partial plans, until a suitable plan is found.

The data that `enqueue` evaluates about a partial plan is then set as that node's `Object domainSpecificMetric` attribute, so that it can be later referenced in the `compare` function to establish which plan is preferred.

In our case, the `enqueue` function asks a different class to compute the relevant data and set it as the `domainSpecificMetric` attribute. We will elaborate on that different class in a moment.
The `enqueue` function also has the crucial role of computing the Heuristic Cost of a node, which is relevant during the comparison because it allows us to distinguish unrefined partial plans from more refined ones.
When all of this is done, the RiskAssessmentSearchStrategy gives the node back to PLATINUm, so that more operations can be done with it.

We want to stress that the `compare` function is the core of what distinguishes different Planners or Search Strategies, and the quality of results depends almost completely on it. As well as the time that it takes to find a suitable plan.
We will elaborate on what the `compare` function does later, after properly explaining which data our implementation computes and how. However, we can still make some considerations.
This function uses a Pareto optimization to find the best possible tradeoff between safety and efficiency. This kind of logic was needed to avoid trivial and extreme solutions which decrease risk by completely removing any degree of collaboration between human and robot. 
It is easy to find a safe plan where the robot does everything and the human watches from a safe distance, but this defeats all advantages of HRC.

As a way to showcase the qualities of our RiskAssessmentPlanner and RiskAssessmentSearchStrategy against less sophisticated solutions, we implemented other classes:
A RiskPlanner and RiskSearchStrategy which only compare partial plans based on the risk value, and a MakespanPlanner and MakespanSearchStrategy which only compare partial plans based on their expected makespan.
We also compared our strategy against the PlannerSearchStrategy, which is provided by PLATINUm and performs a blind depth first search. It has no awareness of the risk factors and makespan and only cares about finding the first suitable plan.

## Model implementation
### Risk factors
In order to evaluate the risk factors of a task, and subsequently of a whole plan, we need to know which tasks PLATINUm is proposing for the current partial plan. Each `SearchSpaceNode` has a reference to a `Plan` class, which in turn has a `Map<DomainComponent, List<DecisionVariable>>`. Each component in the problem domain has its own timeline and we care about the timelines for the Human and Robot components.

A timeline is represented with a `List<DecisionVariable>`, in the case of the Human and Robot components, each `DecisionVariable` represents a task. We can finally know what the task is by calling `task.getValue()` and parsing the string that this method returns.

As an example, let's say that the task we're currently parsing has the following (String) value: `PickPlace-foam-low-chest-regular`. This means that the task is a Pick & Place operation, which is the most common in HRC environments. The following set of values specifies the parameters of this task, which respectively indicate `target`, `intrinsic risk`, `trajectory`, and `speed` of the Pick & Place. 

We will elaborate on these parameters later, what matters is how our implementation parses this value into structured and useful data.
Each of the relevant parameters of a task exists inside our implementation as a Java Enum class, which contain the static pre-compiled values that are associated to each value of the parameter. 

```java
public enum GeometricRisk {  
	legs(  riskValue: 0.2, ...),  
	chest( riskValue: 0.4, ...),  
	head(  riskValue: 0.7, ...),  
	
	//...
}
```

We parse the string we get from PLATINUm into a set of parameters represented by Enums. In order to do this, we split the string on every occurrence of the `-` character and then instantiate the Enum class using the static `valueOf(str)` method. Here's an example:

After every parameter has been parsed into the appropriate class, they are all stored inside a class that represents the whole task. Here's an example of the system:

```java
public RobotTaskRisk(String tokenValue, ...) {  
	//...
	// tokenValue: "PickPlace-foam-low-chest-regular"
	String[] splits = tokenValue.split("-"); 
	//...
	this.geometricRisk = GeometricRisk.valueOf(splits[3]);
	//...
}
```

In our example, the `chest` parameter gets parsed into `GeometricRisk.chest` and we now have access to the risk value associated to this parameter. The same is done with the other parameters, and everything is stored inside the `RobotTaskRisk` class.
This makes it very easy to parse a simple string into structured and useful data that we can use to compute risk and makespan related properties.

As for the time related properties, they are stored in the class that represents the task (`RobotTaskRisk` in this case). Each task knows its start time (which is passed as a parameter to the constructor) and its makespan.
The makespan is calculated using other properties that are stored inside the parameter Enums. We will now talk about those in detail:

The `IntrinsicRisk` class only stores the numeric risk value that corresponds to each parameter.

The `GeometricRisk` class, which was partially shown earlier, stores both the numeric risk value corresponding to a specific trajectory and the distance that the robot has to travel in order to follow the whole trajectory.

The `Speed` class stores, for each of the allowed speed levels, both the absolute speed value and the risk value that is associated to it.

The `Target` class knows what the target of this task is, and it stores the risk values associated with each varying degree of collision risk. This class also knows how much time the human is expected to take in order to complete the task.

Finally, the `Expertise` class know what risk value is associated to each different experience level of the operator. Each experience level also has a time multiplier, based on the assumption that a more skilled human will take less time to carry out the same task.

Human tasks and robot tasks are not equal, since `IntrinsicRisk`, `GeometricRisk`, and `Speed` do not apply to a task that is being carried out by a human. Similarly, `Expertise` does not apply to robots.
This was solved by having two separate classes: `HumanTaskRisk` and `RobotTaskRisk` which both implement the `TaskRisk` interface, so that they both have methods such as `getMakespan()` and others.

For a human task, makespan is obtained by multiplying the expected completion time stored in `Target` with the `Expertise` related time multiplier.
For a robot task, on the other hand, makespan is obtained using the distance and speed values mentioned earlier.

The risk value is computed by `RobotTaskRisk` with the `getRiskValueWithHumanTask(HumanTaskRisk humanTask)` function. This method also needs to know the human task that is being carried out in parallel, in order to estimate the collision probability and to know how experienced the human is.
It does not make sense, for the scope of our project, to assign a risk value to situations with no degree of collaboration. A robot which is working alone cannot harm any human, and a human working alone only needs its own risk estimation capabilities. However, some heuristic techniques may involve risk estimations that only involve one of the actors.

Although risk only matters when both human and robot are working, we still had to design our implementation to allow for `Idle` tasks. `Idle` tasks only last a unit of time and they serve as a transition states between the completion of a task and the beginning of the next one. We decided, for the scope of our project, to have no risk when the robot is idle. However, if the robot is operating while the human is `Idle` (transitioning from a task to another), we consider a small risk because the human is still in the working area.

Calculating makespan and risk this way worked well for our implementation, but more sophisticated strategies could be used.

### The NodeRiskEvaluator Class
After defining the classes needed to parse and represent a single task, we now need a component with the responsibility of computing all the relevant risk and makespan related data on the scale of a plan. As stated earlier, PLATINUm gives our search strategy a partial plan and allows us to label it with data that we can later use to compare different plans.

We will now discuss the implementation of the component charged with receiving a plan (or partial plan), calculating useful metrics and assign this data to the search space node. 
This class is called `NodeRiskEvaluator`.

It makes sense to have all the logic regarding the evaluation of a plan inside its own class, so that it can be used in different ways by multiple components of our implementation. This means that we're able to evaluate the safety and efficiency of a plan even when working with strategies that are not aware of our risk model, or that use it in conjunction with other metrics and techniques.

After receiving a search space node, the `RiskAssessmentSearchStrategy` passes it to the `NodeRiskEvaluator`. The evaluator gets the timelines of the robot and human components in the plan, and then parses the timelines from `List<DecisionVariable>` to `List<HumanTaskRisk>` and `List<RobotTaskRisk>`. 

During the parsing of the timelines, the evaluator starts from 

## Utilities
### Python DDL generator


### The DataCollector Class


### Python chart generator
