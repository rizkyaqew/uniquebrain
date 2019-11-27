#UniqueBrain - Format Input/Output Tugas

### Input Pemeriksaan

* Metode `1` adalah pemeriksaan dalam folder host, dimana **UniqueBrain** server akan mendownload semua file berformat **pdf** dari sebuah folder yang dipublish via **FTP**, berikut adalah input parameter yang **valid**:

```json
{
   "id": "1234567",
   "dosen": "Rizky",
   "kelas": "Bunga Matahari",
   "path": 
   [
      "public_html/unjani/.../periksa_folder_ini",
      "public_html/unjani/.../periksa_folder_ini_juga",
      "..."
   ],
   "threshold": 0.75,
   "min_match": 2
}
```
