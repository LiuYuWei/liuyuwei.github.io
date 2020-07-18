---
layout: post
title:  "Introduction to airflow"
date:   2020-07-18 10:09
categories: tool
permalink: /archivers/introduction-to-airflow
---
# Introduction to airflow.

Airflow是由Airbnb開源的一款工具，是一個以程式設計方式創作，安排和監控工作流程的平臺，當工作流被定義為程式碼時，它們變得更易於維護，可版本化，可測試和協作。

## 設計原則

1. 動態：Airflow配置為程式碼（Python），允許動態生成pipeline。 這允許編寫動態例項化的pipelines程式碼。
2. 優雅：Airflow精益而明確。 使用強大的Jinja模板引擎將引數化指令碼內置於Airflow的核心。
3. 可擴充套件：Airflow可輕鬆定義自己的opertators，執行程式並擴充套件庫，使其符合適合您環境的抽象級別，具有模組化體系結構，並使用訊息佇列來協調任意數量的工作者。 Airflow已準備好擴充套件到無限遠。

## 如果想用docker使用airflow
[這個版本](https://github.com/puckel/docker-airflow)滿推薦的，可以使用這個
操作方式可以看[這裡](https://towardsdatascience.com/getting-started-with-airflow-using-docker-cd8b44dbff98)
```bash
$ docker run -d -p 8080:8080 -v /path/to/dags/on/your/local/machine/:/usr/local/airflow/dags puckel/docker-airflow webserver
```

## Directed Acyclic Graph (DAG):
運行task的總和

## 運行一個task
- `Helloworld.py`的內容如下:

```
from airflow import DAG
from airflow.operators import BashOperator
from datetime import datetime, timedelta

# Following are defaults which can be overridden later on
default_args = {
    'owner': 'manasi',
    'depends_on_past': False,
    'start_date': datetime(2016, 4, 15),
    'email': ['manasidalvi14@gmail.com'],
    'email_on_failure': False,
    'email_on_retry': False,
    'retries': 1,
    'retry_delay': timedelta(minutes=1),
}

dag = DAG('Helloworld', default_args=default_args)

# t1, t2, t3 and t4 are examples of tasks created using operators

t1 = BashOperator(
    task_id='task_1',
    bash_command='echo "Hello World from Task 1"',
    dag=dag)

t2 = BashOperator(
    task_id='task_2',
    bash_command='echo "Hello World from Task 2"',
    dag=dag)

t3 = BashOperator(
    task_id='task_3',
    bash_command='echo "Hello World from Task 3"',
    dag=dag)

t4 = BashOperator(
    task_id='task_4',
    bash_command='echo "Hello World from Task 4"',
    dag=dag)

t2.set_upstream(t1)
t3.set_upstream(t1)
t4.set_upstream(t2)
t4.set_upstream(t3)
```

把`Helloworld.py`放到`/path/to/dags/on/your/local/machine/`後運行以下指令

```
$ airflow test Helloworld task_1 2015-06-01
```

## 想要有趣的看airflow是甚麼?
https://leemeng.tw/a-story-about-airflow-and-data-engineering-using-how-to-use-python-to-catch-up-with-latest-comics-as-an-example.html
