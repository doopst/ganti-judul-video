# ganti-judul-video-youtube
Script untuk mengganti judul dan deskripsi video youtube agar menyesuaikan dengan jumlah views, likes, komentar, subscriber, dan berapa tahun, bulan, dan hari yang lalu video diupload. 

Oke! begini caranya ☺

<b> Step 1</b> <br>
Buka https://script.google.com/ 

<b> Step 2</b> <br>
Klik tombol "Project Baru" atau "New Project" di sebelah kiri bagian atas

<b> Step 3</b> <br>
Setelah loading selesai, pada panel sebelah kiri, klik tombol + di sebelah kanan tulisan "Layanan"

<b> Step 4</b> <br>
Setelah popup muncul, scroll ke bawah pada daftar API sampai ketemu "YouTube Data API V3" lalu diklik

<b> Step 5</b> <br>
Hapus script yang tersedia
```
function myFunction() {
  
}
```
dan ganti dengan script yang ini:
```
function updateTitleAndDescription() {
  var videoID = 'ID_VIDEO_KALIAN'; // ganti ID_VIDEO_KALIAN dangan ID video YouTube kalian! Contoh: t4q3GQZHvcg
  var channelId = "ID_CHANNEL_KALIAN"; // ganti ID_CHANNEL_KALIAN dengan ID channel YouTube kalian! Contoh: UCIbWCZyYUysTkDDNcV-Zqow
  var apiKey = "API_KEY_KALIAN"; // ganti API_KEY_KALIAN dengan API Key yg kalian dapat dari project google cloud console! Contoh: AIxxxxxxxxxxxxxxxxx-xxxxxxx_xxxxxxxxxxx
  var part = 'snippet,statistics';
  var params = {'id': videoID};
  
  // Penghitung statistik video
  var response = YouTube.Videos.list(part, params);
  var video = response.items[0];
  var videoViewsCount = video.statistics.viewCount;
  var videoLikeCount = video.statistics.likeCount;
  var videoCommentCount = video.statistics.commentCount;
  
  // Judul video
  var videoTitle = 'Video ini memiliki ' + videoViewsCount + ' views, ' + videoLikeCount + ' like, dan ' + videoCommentCount + ' komentar'; // ganti sesuai keinginan
  video.snippet.title = videoTitle;

  // Fungsi untuk mendapatkan statistik channel
  var channelUrl = "https://www.googleapis.com/youtube/v3/channels?part=statistics&id=" + channelId + "&key=" + apiKey;
  var channelResponse = UrlFetchApp.fetch(channelUrl);
  var channelData = JSON.parse(channelResponse.getContentText());
  var channelStatistics = channelData.items[0].statistics;
  
  // Penghitung subscriber
  var subscriber = channelStatistics.subscriberCount;

  // Penghitung waktu video diupload, jangan lupa tambahkan moment.js ke project kalian agar tidak ada error!
  var uploadDate = moment('2021-11-21'); // ganti 2021-11-21 sesuai tanggal video kalian diupload, ket: tahun-bulan-tanggal
  var currentDate = moment();
  var duration = moment.duration(currentDate.diff(uploadDate));
  var years = duration.years();
  var months = duration.months();
  var days = duration.days();
  
  // Deskripsi video
  var description = "Hi! Terimakasih telah membuat video ini menjadi ditonton sebanyak " + videoViewsCount + " kali, di like sebanyak " + videoLikeCount + " kali, dan dikomentari sebanyak " + videoCommentCount + " kali, walaupun videonya sudah lama, diuploadnya sudah " + years + " tahun, " + months + " bulan, dan " + days + " hari yang lalu (maaf kalau ga akurat), tepatnya pada tanggal 21 November 2021. Oh iya terima kasih juga telah mensubscribe channel ini sampai memiliki " + subscriber + " subscriber! Walaupun masih sedikit, ya gpp lah!\n\nDan kalau kalian mau subscribe sih gpp, tapi lebih baik gausah subscribe lah, soalnya kan channel ini jarang upload juga. Dan jangan lupa juga follow sosial media lain:\nIG: https://www.instagram.com/teldoop \nTikTok: https://www.tiktok.com/@teldoop\n\nKalau ada yang mau ditanyakan:\nEmail: admin@teldoop.my.id\nTanya Anonim: https://ngl.link/teldoop\nbisa juga DM IG atau tanya langsung di kolom komentar\n\nOke! Thanks for watching walaupun videonya cuma 5 detik😂";
  
  video.snippet.description = description;
  
  try {
    YouTube.Videos.update(video, part);
  } catch(e) {
    
  }
}
```
<b>Keterangan:</b><br><br>
<b>' + videoViewsCount + '</b> adalah penghitung jumlah views video<br>
<b>' + videoLikeCount + '</b> adalah penghitung jumlah like video<br>
<b>' + videoCommentCount + '</b> adalah penghitung jumlah komentar di video<br>
<b>' + subscriber + '</b> adalah penghitung subscriber channel<br>
<b>' + years + '</b> adalah penghitung berapa tahun lalu video diupload<br>
<b>' + months + '</b> adalah penghitung lebih berapa bulan lalu video diupload<br>
<b>' + days + '</b> adalah penghitung lebih berapa hari lalu video diupload<br>
<b>\n</b> adalah pindah baris (cuma buat deskripsi, bukan di judul)<br><br>
Note: untuk di deskripsi video (baris ke37), ganti tanda ' menjadi tanda "
<br><br>
<b>dan jangan lupa juga untuk mengganti:</b><br><br>
<b>ID_VIDEO_KALIAN (pada baris kedua)</b> dengan ID Video YouTube kalian yang akan kalian ganti judul dan deskripsinya, misalnya url video adalah https://www.youtube.com/watch?v=t4q3GQZHvcg maka IDnya adalah <b>t4q3GQZHvcg</b><br><br>
<b>ID_CHANNEL_KALIAN (pada baris ketiga)</b> dengan ID Channel YouTube kalian <b>(bukan @username atau /c/customurl)</b> yang terdapat video yang akan kalian ganti judul dan deskripsinya, misalnya url channel adalah https://www.youtube.com/channel/UCIbWCZyYUysTkDDNcV-Zqow maka IDnya adalah <b>UCIbWCZyYUysTkDDNcV-Zqow</b><br><br>
<b>API_KEY_KALIAN (pada baris ke4)</b> dengan API key yang terdapat pada project google cloud console kalian<br><br>
<b>2021-11-21 (pada baris ke29)</b> dengan tanggal video kalian diupload<br><br>
dan jangan lupa juga untuk mengganti <b>judul (di antara tanda ' pertama sampai tanda ' terakhir di baris ke16)</b> dan <b>deskripsi (di antara tanda " pertama sampai tanda " terakhir di baris ke37)</b> dan jika kalian ingin nantinya jumlah views, likes, komentar, subscriber, berapa tahun yang lalu, bulan yang lalu, dan hari yang lalu tersinkronkan dengan judul dan deskripsi video, copy dan pastekan beberapa keterangan yang dicetak tebal (keterangan ada di bawah script kedua di step 5 di file readme.md ini) ke lanjutan judul dan deskripsi yang kalian ketik<br><br>

