![](https://github.com/jokoeliyanto/apache-airflow/blob/main/image/header%20modul2.png)

# DAGs
DAG adalah kepanjangan dari Directed Acyclic Graphs yang kita gunakan untuk membuat suatu workflow atau kita juga dapat memahami DAG sebagai sekumpulan dari Tasks. DAG inilah yang mencerminkan tentang alur dari workflow beserta relasi antar proses dan ketergantungan antar prosesnya.

Beberapa properties DAG yang paling utama adalah sebagai berikut,
1. `dag_id`: Pengidentifikasi unik di antara semua DAG,
2. `start_date`: Titik waktu awal di mana task pada DAG akan dimulai,
3. `schedule_interval`: Interval waktu sebuah DAG dieksekusi.


# DAG File

DAG file adalah suatu python script yang berfungsi untuk mengkonfigurasi sebuah DAG khusus dengan kode.

Terdapat lima langkah yang perlu diingat untuk menulis sebuah DAG atau workflow:

1. Langkah 1: Impor Modul yang dibutuhkan
```python
# Langkah 1: Impor Modul yang dibutuhkan
import airflow
from airflow import DAG
from airflow.operators.bash_operator import BashOperator

from datetime import timedelta  # untuk menghitung selisih waktu
```

2. Langkah 2: Mengatur Default Argument
```python
# Langkah 2: Mengatur Default Argument
default_args = {
    'owner': 'airflow',
    'start_date' : airflow.utils.dates.days_ago(2),
    'depends_on_past': False,
    'email': ['airflow@example.com'],
    'email_on_failure': False,
    'email_on_retry': False,


    'retries': 1,
    'retry_delay': timedelta(minutes=5)
}
```

3. Langkah 3: Menentukan Spesifikasi DAG
```python
# Langkah 3: Menentukan Spesifikasi DAG
dag = DAG(
    'tutorial',
    default_args=default_args,
    description = 'A simple tutorial DAG',

    schedule_interval=timedelta(days=1)
)
```

4. Langkah 4: Mendefinisikan Task

```python
# Langkah 4: Mendefinisikan Task
t1 = BashOperator(
    task_id='print_date',
    bash_command='date',
    dag=dag
)

t2 = BashOperator(
    task_id='sleep',
    depends_on_past=False,
    bash_command='sleep 5',
    dag=dag
)

templated_command = """
{ % for i in range(5) %}
    echo "{{ ds }}"
    echo "{{ macros.ds_add(ds, 7)}}"
    echo "{{ params.my_param }}"
{% endfor %}
"""

t3 = BashOperator(
    task_id='templated',
    depends_on_past=False,
    bash_command=templated_command,
    params = {'my_param': 'Parameter I passed in'},
    dag=dag
)
```

5. Langkah 5: Mengatur Dependensi

```python
# Langkah 5: Mengatur Dependensi
t1 >> [t2,t3]
```






