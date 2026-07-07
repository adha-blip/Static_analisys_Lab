<img width="991" height="448" alt="01  WUC" src="https://github.com/user-attachments/assets/5c510c56-36cb-4c08-96af-d14160a55e1e" />
**World Useless  Crackmes**

Baik Disini kita akan melakukan static analisis terhadap "World Useless Crackmes"

<img width="1210" height="764" alt="02  WUC" src="https://github.com/user-attachments/assets/beebbb6c-bf0d-42c5-81fa-d3f135bc5280" />

**Identifikasi Awal**
Berbeda dengan program sebelumnya yang menggunakan bahasa C, executable kali ini dikompilasi dari kode sumber C++.
karena dari penggunaan memori dinamis dan kelas bawaan biasanya digunakan dalam C++,
seperti std::allocator (baris 12) dan inisialisasi std::__cxx11::string (baris 13-15) untuk mengelola teks.

**Menemukan hal yg terlihat menarik (Baris 16)**
Baris 16 (Pemanggilan Fungsi Mencurigakan):
Program memanggil fungsi eksternal bernama obfStr000D(local_48).\
Nama fungsi ini cukup mencurigakan dan kemungkinan merupakan singkatan dari Obfuscated String (Teks yang disandikan/diacak).
Teks "beribakberi" tadi diumpankan ke dalam fungsi ini untuk diproses. jadi selanjutnya kita akn coba masuk fungsi ini


<img width="556" height="479" alt="03  UWC" src="https://github.com/user-attachments/assets/1621acf6-c8be-4dc0-a09b-6e07b385d707" />

Disini  kita dapat melihat di baris
97-100: Program membuat string abjad acuan (alphabet referensi) local_68 yang berisi **"abcdefghijklmnopqrstuvwxyz"**
lalu

<img width="556" height="493" alt="04  UWC" src="https://github.com/user-attachments/assets/885fc662-e366-493e-9e5f-b1dd8d532261" />

pada Baris 147-152 : Program melakukan serangkaian perbandingan terhadap elemen array local_70.
Agar fungsi mencetak pesan "Passed" (Berhasil), nilai-nilai dalam local_70 wajib memenuhi pola ini:
- local_70[0] == 0x1a (Desimal: 26)
- local_70[1] == 0x15 (Desimal: 21)
- local_70[uVar4] == 0xf (Desimal: 15)
- local_70[uVar4 + 1] == 0x1b (Desimal: 27)
- local_70[uVar4 * 2] == 0x18 (Desimal: 24)
- local_70[uVar4 * 2 + 1] == 0x11 (Desimal: 17)
Jika kondisi angka-angka hexadecimal tersebut tidak tercapai, program akan mencetak "Try" (Gagal).

**Kesimpulan yang saya temukan**
Program ini memang sangat aneh (sesuai judulnya "Useless").
Program ini sengaja mengirimkan string "beribakberi" dari fungsi main,
tetapi di dalam fungsi pengecekan obfStr000D, program secara hardcoded mengharuskan panjang string sama dengan 6.
Ini menyebabkan teks tidak akan pernah diproses dengan benar.

**Menurut saya Satu-satunya cara agar teks "Passed" muncul secara alami adalah dengan menambal (patching) file binary-nya**
