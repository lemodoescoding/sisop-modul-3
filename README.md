[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/Eu-CByJh)
| NRP | Name |
| :--------: | :------------: |
| 5025241054 | Andie Azril Alfrianto |
| 5025241060 | Christina Tan |
| 5025241061 | Ahmad Satrio Arrohman |

# Praktikum Modul 3 _(Module 3 Lab Work)_

### Laporan Resmi Praktikum Modul 3 _(Module 3 Lab Work Report)_

Di suatu pagi hari yang cerah, Budiman salah satu mahasiswa Informatika ditugaskan oleh dosennya untuk membuat suatu sistem operasi sederhana. Akan tetapi karena Budiman memiliki keterbatasan, Ia meminta tolong kepadamu untuk membantunya dalam mengerjakan tugasnya. Bantulah Budiman untuk membuat sistem operasi sederhana!

_One sunny morning, Budiman, an Informatics student, was assigned by his lecturer to create a simple operating system. However, due to Budiman's limitations, he asks for your help to assist him in completing his assignment. Help Budiman create a simple operating system!_

### Soal 1

> Sebelum membuat sistem operasi, Budiman diberitahu dosennya bahwa Ia harus melakukan beberapa tahap terlebih dahulu. Tahap-tahapan yang dimaksud adalah untuk **mempersiapkan seluruh prasyarat** dan **melakukan instalasi-instalasi** sebelum membuat sistem operasi. Lakukan seluruh tahapan prasyarat hingga [perintah ini](https://github.com/arsitektur-jaringan-komputer/Modul-Sisop/blob/master/Modul3/README-ID.md#:~:text=sudo%20apt%20install%20%2Dy%20busybox%2Dstatic) pada modul!

> _Before creating the OS, Budiman was informed by his lecturer that he must complete several steps first. The steps include **preparing all prerequisites** and **installing** before creating the OS. Complete all the prerequisite steps up to [this command](https://github.com/arsitektur-jaringan-komputer/Modul-Sisop/blob/master/Modul3/README-ID.md#:~:text=sudo%20apt%20install%20%2Dy%20busybox%2Dstatic) in the module!_

**Answer:**

- **Code:**

```bash
sudo bash

sudo apt -y update
sudo apt -y install qemu-system build-essential bison flex libelf-dev libssl-dev bc grub-common grub-pc libncurses-dev libssl-dev mtools grub-pc-bin xorriso tmux

mkdir -p osboot3
cd osboot3

mkdir -p osboot
cd osboot

cd osboot3

wget https://cdn.kernel.org/pub/linux/kernel/v6.x/linux-6.1.1.tar.xz
tar -xvf linux-6.1.1.tar.xz
cd linux-6.1.1

make menuconfig

make -j$(nproc)

cd ../osboot

cp ../linux-6.1.1/arch/x86/boot/bzImage ./
```

- **Explanation:**

  Menggunakan sudo bash untuk masuk ke shell bash sebagai superuser atau root. Selanjutnya install dan perbarui software pendukung, tools yang dibutuhkan seperti qemu, build-essential, bison, flex, dan lainnya. Selanjutnya buat direktori osboot dan download kernel linux menggunakan wget dan extract file tersebut. Lalu, make menuconfig dan memilih fitur-fitur yang dibutuhkan, bisa mengikuti konfigurasinya dari modul, lalu tambahkan ```General devices -> Configure standard kernel features -> Multiple users, groups, and capabilities support``` untuk mengaktifkan multi-user. Setelah selesai konfigurasi, jalankan ```make -j$(nproc)``` untuk build kernel. Setelah itu, akan menghasilkan file bzImage. Lalu copy bzImage ke osboot.

- **Screenshot:**

  Centang bagian multiple users

  ![konfigurasi kernel](./assets/1a.png)
  
  ![build bzImage](./assets/1b.png)

### Soal 2

> Setelah seluruh prasyarat siap, Budiman siap untuk membuat sistem operasinya. Dosen meminta untuk sistem operasi Budiman harus memiliki directory **bin, dev, proc, sys, tmp,** dan **sisop**. Lagi-lagi Budiman meminta bantuanmu. Bantulah Ia dalam membuat directory tersebut!

> _Once all prerequisites are ready, Budiman is ready to create his OS. The lecturer asks that the OS should contain the directories **bin, dev, proc, sys, tmp,** and **sisop**. Help Budiman create these directories!_

**Answer:**

- **Code:**

```bash
cd ~
sudo apt install -y busybox-static

cd osboot3/osboot
mkdir -p myramdisk/{bin,dev,proc,sys,tmp,etc,sisop}

cd myramdisk/dev
cp -a /dev/null ./           
cp -a /dev/tty* ./           
cp -a /dev/zero ./           
cp -a /dev/console ./

cp /usr/bin/busybox myramdisk/bin
cd myramdisk/bin
./busybox --install .
```

- **Explanation:**

  Pertama, install busybox. Di dalam busybox tersedia berbagai utilitas sistem yang digunakan untuk mengonfigurasi perangkat keras, memanipulasi sistem berkas, dan melakukan tugas penting lainnya selama proses booting. Setelah itu, buat direktori bin, dev, proc, sys, tmp, etc,dan sisop dalam myramdisk. Direktori /dev di Linux berisi file perangkat penting seperti null, tty, zero, dan console. Kita akan menyalinnya dari sistem host ke dalam direktori dev di myramdisk. Selanjutnya, kita akan menyalin file BusyBox ke direktori bin yang telah dibuat di dalam myramdisk. Kemudian, kita akan menginstal semua utilitas yang disediakan oleh BusyBox.

- **Screenshot:**

   hasil ls dari myramdisk

  ![dibuat bin, dev, proc, sys, tmp, etc, dan sisop](./assets/2a.png)

### Soal 3

> Budiman lupa, Ia harus membuat sistem operasi ini dengan sistem **Multi User** sesuai permintaan Dosennya. Ia meminta kembali kepadamu untuk membantunya membuat beberapa user beserta directory tiap usernya dibawah directory `home`. Buat pula password tiap user-usernya dan aplikasikan dalam sistem operasi tersebut!

> _Budiman forgot that he needs to create a **Multi User** system as requested by the lecturer. He asks your help again to create several users and their corresponding home directories under the `home` directory. Also set each user's password and apply them in the OS!_

**Format:** `user:pass`

```
root:Iniroot
Budiman:PassBudi
guest:guest
praktikan1:praktikan1
praktikan2:praktikan2
```

**Answer:**

- **Code:**

```bash
mkdir -p myramdisk/{home/root,home/Budiman,home/guest,home/praktikan1,home/praktikan2}

cd myramdisk/etc
touch passwd
touch group

openssl passwd -1 Iniroot
openssl passwd -1 PassBudi
openssl passwd -1 guest
openssl passwd -1 praktikan1
openssl passwd -1 praktikan2

nano passwd
// isi dari passwd //
{
root:$1$DMlRNHHa$HX8SPo0qXI1GU90xRm3KS.:0:0:root:/home/root:/bin/sh
Budiman:$1$DzAJqgPH$O7mHhSUdCvDPoiOb.W8VX.:1000:1000:Budiman:/home/Budiman:/bin/sh
guest:$1$OSxlzVey$1Zjv.sjplAAFCspoW5lbh0:1001:1001:guest:/home/guest:/bin/sh
praktikan1:$1$v4tx2dmS$Fz9H.s0Poqlbm28d8oVEe/:1002:1002:praktikan1:/home/praktikan1:/bin/sh
praktikan2:$1$HPmhfaCm$puML9tyHiDXYBIDSnO.Bo.:1003:1003:praktikan2:/home/praktikan2:/bin/sh
}

cd ..
touch init
nano init

// isi dari init //
{
#!/bin/sh
/bin/mount -t proc none /proc
/bin/mount -t sysfs none /sys

while true
do
    /bin/getty -L tty1 115200 vt100
    sleep 1
done
}

chmod +x init

nano group
// isi dari group //
{
root:x:0:
bin:x:1:root
sys:x:2:root
tty:x:5:root,Budiman,guest,praktikan1,praktikan2
disk:x:6:root
wheel:x:10:root,Budiman,guest,praktikan1,praktikan2
users:x:100:Budiman,guest,praktikan1,praktikan2
}

find . | cpio -oHnewc | gzip > ../myramdisk.gz
```

- **Explanation:**

  Pertama, buat direktori dalam myramdisk dengan format ```/home/(user)```, lalu pindah ke direkori etc dan buat file passwd untuk menyimpan informasi user dan group untuk menyimpan informasi group. Kemudian, gunakan ```openssl passwd -1 (password)``` untuk mendapatkan hash MD5 dari password. Hasilnya nanti akan digunakan sebagai field kedua di file passwd. Lalu, tulis ke file passwd dengan format ```username:password:UID:GID:GECOS:/home/username:/bin/sh```. UID dan GID sama sehingga setiap user memiliki grup yang berisi mereka sendiri. Lalu, buat file init di direktori myramdisk. Script init akan menjadi program pertama yang dijalankan setelah boot. program init didapatkan dari modul sisop. Setelah itu, buat ```chmod +x init``` agar file init bisa dieksekusi. Selanjutnya, buka file group dan salin program ke dalam group. program group didapatkan dari modul sisop, hanya ditambahkan user-usernya saja. ```tty, wheel, dan users``` berisi user biasa agar bisa mengakses terminal atau fitur admin tertentu. Lalu, buat intramfs dengan command ```find . | cpio -oHnewc | gzip > ../myramdisk.gz```

- **Screenshot:**

  hasil generate password

  ![hasil generate openssl -1 {password}](./assets/3a.png)

   masukkan hasil generate ke passwd

  ![isi file passwd](./assets/3b.png)

  isi dari file group

  ![isi file group](./assets/3c.png)


### Soal 4

> Dosen meminta Budiman membuat sistem operasi ini memilki **superuser** layaknya sistem operasi pada umumnya. User root yang sudah kamu buat sebelumnya akan digunakan sebagai superuser dalam sistem operasi milik Budiman. Superuser yang dimaksud adalah user dengan otoritas penuh yang dapat mengakses seluruhnya. Akan tetapi user lain tidak boleh memiliki otoritas yang sama. Dengan begitu user-user selain root tidak boleh mengakses `./root`. Buatlah sehingga tiap user selain superuser tidak dapat mengakses `./root`!

> _The lecturer requests that the OS must have a **superuser** just like other operating systems. The root user created earlier will serve as the superuser in Budiman's OS. The superuser should have full authority to access everything. However, other users should not have the same authority. Therefore, users other than root should not be able to access `./root`. Implement this so that non-superuser accounts cannot access `./root`!_

**Answer:**

- **Code:**

```bash
chmod 644 etc/passwd
chmod 600 etc/group
chown 0:0 home/root
chown 1000:1000 home/Budiman
chown 1001:1001 home/guest
chown 1002:1002 home/praktikan1
chown 1003:1003 home/praktikan2
chmod 700 home/root
```

- **Explanation:**

  ```chmod 644 etc/passwd``` mengatur izin untuk read & write untuk owner, read saja untuk group dan lainnya. ```chmod 600 etc/group``` mengatur izin untuk read & write untuk owner, sedangkan group & lainnya tidak bisa rwx. Lalu, buat ```chown UID:GID home/(user)``` untuk memberikan setiap user hak milik penuh atas home directorinya. Lalu, ```chmod 700 home/root``` untuk memberi owner saja yang bisa rwx, dalam hal ini hanya root sendiri yang mempunyai akses tersebut.

- **Screenshot:**

  Bukti user tidak bisa akses ke root

  ![ss no 4](./assets/4.png)

### Soal 5

> Setiap user rencananya akan digunakan oleh satu orang tertentu. **Privasi dan otoritas tiap user** merupakan hal penting. Oleh karena itu, Budiman ingin membuat setiap user hanya bisa mengakses dirinya sendiri dan tidak bisa mengakses user lain. Buatlah sehingga sistem operasi Budiman dalam melakukan hal tersebut!

> _Each user is intended for an individual. **Privacy and authority** for each user are important. Therefore, Budiman wants to ensure that each user can only access their own files and not those of others. Implement this in Budiman's OS!_

**Answer:**

- **Code:**

  `put your answer here`

- **Explanation:**

  `put your answer here`

- **Screenshot:**

  `put your answer here`

### Soal 6

> Dosen Budiman menginginkan sistem operasi yang **stylish**. Budiman memiliki ide untuk membuat sistem operasinya menjadi stylish. Ia meminta kamu untuk menambahkan tampilan sebuah banner yang ditampilkan setelah suatu user login ke dalam sistem operasi Budiman. Banner yang diinginkan Budiman adalah tulisan `"Welcome to OS'25"` dalam bentuk **ASCII Art**. Buatkanlah banner tersebut supaya Budiman senang! (Hint: gunakan text to ASCII Art Generator)

> _Budiman wants a **stylish** operating system. Budiman has an idea to make his OS stylish. He asks you to add a banner that appears after a user logs in. The banner should say `"Welcome to OS'25"` in **ASCII Art**. Use a text to ASCII Art generator to make Budiman happy!_ (Hint: use a text to ASCII Art generator)

**Answer:**

- **Code:**

  `put your answer here`

- **Explanation:**

  `put your answer here`

- **Screenshot:**

  `put your answer here`

### Soal 7

> Melihat perkembangan sistem operasi milik Budiman, Dosen kagum dengan adanya banner yang telah kamu buat sebelumnya. Kemudian Dosen juga menginginkan sistem operasi Budiman untuk dapat menampilkan **kata sambutan** dengan menyebut nama user yang login. Sambutan yang dimaksud berupa kalimat `"Helloo %USER"` dengan `%USER` merupakan nama user yang sedang menggunakan sistem operasi. Kalimat sambutan ini ditampilkan setelah user login dan setelah banner. Budiman kembali lagi meminta bantuanmu dalam menambahkan fitur ini.

> _Seeing the progress of Budiman's OS, the lecturer is impressed with the banner you created. The lecturer also wants the OS to display a **greeting message** that includes the name of the user who logs in. The greeting should say `"Helloo %USER"` where `%USER` is the name of the user currently using the OS. This greeting should be displayed after user login and after the banner. Budiman asks for your help again to add this feature._

**Answer:**

- **Code:**

  `put your answer here`

- **Explanation:**

  `put your answer here`

- **Screenshot:**

  `put your answer here`

### Soal 8

> Dosen Budiman sudah tua sekali, sehingga beliau memiliki kesulitan untuk melihat tampilan terminal default. Budiman menginisiatif untuk membuat tampilan sistem operasi menjadi seperti terminal milikmu. Modifikasilah sistem operasi Budiman menjadi menggunakan tampilan terminal kalian.

> _Budiman's lecturer is quite old and has difficulty seeing the default terminal display. Budiman takes the initiative to make the OS look like your terminal. Modify Budiman's OS to use your terminal appearance!_

**Answer:**

- **Code:**

  `put your answer here`

- **Explanation:**

  `put your answer here`

- **Screenshot:**

  `put your answer here`

### Soal 9

> Ketika mencoba sistem operasi buatanmu, Budiman tidak bisa mengubah text file menggunakan text editor. Budiman pun menyadari bahwa dalam sistem operasi yang kamu buat tidak memiliki text editor. Budimanpun menyuruhmu untuk menambahkan **binary** yang telah disiapkan sebelumnya ke dalam sistem operasinya. Buatlah sehingga sistem operasi Budiman memiliki **binary text editor** yang telah disiapkan!

> _When trying your OS, Budiman cannot edit text files using a text editor. He realizes that the OS you created does not have a text editor. Budiman asks you to add the prepared **binary** into his OS. Make sure Budiman's OS has the prepared **text editor binary**!_

**Answer:**

- **Code:**

  `put your answer here`

- **Explanation:**

  `put your answer here`

- **Screenshot:**

  `put your answer here`

### Soal 10

> Setelah seluruh fitur yang diminta Dosen dipenuhi dalam sistem operasi Budiman, sudah waktunya Budiman mengumpulkan tugasnya ini ke Dosen. Akan tetapi, Dosen Budiman tidak mau menerima pengumpulan selain dalam bentuk **.iso**. Untuk terakhir kalinya, Budiman meminta tolong kepadamu untuk mengubah seluruh konfigurasi sistem operasi yang telah kamu buat menjadi sebuah **file .iso**.

> After all the features requested by the lecturer have been implemented in Budiman's OS, it's time for Budiman to submit his assignment. However, Budiman's lecturer only accepts submissions in the form of **.iso** files. For the last time, Budiman asks for your help to convert the entire configuration of the OS you created into a **.iso file**.

**Answer:**

- **Code:**

  `put your answer here`

- **Explanation:**

  `put your answer here`

- **Screenshot:**

  `put your answer here`

---

Pada akhirnya sistem operasi Budiman yang telah kamu buat dengan susah payah dikumpulkan ke Dosen mengatasnamakan Budiman. Kamu tidak diberikan credit apapun. Budiman pun tidak memberikan kata terimakasih kepadamu. Kamupun kecewa tetapi setidaknya kamu telah belajar untuk menjadi pembuat sistem operasi sederhana yang andal. Selamat!

_At last, the OS you painstakingly created was submitted to the lecturer under Budiman's name. You received no credit. Budiman didn't even thank you. You feel disappointed, but at least you've learned to become a reliable creator of simple operating systems. Congratulations!_

