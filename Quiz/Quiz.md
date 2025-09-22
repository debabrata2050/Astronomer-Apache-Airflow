# Airflow 101 (Airflow 3)

## Airflow: DAGs 101

#### 1. What's the role of the start date?
- [x] Define when the DAG starts being scheduled ✅
- [ ] Define the trigger interval
- [ ] Avoid running past non-triggered DAG Runs

#### 2. What happens if you don't define a start date?
- [x] Nothing, it's optional ✅
- [ ] That raises an error

#### 3. What's the role of tags?
- [x] The allow to better organizing DAGs ✅
- [x] They allow filtering DAGs ✅
- [ ] They prevent from running DAGs that do not belong to the current user
 
#### 4. How can you avoid assigning the dag object to every task you create?
- [x] with DAG(...) ✅
- [ ] dag = ...
- [x] @dag(..) ✅
- [ ] You can't. You must assign the dag object to every task

#### 5. What happens when two DAGs share the same DAG id?
- [ ] The two DAGs appear on the UI
- [ ] You get an error
- [x] One DAG randomly shows up on the UI ✅
 
#### 6. Does `task_a >> task_b >> task_c` is equivalent to `task_c << task_b << task_a?`
- [x] Yes ✅
- [ ] No

***
