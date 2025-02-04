| Nama | Rifa andiani meilawati |
| :-- | :-- |
|NIM|312210377| 
|Kelas|TI22B2|
|Matakuliah|pemograman web 2|
|dosen|Sufajar Butsianto, S.Kom.,M.Kom.|




# PHP DATABASE
PHP adalah bahasa pemrograman skrip yang sering digunakan untuk pengembangan web. Dalam konteks pengembangan web, PHP sering kali digunakan untuk berinteraksi dengan database untuk menyimpan, mengambil, memperbarui, dan menghapus data.

## 1. Dukungan PHP
Memiliki banyak library yang memungkinkan untuk akses database.
Kecepatan akses dengan menggunakan engine/driver yang khusus untuk setiap database.
Independent terhadap database yang digunakan
PHP mendukung ODBC
## 2. Database
Kumpulan data, yang terorganisir secara logika, dikelola menggunakan metode tertentu yang menjamin konsistensi data.

## 3. DBMS (Database Management System)
Aplikasi yang dirancang untuk menyimpan dan mengelola satu atau lebih database. Fungsi:

Buat database, tabel, dan struktur pendukung
Manipulasi data
Menjaga struktur database
Backup dan Recovery
# PRAKTIKUM - 3
## 1. Mysql Server
Untuk menjalankan MySQL Server dari menu XAMPP Contol
http://localhost/phpmyadmin/
![gambar 1](https://github.com/RIFAANDIANI/Lab8Web/assets/115616294/c421e068-28f1-42ab-bba6-f9e51aa1d36c)
## 2. Mengakses MySQL Client menggunakan PHP MyAdmin
Pastikan webserver Apache dan MySQL server sudah dijalankan. Kemudian buka melalui browser: http://localhost/phpmyadmin/
![image](https://github.com/RIFAANDIANI/Lab8Web/assets/115616294/f22338b0-7588-4a2c-a9a4-8489969089ea)
## 3. Membuat Database: Studi Kasus Data Barang
CREATE DATABASE latihan-1;

## 4. Membuat Tabel

```mysql
CREATE TABLE data_barang (
id_barang int(10) auto_increment Primary Key,
kategori varchar(30),
nama varchar(30),
gambar varchar(100),
harga_beli decimal(10,0),
harga_jual decimal(10,0),
stok int(4)
);
```
## 5. Menambahkan Data
```php
INSERT INTO data_barang (kategori, nama, gambar, harga_beli, harga_jual, stok)
VALUES ('Elektronik', 'HP Samsung Android', 'hp_samsung.jpg', 2000000, 2400000, 5),
('Elektronik', 'HP Xiaomi Android', 'hp_xiaomi.jpg', 1000000, 1400000, 5),
('Elektronik', 'HP OPPO Android', 'hp_oppo.jpg', 1800000, 2300000, 5);
```
## output
![gambar 3](https://github.com/RIFAANDIANI/Lab8Web/assets/115616294/a8942cf3-a2f7-414b-a1b9-e9ab1b80d321)

## 6. Membuat Program CRUD
Buat folder lab8_php_database pada root directory web server, Kemudian untuk mengakses direktory tersebut pada web server dengan mengakses URL: http://localhost/lab8_php_database/

Output
![gambar 5](https://github.com/RIFAANDIANI/Lab8Web/assets/115616294/85c17eba-87e3-4629-a3fb-ec2c4202c407)

## 7. Membuat file koneksi database
Buat file baru dengan nama koneksi.php
```php
<?php
$host = "localhost";
$user = "root";
$pass = "";
$db = "latihan-1";
$conn = mysqli_connect($host, $user, $pass, $db);
if ($conn == false)
{
    echo "Koneksi ke server gagal.";
    die();
} else echo "Koneksi berhasil";
?>
```
Output
![gambar 4](https://github.com/RIFAANDIANI/Lab8Web/assets/115616294/53202c0d-accb-42fb-8638-42381abbf301)

## 8. Membuat file index untuk menampilkan data (Read)
Buat file baru dengan nama index.php
```php
<?php
include("koneksi.php");

// query untuk menampilkan data
$sql = 'SELECT * FROM data_barang';
$result = mysqli_query($conn, $sql);

?>
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Data Barang</title>
    <link href="style.css" rel="stylesheet" type="text/css" />

</head>

<body>
    <div class="container">
        <h1>Data Barang</h1>
        <div class="main">
            <a href="tambah.php">Tambah Barang</a>
            <table>
                <tr>
                    <th>Gambar</th>
                    <th>Nama Barang</th>
                    <th>Kategori</th>
                    <th>Harga Jual</th>
                    <th>Harga Beli</th>
                    <th>Stok</th>
                    <th>Aksi</th>
                </tr>
                <?php if ($result) : ?>
                    <?php while ($row = mysqli_fetch_array($result)) : ?>
                        <tr>
                            <td><img src="gambar/<?= $row['gambar']; ?>" alt="<?=$row['nama']; ?>"></td>
                            <td><?= $row['nama']; ?></td>
                            <td><?= $row['kategori']; ?></td>
                            <td><?= $row['harga_beli']; ?></td>
                            <td><?= $row['harga_jual']; ?></td>
                            <td><?= $row['stok']; ?></td>
                            <td>
                                <a href="ubah.php?id=<?= $row['id_barang']; ?>">Ubah</a>
                                <a href="hapus.php?id=<?= $row['id_barang']; ?>">Hapus</a>
                            </td>
                        </tr>
                    <?php endwhile; else : ?>
                    <tr>
                        <td colspan="7">Belum ada data</td>
                    </tr>
                <?php endif; ?>
            </table>
        </div>
    </div>
</body>
</html>
```
Output
![image](https://github.com/RIFAANDIANI/Lab8Web/assets/115616294/3b841055-7d23-44b7-b1a9-8b6f29e110d0)

## 9. Menambah Data (Create)
Buat file baru dengan nama tambah.php
```php
<?php
error_reporting(E_ALL);
include_once 'koneksi.php';

if (isset($_POST['submit']))
{
    $nama = $_POST['nama'];
    $kategori = $_POST['kategori'];
    $harga_jual = $_POST['harga_jual'];
    $harga_beli = $_POST['harga_beli'];
    $stok = $_POST['stok'];
    $file_gambar = $_FILES['file_gambar'];
    $gambar = null;
    if ($file_gambar['error'] == 0)
    {
        $filename = str_replace(' ', '_',$file_gambar['name']);
        $destination = dirname(__FILE__) .'/gambar/' . $filename;
        if(move_uploaded_file($file_gambar['tmp_name'], $destination))
        {
            $gambar = 'gambar/' . $filename;;
        }
    }
    $sql = 'INSERT INTO data_barang (nama, kategori,harga_jual, harga_beli, stok, gambar) ';
    $sql .= "VALUE ('{$nama}', '{$kategori}','{$harga_jual}','{$harga_beli}', '{$stok}', '{$gambar}')";
    $result = mysqli_query($conn, $sql);
    header('location: index.php'); 
}
?>
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tambah Barang</title>
    <link href="style-tambah.css" rel="stylesheet" type="text/css" />
</head>

<body>
    <div class="container">
        <h1>Tambah Barang</h1>
        <div class="main">
            <form method="post" action="tambah.php" enctype="multipart/form-data">
                <div class="input">
                    <label> Nama Barang</label>
                    <input type="text" name="nama"/>
                </div>
                <div class="input">
                    <label>Kategori</label>
                    <select name="kategori">
                        <option value="Komputer">Komputer</option>
                        <option value="Elektronik">Elektronik</option>
                        <option value="HandPhone">HandPhone</option>
                    </select>
                </div>
                <div class="input">
                    <label>Harga Jual</label>
                    <input type="text" name="harga_jual" />
                </div>
                <div class="input">
                    <label>Harga Beli</label>
                    <input type="text" name="harga_beli" />
                </div>
                <div class="input">
                    <label>Stok</label>
                    <input type="text" name="stok" />
                </div>
                <div class="input">
                    <label>File Gambar</label>
                    <input type="file" name="file_gambar" />
                </div>
                <div class="submit">
                    <input type="submit" name="submit" value="Simpan" />
                </div>
            </form>
        </div>
    </div>
</body>
</html>
```
Output
![tambah](https://github.com/RIFAANDIANI/Lab8Web/assets/115616294/8dfaf304-7696-40c5-876a-5ed64a7aa8d8)
## SETELAH DITAMBAH 
![setelah ditambah](https://github.com/RIFAANDIANI/Lab8Web/assets/115616294/1176efd3-3265-4c4f-b538-51d8118cd65c)

## 10. Mengubah Data (Update)
Buat file baru dengan nama ubah.php
```php
<?php
error_reporting(E_ALL);
include_once 'koneksi.php';

if (isset($_POST['submit']))
{
    $id = $_POST['id'];
    $nama = $_POST['nama'];
    $kategori = $_POST['kategori'];
    $harga_jual = $_POST['harga_jual'];
    $harga_beli = $_POST['harga_beli'];
    $stok = $_POST['stok'];
    $file_gambar = $_FILES['file_gambar'];
    $gambar = null;

    if ($file_gambar['error'] == 0)
    {
        $filename = str_replace(' ', '_', $file_gambar['name']);
        $destination = dirname(__FILE__) . '/gambar/' . $filename;
        if (move_uploaded_file($file_gambar['tmp_name'], $destination))
        {
            $gambar = 'gambar/' . $filename;;
        }    
    }

    $sql = 'UPDATE data_barang SET ';
    $sql .= "nama = '{$nama}', kategori = '{$kategori}', ";
    $sql .= "harga_jual = '{$harga_jual}', harga_beli = '{$harga_beli}', stok = '{$stok}' ";
    if (!empty($gambar))
        $sql .= ", gambar = '{$gambar}' ";
    $sql .= "WHERE id_barang = '{$id}'";
    $result = mysqli_query($conn, $sql);   
    
    header('location: index.php');
}

$id = $_GET['id'];
$sql = "SELECT * FROM data_barang WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);
if (!$result) die('Error: Data tidak tersedia');
$data = mysqli_fetch_array($result);

function is_select($var, $val) {
    if ($var == $val) return 'selected="selected"';
    return false;
}

?>
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Ubah Barang</title>
    <link href="style-ubah.css" rel="stylesheet" type="text/css" />
</head>

<body>
    <div class="container">
        <h1>Ubah Barang</h1>
        <div class="main">
            <form method="post" action="ubah.php" enctype="multipart/form-data">
            <div class="input">
                <label>Nama Barang</label>
                <input type="text" name="nama" value="<?php echo $data['nama'];?>" />
            </div>
                <div class="input">
                    <label>Kategori</label>
                    <select name="kategori">
                        <option <?php echo is_select ('Komputer', $data['kategori']);?> value="Komputer">Komputer</option>
                        <option <?php echo is_select ('Komputer', $data['kategori']);?> value="Elektronik">Elektronik</option>
                        <option <?php echo is_select ('Komputer', $data['kategori']);?> value="Hand Phone">Hand Phone</option>
                    </select>
                </div>
                <div class="input">
                    <label>Harga Jual</label>
                    <input type="text" name="harga_jual" value="<?php echo $data['harga_jual'];?>" />
                </div>
                <div class="input">
                    <label>Harga Beli</label>
                    <input type="text" name="harga_beli" value="<?php echo $data['harga_beli'];?>" />
                </div>
                <div class="input">
                    <label>Stok</label>
                    <input type="text" name="stok" value="<?php echo $data['stok'];?>" />
                </div>
                <div class="input">
                    <label>File Gambar</label>
                    <input type="file" name="file_gambar" />
                </div>
                <div class="submit">
                    <input type="hidden" name="id" value="<?php echo $data['id_barang'];?>" />
                    <input type="submit" name="submit" value="Simpan" />
                </div>
            </form> 
        </div>
    </div>
</body>
</html>
```
Output
![ubah barang](https://github.com/RIFAANDIANI/Lab8Web/assets/115616294/2eb0ebe7-3506-4fe0-8a4a-eccef430baaa)
## HASIL UBAH BARANG
![hasil ubah barang](https://github.com/RIFAANDIANI/Lab8Web/assets/115616294/75237383-278c-4141-8f72-0a1a6be59a3b)


## 11.Menghapus Data (Delete)
Buat file baru dengan nama hapus.php
```php
<?php
include_once 'koneksi.php';
$id = $_GET['id'];
$sql = "DELETE FROM data_barang WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);
header('location: index.php');
?>
```
Output
![hapus barang](https://github.com/RIFAANDIANI/Lab8Web/assets/115616294/be74a800-e654-4296-b245-7aeb6aff3d85)

# FINISH
