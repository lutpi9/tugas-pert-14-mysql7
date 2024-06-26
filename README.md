# Tugas Praktikum { Pertemuan ke 15 } <img src=https://logos-download.com/wp-content/uploads/2016/05/MySQL_logo_logotype.png width="130px" >


|**Nama**|**NIM**|**Kelas**|**Matkul**|
|----|---|-----|------|
|Lutpiah Ainus Shiddik|312310474|TI.23.A5|Basis Data|

<img width="571" alt="image" src="https://github.com/lutpi9/tugas-pert-14-mysql7/assets/147919251/4054059d-84b4-4e6a-b2a8-f1d924333084">

***Query MySQL Pada Tabel Perusahaan***

```
CREATE TABLE Perusahaan(
id_p VARCHAR(10) PRIMARY KEY,
nama VARCHAR(45) NOT NULL,
alamat VARCHAR(45) DEFAULT NULL
);

INSERT INTO Perusahaan VALUES
('P01', 'Kantor Pusat', NULL),
('P02', 'Cabang Bekasi', NULL);
SELECT * FROM Perusahaan;
```

***Output :***

<img width="295" alt="image" src="https://github.com/lutpi9/tugas-pert-14-mysql7/assets/147919251/2de5db61-cf22-41ff-b8f6-241a70fb4bc6">


***Query MySQL Pada Tabel Departemen***

```
CREATE TABLE Departemen(
id_dept VARCHAR(10) PRIMARY KEY,
nama VARCHAR(45) NOT NULL,
id_p VARCHAR(10) NOT NULL,
manajer_nik VARCHAR(10) DEFAULT NULL
);

INSERT INTO Departemen VALUES
('D01', 'Produksi', 'P02', 'N01'),
('D02', 'Marketing', 'P01', 'N03'),
('D03', 'RnD', 'P02', NULL),
('D04', 'Logistik', 'P02', NULL);
SELECT * FROM Departemen;
```

***Output :***

<img width="294" alt="image" src="https://github.com/lutpi9/tugas-pert-14-mysql7/assets/147919251/20a270ef-b09e-48c0-b2cb-c6ef5c79df2e">


***Query MySQL Pada Tabel Karyawan***

```
CREATE TABLE Karyawan(
nik VARCHAR(10) PRIMARY KEY,
nama VARCHAR(45) NOT NULL,
id_dept VARCHAR(10) NOT NULL,
sup_nik VARCHAR(10) DEFAULT NULL
);

INSERT INTO Karyawan VALUES
('N01', 'Ari', 'D01', NULL),
('N02', 'Dina', 'D01', NULL),
('N03', 'Rika', 'D03', NULL),
('N04', 'Ratih', 'D01', 'N01'),
('N05', 'Riko', 'D01', 'N01'),
('N06', 'Dani', 'D02', NULL),
('N07', 'Anis', 'D02', 'N06'),
('N08', 'Dika', 'D02', 'N06');
SELECT * FROM Karyawan;
```

***Output :***

<img width="277" alt="image" src="https://github.com/lutpi9/tugas-pert-14-mysql7/assets/147919251/3083cf7a-3a34-47a1-b228-5eed7c54c316">


***Query MySQL Pada Tabel Project***

```
CREATE TABLE Project(
id_proj VARCHAR(10) PRIMARY KEY,
nama VARCHAR(45) NOT NULL,
tgl_mulai DATETIME,
tgl_selesai DATETIME,
status TINYINT(1)
);

INSERT INTO Project VALUES
('PJ01', 'A', '2019-01-10', '2019-03-10', '1'),
('PJ02', 'B', '2019-02-15', '2019-04-10', '1'),
('PJ03', 'C', '2019-03-21', '2019-05-10', '1');
SELECT * FROM Project;
```

***Output :***

<img width="431" alt="image" src="https://github.com/lutpi9/tugas-pert-14-mysql7/assets/147919251/f78fcb02-30c4-4bed-83c7-05bb656c7d84">


***Query MySQL Pada Tabel Project Deatil***

```
CREATE TABLE Project_detail(
id_proj VARCHAR(10) NOT NULL,
nik VARCHAR(10) NOT NULL
);

INSERT INTO Project_detail VALUES
('PJ01', 'N01'),
('PJ01', 'N02'),
('PJ01', 'N03'),
('PJ01', 'N04'),
('PJ01', 'N05'),
('PJ01', 'N07'),
('PJ01', 'N08'),
('PJ02', 'N01'),
('PJ02', 'N03'),
('PJ02', 'N05'),
('PJ03', 'N03'),
('PJ03', 'N07'),
('PJ03', 'N08');
SELECT * FROM Project_detail;
```

***Output :***

<img width="369" alt="image" src="https://github.com/lutpi9/tugas-pert-14-mysql7/assets/147919251/d177b370-36ca-47ab-836b-7401b11f2442">


## Menampilkan Nama Manajer Tiap Departemen

