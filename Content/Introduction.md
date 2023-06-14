The wide use of robots in production plants has greatly improved the throughput and efficiency of factories in recent years. The introduction of robots in these environments allows businesses to automate hard, dangerous and repetitive tasks. 
However, this transition requires expensive and radical upgrades, and it sacrifices the flexibility and dexterity inherent to human workers. Furthermore, these fully automated production chains could be dangerous to nearby humans.

This is why the field of HRC robotics is rapidly rising in popularity, as it allows businesses to improve their efficiency without sacrificing flexibility and without huge upfront costs.
Chapter 1 will further discuss and introduce the crucial advantages of HRC environments, as well as its main drawback: Safety.

By definition, HRC environments involve proximity between robots and human worker, which everyone would recognize as a relevant safety concern. Since the advantages of HRC are highly sought-after, there have been many attempts at coming up with ways to make HRC safer and easily adoptable.
However, many of these methods end up sacrificing efficiency or flexibility in order to make HRC safer, or they resort to reducing the level of collaboration altogether. Our effort in chapter 3 involves an in-depth analysis of the state of the art in HRC safety standards, and follows it up with a detailed set of requirements for a general and flexible risk awareness model. One of the relevant requirements for this model is that it should enable safety measures that don't compromise the high usefulness of HRC.

After defining our scope and requirements for a safety awareness model, we propose our own framework in chapter 4. Our framework is built off of widely used standards and is suitable for a broad range of applications.
Specifically, we wrote chapters 5 and 6 to showcase how this model should be applied to use cases such as execution time and plan time, respectively.

Since the next part of the thesis requires an understanding of what AI task planning is, we wrote a general introduction to the field in chapter 2. This chapter also explains how timeline-based AI planning works, and it provides a general overview of PLATINUm. PLATINUm is the timeline-based AI planner that we used as a baseline to build a plan time implementation of our suggested model.

From chapter 7 onwards, we discuss the implementation details of a multi-objective planner which uses the model we proposed to make schedules for HRC scenarios. This planner is able to optimize for both safety and efficiency, and shows that it is possible to improve HRC safety without compromising the properties that make it unique, such as efficiency, flexibility and lower upfront costs. 

After walking through the implementation details, we showcase the rules and results of an experiment we made to prove the effectiveness of our model and implementation. The experiment consists in collecting a great amount of data about numerous simulations involving our planner and 3 others. We then provide data visualizations useful to make comparisons between the different planners. Our planner strikes the best balance between safety and efficiency, especially when in situations with a low amount of constraints.

In chapter 9, we also provide support material useful for making presentations about our model, experiment, and results.

Finally, we give some considerations about the project and discuss relevant improvements and changes that could be worked on in the future.