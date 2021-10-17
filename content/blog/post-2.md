---
title: "Membuat Simpel Face  Detection Dengan OpenCV dan Python"
description: "meta description"
image: "images/post/03.jpg"
date: 2021-01-25T11:33:57+06:00
author: "Arfan yoga"
tags: ["python", "pemrograman", ]
categories: ["LifeStyle"]
draft: false
---
<div style="text-align: justify">
Di tutorial kali ini kita akan membuat program face detection, terdengar sulit mungkin, tapi disini Saya akan mencoba menjelaskannya sesimpel mungkin. baik langsung saja kita persiapkan file file yang kita perlukan
</div>

### Perisapan
##### download library yang diperlukan
disini kita perlu mendownload library opencv di python
```
pip install opencv-contrib-python
```
##### download dataset
<div style="text-align: justify">
lalu kita perlu mendownload dataset yang akan kita gunakan untuk mendeteksi wajah, kenapa tidak kita buat sendiri ? ya karna lama hehe, mungkin dilain kesempatan saya akan buatkan tutorialnya
</div>

download dulu dataset nya di [sini](https://github.com/opencv/opencv/blob/master/data/haarcascades/haarcascade_frontalface_default.xml)

### Coding
##### Tahap 1 : Mengakses webcam
<div style="text-align: justify">
sebelum mendeteksi wajah kita perlu mengakses webcam terlebih dahulu untuk mengaksesnya kita dapat menggunakan fungsi bawaan dari open cv, baik langsung saja kita buka code editor kita lalu buat file python.

lalu untuk mengakses webcam salin code dibawah ini
</div>

```
import cv2 as cv

video = cv.VideoCapture(0)
while True:
    isTrue,frame = video.read()
    cv.imshow('image',frame);
    if cv.waitKey(20) & 0xFF==ord('d'):
        break
```


##### Tahap 2 : Mengubah video menjadi grayscale

<div style="text-align: justify">
kenapa tuh harus diubah menjadi grayscale ?? karena kita akan menggunakan algoritma yang mencocokan tingkat gelap dan terang untuk mendeteksi objek, jadi harus kita convert ke grayscale dulu, nah gimana tuh cara convert nya ? untuk cara convertnya kita bisa menggunakan sebuah fungsi yang ada di dalam library openCV yaitu fungsi cvtColor() dengan parameter pertama yaitu gambar atau frame video dan parameter kedua adalah tipe filter yang akan kita aplikasikan, untuk membuat video kita menjadi grayscale lakukan seperti yang ada dibawah ini
</div>

```
frame_gs = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
```
code diatas berfungsi untuk membuat tiap tiap frame yang kita miliki menjadi grayscale atau hitam putih
taruh kode diatas seperti contoh dibawah
```
import cv2 as cv

video = cv.VideoCapture(0)
while True:
    isTrue,frame = video.read()
    frame_gs = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
    cv.imshow('image',frame);
    if cv.waitKey(20) & 0xFF==ord('d'):
        break
```
##### Tahap 3 : Mendeteksi wajah

<div style="text-align: justify">
disini kita akan menggunakan algoritma haar cascade classifier, algoritma ini akan mencocokan gelap terang yang kita dapat setelah mengkonfersikan apa yang ditangkap oleh webcam menjadi grayscale dan mendeteksi apakah objek tersebut merupakan wajah.

untuk melakukanya kita terlebih dahulu harus mengintansiasikan objek dari dataset yang sudah kita download tadi
</div>

```
trained_data = cv.CascadeClassifier("haarcascade_frontalface_default.xml")
```
<div style="text-align: justify">
lalu kita siap mendeteksi wajah, untuk melakukanya kita dapat menggunakan fungsi detectMultiScale(), fungsi ini akan mendeteksi objek yang sesuai dengan dataset dengan menghiraukan ukuran objek baik besar maupun kecil. fungsi tersebut akan mengembalikan titik paling pojok kiri atas dari sebuah objek dan juga panjang dan lebar dari objek tersebut
</div>

```
import cv2 as cv

trained_data = cv.CascadeClassifier("haarcascade_frontalface_default.xml")
video = cv.VideoCapture(0)
while True:
    isTrue,frame = video.read()
    frame_gs = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
    coordinate = trained_data.detectMultiScale(frame_gs)
    cv.imshow('image',frame);
    if cv.waitKey(20) & 0xFF==ord('d'):
        break
```

##### Tahap 4 : Gambar Persegi untuk menandai objek yang dideteksi
<div style="text-align: justify">
untuk menggambar persegi kita dapat memanfaatkan fungsi rectangle yang ada di opencv
fungsi ini memerlukan 5 parameter yaitu frame atau gambar yang akan kita beri ractangle, koordinat sudut paling kiri atas, lalu panjang dan tinggi dari ractangle, warna, dan tebal garis.
nah kita tadi sudah berhasil mendapatkan titik koordinat dari objek yang kita deteksi dan kita beri simpan dalam variable coordinate, selanjutnya kita akan menggambar rectangle untuk masing masing objek yang terdeteksi sebagai wajah
</div>

```
for (x,y,w,h) in coordinate:    
        cv.rectangle(frame,(x,y),(x+w, y+h),(0,255,0),2)        
        cv.imshow('image',frame);
```

##### Sehingga keseluruhan codenya adalah sebagai berikut
```
import cv2 as cv
trained_data = cv.CascadeClassifier("haarcascade_frontalface_default.xml")

video = cv.VideoCapture(0)
while True:
    isTrue,frame = video.read()
    frame_gs = cv.cvtColor(frame, cv.COLOR_BGR2GRAY)
    coordinate = trained_data.detectMultiScale(frame_gs)
    for (x,y,w,h) in coordinate:    
        cv.rectangle(frame,(x,y),(x+w, y+h),(0,255,0),2)        
        cv.imshow('image',frame);
    if cv.waitKey(20) & 0xFF==ord('d'):
        break
```
jalankan code nya dan hasilnya adalah sebagai berikut
![Example image](/images/post/ocv.png)
yang nulis artikel belum mandi hehe :))