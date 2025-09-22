# Airflow 101 (Airflow 3) 🚀

## Airflow: UI

#### 1. What's the best view to use to visualize your assets?  
- [x] Asset View ✅  
- [ ] DAGs View
- [ ] Graph View

#### 2. What's the best view to check the historical states of the DAG Runs and Task Instances for a given DAG?
- [x] Grid View ✅
- [ ] Graph View
- [ ] DAGs View

#### 3. What's the best view to get overall metrics of your Airflow instance?
- [x] The Home View ✅
- [ ] Code View
- [ ] Asset View

#### 4. What's the best view to know if code updates have been applied?
- [ ] Home View
- [x] Code View ✅  
- [ ] Graph View

#### 5. What does the last column on the DAG View show?
- [x] DAG run states of current and previous DAG Runs for a given DAG ✅  
- [ ] Task's states of the current or latest DAG Run for a given DAG

---

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

---

