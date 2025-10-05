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


  { "en": "Apa Partikel Subatomik Bermuatan Negatif?", "id": "Partikel Elektron Yang Bermuatan Negatif." },
  { "en": "Apa Partikel Subatomik Bermuatan Positif?", "id": "Partikel Proton Yang Bermuatan Positif." },
  { "en": "Apa Partikel Subatomik Yang Tidak Bermuatan?", "id": "Partikel Neutron Yang Bersifat Netral." },
  { "en": "Apa Ikatan Atom Dari Berbagi Elektron?", "id": "Ikatan Kovalen, Berbagi Pasangan Elektron." },
  { "en": "Apa Ikatan Akibat Transfer Elektron?", "id": "Ikatan Ionik, Transfer Elektron Valensi." },
  { "en": "Apa Ikatan Antar Atom Logam?", "id": "Ikatan Logam Dengan Lautan Elektron." },
  { "en": "Sebutkan Tiga Jenis Material Dasar Listrik?", "id": "Konduktor, Semikonduktor, Dan Juga Isolator." },
  { "en": "Apa Material Yang Mudah Menghantarkan Listrik?", "id": "Konduktor Memiliki Konduktivitas Sangat Tinggi." },
  { "en": "Apa Material Yang Sulit Menghantarkan Listrik?", "id": "Isolator Memiliki Resistivitas Sangat Tinggi." },
  { "en": "Apa Material Di Antara Konduktor Dan Isolator?", "id": "Semikonduktor, Sifatnya Dapat Diubah." },
  { "en": "Apa Struktur Atom Yang Teratur Dan Berulang?", "id": "Struktur Kristal Yang Sangat Teratur." },
  { "en": "Apa Struktur Atom Yang Tidak Teratur?", "id": "Struktur Amorf Tanpa Keteraturan Jauh." },
  { "en": "Apa Cacat Pada Struktur Kristal Sempurna?", "id": "Dislokasi, Kekosongan, Atau Pengotor." },
  { "en": "Apa Nama Lain Untuk Elektron Terluar?", "id": "Elektron Valensi, Berperan Dalam Ikatan." },
  { "en": "Apa Pita Energi Tempat Elektron Valensi?", "id": "Pita Valensi, Tingkat Energi Tertinggi." },
  { "en": "Apa Pita Energi Yang Elektronnya Bebas Bergerak?", "id": "Pita Konduksi, Elektron Menjadi Konduksi." },
  { "en": "Apa Celah Antara Pita Valensi Dan Konduksi?", "id": "Celah Pita Energi Atau Band Gap." },
  { "en": "Bagaimana Celah Pita Pada Material Konduktor?", "id": "Sangat Kecil Atau Tumpang Tindih." },
  { "en": "Bagaimana Celah Pita Pada Material Isolator?", "id": "Sangat Lebar, Sulit Dieksitasi Elektron." },
  { "en": "Bagaimana Celah Pita Pada Semikonduktor?", "id": "Cukup Kecil Untuk Eksitasi Termal." },
  { "en": "Apa Contoh Material Konduktor Terbaik?", "id": "Perak (Ag), Tembaga (Cu), Emas (Au)." },
  { "en": "Apa Contoh Material Isolator Yang Umum?", "id": "Karet, Kaca, Plastik, Dan Porselen." },
  { "en": "Apa Contoh Material Semikonduktor Murni?", "id": "Silikon (Si) Dan Germanium (Ge)." },
  { "en": "Apa Proses Penambahan Pengotor Pada Semikonduktor?", "id": "Doping Untuk Mengubah Sifat Listrik." },
  { "en": "Semikonduktor Tipe-N Kelebihan Apa?", "id": "Kelebihan Elektron Sebagai Pembawa Muatan." },
  { "en": "Semikonduktor Tipe-P Kelebihan Apa?", "id": "Kelebihan Lubang (Hole) Pembawa Muatan." },
  { "en": "Apa Unsur Doping Untuk Semikonduktor Tipe-N?", "id": "Fosfor (P), Arsen (As), Antimon (Sb)." },
  { "en": "Apa Unsur Doping Untuk Semikonduktor Tipe-P?", "id": "Boron (B), Galium (Ga), Indium (In)." },
  { "en": "Apa Sebutan Untuk Semikonduktor Murni?", "id": "Semikonduktor Intrinsik Tanpa Adanya Doping." },
  { "en": "Apa Sebutan Semikonduktor Setelah Proses Doping?", "id": "Semikonduktor Ekstrinsik Dengan Sifat Baru." },
  { "en": "Apa Satuan Untuk Hambatan Listrik (Resistansi)?", "id": "Ohm, Dengan Simbol Omega (Ω)." },
  { "en": "Apa Satuan Untuk Konduktivitas Listrik?", "id": "Siemens Per Meter Atau (S/m)." },
  { "en": "Apa Satuan Untuk Resistivitas Listrik Material?", "id": "Ohm Meter Atau (Ω·m)." },
  { "en": "Apa Sifat Material Kembali Ke Bentuk Semula?", "id": "Sifat Elastisitas Setelah Beban Dihilangkan." },
  { "en": "Apa Sifat Material Berubah Bentuk Permanen?", "id": "Sifat Plastisitas Atau Deformasi Plastis." },
  { "en": "Apa Kemampuan Material Menahan Beban Tarik?", "id": "Kekuatan Tarik (Tensile Strength) Material." },
  { "en": "Apa Kemampuan Material Menahan Goresan?", "id": "Kekerasan (Hardness) Dari Suatu Material." },
  { "en": "Apa Kemampuan Material Direntangkan Tanpa Putus?", "id": "Daktilitas Atau Sifat Keuletan Material." },
  { "en": "Apa Material Yang Dapat Ditarik Magnet Kuat?", "id": "Material Feromagnetik Seperti Besi, Nikel." },
  { "en": "Apa Material Yang Ditarik Magnet Lemah?", "id": "Material Paramagnetik Seperti Aluminium, Platina." },
  { "en": "Apa Material Yang Ditolak Oleh Medan Magnet?", "id": "Material Diamagnetik Seperti Tembaga, Emas." },
  { "en": "Apa Sisa Magnetisme Setelah Medan Dihilangkan?", "id": "Retentivitas Atau Sisa Medan Magnet." },
  { "en": "Apa Kemampuan Material Menyimpan Energi Listrik?", "id": "Kapasitansi Dalam Medan Listrik Eksternal." },
  { "en": "Apa Material Yang Digunakan Untuk Dielektrik?", "id": "Isolator Listrik Yang Dapat dipolarisasi." },
  { "en": "Apa Konstanta Yang Mengukur Kemampuan Dielektrik?", "id": "Konstanta Dielektrik Atau Permitivitas Relatif." },
  { "en": "Apa Fenomena Timbulnya Tegangan Akibat Tekanan?", "id": "Efek Piezoelektrik Pada Material Tertentu." },
  { "en": "Apa Material Yang Menunjukkan Efek Piezoelektrik?", "id": "Kuarsa, Turmalin, Dan Keramik PZT." },
  { "en": "Apa Kemampuan Material Menghantarkan Panas?", "id": "Konduktivitas Termal Dari Suatu Material." },
  { "en": "Logam Umumnya Punya Konduktivitas Termal Bagaimana?", "id": "Sangat Tinggi, Penghantar Panas Baik." },
  { "en": "Apa Perubahan Dimensi Material Akibat Suhu?", "id": "Ekspansi Termal Atau Pemuaian Termal." },
  { "en": "Apa Material Dengan Titik Leleh Tertinggi?", "id": "Tungsten (W), Sekitar 3422 Derajat Celcius." },
  { "en": "Apa Campuran Dua Atau Lebih Logam?", "id": "Paduan Logam (Alloy) Dengan Sifat Baru." },
  { "en": "Apa Paduan Tembaga Dan Timah (Sn)?", "id": "Perunggu, Lebih Keras Dari Tembaga." },
  { "en": "Apa Paduan Tembaga Dan Seng (Zn)?", "id": "Kuningan, Tahan Korosi Dan Dekoratif." },
  { "en": "Apa Material Yang Terdiri Dari Monomer?", "id": "Polimer, Rantai Molekul Sangat Panjang." },
  { "en": "Apa Contoh Polimer Yang Termoplastik?", "id": "Polietilena (PE), Polivinil Klorida (PVC)." },
  { "en": "Apa Contoh Polimer Yang Termoset?", "id": "Bakelit, Resin Epoksi, Dan Poliuretan." },
  { "en": "Apa Material Anorganik Dan Non-Logam?", "id": "Keramik, Seperti Alumina Dan Silika." },
  { "en": "Bagaimana Sifat Umum Dari Material Keramik?", "id": "Keras, Rapuh, Tahan Suhu Tinggi." },
  { "en": "Apa Gabungan Dua Material Atau Lebih?", "id": "Material Komposit Dengan Sifat Unggul." },
  { "en": "Apa Contoh Material Komposit Umum?", "id": "Beton, Fiberglass, Serat Karbon (CFRP)." },
  { "en": "Apa Material Yang Hambatannya Nol Mutlak?", "id": "Superkonduktor Pada Suhu Sangat Kritis." },
  { "en": "Apa Proses Rusaknya Logam Akibat Reaksi Kimia?", "id": "Korosi Atau Proses Pengaratan Logam." },
  { "en": "Apa Lapisan Pelindung Tipis Pada Silikon?", "id": "Silikon Dioksida (SiO2) Sebagai Isolator." },
  { "en": "Apa Ukuran Hambatan Aliran Arus Listrik?", "id": "Resistivitas, Sifat Intrinsik Suatu Material." },
  { "en": "Bagaimana Resistivitas Konduktor Terhadap Suhu?", "id": "Meningkat Seiring Kenaikan Suhu Udara." },
  { "en": "Bagaimana Resistivitas Semikonduktor Terhadap Suhu?", "id": "Menurun Seiring Kenaikan Suhu Udara." },
  { "en": "Apa Bahan Utama Untuk Membuat Sel Surya?", "id": "Silikon (Si) Jenis Monokristalin, Polikristalin." },
  { "en": "Apa Fenomena Emisi Elektron Dari Logam Panas?", "id": "Emisi Termionik, Dasar Tabung Vakum." },
  { "en": "Apa Satuan Energi Celah Pita (Band Gap)?", "id": "Elektron Volt (eV) Yang Umumnya." },
  { "en": "Apa Tipe Struktur Kristal Pada Tembaga?", "id": "Face-Centered Cubic (FCC) Atau Kubus Pusat Muka." },
  { "en": "Apa Tipe Struktur Kristal Pada Besi?", "id": "Body-Centered Cubic (BCC) Atau Kubus Pusat Badan." },
  { "en": "Apa Itu Elektron Bebas Pada Logam?", "id": "Elektron Yang Tidak Terikat Atom." },
  { "en": "Apa Nama Batas Antar Butir Kristal?", "id": "Batas Butir (Grain Boundary) Material." },
  { "en": "Apa Efek Penambahan Karbon Pada Besi?", "id": "Meningkatkan Kekerasan Dan Kekuatan Besi." },
  { "en": "Apa Nama Lain Untuk Oksida Almunium?", "id": "Alumina (Al2O3), Material Keramik Keras." },
  { "en": "Mengapa Emas (Au) Digunakan Di Konektor?", "id": "Karena Tahan Korosi Dan Konduktif." },
  { "en": "Apa Itu Proses Annealing Pada Logam?", "id": "Pemanasan Untuk Melunakkan Dan Menghilangkan Stres." },
  { "en": "Apa Itu Proses Quenching Pada Baja?", "id": "Pendinginan Cepat Untuk Meningkatkan Kekerasan." },
  { "en": "Apa Fungsi Solder Dalam Rangkaian Elektronik?", "id": "Menyambungkan Komponen Dengan Ikatan Logam." },
  { "en": "Apa Kandungan Utama Material Solder?", "id": "Timah (Sn) Dan Timbal (Pb)." },
  { "en": "Apa Material Yang Digunakan Untuk Filamen Bohlam?", "id": "Tungsten (W) Karena Titik Lelehnya Tinggi." },
  { "en": "Apa Material Yang Digunakan Untuk Elemen Pemanas?", "id": "Nikrom (Paduan Nikel Dan Kromium)." },
  { "en": "Apa Sifat Optik Utama Dari Kaca?", "id": "Transparan Terhadap Cahaya Tampak Sekali." },
  { "en": "Apa Itu Efek Seebeck Pada Material?", "id": "Menghasilkan Tegangan Akibat Perbedaan Suhu." },
  { "en": "Apa Aplikasi Dari Efek Seebeck?", "id": "Termokopel Untuk Mengukur Suhu Ruangan." },
  { "en": "Apa Itu Efek Peltier Pada Material?", "id": "Menciptakan Perbedaan Suhu Dengan Arus." },
  { "en": "Apa Aplikasi Utama Dari Efek Peltier?", "id": "Pendingin Termoelektrik Tanpa Bagian Bergerak." },
  { "en": "Apa Bahan Dasar Dari Magnet Permanen?", "id": "Neodymium, Samarium-Cobalt, Dan Juga Ferit." },
  { "en": "Apa Itu Curie Temperature Pada Magnet?", "id": "Suhu Dimana Magnet Kehilangan Feromagnetismenya." },
  { "en": "Apa Itu Degradasi Pada Material Polimer?", "id": "Penurunan Sifat Akibat Panas, UV." },
  { "en": "Apa Material Inti Pada Transformator Listrik?", "id": "Besi Silikon Untuk Mengurangi Rugi Histeresis." },
  { "en": "Apa Material Isolasi Pada Kabel Listrik?", "id": "Polivinil Klorida (PVC) Atau Karet." },
  { "en": "Apa Kemampuan Material Menyerap Energi Tanpa Patah?", "id": "Ketangguhan (Toughness) Dari Suatu Material." },
  { "en": "Apa Kegagalan Material Akibat Beban Berulang?", "id": "Kelelahan (Fatigue) Material Pada Umumnya." },
  { "en": "Apa Deformasi Material Akibat Beban Konstan?", "id": "Mulur (Creep) Terutama Pada Suhu Tinggi." },
  { "en": "Apa Fungsi Timbal (Pb) Pada Solder?", "id": "Menurunkan Titik Leleh Paduan Solder." },
  { "en": "Apa Cacat Titik Berupa Atom Hilang?", "id": "Kekosongan (Vacancy) Dalam Kisi Kristal." },
  { "en": "Apa Cacat Titik Berupa Atom Sisipan?", "id": " interstisial, Atom Di Antara Kisi." },
  { "en": "Apa Pergerakan Atom Dalam Material Padat?", "id": "Difusi, Akibat Gradien Konsentrasi Partikel." },
  { "en": "Apa Dua Mekanisme Utama Proses Difusi?", "id": "Mekanisme Kekosongan Dan Juga Interstisial." },
  { "en": "Apa Hukum Yang Mengatur Laju Difusi?", "id": "Hukum Fick Pertama Dan Kedua." },
  { "en": "Apa Yang Dijelaskan Oleh Indeks Miller?", "id": "Orientasi Bidang Dalam Kisi Kristal." },
  { "en": "Berapa Jumlah Atom Dalam Sel Satuan BCC?", "id": "Dua Atom Per Sel Satuan." },
  { "en": "Berapa Jumlah Atom Dalam Sel Satuan FCC?", "id": "Empat Atom Per Sel Satuan." },
  { "en": "Apa Faktor Pengepakan Atom (APF) Itu?", "id": "Rasio Volume Atom Dengan Volume Sel." },
  { "en": "Apa Itu Material Dengan Butir Sangat Halus?", "id": "Material Nanokristalin Dengan Sifat Unik." },
  { "en": "Apa Prinsip Larangan Bagi Elektron?", "id": "Prinsip Larangan Pauli Dalam Orbital." },
  { "en": "Apa Fenomena Timbulnya Tegangan Akibat Cahaya?", "id": "Efek Fotovoltaik, Dasar Sel Surya." },
  { "en": "Apa Daerah Penipisan (Depletion Region) Itu?", "id": "Area Tanpa Pembawa Muatan Bebas." },
  { "en": "Dimana Daerah Penipisan Ditemukan?", "id": "Pada Sambungan P-N Semikonduktor Dioda." },
  { "en": "Apa Tegangan Yang Diperlukan Untuk Menyalakan Dioda?", "id": "Tegangan Maju (Forward Voltage) Dioda." },
  { "en": "Apa Itu Rekombinasi Dalam Semikonduktor?", "id": "Elektron Dan Lubang Saling Menetralkan." },
  { "en": "Apa Material Yang Memancarkan Cahaya Saat Dialiri Arus?", "id": "Light Emitting Diode (LED) Material." },
  { "en": "Apa Bahan Dasar Pembuatan LED Warna Merah?", "id": "Galium Arsenida Fosfida (GaAsP)." },
  { "en": "Apa Bahan Dasar Pembuatan LED Warna Biru?", "id": "Galium Nitrida (GaN), Indium Galium Nitrida (InGaN)." },
  { "en": "Apa Efek Timbulnya Tegangan Melintang?", "id": "Efek Hall Akibat Medan Magnet." },
  { "en": "Apa Aplikasi Utama Dari Efek Hall?", "id": "Sensor Posisi, Kecepatan, Dan Arus." },
  { "en": "Apa Kekuatan Dielektrik Suatu Isolator?", "id": "Tegangan Maksimum Sebelum Tembus (Breakdown)." },
  { "en": "Apa Satuan Untuk Kekuatan Dielektrik?", "id": "Volt Per Meter (V/m)." },
  { "en": "Apa Proses Pengerasan Permukaan Logam?", "id": "Carburizing, Nitriding, Atau Juga Plating." },
  { "en": "Apa Itu Diagram Fasa Pada Material?", "id": "Peta Yang Menunjukkan Fasa Material." },
  { "en": "Apa Titik Eutektik Dalam Diagram Fasa?", "id": "Suhu Terendah Campuran Menjadi Cair." },
  { "en": "Apa Sifat Material Yang Memiliki Memori Bentuk?", "id": "Shape Memory Alloy (SMA) Atau Paduan Memori Bentuk." },
  { "en": "Apa Contoh Material Shape Memory Alloy (SMA)?", "id": "Nitinol (Nikel Titanium) Adalah Contohnya." },
  { "en": "Apa Itu Proses Sintering Pada Keramik?", "id": "Pemanasan Bubuk Untuk Membentuk Padatan." },
  { "en": "Apa Itu Polimorfisme Dalam Material Kristal?", "id": "Kemampuan Zat Padat Dengan Banyak Struktur." },
  { "en": "Apa Fenomena Pemancaran Cahaya Oleh Material?", "id": "Luminesens, Seperti Fosforesens Dan Fluoresens." },
  { "en": "Apa Perbedaan Antara Fluoresens Dan Fosforesens?", "id": "Durasi Pendar Cahaya Setelah Eksitasi." },
  { "en": "Apa Itu Indeks Refraksi Suatu Material?", "id": "Ukuran Pembelokan Cahaya Saat Melewatinya." },
  { "en": "Apa Interaksi Cahaya Dengan Permukaan Material?", "id": "Refleksi (Pemantulan), Absorpsi (Penyerapan), Transmisi (Penerusan)." },
  { "en": "Mengapa Logam Terlihat Mengkilap Dan Buram?", "id": "Karena Memantulkan Sebagian Besar Cahaya." },
  { "en": "Apa Itu Pusat Warna (Color Center)?", "id": "Cacat Kisi Kristal Yang Menyerap Cahaya." },
  { "en": "Apa Kegunaan Germanium (Ge) Dalam Elektronik?", "id": "Sebagai Detektor Inframerah Dan Semikonduktor." },
  { "en": "Apa Itu Galium Arsenida (GaAs) Semikonduktor?", "id": "Semikonduktor Dengan Mobilitas Elektron Tinggi." },
  { "en": "Apa Keunggulan Galium Arsenida (GaAs) Dibanding Silikon?", "id": "Lebih Cepat, Tahan Radiasi, Efisien." },
  { "en": "Apa Material Konduktor Transparan Yang Umum?", "id": "Indium Tin Oxide (ITO) Oksida Timah Indium." },
  { "en": "Dimana Indium Tin Oxide (ITO) Sering Digunakan?", "id": "Pada Layar Sentuh, LCD, OLED." },
  { "en": "Apa Itu Material Amorf Atau Gelas Logam?", "id": "Logam Dengan Struktur Atom Tidak Teratur." },
  { "en": "Apa Keuntungan Gelas Logam (Amorphous Metal)?", "id": "Kekuatan Tinggi Dan Ketahanan Korosi." },
  { "en": "Apa Itu Material Magnetostriktif?", "id": "Material Yang Mengubah Bentuk Akibat Magnet." },
  { "en": "Apa Itu Material Elektrostriktif?", "id": "Material Yang Mengubah Bentuk Akibat Listrik." },
  { "en": "Apa Perbedaan Piezoelektrik Dan Elektrostriktif?", "id": "Piezoelektrik Linier, Elektrostriktif Kuadratik." },
  { "en": "Apa Itu Fonon Dalam Kisi Kristal?", "id": "Kuantum Getaran Atom Dalam Kristal." },
  { "en": "Bagaimana Fonon Mempengaruhi Konduktivitas Listrik?", "id": "Menghamburkan Elektron, Meningkatkan Resistivitas." },
  { "en": "Apa Itu Waktu Relaksasi Dalam Konduksi?", "id": "Waktu Rata-Rata Antar Tumbukan Elektron." },
  { "en": "Apa Itu Mobilitas Pembawa Muatan?", "id": "Kemudahan Pembawa Muatan Bergerak." },
  { "en": "Apa Satuan Mobilitas Pembawa Muatan?", "id": "Meter Kuadrat Per Volt-Detik (m²/Vs)." },
  { "en": "Apa Yang Dimaksud Dengan Anisotropi Material?", "id": "Sifat Material Berbeda Di Arah Berbeda." },
  { "en": "Apa Lawan Dari Sifat Anisotropi?", "id": "Isotropi, Sifat Sama Di Segala Arah." },
  { "en": "Apa Contoh Material Yang Sangat Anisotropik?", "id": "Kayu, Komposit Serat Karbon, Kristal." },
  { "en": "Apa Fungsi Kapasitor Dalam Rangkaian?", "id": "Menyimpan Energi Dalam Medan Listrik." },
  { "en": "Apa Material Dielektrik Dalam Kapasitor Keramik?", "id": "Barium Titanat Atau Strontium Titanat." },
  { "en": "Apa Itu Rugi-Rugi Dielektrik (Dielectric Loss)?", "id": "Disipasi Energi Panas Dalam Dielektrik." },
  { "en": "Apa Itu Material Piroelektrik?", "id": "Material Yang Menghasilkan Tegangan Akibat Suhu." },
  { "en": "Apa Itu Material Feroelektrik?", "id": "Material Dengan Polarisasi Listrik Spontan." },
  { "en": "Apa Karakteristik Kurva Histeresis Feroelektrik?", "id": "Menunjukkan Polarisasi Sisa Dan Koersivitas." },
  { "en": "Apa Teknik Untuk Menganalisis Struktur Permukaan?", "id": "Scanning Electron Microscope (SEM) Atau Mikroskop Elektron Pemindai." },
  { "en": "Apa Teknik Untuk Melihat Struktur Atom?", "id": "Transmission Electron Microscope (TEM) Atau Mikroskop Elektron Transmisi." },
  { "en": "Apa Itu Epitaksi Dalam Pertumbuhan Kristal?", "id": "Pertumbuhan Lapisan Kristal Diatas Substrat." },
  { "en": "Apa Proses Deposisi Uap Kimia (CVD)?", "id": "Proses Pembuatan Lapisan Tipis Berkualitas." },
  { "en": "Apa Tujuan Proses Etsa (Etching)?", "id": "Menghilangkan Material Secara Selektif." },
  { "en": "Apa Bahan Utama Pembuatan Serat Optik?", "id": "Silika (SiO2) Dengan Tingkat Kemurnian Tinggi." },
  { "en": "Apa Itu Hamburan Rayleigh Di Serat Optik?", "id": "Penyebab Utama Atenuasi Sinyal Cahaya." },
  { "en": "Apa Itu Kuantum Dot (Quantum Dot)?", "id": "Nanokristal Semikonduktor Dengan Sifat Optik." },
  { "en": "Bagaimana Ukuran Kuantum Dot Mempengaruhi Warna?", "id": "Ukuran Lebih Kecil Memancarkan Cahaya Biru." },
  { "en": "Apa Itu Grafena (Graphene)?", "id": "Lapisan Tunggal Atom Karbon Kristal." },
  { "en": "Apa Sifat Listrik Utama Dari Grafena?", "id": "Konduktivitas Sangat Tinggi, Mobilitas Tinggi." },
  { "en": "Apa Itu Karbon Nanotube (CNT)?", "id": "Struktur Silinder Dari Atom Karbon." },
  { "en": "Apa Itu Paduan Eutektik?", "id": "Campuran Dengan Satu Titik Leleh." },
  { "en": "Apa Itu Efek Meissner Pada Superkonduktor?", "id": "Penolakan Total Medan Magnet Eksternal." },
  { "en": "Apa Itu Kerapatan Arus Kritis Superkonduktor?", "id": "Kerapatan Arus Maksimum Yang Diizinkan." },
  { "en": "Apa Dua Tipe Utama Superkonduktor?", "id": "Superkonduktor Tipe I Dan Tipe II." },
  { "en": "Apa Itu Polimer Konduktif?", "id": "Plastik Yang Dapat Menghantarkan Listrik." },
  { "en": "Apa Contoh Dari Polimer Konduktif?", "id": "Polianilin, Poliprol, Dan Juga PEDOT:PSS." },
  { "en": "Apa Itu Sol-Gel Processing?", "id": "Metode Sintesis Material Dari Larutan." },
  { "en": "Apa Itu Tegangan Permukaan Pada Cairan?", "id": "Kecenderungan Cairan Untuk Mengerut." },
  { "en": "Apa Pengaruh Cacat Kristal Terhadap Kekuatan?", "id": "Umumnya Akan Mengurangi Kekuatan Material." },
  { "en": "Apa Itu Pengerasan Regangan (Strain Hardening)?", "id": "Logam Menjadi Lebih Kuat Saat Dideformasi." },
  { "en": "Apa Itu Hambatan Kontak Listrik?", "id": "Hambatan Pada Antarmuka Dua Konduktor." },
  { "en": "Apa Itu Whisker Dalam Pertumbuhan Kristal?", "id": "Pertumbuhan Kristal Tunggal Yang Tipis." },
  { "en": "Apa Material Untuk Termistor Jenis NTC?", "id": "Oksida Logam Seperti Mangan, Nikel." },
  { "en": "Apa Sifat Termistor NTC Terhadap Suhu?", "id": "Hambatan Menurun Saat Suhu Naik." },
  { "en": "Apa Sifat Termistor PTC Terhadap Suhu?", "id": "Hambatan Naik Drastis Pada Suhu Tertentu." },
  { "en": "Apa Itu Resistivitas Van Der Pauw?", "id": "Metode Pengukuran Resistivitas Lapisan Tipis." },
  { "en": "Apa Itu Efek Termoelektrik Anisotropik?", "id": "Efek Seebeck Bergantung Arah Kristal." },
  { "en": "Apa Itu Konstanta Kisi (Lattice Constant)?", "id": "Dimensi Fisik Dari Sel Satuan." },
  { "en": "Apa Yang Menyebabkan Material Berwarna?", "id": "Penyerapan Selektif Panjang Gelombang Cahaya." },
  { "en": "Apa Itu Teori Pita Pada Zat Padat?", "id": "Model Yang Menjelaskan Struktur Elektronik." },
  { "en": "Apa Itu Permukaan Fermi Dalam Logam?", "id": "Batas Antara Keadaan Elektron Terisi." },
  { "en": "Apa Yang Digambarkan Kurva Stres-Regangan (Stress-Strain)?", "id": "Respons Mekanis Material Terhadap Beban." },
  { "en": "Apa Titik Pada Kurva Stres-Regangan (Stress-Strain)?", "id": "Titik Luluh (Yield Point) Material." },
  { "en": "Apa Batas Stres Maksimum Suatu Material?", "id": "Kekuatan Tarik Tertinggi (Ultimate Tensile Strength)." },
  { "en": "Apa Modulus Young Atau Modulus Elastisitas?", "id": "Ukuran Kekakuan Material Elastis Suatu Benda." },
  { "en": "Apa Rasio Poisson Pada Suatu Material?", "id": "Rasio Regangan Transversal Terhadap Regangan Aksial." },
  { "en": "Apa Batas Lelah (Fatigue Limit) Material?", "id": "Tegangan Maksimum Tanpa Kegagalan Lelah." },
  { "en": "Apa Itu Korosi Galvanik Pada Material?", "id": "Korosi Akibat Dua Logam Berbeda." },
  { "en": "Bagaimana Cara Mencegah Korosi Galvanik?", "id": "Dengan Proteksi Katodik Atau Isolasi Listrik." },
  { "en": "Apa Itu Proses Pelapisan Anodizing?", "id": "Membentuk Lapisan Oksida Pelindung." },
  { "en": "Pada Logam Apa Proses Anodizing Dilakukan?", "id": "Umumnya Pada Aluminium Dan Paduannya." },
  { "en": "Apa Itu Tingkat Fermi (Fermi Level)?", "id": "Tingkat Energi Dengan Probabilitas Setengah Terisi." },
  { "en": "Di Mana Posisi Tingkat Fermi Semikonduktor Intrinsik?", "id": "Di Tengah Celah Pita Energi." },
  { "en": "Di Mana Posisi Tingkat Fermi Semikonduktor Tipe-N?", "id": "Dekat Dengan Pita Konduksi Atas." },
  { "en": "Di Mana Posisi Tingkat Fermi Semikonduktor Tipe-P?", "id": "Dekat Dengan Pita Valensi Bawah." },
  { "en": "Apa Itu Massa Efektif Elektron?", "id": "Massa Partikel Dalam Model Pita Energi." },
  { "en": "Apa Itu Waktu Hidup Pembawa Muatan?", "id": "Waktu Rata-Rata Sebelum Terjadi Rekombinasi." },
  { "en": "Apa Material Magnet Keras (Hard Magnet)?", "id": "Material Yang Sulit Dimagnetkan Dan Dihilangkan." },
  { "en": "Apa Contoh Material Magnet Keras?", "id": "Neodymium Iron Boron (NdFeB), Alnico." },
  { "en": "Apa Material Magnet Lunak (Soft Magnet)?", "id": "Material Mudah Dimagnetkan Dan Dihilangkan." },
  { "en": "Apa Contoh Material Magnet Lunak?", "id": "Besi-Silikon (Fe-Si) Untuk Inti Trafo." },
  { "en": "Apa Yang Diwakili Luas Kurva Histeresis Magnetik?", "id": "Luasnya Mewakili Energi Rugi-Rugi Panas." },
  { "en": "Apa Tiga Mekanisme Polarisasi Dielektrik?", "id": "Elektronik, Ionik, Dan Juga Orientasi." },
  { "en": "Apa Polarisasi Akibat Pergeseran Awan Elektron?", "id": "Polarisasi Elektronik Dalam Medan Listrik." },
  { "en": "Apa Polarisasi Akibat Pergeseran Ion?", "id": "Polarisasi Ionik Pada Material Ionik." },
  { "en": "Apa Polarisasi Akibat Penyejajaran Dipol?", "id": "Polarisasi Orientasi Pada Molekul Polar." },
  { "en": "Apa Itu Tangen Rugi (Loss Tangent)?", "id": "Ukuran Inefisiensi Material Dielektrik." },
  { "en": "Apa Itu Kristal Fotonik (Photonic Crystal)?", "id": "Struktur Periodik Yang Mempengaruhi Foton." },
  { "en": "Apa Itu Metamaterial Dalam Teknik Elektro?", "id": "Material Buatan Dengan Sifat Elektromagnetik Unik." },
  { "en": "Apa Itu Proses Deposisi Uap Fisik (PVD)?", "id": "Pelapisan Material Melalui Proses Penguapan." },
  { "en": "Apa Itu Sputtering Dalam Proses Pelapisan?", "id": "Teknik PVD Menggunakan Bombardir Ion." },
  { "en": "Apa Dua Komponen Utama Material Komposit?", "id": "Matriks (Pengikat) Dan Penguat (Reinforcement)." },
  { "en": "Apa Fungsi Matriks Dalam Material Komposit?", "id": "Mengikat Serat Penguat Secara Bersamaan." },
  { "en": "Apa Fungsi Penguat Dalam Material Komposit?", "id": "Memberikan Kekuatan Dan Kekakuan Utama." },
  { "en": "Apa Contoh Material Untuk Matriks Polimer?", "id": "Resin Epoksi, Poliester, Dan Vinilester." },
  { "en": "Apa Contoh Material Untuk Serat Penguat?", "id": "Serat Karbon, Serat Kaca, Aramid." },
  { "en": "Apa Penyebab Degradasi Polimer Oleh Sinar UV?", "id": "Pemutusan Rantai Polimer Akibat Radiasi." },
  { "en": "Apa Itu Indeks Leleh (Melt Index)?", "id": "Ukuran Kemudahan Aliran Leburan Polimer." },
  { "en": "Apa Itu Suhu Transisi Gelas (Tg)?", "id": "Suhu Saat Polimer Amorf Melunak." },
  { "en": "Apa Itu Paduan Memori Bentuk Satu Arah?", "id": "Hanya Ingat Satu Bentuk Saat Dipanaskan." },
  { "en": "Apa Itu Paduan Memori Bentuk Dua Arah?", "id": "Mengingat Bentuk Panas Dan Dingin." },
  { "en": "Apa Itu Hamburan Fonon (Phonon Scattering)?", "id": "Mekanisme Utama Resistivitas Pada Logam." },
  { "en": "Apa Itu Efek Kulit (Skin Effect)?", "id": "Arus AC Cenderung Mengalir Di Permukaan." },
  { "en": "Apa Pengaruh Efek Kulit (Skin Effect)?", "id": "Meningkatkan Resistansi Efektif Pada Frekuensi Tinggi." },
  { "en": "Apa Itu Kedalaman Kulit (Skin Depth)?", "id": "Ukuran Seberapa Jauh Arus Menembus." },
  { "en": "Apa Itu Material Elektrokromik?", "id": "Material Yang Mengubah Warna Akibat Tegangan." },
  { "en": "Di Mana Aplikasi Material Elektrokromik?", "id": "Jendela Pintar (Smart Windows) Gedung." },
  { "en": "Apa Itu Material Termokromik?", "id": "Material Yang Mengubah Warna Akibat Suhu." },
  { "en": "Apa Itu Material Fotokromik?", "id": "Material Mengubah Warna Akibat Cahaya." },
  { "en": "Apa Itu Paduan Tahan Panas (Superalloy)?", "id": "Paduan Dengan Kekuatan Suhu Tinggi." },
  { "en": "Apa Basis Unsur Untuk Superalloy?", "id": "Nikel (Ni), Kobalt (Co), Atau Besi-Nikel (Fe-Ni)." },
  { "en": "Apa Itu Koefisien Ekspansi Termal (CTE)?", "id": "Ukuran Pemuaian Material Terhadap Suhu." },
  { "en": "Mengapa Koefisien Ekspansi Termal (CTE) Penting?", "id": "Untuk Mencegah Stres Termal Material." },
  { "en": "Apa Material Piezoelektrik Yang Paling Umum?", "id": "Timbal Zirkonat Titanat (PZT)." },
  { "en": "Apa Itu Kopling Elektromekanis?", "id": "Konversi Energi Antara Domain Mekanis-Listrik." },
  { "en": "Apa Itu Angka Kualitas (Q Factor)?", "id": "Ukuran Efisiensi Resonator Mekanis Listrik." },
  { "en": "Apa Itu Keramik Teknis Atau Keramik Maju?", "id": "Keramik Dengan Sifat Dan Kemurnian Tinggi." },
  { "en": "Apa Contoh Keramik Teknis Yang Umum?", "id": "Alumina (Al2O3), Zirkonia (ZrO2), Silikon Nitrida (Si3N4)." },
  { "en": "Apa Keunggulan Keramik Zirkonia (ZrO2)?", "id": "Ketangguhan Retak Yang Sangat Tinggi." },
  { "en": "Apa Kegunaan Silikon Karbida (SiC)?", "id": "Untuk Semikonduktor Daya Dan Suhu Tinggi." },
  { "en": "Apa Itu Semikonduktor Celah Pita Lebar?", "id": "Material Seperti SiC Dan GaN." },
  { "en": "Apa Keuntungan Semikonduktor Celah Pita Lebar?", "id": "Operasi Pada Tegangan Dan Suhu Tinggi." },
  { "en": "Apa Itu Tegangan Tembus Balik (Reverse Breakdown)?", "id": "Tegangan Saat Dioda Mundur Konduktif." },
  { "en": "Apa Dua Mekanisme Tegangan Tembus Balik?", "id": "Efek Zener Dan Efek Longsoran (Avalanche)." },
  { "en": "Kapan Efek Zener Terjadi Pada Dioda?", "id": "Pada Dioda Dengan Doping Sangat Tinggi." },
  { "en": "Kapan Efek Longsoran (Avalanche) Terjadi?", "id": "Pada Dioda Dengan Doping Lebih Rendah." },
  { "en": "Apa Itu Arus Bocor (Leakage Current)?", "id": "Arus Kecil Saat Perangkat Mati." },
  { "en": "Apa Itu Litografi Dalam Fabrikasi Chip?", "id": "Proses Transfer Pola Ke Substrat." },
  { "en": "Apa Itu Wafer Dalam Industri Semikonduktor?", "id": "Substrat Tipis Untuk Fabrikasi Sirkuit." },
  { "en": "Apa Bahan Dasar Wafer Semikonduktor?", "id": "Umumnya Silikon Monokristalin Sangat Murni." },
  { "en": "Apa Yang Dimaksud Dengan Plastik Biodegradable?", "id": "Plastik Yang Dapat Terurai Mikroorganisme." },
  { "en": "Apa Itu Teflon Atau PTFE?", "id": "Polytetrafluoroethylene, Polimer Sangat Tahan Kimia." },
  { "en": "Apa Sifat Listrik Utama Dari Teflon?", "id": "Konstanta Dielektrik Rendah Dan Isolator Baik." },
  { "en": "Apa Itu Proses Ekstrusi Pada Polimer?", "id": "Mendorong Material Melewati Cetakan (Die)." },
  { "en": "Apa Itu Proses Cetak Injeksi (Injection Molding)?", "id": "Menyuntikkan Plastik Lebur Ke Cetakan." },
  { "en": "Apa Itu Permeabilitas Magnetik Suatu Material?", "id": "Kemampuan Material Mendukung Medan Magnet." },
  { "en": "Apa Simbol Untuk Permeabilitas Magnetik?", "id": "Simbol Mu (μ) Dalam Fisika." },
  { "en": "Apa Itu Koersivitas Medan Magnet?", "id": "Kekuatan Medan Magnet Untuk Menghilangkan Magnetisasi." },
  { "en": "Magnet Keras Memiliki Koersivitas Bagaimana?", "id": "Memiliki Nilai Koersivitas Sangat Tinggi." },
  { "en": "Magnet Lunak Memiliki Koersivitas Bagaimana?", "id": "Memiliki Nilai Koersivitas Sangat Rendah." },
  { "en": "Apa Itu Domain Magnetik Dalam Material?", "id": "Wilayah Kecil Dengan Arah Magnetisasi Seragam." },
  { "en": "Apa Itu Dinding Domain (Domain Wall)?", "id": "Antarmuka Yang Memisahkan Domain Magnetik." },
  { "en": "Apa Itu Efek Barkhausen Dalam Magnetisme?", "id": "Perubahan Bertahap Dalam Proses Magnetisasi." },
  { "en": "Apa Itu Viskositas Pada Material Cair?", "id": "Ukuran Ketahanan Cairan Untuk Mengalir." },
  { "en": "Apa Itu Sifat Tiksotropik Suatu Material?", "id": "Viskositas Menurun Seiring Waktu Getaran." },
  { "en": "Apa Itu Material Komposit Laminat?", "id": "Tersusun Dari Beberapa Lapisan (Ply)." },
  { "en": "Apa Itu Material Komposit Sandwich?", "id": "Dua Lapisan Kuat Dengan Inti Ringan." },
  { "en": "Apa Fungsi Inti Pada Komposit Sandwich?", "id": "Memberikan Ketebalan Dan Ketahanan Tekuk." },
  { "en": "Apa Itu Proses Pultrusion Pada Komposit?", "id": "Menarik Serat Melalui Resin Dan Cetakan." },
  { "en": "Apa Itu Cacat Interstisial Frenkel?", "id": "Pasangan Cacat Kekosongan Dan Interstisial." },
  { "en": "Apa Itu Cacat Interstisial Schottky?", "id": "Pasangan Kekosongan Kation Dan Anion." },
  { "en": "Di Mana Cacat Schottky Umum Ditemukan?", "id": "Pada Material Keramik Dengan Ikatan Ionik." },
  { "en": "Apa Itu Perlakuan Panas (Heat Treatment)?", "id": "Proses Mengubah Sifat Fisik Logam." },
  { "en": "Apa Tujuan Dari Tempering Pada Baja?", "id": "Mengurangi Kerapuhan Setelah Proses Quenching." },
  { "en": "Apa Tujuan Dari Normalizing Pada Baja?", "id": "Menghaluskan Ukuran Butir Dan Menyeragamkan." },
  { "en": "Apa Perbedaan Semikonduktor Celah Pita Langsung?", "id": "Rekombinasi Elektron-Lubang Memancarkan Cahaya." },
  { "en": "Apa Perbedaan Semikonduktor Celah Pita Tidak Langsung?", "id": "Rekombinasi Yang Membutuhkan Bantuan Fonon." },
  { "en": "Apa Contoh Semikonduktor Celah Pita Langsung?", "id": "Galium Arsenida (GaAs) Untuk Aplikasi LED." },
  { "en": "Apa Contoh Semikonduktor Celah Pita Tidak Langsung?", "id": "Silikon (Si) Dan Germanium (Ge)." },
  { "en": "Apa Itu Arus Hanyut (Drift Current)?", "id": "Aliran Muatan Akibat Adanya Medan Listrik." },
  { "en": "Apa Itu Arus Difusi (Diffusion Current)?", "id": "Aliran Muatan Akibat Gradien Konsentrasi." },
  { "en": "Apa Itu Proses Generasi Pembawa Muatan?", "id": "Penciptaan Pasangan Elektron Dan Lubang." },
  { "en": "Apa Penyebab Generasi Pembawa Muatan Termal?", "id": "Energi Panas Yang Mengeksitasi Elektron." },
  { "en": "Apa Itu Material Dengan Sifat Optik Ganda?", "id": "Birefringence, Indeks Bias Bergantung Polarisasi." },
  { "en": "Apa Itu Material Dielektrik High-K?", "id": "Material Dengan Konstanta Dielektrik Tinggi." },
  { "en": "Apa Fungsi Dielektrik High-K Di Transistor?", "id": "Mengurangi Arus Bocor Gerbang (Gate)." },
  { "en": "Apa Contoh Material Dielektrik High-K?", "id": "Hafnium Dioksida (HfO2) Dan Zirkonium Dioksida (ZrO2)." },
  { "en": "Apa Itu Material Dielektrik Low-K?", "id": "Material Dengan Konstanta Dielektrik Rendah." },
  { "en": "Apa Fungsi Dielektrik Low-K Di Chip?", "id": "Mengurangi Kapasitansi Parasit Antar Jalur." },
  { "en": "Apa Itu Material Antiferomagnetik?", "id": "Momen Magnetik Tetangga Berlawanan Arah." },
  { "en": "Apa Itu Material Ferrimagnetik (Ferit)?", "id": "Momen Magnetik Tetangga Tidak Seimbang." },
  { "en": "Apa Sifat Listrik Umum Dari Ferit?", "id": "Resistivitas Tinggi, Rugi Arus Eddy Rendah." },
  { "en": "Apa Aplikasi Umum Dari Material Ferit?", "id": "Inti Induktor Frekuensi Tinggi, Trafo." },
  { "en": "Apa Itu Material Antiferoelektrik?", "id": "Dipol Listrik Tetangga Berlawanan Arah." },
  { "en": "Apa Itu Material Multiferroik?", "id": "Memiliki Lebih Dari Satu Sifat Feroik." },
  { "en": "Apa Itu Efek Magnetoelektrik Dalam Material?", "id": "Perubahan Polarisasi Listrik Akibat Magnet." },
  { "en": "Apa Itu Angka Manfaat (Figure of Merit) ZT?", "id": "Ukuran Efisiensi Material Termoelektrik." },
  { "en": "Apa Komponen Dari Angka Manfaat (ZT)?", "id": "Efek Seebeck, Konduktivitas, Suhu." },
  { "en": "Apa Itu Material Antarmuka Termal (TIM)?", "id": "Material Untuk Transfer Panas Antar Permukaan." },
  { "en": "Apa Contoh Material Antarmuka Termal (TIM)?", "id": "Pasta Termal, Bantalan Termal (Thermal Pad)." },
  { "en": "Apa Fungsi Utama Dari Heatsink?", "id": "Menyerap Dan Melepaskan Panas Berlebih." },
  { "en": "Apa Material Yang Umum Untuk Heatsink?", "id": "Aluminium Atau Tembaga Karena Konduktif." },
  { "en": "Apa Itu Material Perubah Fasa (PCM)?", "id": "Menyerap Panas Saat Meleleh, Melepas Saat Membeku." },
  { "en": "Apa Itu Konsentrasi Tegangan (Stress Concentration)?", "id": "Peningkatan Tegangan Di Sekitar Cacat." },
  { "en": "Apa Itu Ketangguhan Retak (Fracture Toughness)?", "id": "Kemampuan Material Menahan Propagasi Retak." },
  { "en": "Apa Itu Mekanisme Retak Getas (Brittle Fracture)?", "id": "Terjadi Tiba-Tiba Tanpa Deformasi Plastis." },
  { "en": "Apa Itu Mekanisme Retak Ulet (Ductile Fracture)?", "id": "Didahului Oleh Deformasi Plastis Besar." },
  { "en": "Apa Fungsi Fluks (Flux) Dalam Penyolderan?", "id": "Membersihkan Oksida Dari Permukaan Logam." },
  { "en": "Apa Itu Proses Solder Reflow?", "id": "Melelehkan Pasta Solder Dengan Oven." },
  { "en": "Apa Itu Proses Solder Gelombang (Wave Soldering)?", "id": "Menyolder Komponen Dengan Gelombang Solder." },
  { "en": "Apa Bahan Kawat Untuk Proses Wire Bonding?", "id": "Emas (Au), Aluminium (Al), Atau Tembaga (Cu)." },
  { "en": "Apa Fungsi Enkapsulasi Pada Komponen Elektronik?", "id": "Melindungi Dari Kelembaban Dan Kerusakan." },
  { "en": "Apa Material Untuk Enkapsulasi Elektronik?", "id": "Senyawa Epoksi, Silikon, Dan Poliuretan." },
  { "en": "Apa Itu Efek Piezoelektrik Terbalik?", "id": "Material Berubah Bentuk Akibat Medan Listrik." },
  { "en": "Apa Itu Impedansi Akustik Material?", "id": "Ukuran Hambatan Gelombang Suara." },
  { "en": "Apa Pentingnya Mencocokkan Impedansi Akustik?", "id": "Untuk Transfer Energi Suara Maksimal." },
  { "en": "Apa Sebutan Untuk Paduan Aluminium Seri 6000?", "id": "Paduan Dengan Magnesium Dan Juga Silikon." },
  { "en": "Apa Sebutan Untuk Paduan Aluminium Seri 7000?", "id": "Paduan Dengan Seng Sebagai Elemen Utama." },
  { "en": "Apa Itu Outgassing Material Di Ruang Vakum?", "id": "Pelepasan Gas Terperangkap Dari Material." },
  { "en": "Mengapa Outgassing Menjadi Masalah Di Vakum?", "id": "Dapat Mengkontaminasi Permukaan Sensitif Lainnya." },
  { "en": "Apa Itu Pengerasan Radiasi (Radiation Hardening)?", "id": "Membuat Komponen Elektronik Tahan Radiasi." },
  { "en": "Apa Dampak Radiasi Pada Semikonduktor?", "id": "Menciptakan Cacat Kisi, Mengubah Sifat." },
  { "en": "Apa Itu Fosfor (Phosphor) Dalam Material?", "id": "Material Yang Menunjukkan Fenomena Luminesens." },
  { "en": "Bagaimana Cara Kerja Fosfor Pada Lampu Fluoresen?", "id": "Mengubah Radiasi UV Menjadi Cahaya Tampak." },
  { "en": "Apa Itu Material Scintillator?", "id": "Memancarkan Foton Ketika Menyerap Radiasi." },
  { "en": "Di Mana Material Scintillator Digunakan?", "id": "Pada Detektor Radiasi Dan Pencitraan Medis." },
  { "en": "Apa Itu Hukum Bragg Dalam Kristalografi?", "id": "Menjelaskan Difraksi Sinar-X Oleh Kristal." },
  { "en": "Apa Isi Dari Hukum Wiedemann-Franz?", "id": "Rasio Konduktivitas Termal-Listrik Adalah Proporsional." },
  { "en": "Apa Yang Dimaksud Dengan Solvasi?", "id": "Interaksi Molekul Pelarut Dengan Zat Terlarut." },
  { "en": "Apa Itu Efek Kirkendall Dalam Difusi?", "id": "Pergerakan Antarmuka Akibat Laju Difusi." },
  { "en": "Apa Itu Paduan Amorf?", "id": "Paduan Logam Dengan Struktur Non-Kristalin." },
  { "en": "Apa Itu Material Ausferitik?", "id": "Struktur Mikro Besi Karbon Kekuatan Tinggi." },
  { "en": "Apa Itu Baja Tahan Karat (Stainless Steel)?", "id": "Paduan Besi Dengan Minimal Kromium 10.5%." },
  { "en": "Apa Fungsi Kromium Pada Baja Tahan Karat?", "id": "Membentuk Lapisan Oksida Pasif Pelindung." },
  { "en": "Apa Tiga Jenis Utama Baja Tahan Karat?", "id": "Austenitik, Feritik, Dan Juga Martensit." },
  { "en": "Apa Karakteristik Baja Tahan Karat Austenitik?", "id": "Non-Magnetik, Ketangguhan Baik, Tahan Korosi." },
  { "en": "Apa Karakteristik Baja Tahan Karat Martensit?", "id": "Sangat Keras, Dapat Diperkeras, Magnetik." },
  { "en": "Apa Itu Paduan Invar?", "id": "Paduan Besi-Nikel Dengan Ekspansi Termal Rendah." },
  { "en": "Apa Itu Paduan Kovar?", "id": "Paduan Dengan Ekspansi Termal Mirip Kaca." },
  { "en": "Apa Aplikasi Dari Paduan Kovar?", "id": "Untuk Sambungan Logam-Kaca Elektronik." },
  { "en": "Apa Itu Dislokasi Ulir (Screw Dislocation)?", "id": "Cacat Garis Sejajar Vektor Burgers." },
  { "en": "Apa Itu Dislokasi Sisi (Edge Dislocation)?", "id": "Cacat Garis Tegak Lurus Vektor Burgers." },
  { "en": "Apa Itu Vektor Burgers?", "id": "Besaran Dan Arah Distorsi Kisi." },
  { "en": "Apa Itu Bidang Slip (Slip Plane)?", "id": "Bidang Dimana Dislokasi Paling Mudah Bergerak." },
  { "en": "Bagaimana Penguatan Batas Butir Bekerja?", "id": "Batas Butir Menghalangi Gerakan Dislokasi." },
  { "en": "Apa Itu Penguatan Larutan Padat?", "id": "Menambahkan Atom Paduan Menghalangi Dislokasi." },
  { "en": "Apa Itu Pengerasan Presipitasi?", "id": "Membentuk Partikel Fasa Kedua Menghalangi Dislokasi." },
  { "en": "Apa Kepanjangan Dari Material GRP?", "id": "Glass-Reinforced Plastic Atau Plastik Bertulang Kaca." },
  { "en": "Apa Kepanjangan Dari Material CFRP?", "id": "Carbon Fiber-Reinforced Polymer Atau Polimer Bertulang Karbon." },
  { "en": "Apa Keunggulan Utama Material CFRP?", "id": "Rasio Kekuatan-Terhadap-Berat Sangat Tinggi." },
  { "en": "Apa Itu Proses Pengerolan (Rolling) Logam?", "id": "Mengurangi Ketebalan Logam Dengan Rol." },
  { "en": "Apa Itu Proses Penempaan (Forging) Logam?", "id": "Membentuk Logam Dengan Tekanan Lokal." },
  { "en": "Apa Itu Metalurgi Serbuk (Powder Metallurgy)?", "id": "Proses Pembuatan Benda Dari Serbuk Logam." },
  { "en": "Apa Itu Kaca Tempered (Tempered Glass)?", "id": "Kaca Yang Diperkuat Melalui Pemanasan." },
  { "en": "Bagaimana Pola Pecahan Dari Kaca Tempered?", "id": "Menjadi Butiran-Butiran Kecil Yang Tumpul." },
  { "en": "Apa Itu Kaca Laminasi (Laminated Glass)?", "id": "Dua Lapisan Kaca Dengan Lapisan Plastik." },
  { "en": "Apa Itu Efek Fotokonduktif?", "id": "Peningkatan Konduktivitas Listrik Akibat Cahaya." },
  { "en": "Apa Itu Varistor Oksida Logam (MOV)?", "id": "Resistor Yang Bergantung Pada Tegangan." },
  { "en": "Apa Fungsi Utama Varistor Oksida Logam (MOV)?", "id": "Sebagai Pelindung Terhadap Lonjakan Tegangan." },
  { "en": "Apa Bahan Utama Pembuatan Varistor?", "id": "Seng Oksida (ZnO) Dengan Aditif." },
  { "en": "Apa Itu Kapasitor Elektrolitik?", "id": "Kapasitor Terpolarisasi Dengan Kapasitansi Tinggi." },
  { "en": "Apa Material Anoda Pada Kapasitor Tantalum?", "id": "Logam Tantalum Sebagai Anoda Utamanya." },
  { "en": "Apa Keunggulan Kapasitor Tantalum?", "id": "Kepadatan Kapasitansi Tinggi Dan Stabil." },
  { "en": "Apa Itu Koefisien Suhu Hambatan (TCR)?", "id": "Perubahan Hambatan Material Per Derajat Suhu." },
  { "en": "Apa Itu Bahan Bakar Termoelektrik Radioisotop (RTG)?", "id": "Generator Listrik Dari Peluruhan Radioaktif." },
  { "en": "Apa Itu Rekristalisasi Pada Logam?", "id": "Pembentukan Butir Baru Setelah Deformasi." },
  { "en": "Apa Itu Suhu Rekristalisasi?", "id": "Suhu Minimum Untuk Proses Rekristalisasi." },
  { "en": "Apa Itu Pengerjaan Dingin (Cold Working)?", "id": "Deformasi Plastis Di Bawah Suhu Rekristalisasi." },
  { "en": "Apa Itu Pengerjaan Panas (Hot Working)?", "id": "Deformasi Di Atas Suhu Rekristalisasi." },
  { "en": "Apa Efek Pengerjaan Dingin Pada Logam?", "id": "Meningkatkan Kekuatan, Mengurangi Keuletan." },
  { "en": "Apa Itu Elastomer?", "id": "Polimer Dengan Elastisitas Sangat Tinggi." },
  { "en": "Apa Itu Korosi Sumuran (Pitting Corrosion)?", "id": "Korosi Lokal Yang Membentuk Lubang Kecil." },
  { "en": "Apa Itu Korosi Celah (Crevice Corrosion)?", "id": "Korosi Lokal Di Dalam Celah Sempit." },
  { "en": "Apa Itu Retak Korosi Tegangan (SCC)?", "id": "Kegagalan Akibat Kombinasi Tegangan Dan Korosi." },
  { "en": "Apa Itu Kopolimer Dalam Material Polimer?", "id": "Polimer Terbentuk Dari Dua Monomer Berbeda." },
  { "en": "Apa Itu Kopolimer Blok (Block Copolymer)?", "id": "Polimer Dari Dua Atau Lebih Blok Monomer." },
  { "en": "Apa Itu Kopolimer Acak (Random Copolymer)?", "id": "Monomer Tersusun Acak Dalam Rantai Polimer." },
  { "en": "Apa Pengaruh Kristalinitas Pada Sifat Polimer?", "id": "Meningkatkan Kekuatan, Kekakuan, Dan Kepadatan." },
  { "en": "Apa Itu Proses Deposisi Lapisan Atom (ALD)?", "id": "Deposisi Lapisan Tipis Atom Per Atom." },
  { "en": "Apa Keunggulan Proses Deposisi Lapisan Atom (ALD)?", "id": "Kontrol Ketebalan Dan Keseragaman Sangat Presisi." },
  { "en": "Apa Itu Tegangan Permukaan (Surface Tension)?", "id": "Sifat Kohesif Cairan Yang Menahan Pecah." },
  { "en": "Apa Itu Sudut Kontak (Contact Angle)?", "id": "Sudut Antara Tetesan Cairan Dan Permukaan." },
  { "en": "Apa Arti Sudut Kontak Di Bawah 90°?", "id": "Permukaan Bersifat Hidrofilik Atau Mudah Basah." },
  { "en": "Apa Arti Sudut Kontak Di Atas 90°?", "id": "Permukaan Bersifat Hidrofobik Atau Sulit Basah." },
  { "en": "Apa Itu Keterbasahan (Wettability) Suatu Permukaan?", "id": "Kemampuan Cairan Untuk Membasahi Permukaan Padat." },
  { "en": "Apa Itu Material Biokompatibel?", "id": "Material Yang Tidak Menimbulkan Reaksi Imun." },
  { "en": "Apa Contoh Logam Biokompatibel Untuk Implan?", "id": "Titanium (Ti) Dan Paduannya Yang Kuat." },
  { "en": "Apa Itu Fenomena Terowongan Kuantum (Quantum Tunneling)?", "id": "Partikel Menembus Penghalang Energi Potensial." },
  { "en": "Apa Itu Efek Kurungan Kuantum (Quantum Confinement)?", "id": "Pembatasan Gerak Elektron Dalam Ruang Kecil." },
  { "en": "Apa Itu Eksiton (Exciton) Dalam Semikonduktor?", "id": "Pasangan Elektron-Lubang Yang Terikat Secara Coulomb." },
  { "en": "Apa Itu Degradasi Fotolisis Pada Polimer?", "id": "Kerusakan Akibat Radiasi Sinar Ultraviolet (UV)." },
  { "en": "Apa Itu Hidrolisis Pada Material Polimer?", "id": "Pemutusan Rantai Polimer Akibat Reaksi Air." },
  { "en": "Apa Itu Polimer Memori Bentuk (SMP)?", "id": "Polimer Yang Dapat Mengingat Bentuk Aslinya." },
  { "en": "Apa Pemicu Untuk Polimer Memori Bentuk?", "id": "Biasanya Panas, Cahaya, Atau Medan Listrik." },
  { "en": "Apa Itu Paduan Entropi Tinggi (HEA)?", "id": "Paduan Dengan Lima Atau Lebih Unsur Utama." },
  { "en": "Apa Prinsip Kerja Sensor Strain Gauge?", "id": "Perubahan Resistansi Akibat Regangan Mekanis." },
  { "en": "Apa Itu Faktor Gauge (Gauge Factor)?", "id": "Ukuran Sensitivitas Dari Strain Gauge." },
  { "en": "Apa Bahan Material Untuk Sensor Gas Oksida?", "id": "Timah Dioksida (SnO2), Seng Oksida (ZnO)." },
  { "en": "Bagaimana Prinsip Kerja Sensor Gas Oksida?", "id": "Perubahan Konduktivitas Saat Gas Teradsorpsi." },
  { "en": "Apa Material Katoda Umum Baterai Lithium-Ion?", "id": "Lithium Cobalt Oxide (LiCoO2) Dan Sejenisnya." },
  { "en": "Apa Material Anoda Umum Baterai Lithium-Ion?", "id": "Grafit (Bentuk Karbon) Yang Umumnya Dipakai." },
  { "en": "Apa Fungsi Elektrolit Dalam Baterai?", "id": "Mengangkut Ion Antara Anoda Dan Katoda." },
  { "en": "Apa Keuntungan Baterai Solid-State?", "id": "Keamanan Lebih Tinggi, Kepadatan Energi Potensial." },
  { "en": "Apa Prinsip Kerja Dari Superkapasitor?", "id": "Penyimpanan Muatan Pada Lapisan Ganda Listrik." },
  { "en": "Apa Material Elektroda Untuk Superkapasitor?", "id": "Karbon Aktif Dengan Luas Permukaan Tinggi." },
  { "en": "Apa Perbedaan Baterai Dan Superkapasitor?", "id": "Superkapasitor Mengisi Daya Lebih Cepat." },
  { "en": "Apa Itu Efek Proksimitas (Proximity Effect)?", "id": "Distribusi Ulang Arus Pada Konduktor Berdekatan." },
  { "en": "Apa Itu Pasivasi Permukaan Semikonduktor?", "id": "Proses Membuat Permukaan Non-Reaktif." },
  { "en": "Apa Bahan Yang Digunakan Untuk Pasivasi Silikon?", "id": "Silikon Dioksida (SiO2) Atau Silikon Nitrida (Si3N4)." },
  { "en": "Apa Itu Gettering Dalam Pemrosesan Wafer?", "id": "Proses Menghilangkan Atau Menonaktifkan Pengotor." },
  { "en": "Apa Itu Tumpukan Epitaksial (Epitaxial Stack)?", "id": "Beberapa Lapisan Epitaksial Dengan Fungsi Berbeda." },
  { "en": "Apa Itu Cacat Tumpukan (Stacking Fault)?", "id": "Kesalahan Urutan Penumpukan Bidang Kristal." },
  { "en": "Apa Itu Plastik Rekayasa (Engineering Plastics)?", "id": "Plastik Dengan Sifat Mekanik Unggul." },
  { "en": "Apa Contoh Plastik Rekayasa Yang Kuat?", "id": "Polikarbonat (PC), Nilon (PA), Asetal (POM)." },
  { "en": "Apa Itu Suhu Distorsi Panas (HDT)?", "id": "Suhu Saat Polimer Berdeformasi." },
  { "en": "Apa Itu Indeks Oksigen Terbatas (LOI)?", "id": "Konsentrasi Oksigen Minimum Untuk Pembakaran." },
  { "en": "Apa Itu Aditif Flame Retardant?", "id": "Zat Kimia Untuk Menghambat Pembakaran." },
  { "en": "Apa Itu Material Ablatif?", "id": "Material Yang Melindungi Dengan Menguap." },
  { "en": "Di Mana Material Ablatif Digunakan?", "id": "Pada Perisai Panas Pesawat Luar Angkasa." },
  { "en": "Apa Itu Hamburan Raman Dalam Material?", "id": "Hamburan Cahaya Tidak Elastis oleh Molekul." },
  { "en": "Apa Kegunaan Spektroskopi Raman?", "id": "Mengidentifikasi Getaran Molekul Dan Struktur." },
  { "en": "Apa Itu Efek Kerr (Efek Elektro-Optik)?", "id": "Perubahan Indeks Bias Akibat Medan Listrik." },
  { "en": "Apa Itu Efek Pockels?", "id": "Efek Kerr Linier Pada Material Tertentu." },
  { "en": "Apa Itu Efek Faraday (Efek Magneto-Optik)?", "id": "Rotasi Polarisasi Cahaya Akibat Medan Magnet." },
  { "en": "Apa Itu Konstanta Verdet?", "id": "Ukuran Kekuatan Efek Faraday Material." },
  { "en": "Apa Itu Paduan Heusler?", "id": "Paduan Feromagnetik Dari Unsur Non-Feromagnetik." },
  { "en": "Apa Itu Efek Wigner Dalam Material?", "id": "Perpindahan Atom Akibat Iradiasi Neutron." },
  { "en": "Apa Itu Efek Magnetokalorik?", "id": "Perubahan Suhu Material Akibat Medan Magnet." },
  { "en": "Apa Aplikasi Dari Efek Magnetokalorik?", "id": "Pendinginan Magnetik Tanpa Menggunakan Gas." },
  { "en": "Apa Itu Material Auxetic?", "id": "Material Dengan Rasio Poisson Negatif." },
  { "en": "Bagaimana Material Auxetic Berperilaku Saat Diregangkan?", "id": "Menjadi Lebih Tebal Di Arah Tegak Lurus." },
  { "en": "Apa Itu Aerogel?", "id": "Material Padat Sintetis Dengan Kepadatan Rendah." },
  { "en": "Apa Sifat Termal Utama Dari Aerogel?", "id": "Isolator Termal Yang Sangat Efektif." },
  { "en": "Apa Itu Gelas Fotolistrik?", "id": "Material Yang Menjadi Gelap Saat Terkena Cahaya." },
  { "en": "Apa Itu Semikonduktor Organik?", "id": "Senyawa Organik Yang Menunjukkan Sifat Semikonduktor." },
  { "en": "Apa Keuntungan Dari Semikonduktor Organik?", "id": "Fleksibel, Murah, Dan Mudah Diproses." },
  { "en": "Apa Contoh Aplikasi Semikonduktor Organik?", "id": "Layar OLED, Sel Surya Organik (OPV)." },
  { "en": "Apa Itu Polarisasi Spontann?", "id": "Polarisasi Listrik Yang Ada Tanpa Medan Listrik." },
  { "en": "Apa Itu Arus Piroelektrik?", "id": "Arus Yang Dihasilkan Akibat Perubahan Suhu." },
  { "en": "Apa Itu Kaca Self-Cleaning?", "id": "Kaca Dengan Lapisan Fotokatalitik Hidrofilik." },
  { "en": "Apa Bahan Pelapis Pada Kaca Self-Cleaning?", "id": "Titanium Dioksida (TiO2) Yang Umumnya." },
  { "en": "Apa Itu Efek Marangoni?", "id": "Transfer Massa Sepanjang Antarmuka Cairan." },
  { "en": "Apa Itu Efek Fotodielektrik?", "id": "Perubahan Konstanta Dielektrik Akibat Cahaya." },
  { "en": "Apa Itu Keramik Transparan?", "id": "Keramik Polikristalin Yang Dapat Mentransmisikan Cahaya." },
  { "en": "Apa Contoh Keramik Transparan?", "id": "Aluminium Oksinitrida (ALON), Spinel (MgAl2O4)." },
  { "en": "Apa Itu Proses Dokter Blading?", "id": "Teknik Membuat Lapisan Tipis Seragam." },
  { "en": "Apa Itu Hamburan Mie (Mie Scattering)?", "id": "Hamburan Cahaya Oleh Partikel Bulat." },
  { "en": "Apa Itu Ikatan Van Der Waals?", "id": "Gaya Tarik Antar Molekul Yang Lemah." },
  { "en": "Apa Peran Ikatan Van Der Waals?", "id": "Penting Dalam Adhesi Dan Sifat Polimer." },
  { "en": "Apa Itu Energi Kohesif Suatu Padatan?", "id": "Energi Yang Diperlukan Memisahkan Komponennya." },
  { "en": "Apa Itu Fonon Akustik?", "id": "Getaran Kisi Di Mana Atom Bergerak Searah." },
  { "en": "Apa Itu Fonon Optik?", "id": "Getaran Kisi Di Mana Atom Bergerak Berlawanan." },
  { "en": "Apa Itu Spintronik (Spintronics)?", "id": "Pemanfaatan Spin Elektron Selain Muatannya." },
  { "en": "Apa Itu Efek Giant Magnetoresistance (GMR)?", "id": "Perubahan Resistansi Besar Akibat Medan Magnet." },
  { "en": "Di Mana Efek Giant Magnetoresistance (GMR) Digunakan?", "id": "Pada Kepala Pembaca Hard Disk Drive (HDD)." },
  { "en": "Apa Itu Tunnel Magnetoresistance (TMR)?", "id": "Efek GMR Melalui Lapisan Isolator Tipis." },
  { "en": "Apa Keunggulan Tunnel Magnetoresistance (TMR) Dari GMR?", "id": "Perubahan Resistansi Yang Jauh Lebih Besar." },
  { "en": "Apa Itu Skyrmion Dalam Magnetisme?", "id": "Pola Putaran Spin Magnetik Kiral." },
  { "en": "Apa Itu Efek Rashba-Edelstein?", "id": "Konversi Arus Muatan Menjadi Akumulasi Spin." },
  { "en": "Apa Itu Efek Spin Hall?", "id": "Menghasilkan Arus Spin Dari Arus Muatan." },
  { "en": "Apa Itu Isolator Topologi?", "id": "Isolator Di Bagian Dalam, Konduktor Di Permukaan." },
  { "en": "Apa Itu Semimetal Weyl?", "id": "Material Dengan Titik Pita Energi Nol." },
  { "en": "Apa Prinsip Kerja Mikroskop Gaya Atom (AFM)?", "id": "Mengukur Interaksi Antara Ujung Runcing Permukaan." },
  { "en": "Apa Mode Operasi Utama Mikroskop Gaya Atom (AFM)?", "id": "Mode Kontak, Non-Kontak, Dan Tapping." },
  { "en": "Apa Fungsi Analisis Spektroskopi Fotoelektron Sinar-X (XPS)?", "id": "Menentukan Komposisi Unsur Dan Keadaan Kimia." },
  { "en": "Apa Itu Energi Bebas Gibbs Dalam Material?", "id": "Ukuran Stabilitas Termodinamika Suatu Fasa." },
  { "en": "Apa Ciri Transformasi Fasa Martensitik?", "id": "Transformasi Tanpa Difusi, Sangat Cepat." },
  { "en": "Apa Itu Proses Nukleasi Dan Pertumbuhan?", "id": "Mekanisme Pembentukan Fasa Baru Dari Fasa Induk." },
  { "en": "Apa Itu Kerapatan Keadaan (Density of States)?", "id": "Jumlah Keadaan Elektron Per Satuan Energi." },
  { "en": "Apa Itu Kristal Cair (Liquid Crystal)?", "id": "Fasa Di Antara Padat Kristal Dan Cair." },
  { "en": "Apa Itu Fasa Nematik Kristal Cair?", "id": "Molekul Sejajar Tetapi Tidak Berlapis." },
  { "en": "Apa Itu Fasa Smektik Kristal Cair?", "id": "Molekul Sejajar Dan Tersusun Berlapis." },
  { "en": "Apa Bahan Material Untuk Sistem Mikro-Elektro-Mekanis (MEMS)?", "id": "Silikon, Polimer, Logam, Dan Keramik." },
  { "en": "Apa Itu Fotorezistor (Photoresist)?", "id": "Material Sensitif Cahaya Untuk Proses Litografi." },
  { "en": "Apa Perbedaan Fotorezistor Positif Dan Negatif?", "id": "Kelarutan Berubah Setelah Terkena Cahaya." },
  { "en": "Apa Itu Elektromigrasi Dalam Sirkuit Terpadu?", "id": "Perpindahan Ion Logam Akibat Arus Listrik." },
  { "en": "Apa Dampak Dari Proses Elektromigrasi?", "id": "Menyebabkan Kegagalan Hubungan Singkat Atau Terbuka." },
  { "en": "Apa Itu Thermal Runaway Pada Perangkat?", "id": "Peningkatan Suhu Yang Menyebabkan Kerusakan." },
  { "en": "Apa Itu Superparamagnetisme?", "id": "Sifat Magnetik Partikel Nano Feromagnetik." },
  { "en": "Apa Itu Anisotropi Magnetokristalin?", "id": "Arah Magnetisasi Mudah Bergantung Sumbu Kristal." },
  { "en": "Apa Itu Cairan Magnetoreologikal (MR Fluid)?", "id": "Viskositas Cairan Berubah Akibat Medan Magnet." },
  { "en": "Apa Itu Komposit Matriks Logam (MMC)?", "id": "Material Penguat Dalam Matriks Berbahan Logam." },
  { "en": "Apa Keunggulan Komposit Matriks Logam (MMC)?", "id": "Kekuatan Tinggi Pada Temperatur Sangat Tinggi." },
  { "en": "Apa Itu Komposit Matriks Keramik (CMC)?", "id": "Serat Penguat Dalam Matriks Berbahan Keramik." },
  { "en": "Apa Keunggulan Komposit Matriks Keramik (CMC)?", "id": "Ketangguhan Retak Lebih Baik Dari Keramik." },
  { "en": "Apa Itu Proses Implantasi Ion?", "id": "Memasukkan Ion Ke Material Padat." },
  { "en": "Apa Tujuan Dari Proses Implantasi Ion?", "id": "Untuk Mengubah Sifat Listrik Atau Mekanik." },
  { "en": "Apa Itu Proses Laser Annealing?", "id": "Pemanasan Lokal Cepat Menggunakan Sinar Laser." },
  { "en": "Apa Itu Pemolesan Mekanis Kimiawi (CMP)?", "id": "Proses Untuk Meratakan Permukaan Wafer." },
  { "en": "Apa Itu Indeks Miller-Bravais?", "id": "Notasi Empat Indeks Untuk Kristal Heksagonal." },
  { "en": "Apa Itu Sel Wigner-Seitz?", "id": "Sel primitif Dengan Simetri Penuh Kisi." },
  { "en": "Apa Itu Zona Brillouin Pertama?", "id": "Sel Primitif Wigner-Seitz Dalam Kisi Resiprokal." },
  { "en": "Apa Itu Kisi Resiprokal (Reciprocal Lattice)?", "id": "Representasi Fourier Dari Suatu Kisi Kristal." },
  { "en": "Apa Itu Faktor Struktur Geometris?", "id": "Amplitudo Hamburan Gelombang Dari Sel Satuan." },
  { "en": "Apa Itu Paduan Intermetalik?", "id": "Paduan Dengan Struktur Kristal Teratur." },
  { "en": "Apa Sifat Umum Paduan Intermetalik?", "id": "Keras, Rapuh, Tahan Suhu Tinggi." },
  { "en": "Apa Itu Dispersi Fonon?", "id": "Hubungan Antara Frekuensi Dan Vektor Gelombang." },
  { "en": "Apa Itu Kecepatan Grup Suatu Gelombang?", "id": "Kecepatan Perambatan Energi Dari Gelombang." },
  { "en": "Apa Itu Plasmon Permukaan (Surface Plasmon)?", "id": "Osilasi Elektron Kolektif Di Permukaan Logam." },
  { "en": "Apa Itu Resonansi Plasmon Permukaan (SPR)?", "id": "Eksitasi Plasmon Permukaan Oleh Cahaya." },
  { "en": "Apa Aplikasi Dari Resonansi Plasmon Permukaan (SPR)?", "id": "Biosensor Untuk Mendeteksi Interaksi Molekuler." },
  { "en": "Apa Itu Material Termoplastik Elastomer (TPE)?", "id": "Campuran Sifat Termoplastik Dan Elastomer." },
  { "en": "Apa Itu Zeolit Dalam Ilmu Material?", "id": "Material Aluminosilikat Kristal Dengan Pori." },
  { "en": "Apa Itu Efek Elektrokalorik?", "id": "Perubahan Suhu Akibat Perubahan Medan Listrik." },
  { "en": "Apa Itu Material Elektret (Electret)?", "id": "Material Dielektrik Dengan Muatan Listrik Permanen." },
  { "en": "Apa Itu Efek Elektro-Girasi?", "id": "Perubahan Rotasi Optik Akibat Medan Listrik." },
  { "en": "Apa Itu Bahan Amorf?", "id": "Zat padat Yang Tidak Memiliki Struktur Kristal." },
  { "en": "Apa Itu Devitrifikasi Material Kaca?", "id": "Proses Kristalisasi Dalam Material Kaca." },
  { "en": "Apa Itu Koefisien Difusi?", "id": "Ukuran Laju Difusi Atom Atau Molekul." },
  { "en": "Bagaimana Suhu Mempengaruhi Koefisien Difusi?", "id": "Meningkat Secara Eksponensial Dengan Suhu." },
  { "en": "Apa Itu Energi Aktivasi Untuk Difusi?", "id": "Energi Minimum Yang Diperlukan Untuk Difusi." },
  { "en": "Apa Itu Diagram TTT (Time-Temperature-Transformation)?", "id": "Menunjukkan Transformasi Fasa Terhadap Waktu." },
  { "en": "Apa Itu Diagram CCT (Continuous Cooling Transformation)?", "id": "Diagram TTT Untuk Laju Pendinginan Kontinu." },
  { "en": "Apa Itu Baja Fasa Ganda (Dual-Phase Steel)?", "id": "Struktur Mikro Ferit Dengan Pulau Martensit." },
  { "en": "Apa Itu Efek Bauschinger?", "id": "Penurunan Batas Luluh Akibat Arah Berlawanan." },
  { "en": "Apa Itu Solvus Line Pada Diagram Fasa?", "id": "Garis Batas Kelarutan Padat Maksimum." },
  { "en": "Apa Itu Solidus Line Pada Diagram Fasa?", "id": "Garis Di Bawahnya Semua Fasa Padat." },
  { "en": "Apa Itu Liquidus Line Pada Diagram Fasa?", "id": "Garis Di Atasnya Semua Fasa Cair." },
  { "en": "Apa Itu Reaksi Peritektik?", "id": "Fasa Cair Dan Padat Bereaksi Membentuk Padat." },
  { "en": "Apa Itu Reaksi Monotektik?", "id": "Satu Fasa Cair Membentuk Padat Dan Cair Lain." },
  { "en": "Apa Itu Aturan Tuas (Lever Rule)?", "id": "Menghitung Fraksi Fasa Dalam Daerah Dua Fasa." },
  { "en": "Apa Itu Aturan Fasa Gibbs?", "id": "Menghubungkan Jumlah Fasa, Komponen, Derajat Kebebasan." },
  { "en": "Apa Itu Tekstur Kristalografi?", "id": "Orientasi Pilihan Dari Butir Kristal." },
  { "en": "Apa Itu Epitaksi Berkas Molekul (MBE)?", "id": "Metode Pertumbuhan Kristal Lapis Demi Lapis." },
  { "en": "Apa Itu Perengkahan (Debonding) Pada Komposit?", "id": "Pemisahan Antara Serat Dan Matriks." },
  { "en": "Apa Itu Delaminasi Pada Komposit Laminat?", "id": "Pemisahan Antara Lapisan Atau Lamina." },
  { "en": "Apa Itu Permitivitas Kompleks?", "id": "Menggambarkan Respons Dielektrik Dan Rugi-Rugi." },
  { "en": "Apa Bagian Imajiner Dari Permitivitas Kompleks?", "id": "Mewakili Rugi-Rugi Dielektrik Material." },
  { "en": "Apa Itu Permeabilitas Kompleks?", "id": "Menggambarkan Respons Magnetik Dan Rugi-Rugi." },
  { "en": "Apa Itu Relaksasi Debye?", "id": "Model Respons Frekuensi Polarisasi Dipol." },
  { "en": "Apa Itu Penuaan (Aging) Pada Material?", "id": "Perubahan Sifat Material Seiring Waktu." },
  { "en": "Apa Itu Pengerasan Penuaan (Age Hardening)?", "id": "Peningkatan Kekuatan Dengan Membentuk Presipitat." },
  { "en": "Apa Itu Sol Gel?", "id": "Proses Kimia Basah Untuk Membuat Oksida." },
  { "en": "Apa Itu Xerogel?", "id": "Gel Kering Dengan Struktur Tetap." },
  { "en": "Apa Itu Polariton?", "id": "Kuasi-Partikel Dari Kopling Foton-Eksitasi." },
  { "en": "Apa Itu Magnon?", "id": "Kuasi-Partikel Dari Eksitasi Spin Kolektif." },
  { "en": "Apa Itu Polaron?", "id": "Kuasi-Partikel Dari Elektron Dan Distorsi Kisi." },
  { "en": "Apa Itu Teori BCS?", "id": "Teori Superkonduktivitas Konvensional Suhu Rendah." },
  { "en": "Apa Itu Pasangan Cooper (Cooper Pair)?", "id": "Pasangan Elektron Yang Memediasi Superkonduktivitas." },
  { "en": "Apa Itu Efek Josephson?", "id": "Arus Mengalir Antara Dua Superkonduktor." },
  { "en": "Apa Itu SQUID (Superconducting Quantum Interference Device)?", "id": "Magnetometer Sangat Sensitif Berbasis Josephson." },
  { "en": "Apa Itu Superkonduktor Suhu Tinggi?", "id": "Superkonduktor Di Atas Titik Didih Nitrogen." },
  { "en": "Apa Contoh Superkonduktor Suhu Tinggi?", "id": "Yttrium Barium Copper Oxide (YBCO) Keramik Kuprat." },
  { "en": "Apa Itu Perisai Magnetik (Magnetic Shielding)?", "id": "Menggunakan Material Untuk Memblokir Medan Magnet." },
  { "en": "Material Apa Yang Baik Untuk Perisai Magnetik?", "id": "Material Permeabilitas Tinggi Seperti Mu-Metal." },
  { "en": "Apa Itu Efek Kondo?", "id": "Peningkatan Resistivitas Logam Pada Suhu Rendah." },
  { "en": "Apa Itu Fermion Berat (Heavy Fermion)?", "id": "Material Dengan Massa Efektif Elektron Besar." },
  { "en": "Apa Itu Cairan Spin Kuantum?", "id": "Keadaan Materi Dengan Spin Tidak Teratur." },
  { "en": "Apa Itu Valleytronics?", "id": "Pemanfaatan Derajat Kebebasan Lembah (Valley)." },
  { "en": "Apa Itu Efek Aharonov-Bohm?", "id": "Partikel Bermuatan Dipengaruhi Medan Elektromagnetik." },
  { "en": "Apa Itu Fraktal Dalam Struktur Material?", "id": "Pola Geometris Yang Berulang Pada Skala." },
  { "en": "Apa Itu Perkolasi Dalam Material Komposit?", "id": "Pembentukan Jalur Konduktif Berkelanjutan." },
  { "en": "Apa Itu Ambang Perkolasi (Percolation Threshold)?", "id": "Konsentrasi Minimum Untuk Membentuk Jalur." },
  { "en": "Apa Itu Self-Assembly Dalam Material?", "id": "Molekul Secara Spontan Membentuk Struktur Teratur." },
  { "en": "Apa Itu Efek Cheerios?", "id": "Kecenderungan Benda Mengapung Untuk Saling Menempel." },
  { "en": "Apa Kegunaan Notasi Kröger-Vink?", "id": "Mendeskripsikan Cacat Titik Elektrik Dalam Kristal." },
  { "en": "Apa Arti Simbol 'V' Dalam Notasi Kröger-Vink?", "id": "Menandakan Adanya Sebuah Kekosongan (Vacancy)." },
  { "en": "Apa Arti Subskrip Dalam Notasi Kröger-Vink?", "id": "Menunjukkan Posisi Kisi Tempat Cacat Berada." },
  { "en": "Apa Arti Superskrip Dalam Notasi Kröger-Vink?", "id": "Menunjukkan Muatan Efektif Dari Cacat." },
  { "en": "Apa Yang Dianalisis Difraksi Elektron Energi Rendah (LEED)?", "id": "Struktur Permukaan Material Kristal Tunggal." },
  { "en": "Apa Prinsip Dasar Spektroskopi Elektron Auger (AES)?", "id": "Analisis Energi Elektron Auger Yang Dipancarkan." },
  { "en": "Apa Informasi Yang Diberikan Spektroskopi Elektron Auger (AES)?", "id": "Komposisi Unsur Pada Permukaan Material." },
  { "en": "Apa Itu Material Kaca-Keramik (Glass-Ceramic)?", "id": "Material Polikristalin Yang Dibuat Dari Kaca." },
  { "en": "Apa Itu Teori Fungsional Kerapatan (DFT)?", "id": "Metode Komputasi Untuk Menghitung Struktur Elektronik." },
  { "en": "Apa Itu Simulasi Dinamika Molekuler (MD)?", "id": "Simulasi Komputer Interaksi Dan Gerak Atom." },
  { "en": "Apa Itu Fenomena Kumis Timah (Tin Whiskers)?", "id": "Pertumbuhan Filamen Konduktif Spontan Dari Timah." },
  { "en": "Apa Masalah Yang Disebabkan Kumis Timah (Tin Whiskers)?", "id": "Dapat Menyebabkan Hubungan Singkat Pada Elektronik." },
  { "en": "Apa Itu Wabah Ungu (Purple Plague)?", "id": "Senyawa Intermetalik Emas-Aluminium Yang Rapuh." },
  { "en": "Di Mana Wabah Ungu (Purple Plague) Terjadi?", "id": "Pada Sambungan Ikatan Kawat Emas-Aluminium." },
  { "en": "Apa Itu Celah Pita Fotonik (Photonic Bandgap)?", "id": "Rentang Frekuensi Cahaya Yang Tidak Dapat Merambat." },
  { "en": "Apa Sifat Material Indeks Refraksi Negatif?", "id": "Membelokkan Cahaya Ke Arah Yang Berlawanan." },
  { "en": "Apa Itu Rekonstruksi Permukaan (Surface Reconstruction)?", "id": "Atom Permukaan Mengatur Ulang Posisi." },
  { "en": "Apa Itu Keadaan Permukaan (Surface States)?", "id": "Keadaan Elektronik Yang Terlokalisasi Di Permukaan." },
  { "en": "Apa Itu Tribologi Dalam Ilmu Material?", "id": "Studi Tentang Gesekan, Keausan, Dan Pelumasan." },
  { "en": "Apa Itu Keausan Adhesif (Adhesive Wear)?", "id": "Transfer Material Antara Dua Permukaan Kontak." },
  { "en": "Apa Itu Keausan Abrasif (Abrasive Wear)?", "id": "Goresan Material Oleh Partikel Keras." },
  { "en": "Apa Itu Suhu Debye?", "id": "Suhu Terkait Frekuensi Getaran Tertinggi Kristal." },
  { "en": "Apa Itu Parameter Grüneisen?", "id": "Menjelaskan Efek Perubahan Volume Pada Getaran." },
  { "en": "Apa Itu Sumur Kuantum (Quantum Well)?", "id": "Struktur Yang Membatasi Gerak Partikel Satu Dimensi." },
  { "en": "Apa Itu Kawat Kuantum (Quantum Wire)?", "id": "Membatasi Gerak Partikel Dalam Dua Dimensi." },
  { "en": "Apa Itu Titik Kuantum (Quantum Dot)?", "id": "Membatasi Gerak Partikel Dalam Tiga Dimensi." },
  { "en": "Apa Itu Superkisi (Superlattice) Semikonduktor?", "id": "Struktur Periodik Dari Lapisan Material Berbeda." },
  { "en": "Apa Itu Gas Elektron Dua Dimensi (2DEG)?", "id": "Elektron Terkurung Bergerak Dalam Dua Dimensi." },
  { "en": "Dimana Gas Elektron Dua Dimensi (2DEG) Ditemukan?", "id": "Pada Antarmuka Heterostruktur Semikonduktor." },
  { "en": "Apa Itu Efek Stark Terkurung Kuantum (QCSE)?", "id": "Pergeseran Tingkat Energi Akibat Medan Listrik." },
  { "en": "Apa Itu Elektron Panas (Hot Electron)?", "id": "Elektron Dengan Energi Kinetik Jauh Lebih Tinggi." },
  { "en": "Apa Itu Dispersi Kromatik Dalam Serat Optik?", "id": "Pelebaran Pulsa Cahaya Akibat Perbedaan Kecepatan." },
  { "en": "Apa Itu Serat Penjaga Polarisasi (PM Fiber)?", "id": "Serat Optik Yang Mempertahankan Polarisasi Cahaya." },
  { "en": "Apa Itu Efek Fotorefraktif?", "id": "Perubahan Indeks Bias Material Akibat Cahaya." },
  { "en": "Apa Itu Soliton Optik?", "id": "Pulsa Cahaya Yang Menjaga Bentuknya Saat Merambat." },
  { "en": "Apa Itu Saturable Absorption?", "id": "Penyerapan Cahaya Menurun Dengan Intensitas Tinggi." },
  { "en": "Apa Itu Z-Scan Technique?", "id": "Metode Mengukur Sifat Optik Nonlinier Material." },
  { "en": "Apa Itu Pembelahan Titik Nol (Zero-Point Splitting)?", "id": "Pemisahan Tingkat Energi Bahkan Tanpa Medan." },
  { "en": "Apa Itu Aturan Seleksi Dalam Transisi Kuantum?", "id": "Aturan Yang Menentukan Transisi Yang Diizinkan." },
  { "en": "Apa Itu Kaca Logam (Metallic Glass)?", "id": "Logam Dengan Struktur Amorf Tidak Beraturan." },
  { "en": "Bagaimana Sifat Mekanik Kaca Logam?", "id": "Kekuatan Sangat Tinggi Namun Rapuh." },
  { "en": "Apa Itu Proses Melt Spinning?", "id": "Metode Pendinginan Cepat Membuat Paduan Amorf." },
  { "en": "Apa Itu Kalorimetri Pemindaian Diferensial (DSC)?", "id": "Mengukur Aliran Panas Terkait Transisi Material." },
  { "en": "Apa Informasi Yang Diperoleh Dari Analisis DSC?", "id": "Suhu Transisi Gelas (Tg), Titik Leleh." },
  { "en": "Apa Itu Analisis Termogravimetri (TGA)?", "id": "Mengukur Perubahan Massa Material Terhadap Suhu." },
  { "en": "Apa Itu Analisis Mekanis Dinamis (DMA)?", "id": "Mengukur Sifat Mekanik Material Terhadap Waktu." },
  { "en": "Apa Itu Modulus Penyimpanan (Storage Modulus)?", "id": "Mewakili Bagian Elastis Dari Material." },
  { "en": "Apa Itu Modulus Kerugian (Loss Modulus)?", "id": "Mewakili Bagian Viskos Dari Material." },
  { "en": "Apa Itu Koefisien Gesekan (Coefficient of Friction)?", "id": "Rasio Antara Gaya Gesek Dan Gaya Normal." },
  { "en": "Apa Perbedaan Gesekan Statis Dan Kinetis?", "id": "Gesekan Statis Lebih Besar Dari Kinetis." },
  { "en": "Apa Itu Pelumasan Hidrodinamik?", "id": "Permukaan Dipisahkan Sepenuhnya Oleh Lapisan Fluida." },
  { "en": "Apa Itu Pelumasan Batas (Boundary Lubrication)?", "id": "Lapisan Pelumas Sangat Tipis Menempel Permukaan." },
  { "en": "Apa Itu Aditif Extreme Pressure (EP)?", "id": "Aditif Pelumas Yang Aktif Pada Tekanan Tinggi." },
  { "en": "Apa Itu Cermet?", "id": "Komposit Partikel Keramik Dalam Matriks Logam." },
  { "en": "Apa Contoh Material Cermet?", "id": "Tungsten Karbida-Kobalt (WC-Co) Untuk Alat Potong." },
  { "en": "Apa Itu Busa Logam (Metal Foam)?", "id": "Logam Dengan Struktur Selular Berpori." },
  { "en": "Apa Sifat Utama Busa Logam?", "id": "Sangat Ringan Dan Mampu Menyerap Energi." },
  { "en": "Apa Itu Bahan Fungsional Bertingkat (FGM)?", "id": "Komposisi Dan Struktur Material Berubah Bertahap." },
  { "en": "Apa Itu Dislokasi Parsial Shockley?", "id": "Dislokasi Yang Memisahkan Cacat Tumpukan." },
  { "en": "Apa Itu Kembaran (Twinning) Dalam Kristal?", "id": "Deformasi Dengan Orientasi Kristal Cermin." },
  { "en": "Apa Itu Mekanisme Penguatan Orowan?", "id": "Dislokasi Melengkung Di Sekitar Partikel Keras." },
  { "en": "Apa Itu Efek Portevin-Le Chatelier (PLC)?", "id": "Deformasi Tidak Stabil Yang Bergerigi." },
  { "en": "Apa Itu Paduan Magnesium?", "id": "Logam Struktural Paling Ringan Yang Ada." },
  { "en": "Apa Kelemahan Utama Paduan Magnesium?", "id": "Reaktivitas Tinggi Dan Ketahanan Korosi Rendah." },
  { "en": "Apa Itu Paduan Berilium?", "id": "Sangat Ringan, Kaku, Dan Stabil Termal." },
  { "en": "Apa Itu Zintl Phase?", "id": "Senyawa Intermetalik Dengan Ikatan Campuran Ionik-Kovalen." },
  { "en": "Apa Itu Semikonduktor Magnetik Terdilusi (DMS)?", "id": "Semikonduktor Didoping Dengan Ion Magnetik." },
  { "en": "Apa Itu Efek Magnetoresistif Kolosal (CMR)?", "id": "Perubahan Resistansi Sangat Besar Akibat Magnet." },
  { "en": "Apa Itu Polaritasi Kisi (Lattice Polarity)?", "id": "Orientasi Atom Dalam Kristal Non-Sentrosimetris." },
  { "en": "Apa Itu Koefisien Peltier?", "id": "Ukuran Panas Yang Diserap Atau Dilepaskan." },
  { "en": "Apa Itu Koefisien Thomson?", "id": "Pemanasan Atau Pendinginan Konduktor Berarus." },
  { "en": "Apa Itu Logam Tanah Jarang (REE)?", "id": "Sekelompok 17 Unsur Kimia Metalik." },
  { "en": "Mengapa Logam Tanah Jarang Penting?", "id": "Digunakan Dalam Magnet Kuat, Laser, Katalis." },
  { "en": "Apa Itu Amalgam?", "id": "Paduan Logam Yang Mengandung Merkuri (Raksa)." },
  { "en": "Apa Itu Deviasi Stoikiometri?", "id": "Senyawa Kristal Dengan Rasio Unsur Bervariasi." },
  { "en": "Apa Itu Senyawa Non-Stoikiometri?", "id": "Senyawa Padat Dengan Rasio Atom Bervariasi." },
  { "en": "Apa Itu Kondensat Bose-Einstein (BEC)?", "id": "Keadaan Materi Pada Suhu Sangat Rendah." },
  { "en": "Apa Itu Efek Jahn-Teller?", "id": "Distorsi Geometris Untuk Menurunkan Energi." },
  { "en": "Apa Itu Transisi Peierls?", "id": "Distorsi Rantai Atom Satu Dimensi." },
  { "en": "Apa Itu Orbital Molekul Hibrida?", "id": "Kombinasi Orbital Atom Membentuk Ikatan." },
  { "en": "Apa Itu Sudut Ikatan (Bond Angle)?", "id": "Sudut Antara Dua Ikatan Kovalen Berdekatan." },
  { "en": "Apa Itu Panjang Ikatan (Bond Length)?", "id": "Jarak Keseimbangan Antara Inti Dua Atom." },
  { "en": "Apa Itu Energi Ikatan (Bond Energy)?", "id": "Energi Yang Dibutuhkan Untuk Memutuskan Ikatan." },
  { "en": "Apa Itu Elektronegativitas?", "id": "Kecenderungan Atom Untuk Menarik Elektron." },
  { "en": "Apa Itu Momen Dipol Listrik?", "id": "Ukuran Pemisahan Muatan Positif Dan Negatif." },
  { "en": "Apa Itu Bilangan Koordinasi Dalam Kristal?", "id": "Jumlah Tetangga Terdekat Suatu Atom." },
  { "en": "Apa Itu Jari-Jari Ionik?", "id": "Ukuran Radius Sebuah Ion Dalam Kristal." },
  { "en": "Apa Itu Energi Kisi (Lattice Energy)?", "id": "Energi Yang Dilepaskan Saat Ion Membentuk Padatan." },
  { "en": "Apa Itu Siklus Born-Haber?", "id": "Siklus Termodinamika Untuk Menghitung Energi Kisi." },
  { "en": "Apa Itu Kaca Spiner (Spin Glass)?", "id": "Sistem Magnetik Dengan Interaksi Yang Frustrasi." },
  { "en": "Apa Itu Frustrasi Geometris?", "id": "Ketidakmampuan Sistem Untuk Memenuhi Semua Interaksi." },
  { "en": "Apa Itu Es Spin (Spin Ice)?", "id": "Material Dengan Entropi Sisa Mirip Es Air." },
  { "en": "Apa Itu Monopol Magnetik Emergen?", "id": "Eksitasi Yang Berperilaku Seperti Monopol Magnetik." },
  { "en": "Apa Itu Metode Bridgman-Stockbarger?", "id": "Metode Pertumbuhan Kristal Dengan Pendinginan Terarah." },
  { "en": "Apa Prinsip Proses Czochralski (CZ)?", "id": "Menarik Ingot Kristal Tunggal Dari Lelehan." },
  { "en": "Apa Itu Metode Silikon Zona Apung (Float-Zone)?", "id": "Pemurnian Batang Silikon Dengan Zona Lelehan." },
  { "en": "Apa Keunggulan Silikon Zona Apung (Float-Zone)?", "id": "Menghasilkan Silikon Dengan Kemurnian Sangat Tinggi." },
  { "en": "Apa Itu Pemurnian Zona (Zone Refining)?", "id": "Memindahkan Pengotor Ke Ujung Batang Material." },
  { "en": "Apa Itu Rekombinasi Auger?", "id": "Energi Rekombinasi Diberikan Ke Elektron Ketiga." },
  { "en": "Apa Dampak Rekombinasi Auger Pada LED?", "id": "Menurunkan Efisiensi Pada Arus Yang Tinggi." },
  { "en": "Apa Itu Efek Purcell Dalam Optik?", "id": "Peningkatan Laju Emisi Spontan Suatu Sumber." },
  { "en": "Apa Itu Polimer Self-Healing?", "id": "Polimer Yang Dapat Memperbaiki Kerusakan Sendiri." },
  { "en": "Apa Itu Disklinsi (Disclination) Dalam Kristal?", "id": "Cacat Rotasi Dalam Susunan Atom Kristal." },
  { "en": "Apa Itu Pita Lüders (Lüders Bands)?", "id": "Pita Deformasi Lokal Pada Logam Tertentu." },
  { "en": "Apa Itu Kerapuhan Cairan Kaca (Glass Fragility)?", "id": "Ukuran Perubahan Viskositas Dekat Suhu Transisi." },
  { "en": "Apa Itu Peniti Fluks (Flux Pinning)?", "id": "Cacat Yang Menjebak Garis Fluks Magnetik." },
  { "en": "Mengapa Peniti Fluks Penting Untuk Superkonduktor?", "id": "Memungkinkan Superkonduktor Membawa Arus Tinggi." },
  { "en": "Apa Itu Panjang Koherensi Superkonduktor?", "id": "Jarak Spasial Rata-Rata Pasangan Cooper." },
  { "en": "Apa Itu Kedalaman Penetrasi London?", "id": "Jarak Tembus Medan Magnet Ke Superkonduktor." },
  { "en": "Apa Ciri Superkonduktor Tak Konvensional?", "id": "Mekanisme Berpasangan Bukan Akibat Getaran Kisi." },
  { "en": "Apa Itu Efek Bias Tukar (Exchange Bias)?", "id": "Pergeseran Kurva Histeresis Magnetik Pada Antarmuka." },
  { "en": "Apa Itu Torsi Transfer Spin (STT)?", "id": "Memanipulasi Magnetisasi Menggunakan Arus Spin." },
  { "en": "Apa Aplikasi Torsi Transfer Spin (STT)?", "id": "Memori Akses Acak Magnetoresistif (MRAM)." },
  { "en": "Apa Itu Spektroskopi Kehilangan Energi Elektron (EELS)?", "id": "Menganalisis Kehilangan Energi Elektron Tidak Elastis." },
  { "en": "Apa Itu Difraksi Hamburan Balik Elektron (EBSD)?", "id": "Teknik Untuk Mengukur Orientasi Kristal." },
  { "en": "Apa Itu Tingkat Landau?", "id": "Tingkat Energi Terkuantisasi Dalam Medan Magnet." },
  { "en": "Apa Itu Efek Hall Kuantum Integer (IQHE)?", "id": "Kuantisasi Presisi Resistansi Hall Dalam 2D." },
  { "en": "Apa Itu Efek Hall Kuantum Fraksional (FQHE)?", "id": "Kuantisasi Resistansi Hall Pada Fraksi Tertentu." },
  { "en": "Apa Itu Isolator Mott?", "id": "Material Yang Seharusnya Konduktor Namun Isolator." },
  { "en": "Mengapa Isolator Mott Bersifat Isolator?", "id": "Akibat Interaksi Tolak-Menolak Kuat Antar Elektron." },
  { "en": "Apa Itu Model Hubbard?", "id": "Model Sederhana Untuk Menggambarkan Interaksi Elektron." },
  { "en": "Apa Itu Suhu Kondo?", "id": "Skala Suhu Karakteristik Untuk Efek Kondo." },
  { "en": "Apa Itu Fermion Majorana Dalam Material?", "id": "Partikel Yang Merupakan Antipartikelnya Sendiri." },
  { "en": "Apa Itu Spektroskopi Terowongan Pemindaian (STM)?", "id": "Pencitraan Permukaan Berdasarkan Arus Terowongan Kuantum." },
  { "en": "Apa Itu Difraksi Neutron?", "id": "Teknik Untuk Menentukan Struktur Magnetik Material." },
  { "en": "Apa Itu Kaca Borosilikat?", "id": "Kaca Tahan Panas Dengan Koefisien Ekspansi Rendah." },
  { "en": "Apa Itu Kaca Aluminosilikat?", "id": "Kaca Dengan Kekuatan Dan Kekerasan Tinggi." },
  { "en": "Apa Itu Efek Fotolistrik Internal?", "id": "Pembangkitan Pembawa Muatan Dalam Semikonduktor." },
  { "en": "Apa Itu Polarisasi Gradien (Flexoelectricity)?", "id": "Timbulnya Polarisasi Listrik Akibat Gradien Regangan." },
  { "en": "Apa Itu Kristal Plastik (Plastic Crystal)?", "id": "Padatan Dengan Orientasi Molekul Tidak Teratur." },
  { "en": "Apa Itu Solvasi Elektron?", "id": "Elektron Terlarut Yang Distabilkan Oleh Molekul Pelarut." },
  { "en": "Apa Itu Teori Landau-Ginzburg?", "id": "Teori Fenomenologis Untuk Transisi Fasa Superkonduktor." },
  { "en": "Apa Itu Parameter Orde (Order Parameter)?", "id": "Ukuran Kuantitatif Tingkat Keteraturan Sistem." },
  { "en": "Apa Itu Kaca Dipol (Dipole Glass)?", "id": "Sistem Dengan Dipol Listrik Membeku Acak." },
  { "en": "Apa Itu Mekanisme Interkalasi?", "id": "Penyisipan Reversibel Ion Ke Dalam Material Induk." },
  { "en": "Di Mana Mekanisme Interkalasi Penting?", "id": "Pada Elektroda Baterai Isi Ulang Lithium-Ion." },
  { "en": "Apa Itu Dendrit Dalam Baterai?", "id": "Struktur Mirip Jarum Yang Dapat Menyebabkan Korsleting." },
  { "en": "Apa Itu Antarmuka Elektrolit Padat (SEI)?", "id": "Lapisan Pasivasi Yang Terbentuk Pada Anoda." },
  { "en": "Apa Itu Elektrolit Polimer?", "id": "Polimer Padat Yang Dapat Menghantarkan Ion." },
  { "en": "Apa Itu Cairan Ionik (Ionic Liquid)?", "id": "Garam Yang Berada Dalam Keadaan Cair." },
  { "en": "Apa Itu Efek Wien?", "id": "Peningkatan Konduktivitas Elektrolit Pada Medan Kuat." },
  { "en": "Apa Itu Efek Debye-Falkenhagen?", "id": "Peningkatan Konduktivitas Pada Frekuensi Sangat Tinggi." },
  { "en": "Apa Itu Bilangan Transport Ion?", "id": "Fraksi Arus Total Yang Dibawa Ion." },
  { "en": "Apa Itu Elektrometalurgi?", "id": "Penggunaan Listrik Untuk Mengekstraksi Dan Memurnikan Logam." },
  { "en": "Apa Itu Proses Hall-Héroult?", "id": "Proses Elektrolisis Untuk Produksi Aluminium." },
  { "en": "Apa Itu Paduan Amalgam?", "id": "Paduan Merkuri Dengan Satu Logam Lainnya." },
  { "en": "Apa Itu Kavitasi Dalam Cairan?", "id": "Pembentukan Gelembung Uap Dalam Cairan." },
  { "en": "Apa Itu Erosi Kavitasi?", "id": "Kerusakan Material Akibat Pecahnya Gelembung Kavitasi." },
  { "en": "Apa Itu Efek Rehbinder?", "id": "Penurunan Kekuatan Padatan Akibat Adsorpsi." },
  { "en": "Apa Itu Penggetasan Hidrogen (Hydrogen Embrittlement)?", "id": "Logam Menjadi Rapuh Akibat Penyerapan Hidrogen." },
  { "en": "Apa Itu Serat Basal (Basalt Fiber)?", "id": "Serat Mineral Mirip Kaca Dari Batuan Basal." },
  { "en": "Apa Itu Material Cerdas (Smart Material)?", "id": "Material Yang Merespons Perubahan Lingkungan." },
  { "en": "Apa Itu Rheologi?", "id": "Studi Tentang Aliran Dan Deformasi Materi." },
  { "en": "Apa Itu Fluida Newtonian?", "id": "Fluida Dengan Viskositas Konstan Terhadap Tegangan." },
  { "en": "Apa Itu Fluida Non-Newtonian?", "id": "Viskositas Berubah Bergantung Pada Tegangan Geser." },
  { "en": "Apa Itu Efek Pengenceran Geser (Shear Thinning)?", "id": "Viskositas Menurun Dengan Peningkatan Laju Geser." },
  { "en": "Apa Itu Efek Penebalan Geser (Shear Thickening)?", "id": "Viskositas Meningkat Dengan Peningkatan Laju Geser." },
  { "en": "Apa Itu Tegangan Luluh (Yield Stress)?", "id": "Tegangan Minimum Yang Diperlukan Agar Mengalir." },
  { "en": "Apa Itu Superplastisitas?", "id": "Kemampuan Material Mengalami Deformasi Sangat Besar." },
  { "en": "Apa Itu Kaca Metalik Curah (BMG)?", "id": "Kaca Logam Yang Dibuat Dalam Ukuran Besar." },
  { "en": "Apa Itu Bilangan Kuantum Topologi?", "id": "Besaran Terkuantisasi Yang Menggambarkan Fase Topologi." },
  { "en": "Apa Itu Semilogam Dirac?", "id": "Material Dengan Dispersi Elektron Linier Tiga Dimensi." },
  { "en": "Apa Itu Efek Hall Spin Kuantum?", "id": "Versi Efek Spin Hall Yang Terkuantisasi." },
  { "en": "Apa Itu Efek Hall Anomali?", "id": "Efek Hall Yang Muncul Pada Material Feromagnetik." },
  { "en": "Apa Itu Teorema Bloch?", "id": "Menjelaskan Bentuk Fungsi Gelombang Dalam Kristal." },
  { "en": "Apa Itu Model Elektron Hampir Bebas?", "id": "Model Struktur Pita Dengan Potensial Lemah." },
  { "en": "Apa Itu Model Ikatan Kuat (Tight-Binding)?", "id": "Model Struktur Pita Berbasis Orbital Atom." },
  { "en": "Apa Itu Hibridisasi Orbital?", "id": "Pencampuran Orbital Atom Menjadi Orbital Hibrida." },
  { "en": "Apa Itu Ikatan Pi (π Bond)?", "id": "Ikatan Kovalen Dengan Tumpang Tindih Orbital Samping." },
  { "en": "Apa Itu Ikatan Sigma (σ Bond)?", "id": "Ikatan Kovalen Dengan Tumpang Tindih Ujung-ke-Ujung." },
  { "en": "Apa Itu Aturan Hume-Rothery?", "id": "Aturan Yang Menentukan Kelarutan Padat Substitusi." },
  { "en": "Apa Itu Transisi Logam-Isolator?", "id": "Transisi Fasa Dari Konduktor Menjadi Isolator." },
  { "en": "Apa Itu Model Ising?", "id": "Model Matematis Untuk Menjelaskan Feromagnetisme." },
  { "en": "Apa Itu Temperatur Kritis?", "id": "Suhu Di Atasnya Fasa Berbeda Tidak Ada." },
  { "en": "Apa Itu Eksponen Kritis?", "id": "Menggambarkan Perilaku Besaran Fisik Dekat Titik Kritis." },
  { "en": "Apa Itu Grup Renormalisasi?", "id": "Teknik Matematis Untuk Menganalisis Transisi Fasa." },
  { "en": "Apa Itu Critical Slowing Down?", "id": "Pelambatan Dinamika Sistem Dekat Titik Kritis." },
  { "en": "Apa Itu Universalitas Dalam Fisika Statistik?", "id": "Sistem Berbeda Menunjukkan Perilaku Kritis Sama." },
  { "en": "Apa Itu Teori Medan Rata-Rata (Mean Field)?", "id": "Pendekatan Sederhana Untuk Memodelkan Interaksi Partikel." },
  { "en": "Apa Itu Efek Jahn-Teller Kooperatif?", "id": "Distorsi Jahn-Teller Yang Teratur Dalam Kristal." },
  { "en": "Apa Itu Orde Muatan (Charge Ordering)?", "id": "Pengaturan Teratur Keadaan Muatan Elektron." },
  { "en": "Apa Itu Orde Orbital (Orbital Ordering)?", "id": "Pengaturan Teratur Orbital Elektron Yang Terisi." },
  { "en": "Apa Itu Paduan Zirkonium (Zircaloy)?", "id": "Paduan Zirkonium Tahan Korosi Untuk Reaktor Nuklir." },
  { "en": "Apa Itu Kerusakan Geser (Shear Banding)?", "id": "Lokalisasi Regangan Geser Yang Intens." },
  { "en": "Apa Itu Diagram Ashby?", "id": "Plot Yang Memetakan Properti Material Berbeda." },
  { "en": "Apa Kegunaan Diagram Ashby?", "id": "Untuk Seleksi Material Dalam Desain Rekayasa." },
  { "en": "Apa Itu Indentasi Nano (Nanoindentation)?", "id": "Teknik Mengukur Sifat Mekanik Skala Nano." }



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
