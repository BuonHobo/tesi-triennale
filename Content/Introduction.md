The wide use of robots in production plants has greatly improved the throughput and efficiency of factories in the recent years. The introduction of robots in this environments allows businesses to automate hard, dangerous and repetitive tasks. 
However, this transition requires expensive and radical upgrades, and it sacrifices the flexibility and dexterity inherent to human workers. Furthermore, these fully automated production chains could be dangerous to nearby humans.

This is why the field of HRC robotics is rapidly rising in popularity, as it allows businesses to improve their efficiency without sacrificing flexibility and without huge upfront costs.
Chapter 1 will further discuss and introduce the crucial advantages of HRC environments, as well as its main drawback: Safety.

By definition, HRC environments involve close proximity between robots and human worker, which everyone would recognize as a relevant safety concern. Since the advantages of HRC are highly sought-after, there have been many attempts at coming up with ways to make HRC safer and easily adoptable.
However, many of these methods end up sacrificing efficiency or flexibility in order to make HRC safer, or they resort to reducing the level of collaboration altogether. Our effort in chapter 3 involves an in-depth analysis of the state of the art in HRC safety standards, and follows it up with a detailed set of requirements for a general and flexible risk awareness model.

After defining our scope and requirements for a safety awareness model, we propose our own framework in chapter 4. Our framework is built off of widely used standards and is suitable for a broad range of applications.
Specifically, we wrote chapters 5 and 6 to showcase how this model should be applied to use cases such as execution time and plan time, respectively.

Since the next part of the thesis requires an understanding of what AI task planning is, we wrote a general introduction to the field in chapter 2. This chapter also explains how timeline-based AI planning works, and it provides a general overview of PLATINUm. PLATINUm is the timeline-based AI planner that we used as a baseline to build a plan time implementation of our proposed model.

From chapter 7 onwards, we discuss the implementation details of a multi-objective planner which uses the model we proposed to make schedules for HRC scenarios. This planner is able to optimize for both safety and efficiency, and shows that it is possible to im 


%%Our model does not sacrifice efficiency%