![](https://github.com/jokoeliyanto/apache-airflow/blob/main/image/header%20modul1.png)

# Pendahuluan
Pada era industri 4.0 ini, keberlimpahan data merupakan suatu keniscayaan. Berbagai tugas mulai daru pengumpulan, pemrosesan, analisis, hingga pemodelan data dibutuhkan setiap saat. Diperlukan suatu sistem yang memudahkan berbagai tugas tersebut secara otomatis karena tidak memungkinkan dilakukan secara manual oleh manusia. Hal tersebut disebabkan oleh Big Data. Karakteristik dari Big Data adalah bukan hanya memiliki volume yang besar namun bermacam jenis dengan kecepatan generasi data yang tinggi. Sebuah sistem untuk mengatus tugas-tugas yang berupa tahanpan-tahapan tertentu biasa disebut sebagai Workflow Management Systems (WFMS).

# Workflow Management Systems(WFMS)

Workflow Management Systems (WFMS) adalah suatu cara untuk mengatur tugas-tugas yang harus dikerjakan untuk mencapai tujuan/tahapan tertentu agar tugas-tugas tersebut tidak repetitive. Secara lebih rinci, **Workflow** merupakan serangkaian tugas-tugas yang harus dikerjakan entah secara berurutan maupun tidak, biasanya tugas-tugas tersebut memiliki ketergantungan satu sama lain. **Management** adalah cara mengatur tugas-tugas diatas. Sedangkan **System** adalah serangkaian komponen yang saling berkaitan demi mencapai tujuan tertentu.


# Apache Airflow

Airflow adalah platform yang dibuat oleh komunitas untuk menulis, menjadwalkan, dan memantau alur kerja secara terprogram.  Secara khusus Apache Airflow dibuat untuk memudahkan proses ETL(Extract, Transform, and Load). Hal ini disampaikan oleh Maxime Beauchemin yaitu si pencipta Airflow dalam artikelnya yang berjudul [The Rise of the Data Engineer](https://www.freecodecamp.org/news/the-rise-of-the-data-engineer-91be18f1e603/)

*We've also observed a general shift away from drag-and-drop ETL (Extract Transform and Load) tools towards a more programmatic approach. Product know-how on platforms like Informatica, IBM Datastage, Cognos, AbInitio or Microsoft SSIS isn't common amongst modern data engineers, and being replaced by more generic software engineering skills along with understanding of programmatic or configuration driven platforms like Airflow, Oozie, Azkabhan or Luigi. It's also fairly common for engineers to develop and manage their own job orchestrator/scheduler.*

Pernyataan di atas pada intinya adalah bahwa ETL pada era Big Data saat ini jauh lebih kompleks, sehingga dibutuhkan tools untuk hal tersebut.

Sebagaimana suatu Workflow Management Systems, Airflow dapat difungsikan untuk beberapa hal sebagai berikut:
1. Mengelola penjadwalan dan menjalankan task untuk pipeline data.
2. Memastikan task berurutan dengan benar berdasarkan dependensinya (Note: Task bisa berupa perintah apapun, tidak melulu merupakan sebuah pipeline data.)
3. Mengelola alokasi sumber daya melalui penjadwalan resource dengan mematikan dan menghidupkan resource mesin melalui task perintah.
4. Menyediakan mekanisme untuk melacak kondisi berbagai task dan memulihkannya dari kegagalan task.

Apache Airflow memiliki banyak fitur, dan didukung dengan integrasi tool eksternal yang banyak seperti: Hive, Pig, Google BigQuery, Amazon Redshift, Amazon S3, dst. Selain itu, hal terpenting lainnya adalah bahwa Airflow memiliki keunggulan pada scaling yang tak terbatas. Maka menjadi wajar jika Airflow menjadi pilihan yang tepat untuk membangun data pipeline saat ini. Beikut adalah berbagai implementasi Airflow:
1. Data Warehousing
2. Data Export from/to production
3. ETL Process
4. Infrastructure Monitoring
5. Experimentation
6. Growth Analytics
7. Search Ranking
8. Operational Work
9. Engangemet Analytics
10. Anomaly Detection
11. Sessionization
12. Email Targeting

# Terminologi Pada Airflow
Sebelum membahas lebih lanjut tentang konsep-konsep dasar pada Airflow maka berikut beberapa terminologi singkat yang perlu diketahui.
1. DAG (Directed Acyclyc Graph) adalah graf asiklik terarah yang digunakan untuk menggambarkan workflow.
2. Operator berfungsi untuk menjalankan suatu tugas di dalamnya. Operator juga menjelaskan tentang Tasks(tugas-tugas) di dalamnya.
3. Task adalah istilah untuk "tugas" yang kemudian dijalankan oleh operator. Task bisa berupa Python function atau eksternal yang bisa dipanggil. Tasks ini lebih baik bersifat idempotent. 
6. Task Instance adalah istilah untuk Worker adalah istilah untuk serangkaian tugas tertentu: DAG + TASK + POINT IN TIME
7. Workflow adalah kumpulan task yang memiliki dependensi terarah. Disebut juga sebagai DAGs yang merupakan kombinasi dari [1-6].


# Arsitektur Airflow
Airflow adalah platform untuk membangun dan menjalankan sebuah work flow. Work Flow direpresentasikan sebagai DAG (a Directed Acyclic Graph), dan berisi bagian-bagian individual dari pekerjaan yang disebut Task, disusun dengan dependensi dan aliran data yang diperhitungkan.

![](https://github.com/jokoeliyanto/apache-airflow/blob/main/image/DAG%20Example2.png)

DAG menentukan dependensi antara task, dan urutan untuk menjalankannya dan menjalankan percobaan ulang. Task sendiri menjelaskan apa yang harus dilakukan, baik itu mengambil data, menjalankan analisis, memicu sistem lain, atau lebih.

Airflow secara umum terdiri dari komponen-komponen berikut:

* **Scheduler**, yang menangani pemicu(triger) alur kerja terjadwal, dan mengirimkan task ke eksekutor untuk dijalankan.

* Sebuah **eksekutor**, yang menangani Task yang sedang berjalan. Dalam instalasi Airflow default, eksekutor menjalankan semua yang ada di dalam schedulerl, tetapi sebagian besar eksekutor yang sesuai dengan produksi sebenarnya mendorong eksekusi task ke worker.

* **Server web**, yang menyajikan antarmuka pengguna yang berguna untuk memeriksa, memicu(triger), dan men-debug perilaku DAG dan task.

* **Folder file DAG**, dibaca oleh scheduler dan eksekutor (dan semua worker yang dimiliki eksekutor)

* **Metadata Database**, digunakan oleh scheduler, eksekutor, dan web server untuk menyimpan status.

![](https://github.com/jokoeliyanto/apache-airflow/blob/main/image/arsitektur%20airflow2.png)

Sebagian besar eksekutor umumnya juga mengenal komponen lainnya dan memungkinkan mereke berkomunikasi dengan worker eksekutor tersebut - seperti antrian task - namun kita masih bisa menganggap bahwa eksekutor dan worker di dalamnya merupakan sebuah komponen logis tunggal di Airflow secara keseluruhan yang menangani task yag sesungguhnya.

# DAG

DAG adalah kepanjangan dari Directed Acyclic Graphs yang kita gunakan untuk membuat suatu workflow atau kita juga dapat memahami DAG sebagai sekumpulan dari Tasks. DAG inilah yang mencerminkan tentang alur dari workflow beserta relasi antar proses dan ketergantungan antar prosesnya.

Beberapa properties DAG yang paling utama adalah sebagai berikut,
1. `dag_id`: Pengidentifikasi unik di antara semua DAG,
2. `start_date`: Titik waktu awal di mana task pada DAG akan dimulai,
3. `schedule_interval`: Interval waktu sebuah DAG dieksekusi.

Meskipun DAG digunakan untuk mengatur task dan mengatur konteks eksekutorya, DAGs tidak melakukan perhitungan yang sebenarnya. Sebagai gantinya, task adalah elemen dari Airflow yang berfungsi untuk melakukan pekerjaan yang ingin dilakukan. Terdapat dua tipe task:
1. Task dapat melakukan beberapa operasi eksplisit, di mana, dalam hal ini, mereka disebut sebagai **operator**, atau,
2. Task dapat menghentikan sementara pelaksanaan task dependensi hingga beberapa kriteria telah terpenuhi, di mana, dalam hal ini mereka disebut sebagai **sensor**.

Setelah kita mendefinisikan DAG, membuat task, dan mendefinisikan dependensinya pada DAG , kita dapat mulai menjalankan task berdasarkan parameter DAG. Secara garis besar, konsep kunci dalam Airflow adalah `execution_time`. Ketika scheduler Airflow sedang berjalan, scheduler tersebut akan menentukan jadwal tanggal dengan interval yang teratur untuk menjalankan task terkait DAG. Waktu pelaksanaan dimulai pada `start_date` DAG dan berulang di setiap `schedule_interval`. Sebagai contoh, waktu eksekusi yang dijadwalkan adalah `(2017–01–01 00:00:00, 2017–01–02 00:00:00, …)` . Untuk setiap `execution_time`, DagRun dibuat dan beroperasi pada konteks waktu eksekusi itu. Dengan kata lain, **DagRun** hanyalah DAG dengan waktu eksekusi tertentu.

Semua tugas yang terkait dengan DagRun disebut sebagai **TaskInstances**. Singkat kata, TaskInstance adalah task yang telah dipakai dan memiliki konteks execution_date. DagRuns dan TaskInstances sendiri adalah konsep sentral dalam Airflow. Setiap DagRun dan TaskInstance dikaitkan dengan entri dalam database metadata Airflow yang mencatat status mereka, misal, “queued”, “running”, “failed”, “skipped”, atau “up for retry”. Membaca dan memperbaharui status ini adalah kunci untuk penjadwalan dan proses eksekusi pada Airflow.




### Referensi
https://airflow.apache.org/
https://medium.com/warung-pintar/airflow-fundamental-4097005a8498
https://yunusmuhammad007.medium.com/konsep-kerja-apache-airflow-db85e34b2fa4
https://imam.digmi.id/post/tutorial-airflow-part-1/
