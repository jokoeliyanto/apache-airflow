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

# Arsitektur Airflow
Airflow adalah platform untuk membangun dan menjalankan sebuah work flow. Work Flow direpresentasikan sebagai DAG (a Directed Acyclic Graph), dan berisi bagian-bagian individual dari pekerjaan yang disebut Task, disusun dengan dependensi dan aliran data yang diperhitungkan.

!["Contoh DAG"](https://github.com/jokoeliyanto/apache-airflow/blob/main/image/DAG%20Example2.png)

DAG menentukan dependensi antara task, dan urutan untuk menjalankannya dan menjalankan percobaan ulang. Task sendiri menjelaskan apa yang harus dilakukan, baik itu mengambil data, menjalankan analisis, memicu sistem lain, atau lebih.

Airflow secara umum terdiri dari komponen-komponen berikut:

* **Scheduler**, yang menangani pemicu(triger) alur kerja terjadwal, dan mengirimkan task ke eksekutor untuk dijalankan.

* Sebuah **eksekutor**, yang menangani Task yang sedang berjalan. Dalam instalasi Airflow default, eksekutor menjalankan semua yang ada di dalam schedulerl, tetapi sebagian besar eksekutor yang sesuai dengan produksi sebenarnya mendorong eksekusi task ke worker.

* **Server web**, yang menyajikan antarmuka pengguna yang berguna untuk memeriksa, memicu(triger), dan men-debug perilaku DAG dan task.

* **Folder file DAG**, dibaca oleh scheduler dan eksekutor (dan semua worker yang dimiliki eksekutor)

* **Metadata Database**, digunakan oleh scheduler, eksekutor, dan web server untuk menyimpan status.

!["Arsitektur Apache Airflow"](https://github.com/jokoeliyanto/apache-airflow/blob/main/image/arsitektur%20airflow2.png)

Sebagian besar eksekutor umumnya juga mengenal komponen lainnya dan memungkinkan mereke berkomunikasi dengan worker eksekutor tersebut - seperti antrian task - namun kita masih bisa menganggap bahwa eksekutor dan worker di dalamnya merupakan sebuah komponen logis tunggal di Airflow secara keseluruhan yang menangani task yag sesungguhnya.

# Pengenalan DAG

DAG adalah kepanjangan dari Directed Acyclic Graphs yang kita gunakan untuk membuat suatu workflow atau kita juga dapat memahami DAG sebagai sekumpulan dari Tasks. DAG inilah yang mencerminkan tentang alur dari workflow beserta relasi antar proses dan ketergantungan antar prosesnya.

Beberapa properties DAG yang paling utama adalah sebagai berikut,
1. `dag_id`: Pengidentifikasi unik di antara semua DAG,
2. `start_date`: Titik waktu awal di mana task pada DAG akan dimulai,
3. `schedule_interval`: Interval waktu sebuah DAG dieksekusi.

Meskipun DAG digunakan untuk mengatur task dan mengatur konteks eksekutorya, DAGs tidak melakukan perhitungan yang sebenarnya. Sebagai gantinya, task adalah elemen dari Airflow yang berfungsi untuk melakukan pekerjaan yang ingin dilakukan. Terdapat dua tipe task:
1. Task dapat melakukan beberapa operasi eksplisit, di mana, dalam hal ini, mereka disebut sebagai **operator**, atau,
2. Task dapat menghentikan sementara pelaksanaan task dependensi hingga beberapa kriteria telah terpenuhi, di mana, dalam hal ini mereka disebut sebagai **sensor**.

Setelah kita mendefinisikan DAG, membuat task, dan mendefinisikan dependensinya pada DAG , kita dapat mulai menjalankan task berdasarkan parameter DAG. Secara garis besar, konsep kunci dalam Airflow adalah `execution_time`. Ketika scheduler Airflow sedang berjalan, scheduler tersebut akan menentukan jadwal tanggal dengan interval yang teratur untuk menjalankan task terkait DAG. Waktu pelaksanaan dimulai pada `start_date` DAG dan berulang di setiap `schedule_interval`. Sebagai contoh, waktu eksekusi yang dijadwalkan adalah `(2017–01–01 00:00:00, 2017–01–02 00:00:00, …)` . Untuk setiap `execution_time`, DagRun dibuat dan beroperasi pada konteks waktu eksekusi itu. Dengan kata lain, **DagRun** hanyalah DAG dengan waktu eksekusi tertentu.

Semua tugas yang terkait dengan DagRun disebut sebagai **TaskInstances**. Singkat kata, TaskInstance adalah task yang telah dipakai dan memiliki konteks execution_date. DagRuns dan TaskInstances sendiri adalah konsep sentral dalam Airflow. Setiap DagRun dan TaskInstance dikaitkan dengan entri dalam database metadata Airflow yang mencatat status mereka, misal, `queued`, `running`, `failed`, `skipped`, atau `up for retry`. Membaca dan memperbaharui status ini adalah kunci untuk penjadwalan dan proses eksekusi pada Airflow.


# Control Flow

DAG dirancang untuk dijalankan berkali-kali, dan beberapa proses dapat terjadi secara paralel. DAG diparameterisasi, selalu menyertakan tanggal mereka `running for` (`execution_date`), tetapi juga dengan parameter opsional lainnya.

Tugas memiliki dependensi yang dideklarasikan satu sama lain. Anda akan melihat ini di DAG menggunakan operator `>>` dan `<<`:
```
first_task >> [second_task, third_task]
third_task << fourth_task
```
Atau, dengan metode `set_upstream` dan `set_downstream`:

```
first_task.set_downstream([second_task, third_task])
third_task.set_upstream(fourth_task)
```

Ketergantungan ini adalah apa yang membentuk `edges` grafik, dan bagaimana Airflow bekerja di urutan mana untuk menjalankan tugas Anda. Secara default, tugas akan menunggu semua tugas hulunya berhasil sebelum dijalankan, tetapi ini bisa saja dikustomisasi menggunakan fitur seperti Branching, LatestOnly, dan Trigger Rules.

Untuk meneruskan data antar tugas, Anda memiliki dua opsi:

1. `XComs` ("Cross-communications"), sebuah sistem di mana Anda dapat memiliki tugas mendorong dan menarik sedikit metadata.

2. Mengunggah dan mengunduh file besar dari layanan penyimpanan (baik yang Anda jalankan, atau bagian dari cloud publik)

Airflow mengirimkan Tugas untuk dijalankan di Pekerja saat ruang tersedia, jadi tidak ada jaminan semua tugas di DAG Anda akan berjalan di pekerja yang sama atau mesin yang sama.

Saat Anda membangun DAG Anda, mereka cenderung menjadi sangat kompleks, sehingga Airflow menyediakan beberapa mekanisme untuk membuat ini lebih berkelanjutan - SubDAG memungkinkan Anda membuat DAG "dapat digunakan kembali" yang dapat Anda sematkan ke yang lain, dan TaskGroups memungkinkan Anda mengelompokkan tugas secara visual di UI.

Ada juga fitur untuk memungkinkan Anda dengan mudah melakukan pra-konfigurasi akses ke sumber daya pusat, seperti penyimpanan data, dalam bentuk Connections & Hooks, dan untuk membatasi konkurensi, melalui Pools.

# User Interface Airflow

Airflow hadir dengan antarmuka pengguna yang memungkinkan kita melihat apa yang dilakukan DAG dan tugasnya, memicu menjalankan DAG, melihat log, dan melakukan beberapa debug terbatas dan penyelesaian masalah dengan DAG kita.

!["User Interface Aairflow"](https://airflow.apache.org/docs/apache-airflow/stable/_images/dags.png)


# Alur Kerja Airflow

Pertama, scheduler membaca folder DAG mem-parsing dan melihat apakah script-nya sudah memenuhi kriteria.

![](https://github.com/jokoeliyanto/apache-airflow/blob/main/image/1.png)

Jika sudah memenuhi kriteria, scheduler akan membuat DagRun di DB dan me-register script-nya agar DagRun berstatus Running.

![](https://github.com/jokoeliyanto/apache-airflow/blob/main/image/2.png)

Selanjutnya, scheduler menjadwalkan TaskInstances untuk dijalankan hingga TaskInstance terjadwalkan “Scheduled”.

![](https://github.com/jokoeliyanto/apache-airflow/blob/main/image/3.png)

Lalu, scheduler mengirim TaskInstance ke Executor agar Executor mengirim TaskInstance ke sistem antrian agar terantrikan atau Queued.

![](https://github.com/jokoeliyanto/apache-airflow/blob/main/image/4.png)

Executor mengeluarkan TaskInstance, dilanjutkan dengan pembaharuan TaskInstance di MetaDB untuk dijalankan. Setelahnya, worker mengeksekusi TaskInstance.
![](https://github.com/jokoeliyanto/apache-airflow/blob/main/image/5.png)

Setelah task selesai, Executor akan memperbaharui TaskInstance ke Success. Walau demikian, DagRun masih berjalan ke task berikutnya dalam Dag tersebut.

![](https://github.com/jokoeliyanto/apache-airflow/blob/main/image/6.png)

Setelah semua task selesai dalam Dag, scheduler akan memperbaharui status menjadiSuccess pada MetaDB DagRun. Jika terdapat task yang gagal, DagRun akan memperbaharuinya ke Failed.

![](https://github.com/jokoeliyanto/apache-airflow/blob/main/image/7.png)

Akhirnya, Web Server membaca MetaDB ke Pembaharuan UI.

![](https://github.com/jokoeliyanto/apache-airflow/blob/main/image/8.png)

# Contoh Penerapan di StatrtUp Indonesia

Penggunaan Apache Airflow oleh Warung Pintar pada Google Cloud Composer

![](https://miro.medium.com/max/1452/1*ouLGL-9ZwLiG1PHHPct0ng.png)


### Referensi
* https://airflow.apache.org/
* https://medium.com/warung-pintar/airflow-fundamental-4097005a8498
* https://yunusmuhammad007.medium.com/konsep-kerja-apache-airflow-db85e34b2fa4
* https://imam.digmi.id/post/tutorial-airflow-part-1/
