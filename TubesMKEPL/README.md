# Student Registration System — Project Praktik CI/CD (Week 10–13)

Project Java konsol (aplikasi pendaftaran mahasiswa) yang sudah disiapkan untuk
praktik CI/CD: Build (W10) → Test (W11) → Inspection/SonarCloud (W12) → Deploy (W13).

## Yang sudah ditambahkan ke project ini
- `pom.xml` — konfigurasi Maven + JUnit 4 + JaCoCo (coverage untuk Sonar).
- `src/test/java/...` — unit test JUnit (PengelolaArrayMahasiswa & StatistikMahasiswa).
- `.github/workflows/maven.yml` — workflow build & test otomatis (W10 & W11).
- `.github/workflows/sonarcloud.yml` — workflow analisis SonarCloud (W12).
- `.gitignore` — supaya file IDE & target/ tidak ikut ter-commit.

> CATATAN JUJUR: nilai ekspektasi di unit test disusun dari membaca logika kode,
> BUKAN dari hasil eksekusi (environment penyiapan tidak punya Maven/JDK lengkap).
> Jalankan `mvn test` sekali untuk memastikan semua hijau sebelum tes/rekaman.

---

## Cara pakai (alur lengkap, cocok untuk latihan tes 11 Juni)

### Persiapan lokal
1. Pastikan Java JDK (17+) dan Maven sudah terpasang: `java -version` dan `mvn -v`.
2. Dari folder `pendaftaranmhsapp/`, jalankan:
   - `mvn compile`  → memastikan kode meng-compile (inti Week 10)
   - `mvn test`     → menjalankan unit test (inti Week 11)
   - `mvn verify`   → test + laporan coverage JaCoCo (dipakai Week 12)

### Week 10 — Continuous Integration (build otomatis)
1. Buat repo kosong di GitHub, push project ini ke branch `main`.
2. Buka tab **Actions** → workflow "Build Project" jalan otomatis.
3. Demonstrasi cegah masalah: ubah kode jadi error sintaks (mis. hapus titik koma),
   push → Actions jadi MERAH. Perbaiki → push → HIJAU lagi.

### Week 11 — Continuous Testing
1. Workflow yang sama (`mvn verify`) sudah menjalankan unit test.
2. Simulasi SUKSES: kode benar → semua test pass → build hijau.
3. Simulasi GAGAL: ubah satu nilai assert (mis. di StatistikMahasiswaTest
   ubah `assertEquals(2021, ...)` jadi `2099`) ATAU rusak logika di Counter/
   StatistikMahasiswa → push → test gagal → build merah. Tunjukkan log error.

### Week 12 — Continuous Inspection (SonarCloud)
Ikuti komentar di `.github/workflows/sonarcloud.yml`:
1. Daftar SonarCloud (login GitHub), buat Organization, import repo.
2. Buat `SONAR_TOKEN`, simpan sebagai GitHub Actions secret.
3. Ganti `sonar.organization` dan `sonar.projectKey` di workflow & pom.
4. Push → lihat dashboard SonarCloud (bugs, code smells, coverage, duplikasi).
5. Simulasi: tambahkan code smell (mis. variabel tak terpakai), push,
   lihat issue baru muncul di analisis perubahan.

### Week 13 — Continuous Deployment / Delivery
Catatan: project bawaan materi (Basic-Java-Maven) memakai GitHub Packages.
Untuk project ini kamu bisa meniru pola yang sama bila diperlukan:
tambahkan `<distributionManagement>` ke GitHub Packages + workflow `mvn deploy`
yang ter-trigger saat membuat Release. (Belum saya tambahkan di sini karena
detail target deploy sebaiknya disamakan dengan yang dicontohkan di Video W13.)