Berikut adalah contoh script (baris ke16) yg sdh berhasil mengubah judul video:<br>
```
var videoTitle = 'Video ini memiliki ' + videoViewsCount + ' views, ' + videoLikeCount + ' like, dan ' + videoCommentCount + ' komentar';
```
<br>
Jika misalnya videonya memiliki 243 views, 4 likes, dan 6 komentar, maka, judul videonya akan menjadi seperti:<br>
<b>Video ini memiliki 243 views, 4 like, dan 6 komentar</b><br><br>

Dan berikut adalah contoh script (baris ke37) yg sdh berhasil mengubah deskripsi video:<br>
```
var description = "Video ini ditonton sebanyak " + videoViewsCount + " kali, di like sebanyak " + videoLikeCount + " kali, dikomentari sebanyak " + videoCommentCount + " kali, diupload " + years + " tahun, " + months + " bulan, dan " + days + " hari yang lalu.\n\nChannel ini memiliki " + subscriber + " subscriber."
```
<br>
Jika misalnya videonya memiliki 243 views, 4 likes, 6 komentar, dan channelnya memiliki 12 subscriber, dan videonya diupload pada 21 November 2021, dan hari ini adalah tanggal 23 Juni 2023, maka deskripsinya akan menjadi seperti:<br><br>
<b>Video ini ditonton sebanyak 243 kali, di like sebanyak 4 kali, dikomentari sebanyak 6 kali, diupload 1 tahun, 7 bulan, dan 0 hari yang lalu.<br><br>Channel ini memiliki 12 subscriber.</b><br><br>

begitulah scriptnya,, "<br><br>
<b>Cara kedua untuk mendapatkan ID Channel, jika sudah berhasil dengan cara yg pertama tadi, skip langkah ini:(jika kalian menggunakan hp, buka di google chrome dan klik titik tiga di kanan atas browser, dan centang opsi "Situs desktop", jangan buka di aplikasi YouTube)</b><br>
- Buka https://www.youtube.com/
- Klik pp youtube kalian di kanan atas
- Scroll ke bawah (jika perlu) sampai ketemu "Setelan" lalu diklik
- di panel sebelah kiri, klik "Setelan lanjutan" yg berada di paling bawah di antara menu-menu lainnya
- Disitu, kalian langsung bisa melihat ID Channel YouTube kalian, ID Channel terletak di bawah ID pengguna dan di atas "Channel default, atau ID pengguna, tapi di awalannya ditambahkan
- Di bagian ID channel, klik tombol "Salin" yg berwarna biru

