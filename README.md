# Sistem Slip Gaji
Sistem Slip Gaji adalah aplikasi berbasis web yang dibangun untuk mengelola pembuatan slip gaji karyawan berdasarkan aturan absensi, lembur, dan reimbursement. Aplikasi ini dibuat untuk memenuhi case study seleksi Dealls, menggunakan FastAPI, PostgreSQL, dan SQLAlchemy, dengan fitur audit log dan pengujian otomatis.

## Fitur

Manajemen Pengguna: Mendukung 100 karyawan dan 1 admin dengan nama pengguna dan kata sandi.
Periode Absensi: Admin dapat membuat periode absensi dengan tanggal mulai dan selesai.
Absensi: Karyawan dapat mengirim absensi harian (tidak diperbolehkan di akhir pekan).
Lembur: Karyawan dapat mengajukan lembur hingga 3 jam per hari.
Reimbursement: Karyawan dapat mengajukan reimbursement dengan jumlah dan deskripsi.
Penggajian: Admin dapat menjalankan penggajian untuk periode tertentu (sekali saja per periode).
Slip Gaji: Karyawan dapat melihat slip gaji dengan rincian absensi, lembur, dan reimbursement.
Ringkasan Penggajian: Admin dapat melihat ringkasan gaji semua karyawan.
Audit Log: Setiap tindakan dicatat dengan request_id, user_id, dan alamat IP untuk pelacakan


## Instalasi

Kloning Repositori
git clone 
cd sistem-slip-gaji


Buat Virtual Environment
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate


Instal Dependensi
pip install -r requirements.txt


## Konfigurasi Database

Buat database PostgreSQL bernama slip_gaji_db.
Perbarui DATABASE_URL di sistem_slip_gaji.py dengan kredensial Anda:DATABASE_URL = "postgresql://<user>:<samuel123>@localhost:5432/slip_gaji_db"




## Jalankan Aplikasi
uvicorn sistem_slip_gaji:app --reload

Aplikasi akan berjalan di http://localhost:8000.


## Penggunaan API
API menggunakan format JSON. Berikut adalah endpoint utama:
1. Membuat Periode Absensi (Admin)

Endpoint: POST /periode-absensi
Body:{
  "tanggal_mulai": "2025-06-01",
  "tanggal_selesai": "2025-06-30"
}


Respons: Detail periode yang dibuat.

2. Mengirim Absensi (Karyawan)

Endpoint: POST /absensi
Body:{
  "tanggal": "2025-06-02",
  "id_periode": 1
}


Catatan: Tidak boleh di akhir pekan.

3. Mengirim Lembur (Karyawan)

Endpoint: POST /lembur
Body:{
  "tanggal": "2025-06-02",
  "jam": 2,
  "id_periode": 1
}


Catatan: Maksimal 3 jam per hari.

4. Mengirim Reimbursement (Karyawan)

Endpoint: POST /reimbursement
Body:{
  "jumlah": 100000,
  "deskripsi": "Perjalanan dinas",
  "id_periode": 1
}



5. Menjalankan Penggajian (Admin)

Endpoint: POST /jalankan-penggajian/{id_periode}
Respons: Konfirmasi penggajian selesai.

6. Melihat Slip Gaji (Karyawan)

Endpoint: GET /slip-gaji/{id_periode}
Respons:{
  "id_pengguna": 2,
  "id_periode": 1,
  "hari_absensi": 20,
  "jam_lembur": 5,
  "total_reimbursement": 100000,
  "gaji_pokok": 4000000,
  "bayaran_lembur": 250000,
  "total_gaji_bersih": 4250000
}



7. Melihat Ringkasan Penggajian (Admin)

Endpoint: GET /ringkasan-penggajian/{id_periode}
Respons: Daftar gaji bersih per karyawan dan total gaji.

Catatan: Autentikasi saat ini menggunakan token sederhana. Untuk produksi, gunakan JWT.
## Struktur Database

pengguna: Menyimpan data pengguna (admin/karyawan), nama pengguna, kata sandi, gaji.
periode_absensi: Menyimpan periode absensi (tanggal mulai, selesai, status penggajian).
absensi: Mencatat absensi harian karyawan.
lembur: Mencatat jam lembur karyawan.
reimbursement: Mencatat pengajuan reimbursement.
log_audit: Mencatat tindakan pengguna dengan request_id dan alamat IP.

Semua tabel memiliki kolom dibuat_pada, diperbarui_pada, dibuat_oleh, dan diperbarui_oleh untuk pelacakan.
## Pengujian
Jalankan tes otomatis dengan:
pytest

Contoh tes termasuk validasi pengiriman absensi di akhir pekan.
Arsitektur Perangkat Lunak

Backend: FastAPI untuk API HTTP dengan respons JSON.
Database: PostgreSQL dengan SQLAlchemy sebagai ORM.
Keamanan: Hashing kata sandi dengan bcrypt.
Audit: Log audit mencatat setiap tindakan dengan request_id dan alamat IP.
Skalabilitas: Indeks pada id_pengguna, id_periode, dan tanggal untuk kueri cepat.

Skalabilitas dan Performa

Indeks Database: Optimalkan kueri dengan indeks pada kolom yang sering digunakan.
Caching: Gunakan caching (misalnya, Redis) untuk ringkasan penggajian.
Asynchronous: Operasi database asinkronus untuk performa lebih baik.

Kontribusi

Fork repositori ini.
Buat cabang baru (git checkout -b fitur-baru).
Commit perubahan (git commit -m 'Menambahkan fitur X').
Push ke cabang (git push origin fitur-baru).
Buat Pull Request.
