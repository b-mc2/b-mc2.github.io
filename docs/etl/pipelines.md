# ETL Pipelines
An ETL pipeline is the set of processes used to move data from a source or multiple sources into a database such as a data warehouse.
ETL stands for “extract, transform, load,” the three interdependent processes of data integration used to pull data from
one database and move it to another. Once loaded, data can be used for reporting, analysis, and deriving actionable business insights. 

## Luigi
[Documentation]("https://luigi.readthedocs.io/en/stable/")
Luigi is a Python (2.7, 3.6, 3.7 tested) package that helps you build complex pipelines of batch jobs. It handles 
dependency resolution, workflow management, visualization, handling failures, command line integration, and much more.

```python
"""
You can run this example like this:
    .. code:: console
            $ luigi --module examples.hello_world examples.HelloWorldTask --local-scheduler
If that does not work, see :ref:`CommandLine`.
"""
import luigi


class HelloWorldTask(luigi.Task):
    task_namespace = 'examples'

    def run(self):
        print("{task} says: Hello world!".format(task=self.__class__.__name__))


if __name__ == '__main__':
    luigi.run(['examples.HelloWorldTask', '--workers', '1', '--local-scheduler'])
```

___
## Bonobo
[Documentation]("https://www.bonobo-project.org/")

Bonobo is a lightweight Extract-Transform-Load (ETL) framework for Python 3.5+.
It provides tools for building data transformation pipelines, using plain python primitives, and executing them in parallel.
Bonobo is the swiss army knife for everyday's data.

```python
import datetime
import time

import bonobo


def extract():
    """Placeholder, change, rename, remove... """
    for x in range(60):
        if x:
            time.sleep(1)
        yield datetime.datetime.now()


def get_graph():
    graph = bonobo.Graph()
    graph.add_chain(extract, print)

    return graph


if __name__ == "__main__":
    parser = bonobo.get_argument_parser()
    with bonobo.parse_args(parser):
        bonobo.run(get_graph())
```


___