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

## Airflow: DAG Scheduling

#### 1. If a DAG runs every day at midnight with a `start_date=datetime(2025, 1, 1)`, when will the 3rd DAG run be triggered?
- [ ] 2025-01-01 00:00
- [ ] 2025-01-02 00:00
- [x] 2025-01-03 00:00 ✅
- [ ] 2025-01-04 00:00

#### 2. If a DAG with a "@daily" schedule is triggered at 00:00 on 2025-02-02, what's the logical date value for this DAG Run?
- [x] 2025-02-01 00:00 ✅
- [ ] 2025-02-02 00:00
- [ ] 2025-02-03 00:00

#### 3. If a DAG with a "@daily" schedule is triggered at 00:00 on 2025-02-02, what's the `data_interval_end` value for this DAG Run (assuming data intervals are used)?
- [ ] 2025-02-01
- [x] 2025-02-02 ✅
- [ ] 2025-02-03
- [ ] 2025-02-04

#### 4. With `catchup=False`, what happens when you run your DAG for the first time, and it has a `start_date` defined as 30 days ago?
- [ ] Nothing
- [x] The latest non-triggered DAG Run is triggered ✅
- [ ] All non-triggered DAG Runs get triggered

#### 5. `logical_date = data_interval_start = data_interval_end` by default?
- [ ] Yes
- [x] No ✅

---

## Airflow: XComs 101

#### 1. Select the 4 factors that define the uniqueness of an XCom
- [x] key ✅  
- [ ] value
- [ ] timestamp
- [x] dag_id ✅
- [x] task_id ✅
- [x] logical_date ✅

#### 2. Is it possible to push an XCom without explicitly specifying a key?
- [x] Yes ✅  
- [ ] No  

#### 3. An XCom is pushed into...
- [ ] The scheduler  
- [ ] The worker  
- [ ] The webserver  
- [x] The database ✅  

#### 4. With Postgres, can you share 2GB of data between 2 tasks with an XCom?
- [ ] Yes  
- [x] No ✅  

#### 5. How does the Scheduler know which XCom to choose for a given DAGRun when multiple XComs have the same key, dag_id, and task_id?
- [ ] It selects one XCom randomly  
- [x] It selects the XCom based on the logical date ✅  
- [ ] You can't have multiple XComs with the same key, dag_id and task_id  

---

# Airflow: Sensors

## Waiting for files in a S3 bucket 📂

#### 1. You can't find the connection type Amazon Web Services. What should you do?
- [x] Install `apache-airflow-providers-amazon` ✅
- [ ] Install `boto3`
- [ ] Use the `http` connection type

#### 2. If the file never arrives in the S3 bucket, when will the `S3KeySensor` time out?
- [ ] In 24 hours
- [x] In 7 days ✅
- [ ] Never

#### 3. Does the Sensor instantly detect the file when it arrives in the bucket?
- [ ] Yes  
- [x] No, it depends on the `poke_interval` ✅

---

# [Airflow 2] Airflow: Debug DAGs

#### 1. This DAG doesn’t show up on the UI. Why?
```python
from airflow.decorators import dag,task
from pendulum import datetime

@dag(
    'test_dag',
    start_date = datetime(2023,3,1)
)
def test_dag():
     @task
    def test_task():
        return ' Hello World'
    test_task()
```
- [ ] The schedule parameter is missing
- [ ] The start_date is in the past
- [x] test_dag() is not called ✅

#### 2. On manually triggering this DAG you don't see any task execution. Why?
```python
from airflow.decorators import dag,task
from pendulum import datetime

@dag(
    'test_dag',
    start_date = datetime(3030,3,1)
)
def test_dag():
     @task
    def test_task():
        return 'Hello World'
    test_task()

test_dag()
```
- [ ] There is no end_date
- [x] The start_date is in the future ✅
- [ ] Because there is only one task and no proper pipeline

#### 3. How many running DAG runs will you get as soon as you unpause this DAG?
```python
from airflow.decorators import dag,task
from pendulum import datetime

@dag(
    'test_dag',
    start_date = datetime(2023,1,1),
    schedule = '@daily',
    catchup = False
)
def test_dag():
    @task
    def my_task():
        return 'Hello World'
    my_task()

test_dag()
```
- [ ] 0
- [x] 1 ✅
- [ ] 16
- [ ] 32

#### 4. You just finished writing your DAG and saved the file in the dags folder.  
How long will it take to appear on the UI?
- [x] By default it may take up to 5 minutes or more. ✅
- [ ] It will be added instantly
- [ ] It may take upto 30 seconds.

#### 5. Is it possible to run tasks that are dependant on different versions of Python in the same DAG?
- [x] Yes ✅
- [ ] No
