# Scheduling Tasks

On how to schedule tasks 


___
## Dagster
[Documentation]("https://docs.dagster.io/getting-started")

Dagster is a data orchestrator. It lets you define jobs in terms of the data flow between logical components called ops. These jobs can be developed locally and run anywhere.
Dagster uses decorators like `@op` to decorate functions and `@job` to group `@op` into a single flow.


```python
from dagster import job, op

@op
def get_name():
    return "dagster"

@op
def hello(name: str):
    print(f"Hello, {name}!")

@job
def hello_dagster():
    hello(get_name())
```

Then run with the follow command
```commandline
dagit -f hello_world.py
```
Which will serve up a web UI

![Dagster](https://docs.dagster.io/_next/image?url=https%3A%2F%2Fdagster-docs-versioned-content.s3.us-west-1.amazonaws.com%2Fversioned_images%2F0.13.12%2Fgetting-started%2Fdagit-run.png&w=3840&q=75)


___
## Prefect
[Documentation]("https://orion-docs.prefect.io/")

Prefect Orion is the second-generation workflow orchestration engine from Prefect, now available as a technical preview.
Orion has been designed from the ground up to handle the dynamic, scalable workloads that the modern data stack demands. Powered by a brand-new, asynchronous rules engine, it represents an enormous amount of research, development, and dedication to a simple idea:
You should love your workflows again.

```python
from prefect import flow, task
from typing import List
import httpx


@task(retries=3)
def get_stars(repo: str):
    url = f"https://api.github.com/repos/{repo}"
    count = httpx.get(url).json()["stargazers_count"]
    print(f"{repo} has {count} stars!")


@flow(name="Github Stars")
def github_stars(repos: List[str]):
    for repo in repos:
        get_stars(repo)


# run the flow!
github_stars(["PrefectHQ/Prefect", "PrefectHQ/miter-design"])
```

Then run with the follow command
```commandline
prefect orion start
```
Which will serve up a web UI

![Prefect Orion](https://orion-docs.prefect.io/img/tutorials/hello-orion-dashboard.png)


---
## Celery
[Documentation](https://docs.celeryproject.org/en/latest/getting-started/first-steps-with-celery.html#application)


```python
from celery import Celery

app = Celery('tasks', broker='pyamqp://guest@localhost//')

@app.task
def add(x, y):
    return x + y
```

---
## Schedule
[Documentation](https://schedule.readthedocs.io/en/stable/)

Python job scheduling for humans. Run Python functions (or any other callable) periodically using a friendly syntax.

- A simple to use API for scheduling jobs, made for humans.
- In-process scheduler for periodic jobs. No extra processes needed!
- Very lightweight and no external dependencies.
- Excellent test coverage.
- Tested on Python 3.6, 3.7, 3.8 and 3.9

Schedule is not a ‘one size fits all’ scheduling library. This library is designed to be a simple solution for simple scheduling problems. You should probably look somewhere else if you need:

- Job persistence (remember schedule between restarts)
- Exact timing (sub-second precision execution)
- Concurrent execution (multiple threads)
- Localization (time zones, workdays or holidays)

Schedule does not account for the time it takes for the job function to execute. To guarantee a stable execution schedule you need to move long-running jobs off the main-thread (where the scheduler runs). See Parallel execution for a sample implementation.

Simple Single threaded execution:
```python
import schedule
import time

def job():
    print("I'm working...")

schedule.every(10).minutes.do(job)
schedule.every().hour.do(job)
schedule.every().day.at("10:30").do(job)
schedule.every().monday.do(job)
schedule.every().wednesday.at("13:15").do(job)
schedule.every().minute.at(":17").do(job)

while True:
    schedule.run_pending()
    time.sleep(1)
```

Multi threaded execution:

[More Info](https://schedule.readthedocs.io/en/stable/parallel-execution.html)

```python
import threading
import time
import schedule

def job():
    print("I'm running on thread %s" % threading.current_thread())

def run_threaded(job_func):
    job_thread = threading.Thread(target=job_func)
    job_thread.start()

schedule.every(10).seconds.do(run_threaded, job)
schedule.every(10).seconds.do(run_threaded, job)
schedule.every(10).seconds.do(run_threaded, job)
schedule.every(10).seconds.do(run_threaded, job)
schedule.every(10).seconds.do(run_threaded, job)


while 1:
    schedule.run_pending()
    time.sleep(1)
```