Airflow: DAGs 101

What's the role of the start date?

 Define when the DAG starts being scheduled
 Define the trigger interval
 Avoid running past non-triggered DAG Runs

What happens if you don't define a start date?

 Nothing, it's optional
 That raises an error

What's the role of tags?

 The allow to better organizing DAGs
 They allow filtering DAGs
 They prevent from running DAGs that do not belong to the current user
Question 4:  Correct answer
How can you avoid assigning the dag object to every task you create?

 with DAG(...)
 dag = ...
 @dag(..)
 You can't. You must assign the dag object to every task
Question 5:  Incorrect answer
What happens when two DAGs share the same DAG id?

 The two DAGs appear on the UI
 You get an error
 One DAG randomly shows up on the UI
Question 6:  Correct answer
Does task_a >> task_b >> task_c

is equivalent to

task_c << task_b << task_a?

 

 Yes
 No