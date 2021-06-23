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
Sebelum membahas lebih lanjut tentang konsep-konsep dasar pada Airflow maka berikut beberapa terminologi dasar yang perlu dipahami.
1. DAG (Directed Acyclyc Graph) adalah graf asiklik terarah yang digunakan untuk menggambarkan workflow.
2. Operator berfungsi untuk menjalankan suatu tugas di dalamnya. Operator juga menjelaskan tentang Tasks(tugas-tugas) di dalamnya.
3. Task adalah istilah untuk "tugas" yang kemudian dijalankan oleh operator. Task bisa berupa Python function atau eksternal yang bisa dipanggil. Tasks ini lebih baik bersifat idempotent. 
6. Task Instance adalah istilah untuk Worker adalah istilah untuk serangkaian tugas tertentu: DAG + TASK + POINT IN TIME
7. Workflow adalah kumpulan task yang memiliki dependensi terarah. Disebut juga sebagai DAGs yang merupakan kombinasi dari [1-6].


# Konsep Utama Airflow (Arsitektur & Cara Kerja)
## Arsitektur Airflow
Apache Airflow memiliki beberapa komponen diantaranya: Worker, Scheduler, Web UI (Dashboard), Web Server, Database, Executor, dan Worker. Berikut penjelasan singkat komponen utama pada Airflow:

![](https://imam.digmi.id/images/tutorial-airflow/part-1-2.png)

1. Task: Tasks adalah “aktivitas” yang kamu buat kemudian dijalankan oleh Operator. Task bisa berupa Python function atau eksternal yang bisa dipanggil. Tasks ini diharapkan bersifat idempotent 

Yang perlu diingat bahwa: Dalam membuat Tasks, terdapat task_id sama halnya dengan dag_id ini bersifat unik tidak boleh digunakan berulang kali dalam satu konteks DAG itu sendiri tapi task_id boleh sama dengan DAG lainnya, misal: dag_1 memiliki task_a dan task_b maka kita boleh menggunakan task_id yang sama pada dag_2.
2.  Webserver: Proses ini menjalankan aplikasi Flask sederhana dengan gunicorn yang membaca status semua task dari database metadata dan membuat status ini untuk UI Web.
3. Web UI: Komponen ini memungkinkan pengguna di sisi klien untuk melihat dan mengedit status task dalam database metadata. Karena terpisahnya komponen antara Scheduler dan database, UI Web memungkinkan pengguna untuk memanipulasi aktivitas Scheduler.
4. Scheduler: Scheduler, atau ‘Penjadwal’ adalah pemroses berupa daemon yang menggunakan definisi dari DAG. Bila dihubungkan dengan task dalam database metadata, scheduler berfungsi untuk menentukan task mana yang perlu dieksekusi lebih dulu serta prioritas pelaksanaannya. Pada umumnya, scheduler sendiri dijalankan sebagai sebuah service.
5. Database Metadata: Sekumpulan data yang menyimpan informasi mengenai status dari task. Update dari database sendiri dilakukan dengan menggunakan lapisan abstraksi yang diimplementasikan pada SQLAlchemy.
6. Executor: Executor adalah pemroses antrian pesan yang berhubungan dengan scheduler dalam menentukan proses worker agar benar-benar melaksanakan setiap task sesuai jadwal. Terdapat beberapa jenis executor, di mana masing-masing executor tersebut memiliki metode khusus untuk memfasilitasi worker bekerja, maupun dalam mengeksekusi task. Misal, LocalExecutor menjalankan tugas secara paralel pada mesin yang sama dengan proses scheduler. Ada pula executor lain seperti CeleryExecutor yang mengeksekusi task menggunakan proses yang ada pada sekelompok mesin worker yang terpisah.
7. Worker: Ini adalah pemroses yang benar-benar melaksanakan logika task dan ditentukan pada executor apa yang digunakan.

8. Scheduler adalah “petugas” yang bertanggung jawab dalam memantau semua DAG beserta Tasks yang ada.
9. Executor adalah  pemroses antrian pesan yang berhubungan dengan scheduler dalam menentukan proses worker agar benar-benar melaksanakan setiap task sesuai jadwal. 
10. Worker pemroses yang benar-benar melaksanakan logika task dan ditentukan pada executor apa yang digunakan.







### Referensi
https://airflow.apache.org/
https://medium.com/warung-pintar/airflow-fundamental-4097005a8498
https://yunusmuhammad007.medium.com/konsep-kerja-apache-airflow-db85e34b2fa4
https://imam.digmi.id/post/tutorial-airflow-part-1/
