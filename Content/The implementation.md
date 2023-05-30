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
    - FromString constructor
    - Easy interface with platinum
- The NodeRiskEvaluator Class
    - Can be used by any planning strategy
    - Parses the whole plan into sequences of risk aware tasks
    - The risk factor of each robot task uses the weighted average of the parallel human tasks
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

Suitable plans are found through the use of a Search Strategy class, which determines how the planner explores the space of possible solutions in order to find a plan that achieves the desired goal state. The choice of search strategy can have a significant impact on the efficiency and effectiveness of the planning process. There are many different search strategies that can be used by planners, each with its own strengths and weaknesses. Some common search strategies include depth-first search, breadth-first search, best-first search, and iterative deepening search. The choice of search strategy depends on factors such as the size and complexity of the problem space, the available computational resources, and the desired trade-off between speed and optimality.

### Building on top of PLATINUm
In order to expand PLATINUm's capabilites with the ones provided by our model, we had to extend the two very important classes that were mentioned above: namely the Planner class and the SearchStrategy class.

PLATINUm's high degree of flexibility allowed us to make a new RiskAssessmentPlanner class very easily. The steps to make it work were configuring the Planner's modules using decorators. To be more specific, we configured the RiskAssessmentPlanner to have no planning timeout. This allows the Planner to keep looking for solutions for an unlimited amount of time. This was necessary because, as we'll explain later in the paper, the time to find a suitable plan can be quite high.
The other important configuration step in RiskAssessmentPlanner was setting its search strategy to be our own RiskAssessmentSearchStrategy.

The RiskAssessmentSearchStrategy is one of the most important parts of this project, since it is where some of the most crucial algorithmic parts of the implementation reside. The RiskAssessmentSearchStrategy expands the SearchStrategy class, and in order for it to work as we intend it to, we had to override some methods.

The first method we had to override was the `void enqueue(SearchSpaceNode node)` function. This function is responsible for receiving a partial plan and use it to make a wide variety of estimations about it. These estimations can be used later by the `int compare(SearchSpaceNode searchSpaceNode1, SearchSpaceNode searchSpaceNode2)` function to establish which partial plan the Problem Solving Component should consider to be better. The better node will be further refined and compared again with other partial plans, until a suitable plan is found.

The data that `enqueue` computes about a partial plan is then set as that node's `Object domainSpecificMetric` attribute, so that it can be later referenced in the `compare` function to establish which plan is preferred.

In our case, the `enqueue` function asks a different class to evaluate all of the relevant data and set it as the `domainSpecificMetric` attribute. We will elaborate on that different class in a moment.
The enqueue function also has the crucial role of computing the Heuristic Cost of a node, which is relevant in the compare 
%%Other Planners and Search Strategies%%

## Model implementation
### Risk factors


### The NodeRiskEvaluator Class


## Utilities
### Python DDL generator


### The DataCollector Class


### Python chart generator
