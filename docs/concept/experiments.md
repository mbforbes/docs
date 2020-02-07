# Experiments

![Parallel wordcount experiment graph](../images/parallel-wordcount.png)

A Beaker experiment may contain multiple tasks.
A task may depend on the results of another task in its experiment.
In this example, the `merge` task depends on the results of the `count-1`, `count-2`, and `count-3` tasks.
The count tasks are executed in parallel and the merge task runs automatically once they complete.
