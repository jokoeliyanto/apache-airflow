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

Apache Airflow memiliki banyak fitur, dan didukung dengan integrasi tool eksternal yang banyak seperti: Hive, Pig, Google BigQuery, Amazon Redshift, Amazon S3, dst. Selain itu, hal terpenting lainnya adalah bahwa Airflow memiliki keunggulan pada scaling yang tak terbatas. Maka menjadi wajar jika Airflow menjadi pilihan yang tepat untuk membangun data pipeline saat ini.









### Referensi
https://airflow.apache.org/
https://medium.com/warung-pintar/airflow-fundamental-4097005a8498
https://yunusmuhammad007.medium.com/konsep-kerja-apache-airflow-db85e34b2fa4
https://imam.digmi.id/post/tutorial-airflow-part-1/
