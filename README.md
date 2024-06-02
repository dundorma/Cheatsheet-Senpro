# Cheatsheet-Senpro

[SSH Keygen](#ssh-keygen)

[git (action, etc)](#git)

[Manipulasi direktori dan file pada terminal](#manipulasi-direktori)

# Content
<details>
<summary><h2 id="ssh-keygen">SSH Keygen</h2></summary>
  <div>

### Key Generation with `ssh-keygen`

`ssh-keygen` is a command-line tool that generates a new SSH key pair. Here's a basic guide on how it works:

1. **Open a terminal** and run the following command:
   ```bash
   ssh-keygen
   ```

2. **Follow the prompts**:
   - **Enter file in which to save the key**: You can press Enter to accept the default location (usually `~/.ssh/id_rsa`).
   - **Enter passphrase (empty for no passphrase)**: Adding a passphrase adds an extra layer of security, requiring you to enter the passphrase whenever you use the private key.
   - **Enter same passphrase again**: Confirm the passphrase.

3. **Completion**: After you complete these steps, you will have two new files:
   - `id_rsa`: This is your private key. Keep this file secure and never share it.
   - `id_rsa.pub`: This is your public key. You can share this file with any SSH server you want to connect to.

### Using SSH Keys for Authentication

1. **Copy the Public Key to the Server**:
   - You can use `ssh-copy-id` to copy your public key to a server:
     ```bash
     ssh-copy-id user@remote_host
     ```
   - Alternatively, you can manually append the contents of `id_rsa.pub` to the `~/.ssh/authorized_keys` file on the server.

2. **Connect to the Server**:
   - Once the public key is added to the server, you can connect to it without a password (or just with your passphrase if you set one):
     ```bash
     ssh user@remote_host
     ```

     ### Example

Here's a step-by-step example:

1. **Generate SSH Keys**:
   ```bash
   ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
   ```

2. **View the Public Key**:
   ```bash
   cat ~/.ssh/id_rsa.pub
   ```

3. **Copy the Public Key to a Server**:
   ```bash
   ssh-copy-id user@remote_host
   ```
   
4. **Connect to the Server**:
   ```bash
   ssh user@remote_host
   ```
   
  </div>
</details>

<details>
   <summary>
      <h2 id="git">Git Action</h2>
   </summary>
   <div>

### Contoh config file github workflow
```yml
name: Test, Build, and Deploy

on:
   push:
      branches:
         - main

jobs:
   test-build:
      runs-on: ubuntu-latest
      strategy:
         matrix:
            node-version: [16.x]

      steps:
         - uses: actions/checkout@v2
         - name: Testing Build pre-Deploy
            uses: actions/setup-node@v2
            with:
               node-version: ${{ matrix.node-version }}
               cache: "npm"
         - run: npm i
         - run: npm run build

   deploy:
      needs: test-build
      runs-on: ubuntu-latest

      strategy:
         matrix:
            node-version: [16.x]
      steps:
         - name: build app on vm
            uses: appleboy/ssh-action@master
            with:
               host: ${{ secrets.HOST }}
               username: ${{ secrets.USERNAME }}
               password: ${{ secrets.PASSWORD }}
               port: ${{ secrets.PORT }}
               script: |
                  eval "$(ssh-agent -s)"
                  ssh-add ~/.ssh/<<namafileprivkey>>
                  echo "cek folder project";
                  [ ! -d "${HOME}/repo/task-fusion" ] &&
                  {
                     echo "Cloning";
                     mkdir -p ~/repo;
                     cd ~/repo;
                     git clone https://github.com/zakiakmal/task-fusion.git;
                  } ||
                  {
                     echo "building";
                     cd ~/repo/task-fusion;
                     git restore .;
                     git pull origin main;
                  }
```
### Penjelasan

Kode di atas menunjukkan workflow dari github action yang memiliki sintaks yml. 

`on`: use `on` to define which events can cause the workflow to run. Contohnya `on: [push, fork]`

`jobs`: A workflow run is made up of one or more `jobs`, which run in parallel by default. To run jobs sequentially, you can define dependencies on other jobs using the `jobs.<job_id>.needs` keyword.

`strategy: matrix:`: Use `jobs.<job_id>.strategy.matrix` to define a matrix of different job configurations. Within your matrix, define one or more variables followed by an array of values.

Kemudian terdapat `${{ secrets.X }}` di dalam yml tersebut. maksud dari `${{ secrets.X }}` adalah dia mengambil variabel secrets yang bisa kita input pada settingan github, dan `X` merupakan nama variabel nya.

### Doing git with ssh

Untuk dapat melakukan perintah seperti clone, push, pull, etc,  menggunakan git ssh langkah yang perlu kita lakukan adalah
1. generate ssh seperti pada bagian [SSH Keygen](#ssh-keygen)
2. tambahkan public key yang telah digenerate ke dalam `SSH and GPG Keys` pada setting github sebagai SSH (New SSH)
3. tambah private ssh key ke dalam ssh authentication agent, dengan menjalankan perintah
      ```sh
      eval $(ssh-agent -s)
      ssh-add <<file_path_to_private_key>>
      ```

   </div>
</details>

<details>
<summary><h2 id="manipulasi-direktori">Manipulasi direktori dan file pada terminal</h2></summary>
  <div>

### Membuat Direktori

Untuk membuat direktori baru, gunakan perintah `mkdir` diikuti dengan nama direktori:
```sh
mkdir nama_direktori
```

### Menghapus Direktori

Untuk menghapus direktori kosong, gunakan perintah `rmdir`:
```sh
rmdir nama_direktori
```
Untuk menghapus direktori beserta isinya, gunakan perintah `rm -r`:
```sh
rm -r nama_direktori
```

### Melihat Isi Direktori

Untuk melihat isi dari sebuah direktori, gunakan perintah `ls`:
```sh
ls nama_direktori
```
Untuk melihat isi direktori beserta detail tambahan, gunakan perintah `ls -l`:
```sh
ls -l nama_direktori
```
Untuk melihat isi direktori termasuk file tersembunyi, gunakan perintah `ls -a`:
```sh
ls -a nama_direktori
```

### Membuat File

Untuk membuat file baru, gunakan perintah `touch` diikuti dengan nama file:
```sh
touch nama_file
```

### Menghapus File

Untuk menghapus file, gunakan perintah `rm` diikuti dengan nama file:
```sh
rm nama_file
```

### Menyalin File atau Direktori

Untuk menyalin file, gunakan perintah `cp` diikuti dengan nama file sumber dan tujuan:
```sh
cp file_sumber file_tujuan
```
Untuk menyalin direktori beserta isinya, gunakan perintah `cp -r`:
```sh
cp -r direktori_sumber direktori_tujuan
```

### Memindahkan atau Mengganti Nama File atau Direktori

Untuk memindahkan atau mengganti nama file atau direktori, gunakan perintah `mv` diikuti dengan nama file atau direktori sumber dan tujuan:
```sh
mv sumber tujuan
```

### Membuka File

Untuk membuka file dengan editor teks, misalnya `nano` atau `vim` (kalau kroco, pakai nano aja):
```sh
nano nama_file
```
atau
```sh
vim nama_file
```

### Melihat Isi File

Untuk melihat isi file tanpa membukanya di editor, gunakan perintah `cat`, `less`, atau `more`:
```sh
cat nama_file
```
atau
```sh
less nama_file
```
atau
```sh
more nama_file
```

  </div>
</details>

