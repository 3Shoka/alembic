
Android Studio merupakan standar IDE yang digunakan untuk membangun aplikasi mobile berbasis android yang sudah lengkap dengan semua tools maupun SDK nya, tapi bagaimana jika kita ingin membangun aplikasi berbasis android tapi tidak mau menggunakan Android studio dan hanya ingin menginstal Android SDK nya saja ?

Cara mendownload Android SDK tools tanpa menggunakan IDE salah satunya adalah menggunakan Android command line tools yang bisa di download di [download page](https://developer.android.com/studio/index.html#command-tools) Android Studio dan menggunakan sdkmanager untuk mendownload paket SDK lainnya.

sdkmanager adalah command line tools yang bisa digunakan untuk menampilkan, menginstall, mengupdate maupun uninstall package untuk Android SDK, jika sudah menggunakan Android Studio kita tidak memerlukan sdkmanager ini, karena sudah terinstall otomatis didalamnya. sdkmanager ini menyediakan package Android SDK tools, terletak di folder tools/bin.

Download dan ekstrak sdk tools tadi, misal di folder ~/Android maka sdkmanager bisa ditemukan di folder ~/Android/tools/bin. Setelah itu kita tambahkan folder Android tadi sebagai ANDROID_HOME dan folder tools maupun bin nya di sistem path kita agar bisa eksekusi sdkmanager dari mana saja, bisa dengan cara menambahkan kode berikut ke .bashrc 

``` bash
export ANDROID_HOME=/home/ashoka/Android
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/tools/bin

```

Setelah selesai, jalankan `sdkmanager --update` di terminal, untuk mengupdate package yang terinstall, jika ada warning *File bla bla bla .android/repositories.cfg could not be loaded.* jalankan perintah `touch ~/.android/repositories.cfg` dan jalankan lagi update nya. Kemudian download paket yang kita perlukan, misal `sdkmanager --verbose 'platforms;android-23' 'system-images;android-23;google_apis;x86_64'` untuk mendownload package API 23 untuk emulator x86_64. more info `sdkmanager --help` atau https://developer.android.com/studio/command-line/sdkmanager.html

paket yg dibutuhkan :
-- platforms;android-23 (setelah ini terus di update sdkamanager nya)
-- system-images;android-23;default;x86_64

buat avd nya (cobain aja salahsatu)
<!-- avdmanager --verbose create avd -n avdN5X -k "system-images;android-23;default;x86_64" -b x86_64 -d 9 -->
`avdmanager --verbose create avd -n test1 -k 'system-images;android-23;default;x86_64'`
<!-- avdmanager --verbose create avd -n pixel -k 'system-images;android-26;google_apis;x86' -d 17 -->
more info `avdmanager --help` atau https://developer.android.com/studio/command-line/avdmanager.html

jalankan emulator dengan `emulator @test1`
