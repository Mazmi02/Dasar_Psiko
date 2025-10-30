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


  {
    "en": "Apa Itu Psikologi?",
    "id": "Ilmu Tentang Jiwa Dan Perilaku."
  },
  {
    "en": "Apa Itu Psikotes?",
    "id": "Tes Mengukur Aspek Psikologis Individu."
  },
  {
    "en": "Siapa Bapak Psikologi Modern?",
    "id": "Wilhelm Wundt Adalah Bapak Psikologi."
  },
  {
    "en": "Apa Itu Kepribadian?",
    "id": "Pola Khas Pikiran, Perasaan, Perilaku."
  },
  {
    "en": "Apa Itu IQ?",
    "id": "Singkatan Dari Intelligence Quotient."
  },
  {
    "en": "Apa Fungsi Utama Psikotes?",
    "id": "Mengevaluasi Potensi Dan Kompetensi Individu."
  },
  {
    "en": "Apa Itu Emosi?",
    "id": "Reaksi Afektif Terhadap Suatu Rangsangan."
  },
  {
    "en": "Apa Itu Memori?",
    "id": "Kemampuan Menyimpan Dan Mengingat Informasi."
  },
  {
    "en": "Apa Itu Kognisi?",
    "id": "Proses Mental Memperoleh Pengetahuan."
  },
  {
    "en": "Apa Itu Tes Wartegg?",
    "id": "Tes Psikologi Menggunakan Stimulus Gambar."
  },
  {
    "en": "Apa Itu Tes Kraepelin?",
    "id": "Tes Psikologi Mengukur Kecepatan Kerja."
  },
  {
    "en": "Apa Itu Psikoanalisis?",
    "id": "Teori Freud Tentang Ketidaksadaran."
  },
  {
    "en": "Siapa Sigmund Freud?",
    "id": "Pendiri Aliran Psikoanalisis."
  },
  {
    "en": "Siapa Carl Jung?",
    "id": "Psikolog Analitis Terkenal Dari Swiss."
  },
  {
    "en": "Apa Itu Introvert?",
    "id": "Tipe Kepribadian Fokus Pada Internal."
  },
  {
    "en": "Apa Itu Ekstrovert?",
    "id": "Tipe Kepribadian Fokus Pada Eksternal."
  },
  {
    "en": "Apa Itu Persepsi?",
    "id": "Proses Menafsirkan Informasi Sensorik."
  },
  {
    "en": "Apa Itu Belajar (Learning)?",
    "id": "Perubahan Perilaku Akibat Pengalaman."
  },
  {
    "en": "Apa Itu Motivasi?",
    "id": "Dorongan Internal Untuk Bertindak."
  },
  {
    "en": "Apa Itu Stres?",
    "id": "Respon Tubuh Terhadap Tekanan."
  },
  {
    "en": "Apa Itu Tes Pauli?",
    "id": "Tes Menghitung Angka Mirip Kraepelin."
  },
  {
    "en": "Apa Itu Tes MBTI?",
    "id": "Tes Indikator Tipe Kepribadian."
  },
  {
    "en": "Apa Itu Tes DISC?",
    "id": "Tes Mengukur Perilaku Dominan."
  },
  {
    "en": "Apa Itu EQ?",
    "id": "Singkatan Dari Emotional Quotient."
  },
  {
    "en": "Apa Itu Psikologi Klinis?",
    "id": "Cabang Psikologi Mendiagnosis Gangguan Mental."
  },
  {
    "en": "Apa Itu Psikologi Sosial?",
    "id": "Studi Interaksi Individu Dan Kelompok."
  },
  {
    "en": "Apa Itu Psikologi Industri?",
    "id": "Psikologi Dalam Konteks Tempat Kerja."
  },
  {
    "en": "Apa Itu Bipolar?",
    "id": "Gangguan Suasana Hati Ekstrem."
  },
  {
    "en": "Apa Itu Skizofrenia?",
    "id": "Gangguan Mental Realitas Terdistorsi."
  },
  {
    "en": "Apa Itu Fobia?",
    "id": "Rasa Takut Berlebihan Pada Sesuatu."
  },
  {
    "en": "Apa Itu Tes IST?",
    "id": "Tes Mengukur Struktur Kecerdasan."
  },
  {
    "en": "Apa Itu TPA?",
    "id": "Singkatan Tes Potensi Akademik."
  },
  {
    "en": "Apa Itu Tes BAUM?",
    "id": "Tes Menggambar Pohon."
  },
  {
    "en": "Apa Itu Tes DAP?",
    "id": "Tes Menggambar Orang (Draw A Person)."
  },
  {
    "en": "Apa Itu Tes HTP?",
    "id": "Tes Menggambar Rumah, Pohon, Orang."
  },
  {
    "en": "Apa Arti Validitas Tes?",
    "id": "Sejauh Mana Tes Mengukur Seharusnya."
  },
  {
    "en": "Apa Arti Reliabilitas Tes?",
    "id": "Sejauh Mana Tes Konsisten."
  },
  {
    "en": "Apa Itu Norma Tes?",
    "id": "Standar Perbandingan Skor Tes."
  },
  {
    "en": "Apa Itu Id Dalam Psikoanalisis?",
    "id": "Aspek Kepribadian Pendorong Insting."
  },
  {
    "en": "Apa Itu Ego Dalam Psikoanalisis?",
    "id": "Aspek Kepribadian Pengendali Realitas."
  },
  {
    "en": "Apa Itu Superego Dalam Psikoanalisis?",
    "id": "Aspek Kepribadian Berisi Moral."
  },
  {
    "en": "Apa Itu Alam Bawah Sadar?",
    "id": "Pikiran, Perasaan Yang Tidak Disadari."
  },
  {
    "en": "Apa Itu Behaviorisme?",
    "id": "Aliran Psikologi Fokus Perilaku."
  },
  {
    "en": "Siapa Ivan Pavlov?",
    "id": "Ilmuwan Dikenal Teori Pengkondisian Klasik."
  },
  {
    "en": "Siapa B.F. Skinner?",
    "id": "Psikolog Teori Pengkondisian Operan."
  },
  {
    "en": "Apa Itu Psikologi Humanistik?",
    "id": "Aliran Fokus Potensi Positif Manusia."
  },
  {
    "en": "Siapa Abraham Maslow?",
    "id": "Pencetus Teori Hierarki Kebutuhan."
  },
  {
    "en": "Siapa Carl Rogers?",
    "id": "Tokoh Penting Psikologi Humanistik."
  },
  {
    "en": "Apa Itu Psikologi Kognitif?",
    "id": "Studi Tentang Proses Mental Internal."
  },
  {
    "en": "Apa Itu Inteligensi?",
    "id": "Kemampuan Berpikir, Belajar, Adaptasi."
  },
  {
    "en": "Apa Itu Bakat?",
    "id": "Potensi Bawaan Untuk Keterampilan Tertentu."
  },
  {
    "en": "Apa Itu Minat?",
    "id": "Kecenderungan Menyukai Suatu Aktivitas."
  },
  {
    "en": "Apa Itu Tes Rorschach?",
    "id": "Tes Kepribadian Menggunakan Bintik Tinta."
  },
  {
    "en": "Apa Itu Tes TAT?",
    "id": "Tes Proyektif Menggunakan Gambar Adegan."
  },
  {
    "en": "Apa Itu Observasi?",
    "id": "Metode Pengumpulan Data Lewat Pengamatan."
  },
  {
    "en": "Apa Itu Wawancara?",
    "id": "Metode Pengumpulan Data Lewat Tanya Jawab."
  },
  {
    "en": "Apa Itu Kuesioner?",
    "id": "Alat Pengumpulan Data Berisi Pertanyaan."
  },
  {
    "en": "Apa Itu Psikopat?",
    "id": "Individu Dengan Gangguan Kepribadian Antisosial."
  },
  {
    "en": "Apa Itu Narsistik?",
    "id": "Kepribadian Terlalu Mencintai Diri Sendiri."
  },
  {
    "en": "Apa Itu Depresi?",
    "id": "Gangguan Suasana Hati Sedih Mendalam."
  },
  {
    "en": "Apa Itu Kecemasan?",
    "id": "Perasaan Khawatir Berlebihan Dan Intens."
  },
  {
    "en": "Apa Itu Psikologi Pendidikan?",
    "id": "Cabang Psikologi Fokus Proses Belajar."
  },
  {
    "en": "Apa Itu Konseling?",
    "id": "Proses Bantuan Profesional Mengatasi Masalah."
  },
  {
    "en": "Apa Itu Psikoterapi?",
    "id": "Terapi Untuk Masalah Psikologis."
  },
  {
    "en": "Apa Itu Logika?",
    "id": "Ilmu Tentang Penalaran Yang Sah."
  },
  {
    "en": "Apa Itu Analogi?",
    "id": "Persamaan Hubungan Antara Dua Hal."
  },
  {
    "en": "Apa Itu Silogisme?",
    "id": "Penarikan Kesimpulan Dari Dua Premis."
  },
  {
    "en": "Apa Itu Tes Penalaran Verbal?",
    "id": "Tes Mengukur Kemampuan Bahasa."
  },
  {
    "en": "Apa Itu Tes Penalaran Numerik?",
    "id": "Tes Mengukur Kemampuan Angka."
  },
  {
    "en": "Apa Itu Tes Penalaran Logis?",
    "id": "Tes Mengukur Kemampuan Berpikir Runtut."
  },
  {
    "en": "Apa Itu Tes Spasial?",
    "id": "Tes Mengukur Kemampuan Persepsi Ruang."
  },
  {
    "en": "Apa Itu Deret Angka?",
    "id": "Pola Urutan Angka."
  },
  {
    "en": "Apa Itu Sinonim?",
    "id": "Persamaan Kata."
  },
  {
    "en": "Apa Itu Antonim?",
    "id": "Lawan Kata."
  },
  {
    "en": "Apa Itu Tes RMIB?",
    "id": "Tes Mengukur Minat Jabatan."
  },
  {
    "en": "Apa Itu Tes PAPI Kostick?",
    "id": "Tes Mengukur Persepsi Diri Kerja."
  },
  {
    "en": "Apa Itu Sikap Kerja?",
    "id": "Evaluasi Terhadap Pekerjaan Seseorang."
  },
  {
    "en": "Apa Itu Efek Halo?",
    "id": "Bias Kognitif Kesan Menyeluruh."
  },
  {
    "en": "Apa Itu Defense Mechanism?",
    "id": "Mekanisme Pertahanan Ego."
  },
  {
    "en": "Apa Itu Proyeksi?",
    "id": "Mekanisme Pertahanan Melempar Kesalahan."
  },
  {
    "en": "Apa Itu Rasionalisasi?",
    "id": "Mekanisme Pertahanan Mencari Alasan."
  },
  {
    "en": "Apa Itu Represi?",
    "id": "Mekanisme Pertahanan Menekan Memori."
  },
  {
    "en": "Apa Itu Sublimasi?",
    "id": "Mekanisme Pertahanan Mengalihkan Energi."
  },
  {
    "en": "Apa Itu Kematangan Emosional?",
    "id": "Kemampuan Mengelola Emosi Secara Dewasa."
  },
  {
    "en": "Apa Itu Tes CFIT?",
    "id": "Tes Kecerdasan Bebas Pengaruh Budaya."
  },
  {
    "en": "Apa Itu Gairah?",
    "id": "Antusiasme Besar Terhadap Sesuatu."
  },
  {
    "en": "Apa Itu Insting?",
    "id": "Perilaku Bawaan Lahir."
  },
  {
    "en": "Apa Itu Intuisi?",
    "id": "Pemahaman Langsung Tanpa Penalaran Sadar."
  },
  {
    "en": "Apa Itu Gangguan Kepribadian?",
    "id": "Pola Perilaku Kaku Tidak Sehat."
  },
  {
    "en": "Apa Itu PTSD?",
    "id": "Gangguan Stres Pasca Trauma."
  },
  {
    "en": "Apa Itu OCD?",
    "id": "Gangguan Obsesif Kompulsif."
  },
  {
    "en": "Apa Itu Psikologi Abnormal?",
    "id": "Studi Perilaku, Pikiran Abnormal."
  },
  {
    "en": "Apa Itu ADHD?",
    "id": "Gangguan Pemusatan Perhatian Hiperaktif."
  },
  {
    "en": "Apa Itu Disleksia?",
    "id": "Kesulitan Belajar Membaca."
  },
  {
    "en": "Apa Itu Teori Big Five?",
    "id": "Model Lima Faktor Besar Kepribadian."
  },
  {
    "en": "Apa Itu Openness?",
    "id": "Keterbukaan Terhadap Pengalaman Baru."
  },
  {
    "en": "Apa Itu Conscientiousness?",
    "id": "Sifat Hati-Hati, Teratur, Disiplin."
  },
  {
    "en": "Apa Itu Extraversion?",
    "id": "Sifat Mudah Bergaul, Energik."
  },
  {
    "en": "Apa Itu Agreeableness?",
    "id": "Sifat Ramah, Kooperatif, Percaya."
  },
  {
    "en": "Apa Itu Neuroticism?",
    "id": "Kecenderungan Mengalami Emosi Negatif."
  },
  {
    "en": "Apa Itu Tes EPPS?",
    "id": "Tes Mengukur Kebutuhan Psikologis Individu."
  },
  {
    "en": "Apa Itu Psikologi Perkembangan?",
    "id": "Studi Perubahan Manusia Sepanjang Hidup."
  },
  {
    "en": "Siapa Jean Piaget?",
    "id": "Psikolog Dikenal Teori Perkembangan Kognitif."
  },
  {
    "en": "Apa Itu Tahap Sensorimotor?",
    "id": "Tahap Awal Perkembangan Kognitif Piaget."
  },
  {
    "en": "Apa Itu Tahap Praoperasional?",
    "id": "Tahap Kedua Kognitif Piaget."
  },
  {
    "en": "Apa Itu Tahap Operasional Konkret?",
    "id": "Tahap Ketiga Kognitif Piaget."
  },
  {
    "en": "Apa Itu Tahap Operasional Formal?",
    "id": "Tahap Akhir Kognitif Piaget."
  },
  {
    "en": "Siapa Erik Erikson?",
    "id": "Psikolog Teori Perkembangan Psikososial."
  },
  {
    "en": "Apa Itu Kepercayaan Vs Ketidakpercayaan?",
    "id": "Tahap Pertama Psikososial Erikson."
  },
  {
    "en": "Apa Itu Otonomi Vs Keraguan?",
    "id": "Tahap Kedua Psikososial Erikson."
  },
  {
    "en": "Apa Itu Inisiatif Vs Rasa Bersalah?",
    "id": "Tahap Ketiga Psikososial Erikson."
  },
  {
    "en": "Apa Itu Industri Vs Inferioritas?",
    "id": "Tahap Keempat Psikososial Erikson."
  },
  {
    "en": "Apa Itu Identitas Vs Kebingungan Peran?",
    "id": "Tahap Kelima Psikososial Erikson."
  },
  {
    "en": "Apa Itu Keintiman Vs Isolasi?",
    "id": "Tahap Keenam Psikososial Erikson."
  },
  {
    "en": "Apa Itu Generativitas Vs Stagnasi?",
    "id": "Tahap Ketujuh Psikososial Erikson."
  },
  {
    "en": "Apa Itu Integritas Ego Vs Keputusasaan?",
    "id": "Tahap Akhir Psikososial Erikson."
  },
  {
    "en": "Siapa Lawrence Kohlberg?",
    "id": "Psikolog Teori Perkembangan Moral."
  },
  {
    "en": "Apa Itu Moralitas Prakonvensional?",
    "id": "Tahap Awal Moral Kohlberg."
  },
  {
    "en": "Apa Itu Moralitas Konvensional?",
    "id": "Tahap Menengah Moral Kohlberg."
  },
  {
    "en": "Apa Itu Moralitas Pascakonvensional?",
    "id": "Tahap Tertinggi Moral Kohlberg."
  },
  {
    "en": "Apa Itu Teori Belajar Sosial?",
    "id": "Belajar Melalui Observasi."
  },
  {
    "en": "Siapa Albert Bandura?",
    "id": "Pencetus Teori Belajar Sosial."
  },
  {
    "en": "Apa Itu Eksperimen Boneka Bobo?",
    "id": "Eksperimen Agresi Terkenal Bandura."
  },
  {
    "en": "Apa Itu Efikasi Diri?",
    "id": "Keyakinan Kemampuan Diri Sendiri."
  },
  {
    "en": "Apa Itu Psikologi Gestalt?",
    "id": "Aliran Psikologi Fokus Keseluruhan."
  },
  {
    "en": "Apa Prinsip Utama Gestalt?",
    "id": "Keseluruhan Lebih Dari Bagian."
  },
  {
    "en": "Apa Itu Figur Dan Latar?",
    "id": "Prinsip Persepsi Visual Gestalt."
  },
  {
    "en": "Apa Itu Kedekatan (Proximity)?",
    "id": "Prinsip Pengelompokan Gestalt."
  },
  {
    "en": "Apa Itu Kesamaan (Similarity)?",
    "id": "Prinsip Pengelompokan Gestalt."
  },
  {
    "en": "Apa Itu Kontinuitas (Continuity)?",
    "id": "Prinsip Pengelompokan Gestalt."
  },
  {
    "en": "Apa Itu Penutupan (Closure)?",
    "id": "Prinsip Pengelompokan Gestalt."
  },
  {
    "en": "Apa Itu Wawancara Psikologi?",
    "id": "Wawancara Menggali Aspek Psikologis."
  },
  {
    "en": "Apa Itu Wawancara Terstruktur?",
    "id": "Wawancara Dengan Pertanyaan Baku."
  },
  {
    "en": "Apa Itu Wawancara Tidak Terstruktur?",
    "id": "Wawancara Bebas Tanpa Pertanyaan Baku."
  },
  {
    "en": "Apa Itu Tes Intelegensi Stanford-Binet?",
    "id": "Tes IQ Individual Terkenal."
  },
  {
    "en": "Apa Itu Skala Wechsler?",
    "id": "Serangkaian Tes IQ Individual."
  },
  {
    "en": "Apa Itu WAIS?",
    "id": "Tes Wechsler Untuk Orang Dewasa."
  },
  {
    "en": "Apa Itu WISC?",
    "id": "Tes Wechsler Untuk Anak-Anak."
  },
  {
    "en": "Apa Itu WPPSI?",
    "id": "Tes Wechsler Untuk Prasekolah."
  },
  {
    "en": "Apa Itu Skor Mentah (Raw Score)?",
    "id": "Skor Awal Tes Sebelum Diolah."
  },
  {
    "en": "Apa Itu Skor Standar?",
    "id": "Skor Terdistribusi Sesuai Norma."
  },
  {
    "en": "Apa Itu Persentil?",
    "id": "Peringkat Relatif Dalam Kelompok."
  },
  {
    "en": "Apa Itu Kurva Normal?",
    "id": "Distribusi Skor Berbentuk Lonceng."
  },
  {
    "en": "Apa Itu Deviasi Standar?",
    "id": "Ukuran Sebaran Data Dari Rata-Rata."
  },
  {
    "en": "Apa Itu Analisis Faktor?",
    "id": "Metode Statistik Mengidentifikasi Pola."
  },
  {
    "en": "Apa Itu Intelegensi Cair (Fluid)?",
    "id": "Kemampuan Penalaran Abstrak."
  },
  {
    "en": "Apa Itu Intelegensi Kristal (Crystallized)?",
    "id": "Pengetahuan Akumulasi Dari Pengalaman."
  },
  {
    "en": "Siapa Pencetus Teori Inteligensi Ganda?",
    "id": "Howard Gardner."
  },
  {
    "en": "Apa Itu Intelegensi Musikal?",
    "id": "Kecerdasan Dalam Musik."
  },
  {
    "en": "Apa Itu Intelegensi Kinestetik?",
    "id": "Kecerdasan Dalam Gerakan Tubuh."
  },
  {
    "en": "Apa Itu Intelegensi Interpersonal?",
    "id": "Kecerdasan Memahami Orang Lain."
  },
  {
    "en": "Apa Itu Intelegensi Intrapersonal?",
    "id": "Kecerdasan Memahami Diri Sendiri."
  },
  {
    "en": "Apa Itu Intelegensi Naturalis?",
    "id": "Kecerdasan Memahami Alam."
  },
  {
    "en": "Apa Itu Intelegensi Linguistik?",
    "id": "Kecerdasan Dalam Bahasa."
  },
  {
    "en": "Apa Itu Intelegensi Logis-Matematis?",
    "id": "Kecerdasan Angka Dan Logika."
  },
  {
    "en": "Apa Itu Intelegensi Spasial?",
    "id": "Kecerdasan Visual Ruang."
  },
  {
    "en": "Siapa Pencetus Teori Triarkis Intelegensi?",
    "id": "Robert Sternberg."
  },
  {
    "en": "Apa Itu Intelegensi Analitis?",
    "id": "Bagian Teori Triarkis Sternberg."
  },
  {
    "en": "Apa Itu Intelegensi Kreatif?",
    "id": "Bagian Teori Triarkis Sternberg."
  },
  {
    "en": "Apa Itu Intelegensi Praktis?",
    "id": "Bagian Teori Triarkis Sternberg."
  },
  {
    "en": "Apa Itu Tes Kreativitas?",
    "id": "Tes Mengukur Kemampuan Berpikir Kreatif."
  },
  {
    "en": "Apa Itu Berpikir Divergen?",
    "id": "Menghasilkan Banyak Ide Berbeda."
  },
  {
    "en": "Apa Itu Berpikir Konvergen?",
    "id": "Menemukan Satu Solusi Tepat."
  },
  {
    "en": "Apa Itu DSM-5?",
    "id": "Panduan Diagnostik Gangguan Mental."
  },
  {
    "en": "Apa Itu ICD-11?",
    "id": "Klasifikasi Penyakit Internasional WHO."
  },
  {
    "en": "Apa Itu Efek Plasebo?",
    "id": "Efek Positif Akibat Sugesti."
  },
  {
    "en": "Apa Itu Efek Nosebo?",
    "id": "Efek Negatif Akibat Sugesti."
  },
  {
    "en": "Apa Itu Studi Kasus?",
    "id": "Penelitian Mendalam Satu Individu."
  },
  {
    "en": "Apa Itu Survei?",
    "id": "Metode Penelitian Menggunakan Kuesioner."
  },
  {
    "en": "Apa Itu Eksperimen?",
    "id": "Metode Penelitian Menguji Sebab Akibat."
  },
  {
    "en": "Apa Itu Variabel Independen?",
    "id": "Variabel Yang Dimanipulasi Peneliti."
  },
  {
    "en": "Apa Itu Variabel Dependen?",
    "id": "Variabel Yang Diukur Peneliti."
  },
  {
    "en": "Apa Itu Kelompok Kontrol?",
    "id": "Kelompok Pembanding Dalam Eksperimen."
  },
  {
    "en": "Apa Itu Kelompok Eksperimen?",
    "id": "Kelompok Menerima Perlakuan."
  },
  {
    "en": "Apa Itu Random Assignment?",
    "id": "Penempatan Acak Ke Kelompok."
  },
  {
    "en": "Apa Itu Studi Korelasi?",
    "id": "Penelitian Mengukur Hubungan Variabel."
  },
  {
    "en": "Apa Itu Korelasi Positif?",
    "id": "Kedua Variabel Bergerak Searah."
  },
  {
    "en": "Apa Itu Korelasi Negatif?",
    "id": "Kedua Variabel Bergerak Berlawanan Arah."
  },
  {
    "en": "Apa Itu Ilusi Optik?",
    "id": "Persepsi Visual Yang Menipu."
  },
  {
    "en": "Apa Itu Disonansi Kognitif?",
    "id": "Ketidaknyamanan Akibat Pikiran Bertentangan."
  },
  {
    "en": "Siapa Leon Festinger?",
    "id": "Pencetus Teori Disonansi Kognitif."
  },
  {
    "en": "Apa Itu Atribusi?",
    "id": "Proses Menjelaskan Penyebab Perilaku."
  },
  {
    "en": "Apa Itu Bias Konfirmasi?",
    "id": "Mencari Bukti Mendukung Keyakinan Sendiri."
  },
  {
    "en": "Apa Itu Heuristik?",
    "id": "Jalan Pintas Mental Pengambilan Keputusan."
  },
  {
    "en": "Apa Itu Heuristik Ketersediaan?",
    "id": "Menilai Berdasarkan Ingatan Yang Mudah."
  },
  {
    "en": "Apa Itu Heuristik Representativitas?",
    "id": "Menilai Berdasarkan Stereotip."
  },
  {
    "en": "Apa Itu Psikologi Forensik?",
    "id": "Aplikasi Psikologi Dalam Hukum."
  },
  {
    "en": "Apa Itu Psikologi Olahraga?",
    "id": "Aplikasi Psikologi Dalam Olahraga."
  },
  {
    "en": "Apa Itu Memori Jangka Pendek?",
    "id": "Penyimpanan Informasi Sementara."
  },
  {
    "en": "Apa Itu Memori Jangka Panjang?",
    "id": "Penyimpanan Informasi Permanen."
  },
  {
    "en": "Apa Itu Memori Sensorik?",
    "id": "Penyimpanan Awal Informasi Sensorik."
  },
  {
    "en": "Apa Itu Chunking?",
    "id": "Strategi Mengelompokkan Informasi."
  },
  {
    "en": "Apa Itu Amnesia?",
    "id": "Kehilangan Ingatan."
  },
  {
    "en": "Apa Itu Konformitas?",
    "id": "Menyesuaikan Perilaku Dengan Kelompok."
  },
  {
    "en": "Apa Itu Eksperimen Asch?",
    "id": "Eksperimen Klasik Tentang Konformitas."
  },
  {
    "en": "Apa Itu Tes MMPI (Minnesota Multiphasic Personality Inventory)?",
    "id": "Tes Inventori Kepribadian Mendeteksi Psikopatologi."
  },
  {
    "en": "Apa Tujuan Tes Gambar Pohon?",
    "id": "Mengukur Aspek Kepribadian Tak Sadar."
  },
  {
    "en": "Apa Yang Dinilai Tes Gambar Orang?",
    "id": "Persepsi Diri Dan Interaksi Sosial."
  },
  {
    "en": "Apa Yang Dinilai Tes Gambar Rumah?",
    "id": "Persepsi Terhadap Kehangatan Keluarga."
  },
  {
    "en": "Apa Itu Tes Kesehatan Jiwa (Keswa)?",
    "id": "Tes Mengukur Kestabilan Mental Emosional."
  },
  {
    "en": "Apa Itu Integritas?",
    "id": "Konsistensi Antara Tindakan Dan Nilai."
  },
  {
    "en": "Apa Itu Loyalitas?",
    "id": "Kepatuhan Atau Kesetiaan."
  },
  {
    "en": "Apa Itu Kepemimpinan?",
    "id": "Kemampuan Mempengaruhi Orang Lain."
  },
  {
    "en": "Apa Itu Stres Toleransi?",
    "id": "Kemampuan Bekerja Baik Di Bawah Tekanan."
  },
  {
    "en": "Apa Itu Kerja Sama Tim?",
    "id": "Kemampuan Bekerja Efektif Dalam Kelompok."
  },
  {
    "en": "Apa Itu Motivasi Berprestasi?",
    "id": "Dorongan Untuk Sukses."
  },
  {
    "en": "Apa Yang Diukur Tes Kraepelin?",
    "id": "Kecepatan, Ketelitian, Stamina Kerja."
  },
  {
    "en": "Apa Itu Grafik Puncak Kraepelin?",
    "id": "Menunjukkan Puncak Performa Kerja."
  },
  {
    "en": "Apa Itu Grafik Penurunan Kraepelin?",
    "id": "Menunjukkan Kelelahan Atau Penurunan Motivasi."
  },
  {
    "en": "Apa Itu Tes Army Alpha?",
    "id": "Tes Inteligensi Kelompok Militer Verbal."
  },
  {
    "en": "Apa Itu Tes Army Beta?",
    "id": "Tes Inteligensi Kelompok Militer Non-Verbal."
  },
  {
    "en": "Apa Itu Tes Penalaran Analitis?",
    "id": "Kemampuan Menganalisis Situasi Kompleks."
  },
  {
    "en": "Apa Itu Tes Spasial Kubus?",
    "id": "Tes Memvisualisasikan Jaring-Jaring Kubus."
  },
  {
    "en": "Apa Itu Tes Rotasi Gambar?",
    "id": "Tes Membayangkan Perputaran Objek Tiga Dimensi."
  },
  {
    "en": "Apa Itu Tes Deret Gambar?",
    "id": "Tes Menemukan Pola Gambar Berikutnya."
  },
  {
    "en": "Apa Itu Tes Ketelitian?",
    "id": "Tes Mencari Perbedaan Detail Gambar."
  },
  {
    "en": "Apa Itu Psikopatologi?",
    "id": "Studi Ilmiah Tentang Gangguan Mental."
  },
  {
    "en": "Apa Itu Skala 'N' PAPI (Perception And Preference Inventory)?",
    "id": "Kebutuhan Menyelesaikan Tugas (Need Finish Task)."
  },
  {
    "en": "Apa Itu Skala 'G' PAPI (Perception And Preference Inventory)?",
    "id": "Kebutuhan Bekerja Keras (Hard Intense Worker)."
  },
  {
    "en": "Apa Itu Skala 'L' PAPI (Perception And Preference Inventory)?",
    "id": "Peran Sebagai Pemimpin (Leadership Role)."
  },
  {
    "en": "Apa Itu Skala 'I' PAPI (Perception And Preference Inventory)?",
    "id": "Peran Membuat Keputusan (Decision Maker)."
  },
  {
    "en": "Apa Itu Skala 'T' PAPI (Perception And Preference Inventory)?",
    "id": "Peran Sibuk (Pace, Busy)."
  },
  {
    "en": "Apa Itu Skala 'V' PAPI (Perception And Preference Inventory)?",
    "id": "Peran Kekuatan Fisik (Physical Vigour)."
  },
  {
    "en": "Apa Itu Skala 'X' PAPI (Perception And Preference Inventory)?",
    "id": "Kebutuhan Diperhatikan (Need To Be Noticed)."
  },
  {
    "en": "Apa Itu Skala 'B' PAPI (Perception And Preference Inventory)?",
    "id": "Kebutuhan Diatur (Need To Be Managed)."
  },
  {
    "en": "Apa Itu Skala 'O' PAPI (Perception And Preference Inventory)?",
    "id": "Kebutuhan Diterima Kelompok (Belongingness)."
  },
  {
    "en": "Apa Itu Skala 'S' PAPI (Perception And Preference Inventory)?",
    "id": "Kebutuhan Relasi Sosial (Social Extension)."
  },
  {
    "en": "Apa Itu Skala 'W' PAPI (Perception And Preference Inventory)?",
    "id": "Kebutuhan Mengikuti Aturan (Need For Rules)."
  },
  {
    "en": "Apa Itu Skala 'F' PAPI (Perception And Preference Inventory)?",
    "id": "Kebutuhan Membantu Atasan (Support Superior)."
  },
  {
    "en": "Apa Itu Skala 'Z' PAPI (Perception And Preference Inventory)?",
    "id": "Kebutuhan Akan Perubahan (Need For Change)."
  },
  {
    "en": "Apa Kebutuhan Afiliasi EPPS (Edwards Personal Preference Schedule)?",
    "id": "Kebutuhan Berteman, Berkelompok."
  },
  {
    "en": "Apa Kebutuhan Otonomi EPPS (Edwards Personal Preference Schedule)?",
    "id": "Kebutuhan Mandiri, Bebas."
  },
  {
    "en": "Apa Kebutuhan Dominasi EPPS (Edwards Personal Preference Schedule)?",
    "id": "Kebutuhan Memimpin, Mengontrol Orang."
  },
  {
    "en": "Apa Kebutuhan Agresi EPPS (Edwards Personal Preference Schedule)?",
    "id": "Kebutuhan Menyerang Pandangan Orang Lain."
  },
  {
    "en": "Apa Kebutuhan Eksibisi EPPS (Edwards Personal Preference Schedule)?",
    "id": "Kebutuhan Menjadi Pusat Perhatian."
  },
  {
    "en": "Apa Kebutuhan Nurturance EPPS (Edwards Personal Preference Schedule)?",
    "id": "Kebutuhan Memberi Simpati, Membantu."
  },
  {
    "en": "Apa Kebutuhan Achievement EPPS (Edwards Personal Preference Schedule)?",
    "id": "Kebutuhan Berprestasi, Sukses."
  },
  {
    "en": "Apa Kebutuhan Deference EPPS (Edwards Personal Preference Schedule)?",
    "id": "Kebutuhan Mengikuti Arahan."
  },
  {
    "en": "Apa Kebutuhan Order EPPS (Edwards Personal Preference Schedule)?",
    "id": "Kebutuhan Teratur, Rapi."
  },
  {
    "en": "Apa Kebutuhan Succorance EPPS (Edwards Personal Preference Schedule)?",
    "id": "Kebutuhan Dibantu, Dikasihani."
  },
  {
    "en": "Apa Kebutuhan Abasement EPPS (Edwards Personal Preference Schedule)?",
    "id": "Kebutuhan Merasa Bersalah, Menerima Hukuman."
  },
  {
    "en": "Apa Kebutuhan Endurance EPPS (Edwards Personal Preference Schedule)?",
    "id": "Kebutuhan Bertahan Menyelesaikan Tugas."
  },
  {
    "en": "Apa Kebutuhan Heterosexuality EPPS (Edwards Personal Preference Schedule)?",
    "id": "Kebutuhan Bergaul Dengan Lawan Jenis."
  },
  {
    "en": "Apa Kebutuhan Change EPPS (Edwards Personal Preference Schedule)?",
    "id": "Kebutuhan Akan Perubahan, Hal Baru."
  },
  {
    "en": "Apa Itu Gambar Dinamis?",
    "id": "Gambar Orang Dalam Suatu Aktivitas."
  },
  {
    "en": "Apa Itu Tes Gambar Keluarga?",
    "id": "Tes Memahami Dinamika Keluarga."
  },
  {
    "en": "Apa Itu Keterampilan Interpersonal?",
    "id": "Kemampuan Berinteraksi Dengan Orang Lain."
  },
  {
    "en": "Apa Itu Komunikasi Efektif?",
    "id": "Penyampaian Pesan Jelas Dan Dimengerti."
  },
  {
    "en": "Apa Itu Pemecahan Masalah?",
    "id": "Proses Menemukan Solusi Masalah."
  },
  {
    "en": "Apa Itu Pengambilan Keputusan?",
    "id": "Proses Memilih Opsi Terbaik."
  },
  {
    "en": "Apa Itu Tes SSCT (Sacks Sentence Completion Test)?",
    "id": "Tes Melengkapi Kalimat."
  },
  {
    "en": "Apa Tujuan Tes SSCT (Sacks Sentence Completion Test)?",
    "id": "Mengungkapkan Perasaan, Sikap, Konflik."
  },
  {
    "en": "Apa Itu Leaderless Group Discussion (LGD)?",
    "id": "Diskusi Kelompok Tanpa Pemimpin Ditunjuk."
  },
  {
    "en": "Apa Yang Dinilai LGD (Leaderless Group Discussion)?",
    "id": "Kepemimpinan, Komunikasi, Dan Kerjasama."
  },
  {
    "en": "Apa Itu Tes Logika Aritmatika?",
    "id": "Tes Kemampuan Berhitung Dasar."
  },
  {
    "en": "Apa Itu Tes Logika Penalaran?",
    "id": "Tes Kemampuan Menarik Kesimpulan Logis."
  },
  {
    "en": "Apa Itu Tes Informasi Umum?",
    "id": "Tes Pengetahuan Dasar Wawasan Kebangsaan."
  },
  {
    "en": "Apa Itu Motivasi Intrinsik?",
    "id": "Dorongan Internal Dari Dalam Diri."
  },
  {
    "en": "Apa Itu Motivasi Ekstrinsik?",
    "id": "Dorongan Eksternal Dari Luar Diri."
  },
  {
    "en": "Apa Itu Skala Kecemasan?",
    "id": "Alat Ukur Tingkat Kecemasan."
  },
  {
    "en": "Apa Itu Skala Depresi?",
    "id": "Alat Ukur Tingkat Depresi."
  },
  {
    "en": "Apa Itu Tes BDI (Beck Depression Inventory)?",
    "id": "Inventori Depresi Beck."
  },
  {
    "en": "Apa Itu Tes BAI (Beck Anxiety Inventory)?",
    "id": "Inventori Kecemasan Beck."
  },
  {
    "en": "Apa Itu Tes STAI (State-Trait Anxiety Inventory)?",
    "id": "Inventori Kecemasan State-Trait."
  },
  {
    "en": "Apa Itu Hipomania?",
    "id": "Episode Mania Ringan."
  },
  {
    "en": "Apa Itu Delusi?",
    "id": "Keyakinan Salah Yang Dipertahankan Kuat."
  },
  {
    "en": "Apa Itu Halusinasi?",
    "id": "Pengalaman Sensorik Tanpa Rangsangan Nyata."
  },
  {
    "en": "Apa Itu Halusinasi Auditori?",
    "id": "Mendengar Suara Tanpa Sumber."
  },
  {
    "en": "Apa Itu Halusinasi Visual?",
    "id": "Melihat Sesuatu Tanpa Sumber."
  },
  {
    "en": "Apa Itu Psikosomatis?",
    "id": "Gejala Fisik Akibat Faktor Psikologis."
  },
  {
    "en": "Apa Itu Trauma Psikologis?",
    "id": "Respon Emosional Mendalam Kejadian Buruk."
  },
  {
    "en": "Apa Itu Coping Mechanism?",
    "id": "Strategi Mengatasi Stres Atau Masalah."
  },
  {
    "en": "Apa Itu Problem-Focused Coping?",
    "id": "Mengatasi Stres Dengan Menyelesaikan Masalah."
  },
  {
    "en": "Apa Itu Emotion-Focused Coping?",
    "id": "Mengatasi Stres Dengan Mengatur Emosi."
  },
  {
    "en": "Apa Itu Tes SPM (Standard Progressive Matrices)?",
    "id": "Tes Matriks Progresif Standar Raven."
  },
  {
    "en": "Apa Yang Diukur Tes SPM (Standard Progressive Matrices)?",
    "id": "Kecerdasan Umum (Faktor 'G')."
  },
  {
    "en": "Apa Itu Tes APM (Advanced Progressive Matrices)?",
    "id": "Tes Matriks Progresif Lanjutan Raven."
  },
  {
    "en": "Apa Itu Tes CPM (Coloured Progressive Matrices)?",
    "id": "Tes Matriks Progresif Warna Raven."
  },
  {
    "en": "Apa Itu Tes TIKI (Tes Inteligensi Kolektif Indonesia)?",
    "id": "Tes Inteligensi Kolektif Indonesia."
  },
  {
    "en": "Apa Itu Tes GATB (General Aptitude Test Battery)?",
    "id": "Tes Bakat Rangkaian."
  },
  {
    "en": "Apa Itu Bakat Verbal?",
    "id": "Kemampuan Memahami Menggunakan Bahasa."
  },
  {
    "en": "Apa Itu Bakat Numerik?",
    "id": "Kemampuan Memahami Operasi Angka."
  },
  {
    "en": "Apa Itu Bakat Skolastik?",
    "id": "Kombinasi Bakat Verbal Dan Numerik."
  },
  {
    "en": "Apa Itu Bakat Klerikal?",
    "id": "Kemampuan Tugas Administratif Detail."
  },
  {
    "en": "Apa Itu Tes Perseptual?",
    "id": "Tes Kecepatan Persepsi Detail."
  },
  {
    "en": "Apa Itu Tes DAT (Differential Aptitude Tests)?",
    "id": "Tes Bakat Diferensial."
  },
  {
    "en": "Apa Itu Skala Likert?",
    "id": "Skala Pengukuran Sikap Psikologis."
  },
  {
    "en": "Apa Itu Validitas Isi?",
    "id": "Keterwakilan Item Tes."
  },
  {
    "en": "Apa Itu Validitas Konstruk?",
    "id": "Ketepatan Pengukuran Konsep Teoritis."
  },
  {
    "en": "Apa Itu Validitas Kriteria?",
    "id": "Hubungan Skor Tes Dengan Kriteria."
  },
  {
    "en": "Apa Itu Tipe Kepribadian A?",
    "id": "Kompetitif, Ambisius, Tidak Sabaran."
  },
  {
    "en": "Apa Itu Tipe Kepribadian B?",
    "id": "Rileks, Sabar, Mudah Bergaul."
  },
  {
    "en": "Apa Itu Eksperimen Kepatuhan Milgram?",
    "id": "Tes Kepatuhan Terhadap Otoritas."
  },
  {
    "en": "Siapa Stanley Milgram?",
    "id": "Psikolog Sosial Dikenal Eksperimen Kepatuhan."
  },
  {
    "en": "Apa Itu Efek Pengamat (Bystander Effect)?",
    "id": "Orang Cenderung Tidak Membantu."
  },
  {
    "en": "Apa Itu Stanford Prison Experiment?",
    "id": "Eksperimen Peran Sosial Zimbardo."
  },
  {
    "en": "Siapa Philip Zimbardo?",
    "id": "Psikolog Eksperimen Penjara Stanford."
  },
  {
    "en": "Apa Itu Kesalahan Atribusi Fundamental?",
    "id": "Menyalahkan Kepribadian Daripada Situasi."
  },
  {
    "en": "Apa Itu Bias Aktor-Pengamat?",
    "id": "Atribusi Berbeda Untuk Diri Sendiri."
  },
  {
    "en": "Apa Itu Stereotip?",
    "id": "Generalisasi Berlebihan Tentang Kelompok."
  },
  {
    "en": "Apa Itu Prasangka (Prejudice)?",
    "id": "Sikap Negatif Terhadap Kelompok."
  },
  {
    "en": "Apa Itu Diskriminasi?",
    "id": "Perilaku Negatif Terhadap Kelompok."
  },
  {
    "en": "Apa Itu Groupthink?",
    "id": "Pengambilan Keputusan Kelompok Buruk."
  },
  {
    "en": "Apa Itu Polarisasi Kelompok?",
    "id": "Opini Kelompok Menjadi Ekstrem."
  },
  {
    "en": "Apa Itu Deindividuasi?",
    "id": "Kehilangan Jati Diri Dalam Kelompok."
  },
  {
    "en": "Apa Itu Locus Of Control (LOC)?",
    "id": "Pusat Kendali Kehidupan."
  },
  {
    "en": "Apa Itu Locus Of Control Internal?",
    "id": "Keyakinan Mengendalikan Hidup Sendiri."
  },
  {
    "en": "Apa Itu Locus Of Control Eksternal?",
    "id": "Keyakinan Nasib Dikendalikan Luar."
  },
  {
    "en": "Siapa Julian Rotter?",
    "id": "Pencetus Teori Locus Of Control."
  },
  {
    "en": "Apa Itu Self-Concept?",
    "id": "Gambaran Total Diri Sendiri."
  },
  {
    "en": "Apa Itu Self-Esteem?",
    "id": "Evaluasi Nilai Diri Sendiri."
  },
  {
    "en": "Apa Itu Tes 16PF (Sixteen Personality Factor)?",
    "id": "Tes Kepribadian Raymond Cattell."
  },
  {
    "en": "Siapa Raymond Cattell?",
    "id": "Psikolog Teori 16 Faktor Kepribadian."
  },
  {
    "en": "Apa Itu Insight Dalam Psikologi?",
    "id": "Pemahaman Masalah Mendadak."
  },
  {
    "en": "Apa Itu Mental Set?",
    "id": "Kecenderungan Menggunakan Solusi Lama."
  },
  {
    "en": "Apa Itu Fiksasi Fungsional?",
    "id": "Hambatan Melihat Fungsi Baru Objek."
  },
  {
    "en": "Apa Itu Konsolidasi Memori?",
    "id": "Proses Penguatan Memori."
  },
  {
    "en": "Apa Itu Amnesia Retrograd?",
    "id": "Lupa Ingatan Masa Lalu."
  },
  {
    "en": "Apa Itu Amnesia Anterograd?",
    "id": "Gagal Membentuk Ingatan Baru."
  },
  {
    "en": "Apa Itu Gangguan Kecemasan Umum (GAD)?",
    "id": "Kecemasan Kronis Berlebihan."
  },
  {
    "en": "Apa Itu Gangguan Panik?",
    "id": "Serangan Panik Tiba-Tiba Berulang."
  },
  {
    "en": "Apa Itu Agorafobia?",
    "id": "Takut Tempat Ramai, Susah Kabur."
  },
  {
    "en": "Apa Itu Gangguan Stres Akut?",
    "id": "Respon Stres Segera Setelah Trauma."
  },
  {
    "en": "Apa Itu Distimia?",
    "id": "Depresi Ringan Jangka Panjang."
  },
  {
    "en": "Apa Itu Siklotimia?",
    "id": "Gangguan Mood Bipolar Versi Ringan."
  },
  {
    "en": "Apa Itu Gangguan Kepribadian Antisosial?",
    "id": "Mengabaikan Hak Orang Lain, Kurang Empati."
  },
  {
    "en": "Apa Itu Gangguan Kepribadian Borderline?",
    "id": "Ketidakstabilan Emosi, Hubungan, Citra Diri."
  },
  {
    "en": "Apa Itu Gangguan Kepribadian Histrionik?",
    "id": "Mencari Perhatian Berlebihan, Emosional."
  },
  {
    "en": "Apa Itu Gangguan Kepribadian Narsistik?",
    "id": "Kebutuhan Admirasi, Merasa Superior."
  },
  {
    "en": "Apa Itu Gangguan Kepribadian Skizoid?",
    "id": "Pola Pelepasan Diri Hubungan Sosial."
  },
  {
    "en": "Apa Itu Gangguan Kepribadian Skizotipal?",
    "id": "Pola Pemikiran Eksentrik, Aneh."
  },
  {
    "en": "Apa Itu Gangguan Kepribadian Paranoid?",
    "id": "Pola Kecurigaan Berlebihan."
  },
  {
    "en": "Apa Itu Gangguan Kepribadian Dependen?",
    "id": "Kebutuhan Diurus, Takut Ditinggalkan."
  },
  {
    "en": "Apa Itu Gangguan Kepribadian Avoidant?",
    "id": "Pola Penghindaran Sosial, Takut Penolakan."
  },
  {
    "en": "Apa Itu Gangguan Kepribadian Obsesif-Kompulsif (OCPD)?",
    "id": "Pola Keteraturan, Perfeksionisme, Kontrol."
  },
  {
    "en": "Apa Itu Psikologi Industri Dan Organisasi (PIO)?",
    "id": "Psikologi Di Tempat Kerja."
  },
  {
    "en": "Apa Itu Analisis Jabatan?",
    "id": "Studi Detail Tugas Pekerjaan."
  },
  {
    "en": "Apa Itu Spesifikasi Jabatan?",
    "id": "Kualifikasi SDM Untuk Jabatan."
  },
  {
    "en": "Apa Itu Deskripsi Jabatan?",
    "id": "Uraian Tugas Dan Tanggung Jawab."
  },
  {
    "en": "Apa Itu Kepuasan Kerja?",
    "id": "Perasaan Positif Terhadap Pekerjaan."
  },
  {
    "en": "Apa Itu Burnout?",
    "id": "Kelelahan Emosional, Fisik Akibat Kerja."
  },
  {
    "en": "Apa Itu Assessment Center?",
    "id": "Metode Penilaian Kompetensi Multi-Alat."
  },
  {
    "en": "Apa Itu In-Basket Exercise?",
    "id": "Simulasi Tugas Administratif Assessment Center."
  },
  {
    "en": "Apa Itu Role Play?",
    "id": "Simulasi Situasi Kerja Spesifik."
  },
  {
    "en": "Apa Itu Stres Kerja?",
    "id": "Respon Stres Akibat Tuntutan Pekerjaan."
  },
  {
    "en": "Apa Itu Tes Kuder (Kuder Preference Record)?",
    "id": "Tes Mengukur Minat Vokasional."
  },
  {
    "en": "Apa Itu Tes Holland (RIASEC)?",
    "id": "Tes Minat Jabatan Enam Tipe."
  },
  {
    "en": "Apa Tipe Realistis Holland?",
    "id": "Suka Bekerja Dengan Alat, Mesin."
  },
  {
    "en": "Apa Tipe Investigatif Holland?",
    "id": "Suka Menganalisis, Memecahkan Masalah."
  },
  {
    "en": "Apa Tipe Artistik Holland?",
    "id": "Suka Ekspresi Diri, Kreatif."
  },
  {
    "en": "Apa Tipe Sosial Holland?",
    "id": "Suka Membantu, Mengajar Orang Lain."
  },
  {
    "en": "Apa Tipe Enterprising Holland?",
    "id": "Suka Memimpin, Mempengaruhi Orang."
  },
  {
    "en": "Apa Tipe Konvensional Holland?",
    "id": "Suka Bekerja Data, Teratur, Rapi."
  },
  {
    "en": "Apa Itu Tes MSDT (Mental Skills Descriptive Test)?",
    "id": "Tes Mengukur Keterampilan Mental Dasar."
  },
  {
    "en": "Apa Itu Tes Mental Ideologi (MI)?",
    "id": "Tes Penilaian Pemahaman Ideologi Negara."
  },
  {
    "en": "Apa Itu Tes Kesamaptaan Jasmani?",
    "id": "Tes Kebugaran Fisik Calon."
  },
  {
    "en": "Apa Hubungan Psikologi Dan Kesamaptaan?",
    "id": "Mental Kuat Mendukung Fisik Kuat."
  },
  {
    "en": "Apa Itu Wawancara Psikologi Seleksi?",
    "id": "Wawancara Mendalam Menggali Kepribadian."
  },
  {
    "en": "Apa Itu Penelusuran Rekam Jejak?",
    "id": "Pemeriksaan Latar Belakang Calon."
  },
  {
    "en": "Apa Itu Psikometri?",
    "id": "Ilmu Pengukuran Aspek Psikologis."
  },
  {
    "en": "Apa Itu Tes Proyektif?",
    "id": "Tes Kepribadian Menggunakan Stimulus Ambigu."
  },
  {
    "en": "Apa Itu Tes Objektif?",
    "id": "Tes Kepribadian Pilihan Jawaban Baku."
  },
  {
    "en": "Apa Itu Inventori Kepribadian?",
    "id": "Tes Mengukur Trait Kepribadian Spesifik."
  },
  {
    "en": "Apa Itu Skala Validitas MMPI (Minnesota Multiphasic Personality Inventory)?",
    "id": "Mengukur Kejujuran Respon Tes."
  },
  {
    "en": "Apa Itu Skala L (Lie) MMPI?",
    "id": "Mengukur Upaya Tampil Sangat Baik."
  },
  {
    "en": "Apa Itu Skala F (Infrequency) MMPI?",
    "id": "Mengukur Pola Jawaban Aneh."
  },
  {
    "en": "Apa Itu Skala K (Correction) MMPI?",
    "id": "Mengukur Sikap Defensif Responden."
  },
  {
    "en": "Apa Itu Neurotransmiter?",
    "id": "Zat Kimia Pembawa Pesan Otak."
  },
  {
    "en": "Apa Itu Serotonin?",
    "id": "Neurotransmiter Terkait Mood, Tidur."
  },
  {
    "en": "Apa Itu Dopamin?",
    "id": "Neurotransmiter Terkait Motivasi, Kesenangan."
  },
  {
    "en": "Apa Itu Kortisol?",
    "id": "Hormon Utama Stres."
  },
  {
    "en": "Apa Itu Adrenalin?",
    "id": "Hormon Respon Lawan Atau Lari."
  },
  {
    "en": "Apa Itu Lobus Frontal?",
    "id": "Bagian Otak Untuk Perencanaan, Keputusan."
  },
  {
    "en": "Apa Itu Lobus Temporal?",
    "id": "Bagian Otak Untuk Pendengaran, Memori."
  },
  {
    "en": "Apa Itu Lobus Parietal?",
    "id": "Bagian Otak Untuk Sensasi Sentuhan."
  },
  {
    "en": "Apa Itu Lobus Oksipital?",
    "id": "Bagian Otak Untuk Penglihatan."
  },
  {
    "en": "Apa Itu Amigdala?",
    "id": "Bagian Otak Pusat Emosi, Terutama Takut."
  },
  {
    "en": "Apa Itu Hipokampus?",
    "id": "Bagian Otak Kunci Pembentukan Memori."
  },
  {
    "en": "Apa Itu Saraf Sensorik?",
    "id": "Saraf Mengirim Sinyal Ke Otak."
  },
  {
    "en": "Apa Itu Saraf Motorik?",
    "id": "Saraf Mengirim Sinyal Dari Otak."
  },
  {
    "en": "Apa Itu Sistem Saraf Pusat (SSP)?",
    "id": "Terdiri Dari Otak Dan Sumsum."
  },
  {
    "en": "Apa Itu Sistem Saraf Tepi?",
    "id": "Saraf Diluar Sistem Saraf Pusat."
  },
  {
    "en": "Apa Itu Sistem Saraf Otonom?",
    "id": "Mengontrol Fungsi Tubuh Otomatis."
  },
  {
    "en": "Apa Itu Sistem Saraf Simpatik?",
    "id": "Mengaktifkan Respon 'Lawan Atau Lari'."
  },
  {
    "en": "Apa Itu Sistem Saraf Parasimpatik?",
    "id": "Mengaktifkan Respon 'Istirahat Dan Cerna'."
  },
  {
    "en": "Apa Itu Ritme Sirkadian?",
    "id": "Siklus Biologis Tubuh 24 Jam."
  },
  {
    "en": "Apa Itu Tahap Tidur REM (Rapid Eye Movement)?",
    "id": "Tahap Tidur Dimana Mimpi Terjadi."
  },
  {
    "en": "Apa Itu Insomnia?",
    "id": "Kesulitan Tidur Kronis."
  },
  {
    "en": "Apa Itu Narkolepsi?",
    "id": "Gangguan Tidur Serangan Kantuk Mendadak."
  },
  {
    "en": "Apa Itu Sleep Apnea?",
    "id": "Henti Napas Saat Tidur."
  },
  {
    "en": "Apa Itu Psikologi Positif?",
    "id": "Studi Ilmiah Kebahagiaan Manusia."
  },
  {
    "en": "Siapa Martin Seligman?",
    "id": "Tokoh Utama Psikologi Positif."
  },
  {
    "en": "Apa Itu Flow State?",
    "id": "Kondisi Fokus Penuh, Terlibat Total."
  },
  {
    "en": "Siapa Mihaly Csikszentmihalyi?",
    "id": "Pencetus Konsep 'Flow'."
  },
  {
    "en": "Apa Itu Learned Helplessness?",
    "id": "Rasa Pasrah Akibat Kegagalan Berulang."
  },
  {
    "en": "Apa Itu Optimisme?",
    "id": "Kecenderungan Berharap Hasil Positif."
  },
  {
    "en": "Apa Itu Pesimisme?",
    "id": "Kecenderungan Berharap Hasil Negatif."
  },
  {
    "en": "Apa Itu Resiliensi?",
    "id": "Kemampuan Bangkit Dari Kesulitan."
  },
  {
    "en": "Apa Itu Terapi Kognitif Perilaku (CBT)?",
    "id": "Terapi Mengubah Pola Pikir Perilaku."
  },
  {
    "en": "Siapa Aaron Beck?",
    "id": "Pengembang Terapi Kognitif."
  },
  {
    "en": "Apa Itu Distorsi Kognitif?",
    "id": "Pola Pikir Negatif Tidak Akurat."
  },
  {
    "en": "Apa Itu Pemikiran Hitam-Putih?",
    "id": "Distorsi Kognitif 'Semua Atau Tidak'."
  },
  {
    "en": "Apa Itu Generalisasi Berlebihan?",
    "id": "Distorsi Kognitif Satu Kejadian Negatif."
  },
  {
    "en": "Apa Itu Personalisasi?",
    "id": "Distorsi Kognitif Menyalahkan Diri Sendiri."
  },
  {
    "en": "Apa Itu Katastrofis?",
    "id": "Distorsi Kognitif Membesar-besarkan Hal Negatif."
  },
  {
    "en": "Apa Itu Terapi Humanistik?",
    "id": "Terapi Fokus Aktualisasi Diri."
  },
  {
    "en": "Apa Itu Client-Centered Therapy?",
    "id": "Terapi Humanistik Carl Rogers."
  },
  {
    "en": "Apa Itu Unconditional Positive Regard?",
    "id": "Penerimaan Klien Tanpa Syarat."
  },
  {
    "en": "Apa Itu Empati?",
    "id": "Kemampuan Memahami Perasaan Orang Lain."
  },
  {
    "en": "Apa Itu Simpati?",
    "id": "Merasa Kasihan Terhadap Orang Lain."
  },
  {
    "en": "Apa Itu Terapi Gestalt?",
    "id": "Terapi Fokus Kesadaran 'Disini Sekarang'."
  },
  {
    "en": "Siapa Fritz Perls?",
    "id": "Pendiri Terapi Gestalt."
  },
  {
    "en": "Apa Itu Teknik Kursi Kosong?",
    "id": "Teknik Terapi Gestalt Mengatasi Konflik."
  },
  {
    "en": "Apa Itu Psikofarmakologi?",
    "id": "Studi Efek Obat Pada Psikologis."
  },
  {
    "en": "Apa Itu Antidepresan?",
    "id": "Obat Mengobati Gejala Depresi."
  },
  {
    "en": "Apa Itu Anxiolytics?",
    "id": "Obat Mengurangi Gejala Kecemasan."
  },
  {
    "en": "Apa Itu Antipsikotik?",
    "id": "Obat Mengobati Gejala Psikosis."
  },
  {
    "en": "Apa Itu Psikologi Lintas Budaya?",
    "id": "Studi Perbandingan Psikologi Antar Budaya."
  },
  {
    "en": "Apa Itu Budaya Kolektivis?",
    "id": "Budaya Mementingkan Harmoni Kelompok."
  },
  {
    "en": "Apa Itu Budaya Individualis?",
    "id": "Budaya Mementingkan Pencapaian Individu."
  },
  {
    "en": "Apa Itu Tes PMK (Penelusuran Mental Kepribadian)?",
    "id": "Tes Wawancara Mendalam Seleksi Polri."
  },
  {
    "en": "Apa Itu Mental Blocking?",
    "id": "Hambatan Mental Saat Berpikir."
  },
  {
    "en": "Apa Itu Tes Krapelin Tipe Penambahan?",
    "id": "Menjumlahkan Angka Berdekatan."
  },
  {
    "en": "Apa Itu Puncak Tertinggi Tes Pauli?",
    "id": "Kinerja Maksimal Individu."
  },
  {
    "en": "Apa Itu Stabilitas Kinerja Pauli?",
    "id": "Konsistensi Hasil Penjumlahan Antar Waktu."
  },
  {
    "en": "Apa Itu Tes Wartegg Kotak 1?",
    "id": "Titik Di Tengah, Simbol Pusat Diri."
  },
  {
    "en": "Apa Itu Tes Wartegg Kotak 2?",
    "id": "Garis Melengkung, Simbol Fleksibilitas Emosi."
  },
  {
    "en": "Apa Itu Tes Wartegg Kotak 3?",
    "id": "Garis Tegak Lurus, Simbol Ambisi."
  },
  {
    "en": "Apa Itu Tes Wartegg Kotak 4?",
    "id": "Kotak Hitam Kecil, Simbol Masalah."
  },
  {
    "en": "Apa Itu Tes Wartegg Kotak 5?",
    "id": "Garis Diagonal Berlawanan, Simbol Energi."
  },
  {
    "en": "Apa Itu Tes Wartegg Kotak 6?",
    "id": "Garis Horisontal Vertikal, Simbol Analisis."
  },
  {
    "en": "Apa Itu Tes Wartegg Kotak 7?",
    "id": "Titik Melengkung, Simbol Kepekaan."
  },
  {
    "en": "Apa Itu Tes Wartegg Kotak 8?",
    "id": "Lengkungan Besar, Simbol Perlindungan Sosial."
  },
  {
    "en": "Apa Urutan Menggambar Wartegg Normal?",
    "id": "Biasanya Sesuai Urutan Angka."
  },
  {
    "en": "Apa Arti Menggambar Wartegg Terbalik?",
    "id": "Indikasi Oposisi Atau Cara Unik."
  },
  {
    "en": "Apa Arti Tekanan Garis Gambar?",
    "id": "Menunjukkan Tingkat Energi Psikis."
  },
  {
    "en": "Apa Arti Tekanan Garis Tebal?",
    "id": "Energi Kuat, Agresivitas, Percaya Diri."
  },
  {
    "en": "Apa Arti Tekanan Garis Tipis?",
    "id": "Energi Lemah, Keraguan, Kepekaan."
  },
  {
    "en": "Apa Arti Gambar Pohon Rindang?",
    "id": "Kebutuhan Perlindungan, Kehangatan."
  },
  {
    "en": "Apa Arti Gambar Pohon Tanpa Daun?",
    "id": "Perasaan Kosong, Depresi, Tertutup."
  },
  {
    "en": "Apa Arti Gambar Pohon Berbuah?",
    "id": "Orientasi Hasil, Kematangan, Produktif."
  },
  {
    "en": "Apa Arti Gambar Akar Pohon?",
    "id": "Keterikatan Masa Lalu, Dasar Realitas."
  },
  {
    "en": "Apa Arti Gambar Pohon Miring?",
    "id": "Ketidakstabilan Emosi, Tekanan."
  },
  {
    "en": "Apa Arti Gambar Orang Jelas?",
    "id": "Penerimaan Diri Yang Baik."
  },
  {
    "en": "Apa Arti Gambar Orang Tidak Lengkap?",
    "id": "Konflik, Penolakan Bagian Diri."
  },
  {
    "en": "Apa Arti Gambar Wajah Jelas?",
    "id": "Kebutuhan Komunikasi Sosial."
  },
  {
    "en": "Apa Arti Gambar Tangan Disembunyikan?",
    "id": "Rasa Bersalah, Sulit Berinteraksi."
  },
  {
    "en": "Apa Arti Gambar Kaki Besar?",
    "id": "Kebutuhan Akan Rasa Aman."
  },
  {
    "en": "Apa Itu Memori Eksplisit?",
    "id": "Memori Yang Disadari Secara Sadar."
  },
  {
    "en": "Apa Itu Memori Implisit?",
    "id": "Memori Yang Tidak Disadari."
  },
  {
    "en": "Apa Itu Memori Semantik?",
    "id": "Memori Tentang Fakta, Pengetahuan Umum."
  },
  {
    "en": "Apa Itu Memori Episodik?",
    "id": "Memori Tentang Pengalaman Pribadi."
  },
  {
    "en": "Apa Itu Memori Prosedural?",
    "id": "Memori Keterampilan Motorik (Cara Melakukan)."
  },
  {
    "en": "Apa Itu Priming?",
    "id": "Paparan Stimulus Mempengaruhi Respon Berikutnya."
  },
  {
    "en": "Apa Itu Interferensi Proaktif?",
    "id": "Ingatan Lama Mengganggu Ingatan Baru."
  },
  {
    "en": "Apa Itu Interferensi Retroaktif?",
    "id": "Ingatan Baru Mengganggu Ingatan Lama."
  },
  {
    "en": "Apa Itu Efek Posisi Serial?",
    "id": "Mengingat Awal Akhir Daftar."
  },
  {
    "en": "Apa Itu Efek Primacy?",
    "id": "Kecenderungan Mengingat Item Awal."
  },
  {
    "en": "Apa Itu Efek Recency?",
    "id": "Kecenderungan Mengingat Item Akhir."
  },
  {
    "en": "Apa Itu Mnemonic?",
    "id": "Alat Bantu Ingatan."
  },
  {
    "en": "Apa Itu Metode Loci?",
    "id": "Mnemonic Menggunakan Lokasi Visual."
  },
  {
    "en": "Apa Itu Akronim?",
    "id": "Mnemonic Menggunakan Huruf Awal."
  },
  {
    "en": "Apa Itu Akrostik?",
    "id": "Mnemonic Menggunakan Kalimat."
  },
  {
    "en": "Apa Itu Kurva Lupa Ebbinghaus?",
    "id": "Grafik Penurunan Ingatan Seiring Waktu."
  },
  {
    "en": "Apa Itu Deja Vu?",
    "id": "Perasaan Pernah Mengalami Situasi Baru."
  },
  {
    "en": "Apa Itu Jamais Vu?",
    "id": "Perasaan Asing Terhadap Situasi Familiar."
  },
  {
    "en": "Apa Itu Tip-Of-The-Tongue Phenomenon?",
    "id": "Kesulitan Mengingat Kata Yang Dikenal."
  },
  {
    "en": "Apa Itu Memori Palsu (False Memory)?",
    "id": "Ingatan Kejadian Yang Tidak Terjadi."
  },
  {
    "en": "Apa Itu Bias Hindsight?",
    "id": "Merasa Tahu Sesuatu Setelah Terjadi."
  },
  {
    "en": "Apa Itu Efek Dunning-Kruger?",
    "id": "Tidak Kompeten Merasa Sangat Kompeten."
  },
  {
    "en": "Apa Itu Hukum Yerkes-Dodson?",
    "id": "Hubungan Kinerja Dan Gairah (Arousal)."
  },
  {
    "en": "Apa Itu Kinerja Puncak?",
    "id": "Kinerja Terbaik Pada Gairah Sedang."
  },
  {
    "en": "Apa Itu Teori Emosi James-Lange?",
    "id": "Respon Fisiologis Menyebabkan Emosi."
  },
  {
    "en": "Apa Itu Teori Emosi Cannon-Bard?",
    "id": "Respon Fisiologis Emosi Terjadi Bersamaan."
  },
  {
    "en": "Apa Itu Teori Emosi Schachter-Singer?",
    "id": "Fisiologis Ditambah Label Kognitif."
  },
  {
    "en": "Apa Itu Ekspresi Wajah Universal?",
    "id": "Ekspresi Emosi Sama Antar Budaya."
  },
  {
    "en": "Siapa Paul Ekman?",
    "id": "Peneliti Terkenal Ekspresi Wajah Universal."
  },
  {
    "en": "Apa Enam Emosi Dasar Ekman?",
    "id": "Marah, Takut, Jijik, Kaget, Senang, Sedih."
  },
  {
    "en": "Apa Itu Microexpression?",
    "id": "Ekspresi Wajah Singkat Tak Terkontrol."
  },
  {
    "en": "Apa Itu Bahasa Tubuh?",
    "id": "Komunikasi Non-Verbal Melalui Gerakan."
  },
  {
    "en": "Apa Itu Psikologi Evolusioner?",
    "id": "Psikologi Berbasis Adaptasi Evolusi."
  },
  {
    "en": "Apa Itu Altruisme?",
    "id": "Menolong Orang Lain Tanpa Pamrih."
  },
  {
    "en": "Apa Itu Kin Selection?",
    "id": "Menolong Kerabat Untuk Melanjutkan Gen."
  },
  {
    "en": "Apa Itu Altruisme Resiprokal?",
    "id": "Menolong Sekarang, Ditolong Nanti."
  },
  {
    "en": "Apa Itu Atase (Attachment)?",
    "id": "Ikatan Emosional Kuat Antar Individu."
  },
  {
    "en": "Siapa John Bowlby?",
    "id": "Pencetus Teori Atase."
  },
  {
    "en": "Apa Itu 'Situasi Asing' Ainsworth?",
    "id": "Eksperimen Mengukur Tipe Atase Anak."
  },
  {
    "en": "Siapa Mary Ainsworth?",
    "id": "Psikolog Pengembang Eksperimen 'Situasi Asing'."
  },
  {
    "en": "Apa Itu Atase Aman (Secure Attachment)?",
    "id": "Anak Percaya Pengasuh, Merasa Aman."
  },
  {
    "en": "Apa Itu Atase Cemas-Ambivalen?",
    "id": "Anak Cemas Ditinggal, Sulit Ditenangkan."
  },
  {
    "en": "Apa Itu Atase Cemas-Menghindar?",
    "id": "Anak Menghindari Pengasuh Saat Stres."
  },
  {
    "en": "Apa Itu Atase Tidak Teratur?",
    "id": "Perilaku Anak Bingung, Kontradiktif."
  },
  {
    "en": "Apa Itu Pola Asuh Otoriter?",
    "id": "Aturan Ketat, Hukuman Keras, Kurang Hangat."
  },
  {
    "en": "Apa Itu Pola Asuh Otoritatif?",
    "id": "Aturan Jelas, Hangat, Responsif."
  },
  {
    "en": "Apa Itu Pola Asuh Permisif?",
    "id": "Sangat Hangat, Tuntutan Rendah."
  },
  {
    "en": "Apa Itu Pola Asuh Abai?",
    "id": "Tidak Terlibat, Tidak Responsif."
  },
  {
    "en": "Siapa Diana Baumrind?",
    "id": "Psikolog Pencetus Tiga Pola Asuh."
  },
  {
    "en": "Apa Itu Pubertas?",
    "id": "Masa Kematangan Seksual Fisik."
  },
  {
    "en": "Apa Itu Menarche?",
    "id": "Menstruasi Pertama Pada Wanita."
  },
  {
    "en": "Apa Itu Spermarche?",
    "id": "Ejakulasi Pertama Pada Pria."
  },
  {
    "en": "Apa Itu Identitas Ego?",
    "id": "Pemahaman Diri Konsisten (Konsep Erikson)."
  },
  {
    "en": "Apa Itu Krisis Identitas?",
    "id": "Periode Eksplorasi Identitas Masa Remaja."
  },
  {
    "en": "Apa Itu Moratorium Identitas?",
    "id": "Status Krisis, Belum Ada Komitmen."
  },
  {
    "en": "Apa Itu Pencapaian Identitas?",
    "id": "Status Setelah Krisis, Sudah Komitmen."
  },
  {
    "en": "Apa Itu Foreclosure Identitas?",
    "id": "Status Komitmen Tanpa Mengalami Krisis."
  },
  {
    "en": "Apa Itu Difusi Identitas?",
    "id": "Status Belum Krisis, Belum Komitmen."
  },
  {
    "en": "Siapa James Marcia?",
    "id": "Pengembang Empat Status Identitas."
  },
  {
    "en": "Apa Itu 'Personal Fable' Remaja?",
    "id": "Keyakinan Diri Unik, Tak Terkalahkan."
  },
  {
    "en": "Apa Itu 'Imaginary Audience' Remaja?",
    "id": "Perasaan Selalu Diperhatikan Orang Lain."
  },
  {
    "en": "Siapa David Elkind?",
    "id": "Pencetus Konsep Egosentrisme Remaja."
  },
  {
    "en": "Apa Itu Teori Aktivitas Penuaan?",
    "id": "Lansia Bahagia Jika Tetap Aktif."
  },
  {
    "en": "Apa Itu Teori Disengagement Penuaan?",
    "id": "Lansia Mundur Bertahap Dari Sosial."
  },
  {
    "en": "Apa Itu Demensia?",
    "id": "Penurunan Kognitif Berat."
  },
  {
    "en": "Apa Itu Penyakit Alzheimer?",
    "id": "Penyebab Umum Demensia."
  },
  {
    "en": "Apa Lima Tahap Berduka (Kubler-Ross)?",
    "id": "Penolakan, Marah, Tawar-Menawar, Depresi, Penerimaan."
  },
  {
    "en": "Apa Itu Perilaku Prososial?",
    "id": "Perilaku Menguntungkan Orang Lain."
  },
  {
    "en": "Apa Itu Perilaku Antisosial?",
    "id": "Perilaku Merugikan Orang Lain."
  },
  {
    "en": "Apa Itu Agresi?",
    "id": "Perilaku Bertujuan Menyakiti Orang Lain."
  },
  {
    "en": "Apa Itu Agresi Instrumental?",
    "id": "Agresi Untuk Mencapai Tujuan Tertentu."
  },
  {
    "en": "Apa Itu Agresi Permusuhan (Hostile)?",
    "id": "Agresi Didorong Kemarahan."
  },
  {
    "en": "Apa Itu Hipotesis Frustrasi-Agresi?",
    "id": "Frustrasi Meningkatkan Kemungkinan Agresi."
  },
  {
    "en": "Apa Itu Katarsis?",
    "id": "Pelepasan Energi Emosional (Misal: Marah)."
  },
  {
    "en": "Apa Itu Tes Kesiapan Sekolah?",
    "id": "Mengukur Kesiapan Anak Masuk Sekolah."
  },
  {
    "en": "Apa Itu Tes Kematangan Sosial (Social Maturity)?",
    "id": "Mengukur Kemampuan Adaptasi Sosial."
  },
  {
    "en": "Apa Itu Tes VMI (Visual Motor Integration)?",
    "id": "Tes Koordinasi Visual Dan Motorik."
  },
  {
    "en": "Apa Itu Tes Frostig?",
    "id": "Tes Mengukur Persepsi Visual."
  },
  {
    "en": "Apa Itu Tes Bender-Gestalt?",
    "id": "Tes Persepsi Visual Motorik."
  },
  {
    "en": "Apa Itu Disgrafia?",
    "id": "Kesulitan Belajar Menulis."
  },
  {
    "en": "Apa Itu Diskalkulia?",
    "id": "Kesulitan Belajar Matematika."
  },
  {
    "en": "Apa Itu Gangguan Spektrum Autisme (ASD)?",
    "id": "Gangguan Neurodevelopmental Interaksi Sosial."
  },
  {
    "en": "Apa Itu Tes Vineland?",
    "id": "Skala Perilaku Adaptif Vineland."
  },
  {
    "en": "Apa Itu Perilaku Adaptif?",
    "id": "Keterampilan Hidup Sehari-Hari."
  },
  {
    "en": "Apa Itu Sindrom Asperger?",
    "id": "Bentuk Autisme Fungsi Tinggi (Dulu)."
  },
  {
    "en": "Apa Itu Ekolalia?",
    "id": "Mengulang Ucapan Orang Lain."
  },
  {
    "en": "Apa Itu Tes Saringan Denver?",
    "id": "Tes Skrining Perkembangan Anak."
  },
  {
    "en": "Apa Itu Motorik Kasar?",
    "id": "Gerakan Menggunakan Otot Besar."
  },
  {
    "en": "Apa Itu Motorik Halus?",
    "id": "Gerakan Menggunakan Otot Kecil."
  },
  {
    "en": "Apa Itu Lateralitas?",
    "id": "Preferensi Penggunaan Sisi Tubuh."
  },
  {
    "en": "Apa Itu Kidal?",
    "id": "Preferensi Menggunakan Tangan Kiri."
  },
  {
    "en": "Apa Itu Psikologi Klinis Anak?",
    "id": "Fokus Masalah Psikologis Anak."
  },
  {
    "en": "Apa Itu Terapi Bermain (Play Therapy)?",
    "id": "Terapi Menggunakan Bermain."
  },
  {
    "en": "Apa Itu Bullying?",
    "id": "Agresi Berulang Oleh Pihak Kuat."
  },
  {
    "en": "Apa Itu Cyberbullying?",
    "id": "Bullying Menggunakan Teknologi Digital."
  },
  {
    "en": "Apa Itu Kecanduan Internet?",
    "id": "Penggunaan Internet Kompulsif Berlebihan."
  },
  {
    "en": "Apa Itu Nomofobia?",
    "id": "Takut Berlebihan Tanpa Ponsel."
  },
  {
    "en": "Apa Itu Fear Of Missing Out (FOMO)?",
    "id": "Takut Ketinggalan Momen Berharga."
  },
  {
    "en": "Apa Itu Joy Of Missing Out (JOMO)?",
    "id": "Menikmati Momen Tanpa Takut Ketinggalan."
  },
  {
    "en": "Apa Itu Self-Disclosure?",
    "id": "Proses Mengungkapkan Informasi Pribadi."
  },
  {
    "en": "Apa Itu Teori Penetrasi Sosial?",
    "id": "Hubungan Berkembang Dari Dangkal Mendalam."
  },
  {
    "en": "Apa Itu Neuropsikologi?",
    "id": "Studi Hubungan Otak Dan Perilaku."
  },
  {
    "en": "Apa Itu Tes Stroop?",
    "id": "Tes Mengukur Waktu Reaksi Kognitif."
  },
  {
    "en": "Apa Itu Efek Stroop?",
    "id": "Perlambatan Reaksi Membaca Warna Tinta."
  },
  {
    "en": "Apa Itu Tes Wisconsin Card Sorting (WCST)?",
    "id": "Tes Mengukur Fleksibilitas Kognitif."
  },
  {
    "en": "Apa Itu Tes Trail Making (TMT)?",
    "id": "Tes Mengukur Perhatian, Kecepatan Visual."
  },
  {
    "en": "Apa Itu Tes Halstead-Reitan?",
    "id": "Baterai Tes Neuropsikologi Komprehensif."
  },
  {
    "en": "Apa Itu Tes Luria-Nebraska?",
    "id": "Baterai Tes Neuropsikologi Lainnya."
  },
  {
    "en": "Apa Itu Afasia?",
    "id": "Gangguan Bahasa Akibat Kerusakan Otak."
  },
  {
    "en": "Apa Itu Area Broca?",
    "id": "Area Otak Untuk Produksi Bahasa."
  },
  {
    "en": "Apa Itu Afasia Broca?",
    "id": "Kesulitan Memproduksi Ucapan Lancar."
  },
  {
    "en": "Apa Itu Area Wernicke?",
    "id": "Area Otak Untuk Pemahaman Bahasa."
  },
  {
    "en": "Apa Itu Afasia Wernicke?",
    "id": "Kesulitan Memahami Bahasa."
  },
  {
    "en": "Apa Itu Corpus Callosum?",
    "id": "Jalur Saraf Penghubung Dua Belahan Otak."
  },
  {
    "en": "Apa Itu Pasien Split-Brain?",
    "id": "Pasien Corpus Callosum Terputus."
  },
  {
    "en": "Apa Itu Plastisitas Otak?",
    "id": "Kemampuan Otak Berubah, Beradaptasi."
  },
  {
    "en": "Apa Itu Neurogenesis?",
    "id": "Pembentukan Sel Saraf Baru."
  },
  {
    "en": "Apa Itu Tes Intelegensi Culture Fair?",
    "id": "Tes Dirancang Mengurangi Bias Budaya."
  },
  {
    "en": "Apa Itu Tes DAM (Draw-A-Man)?",
    "id": "Tes Gambar Orang Oleh Goodenough."
  },
  {
    "en": "Apa Itu Psikologi Konsumen?",
    "id": "Studi Perilaku Pembelian Konsumen."
  },
  {
    "en": "Apa Itu Branding?",
    "id": "Proses Penciptaan Citra Merek."
  },
  {
    "en": "Apa Itu Loyalitas Merek?",
    "id": "Komitmen Konsumen Membeli Merek Tertentu."
  },
  {
    "en": "Apa Itu Subliminal Perception?",
    "id": "Persepsi Stimulus Dibawah Ambang Sadar."
  },
  {
    "en": "Apa Itu Psikologi Kriminal?",
    "id": "Studi Pikiran, Niat, Reaksi Kriminal."
  },
  {
    "en": "Apa Itu Profiling Kriminal?",
    "id": "Membuat Profil Pelaku Berdasar Bukti."
  },
  {
    "en": "Apa Itu Sosiopat?",
    "id": "Istilah Lama Untuk Antisosial."
  },
  {
    "en": "Apa Itu Triad MacDonald?",
    "id": "Tiga Perilaku Prediktor Kekerasan (Dulu)."
  },
  {
    "en": "Apa Itu Tes Lüscher?",
    "id": "Tes Psikologi Menggunakan Persepsi Warna."
  },
  {
    "en": "Apa Itu Grafologi?",
    "id": "Analisis Tulisan Tangan Mengukur Kepribadian."
  },
  {
    "en": "Apakah Grafologi Valid Ilmiah?",
    "id": "Tidak Dianggap Valid Secara Ilmiah."
  },
  {
    "en": "Apa Itu Efek Barnum?",
    "id": "Kecenderungan Menerima Deskripsi Umum Diri."
  },
  {
    "en": "Apa Itu Cold Reading?",
    "id": "Teknik Menebak Informasi Spesifik."
  },
  {
    "en": "Apa Itu Validitas Wajah (Face Validity)?",
    "id": "Sejauh Mana Tes Tampak Mengukur."
  },
  {
    "en": "Apa Itu Hipnosis?",
    "id": "Kondisi Kesadaran Terfokus."
  },
  {
    "en": "Apa Itu Sugesti Paska-Hipnosis?",
    "id": "Perintah Dijalankan Setelah Hipnosis."
  },
  {
    "en": "Apa Itu Terapi EMDR (Eye Movement Desensitization and Reprocessing)?",
    "id": "Terapi Memproses Ulang Trauma."
  },
  {
    "en": "Siapa Francine Shapiro?",
    "id": "Pengembang Terapi EMDR."
  },
  {
    "en": "Apa Itu Terapi Keluarga?",
    "id": "Terapi Melibatkan Anggota Keluarga."
  },
  {
    "en": "Apa Itu Terapi Kelompok?",
    "id": "Terapi Dilakukan Dalam Kelompok."
  },
  {
    "en": "Apa Itu Psikodinamika?",
    "id": "Teori Fokus Kekuatan Bawah Sadar."
  },
  {
    "en": "Apa Itu Transferensi (Transference)?",
    "id": "Perasaan Klien Ke Terapis."
  },
  {
    "en": "Apa Itu Kontratransferensi (Countertransference)?",
    "id": "Respon Emosional Terapis Ke Klien."
  },
  {
    "en": "Apa Itu Asosiasi Bebas?",
    "id": "Teknik Psikoanalisis Mengungkapkan Pikiran."
  },
  {
    "en": "Apa Itu Analisis Mimpi?",
    "id": "Teknik Psikoanalisis Menafsirkan Mimpi."
  },
  {
    "en": "Apa Itu Konten Laten Mimpi?",
    "id": "Makna Tersembunyi Mimpi (Freud)."
  },
  {
    "en": "Apa Itu Konten Manifest Mimpi?",
    "id": "Alur Cerita Aktual Mimpi (Freud)."
  },
  {
    "en": "Apa Itu Arketipe (Archetype) Jung?",
    "id": "Simbol Universal Bawah Sadar Kolektif."
  },
  {
    "en": "Apa Itu Bawah Sadar Kolektif?",
    "id": "Konsep Jung Ingatan Warisan Universal."
  },
  {
    "en": "Apa Itu Persona (Jung)?",
    "id": "Topeng Sosial Yang Kita Tampilkan."
  },
  {
    "en": "Apa Itu Shadow (Jung)?",
    "id": "Sisi Gelap Kepribadian Kita."
  },
  {
    "en": "Apa Itu Anima (Jung)?",
    "id": "Aspek Feminin Dalam Pria."
  },
  {
    "en": "Apa Itu Animus (Jung)?",
    "id": "Aspek Maskulin Dalam Wanita."
  },
  {
    "en": "Apa Itu Individuasi (Jung)?",
    "id": "Proses Menjadi Diri Sendiri Sepenuhnya."
  },
  {
    "en": "Apa Itu Teori Atribusi?",
    "id": "Proses Menjelaskan Penyebab Perilaku."
  },
  {
    "en": "Apa Itu Atribusi Internal (Disposisional)?",
    "id": "Menyalahkan Faktor Kepribadian."
  },
  {
    "en": "Apa Itu Atribusi Eksternal (Situasional)?",
    "id": "Menyalahkan Faktor Situasi."
  },
  {
    "en": "Apa Itu Self-Serving Bias?",
    "id": "Bias Sukses (Internal), Gagal (Eksternal)."
  },
  {
    "en": "Apa Itu Efek Just-World?",
    "id": "Keyakinan Dunia Adil (Dapat Buruk)."
  },
  {
    "en": "Apa Itu Teori Keadilan (Equity Theory)?",
    "id": "Motivasi Keseimbangan Input Output Hubungan."
  },
  {
    "en": "Apa Itu Teori Pertukaran Sosial?",
    "id": "Hubungan Didasarkan Analisis Untung Rugi."
  },
  {
    "en": "Apa Itu Ketaatan (Obedience)?",
    "id": "Patuh Pada Perintah Otoritas."
  },
  {
    "en": "Apa Itu Foot-In-The-Door Technique?",
    "id": "Memulai Permintaan Kecil Dulu."
  },
  {
    "en": "Apa Itu Door-In-The-Face Technique?",
    "id": "Memulai Permintaan Besar Dulu."
  },
  {
    "en": "Apa Itu Low-Balling Technique?",
    "id": "Menawarkan Harga Baik, Lalu Menaikkan."
  },
  {
    "en": "Apa Itu Persuasi?",
    "id": "Proses Mengubah Sikap Seseorang."
  },
  {
    "en": "Apa Itu Rute Sentral Persuasi?",
    "id": "Persuasi Menggunakan Argumen Logis."
  },
  {
    "en": "Apa Itu Rute Periferal Persuasi?",
    "id": "Persuasi Menggunakan Isyarat Dangkal."
  },
  {
    "en": "Apa Itu Efek Sleeper?",
    "id": "Pesan Awalnya Lemah Menjadi Kuat."
  },
  {
    "en": "Apa Itu Inokulasi Sikap?",
    "id": "Membuat Orang Kebal Persuasi."
  },
  {
    "en": "Apa Itu Social Loafing?",
    "id": "Berkurangnya Usaha Saat Bekerja Kelompok."
  },
  {
    "en": "Apa Itu Fasilitasi Sosial?",
    "id": "Peningkatan Kinerja Dihadapan Orang Lain."
  },
  {
    "en": "Apa Itu Social Impairment?",
    "id": "Penurunan Kinerja Dihadapan Orang Lain."
  },
  {
    "en": "Apa Itu Ambang Batas Absolut?",
    "id": "Stimulasi Minimum Yang Dapat Dideteksi."
  },
  {
    "en": "Apa Itu Ambang Batas Diferensial?",
    "id": "Perbedaan Terkecil Antar Stimulus."
  },
  {
    "en": "Apa Itu Hukum Weber?",
    "id": "Perbedaan Stimulus Proporsional."
  },
  {
    "en": "Apa Itu Adaptasi Sensorik?",
    "id": "Penurunan Sensitivitas Stimulus Konstan."
  },
  {
    "en": "Apa Itu Persepsi Subliminal?",
    "id": "Deteksi Stimulus Dibawah Ambang Sadar."
  },
  {
    "en": "Apa Itu Constancy (Persepsi)?",
    "id": "Menganggap Objek Stabil (Ukuran, Warna)."
  },
  {
    "en": "Apa Itu Ukuran Constancy?",
    "id": "Objek Terlihat Sama Ukurannya."
  },
  {
    "en": "Apa Itu Warna Constancy?",
    "id": "Objek Terlihat Sama Warnanya."
  },
  {
    "en": "Apa Itu Bentuk Constancy?",
    "id": "Objek Terlihat Sama Bentuknya."
  },
  {
    "en": "Apa Itu Ilusi Müller-Lyer?",
    "id": "Ilusi Optik Panjang Garis Berbeda."
  },
  {
    "en": "Apa Itu Ilusi Ponzo?",
    "id": "Ilusi Optik Ukuran Relatif."
  },
  {
    "en": "Apa Itu Teori Deteksi Sinyal?",
    "id": "Memprediksi Deteksi Sinyal Lemah."
  },
  {
    "en": "Apa Itu Pengkondisian Klasik?",
    "id": "Belajar Asosiasi Antara Dua Stimulus."
  },
  {
    "en": "Apa Itu Stimulus Tidak Terkondisi (UCS)?",
    "id": "Stimulus Alami Memicu Respon."
  },
  {
    "en": "Apa Itu Respon Tidak Terkondisi (UCR)?",
    "id": "Respon Alami Terhadap UCS."
  },
  {
    "en": "Apa Itu Stimulus Terkondisi (CS)?",
    "id": "Stimulus Netral Menjadi Terkondisi."
  },
  {
    "en": "Apa Itu Respon Terkondisi (CR)?",
    "id": "Respon Belajar Terhadap CS."
  },
  {
    "en": "Apa Itu Akuisisi (Acquisition)?",
    "id": "Tahap Awal Belajar Asosiasi."
  },
  {
    "en": "Apa Itu Ekstingsi (Extinction)?",
    "id": "Melemahnya Respon Terkondisi."
  },
  {
    "en": "Apa Itu Pemulihan Spontan?",
    "id": "Munculnya Kembali CR Setelah Ekstingsi."
  },
  {
    "en": "Apa Itu Generalisasi Stimulus?",
    "id": "Respon Sama Stimulus Mirip CS."
  },
  {
    "en": "Apa Itu Diskriminasi Stimulus?",
    "id": "Respon Berbeda Stimulus Berbeda."
  },
  {
    "en": "Apa Itu Pengkondisian Operan?",
    "id": "Belajar Asosiasi Perilaku Konsekuensi."
  },
  {
    "en": "Apa Itu Hukum Efek Thorndike?",
    "id": "Perilaku Diikuti Hadiah Akan Diulang."
  },
  {
    "en": "Siapa Edward Thorndike?",
    "id": "Psikolog Dikenal Hukum Efek."
  },
  {
    "en": "Apa Itu Kotak Skinner?",
    "id": "Alat Eksperimen Pengkondisian Operan."
  },
  {
    "en": "Apa Itu Penguatan (Reinforcement)?",
    "id": "Konsekuensi Memperkuat Perilaku."
  },
  {
    "en": "Apa Itu Penguatan Positif?",
    "id": "Menambahkan Sesuatu Menyenangkan."
  },
  {
    "en": "Apa Itu Penguatan Negatif?",
    "id": "Menghilangkan Sesuatu Tidak Menyenangkan."
  },
  {
    "en": "Apa Itu Hukuman (Punishment)?",
    "id": "Konsekuensi Melemahkan Perilaku."
  },
  {
    "en": "Apa Itu Hukuman Positif?",
    "id": "Menambahkan Sesuatu Tidak Menyenangkan."
  },
  {
    "en": "Apa Itu Hukuman Negatif?",
    "id": "Menghilangkan Sesuatu Menyenangkan."
  },
  {
    "en": "Apa Itu Pembentukan (Shaping)?",
    "id": "Membentuk Perilaku Kompleks Bertahap."
  },
  {
    "en": "Apa Itu Jadwal Rasio Tetap?",
    "id": "Penguatan Setelah Jumlah Respon Tetap."
  },
  {
    "en": "Apa Itu Jadwal Rasio Variabel?",
    "id": "Penguatan Setelah Jumlah Respon Acak."
  },
  {
    "en": "Apa Itu Jadwal Interval Tetap?",
    "id": "Penguatan Setelah Waktu Tetap."
  },
  {
    "en": "Apa Itu Jadwal Interval Variabel?",
    "id": "Penguatan Setelah Waktu Acak."
  },
  {
    "en": "Mana Jadwal Paling Kuat?",
    "id": "Jadwal Rasio Variabel."
  },
  {
    "en": "Apa Itu Belajar Laten?",
    "id": "Belajar Tersembunyi Tanpa Penguatan Langsung."
  },
  {
    "en": "Apa Itu Peta Kognitif?",
    "id": "Representasi Mental Ruang (Tolman)."
  },
  {
    "en": "Siapa Edward Tolman?",
    "id": "Psikolog Konsep Peta Kognitif."
  },
  {
    "en": "Apa Itu Belajar Observasional?",
    "id": "Belajar Dengan Mengamati Orang Lain."
  },
  {
    "en": "Apa Itu Pemodelan (Modeling)?",
    "id": "Proses Meniru Perilaku Orang Lain."
  },
  {
    "en": "Apa Itu Emosi Primer?",
    "id": "Emosi Dasar Universal (Misal: Takut)."
  },
  {
    "en": "Apa Itu Emosi Sekunder?",
    "id": "Emosi Kompleks Gabungan (Misal: Cemburu)."
  },
  {
    "en": "Apa Itu Roda Emosi Plutchik?",
    "id": "Model Teori Emosi."
  },
  {
    "en": "Apa Itu Alexithymia?",
    "id": "Kesulitan Mengidentifikasi, Mendeskripsikan Emosi."
  },
  {
    "en": "Apa Itu Teori Drive-Reduction?",
    "id": "Motivasi Timbul Dari Kebutuhan Fisiologis."
  },
  {
    "en": "Apa Itu Homeostasis?",
    "id": "Keseimbangan Fisiologis Internal Tubuh."
  },
  {
    "en": "Apa Itu Teori Arousal Optimal?",
    "id": "Motivasi Mencari Tingkat Gairah Optimal."
  },
  {
    "en": "Apa Itu Hierarki Kebutuhan Maslow?",
    "id": "Teori Motivasi Kebutuhan Bertingkat."
  },
  {
    "en": "Apa Tingkat Terendah Maslow?",
    "id": "Kebutuhan Fisiologis (Makan, Tidur)."
  },
  {
    "en": "Apa Tingkat Kedua Maslow?",
    "id": "Kebutuhan Rasa Aman."
  },
  {
    "en": "Apa Tingkat Ketiga Maslow?",
    "id": "Kebutuhan Cinta Dan Rasa Memiliki."
  },
  {
    "en": "Apa Tingkat Keempat Maslow?",
    "id": "Kebutuhan Harga Diri."
  },
  {
    "en": "Apa Tingkat Puncak Maslow?",
    "id": "Kebutuhan Aktualisasi Diri."
  },
  {
    "en": "Apa Itu Aktualisasi Diri?",
    "id": "Pencapaian Potensi Penuh Diri."
  },
  {
    "en": "Apa Itu Motivasi Kompetensi?",
    "id": "Dorongan Untuk Menjadi Mampu, Efektif."
  },
  {
    "en": "Apa Itu Teori Self-Determination?",
    "id": "Motivasi Tiga Kebutuhan (Otonomi, Kompetensi, Keterhubungan)."
  },
  {
    "en": "Apa Itu Tes Rorschach Valid?",
    "id": "Validitasnya Masih Sangat Diperdebatkan."
  },
  {
    "en": "Apa Itu Tes TAT (Thematic Apperception Test) Valid?",
    "id": "Validitasnya Juga Diperdebatkan."
  }



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
