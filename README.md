# Praktikum RTOS - SEMESTER 5 (3 Teknik Komputer A)

- **Diva Sakananda (3222600013)**
- **Billy Lukito Danuharja (3222600027)**


# Task 7 Exercise 8 - Demonstrate	how	to	eliminate	resource contention	in	a	selective	manner	-	using	a	semaphore to	protect	the	critical	code	section

# RTOS LED Control with Semaphore (CMSIS-RTOS)

Kode ini adalah implementasi RTOS (Real-Time Operating System) menggunakan CMSIS-RTOS untuk mengontrol beberapa LED yang dihubungkan dengan GPIO STM32. Program terdiri dari tiga task utama yang bertanggung jawab untuk mengontrol LED secara paralel, dengan sinkronisasi akses ke sumber daya bersama (shared resource) menggunakan semaphore.

## Fitur

- **Kontrol LED Paralel**: 
  - Green LED: Dikontrol dengan `GreenLEDTask`.
  - Red LED: Dikontrol dengan `RedLEDTask`.
  - Orange LED: Dikontrol dengan `OrangeLEDTask` (berkedip secara independen).
- **Sinkronisasi Semaphore**: Memastikan akses yang aman ke sumber daya bersama.
- **Integrasi Timer**: Timer2 mengukur durasi akses ke sumber daya bersama.

---

## Penjelasan Task

### 1. GreenLEDTask
- **Fungsionalitas**:
  - Menyalakan LED hijau.
  - Mengambil semaphore sebelum mengakses sumber daya bersama.
  - Mengakses sumber daya bersama menggunakan `AccessSharedData()`.
  - Menunggu selama 200 ms menggunakan `osDelay`.

### 2. RedLEDTask
- **Fungsionalitas**:
  - Menyalakan LED merah.
  - Mengambil semaphore sebelum mengakses sumber daya bersama.
  - Mengakses sumber daya bersama menggunakan `AccessSharedData()`.
  - Menunggu selama 550 ms menggunakan `osDelay`.

### 3. OrangeLEDTask
- **Fungsionalitas**:
  - Berkedip LED oranye dengan toggle setiap 50 ms.
  - Tidak mengakses sumber daya bersama.
  - Berjalan secara independen dengan prioritas lebih tinggi untuk memastikan kedipan terus berjalan.

---

## Sumber Daya Bersama dan Sinkronisasi

### **AccessSharedData()**
Mensimulasikan sebuah bagian kritis yang:
- Mengakses sumber daya bersama.
- Menggunakan Timer2 untuk menghitung durasi akses.
- Memerlukan semaphore (`CriticalResourceSemaphore`) untuk bisa mengaksesnya.

### **CriticalResourceSemaphore**
- Semaphore biner untuk mensinkronisasi akses ke `AccessSharedData()`.
- Memastikan hanya satu task yang dapat mengakses sumber daya bersama pada satu waktu.

---


## Dependencies

- **CMSIS-RTOS**: Untuk implementasi RTOS.
- **STM32 HAL**: Untuk konfigurasi GPIO dan Timer.

---

## Cara Kerja

1. Program menginisialisasi peripheral GPIO dan Timer.
2. Task RTOS dibuat:
   - `GreenLEDTask` dan `RedLEDTask` bersaing untuk mengakses sumber daya bersama menggunakan semaphore.
   - `OrangeLEDTask` berkedip secara independen.
3. Semaphore (`CriticalResourceSemaphore`) memastikan akses yang aman dan terkoordinasi ke `AccessSharedData()`.
4. Task berjalan secara paralel berdasarkan prioritas dan delay yang ditentukan.

## Hubungan Antar Task

- **GreenLEDTask** dan **RedLEDTask** bersaing untuk mengakses sumber daya bersama (`AccessSharedData`), tetapi **semaphore** (`CriticalResourceSemaphore`) mencegah konflik.
- **OrangeLEDTask** tidak mengakses sumber daya bersama dan berjalan secara independen dengan prioritas lebih tinggi untuk memastikan blinking terus berjalan.

## Foto Hardware
![50887ca8-34f7-493a-b62b-17cf0d5a1d6e](https://github.com/user-attachments/assets/4be5e987-f7cf-4150-80d5-1ed9a872ca01)

## Video Hardware