```
Select Departemen.nama AS Departemen, Karyawan.nama AS Manajer
FROM Departemen
LEFT JOIN Karyawan ON Karyawan.nik = Departemen.manajer_nik;
```

***Output :***

<img width="467" alt="image" src="https://github.com/lutpi9/tugas-pert-14-mysql7/assets/147919251/d2c15b3f-6f7f-4bdf-91d6-fe2934a0db10">


## Menampilkan Nama Supervisor Tiap Karyawan

```
SELECT Karyawan.nik, Karyawan.nama, Departemen.nama AS Departemen, Supervisor.nama AS Supervisor
FROM Karyawan
LEFT JOIN Karyawan AS Supervisor ON Supervisor.nik = Karyawan.sup_nik
LEFT JOIN Departemen ON Departemen.id_dept = Karyawan.id_dept;
```
***Output :***

<img width="669" alt="image" src="https://github.com/lutpi9/tugas-pert-14-mysql7/assets/147919251/08bd0bcf-6617-41eb-8c43-b350ebc8afae">


## Menampilkan Daftar Karyawan Yang Bekerja Pada Project A
```
SELECT Karyawan.nik, Karyawan.nama
FROM Karyawan
JOIN Project_detail ON Project_detail.nik = Karyawan.nik
JOIN Project ON Project.id_proj = Project_detail.id_proj
WHERE Project.nama = 'A';
```
***Output :***

<img width="377" alt="image" src="https://github.com/lutpi9/tugas-pert-14-mysql7/assets/147919251/8a3e0fdd-a48c-4459-8daf-b58e1e666295">


# Soal Latihan Praktikum

## 1. Departemen Apa Saja Yang Terlibat Dalam Tiap-tiap Project.

```
SELECT Project.nama AS Project, GROUP_CONCAT(Departemen.nama) AS Departemen
FROM Project
INNER JOIN Project_detail ON Project.id_proj = Project_detail.id_proj
INNER JOIN Karyawan ON Project_detail.nik = Karyawan.nik
INNER JOIN Departemen ON Karyawan.id_dept = Departemen.id_dept
GROUP BY Project.id_proj;
```
***Output :***

<img width="540" alt="image" src="https://github.com/lutpi9/tugas-pert-14-mysql7/assets/147919251/d0eecec6-0cb5-43d2-b49a-3191cf429fe9">


## 2. Jumlah Karyawan Tiap Departemen Yang Bekerja Pada Tiap-tiap Project.

```
SELECT Project.nama AS Project, Departemen.nama AS Departemen, COUNT(*) AS 'Jumlah Karyawan'
FROM Project
INNER JOIN Project_detail ON Project.id_proj = Project_detail.id_proj
INNER JOIN Karyawan ON Project_detail.nik = Karyawan.nik
INNER JOIN Departemen ON Karyawan.id_dept = Departemen.id_dept
GROUP BY Project.id_proj, Departemen.id_dept;
```
***Output :***

<img width="626" alt="image" src="https://github.com/lutpi9/tugas-pert-14-mysql7/assets/147919251/b99f0395-9d40-421a-9649-a171e3dde213">


## 3. Ada Berapa Project Yang Sedang Dikerjakan Oleh Departemen ***RnD***? (ket: project berjalan adalah yang statusnya 1).

```
SELECT COUNT(*) AS 'Jumlah Project'
FROM Project
INNER JOIN Project_detail ON Project.id_proj = Project_detail.id_proj
INNER JOIN Karyawan ON Project_detail.nik = Karyawan.nik
INNER JOIN Departemen ON Karyawan.id_dept = Departemen.id_dept
WHERE Departemen.nama = 'RnD' AND Project.status = 1;
```
***Output :***

<img width="439" alt="image" src="https://github.com/lutpi9/tugas-pert-14-mysql7/assets/147919251/85ca3518-a142-4b6b-99dd-1bf8668f1170">


## 4. Berapa banyak Project yang sedang dikerjakan oleh Ari ?

```
SELECT COUNT(*) AS 'Jumlah Project'
FROM Project_detail
INNER JOIN Karyawan ON Project_detail.nik = Karyawan.nik
WHERE Karyawan.nama = 'Ari' AND Project_detail.id_proj IN (SELECT id_proj FROM Project WHERE status = 1);
```
***Output :***

<img width="641" alt="image" src="https://github.com/lutpi9/tugas-pert-14-mysql7/assets/147919251/06eeb283-bd1a-4782-97b2-9e5a912c6f35">


## 5. Siapa Saja Yang Mengerjakan Project B ?

```
SELECT Karyawan.nama
FROM Project_detail
INNER JOIN Karyawan ON Project_detail.nik = Karyawan.nik
WHERE Project_detail.id_proj IN (SELECT id_proj FROM Project WHERE nama = 'B');
```
***Output :***

<img width="503" alt="image" src="https://github.com/lutpi9/tugas-pert-14-mysql7/assets/147919251/2b1b88d6-685e-4c85-81dc-63b05cfab83a">
