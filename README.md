## Commit 1 Reflection Notes

Pada milestone pertama, saya mempelajari cara membuat web server sederhana menggunakan Rust.
Fungsi handle_connection menerima parameter TcpStream yang merepresentasikan koneksi
TCP dari browser ke server.

Di dalam fungsi tersebut, BufReader digunakan untuk membungkus stream agar bisa dibaca
baris per baris secara efisien. Method .lines() menghasilkan iterator atas setiap baris dari
HTTP request, .map() digunakan untuk meng-unwrap setiap Result<String> menjadi String,
dan .take_while(|line| !line.is_empty()) digunakan untuk berhenti membaca ketika menemukan
baris kosong — karena dalam protokol HTTP, header diakhiri dengan baris kosong.

Hasil akhirnya adalah Vec<String> yang berisi seluruh baris HTTP request header yang
dikirimkan oleh browser, seperti method (GET), path, versi HTTP, Host, User-Agent, dll.
Ini menunjukkan bahwa komunikasi antara browser dan server menggunakan format HTTP standar.

## Commit 2 Reflection Notes

Pada milestone kedua, saya memodifikasi handle_connection agar dapat mengirimkan respons
HTML ke browser. Beberapa hal baru yang saya pelajari:

Pertama, fs::read_to_string("hello.html") digunakan untuk membaca isi file HTML menjadi
sebuah String. File ini harus berada di direktori yang sama dengan tempat kita menjalankan
cargo run.

Kedua, respons HTTP memiliki format tertentu: diawali dengan status line (HTTP/1.1 200 OK),
diikuti header seperti Content-Length yang memberi tahu browser ukuran body dalam bytes,
kemudian dua baris kosong sebagai pemisah antara header dan body, dan terakhir
body berupa isi HTML. Tanpa Content-Length yang benar, browser mungkin tidak dapat
merender halaman dengan benar.

Ketiga, stream.write_all(response.as_bytes()) digunakan untuk mengirim response sebagai
bytes melalui TCP stream ke browser.

![Commit 2 screen capture](assets/images/commit2.png)