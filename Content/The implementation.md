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

During the parsing of the timelines, the evaluator starts from $t=0$ and it progressively adds up the makespan data of each task in order to initialize tasks with the correct start time. This allows the evaluator to avoid losing information regarding concurrent tasks from different timelines.
After this step, the `NodeRiskEvaluator` can finally start computing useful metrics, which we will explain below:

`average risk` is the average risk value of a robot task in the evaluated plan.
In order to calculate this value, the evaluator has to compute the risk of each task, but some robot tasks overlap with multiple human tasks. In that case, each possible overlap is considered and the final task risk value is the average of every overlap, weighted according to the overlap time interval.
When all task risk values are evaluated, the average is returned.

`max risk` is the risk value of the riskiest task in the current plan. The risk value is calculated in the same way as above. This value is needed because the average risk is a flat value and it doesn't allow us to know how risk is distributed across tasks. Knowing `max risk` helps us avoid plans where there are unreasonably high risk spikes.

`possible collisions` is an array which indicates, for each target type, how many times human and robot are assigned the same task at the same time. Collisions are the riskiest situations, so knowing which and how many there are allows us to greatly reduce risk. Furthermore, reducing collisions can avoid safety stops, thus making the plan also more efficient.

`best makespan` indicates the best estimate for the whole plan's makespan, which is calculated by choosing the biggest value between `human makespan` and `best robot makespan`.

`human makespan` is the time it takes for the human to complete all of the tasks that were allocated to them.

`best robot makespan` is the time it takes the robot to complete all of its tasks.

`estimated robot makespan` is a version of the robot makespan where possibly colliding tasks are penalized with a time multiplier. This could be used in some search strategies to better represent the impact that collisions may have on the plan execution time.

`robot tasks` is an array which stores, for each task type, how many tasks of that kind were allocated to the robot.

`human tasks` is an array which stores, for each task type, how many tasks of that kind were allocated to the human.

After computing this data, the evaluator stores it in a `NodeData` object, which will be assigned to the `SearchSpaceNode` as its `DomainSpecificMetric` attribute so that it can be referenced later by our `RiskAssessmentSearchStrategy`'s `compare` function.

After defining every component, we can now discuss what logic `RiskAssessmentSearchStrategy` uses in its `compare` method to optimize for both makespan and safety.
First, the two nodes' heuristic cost, as evaluated by PLATINUm, is compared in order to distinguish nodes with different levels of refinement.
Then, the dominance condition is checked to see if one of the nodes has both a lower `average risk` and a lower `best makespan`. If so, the dominant node is selected.
If no node is dominant, then the `max risk` attribute is used to filter out plans with unreasonable risk spikes. 
After that, the strategy chooses the plan with less possible collisions.
If both plans have the same amount of collisions, then the one with the lowest `best makespan` is selected.
If no node has been chosen yet, then the final decision is based on which one has the lowest `average risk`.

All of the components involved in finding plans with good tradeoffs between risk and efficiency have been defined. Their effectiveness will be showcased with the help of an experiment, later on in the paper.

## Utilities
### Python DDL generator
PLATINUm requires the problem domain to be described in a separate `.ddl` text file, which has its own syntax. This file specifies the components involved in the plan, the values that their state variables can assume, the transition rules and constraints between different state variables, the kind parameters that are accepted for each task, etc.

For the first few test runs, it was enough to write this file manually. However, as the plans start getting more complex, writing the `.ddl` file manually becomes repetitive, tedious and extremely time consuming. The file itself could get a thousand lines long and it would require us to specify each possible combination of parameters such as target, speed, trajectory, etc.

It seemed necessary for us to write an automated tool which would enable us to quickly make variations and explore the problem space. We chose to make this tool in python.

The tool is highly configurable and it lets us precisely tweak all the relevant properties of a planning problem. 

The first relevant parameter is a map, where we can specify what kind of targets are allowed and what intrinsic risk is associated with them. This makes it easy to add new targets with new intrinsic risk values. Different targets are allowed to have the same risk value. When adding a target or an intrinsic risk value, the parameter Enums in the java implementation must be updated to contain the same parameter and the associated risk and makespan data.

```python
MATERIAL_RISK_VALUES = {"foam": "low", "wood": "medium", "metal": "high"}
```

For each of these targets, we can also specify the amount of associated tasks. The tool also lets us specify how many of those tasks can be carried out by both the robot and the human, or just one of them. In the example below, three tasks for each target have to be carried out by the robot, and three by the human.

```python
MATERIAL_COORDINATION_AMOUNT_VALUES = {  
	"foam" : {"human": 3, "robot": 3, "": 0},  
	"wood" : {"human": 3, "robot": 3, "": 0},  
	"metal": {"human": 3, "robot": 3, "": 0},  
}
```

The tool also allows us to define all the possible trajectory and speed parameters, as well as the human's expertise level.
The python code is highly modular and it is based on layered functions with different abstraction levels, it is easy to modify any part of the generated `.ddl` files by changing the associated function in the generator. 

These functions are only responsible for generating working code, so the tool does a post processing step on the generated file content in order to fix the indentation and make it more human readable. However, in most if not all cases, quickly reading the configuration parameters of the python script was enough to understand exactly what the `.ddl` file describes to PLATINUm in hundreds of lines.

### The DataCollector Class
In order to test our search strategies and gather data about their effectiveness at finding plans with our desired qualities, we had to face some challenges.
Finding the solution for a complex plan can be a very hard and time consuming task for PLATINUm, especially for problems with very little constraints on the state variables. 
Having few constraints means that PLATINUm has to make a large number of choices and compare a lot of different partial plans.

Unfortunately, our problems were meant to test and showcase the search strategy's effectiveness at making choices and tradeoffs, so a high branching factor was not something we could avoid.
The time to plan a single schedule was not exceptionally high, but running extensive benchmarks meant that we had to run the program hundreds of times, for a total runtime of many hours.

The only solution was letting it run overnight, so a tool was needed to manage many runs over an extended period of time, collect the data and store it in a useful format.
This is where de `DataCollector` class comes in. This tool takes a set of search strategies and a folder with `.ddl` files in it as input, and it outputs a single csv files with all the data it collected during hours of planning.

### Python chart generator
Finally, we needed tools to visualize the collected data in order to know how our `RiskAssessmentSearchStrategy` compared to the other ones. 
The solution we opted for was a python tool capable of using the single `.csv` file generated by the `DataCollector` class to make many useful graphs.

The tool uses popular and well-tested data science libraries such as `pandas`, `numpy`, `matplotlib` and `seaborn`. It is composed of two main files.

The first file contains many separate and independent functions. All these functions have the same structure:

```python
def a_beautiful_graph(data: DataFrame, dest: str):
	# Make graph based on 'data'
	make_beautiful_graph(data)
	
    # Save the image in the given destination
    matplotlib.pyplot.savefig(dest)
```

Each of these functions takes a `pandas.DataFrame` and a destination path, then saves its graph in the given location.

The second file takes all the `.csv` files in a folder, and for each of them it loads and prepares the `DataFrame`. Then it dynamically calls all the functions in the first file (without being coupled with them) so that they save the chart inside the right folder for the current `.csv`.
This makes it very easy to add new charts, because all it takes is adding a function in the first file and the tool will automatically make that graph for every dataset that was previously collected.