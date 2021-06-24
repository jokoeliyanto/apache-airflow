![](https://github.com/jokoeliyanto/apache-airflow/blob/main/image/header%20modul2.png)

# DAG
DAG adalah kepanjangan dari Directed Acyclic Graphs yang kita gunakan untuk membuat suatu workflow atau kita juga dapat memahami DAG sebagai sekumpulan dari Tasks. DAG inilah yang mencerminkan tentang alur dari workflow beserta relasi antar proses dan ketergantungan antar prosesnya.

!["Contoh DAG"](https://github.com/jokoeliyanto/apache-airflow/blob/main/image/DAG%20Example2.png)

DAG menentukan dependensi antara task, dan urutan untuk menjalankannya dan menjalankan percobaan ulang. Task sendiri menjelaskan apa yang harus dilakukan, baik itu mengambil data, menjalankan analisis, memicu sistem lain, atau lebih.

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

# Praktik Apache Airflow Dasar

Pada repository ini terdapat 3 buah script python yang berbentuk DAG file:
1. DAG Hello World : Merupakan DAG pertama yang akan kita buat sebagai bahan pembelajaran
2. DAG 2
3. DAG 3
4. DAG 4
5. DAG 5
6. DAG 6