<b>Untuk mendapatkan API KEY:</b><br>
- Buka https://console.cloud.google.com/
- Klik "Select a project" di samping kanan logo google cloud pada bagian kiri atas
- Klik "NEW PROJECT" di bagian kanan atas popup
- Namai project kalian sesuai keinginan, Misalnya: ganti judul video
- Klik "CREATE"
- Di panel sebelah kiri, klik "APIs & Services"
- Lalu klik "Enabled APIs & Services"
- Klik tombol "+ ENABLE APIS AND SERVICES" di bawah kolom pencarian
- Scroll ke bawah sampai menemukan "YouTube Data API V3" lalu diklik
- Klik tombol "ENABLE" yg berwarna biru lalu tunggu loading
- Lalu klik "Credentials" pada panel sebelah kiri
- Di bawah kolom pencarian, klik "+ CREATE CREDENTIALS"
- Pilih API Key
- Tunggu Loading sampai muncul popup "API key created"
- Copykan API keynya (yang ada di kolom "Your API key")
- Klik "Close"
- Pada bagian API Keys, kan ada tuh API key yg sdh kalian buat tadi, tinggal dekatkan cursor ke API key yg kalian buat tadi, habis itu muncul tombol titik tiga di bawah tulisan "Actions", itu kalian klik, habis itu klik "Edit API Key"
- Scroll sampai bawah (jika perlu) sampai kalian ketemu bagian "API restrictions"
- Disitu ada 2 opsi, secara default, opsi ini diatur ke "Don't restrict key" tapi, supaya API Key nya berfungsi di scriptnya, kalian pilih "Restrict Key"
- Kemudian dibawahnya akan muncul menu dropdown, silahkan kalian scroll sampai bawah sampai ketemu "YouTube Data API V3" lalu diklik, lalu klik tombol OK
- Klik tombol "SAVE" yg berwarna biru di paling bawah
- dah,, itu aja ☺

<b> Step 6</b><br><br>
Tambahkan file dengan mengklik tombol + di sebelah kanan tulisan "File" pada panel sebelah kiri.
kemudian beri nama file tersebut, misalnya: moment.gs

<b> Step 7</b><br><br>
Buka https://momentjs.com/

<b> Step 8</b><br><br>
Cari tulisan "Download" dan dibawah tulisan "Download", klik tombol "moment.js"

<b> Step 9</b><br><br>
Copy semua script yg ada di laman tersebut, lalu pastekan di file kedua yg kalian buat di Google Apps Script, (yg di atas diberi nama moment.gs) lalu tekan Ctrl + S pada keyboard

<b> Step 10</b><br><br>
Pada panel di sebelah kiri, kembali ke file pertama yg dibuat secara otomatis saat kalian buat project

<b> Step 11</b><br><br>
Klik tombol "Jalankan"

<b> Step 12</b><br><br>
Jika muncul popup, klik tombol "Berikan Akses"

<b> Step 13</b><br><br>
Pilih akun google yg sama seperti email akun youtube yg videonya akan kalian ubah judulnya, email akun google apps script saat ini dengan email akun YouTube harus sama agar tidak terjadi error

<b> Step 14</b><br><br>
jika terdapat pesan pada log eksekusi "Peringatan	Eksekusi selesai", Judul videonya berhasil diubah

<b> Step 15</b><br><br>
Agar judul dan deskripsi videonya berubah secara otomatis, arahkan kursor ke bagian paling kiri, lalu klik "Pemicu"

<b> Step 16</b><br><br>
Pada bagian kanan bawah, klik tombol "+ Tambahkan Pemicu" yg berwarna biru

<b> Step 17</b><br><br>
Scroll ke bawah (jika perlu), lalu di bagian "Pilih jenis pemicu berdasarkan waktu" pilih sesuai keinginan

<b> Step 18</b><br><br>
misalnya jika memilih "Timer menit" pada bagian di atasnya, di bagian "Pilih interval menit", pilih sesuai keinginan, misalnya "setiap 10 menit"

<b> Step 19</b><br><br>
Lalu klik simpan, dan judul video kalian akan berubah sesuai yang kalian tentukan

Semoga bermanfaat ☺, dan maaf kalau ada typo
