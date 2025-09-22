# 🌬️ Airflow — DAGs 101

## 🧰 Default Arguments in Airflow

- You can apply arguments to tasks like:  
  🔁 `retries`  
  ⏳ `execution_timeout`  
  📧 `email_on_failure`, etc.

- 🛑 **Default behavior**:  
  Tasks have `retries=0` (fail immediately on error).

### 🔄 Problem with Repetition

Manually setting `retries=3` on every task is repetitive:

```python
task_a = PythonOperator(task_id='task_a', python_callable=print_a, retries=3)
```

Doing this for every task? 😩 Not ideal.

✅ Solution: `default_args`  
Avoid repetition by passing a default_args dictionary to the DAG:

```python
default_args = {
    'retries': 3,
}

@dag('my_dag',
     start_date=datetime(2025, 1, 1),
     default_args=default_args,
     description='A simple tutorial DAG',
     tags=['data_science'],
     schedule='@daily')
def my_dag():
    task_a = PythonOperator(task_id='task_a', python_callable=print_a)
    task_b = PythonOperator(task_id='task_b', python_callable=print_b)
    task_c = PythonOperator(task_id='task_c', python_callable=print_c)
    task_d = PythonOperator(task_id='task_d', python_callable=print_d)
    task_e = PythonOperator(task_id='task_e', python_callable=print_e)

    chain(task_a, [task_b, task_c], [task_d, task_e])
```
🎉 All tasks inherit retries=3 — no repetition needed.


## 📘 DAG (Directed Acyclic Graph)

- 🔑 **Unique Identifier**  
  Every DAG must have a **unique `dag_id`**.

- 🕐 **Start Date (Optional)**  
  Set using `start_date`. Defaults to `None` if not specified.

- ⏰ **Schedule Interval (Optional)**  
  Defines how often the DAG should run  
  _Example_: `'@daily'`, `'0 12 * * *'`.

- 📝 **Description & Tags**  
  Strongly recommended for:
  - Better documentation
  - Easier filtering in the Airflow UI

---

## ✅ Tasks in a DAG

- 🔑 **Unique Task ID**  
  Each task must have a **unique `task_id`** within its DAG.

- 🧰 **Where to Start**  
  Check out the [Astronomer Registry](https://registry.astronomer.io/) to explore available operators, sensors, and components.

- ⚙️ **Default Arguments (`default_args`)**  
  Apply shared parameters to multiple tasks using a dictionary.  
  _Common keys_: `start_date`, `retries`, `owner`, etc.

---

## 🔗 Defining Task Dependencies

➡️ **Bitshift Operators**  
  Use `>>` and `<<` to define task order.  
  ```python
  task1 >> task2  # task1 runs before task2
  ```
📚 **Lists of Tasks**  
  Define dependencies involving multiple tasks:
  ```python
  [task1, task2] >> task3

  ```
  
⛓️ **`chain()` Utility Function**  
  Cleanly define complex chains:
  ```python
  from airflow.models.dag import chain
  chain(task1, [task2, task3], task4)
  ```

  ****

# 🛠️ Practice: Creating your second data pipeline
## 📘 Introduction  
In this activity, you will create a second data pipeline from scratch that:

- 📄 Create a file using the `BashOperator`  
- 🐍 Read the file using the `PythonOperator`

At the end of this activity, you will be able to:

- 📚 Find the documentation you need to implement an operator  
- 💻 Execute Bash commands from your DAG  
- 🧩 Create a DAG following best practices  
- 🐍 Execute Python functions from your DAG  
- 🔗 Define dependencies between your tasks


## ✅ Prerequisites  
- 🖥️ Local development environment with the Astro CLI

## 🧭 Instructions

### 🪜 Step 1: Defining the DAG

📄 Create a dag file `check_dag.py`. Then, define a DAG with the identifier `check_dag`.

🕛 We expect `check_dag` to run **every day at midnight** from the **1st of January 2025**.

📝 Also, the DAG should have the following description:  
`"DAG to check data"` and belongs to the `data_engineering` team.


### 🪜 Step 2: Creating the Tasks

We want to add **three tasks** to this DAG.

1. 📝 The first task executes the following Bash command:
   ```bash
   echo "Hi there!" > /tmp/dummy
   ```
   ✅ This creates a file `dummy` in the `/tmp` directory with `"Hi there!"`.  
  🏷️ The task's name should be `create_file`  
  ➕ Use the `@task.bash` decorator for Bash commands.
2. 🧪 The second task executes the following Bash command:
   ```bash
   test -f /tmp/dummy
   ```
   ✅ This verifies that the file `dummy` exists in the `/tmp` directory.  
   🏷️ The task's name should be `check_file`
3. 🐍 The third task executes the following Python function:
   ```python
   print(open('/tmp/dummy', 'rb').read())
   ```
   ✅ This reads and prints on the standard output the content of the `dummy` file.

### 🪜 Step 3: Defining the dependencies

🔗 You should define the dependencies to get the order of execution:

```
create_file → check_file → read_file
```

🧪 Finally, make sure that you have no errors by going to the Airflow UI.  
👀 You should be able to see your DAG.

## 🧾 Final DAG Code: `check_dag.py`:

```python
from airflow.sdk import dag, task
from pendulum import datetime

@dag(
    schedule="@daily",
    start_date=datetime(2025, 1, 1),
    description="DAG to check data",
    tags=["data_engineering"],
)

def check_dag():

    @task.bash
    def create_file():
        return 'echo "Hi there!" >/tmp/dummy'

    @task.bash
    def check_file_exists():
        return 'test -f /tmp/dummy'
    
    @task
    def read_file():
        print(open('/tmp/dummy', 'rb').read())

    create_file() >> check_file_exists() >> read_file()

check_dag()
```
