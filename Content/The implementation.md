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

## Building on top of PLATINUm
### PLATINUm's architecture
As stated earlier, and in [[Timeline-based planning and execution with uncertainty. Theory, modeling methodologies and practice]], PLATINUm is a general-purpose framework for planning and execution with timelines under uncertainty, which implements a hierarchical solving procedure.
The PLATINUm framework has a layered architecture, which consists of two main components: the Representation Framework and the Problem Solving component.
The Representation Framework is responsible for representing the planning domain and problem in a structured manner. It consists of a plan database that stores information about the planning domain, such as state variables, decomposition rules, and synchronization rules. The plan database is structured in such a way that it can support hierarchical modeling of timeline-based domains.
The Problem Solving component is responsible for generating plans that satisfy the given planning goals. It consists of a planner that uses search heuristics to leverage the hierarchical structure of the planning domain during plan generation. The planner is capable of dealing with temporal uncertainty during plan generation by leveraging information about the temporal uncertainty of the planning domain.
The Problem Solving component works in conjunction with the Representation Framework to support hierarchical modeling and solving of timeline-based planning problems under uncertainty. The planner uses the information stored in the plan database, such as state variables, decomposition rules, and synchronization rules. The final 
The Planner's aim is to synthesize a set of operations that, given an initial state, allow the system to reach a desired goal state. The reasoning process relies on a model which represents a general description of the problem to solve. The model provides a representation of the environment in terms of the possible states of the world and the actions the system can perform to interact with the environment.

### Extending the Planner class


### Extending the SearchStrategy Class


## Model implementation
### Risk factors


### The NodeRiskEvaluator Class


## Utilities
### Python DDL generator


### The DataCollector Class


### Python chart generator
