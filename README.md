<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


  { "en": "Apa itu jaringan komputer?", "id": "Kumpulan komputer yang saling terhubung." },
  { "en": "Kepanjangan LAN (Local Area Network)?", "id": "Local Area Network." },
  { "en": "Cakupan jaringan LAN (Local Area Network)?", "id": "Area lokal seperti gedung kantor." },
  { "en": "Kepanjangan WAN (Wide Area Network)?", "id": "Wide Area Network." },
  { "en": "Cakupan jaringan WAN (Wide Area Network)?", "id": "Area luas seperti antar kota." },
  { "en": "Fungsi utama Router?", "id": "Menghubungkan jaringan yang berbeda." },
  { "en": "Fungsi utama Switch?", "id": "Menghubungkan perangkat dalam satu LAN." },
  { "en": "Apa itu alamat IP (Internet Protocol)?", "id": "Alamat unik perangkat di jaringan." },
  { "en": "Model referensi jaringan populer?", "id": "OSI dan TCP/IP." },
  { "en": "Berapa lapis model OSI (Open Systems Interconnection)?", "id": "Tujuh lapisan (layers)." },
  { "en": "Lapisan fisik pada OSI (Open Systems Interconnection)?", "id": "Layer 1, Physical Layer." },
  { "en": "Lapisan aplikasi pada OSI (Open Systems Interconnection)?", "id": "Layer 7, Application Layer." },
  { "en": "Fungsi utama Firewall?", "id": "Menyaring lalu lintas jaringan berbahaya." },
  { "en": "Apa itu Malware?", "id": "Perangkat lunak yang merusak sistem." },
  { "en": "Contoh jenis Malware?", "id": "Virus, Worm, Trojan, Ransomware." },
  { "en": "Apa itu Phishing?", "id": "Penipuan untuk mencuri data sensitif." },
  { "en": "Media umum untuk Phishing?", "id": "Email, SMS, dan situs web palsu." },
  { "en": "Fungsi utama Antivirus?", "id": "Mendeteksi dan menghapus perangkat lunak jahat." },
  { "en": "Tiga pilar keamanan informasi (CIA)?", "id": "Confidentiality, Integrity, Availability (CIA)." },
  { "en": "Arti pilar 'Confidentiality'?", "id": "Menjaga kerahasiaan data." },
  { "en": "Arti pilar 'Integrity'?", "id": "Memastikan data tidak diubah." },
  { "en": "Arti pilar 'Availability'?", "id": "Memastikan sistem selalu tersedia." },
  { "en": "Kepanjangan TCP (Transmission Control Protocol)?", "id": "Transmission Control Protocol." },
  { "en": "Sifat protokol TCP (Transmission Control Protocol)?", "id": "Connection-oriented, andal." },
  { "en": "Kepanjangan UDP (User Datagram Protocol)?", "id": "User Datagram Protocol." },
  { "en": "Sifat protokol UDP (User Datagram Protocol)?", "id": "Connectionless, cepat, tidak andal." },
  { "en": "Kepanjangan DNS (Domain Name System)?", "id": "Domain Name System." },
  { "en": "Fungsi utama DNS (Domain Name System)?", "id": "Menerjemahkan nama domain ke alamat IP." },
  { "en": "Kepanjangan HTTP (Hypertext Transfer Protocol)?", "id": "Hypertext Transfer Protocol." },
  { "en": "Versi aman dari HTTP (Hypertext Transfer Protocol)?", "id": "HTTPS (Hypertext Transfer Protocol Secure)." },
  { "en": "Kepanjangan VPN (Virtual Private Network)?", "id": "Virtual Private Network." },
  { "en": "Fungsi utama VPN (Virtual Private Network)?", "id": "Membuat koneksi aman melalui internet." },
  { "en": "Apa itu Enkripsi?", "id": "Proses mengacak data agar aman." },
  { "en": "Proses kebalikan dari enkripsi?", "id": "Dekripsi." },
  { "en": "Alamat fisik sebuah perangkat?", "id": "MAC Address." },
  { "en": "Perangkat pemancar sinyal Wi-Fi?", "id": "Access Point (AP)." },
  { "en": "Standar keamanan Wi-Fi modern?", "id": "WPA3." },
  { "en": "Komputer penyedia layanan?", "id": "Server." },
  { "en": "Komputer pengguna layanan?", "id": "Client." },
  { "en": "Kepanjangan FTP (File Transfer Protocol)?", "id": "File Transfer Protocol." },
  { "en": "Fungsi utama FTP (File Transfer Protocol)?", "id": "Untuk transfer file antar komputer." },
  { "en": "Unit data dalam jaringan?", "id": "Packet atau datagram." },
  { "en": "Kapasitas transfer data?", "id": "Bandwidth." },
  { "en": "Waktu tunda pengiriman data?", "id": "Latency atau ping." },
  { "en": "Kepanjangan DHCP (Dynamic Host Configuration Protocol)?", "id": "Dynamic Host Configuration Protocol." },
  { "en": "Fungsi utama DHCP (Dynamic Host Configuration Protocol)?", "id": "Memberi IP address secara otomatis." },
  { "en": "Gerbang keluar jaringan lokal?", "id": "Gateway." },
  { "en": "Apa itu 'Subnet Mask'?", "id": "Pembeda network dan host ID." },
  { "en": "Topologi jaringan bentuk bintang?", "id": "Topologi Star." },
  { "en": "Topologi jaringan bentuk cincin?", "id": "Topologi Ring." },
  { "en": "Kejahatan di dunia maya?", "id": "Cybercrime." },
  { "en": "Unit siber di Polri?", "id": "Direktorat Tindak Pidana Siber." },
  { "en": "Investigasi barang bukti digital?", "id": "Forensik Digital." },
  { "en": "Serangan melumpuhkan server?", "id": "Denial-of-Service (DoS) attack." },
  { "en": "Serangan DoS (Denial-of-Service) dari banyak sumber?", "id": "DDoS (Distributed Denial-of-Service)." },
  { "en": "Pintu virtual layanan jaringan?", "id": "Port." },
  { "en": "Nomor port untuk web server?", "id": "Port 80 (HTTP), 443 (HTTPS)." },
  { "en": "Celah keamanan pada sistem?", "id": "Vulnerability." },
  { "en": "Kode yang manfaatkan 'vulnerability'?", "id": "Exploit." },
  { "en": "Serangan pada celah belum diketahui?", "id": "Zero-day attack." },
  { "en": "Proses verifikasi identitas pengguna?", "id": "Autentikasi (Authentication)." },
  { "en": "Proses pemberian hak akses?", "id": "Otorisasi (Authorization)." },
  { "en": "Malware yang memata-matai pengguna?", "id": "Spyware." },
  { "en": "Malware yang menyandera data?", "id": "Ransomware." },
  { "en": "Malware berkedok program lain?", "id": "Trojan Horse." },
  { "en": "Malware yang menyebar sendiri?", "id": "Worm." },
  { "en": "Manipulasi psikologis dapatkan data?", "id": "Social Engineering." },
  { "en": "Serangan coba semua kata sandi?", "id": "Brute-force attack." },
  { "en": "Kepanjangan IDS (Intrusion Detection System)?", "id": "Intrusion Detection System." },
  { "en": "Fungsi IDS (Intrusion Detection System)?", "id": "Mendeteksi adanya penyusup." },
  { "en": "Kepanjangan IPS (Intrusion Prevention System)?", "id": "Intrusion Prevention System." },
  { "en": "Fungsi IPS (Intrusion Prevention System)?", "id": "Mendeteksi dan mencegah penyusup." },
  { "en": "Sistem umpan untuk peretas?", "id": "Honeypot." },
  { "en": "Catatan aktivitas pada sistem?", "id": "Log." },
  { "en": "Proses mencadangkan data?", "id": "Backup." },
  { "en": "Rencana pemulihan pasca bencana (DRP)?", "id": "Disaster Recovery Plan (DRP)." },
  { "en": "Autentikasi pakai ciri fisik?", "id": "Biometrik." },
  { "en": "Autentikasi dua langkah (2FA)?", "id": "Two-Factor Authentication (2FA)." },
  { "en": "Protokol internet versi 4 (IPv4)?", "id": "IPv4." },
  { "en": "Protokol internet versi 6 (IPv6)?", "id": "IPv6." },
  { "en": "Kelebihan utama IPv6 (Internet Protocol version 6)?", "id": "Jumlah alamat jauh lebih banyak." },
  { "en": "Perintah tes koneksi?", "id": "Ping." },
  { "en": "Perintah lacak rute paket?", "id": "Traceroute." },
  { "en": "Server perantara koneksi internet?", "id": "Proxy Server." },
  { "en": "Nama jaringan Wi-Fi (SSID)?", "id": "SSID (Service Set Identifier)." },
  { "en": "Menyembunyikan nama jaringan Wi-Fi (SSID)?", "id": "Hiding SSID." },
  { "en": "Filter akses berdasar MAC address?", "id": "MAC Filtering." },
  { "en": "Jaringan komputer yang terinfeksi?", "id": "Botnet." },
  { "en": "Insiden kebocoran data sensitif?", "id": "Data Breach." },
  { "en": "Media transmisi jaringan fisik?", "id": "Kabel Tembaga, Fiber Optik." },
  { "en": "Media transmisi non-fisik?", "id": "Gelombang radio (nirkabel)." },
  { "en": "Kelebihan kabel Fiber Optik?", "id": "Sangat cepat, tahan interferensi." },
  { "en": "Perangkat yang menghubungkan komputer?", "id": "Hub atau Switch." },
  { "en": "Perangkat usang pengganti Switch?", "id": "Hub." },
  { "en": "Perangkat modulasi sinyal internet?", "id": "Modem." },
  { "en": "Fungsi zona DMZ (Demilitarized Zone)?", "id": "Area isolasi untuk server publik." },
  { "en": "Kepanjangan SIEM (Security Information and Event Management)?", "id": "Security Information and Event Management." },
  { "en": "Tujuan utama sistem SIEM (Security Information and Event Management)?", "id": "Analisa log keamanan secara terpusat." },
  { "en": "Apa itu enkripsi simetris?", "id": "Satu kunci untuk enkripsi dan dekripsi." },
  { "en": "Apa itu enkripsi asimetris?", "id": "Dua kunci: publik dan privat." },
  { "en": "Contoh algoritma simetris?", "id": "AES (Advanced Encryption Standard)." },
  { "en": "Contoh algoritma asimetris?", "id": "RSA (Rivest-Shamir-Adleman)." },
  { "en": "Apa itu 'hashing'?", "id": "Mengubah data menjadi string unik." },
  { "en": "Fungsi utama 'hashing'?", "id": "Memverifikasi integritas sebuah data." },
  { "en": "Contoh algoritma hash?", "id": "SHA-256, MD5." },
  { "en": "Definisi 'digital signature'?", "id": "Tanda tangan digital untuk otentikasi." },
  { "en": "Kepanjangan PKI (Public Key Infrastructure)?", "id": "Public Key Infrastructure." },
  { "en": "Fungsi utama PKI (Public Key Infrastructure)?", "id": "Mengelola sertifikat dan kunci digital." },
  { "en": "Kepanjangan SSL/TLS?", "id": "Secure Sockets Layer / Transport Layer Security." },
  { "en": "Fungsi SSL/TLS (Secure Sockets Layer / Transport Layer Security)?", "id": "Mengamankan koneksi website (HTTPS)." },
  { "en": "Kepanjangan ARP (Address Resolution Protocol)?", "id": "Address Resolution Protocol." },
  { "en": "Fungsi utama ARP (Address Resolution Protocol)?", "id": "Memetakan alamat IP ke MAC address." },
  { "en": "Kepanjangan ICMP (Internet Control Message Protocol)?", "id": "Internet Control Message Protocol." },
  { "en": "Perintah yang menggunakan ICMP (Internet Control Message Protocol)?", "id": "Ping dan traceroute." },
  { "en": "Protokol email untuk mengirim?", "id": "SMTP (Simple Mail Transfer Protocol)." },
  { "en": "Protokol email untuk menerima?", "id": "POP3 atau IMAP." },
  { "en": "Kepanjangan NTP (Network Time Protocol)?", "id": "Network Time Protocol." },
  { "en": "Fungsi NTP (Network Time Protocol)?", "id": "Sinkronisasi waktu di semua perangkat." },
  { "en": "Kepanjangan SNMP (Simple Network Management Protocol)?", "id": "Simple Network Management Protocol." },
  { "en": "Fungsi SNMP (Simple Network Management Protocol)?", "id": "Memantau dan mengelola perangkat jaringan." },
  { "en": "Kepanjangan VLAN (Virtual Local Area Network)?", "id": "Virtual Local Area Network." },
  { "en": "Fungsi utama VLAN (Virtual Local Area Network)?", "id": "Segmentasi logis dalam jaringan LAN." },
  { "en": "Kepanjangan QoS (Quality of Service)?", "id": "Quality of Service." },
  { "en": "Tujuan QoS (Quality of Service)?", "id": "Memberi prioritas pada trafik penting." },
  { "en": "Contoh trafik prioritas QoS (Quality of Service)?", "id": "Panggilan suara (VoIP), video conference." },
  { "en": "Apa itu 'port security'?", "id": "Membatasi akses pada port switch." },
  { "en": "Kepanjangan Pusinafis?", "id": "Pusat Indonesia Automatic Fingerprint Identification System." },
  { "en": "Tugas utama Pusinafis?", "id": "Mengelola data identifikasi sidik jari." },
  { "en": "Kepanjangan ETLE (Electronic Traffic Law Enforcement)?", "id": "Electronic Traffic Law Enforcement." },
  { "en": "Tantangan keamanan ETLE (Electronic Traffic Law Enforcement)?", "id": "Melindungi data pelanggaran dan privasi." },
  { "en": "Apa itu 'incident response'?", "id": "Prosedur penanganan insiden keamanan siber." },
  { "en": "Tahap pertama 'incident response'?", "id": "Preparation (Persiapan)." },
  { "en": "Tahap terakhir 'incident response'?", "id": "Lessons Learned (Pembelajaran)." },
  { "en": "Apa itu serangan 'MitM' (Man-in-the-Middle)?", "id": "Man-in-the-Middle, penyadapan di tengah." },
  { "en": "Apa itu 'SQL Injection'?", "id": "Menyisipkan perintah SQL jahat." },
  { "en": "Cara cegah 'SQL Injection'?", "id": "Gunakan prepared statements, validasi input." },
  { "en": "Apa itu 'Cross-Site Scripting' (XSS)?", "id": "Menyisipkan script jahat di website." },
  { "en": "Beda DoS (Denial-of-Service) dan DDoS (Distributed Denial-of-Service)?", "id": "DoS satu sumber, DDoS banyak sumber." },
  { "en": "Apa itu 'penetration testing'?", "id": "Uji coba peretasan yang sah." },
  { "en": "Tujuan 'penetration testing'?", "id": "Menemukan celah keamanan sistem." },
  { "en": "Definisi 'red team'?", "id": "Tim yang berperan sebagai penyerang." },
  { "en": "Definisi 'blue team'?", "id": "Tim yang berperan sebagai pihak bertahan." },
  { "en": "Definisi 'purple team'?", "id": "Kolaborasi antara red dan blue team." },
  { "en": "Apa itu 'sandboxing'?", "id": "Menjalankan aplikasi di lingkungan terisolasi." },
  { "en": "Tujuan 'sandboxing'?", "id": "Mencegah program jahat merusak sistem." },
  { "en": "Prinsip keamanan 'least privilege'?", "id": "Beri hak akses seminimal mungkin." },
  { "en": "Strategi keamanan 'defense in depth'?", "id": "Menerapkan keamanan secara berlapis-lapis." },
  { "en": "Apa itu 'threat intelligence'?", "id": "Informasi tentang ancaman siber terkini." },
  { "en": "Kesalahan akibat kelalaian manusia?", "id": "Human error." },
  { "en": "Masuk area aman tanpa izin?", "id": "Tailgating atau piggybacking." },
  { "en": "Mencari info dari tempat sampah?", "id": "Dumpster diving." },
  { "en": "Malware yang bersembunyi di sistem?", "id": "Rootkit." },
  { "en": "Malware yang merekam ketikan keyboard?", "id": "Keylogger." },
  { "en": "Phishing yang menargetkan pejabat?", "id": "Whaling." },
  { "en": "Phishing melalui telepon?", "id": "Vishing (Voice Phishing)." },
  { "en": "Phishing melalui SMS?", "id": "Smishing (SMS Phishing)." },
  { "en": "Kepanjangan WAF (Web Application Firewall)?", "id": "Web Application Firewall." },
  { "en": "Fungsi WAF (Web Application Firewall)?", "id": "Melindungi aplikasi web dari serangan." },
  { "en": "Aktivitas memindai port terbuka?", "id": "Port scanning." },
  { "en": "Tools untuk 'port scanning'?", "id": "Nmap." },
  { "en": "Aktivitas menangkap paket data?", "id": "Packet sniffing." },
  { "en": "Tools untuk 'packet sniffing'?", "id": "Wireshark." },
  { "en": "Serangan mengambil alih sesi?", "id": "Session hijacking." },
  { "en": "Filter untuk memblokir situs?", "id": "URL filtering." },
  { "en": "Batasan akses berdasar geografi?", "id": "Geofencing." },
  { "en": "Daftar aturan izin akses (ACL)?", "id": "Access Control List (ACL)." },
  { "en": "Pusat komando siber Polri?", "id": "Command Center Dittipidsiber." },
  { "en": "Dokumentasi riwayat barang bukti?", "id": "Chain of Custody." },
  { "en": "Pentingnya 'chain of custody'?", "id": "Menjaga keaslian barang bukti." },
  { "en": "Memastikan file tidak berubah?", "id": "Menggunakan file hashing (SHA-256)." },
  { "en": "Alat pencegah penulisan data?", "id": "Write blocker." },
  { "en": "Analisa data dari memori RAM?", "id": "RAM forensics atau memory forensics." },
  { "en": "Seni menyembunyikan pesan?", "id": "Steganografi." },
  { "en": "Pencurian sumber daya untuk 'mining'?", "id": "Cryptojacking." },
  { "en": "Video atau audio manipulasi AI?", "id": "Deepfake." },
  { "en": "Ancaman 'deepfake' bagi Polri?", "id": "Disinformasi dan pencemaran nama baik." },
  { "en": "Pelaku ancaman siber?", "id": "Threat actor." },
  { "en": "Peretasan untuk tujuan baik?", "id": "Ethical hacking." },
  { "en": "Program hadiah penemu 'bug'?", "id": "Bug bounty program." },
  { "en": "Lapisan OSI (Open Systems Interconnection) untuk routing?", "id": "Layer 3, Network Layer." },
  { "en": "Lapisan OSI (Open Systems Interconnection) untuk switch?", "id": "Layer 2, Data Link Layer." },
  { "en": "Lapisan OSI (Open Systems Interconnection) untuk enkripsi?", "id": "Layer 6, Presentation Layer." },
  { "en": "Standar kabel ethernet umum?", "id": "Cat5e, Cat6, Cat7." },
  { "en": "Perangkat jaringan usang?", "id": "Hub dan Repeater." },
  { "en": "Alamat IP (Internet Protocol) kelas A?", "id": "Range 1.0.0.0 hingga 126.0.0.0." },
  { "en": "Alamat IP (Internet Protocol) kelas B?", "id": "Range 128.0.0.0 hingga 191.255.0.0." },
  { "en": "Alamat IP (Internet Protocol) kelas C?", "id": "Range 192.0.0.0 hingga 223.255.255.0." },
  { "en": "Alamat IP (Internet Protocol) untuk 'localhost'?", "id": "127.0.0.1." },
  { "en": "Apa itu 'subnetting'?", "id": "Membagi jaringan besar menjadi kecil." },
  { "en": "Manfaat 'subnetting'?", "id": "Efisiensi alamat IP, mengurangi trafik." },
  { "en": "Apa itu 'APIPA' (Automatic Private IP Addressing)?", "id": "Alamat IP otomatis jika DHCP gagal." },
  { "en": "Range alamat APIPA (Automatic Private IP Addressing)?", "id": "169.254.0.0 hingga 169.254.255.255." },
  { "en": "Tujuan kebijakan medsos anggota?", "id": "Jaga citra institusi, cegah kebocoran." },
  { "en": "Definisi kebijakan BYOD (Bring Your Own Device)?", "id": "Bring Your Own Device." },
  { "en": "Risiko kebijakan BYOD (Bring Your Own Device)?", "id": "Keamanan data institusi lebih rentan." },
  { "en": "Contoh klasifikasi data Polri?", "id": "Sangat Rahasia, Rahasia, Terbatas." },
  { "en": "Tujuan audit keamanan sistem?", "id": "Mengevaluasi efektivitas kontrol keamanan." },
  { "en": "Definisi 'risk management'?", "id": "Proses mengelola risiko keamanan informasi." },
  { "en": "Apa itu forensik jaringan?", "id": "Proses investigasi pada lalu lintas jaringan." },
  { "en": "Apa itu forensik seluler?", "id": "Proses investigasi pada perangkat mobile." },
  { "en": "Tantangan forensik seluler?", "id": "Enkripsi, beragam OS, kerusakan fisik." },
  { "en": "Apa itu forensik cloud?", "id": "Investigasi data di lingkungan cloud." },
  { "en": "Tantangan forensik cloud?", "id": "Masalah yurisdiksi dan akses data." },
  { "en": "Apa itu 'static analysis' malware?", "id": "Analisa kode tanpa menjalankan program." },
  { "en": "Apa itu 'dynamic analysis' malware?", "id": "Analisa perilaku saat program dijalankan." },
  { "en": "Tujuan 'imaging' barang bukti?", "id": "Membuat salinan bit-per-bit identik." },
  { "en": "Kenapa bekerja pada salinan/image?", "id": "Agar barang bukti asli tidak berubah." },
  { "en": "Fungsi kontrol akses biometrik?", "id": "Batasi akses ruang server via sidik jari." },
  { "en": "Sistem pemadam api server?", "id": "Gas bersih (clean agent) seperti FM-200." },
  { "en": "Kenapa server tak pakai air?", "id": "Air akan merusak semua perangkat elektronik." },
  { "en": "Fungsi UPS (Uninterruptible Power Supply) di pusat data?", "id": "Catu daya darurat sementara." },
  { "en": "Fungsi genset di pusat data?", "id": "Catu daya cadangan jangka panjang." },
  { "en": "Tugas utama patroli siber?", "id": "Memonitor konten negatif di internet." },
  { "en": "Tujuan 'cyber counter-intelligence'?", "id": "Melawan spionase siber dari lawan." },
  { "en": "Tugas tim 'online undercover'?", "id": "Menyusup ke dalam grup kriminal online." },
  { "en": "Apa itu 'social media analytics'?", "id": "Analisa tren dan sentimen publik." },
  { "en": "Dasar hukum kejahatan siber?", "id": "Undang-Undang ITE." },
  { "en": "Syarat penyadapan yang legal?", "id": "Harus ada izin dari pengadilan." },
  { "en": "Landasan hukum perlindungan data?", "id": "Undang-Undang Perlindungan Data Pribadi (PDP)." },
  { "en": "Contoh kerja siber internasional?", "id": "Dengan Interpol, FBI, atau polisi lain." },
  { "en": "Definisi 'acceptable use policy' (AUP)?", "id": "Aturan penggunaan aset IT institusi." },
  { "en": "Konsekuensi pelanggaran AUP (Acceptable Use Policy)?", "id": "Sanksi disiplin hingga pemecatan." },
  { "en": "Apa itu 'data retention policy'?", "id": "Kebijakan berapa lama data disimpan." },
  { "en": "Apa itu 'data disposal policy'?", "id": "Prosedur pemusnahan data secara aman." },
  { "en": "Metode pemusnahan hard disk?", "id": "Degaussing atau dihancurkan secara fisik." },
  { "en": "Prinsip 'need-to-know basis'?", "id": "Akses data hanya jika benar-benar perlu." },
  { "en": "Apa itu 'separation of duties'?", "id": "Pemisahan tugas untuk cegah penyalahgunaan." },
  { "en": "Apa itu 'security awareness training'?", "id": "Pelatihan kesadaran keamanan bagi personel." },
  { "en": "Topik utama 'security awareness'?", "id": "Phishing, password kuat, keamanan fisik." },
  { "en": "Definisi 'insider threat'?", "id": "Ancaman keamanan dari dalam institusi." },
  { "en": "Motif 'insider threat'?", "id": "Kecewa, sabotase, atau keuntungan finansial." },
  { "en": "Apa itu 'system hardening'?", "id": "Proses mengurangi permukaan serangan sistem." },
  { "en": "Contoh 'system hardening'?", "id": "Matikan port, uninstall software tak perlu." },
  { "en": "Apa itu 'network segmentation'?", "id": "Memecah jaringan besar menjadi kecil." },
  { "en": "Tujuan 'network segmentation'?", "id": "Membatasi penyebaran serangan jika terjadi." },
  { "en": "Definisi 'zero trust architecture'?", "id": "Jangan percaya siapapun, selalu verifikasi." },
  { "en": "Prinsip utama 'zero trust'?", "id": "Never trust, always verify." },
  { "en": "Apa itu 'mobile device management' (MDM)?", "id": "Manajemen keamanan untuk perangkat mobile." },
  { "en": "Fungsi MDM (Mobile Device Management) Polri?", "id": "Kontrol aplikasi, hapus data jarak jauh." },
  { "en": "Apa itu 'information sharing center'?", "id": "Pusat berbagi informasi ancaman siber." },
  { "en": "Definisi 'dark web'?", "id": "Bagian internet butuh software khusus." },
  { "en": "Aktivitas ilegal di 'dark web'?", "id": "Jual beli narkoba, data curian." },
  { "en": "Tujuan patroli 'dark web'?", "id": "Mengungkap jaringan kejahatan terorganisir." },
  { "en": "Apa itu 'cryptocurrency'?", "id": "Mata uang digital terdesentralisasi." },
  { "en": "Tantangan 'cryptocurrency' bagi Polri?", "id": "Sulit dilacak untuk pencucian uang." },
  { "en": "Apa itu 'cryptocurrency mixing'?", "id": "Layanan untuk mengaburkan jejak transaksi." },
  { "en": "Definisi OSINT (Open-Source Intelligence)?", "id": "Open-Source Intelligence." },
  { "en": "Sumber data OSINT (Open-Source Intelligence)?", "id": "Media sosial, berita, situs publik." },
  { "en": "Apa itu 'SOC' (Security Operations Center)?", "id": "Security Operations Center." },
  { "en": "Tugas utama analis SOC (Security Operations Center)?", "id": "Memantau, mendeteksi, merespon ancaman." },
  { "en": "Apa itu 'threat hunting'?", "id": "Proaktif mencari ancaman dalam jaringan." },
  { "en": "Beda 'threat hunting' & SOC (Security Operations Center)?", "id": "Hunting proaktif, SOC umumnya reaktif." },
  { "en": "Apa itu 'indicators of compromise' (IOC)?", "id": "Tanda-tanda sistem telah disusupi." },
  { "en": "Contoh IOC (Indicators of Compromise)?", "id": "Alamat IP jahat, hash file malware." },
  { "en": "Apa itu 'indicators of attack'?", "id": "Tanda-tanda sebuah serangan sedang berlangsung." },
  { "en": "Apa itu 'attack vector'?", "id": "Jalur atau cara serangan dilakukan." },
  { "en": "Contoh 'attack vector'?", "id": "Email phishing, celah keamanan software." },
  { "en": "Apa itu 'attack surface'?", "id": "Total area sistem yang bisa diserang." },
  { "en": "Apa itu 'lateral movement'?", "id": "Pergerakan peretas di dalam jaringan." },
  { "en": "Tujuan 'lateral movement'?", "id": "Mencari data berharga, memperluas akses." },
  { "en": "Apa itu 'privilege escalation'?", "id": "Meningkatkan hak akses dari biasa ke admin." },
  { "en": "Apa itu 'data exfiltration'?", "id": "Proses pencurian dan pengiriman data keluar." },
  { "en": "Apa itu 'command and control' server?", "id": "Server pengendali malware atau botnet." },
  { "en": "Apa itu 'firewall rule'?", "id": "Aturan spesifik untuk blokir/izinkan trafik." },
  { "en": "Aturan implisit terakhir firewall?", "id": "Implicit Deny (Tolak Semua)." },
  { "en": "Apa itu 'stateless firewall'?", "id": "Memeriksa paket secara individual." },
  { "en": "Apa itu 'stateful firewall'?", "id": "Memonitor status koneksi jaringan." },
  { "en": "Mana yang lebih aman?", "id": "Stateful firewall." },
  { "en": "Apa itu 'next-generation firewall' (NGFW)?", "id": "Firewall dengan fitur lebih canggih." },
  { "en": "Fitur NGFW (Next-Generation Firewall)?", "id": "IPS, kontrol aplikasi, inspeksi SSL." },
  { "en": "Apa itu 'virtual security appliance'?", "id": "Perangkat keamanan dalam bentuk software." },
  { "en": "Apa itu 'security baseline'?", "id": "Standar konfigurasi keamanan minimum." },
  { "en": "Tujuan 'security baseline'?", "id": "Memastikan semua sistem punya dasar aman." },
  { "en": "Apa itu 'change management'?", "id": "Prosedur formal untuk perubahan sistem." },
  { "en": "Tujuan 'change management'?", "id": "Mencegah error akibat perubahan sembarangan." },
  { "en": "Apa itu 'digital watermark'?", "id": "Tanda digital tersembunyi pada file." },
  { "en": "Fungsi 'digital watermark'?", "id": "Membuktikan kepemilikan, melacak kebocoran." },
  { "en": "Apa itu 'data loss prevention' (DLP)?", "id": "Sistem pencegah kebocoran data." },
  { "en": "Cara kerja DLP (Data Loss Prevention)?", "id": "Memantau, mendeteksi, memblokir transfer data." },
  { "en": "Peran 'forensic image' di pengadilan?", "id": "Sebagai duplikat sah barang bukti." },
  { "en": "Apa itu 'time stamping'?", "id": "Memberi cap waktu pada data/log." },
  { "en": "Pentingnya 'time stamping'?", "id": "Membuktikan waktu kejadian secara akurat." },
  { "en": "Apa itu 'RAID' (Redundant Array of Independent Disks)?", "id": "Redundant Array of Independent Disks." },
  { "en": "Fungsi 'RAID' di server?", "id": "Meningkatkan performa dan redundansi data." },
  { "en": "Kepanjangan ERI (Electronic Registration and Identification) Polri?", "id": "Electronic Registration and Identification." },
  { "en": "Fungsi utama sistem ERI (Electronic Registration and Identification)?", "id": "Registrasi dan identifikasi kendaraan bermotor." },
  { "en": "Jaringan pada sistem Mambis?", "id": "Koneksi aman ke database biometrik." },
  { "en": "Keamanan aplikasi Polri Super App?", "id": "Enkripsi data, autentikasi pengguna." },
  { "en": "Beda utama hashing dan enkripsi?", "id": "Enkripsi bisa dibalik, hashing tidak." },
  { "en": "Kegunaan enkripsi simetris?", "id": "Enkripsi data dalam volume besar." },
  { "en": "Kegunaan enkripsi asimetris?", "id": "Pertukaran kunci aman, tanda tangan digital." },
  { "en": "Beda utama VPN (Virtual Private Network) dan Proxy?", "id": "VPN mengenkripsi semua lalu lintas data." },
  { "en": "Beda virus dan worm?", "id": "Worm menyebar sendiri tanpa program inang." },
  { "en": "Tujuan 'Risk Assessment'?", "id": "Mengidentifikasi dan mengevaluasi risiko keamanan." },
  { "en": "Tujuan 'Business Impact Analysis'?", "id": "Mengukur dampak gangguan pada operasi." },
  { "en": "Apa itu standar ISO 27001?", "id": "Standar manajemen keamanan informasi (ISMS)." },
  { "en": "Prinsip dasar UU PDP (Undang-Undang Perlindungan Data Pribadi)?", "id": "Melindungi hak privasi data pribadi." },
  { "en": "Definisi 'data controller'?", "id": "Pihak yang mengendalikan pemrosesan data." },
  { "en": "Definisi 'data processor'?", "id": "Pihak yang memproses data untuk controller." },
  { "en": "Kepanjangan IaaS (Infrastructure as a Service)?", "id": "Infrastructure as a Service." },
  { "en": "Kepanjangan PaaS (Platform as a Service)?", "id": "Platform as a Service." },
  { "en": "Kepanjangan SaaS (Software as a Service)?", "id": "Software as a Service." },
  { "en": "Contoh layanan SaaS (Software as a Service)?", "id": "Google Mail, Microsoft 365." },
  { "en": "Risiko keamanan utama pada cloud?", "id": "Konfigurasi salah, akses tidak sah." },
  { "en": "Keamanan pada kontainer Docker?", "id": "Image scanning, network policies." },
  { "en": "Definisi 'Virtual Desktop Infrastructure' (VDI)?", "id": "Menjalankan desktop di server pusat." },
  { "en": "Kepanjangan VDI (Virtual Desktop Infrastructure)?", "id": "Virtual Desktop Infrastructure." },
  { "en": "Apa itu hypervisor?", "id": "Software untuk membuat mesin virtual." },
  { "en": "Beda hypervisor tipe 1 & 2?", "id": "Tipe 1 di hardware, Tipe 2 di OS." },
  { "en": "Risiko keamanan utama IoT (Internet of Things)?", "id": "Perangkat lemah, mudah diretas." },
  { "en": "Keamanan pada 'Body Worn Camera'?", "id": "Enkripsi rekaman, kontrol akses aman." },
  { "en": "Definisi 'memory forensics'?", "id": "Analisa bukti digital dari memori RAM." },
  { "en": "Apa itu 'live forensics'?", "id": "Analisa pada sistem yang sedang berjalan." },
  { "en": "Definisi 'cyber kill chain'?", "id": "Tahapan-tahapan dalam sebuah serangan siber." },
  { "en": "Tahap pertama 'kill chain'?", "id": "Reconnaissance (Pengintaian)." },
  { "en": "Tahap terakhir 'kill chain'?", "id": "Actions on Objectives (Tujuan akhir)." },
  { "en": "Definisi 'honeypot' tingkat lanjut?", "id": "Sistem umpan untuk studi metode peretas." },
  { "en": "Apa itu 'honeynet'?", "id": "Jaringan berisi beberapa sistem honeypot." },
  { "en": "Definisi 'sinkhole' DNS (Domain Name System)?", "id": "Mengarahkan trafik jahat ke server aman." },
  { "en": "Apa itu 'domain generation algorithm' (DGA)?", "id": "Malware membuat nama domain acak." },
  { "en": "Tujuan DGA (Domain Generation Algorithm) pada malware?", "id": "Menghindari blokir server C2-nya." },
  { "en": "Apa itu 'fast flux' DNS?", "id": "Teknik ganti alamat IP dengan cepat." },
  { "en": "Tujuan 'fast flux'?", "id": "Menyembunyikan lokasi server sebenarnya." },
  { "en": "Apa itu 'header' email?", "id": "Informasi teknis pengiriman sebuah email." },
  { "en": "Fungsi analisa 'header' email?", "id": "Melacak asal-usul email phishing." },
  { "en": "Apa itu 'SPF' record?", "id": "Mencegah pemalsuan alamat email pengirim." },
  { "en": "Kepanjangan SPF (Sender Policy Framework)?", "id": "Sender Policy Framework." },
  { "en": "Apa itu 'DKIM' (DomainKeys Identified Mail)?", "id": "Menambahkan tanda tangan digital ke email." },
  { "en": "Kepanjangan DKIM (DomainKeys Identified Mail)?", "id": "DomainKeys Identified Mail." },
  { "en": "Apa itu 'DMARC' (Domain-based Message Authentication, Reporting, and Conformance)?", "id": "Kebijakan untuk autentikasi email." },
  { "en": "Fungsi DMARC (Domain-based Message Authentication, Reporting, and Conformance)?", "id": "Melindungi dari spoofing dan phishing email." },
  { "en": "Apa itu 'file carving'?", "id": "Memulihkan file dari data mentah." },
  { "en": "Apa itu 'slack space'?", "id": "Area kosong di akhir sebuah file." },
  { "en": "Fungsi analisa 'slack space'?", "id": "Menemukan data tersembunyi yang dihapus." },
  { "en": "Apa itu 'metadata' file?", "id": "Informasi tentang sebuah file." },
  { "en": "Contoh metadata?", "id": "Tanggal dibuat, tipe kamera, lokasi." },
  { "en": "Tools untuk ekstraksi metadata?", "id": "ExifTool." },
  { "en": "Apa itu 'load balancer'?", "id": "Membagi beban trafik ke beberapa server." },
  { "en": "Manfaat 'load balancer'?", "id": "Meningkatkan ketersediaan dan performa." },
  { "en": "Apa itu 'failover'?", "id": "Sistem cadangan mengambil alih otomatis." },
  { "en": "Apa itu 'high availability'?", "id": "Sistem dirancang minim waktu henti." },
  { "en": "Definisi 'single point of failure' (SPOF)?", "id": "Satu komponen gagal, sistem mati." },
  { "en": "Cara hindari 'SPOF' (Single Point of Failure)?", "id": "Gunakan redundansi dan sistem failover." },
  { "en": "Apa itu 'API' (Application Programming Interface)?", "id": "Application Programming Interface." },
  { "en": "Risiko keamanan API (Application Programming Interface)?", "id": "Akses tidak sah, kebocoran data." },
  { "en": "Apa itu 'rate limiting' API?", "id": "Membatasi jumlah permintaan ke API." },
  { "en": "Tujuan 'rate limiting'?", "id": "Mencegah serangan Denial-of-Service." },
  { "en": "Apa itu 'containerization'?", "id": "Mengemas aplikasi dalam lingkungan terisolasi." },
  { "en": "Contoh platform 'containerization'?", "id": "Docker, Podman." },
  { "en": "Apa itu 'orchestration'?", "id": "Manajemen otomatis untuk banyak kontainer." },
  { "en": "Contoh platform 'orchestration'?", "id": "Kubernetes, Docker Swarm." },
  { "en": "Apa itu 'Infrastructure as Code' (IaC)?", "id": "Mengelola infrastruktur melalui kode." },
  { "en": "Manfaat 'IaC' (Infrastructure as Code)?", "id": "Otomatisasi, konsistensi, pemulihan cepat." },
  { "en": "Apa itu 'DevSecOps'?", "id": "Mengintegrasikan keamanan dalam siklus DevOps." },
  { "en": "Prinsip 'DevSecOps'?", "id": "Keamanan adalah tanggung jawab semua." },
  { "en": "Apa itu 'supply chain attack'?", "id": "Menyerang target melalui software pihak ketiga." },
  { "en": "Contoh 'supply chain attack'?", "id": "Menyisipkan malware di library software." },
  { "en": "Apa itu 'disinformation'?", "id": "Penyebaran informasi palsu secara sengaja." },
  { "en": "Apa itu 'malinformation'?", "id": "Penyebaran informasi asli untuk merugikan." },
  { "en": "Tujuan 'digital attribution'?", "id": "Mengidentifikasi pelaku di balik serangan." },
  { "en": "Tantangan 'digital attribution'?", "id": "Pelaku sering menggunakan anonimitas." },
  { "en": "Apa itu 'clean desk policy'?", "id": "Kebijakan meja kerja yang bersih." },
  { "en": "Tujuan 'clean desk policy'?", "id": "Mencegah pencurian informasi fisik." },
  { "en": "Apa itu 'shoulder surfing'?", "id": "Mengintip layar untuk mencuri informasi." },
  { "en": "Cara cegah 'shoulder surfing'?", "id": "Gunakan privacy screen, waspada sekitar." },
  { "en": "Apa itu 'BIOS/UEFI password'?", "id": "Password untuk akses level firmware." },
  { "en": "Apa itu 'full disk encryption'?", "id": "Mengenkripsi seluruh isi hard drive." },
  { "en": "Contoh 'full disk encryption'?", "id": "BitLocker (Windows), FileVault (Mac)." },
  { "en": "Apa itu 'certificate authority' (CA)?", "id": "Entitas yang menerbitkan sertifikat digital." },
  { "en": "Fungsi 'Certificate Authority' (CA)?", "id": "Memverifikasi identitas pemilik sertifikat." },
  { "en": "Apa itu 'root certificate'?", "id": "Sertifikat digital paling terpercaya." },
  { "en": "Apa itu 'certificate pinning'?", "id": "Mengasosiasikan host dengan sertifikatnya." },
  { "en": "Tujuan 'certificate pinning'?", "id": "Mencegah serangan Man-in-the-Middle." },
  { "en": "Apa itu 'HTTP Strict Transport Security' (HSTS)?", "id": "Memaksa browser hanya gunakan HTTPS." },
  { "en": "Kepanjangan HSTS (HTTP Strict Transport Security)?", "id": "HTTP Strict Transport Security." },
  { "en": "Apa itu 'Content Security Policy' (CSP)?", "id": "Mencegah serangan XSS dan injeksi." },
  { "en": "Kepanjangan CSP (Content Security Policy)?", "id": "Content Security Policy." },
  { "en": "Lima fungsi kerangka NIST (National Institute of Standards and Technology)?", "id": "Identify, Protect, Detect, Respond, Recover." },
  { "en": "Tujuan kerangka NIST (National Institute of Standards and Technology)?", "id": "Membantu organisasi mengelola risiko siber." },
  { "en": "Apa itu MITRE ATT&CK?", "id": "Database taktik dan teknik peretas." },
  { "en": "Tujuan MITRE ATT&CK?", "id": "Memahami dan melawan metode penyerang." },
  { "en": "Definisi 'Cyber Resilience'?", "id": "Kemampuan sistem untuk pulih dari serangan." },
  { "en": "Apa itu kriptografi eliptik (ECC)?", "id": "Enkripsi kuat dengan kunci lebih pendek." },
  { "en": "Kepanjangan ECC (Elliptic Curve Cryptography)?", "id": "Elliptic Curve Cryptography." },
  { "en": "Konsep 'quantum cryptography'?", "id": "Keamanan berbasis prinsip fisika kuantum." },
  { "en": "Ancaman komputer kuantum?", "id": "Dapat memecahkan enkripsi klasik." },
  { "en": "Apa itu 'Perfect Forward Secrecy' (PFS)?", "id": "Kunci sesi unik setiap koneksi." },
  { "en": "Tujuan PFS (Perfect Forward Secrecy)?", "id": "Melindungi sesi lampau jika kunci bocor." },
  { "en": "Apa itu 'salted hash'?", "id": "Menambahkan data acak sebelum hashing." },
  { "en": "Tujuan 'salting'?", "id": "Mencegah serangan 'rainbow table'." },
  { "en": "Kepanjangan SSO (Single Sign-On)?", "id": "Single Sign-On." },
  { "en": "Keuntungan SSO (Single Sign-On)?", "id": "Satu kali login untuk banyak aplikasi." },
  { "en": "Apa itu 'Federated Identity'?", "id": "Mempercayai identitas dari domain lain." },
  { "en": "Kepanjangan FIM (Federated Identity Management)?", "id": "Federated Identity Management." },
  { "en": "Definisi RBAC (Role-Based Access Control)?", "id": "Akses berdasarkan peran atau jabatan." },
  { "en": "Definisi ABAC (Attribute-Based Access Control)?", "id": "Akses berdasarkan atribut pengguna/sumber daya." },
  { "en": "Kepanjangan PAM (Privileged Access Management)?", "id": "Privileged Access Management." },
  { "en": "Tujuan PAM (Privileged Access Management)?", "id": "Mengelola dan mengawasi akun berhak istimewa." },
  { "en": "Apa itu 'reverse engineering' malware?", "id": "Membongkar kode untuk memahami malware." },
  { "en": "Apa itu 'packer' malware?", "id": "Alat untuk mengompres dan menyamarkan malware." },
  { "en": "Apa itu 'obfuscation'?", "id": "Membuat kode sulit dibaca manusia." },
  { "en": "Tujuan 'obfuscation' malware?", "id": "Menghindari deteksi oleh antivirus." },
  { "en": "Fungsi YARA rules?", "id": "Pola untuk mengidentifikasi file malware." },
  { "en": "SOP (Standard Operating Procedure) keamanan Command Center?", "id": "Akses terbatas, monitor fisik, log ketat." },
  { "en": "Prosedur hapus data BWC (Body Worn Camera)?", "id": "Sesuai kebijakan retensi, penghapusan aman." },
  { "en": "Tantangan yurisdiksi siber?", "id": "Pelaku dan korban beda negara." },
  { "en": "Apa itu 'Mutual Legal Assistance'?", "id": "Perjanjian bantuan hukum antar negara." },
  { "en": "Risiko keamanan pada AI/ML?", "id": "Data poisoning, adversarial attacks." },
  { "en": "Apa itu 'data poisoning'?", "id": "Merusak data latih model AI." },
  { "en": "Apa itu 'adversarial attack' AI?", "id": "Input dirancang untuk menipu model AI." },
  { "en": "Keamanan pada drone Polri?", "id": "Enkripsi sinyal kendali dan video." },
  { "en": "Apa itu 'zero-day broker'?", "id": "Pihak yang menjual celah zero-day." },
  { "en": "Apa itu 'credential stuffing'?", "id": "Mencoba kredensial bocor di banyak situs." },
  { "en": "Beda 'credential stuffing' & 'brute-force'?", "id": "Stuffing coba data bocor, brute-force acak." },
  { "en": "Apa itu 'password spraying'?", "id": "Mencoba satu password ke banyak akun." },
  { "en": "Apa itu 'golden ticket attack'?", "id": "Serangan pada Microsoft Active Directory." },
  { "en": "Apa itu 'pass the hash'?", "id": "Menggunakan hash password untuk autentikasi." },
  { "en": "Apa itu 'living off the land'?", "id": "Menyerang pakai tools bawaan sistem." },
  { "en": "Tujuan 'living off the land'?", "id": "Menghindari deteksi, tampak aktivitas normal." },
  { "en": "Apa itu 'fileless malware'?", "id": "Malware yang beroperasi di memori (RAM)." },
  { "en": "Tantangan 'fileless malware'?", "id": "Sangat sulit dideteksi antivirus tradisional." },
  { "en": "Apa itu 'DNS over HTTPS' (DoH)?", "id": "Mengenkripsi kueri DNS via HTTPS." },
  { "en": "Kepanjangan DoH (DNS over HTTPS)?", "id": "DNS over HTTPS." },
  { "en": "Apa itu 'DNS over TLS' (DoT)?", "id": "Mengenkripsi kueri DNS via TLS." },
  { "en": "Kepanjangan DoT (DNS over TLS)?", "id": "DNS over TLS." },
  { "en": "Apa itu 'DNSSEC' (Domain Name System Security Extensions)?", "id": "Memastikan respons DNS asli dan valid." },
  { "en": "Kepanjangan DNSSEC?", "id": "Domain Name System Security Extensions." },
  { "en": "Apa itu 'OS hardening'?", "id": "Proses mengamankan sistem operasi." },
  { "en": "Apa itu 'application whitelisting'?", "id": "Hanya izinkan aplikasi terpercaya berjalan." },
  { "en": "Apa itu 'application blacklisting'?", "id": "Melarang aplikasi berbahaya berjalan." },
  { "en": "Mana lebih aman, whitelisting/blacklisting?", "id": "Whitelisting lebih aman dan ketat." },
  { "en": "Apa itu 'sandnet'?", "id": "Jaringan terisolasi untuk analisis malware." },
  { "en": "Apa itu 'threat modeling'?", "id": "Proses identifikasi ancaman saat desain." },
  { "en": "Tujuan 'threat modeling'?", "id": "Membangun sistem aman sejak awal." },
  { "en": "Apa itu 'Fuzzing'?", "id": "Mengirim data acak untuk cari bug." },
  { "en": "Tujuan 'fuzz testing'?", "id": "Menemukan celah keamanan seperti buffer overflow." },
  { "en": "Apa itu 'buffer overflow'?", "id": "Menulis data melebihi kapasitas buffer." },
  { "en": "Apa itu 'canary value'?", "id": "Teknik deteksi serangan buffer overflow." },
  { "en": "Apa itu 'Address Space Layout Randomization' (ASLR)?", "id": "Mengacak lokasi memori program." },
  { "en": "Kepanjangan ASLR (Address Space Layout Randomization)?", "id": "Address Space Layout Randomization." },
  { "en": "Tujuan ASLR (Address Space Layout Randomization)?", "id": "Mempersulit eksploitasi celah memori." },
  { "en": "Apa itu 'Data Execution Prevention' (DEP)?", "id": "Mencegah eksekusi kode dari area data." },
  { "en": "Kepanjangan DEP (Data Execution Prevention)?", "id": "Data Execution Prevention." },
  { "en": "Apa itu 'tamper-proofing'?", "id": "Membuat sistem sulit untuk dimodifikasi." },
  { "en": "Apa itu 'security by design'?", "id": "Memasukkan keamanan sejak awal pengembangan." },
  { "en": "Apa itu 'security by default'?", "id": "Konfigurasi awal sistem paling aman." },
  { "en": "Definisi 'uptime'?", "id": "Waktu sistem beroperasi tanpa gangguan." },
  { "en": "Definisi 'downtime'?", "id": "Waktu sistem tidak dapat beroperasi." },
  { "en": "Apa itu 'Service Level Agreement' (SLA)?", "id": "Perjanjian tingkat layanan dengan vendor." },
  { "en": "Kepanjangan SLA (Service Level Agreement)?", "id": "Service Level Agreement." },
  { "en": "Contoh isi SLA (Service Level Agreement)?", "id": "Jaminan uptime, waktu respons." },
  { "en": "Apa itu 'cold site'?", "id": "Situs pemulihan bencana tanpa peralatan." },
  { "en": "Apa itu 'warm site'?", "id": "Situs pemulihan dengan sebagian peralatan." },
  { "en": "Apa itu 'hot site'?", "id": "Situs pemulihan bencana siap pakai." },
  { "en": "Mana pemulihan tercepat?", "id": "Hot site." },
  { "en": "Apa itu 'Recovery Time Objective' (RTO)?", "id": "Target waktu maksimal untuk pemulihan." },
  { "en": "Kepanjangan RTO (Recovery Time Objective)?", "id": "Recovery Time Objective." },
  { "en": "Apa itu 'Recovery Point Objective' (RPO)?", "id": "Toleransi kehilangan data maksimal." },
  { "en": "Kepanjangan RPO (Recovery Point Objective)?", "id": "Recovery Point Objective." },
  { "en": "Apa itu 'tabletop exercise'?", "id": "Latihan simulasi penanganan insiden." },
  { "en": "Tujuan 'tabletop exercise'?", "id": "Menguji kesiapan tim dan prosedur." },
  { "en": "Apa itu 'out-of-band management'?", "id": "Manajemen perangkat via jaringan terpisah." },
  { "en": "Tujuan 'out-of-band management'?", "id": "Akses perangkat saat jaringan utama mati." },
  { "en": "Apa itu 'agentless monitoring'?", "id": "Monitoring tanpa instal software di target." },
  { "en": "Apa itu 'agent-based monitoring'?", "id": "Monitoring dengan instal software di target." },
  { "en": "Apa itu 'log normalization'?", "id": "Mengubah format log menjadi standar." },
  { "en": "Apa itu 'log aggregation'?", "id": "Mengumpulkan log dari banyak sumber." },
  { "en": "Apa itu 'log correlation'?", "id": "Menghubungkan event dari log berbeda." },
  { "en": "Tujuan 'log correlation'?", "id": "Mendeteksi pola serangan yang kompleks." },
  { "en": "Beda 'chain of evidence' & 'custody'?", "id": "Evidence alur bukti, custody alur penanganan." },
  { "en": "Syarat bukti digital di pengadilan?", "id": "Asli, otentik, dan tidak berubah." },
  { "en": "Konvensi internasional kejahatan siber?", "id": "The Budapest Convention on Cybercrime." },
  { "en": "Dilema etis 'online undercover'?", "id": "Penipuan demi pengungkapan kejahatan." },
  { "en": "Apa itu 'key lifecycle management'?", "id": "Manajemen siklus hidup kunci kriptografi." },
  { "en": "Tahap 'key lifecycle'?", "id": "Pembuatan, distribusi, penyimpanan, pemusnahan." },
  { "en": "Kepanjangan HSM (Hardware Security Module)?", "id": "Hardware Security Module." },
  { "en": "Fungsi utama HSM (Hardware Security Module)?", "id": "Melindungi dan mengelola kunci digital." },
  { "en": "Apa itu 'Certificate Revocation List' (CRL)?", "id": "Daftar sertifikat digital tidak valid." },
  { "en": "Kepanjangan CRL (Certificate Revocation List)?", "id": "Certificate Revocation List." },
  { "en": "Apa itu OCSP (Online Certificate Status Protocol)?", "id": "Protokol cek status sertifikat realtime." },
  { "en": "Beda CRL (Certificate Revocation List) dan OCSP (Online Certificate Status Protocol)?", "id": "OCSP lebih cepat dan real-time." },
  { "en": "Tantangan forensik sistem SCADA/ICS?", "id": "Sistem lawas, tidak boleh ada downtime." },
  { "en": "Apa itu teknik 'anti-forensics'?", "id": "Upaya menghapus atau menyembunyikan bukti." },
  { "en": "Contoh teknik 'anti-forensics'?", "id": "Data wiping, enkripsi, steganografi." },
  { "en": "Apa itu 'timeline analysis'?", "id": "Menyusun urutan waktu kejadian digital." },
  { "en": "Kenapa 'social engineering' efektif?", "id": "Memanfaatkan kelemahan psikologis manusia." },
  { "en": "Faktor pendorong 'threat actor'?", "id": "Uang, politik, ideologi, atau ego." },
  { "en": "Apa itu 'security culture'?", "id": "Kesadaran keamanan menjadi kebiasaan." },
  { "en": "Respon ransomware dan data breach?", "id": "Isolasi sistem, investigasi, lapor pimpinan." },
  { "en": "SOP (Standard Operating Procedure) perangkat BYOD (Bring Your Own Device) hilang?", "id": "Lapor segera, lakukan remote wipe." },
  { "en": "Beda 'state-sponsored' & 'cybercrime'?", "id": "Motivasi, sumber daya, dan targetnya." },
  { "en": "Apa itu 'write-once media'?", "id": "Media yang hanya bisa ditulis sekali." },
  { "en": "Contoh 'write-once media'?", "id": "CD-R, DVD-R." },
  { "en": "Kegunaannya dalam forensik?", "id": "Menyimpan image bukti agar tidak berubah." },
  { "en": "Apa itu 'order of volatility'?", "id": "Urutan pengumpulan bukti dari paling rentan." },
  { "en": "Contoh data paling 'volatile'?", "id": "Data di RAM dan register CPU." },
  { "en": "Apa itu 'memory dump'?", "id": "Proses menyalin seluruh isi RAM." },
  { "en": "Definisi 'active defense'?", "id": "Tindakan proaktif melawan penyerang." },
  { "en": "Contoh 'active defense'?", "id": "Honeypot, sinkhole, 'hack back'." },
  { "en": "Legalitas 'hack back'?", "id": "Umumnya ilegal, main hakim sendiri." },
  { "en": "Apa itu 'tarpitting'?", "id": "Sengaja memperlambat koneksi penyerang." },
  { "en": "Tujuan 'tarpitting'?", "id": "Menghabiskan waktu dan sumber daya penyerang." },
  { "en": "Apa itu 'polymorphic malware'?", "id": "Malware yang mengubah kodenya sendiri." },
  { "en": "Tujuan 'polymorphic malware'?", "id": "Menghindari deteksi berbasis signature." },
  { "en": "Apa itu 'metamorphic malware'?", "id": "Malware yang menulis ulang dirinya sendiri." },
  { "en": "Apa itu 'endpoint security'?", "id": "Mengamankan perangkat akhir seperti laptop." },
  { "en": "Contoh solusi 'endpoint security'?", "id": "Antivirus, HIPS, EDR." },
  { "en": "Kepanjangan EDR (Endpoint Detection and Response)?", "id": "Endpoint Detection and Response." },
  { "en": "Fungsi EDR (Endpoint Detection and Response)?", "id": "Deteksi dan respon ancaman canggih." },
  { "en": "Apa itu 'User Behavior Analytics' (UBA)?", "id": "Analisa perilaku pengguna deteksi anomali." },
  { "en": "Kepanjangan UBA/UEBA?", "id": "User and Entity Behavior Analytics." },
  { "en": "Apa itu 'security orchestration'?", "id": "Menghubungkan alat keamanan berbeda." },
  { "en": "Kepanjangan SOAR?", "id": "Security Orchestration, Automation, and Response." },
  { "en": "Tujuan SOAR (Security Orchestration, Automation, and Response)?", "id": "Otomatisasi respon insiden keamanan." },
  { "en": "Apa itu 'threat feed'?", "id": "Sumber data intelijen ancaman." },
  { "en": "Apa itu 'black-box testing'?", "id": "Pengujian tanpa pengetahuan internal sistem." },
  { "en": "Apa itu 'white-box testing'?", "id": "Pengujian dengan pengetahuan penuh sistem." },
  { "en": "Apa itu 'gray-box testing'?", "id": "Pengujian dengan pengetahuan sebagian." },
  { "en": "Apa itu 'static application security testing' (SAST)?", "id": "Analisa kode sumber cari kerentanan." },
  { "en": "Kepanjangan SAST?", "id": "Static Application Security Testing." },
  { "en": "Apa itu 'dynamic application security testing' (DAST)?", "id": "Pengujian aplikasi saat berjalan." },
  { "en": "Kepanjangan DAST?", "id": "Dynamic Application Security Testing." },
  { "en": "Apa itu 'interactive application security testing' (IAST)?", "id": "Kombinasi SAST dan DAST." },
  { "en": "Kepanjangan IAST?", "id": "Interactive Application Security Testing." },
  { "en": "Apa itu 'secure code review'?", "id": "Tinjauan manual kode cari kelemahan." },
  { "en": "Apa itu 'mean time to detect' (MTTD)?", "id": "Rata-rata waktu untuk deteksi ancaman." },
  { "en": "Kepanjangan MTTD?", "id": "Mean Time To Detect." },
  { "en": "Apa itu 'mean time to respond' (MTTR)?", "id": "Rata-rata waktu untuk merespon insiden." },
  { "en": "Kepanjangan MTTR?", "id": "Mean Time To Respond." },
  { "en": "Pentingnya menurunkan MTTD/MTTR?", "id": "Meminimalkan dampak kerusakan akibat serangan." },
  { "en": "Apa itu 'data sovereignty'?", "id": "Data tunduk pada hukum negara lokasi." },
  { "en": "Apa itu 'data residency'?", "id": "Lokasi geografis tempat data disimpan." },
  { "en": "Tantangan 'data sovereignty' cloud?", "id": "Data bisa disimpan di negara lain." },
  { "en": "Apa itu 'secure boot'?", "id": "Memastikan hanya OS terpercaya yang berjalan." },
  { "en": "Apa itu 'trusted platform module' (TPM)?", "id": "Chip perangkat keras untuk keamanan." },
  { "en": "Kepanjangan TPM (Trusted Platform Module)?", "id": "Trusted Platform Module." },
  { "en": "Fungsi utama TPM (Trusted Platform Module)?", "id": "Menyimpan kunci enkripsi secara aman." },
  { "en": "Apa itu 'air-gapped network'?", "id": "Jaringan yang terisolasi secara fisik." },
  { "en": "Risiko 'air-gapped network'?", "id": "Bisa ditembus via media fisik (USB)." },
  { "en": "Apa itu 'bastion host'?", "id": "Server khusus sebagai gerbang akses." },
  { "en": "Sinonim 'bastion host'?", "id": "Jump server atau jump host." },
  { "en": "Apa itu 'network access control' (NAC)?", "id": "Mengontrol akses perangkat ke jaringan." },
  { "en": "Kepanjangan NAC (Network Access Control)?", "id": "Network Access Control." },
  { "en": "Cara kerja NAC (Network Access Control)?", "id": "Memeriksa postur keamanan sebelum akses." },
  { "en": "Apa itu 'posture assessment'?", "id": "Pengecekan status keamanan perangkat." },
  { "en": "Contoh 'posture assessment'?", "id": "Cek antivirus, patch, firewall aktif." },
  { "en": "Apa itu 'quarantine network'?", "id": "Jaringan isolasi untuk perangkat tak patuh." },
  { "en": "Apa itu 'deception technology'?", "id": "Teknologi untuk menipu dan mendeteksi penyerang." },
  { "en": "Contoh 'deception technology'?", "id": "Honeypot, honeytoken, honey-credentials." },
  { "en": "Apa itu 'privacy by design'?", "id": "Memasukkan privasi sejak awal pengembangan." },
  { "en": "Prinsip 'privacy by design'?", "id": "Proaktif, default privasi, transparan." },
  { "en": "Apa itu 'data minimization'?", "id": "Mengumpulkan data sesedikit mungkin." },
  { "en": "Apa itu 'purpose limitation'?", "id": "Data hanya dipakai sesuai tujuan." },
  { "en": "Apa itu 'anonymization'?", "id": "Menghapus informasi identitas pribadi." },
  { "en": "Apa itu 'pseudonymization'?", "id": "Mengganti identitas dengan nama samaran." },
  { "en": "Beda 'anonymization' & 'pseudonymization'?", "id": "Pseudonymization masih bisa dikembalikan." },
  { "en": "Apa itu 'right to be forgotten'?", "id": "Hak subjek data meminta penghapusan." },
  { "en": "Tantangan 'right to be forgotten'?", "id": "Menghapus data dari semua backup." },
  { "en": "Apa itu 'data protection officer' (DPO)?", "id": "Pejabat pengawas perlindungan data." },
  { "en": "Kepanjangan DPO (Data Protection Officer)?", "id": "Data Protection Officer." },
  { "en": "Apa itu 'password complexity'?", "id": "Syarat kekuatan sebuah password." },
  { "en": "Contoh 'password complexity'?", "id": "Panjang, huruf besar-kecil, angka, simbol." },
  { "en": "Apa itu 'password history'?", "id": "Melarang penggunaan password lama." },
  { "en": "Apa itu 'password expiration'?", "id": "Masa berlaku sebuah password." },
  { "en": "Apa itu 'account lockout policy'?", "id": "Kebijakan kunci akun setelah gagal login." },
  { "en": "Tujuan 'account lockout'?", "id": "Mencegah serangan brute-force." },
  { "en": "Apa itu 'CAPTCHA'?", "id": "Tes untuk bedakan manusia-bot." },
  { "en": "Apa itu 'code signing'?", "id": "Memberi tanda tangan digital pada software." },
  { "en": "Tujuan 'code signing'?", "id": "Memastikan integritas dan asal software." },
  { "en": "Apa itu 'software composition analysis' (SCA)?", "id": "Analisa komponen pihak ketiga software." },
  { "en": "Tujuan SCA (Software Composition Analysis)?", "id": "Mencari kerentanan di library eksternal." },
  { "en": "Apa itu 'software bill of materials' (SBOM)?", "id": "Daftar semua komponen dalam software." },
  { "en": "Pentingnya SBOM (Software Bill of Materials)?", "id": "Transparansi dan manajemen kerentanan." },
  { "en": "Prinsip 'non-repudiation'?", "id": "Mencegah penyangkalan sebuah aksi." },
  { "en": "Contoh 'non-repudiation'?", "id": "Tanda tangan digital pada transaksi." }



        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
