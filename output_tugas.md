#UniqueBrain - Format Input/Output Tugas

### Input Pemeriksaan

* Metode `1` adalah pemeriksaan dalam folder host, dimana **UniqueBrain** server akan mendownload semua file berformat **pdf** dari sebuah folder yang dipublish via **FTP**, berikut adalah input parameter yang **valid**:

```json
{
   "id": "1234567",
   "dosen": "Rizky",
   "email": "email@dosen.com",
   "judul": "Bunga Matahari",
   "threshold": 0.75,
   "min_match": 2,
   "folder" : "/unjani/.../folder/periksa",
   "report":
   {
      "logo_kiri": "PNG|JPG|URL|NULL",
      "logo_kanan": "PNG|JPG|URL|NULL",
      "upload_html": "/unjani/../folder/hasil/index.html",
      "upload_pdf": "/unjani/../folder/hasil/hasil.pdf"
   }
}
```
Berikut adalah penjelasan dari parameter diatas:

| Parameter | Default | Type | Deskripsi |
| --------- | --------| ---- | --------- |
| `id` | `NOT NULL` | **STRING** | Identitas (ID) tugas yang akan diperiksa, harus unik |
| `dosen` | `NOT NULL` | **STRING** | Nama dosen yang akan diperiksa |
| `email` | `NULL` | **STRING** | Alamat email dosen dimana laporan pemeriksaan akan dikirim, jika `NULL` maka tidak akan dikirim |
| `judul` | `NOT NULL` | **STRING** | Nama judul tugas yang dibuat oleh dosen |
| `threshold` | **0.75** | **DOUBLE** | Threshold kesamaan kalimat hingga diduga sebagai plagiarisme, angka dari **0.01** hingga **1.0** |
| `min_match` | **2** | **DOUBLE** | Persentase (%) minimal kesamaan antara dokumen tugas hingga dokumen tersebut diduga plagiat, angka dari **0.1** hingga **100** |
| `folder` | `NOT NULL` | **STRING** | Lokasi relative folder di dalam hosting, dimana semua file berformat **PDF** akan diperiksa. **PENTING**: 
1. Nama folder tidak boleh diawali dengan **public_html** karena **UniqueBrain** akan secara default menggunakan folder **public_html** sebagai **root** dari semua folder
2. Nama folder harus diawali dengan tanda backslash `/`
Misalkan nama relative folder adalah `/unjani/file_gue` maka **UniqueBrain** akan membaca sebagai `/home/{USER}/public_html/unjani/file_gue`|
