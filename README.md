<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Arduino Dasar</title>
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
        <h1>Pengetahuan Arduino Dasar</h1>
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


  { "en": "Kecandan?", "id": "Gurauan." },
  { "en": "Kecantol?", "id": "Tersangkut." },
  { "en": "Kecap?", "id": "Cicip." },
  { "en": "Kecapi?", "id": "Alat musik." },
  { "en": "Kece?", "id": "Tampan (prokem)." },
  { "en": "Kecebong?", "id": "Berudu." },
  { "en": "Kecele?", "id": "Tertipu." },
  { "en": "Kecemasan?", "id": "Kekhawatiran." },
  { "en": "Kecemerlangan?", "id": "Kegemilangan." },
  { "en": "Keceng?", "id": "Kelereng (arkais)." },
  { "en": "Kecepek?", "id": "Bunyi (arkais)." },
  { "en": "Kecer?", "id": "Alat musik." },
  { "en": "Kecerahan?", "id": "Kejelasan." },
  { "en": "Kecerobohan?", "id": "Kelalaian." },
  { "en": "Kecerdasan?", "id": "Kepintaran." },
  { "en": "Kecerdikan?", "id": "Kepandaian." },
  { "en": "Kecerewetan?", "id": "Kebawelan." },
  { "en": "Keceriaan?", "id": "Kegembiraan." },
  { "en": "Kecermatan?", "id": "Ketelitian." },
  { "en": "Kecuali?", "id": "Selain." },
  { "en": "Kecubung?", "id": "Tanaman." },
  { "en": "Kecumik?", "id": "Kumat-kamit." },
  { "en": "Kecundang?", "id": "Kalah." },
  { "en": "Kecup?", "id": "Cium." },
  { "en": "Kecurangan?", "id": "Ketidakjujuran." },
  { "en": "Kedabu?", "id": "Pohon." },
  { "en": "Kedah?", "id": "Terbuka." },
  { "en": "Kedai?", "id": "Warung." },
  { "en": "Kedal?", "id": "Belang kulit." },
  { "en": "Kedaluwarsa?", "id": "Lewat tempo." },
  { "en": "Kedam?", "id": "Lembap (arkais)." },
  { "en": "Kedamaian?", "id": "Ketenangan." },
  { "en": "Kedang?", "id": "Ulur (arkais)." },
  { "en": "Kedangkai?", "id": "Kurus (arkais)." },
  { "en": "Kedap?", "id": "Rapat." },
  { "en": "Kedaulatan?", "id": "Kekuasaan." },
  { "en": "Kedau?", "id": "Sakit (arkais)." },
  { "en": "Kedaung?", "id": "Pohon." },
  { "en": "Kedek?", "id": "Dekat (arkais)." },
  { "en": "Kedekai?", "id": "Burung." },
  { "en": "Kedekatan?", "id": "Keakraban." },
  { "en": "Kedelai?", "id": "Kacang." },
  { "en": "Kedemat?", "id": "Penyakit." },
  { "en": "Kedemplung?", "id": "Jatuh (Jawa)." },
  { "en": "Kedengkang?", "id": "Bunyi." },
  { "en": "Kedengkik?", "id": "Kurus kering." },
  { "en": "Keder?", "id": "Takut." },
  { "en": "Kedermawanan?", "id": "Kemurahan hati." },
  { "en": "Kedewasaan?", "id": "Kematangan." },
  { "en": "Kediaman?", "id": "Rumah." },
  { "en": "Kedigdayaan?", "id": "Kesaktian." },
  { "en": "Kedikit?", "id": "Sedikit." },
  { "en": "Kedip?", "id": "Kelip." },
  { "en": "Kedodoran?", "id": "Terlalu besar." },
  { "en": "Kedok?", "id": "Topeng." },
  { "en": "Kedondong?", "id": "Buah." },
  { "en": "Kedongkolan?", "id": "Kejengkelan." },
  { "en": "Kedot?", "id": "Kerut." },
  { "en": "Kedu?", "id": "Daerah." },
  { "en": "Kêdu?", "id": "Kedu (Jawa)." },
  { "en": "Keduduk?", "id": "Tanaman." },
  { "en": "Kedudukan?", "id": "Posisi." },
  { "en": "Keduk?", "id": "Gali." },
  { "en": "Kedul?", "id": "Tumpul (arkais)." },
  { "en": "Kedung?", "id": "Lubuk." },
  { "en": "Kedunguan?", "id": "Kebodohan." },
  { "en": "Kedut?", "id": "Kerut." },
  { "en": "Kedutaan?", "id": "Perwakilan negara." },
  { "en": "Keefektifan?", "id": "Kemanjuran." },
  { "en": "Keefisienan?", "id": "Kesangkilan." },
  { "en": "Kefa?", "id": "Batu (Yunani)." },
  { "en": "Keganasan?", "id": "Kebuatan." },
  { "en": "Keganjilan?", "id": "Keanehan." },
  { "en": "Kegantengan?", "id": "Ketampanan." },
  { "en": "Kegarangan?", "id": "Kegalakan." },
  { "en": "Kegatalan?", "id": "Kenakalan." },
  { "en": "Kegawatan?", "id": "Kedaruratan." },
  { "en": "Kegembiraan?", "id": "Kesenangan." },
  { "en": "Kegemilangan?", "id": "Kecemerlangan." },
  { "en": "Kegemukan?", "id": "Obesitas." },
  { "en": "Kegenitan?", "id": "Kecentilan." },
  { "en": "Kegentingan?", "id": "Kekritisan." },
  { "en": "Kegerahan?", "id": "Kepanasan." },
  { "en": "Kegesitan?", "id": "Ketangkasan." },
  { "en": "Kegigihan?", "id": "Ketekunan." },
  { "en": "Kegilaan?", "id": "Keedanan." },
  { "en": "Kegirangan?", "id": "Kegembiraan." },
  { "en": "Kegugupan?", "id": "Kepanikan." },
  { "en": "Keguguran?", "id": "Abortus." },
  { "en": "Kegunaan?", "id": "Manfaat." },
  { "en": "Kehalusan?", "id": "Kelembutan." },
  { "en": "Kehangatan?", "id": "Kepanasan sedang." },
  { "en": "Kehausan?", "id": "Kedahagaan." },
  { "en": "Kehebatan?", "id": "Keluarbiasaan." },
  { "en": "Kehebohan?", "id": "Kegemparan." },
  { "en": "Kehematan?", "id": "Keiritan." },
  { "en": "Kehendak?", "id": "Keinginan." },
  { "en": "Keheningan?", "id": "Kesunyian." },
  { "en": "Keheranan?", "id": "Ketakjuban." },
  { "en": "Kehormatan?", "id": "Kemuliaan." },
  { "en": "Kehujanan?", "id": "Terkena hujan." },
  { "en": "Kehutanan?", "id": "Perihal hutan." },
  { "en": "Keikhlasan?", "id": "Ketulusan." },
  { "en": "Keikutsertaan?", "id": "Partisipasi." },
  { "en": "Keilmuan?", "id": "Saintifik." },
  { "en": "Keindahan?", "id": "Keelokan." },
  { "en": "Keinginan?", "id": "Hasrat." },
  { "en": "Keinsafan?", "id": "Kesadaran." },
  { "en": "Keislaman?", "id": "Hal Islam." },
  { "en": "Keistimewaan?", "id": "Kekhususan." },
  { "en": "Kejadian?", "id": "Peristiwa." },
  { "en": "Kejahatan?", "id": "Kriminalitas." },
  { "en": "Kejam?", "id": "Bengis." },
  { "en": "Kejang?", "id": "Kaku otot." },
  { "en": "Kejar?", "id": "Buru." },
  { "en": "Kejelasan?", "id": "Keterangan." },
  { "en": "Keji?", "id": "Jahat." },
  { "en": "Kejijikan?", "id": "Kemuakan." },
  { "en": "Kejiwaan?", "id": "Mental." },
  { "en": "Kejora?", "id": "Bintang pagi." },
  { "en": "Keju?", "id": "Produk susu." },
  { "en": "Kejuaraan?", "id": "Kompetisi." },
  { "en": "Kejujuran?", "id": "Kelurusan hati." },
  { "en": "Kejuruan?", "id": "Vokasi." },
  { "en": "Kejut?", "id": "Kaget." },
  { "en": "Kekaguman?", "id": "Ketakjuban." },
  { "en": "Kekakuan?", "id": "Ketidaklenturan." },
  { "en": "Kekalahan?", "id": "Ketewasan." },
  { "en": "Kekal?", "id": "Abadi." },
  { "en": "Kekar?", "id": "Kuat." },
  { "en": "Kekasaran?", "id": "Ketidakhadiran." },
  { "en": "Kekasih?", "id": "Pacar." },
  { "en": "Kekayaan?", "id": "Harta." },
  { "en": "Kekerabatan?", "id": "Persaudaraan." },
  { "en": "Kekerdilan?", "id": "Ukuran kecil." },
  { "en": "Kekeringan?", "id": "Musim kemarau." },
  { "en": "Kekeruhan?", "id": "Ketidakjernihan." },
  { "en": "Kekesalan?", "id": "Kejengkelan." },
  { "en": "Kekhawatiran?", "id": "Kecemasan." },
  { "en": "Kekhilafan?", "id": "Kesalahan." },
  { "en": "Kekhususan?", "id": "Keistimewaan." },
  { "en": "Kekokohan?", "id": "Kekuatan." },
  { "en": "Kekolotan?", "id": "Kekunoan." },
  { "en": "Kekompakan?", "id": "Kesatuan." },
  { "en": "Kekosongan?", "id": "Kehampaan." },
  { "en": "Kekotoran?", "id": "Kenajisan." },
  { "en": "Kekuatan?", "id": "Tenaga." },
  { "en": "Kekunoan?", "id": "Keantikan." },
  { "en": "Kekurangan?", "id": "Defisit." },
  { "en": "Kelabakan?", "id": "Gugup." },
  { "en": "Kelabu?", "id": "Abu-abu." },
  { "en": "Keladi?", "id": "Talas." },
  { "en": "Kelahi?", "id": "Bertengkar." },
  { "en": "Kelainan?", "id": "Anomali." },
  { "en": "Kelajang?", "id": "Bujang." },
  { "en": "Kelak?", "id": "Nanti." },
  { "en": "Kelakar?", "id": "Canda." },
  { "en": "Kelalaian?", "id": "Kealpaan." },
  { "en": "Kelam?", "id": "Gelap." },
  { "en": "Kelamaan?", "id": "Terlalu lama." },
  { "en": "Kelambanan?", "id": "Kelembapan." },
  { "en": "Kelambu?", "id": "Tirai nyamuk." },
  { "en": "Kelamin?", "id": "Jenis." },
  { "en": "Kelana?", "id": "Pengembara." },
  { "en": "Kelancaran?", "id": "Kemudahan." },
  { "en": "Kelangkaan?", "id": "Kejarangan." },
  { "en": "Kelanjutan?", "id": "Kesinambungan." },
  { "en": "Kelapa?", "id": "Nyiur." },
  { "en": "Kelaparan?", "id": "Kekurangan makan." },
  { "en": "Kelas?", "id": "Golongan." },
  { "en": "Kelasi?", "id": "Pelaut." },
  { "en": "Kelayakan?", "id": "Kepatutan." },
  { "en": "Kelebatan?", "id": "Kilasan." },
  { "en": "Kelebihan?", "id": "Surplus." },
  { "en": "Keledai?", "id": "Hewan." },
  { "en": "Kelekatu?", "id": "Serangga." },
  { "en": "Kelelahan?", "id": "Kepenatan." },
  { "en": "Kelelawar?", "id": "Mamalia terbang." },
  { "en": "Keleluasaan?", "id": "Kebebasan." },
  { "en": "Kelemahan?", "id": "Kekurangan." },
  { "en": "Kelembutan?", "id": "Kehalusan." },
  { "en": "Kelenaan?", "id": "Kelalaian." },
  { "en": "Kelengkapan?", "id": "Atribut." },
  { "en": "Kelengangan?", "id": "Kesepian." },
  { "en": "Kelenjar?", "id": "Glandula." },
  { "en": "Kelenteng?", "id": "Klenteng." },
  { "en": "Kelepasan?", "id": "Kebebasan." },
  { "en": "Kelereng?", "id": "Guli." },
  { "en": "Kelesuan?", "id": "Kelemahan." },
  { "en": "Keletihan?", "id": "Kelelahan." },
  { "en": "Kelezatan?", "id": "Kenikmatan." },
  { "en": "Kelian?", "id": "Kepala desa (Bali)." },
  { "en": "Keliaran?", "id": "Kegelapan." },
  { "en": "Kelicikan?", "id": "Kecurangan." },
  { "en": "Keliling?", "id": "Sekitar." },
  { "en": "Kelim?", "id": "Lipatan jahitan." },
  { "en": "Kelinci?", "id": "Hewan." },
  { "en": "Kelip?", "id": "Kedip." },
  { "en": "Kelir?", "id": "Layar wayang." },
  { "en": "Keliru?", "id": "Salah." },
  { "en": "Kelobot?", "id": "Kulit jagung." },
  { "en": "Kelok?", "id": "Belokan." },
  { "en": "Kelola?", "id": "Urus." },
  { "en": "Kelompok?", "id": "Grup." },
  { "en": "Kelonggaran?", "id": "Keleluasaan." },
  { "en": "Kelontong?", "id": "Pedagang." },
  { "en": "Kelopak?", "id": "Bagian bunga." },
  { "en": "Kelu?", "id": "Bisu." },
  { "en": "Keluaga?", "id": "Keluarga (arkais)." },
  { "en": "Keluarga?", "id": "Sanaita." },
  { "en": "Keluaran?", "id": "Hasil." },
  { "en": "Keluh?", "id": "Kesah." },
  { "en": "Keluhan?", "id": "Aduan." },
  { "en": "Keluk?", "id": "Lengkung." },
  { "en": "Kelumit?", "id": "Sedikit." },
  { "en": "Kelupasan?", "id": "Pengelupasan." },
  { "en": "Kelurahan?", "id": "Wilayah administrasi." },
  { "en": "Keluron?", "id": "Keguguran." },
  { "en": "Kelurusan?", "id": "Kejujuran." },
  { "en": "Kelut?", "id": "Kacau (arkais)." },
  { "en": "Keluwesan?", "id": "Fleksibilitas." },
  { "en": "Kemah?", "id": "Tenda." },
  { "en": "Kemahiran?", "id": "Keahlian." },
  { "en": "Kemajuan?", "id": "Progres." },
  { "en": "Kemakmuran?", "id": "Kesejahteraan." },
  { "en": "Kemampuan?", "id": "Kapasitas." },
  { "en": "Kemanan?", "id": "Aman." },
  { "en": "Kemandirian?", "id": "Otonomi." },
  { "en": "Kemarahan?", "id": "Murka." },
  { "en": "Kemarau?", "id": "Musim kering." },
  { "en": "Kemarin?", "id": "Sehari lalu." },
  { "en": "Kematangan?", "id": "Kedewasaan." },
  { "en": "Kematian?", "id": "Ajal." },
  { "en": "Kembali?", "id": "Pulang." },
  { "en": "Kembar?", "id": "Sama." },
  { "en": "Kembara?", "id": "Kelana." },
  { "en": "Kembang?", "id": "Bunga." },
  { "en": "Kembol?", "id": "Saku." },
  { "en": "Kemelut?", "id": "Krisis." },
  { "en": "Kemerdekaan?", "id": "Kebebasan." },
  { "en": "Kemesraan?", "id": "Keakraban." },
  { "en": "Kemewahan?", "id": "Keglamoran." },
  { "en": "Kemiripan?", "id": "Kesamaan." },
  { "en": "Kemiskinan?", "id": "Kefakiran." },
  { "en": "Kemolekan?", "id": "Keindahan." },
  { "en": "Kempas?", "id": "Kembang-kempis." },
  { "en": "Kempis?", "id": "Kempas." },
  { "en": "Kemudi?", "id": "Setir." },
  { "en": "Kemudian?", "id": "Lalu." },
  { "en": "Kemudahan?", "id": "Fasilitas." },
  { "en": "Kemuliaan?", "id": "Keagungan." },
  { "en": "Kemunduran?", "id": "Regresi." },
  { "en": "Kemungkinan?", "id": "Probabilitas." },
  { "en": "Kemunafikan?", "id": "Hipokrisi." },
  { "en": "Kemunculan?", "id": "Penampakan." },
  { "en": "Kemurahan?", "id": "Kedermawanan." },
  { "en": "Kemurkaan?", "id": "Amarah." },
  { "en": "Kemurnian?", "id": "Keaslian." },
  { "en": "Kemusnahan?", "id": "Kepunahan." },
  { "en": "Kena?", "id": "Terkena." },
  { "en": "Kenalan?", "id": "Orang dikenal." },
  { "en": "Kenamaan?", "id": "Terkemuka." },
  { "en": "Kenang?", "id": "Ingat." },
  { "en": "Kenanga?", "id": "Bunga." },
  { "en": "Kenapa?", "id": "Mengapa." },
  { "en": "Kencan?", "id": "Pertemuan." },
  { "en": "Kencang?", "id": "Cepat." },
  { "en": "Kencing?", "id": "Buang air kecil." },
  { "en": "Kendala?", "id": "Hambatan." },
  { "en": "Kendali?", "id": "Kontrol." },
  { "en": "Kendaraan?", "id": "Transportasi." },
  { "en": "Kendi?", "id": "Wadah air." },
  { "en": "Kenduri?", "id": "Selamatan." },
  { "en": "Kental?", "id": "Pekat." },
  { "en": "Kentara?", "id": "Jelas." },
  { "en": "Kentut?", "id": "Buang angin." },
  { "en": "Kenyataan?", "id": "Fakta." },
  { "en": "Kenyal?", "id": "Elastis." },
  { "en": "Kenyang?", "id": "Puas makan." },
  { "en": "Keok?", "id": "Kalah." },
  { "en": "Keoptimisan?", "id": "Optimisme." },
  { "en": "Keorisinalan?", "id": "Keaslian." },
  { "en": "Kepada?", "id": "Untuk." },
  { "en": "Kepahitan?", "id": "Kegetiran." },
  { "en": "Kepakaran?", "id": "Keahlian." },
  { "en": "Kepala?", "id": "Pimpinan." },
  { "en": "Kepalsuan?", "id": "Ketidakaslian." },
  { "en": "Kepandaian?", "id": "Kecerdasan." },
  { "en": "Kepanikan?", "id": "Kegugupan." },
  { "en": "Kepanjangan?", "id": "Terlalu panjang." },
  { "en": "Kepantasan?", "id": "Kelayakan." },
  { "en": "Kepastian?", "id": "Ketentuan." },
  { "en": "Kepatuhan?", "id": "Ketaatan." },
  { "en": "Kepekaan?", "id": "Sensitivitas." },
  { "en": "Kepedihan?", "id": "Kesedihan." },
  { "en": "Kepedulian?", "id": "Perhatian." },
  { "en": "Kepegawaian?", "id": "Personalia." },
  { "en": "Kepeloporan?", "id": "Perintisan." },
  { "en": "Kepemilikan?", "id": "Hak milik." },
  { "en": "Kepemimpinan?", "id": "Manajemen." },
  { "en": "Kependekan?", "id": "Singkatan." },
  { "en": "Kependetaan?", "id": "Imamat." },
  { "en": "Kependudukan?", "id": "Demografi." },
  { "en": "Kepentingan?", "id": "Urusan." },
  { "en": "Kepenghuluan?", "id": "Jabatan penghulu." },
  { "en": "Kepergian?", "id": "Berangkat." },
  { "en": "Kepercayaan?", "id": "Keyakinan." },
  { "en": "Keperluan?", "id": "Kebutuhan." },
  { "en": "Kepesatan?", "id": "Kecepatan." },
  { "en": "Kepiluan?", "id": "Kesedihan." },
  { "en": "Kepincangan?", "id": "Ketidakseimbangan." },
  { "en": "Kepintaran?", "id": "Kecerdasan." },
  { "en": "Kepiting?", "id": "Rajungan." },
  { "en": "Keplok?", "id": "Tepuk tangan." },
  { "en": "Kepompong?", "id": "Pupa." },
  { "en": "Kepopuleran?", "id": "Ketenaran." },
  { "en": "Képrét?", "id": "Percik (Sunda)." },
  { "en": "Kepribadian?", "id": "Karakter." },
  { "en": "Keprihatinan?", "id": "Kekhawatiran." },
  { "en": "Kepruk?", "id": "Pukul." },
  { "en": "Kepul?", "id": "Asap." },
  { "en": "Kepulaga?", "id": "Kapulaga." },
  { "en": "Kepulauan?", "id": "Gugusan pulau." },
  { "en": "Kepunahan?", "id": "Kelenyapan." },
  { "en": "Kepurbaan?", "id": "Kekunoan." },
  { "en": "Kepustakaan?", "id": "Literatur." },
  { "en": "Kepuasan?", "id": "Kesenangan." },
  { "en": "Keputihan?", "id": "Penyakit wanita." },
  { "en": "Keputusan?", "id": "Ketetapan." },
  { "en": "Kera?", "id": "Monyet." },
  { "en": "Kerabat?", "id": "Saudara." },
  { "en": "Kerabik?", "id": "Sobek." },
  { "en": "Kerabu?", "id": "Perhiasan telinga." },
  { "en": "Keracak?", "id": "Cepat (arkais)." },
  { "en": "Kerah?", "id": "Bagian leher baju." },
  { "en": "Kerai?", "id": "Tirai bambu." },
  { "en": "Kerajinan?", "id": "Ketekunan." },
  { "en": "Kerajaan?", "id": "Negara raja." },
  { "en": "Kerak?", "id": "Lapisan keras." },
  { "en": "Kerakal?", "id": "Batu kecil." },
  { "en": "Keramat?", "id": "Suci." },
  { "en": "Keramas?", "id": "Cuci rambut." },
  { "en": "Keramba?", "id": "Kurungan ikan." },
  { "en": "Keramik?", "id": "Tembikar." },
  { "en": "Keran?", "id": "Pancuran air." },
  { "en": "Keranda?", "id": "Usungan mayat." },
  { "en": "Kerang?", "id": "Hewan bercangkang." },
  { "en": "Kerangka?", "id": "Rangka." },
  { "en": "Kerangkeng?", "id": "Kurungan." },
  { "en": "Kerani?", "id": "Juru tulis." },
  { "en": "Keranjang?", "id": "Wadah anyaman." },
  { "en": "Keranjingan?", "id": "Ketagihan." },
  { "en": "Keranji?", "id": "Pohon." },
  { "en": "Kerap?", "id": "Sering." },
  { "en": "Kerapatan?", "id": "Densitas." },
  { "en": "Kerapu?", "id": "Ikan." },
  { "en": "Keras?", "id": "Kuat." },
  { "en": "Kerasan?", "id": "Betah (Jawa)." },
  { "en": "Kerat?", "id": "Potong." },
  { "en": "Keratin?", "id": "Protein rambut." },
  { "en": "Keraton?", "id": "Istana." },
  { "en": "Kerawai?", "id": "Serangga." },
  { "en": "Kerawak?", "id": "Tidak teliti." },
  { "en": "Kerawang?", "id": "Ukiran tembus." },
  { "en": "Kerawat?", "id": "Rotan." },
  { "en": "Kerbang?", "id": "Kibas (arkais)." },
  { "en": "Kerbau?", "id": "Hewan ternak." },
  { "en": "Kercap?", "id": "Bunyi." },
  { "en": "Kercing?", "id": "Bunyi." },
  { "en": "Kercit?", "id": "Bunyi." },
  { "en": "Kerdak?", "id": "Endapan." },
  { "en": "Kerdam?", "id": "Bunyi." },
  { "en": "Kerdil?", "id": "Kate." },
  { "en": "Kerdip?", "id": "Kedip." },
  { "en": "Kerdud?", "id": "Kerut." },
  { "en": "Kerdus?", "id": "Kardus." },
  { "en": "Kere?", "id": "Miskin (kasar)." },
  { "en": "Kerebok?", "id": "Robek (arkais)." },
  { "en": "Kereceng?", "id": "Bunyi." },
  { "en": "Kerecut?", "id": "Mengkerut." },
  { "en": "Keredak?", "id": "Kotoran." },
  { "en": "Keredep?", "id": "Berkedip." },
  { "en": "Keredok?", "id": "Makanan." },
  { "en": "Kereket?", "id": "Bunyi." },
  { "en": "Keremi?", "id": "Cacing." },
  { "en": "Keremot?", "id": "Keriput." },
  { "en": "Kerempagi?", "id": "Kurus." },
  { "en": "Kerempeng?", "id": "Sangat kurus." },
  { "en": "Kerempung?", "id": "Perut." },
  { "en": "Keren?", "id": "Bagus." },
  { "en": "Kerencang?", "id": "Bunyi." },
  { "en": "Kereng?", "id": "Keras (tanah)." },
  { "en": "Kerengga?", "id": "Semut rangrang." },
  { "en": "Kerengkam?", "id": "Tumbuhan laut." },
  { "en": "Kerepek?", "id": "Keripik." },
  { "en": "Kerepes?", "id": "Raba." },
  { "en": "Keresek?", "id": "Bunyi daun kering." },
  { "en": "Kereseng?", "id": "Sombong." },
  { "en": "Keresot?", "id": "Geser." },
  { "en": "Kereta?", "id": "Kendaraan." },
  { "en": "Keretan?", "id": "Geretan." },
  { "en": "Keretek?", "id": "Rokok." },
  { "en": "Keri?", "id": "Ngeri." },
  { "en": "Keriaan?", "id": "Pesta." },
  { "en": "Keriang-keriut?", "id": "Bunyi." },
  { "en": "Kerias?", "id": "Pembersih." },
  { "en": "Keributan?", "id": "Kegaduhan." },
  { "en": "Kerical?", "id": "Budak." },
  { "en": "Keridas?", "id": "Kudis." },
  { "en": "Keridik?", "id": "Jangkrik." },
  { "en": "Kerih?", "id": "Meringis." },
  { "en": "Kerik?", "id": "Gores." },
  { "en": "Kerikal?", "id": "Keranjang." },
  { "en": "Kerikil?", "id": "Batu kecil." },
  { "en": "Kerikit?", "id": "Gigit." },
  { "en": "Kerimut?", "id": "Kerut." },
  { "en": "Kerinan?", "id": "Kesiangan (Jawa)." },
  { "en": "Kerincing?", "id": "Bunyi." },
  { "en": "Kerinduan?", "id": "Keinginan." },
  { "en": "Kering?", "id": "Tidak basah." },
  { "en": "Keringat?", "id": "Peluh." },
  { "en": "Keringanan?", "id": "Dispensasi." },
  { "en": "Kerinjal?", "id": "Loncat." },
  { "en": "Kerinting?", "id": "Keriting." },
  { "en": "Kerip?", "id": "Gigit." },
  { "en": "Keripik?", "id": "Makanan ringan." },
  { "en": "Keriput?", "id": "Kerut." },
  { "en": "Keris?", "id": "Senjata." },
  { "en": "Kerisi?", "id": "Ikan." },
  { "en": "Kerisik?", "id": "Bunyi." },
  { "en": "Kerising?", "id": "Menyeringai." },
  { "en": "Keristen?", "id": "Kristen." },
  { "en": "Kerisut?", "id": "Keriput." },
  { "en": "Kerit?", "id": "Gigit." },
  { "en": "Keritik?", "id": "Kritik." },
  { "en": "Keriting?", "id": "Ikal." },
  { "en": "Kerito?", "id": "Kereta (Jawa)." },
  { "en": "Keriuk?", "id": "Bunyi ayam." },
  { "en": "Keriut?", "id": "Bunyi." },
  { "en": "Kerja?", "id": "Pekerjaan." },
  { "en": "Kerjal?", "id": "Tidak rata." },
  { "en": "Kerjang?", "id": "Tendang." },
  { "en": "Kerjap?", "id": "Kedip." },
  { "en": "Kerjantara?", "id": "Pekerjaan sementara." },
  { "en": "Kerkah?", "id": "Gigit." },
  { "en": "Kerkap?", "id": "Gigit (arkais)." },
  { "en": "Kerkau?", "id": "Cakar." },
  { "en": "Kerkop?", "id": "Kuburan Belanda." },
  { "en": "Kerkup?", "id": "Kudis." },
  { "en": "Kerling?", "id": "Lirik." },
  { "en": "Kerlip?", "id": "Kelip." },
  { "en": "Kermak?", "id": "Cacing." },
  { "en": "Kermanici?", "id": "Daging." },
  { "en": "Kermes?", "id": "Pewarna merah." },
  { "en": "Kermi?", "id": "Cacing." },
  { "en": "Kernai?", "id": "Sayat." },
  { "en": "Kerneli?", "id": "Senapan (Belanda)." },
  { "en": "Kernu?", "id": "Tanduk (arkais)." },
  { "en": "Kernyat?", "id": "Nyaring." },
  { "en": "Kernyau?", "id": "Berisik." },
  { "en": "Kernyih?", "id": "Menyeringai." },
  { "en": "Kernyit?", "id": "Kerut." },
  { "en": "Kernyut?", "id": "Denyut." },
  { "en": "Kero?", "id": "Juling." },
  { "en": "Kerobak?", "id": "Robek." },
  { "en": "Kerobek?", "id": "Sobek." },
  { "en": "Keroco?", "id": "Bawahan." },
  { "en": "Kerogen?", "id": "Zat organik." },
  { "en": "Keroh?", "id": "Dengkur." },
  { "en": "Kerohanian?", "id": "Spiritual." },
  { "en": "Kerok?", "id": "Kikis." },
  { "en": "Kerokal?", "id": "Bising." },
  { "en": "Keromi?", "id": "Kromium." },
  { "en": "Keromong?", "id": "Gamelan." },
  { "en": "Kerona?", "id": "Mahkota matahari." },
  { "en": "Keroncang?", "id": "Bunyi." },
  { "en": "Keronco?", "id": "Rantai (arkais)." },
  { "en": "Keroncong?", "id": "Musik." },
  { "en": "Keroncor?", "id": "Belut." },
  { "en": "Kerondong?", "id": "Berlubang." },
  { "en": "Kerong?", "id": "Lubang (arkais)." },
  { "en": "Kerongkongan?", "id": "Tenggorokan." },
  { "en": "Kerongsang?", "id": "Bros." },
  { "en": "Kerontang?", "id": "Kering." },
  { "en": "Keropas?", "id": "Kikis." },
  { "en": "Keropeng?", "id": "Kudis kering." },
  { "en": "Keropos?", "id": "Rapuh." },
  { "en": "Kerosi?", "id": "Karosi." },
  { "en": "Kerosin?", "id": "Minyak tanah." },
  { "en": "Kerosok?", "id": "Bunyi." },
  { "en": "Kerotot?", "id": "Tidak rata." },
  { "en": "Keroyok?", "id": "Serang bersama." },
  { "en": "Kerpai?", "id": "Kapas." },
  { "en": "Kerpak?", "id": "Bunyi." },
  { "en": "Kerpas?", "id": "Bunyi." },
  { "en": "Kerpuk?", "id": "Bunyi." },
  { "en": "Kerpus?", "id": "Tutup kepala." },
  { "en": "Kers?", "id": "Ceri (Belanda)." },
  { "en": "Kersai?", "id": "Rennyah." },
  { "en": "Kersang?", "id": "Kering." },
  { "en": "Kersen?", "id": "Ceri." },
  { "en": "Kersik?", "id": "Pasir." },
  { "en": "Kertah?", "id": "Ulat (arkais)." },
  { "en": "Kertang?", "id": "Bunyi." },
  { "en": "Kertas?", "id": "Bahan tulis." },
  { "en": "Kertau?", "id": "Pohon." },
  { "en": "Kertuk?", "id": "Bunyi." },
  { "en": "Kertus?", "id": "Peluru (Belanda)." },
  { "en": "Keruan?", "id": "Tentu." },
  { "en": "Keruan?", "id": "Pasti." },
  { "en": "Kerubim?", "id": "Malaikat." },
  { "en": "Kerubin?", "id": "Malaikat." },
  { "en": "Kerubung?", "id": "Kerumun." },
  { "en": "Kerucut?", "id": "Bentuk." },
  { "en": "Kerudung?", "id": "Penutup kepala." },
  { "en": "Keruh?", "id": "Tidak jernih." },
  { "en": "Keruing?", "id": "Pohon." },
  { "en": "Keruit?", "id": "Gerak." },
  { "en": "Keruk?", "id": "Gali." },
  { "en": "Kerukut?", "id": "Keriting." },
  { "en": "Kerul?", "id": "Ikal." },
  { "en": "Keruma?", "id": "Siput." },
  { "en": "Kerumun?", "id": "Kumpul." },
  { "en": "Kerumus?", "id": "Kumpul (arkais)." },
  { "en": "Kerun?", "id": "Turun (arkais)." },
  { "en": "Kerung?", "id": "Lekuk." },
  { "en": "Kerunku?", "id": "Tempayan (arkais)." },
  { "en": "Keruntang?", "id": "Peti mayat." },
  { "en": "Kerunting?", "id": "Bunyi." },
  { "en": "Keruntung?", "id": "Wadah." },
  { "en": "Kerunyut?", "id": "Kerut." },
  { "en": "Kerup?", "id": "Bunyi." },
  { "en": "Kerupuk?", "id": "Makanan ringan." },
  { "en": "Kerusi?", "id": "Kursi." },
  { "en": "Kerut?", "id": "Lipatan." },
  { "en": "Kerutan?", "id": "Lipatan kulit." },
  { "en": "Kerutup?", "id": "Bunyi." },
  { "en": "Keruwit?", "id": "Rumit." },
  { "en": "Kesa?", "id": "Pertama (Sanskerta)." },
  { "en": "Kesah?", "id": "Keluh." },
  { "en": "Kesak?", "id": "Seka." },
  { "en": "Kesal?", "id": "Jengkel." },
  { "en": "Kesambet?", "id": "Kena pengaruh roh." },
  { "en": "Kesambi?", "id": "Pohon." },
  { "en": "Kesan?", "id": "Impresi." },
  { "en": "Kesang?", "id": "Buang ingus." },
  { "en": "Kesangsang?", "id": "Tersangkut." },
  { "en": "Kesap?", "id": "Kering." },
  { "en": "Kesasar?", "id": "Tersesat." },
  { "en": "Kesat?", "id": "Tidak licin." },
  { "en": "Kesatria?", "id": "Ksatria." },
  { "en": "Kesek?", "id": "Gosok." },
  { "en": "Kesel?", "id": "Lelah (Jawa)." },
  { "en": "Keseleo?", "id": "Terilir." },
  { "en": "Kesemek?", "id": "Buah." },
  { "en": "Kesengsem?", "id": "Terpesona (Jawa)." },
  { "en": "Keseran?", "id": "Geser (arkais)." },
  { "en": "Keseser?", "id": "Terdampar." },
  { "en": "Keset?", "id": "Alas kaki." },
  { "en": "Kesi?", "id": "Riak (arkais)." },
  { "en": "Kesiap?", "id": "Terkejut." },
  { "en": "Kesik?", "id": "Gesit (arkais)." },
  { "en": "Kesima?", "id": "Terpana." },
  { "en": "Kesimbukan?", "id": "Tanaman." },
  { "en": "Kesinan?", "id": "Terlalu asin." },
  { "en": "Kesip?", "id": "Isap." },
  { "en": "Kesitu?", "id": "Ke situ." },
  { "en": "Kesmaran?", "id": "Kasmaran." },
  { "en": "Kesohor?", "id": "Terkenal." },
  { "en": "Kesomplok?", "id": "Terbentur." },
  { "en": "Kesot?", "id": "Geser." },
  { "en": "Kesting?", "id": "Renda (Belanda)." },
  { "en": "Kesturi?", "id": "Kasturi." },
  { "en": "Kesuma?", "id": "Bunga." },
  { "en": "Kesumat?", "id": "Dendam." },
  { "en": "Kesumba?", "id": "Pewarna merah." },
  { "en": "Kesup?", "id": "Isap." },
  { "en": "Kesusu?", "id": "Tergesa-gesa (Jawa)." },
  { "en": "Kesut?", "id": "Usap." },
  { "en": "Ketaban?", "id": "Tebal (arkais)." },
  { "en": "Ketagehan?", "id": "Ketagihan." },
  { "en": "Ketak?", "id": "Bunyi." },
  { "en": "Ketakong?", "id": "Siput." },
  { "en": "Ketakwaan?", "id": "Ketaatan agama." },
  { "en": "Ketam?", "id": "Kepiting." },
  { "en": "Ketampi?", "id": "Kusta (arkais)." },
  { "en": "Ketan?", "id": "Beras." },
  { "en": "Ketang?", "id": "Ikan." },
  { "en": "Ketap?", "id": "Gigit." },
  { "en": "Ketapang?", "id": "Pohon." },
  { "en": "Ketar?", "id": "Getar." },
  { "en": "Ketat?", "id": "Rapat." },
  { "en": "Ketaton?", "id": "Terluka (Jawa)." },
  { "en": "Ketawa?", "id": "Tertawa." },
  { "en": "Ketaya?", "id": "Ayunan." },
  { "en": "Ketayap?", "id": "Topi." },
  { "en": "Ketegar?", "id": "Keras (arkais)." },
  { "en": "Ketek?", "id": "Ketiak." },
  { "en": "Ketel?", "id": "Cerek." },
  { "en": "Ketela?", "id": "Ubi." },
  { "en": "Ketemu?", "id": "Jumpa." },
  { "en": "Keter?", "id": "Gemetar (arkais)." },
  { "en": "Keterangan?", "id": "Informasi." },
  { "en": "Keterlaluan?", "id": "Berlebihan." },
  { "en": "Keterlibatan?", "id": "Partisipasi." },
  { "en": "Ketersediaan?", "id": "Stok." },
  { "en": "Ketes?", "id": "Tetes." },
  { "en": "Ketiak?", "id": "Bagian bawah lengan." },
  { "en": "Ketial?", "id": "Keras kepala." },
  { "en": "Ketiau?", "id": "Pohon." },
  { "en": "Ketib?", "id": "Khatib." },
  { "en": "Ketiding?", "id": "Keranjang." },
  { "en": "Ketika?", "id": "Saat." },
  { "en": "Ketil?", "id": "Sentil." },
  { "en": "Ketilang?", "id": "Burung." },
  { "en": "Ketimbul?", "id": "Terapung." },
  { "en": "Ketimbung?", "id": "Bunyi." },
  { "en": "Ketimon?", "id": "Mentimun (Jawa)." },
  { "en": "Keting?", "id": "Ikan." },
  { "en": "Ketinggalan?", "id": "Terlambat." },
  { "en": "Ketip?", "id": "Gigit nyamuk." },
  { "en": "Ketipak?", "id": "Bunyi." },
  { "en": "Ketipung?", "id": "Gendang." },
  { "en": "Ketirah?", "id": "Tanaman." },
  { "en": "Ketis?", "id": "Sentil." },
  { "en": "Ketitir?", "id": "Burung." },
  { "en": "Keto?", "id": "Pincang (arkais)." },
  { "en": "Ketok?", "id": "Pukul." },
  { "en": "Keton?", "id": "Senyawa kimia." },
  { "en": "Ketonggeng?", "id": "Kalajengking." },
  { "en": "Ketoprak?", "id": "Makanan/Sandiwara." },
  { "en": "Ketu?", "id": "Tutup kepala (India)." },
  { "en": "Ketua?", "id": "Pemimpin." },
  { "en": "Ketuat?", "id": "Kutil." },
  { "en": "Ketubah?", "id": "Kontrak nikah Yahudi." },
  { "en": "Ketuban?", "id": "Cairan pelindung janin." },
  { "en": "Ketuhanan?", "id": "Perihal Tuhan." },
  { "en": "Ketuir?", "id": "Burung." },
  { "en": "Ketuk?", "id": "Bunyi." },
  { "en": "Ketul?", "id": "Tumpul." },
  { "en": "Ketumbar?", "id": "Rempah." },
  { "en": "Ketumbi?", "id": "Penyakit kulit." },
  { "en": "Ketumbit?", "id": "Tanaman." },
  { "en": "Ketumpang?", "id": "Tumbuhan." },
  { "en": "Ketun?", "id": "Luka bakar (arkais)." },
  { "en": "Ketungging?", "id": "Kalajengking." },
  { "en": "Ketupat?", "id": "Makanan." },
  { "en": "Kewajiban?", "id": "Keharusan." },
  { "en": "Kewalahan?", "id": "Repot." },
  { "en": "Kewarganegaraan?", "id": "Status warga." },
  { "en": "Kewenangan?", "id": "Otoritas." },
  { "en": "Kewibawaan?", "id": "Karisma." },
  { "en": "Kha?", "id": "Huruf Arab." },
  { "en": "Khabar?", "id": "Kabar (ejaan lama)." },
  { "en": "Khadam?", "id": "Pelayan (Arab)." },
  { "en": "Khafi?", "id": "Tersembunyi (Arab)." },
  { "en": "Khair?", "id": "Baik (Arab)." },
  { "en": "Khal?", "id": "Cuka (Arab)." },
  { "en": "Khalas?", "id": "Selesai (Arab)." },
  { "en": "Khalayak?", "id": "Publik." },
  { "en": "Khali?", "id": "Sunyi (Arab)." },
  { "en": "Khalifah?", "id": "Pemimpin Islam." },
  { "en": "Khalik?", "id": "Pencipta." },
  { "en": "Khalikul?", "id": "Pencipta (Arab)." },
  { "en": "Khalis?", "id": "Murni (Arab)." },
  { "en": "Khalkah?", "id": "Lingkaran (Arab)." },
  { "en": "Khamar?", "id": "Minuman keras (Arab)." },
  { "en": "Khamir?", "id": "Ragi." },
  { "en": "Khanah?", "id": "Rumah (Persia)." },
  { "en": "Kharab?", "id": "Rusak (Arab)." },
  { "en": "Khas?", "id": "Istimewa." },
  { "en": "Khasiat?", "id": "Manfaat." },
  { "en": "Khasumat?", "id": "Permusuhan (Arab)." },
  { "en": "Khat?", "id": "Tulisan Arab." },
  { "en": "Khatam?", "id": "Selesai." },
  { "en": "Khatib?", "id": "Pembaca khotbah." },
  { "en": "Khatifah?", "id": "Permadani (Arab)." },
  { "en": "Khatimah?", "id": "Akhir." },
  { "en": "Khatulistiwa?", "id": "Garis tengah bumi." },
  { "en": "Khauf?", "id": "Takut (Arab)." },
  { "en": "Khaul?", "id": "Peringatan kematian." },
  { "en": "Khawas?", "id": "Khusus (Arab)." },
  { "en": "Khawatir?", "id": "Cemas." },
  { "en": "Khayal?", "id": "Fantasi." },
  { "en": "Khayali?", "id": "Bersifat khayal." },
  { "en": "Khazanah?", "id": "Harta benda." },
  { "en": "Kheda?", "id": "Kandang gajah." },
  { "en": "Kher?", "id": "Baik (ejaan lama)." },
  { "en": "Khianat?", "id": "Ingkar janji." },
  { "en": "Khiar?", "id": "Pilihan (Islam)." },
  { "en": "Khidaah?", "id": "Tipu daya (Arab)." },
  { "en": "Khidmah?", "id": "Pelayanan." },
  { "en": "Khilaf?", "id": "Salah." },
  { "en": "Khilafah?", "id": "Kekhalifahan." },
  { "en": "Khilafiah?", "id": "Perbedaan pendapat." },
  { "en": "Khinzir?", "id": "Babi (Arab)." },
  { "en": "Khisit?", "id": "Nama tumbuhan." },
  { "en": "Khitah?", "id": "Garis perjuangan." },
  { "en": "Khitan?", "id": "Sunat." },
  { "en": "Khizanah?", "id": "Lemari (Arab)." },
  { "en": "Khoja?", "id": "Pedagang India." },
  { "en": "Khotbah?", "id": "Ceramah agama." },
  { "en": "Khuduk?", "id": "Tunduk (Arab)." },
  { "en": "Khulafa?", "id": "Para khalifah." },
  { "en": "Khuldi?", "id": "Surga keabadian." },
  { "en": "Khuluk?", "id": "Akhlak (Arab)." },
  { "en": "Khunsa?", "id": "Berkelamin ganda." },
  { "en": "Khurafat?", "id": "Takhayul." },
  { "en": "Khusuf?", "id": "Gerhana bulan." },
  { "en": "Khusus?", "id": "Spesial." },
  { "en": "Khusyuk?", "id": "Tekun (ibadah)." },
  { "en": "Ki?", "id": "Gelar kehormatan." },
  { "en": "Kia?", "id": "Kakak (arkais)." },
  { "en": "Kiah?", "id": "Ukur (arkais)." },
  { "en": "Kiai?", "id": "Ulama." },
  { "en": "Kiak?", "id": "Bunyi." },
  { "en": "Kial?", "id": "Gaya." },
  { "en": "Kiam?", "id": "Berdiri (salat)." },
  { "en": "Kiamat?", "id": "Hari akhir." },
  { "en": "Kiambang?", "id": "Tanaman air." },
  { "en": "Kian?", "id": "Makin." },
  { "en": "Kiang?", "id": "Kijang (arkais)." },
  { "en": "Kiani?", "id": "Kerajaan (Persia)." },
  { "en": "Kiano?", "id": "Alat musik." },
  { "en": "Kiap?", "id": "Lipat (arkais)." },
  { "en": "Kias?", "id": "Perumpamaan." },
  { "en": "Kiasan?", "id": "Perbandingan." },
  { "en": "Kiasmus?", "id": "Gaya bahasa." },
  { "en": "Kiat?", "id": "Cara." },
  { "en": "Kibar?", "id": "Berkibar." },
  { "en": "Kibas?", "id": "Goyang." },
  { "en": "Kibir?", "id": "Sombong." },
  { "en": "Kibik?", "id": "Kubik (arkais)." },
  { "en": "Kiblat?", "id": "Arah salat." },
  { "en": "Kicang?", "id": "Tipu (arkais)." },
  { "en": "Kicau?", "id": "Suara burung." },
  { "en": "Kici?", "id": "Kapal layar kecil." },
  { "en": "Kicu?", "id": "Tipu." },
  { "en": "Kicut?", "id": "Bunyi." },
  { "en": "Kidab?", "id": "Tipu (arkais)." },
  { "en": "Kidal?", "id": "Pengguna tangan kiri." },
  { "en": "Kidamat?", "id": "Layanan (Arab)." },
  { "en": "Kidang?", "id": "Kijang." },
  { "en": "Kidib?", "id": "Bohong (Arab)." },
  { "en": "Kidul?", "id": "Selatan (Jawa)." },
  { "en": "Kidung?", "id": "Nyanyian." },
  { "en": "Kifarat?", "id": "Kafarat." },
  { "en": "Kifayah?", "id": "Kecukupan." },
  { "en": "Kihanat?", "id": "Ramalan (arkais)." },
  { "en": "Kijai?", "id": "Tanaman." },
  { "en": "Kijang?", "id": "Rusa kecil." },
  { "en": "Kijing?", "id": "Kerang." },
  { "en": "Kik?", "id": "Kain." },
  { "en": "Kikih?", "id": "Tawa tertahan." },
  { "en": "Kikik?", "id": "Kikih." },
  { "en": "Kikil?", "id": "Kulit kaki sapi." },
  { "en": "Kikir?", "id": "Pelit." },
  { "en": "Kikis?", "id": "Hapus." },
  { "en": "Kikitir?", "id": "Surat pajak (Sunda)." },
  { "en": "Kiku?", "id": "Krisan (Jepang)." },
  { "en": "Kikuk?", "id": "Canggung." },
  { "en": "Kikus?", "id": "Kain." },
  { "en": "Kila?", "id": "Kilau (arkais)." },
  { "en": "Kilah?", "id": "Dalih." },
  { "en": "Kilai?", "id": "Kain." },
  { "en": "Kilak?", "id": "Kilat (arkais)." },
  { "en": "Kilal?", "id": "Berkilau." },
  { "en": "Kilang?", "id": "Tempat pengolahan." },
  { "en": "Kilap?", "id": "Cahaya." },
  { "en": "Kilas?", "id": "Cepat." },
  { "en": "Kilat?", "id": "Cahaya cepat." },
  { "en": "Kilau?", "id": "Sinar." },
  { "en": "Kili?", "id": "Alat pintal." },
  { "en": "Kilik?", "id": "Sentuh." },
  { "en": "Kilir?", "id": "Asah." },
  { "en": "Kilo?", "id": "Seribu (awalan)." },
  { "en": "Kiloton?", "id": "Satuan daya ledak." },
  { "en": "Kilovolt?", "id": "Satuan tegangan." },
  { "en": "Kilowatt?", "id": "Satuan daya." },
  { "en": "Kim?", "id": "Permainan." },
  { "en": "Kima?", "id": "Kerang besar." },
  { "en": "Kimah?", "id": "Harga (Arab)." },
  { "en": "Kimantu?", "id": "Sihir (arkais)." },
  { "en": "Kimar?", "id": "Judi (Arab)." },
  { "en": "Kimia?", "id": "Ilmu zat." },
  { "en": "Kimiagrafi?", "id": "Teknik cetak." },
  { "en": "Kimiawan?", "id": "Ahli kimia." },
  { "en": "Kimkha?", "id": "Kain sutra." },
  { "en": "Kimlo?", "id": "Masakan." },
  { "en": "Kimograf?", "id": "Alat rekam gerak." },
  { "en": "Kimono?", "id": "Pakaian Jepang." },
  { "en": "Kimpal?", "id": "Padat." },
  { "en": "Kimput?", "id": "Sempit (arkais)." },
  { "en": "Kimus?", "id": "Bubur lambung." },
  { "en": "Kina?", "id": "Pohon obat." },
  { "en": "Kinang?", "id": "Sirih." },
  { "en": "Kinantan?", "id": "Ayam putih." },
  { "en": "Kinca?", "id": "Air gula." },
  { "en": "Kincah?", "id": "Bersihkan." },
  { "en": "Kincak?", "id": "Lompat." },
  { "en": "Kincang?", "id": "Cepat." },
  { "en": "Kincau?", "id": "Kacau." },
  { "en": "Kincir?", "id": "Roda putar." },
  { "en": "Kincit?", "id": "Tahi." },
  { "en": "Kincu?", "id": "Pewarna." },
  { "en": "Kincup?", "id": "Kuncup." },
  { "en": "Kundai?", "id": "Sanggul." },
  { "en": "Kindap?", "id": "Sembunyi (arkais)." },
  { "en": "Kinesika?", "id": "Ilmu gerak tubuh." },
  { "en": "Kinesimeter?", "id": "Pengukur gerak." },
  { "en": "Kinetik?", "id": "Berkaitan gerak." },
  { "en": "Kinetika?", "id": "Ilmu laju reaksi." },
  { "en": "Kingfisher?", "id": "Burung." },
  { "en": "Kingking?", "id": "Jinjing (arkais)." },
  { "en": "Kingkip?", "id": "Rapat." },
  { "en": "Kingkong?", "id": "Kera raksasa." },
  { "en": "Kini?", "id": "Sekarang." },
  { "en": "Kinja?", "id": "Lompat." },
  { "en": "Kinjai?", "id": "Kunjung." },
  { "en": "Kinredio?", "id": "Jaringan radio." },
  { "en": "Kintaka?", "id": "Surat (arkais)." },
  { "en": "Kintal?", "id": "Kwintal." },
  { "en": "Kinyam?", "id": "Rasa (Minang)." },
  { "en": "Kinyang?", "id": "Batu." },
  { "en": "Kios?", "id": "Warung kecil." },
  { "en": "Kipa?", "id": "Topi." },
  { "en": "Kipai?", "id": "Kibas." },
  { "en": "Kipang?", "id": "Makanan ringan." },
  { "en": "Kipar?", "id": "Lari (arkais)." },
  { "en": "Kipas?", "id": "Alat pendingin." },
  { "en": "Kiper?", "id": "Penjaga gawang." },
  { "en": "Kiprah?", "id": "Gerak tari." },
  { "en": "Kiprat?", "id": "Percik." },
  { "en": "Kipsiau?", "id": "Mengaku (Tionghoa)." },
  { "en": "Kipu?", "id": "Simpul tali Inca." },
  { "en": "Kira?", "id": "Duga." },
  { "en": "Kirab?", "id": "Pawai." },
  { "en": "Kirai?", "id": "Kibas." },
  { "en": "Kiramat?", "id": "Keramat." },
  { "en": "Kiran?", "id": "Sinar (Sanskerta)." },
  { "en": "Kirana?", "id": "Cantik." },
  { "en": "Kirap?", "id": "Kibas." },
  { "en": "Kiras?", "id": "Ukuran isi." },
  { "en": "Kirbat?", "id": "Kantung kulit." },
  { "en": "Kiri?", "id": "Sisi." },
  { "en": "Kirik?", "id": "Anak anjing." },
  { "en": "Kirim?", "id": "Hantar." },
  { "en": "Kirinyu?", "id": "Tanaman." },
  { "en": "Kirmizi?", "id": "Merah tua." },
  { "en": "Kiru?", "id": "Mandi (arkais)." },
  { "en": "Kiruh?", "id": "Keruh." },
  { "en": "Kis?", "id": "Peti (Belanda)." },
  { "en": "Kisah?", "id": "Cerita." },
  { "en": "Kisai?", "id": "Ayak." },
  { "en": "Kisat?", "id": "Kering (arkais)." },
  { "en": "Kiset?", "id": "Kantong tembakau." },
  { "en": "Kisi?", "id": "Jeruji." },
  { "en": "Kismah?", "id": "Bagian (Arab)." },
  { "en": "Kismat?", "id": "Takdir." },
  { "en": "Kismis?", "id": "Anggur kering." },
  { "en": "Kisruh?", "id": "Kacau." },
  { "en": "Kista?", "id": "Kantung berisi cairan." },
  { "en": "Kisut?", "id": "Keriput." },
  { "en": "Kit?", "id": "Alat." },
  { "en": "Kita?", "id": "Kami (inklusif)." },
  { "en": "Kitab?", "id": "Buku." },
  { "en": "Kitang?", "id": "Ikan." },
  { "en": "Kitar?", "id": "Sekitar." },
  { "en": "Kitik?", "id": "Geli." },
  { "en": "Kitin?", "id": "Zat tanduk." },
  { "en": "Kiting?", "id": "Pincang." },
  { "en": "Kitis?", "id": "Sentil (arkais)." },
  { "en": "Kitri?", "id": "Pohon." },
  { "en": "Kiu?", "id": "Isyarat biliar." },
  { "en": "Kiuk?", "id": "Bunyi ayam." },
  { "en": "Kiwari?", "id": "Sekarang (Sunda)." },
  { "en": "Klab?", "id": "Klub." },
  { "en": "Klabat?", "id": "Rempah." },
  { "en": "Klak?", "id": "Bunyi." },
  { "en": "Klakah?", "id": "Daun pisang kering." },
  { "en": "Klaker?", "id": "Bantalan roda." },
  { "en": "Klakklik?", "id": "Bunyi." },
  { "en": "Klakson?", "id": "Bel mobil." },
  { "en": "Klam?", "id": "Penjepit." },
  { "en": "Klamidospora?", "id": "Spora." },
  { "en": "Klan?", "id": "Suku." },
  { "en": "Klandestin?", "id": "Rahasia." },
  { "en": "Klang?", "id": "Bunyi." },
  { "en": "Klante?", "id": "Klien (arkais)." },
  { "en": "Klaras?", "id": "Daun pisang kering." },
  { "en": "Klaret?", "id": "Anggur merah." },
  { "en": "Klarifikasi?", "id": "Penjelasan." },
  { "en": "Klarinet?", "id": "Alat musik tiup." },
  { "en": "Klasemen?", "id": "Peringkat." },
  { "en": "Klasik?", "id": "Kuno." },
  { "en": "Klasifikasi?", "id": "Penggolongan." },
  { "en": "Klausa?", "id": "Bagian kalimat." },
  { "en": "Klausul?", "id": "Pasal." },
  { "en": "Klaustrofobia?", "id": "Takut ruang sempit." },
  { "en": "Klaver?", "id": "Semanggi (Belanda)." },
  { "en": "Klawing?", "id": "Batu akik." },
  { "en": "Klebet?", "id": "Kibar." },
  { "en": "Kleidotomi?", "id": "Operasi." },
  { "en": "Kleistogami?", "id": "Penyerbukan tertutup." },
  { "en": "Klelep?", "id": "Tenggelam (Jawa)." },
  { "en": "Klem?", "id": "Penjepit." },
  { "en": "Klemensi?", "id": "Pengampunan." },
  { "en": "Klen?", "id": "Klan." },
  { "en": "Klengkeng?", "id": "Lengkeng." },
  { "en": "Klenik?", "id": "Mistik." },
  { "en": "Klentang?", "id": "Bunyi." },
  { "en": "Klenteng?", "id": "Tempat ibadah Tionghoa." },
  { "en": "Klep?", "id": "Katup." },
  { "en": "Kleptofobia?", "id": "Takut mencuri." },
  { "en": "Kleptoman?", "id": "Pencuri kompulsif." },
  { "en": "Kleptomania?", "id": "Penyakit suka mencuri." },
  { "en": "Klerikal?", "id": "Kependetaan." },
  { "en": "Klerikus?", "id": "Rohaniwan." },
  { "en": "Klerus?", "id": "Kaum rohaniwan." },
  { "en": "Klien?", "id": "Pelanggan." },
  { "en": "Klier?", "id": "Kelenjar (Belanda)." },
  { "en": "Klik?", "id": "Bunyi." },
  { "en": "Klimaks?", "id": "Puncak." },
  { "en": "Klimakterium?", "id": "Masa menopause." },
  { "en": "Klimat?", "id": "Iklim." },
  { "en": "Klimatolog?", "id": "Ahli iklim." },
  { "en": "Klimatologi?", "id": "Ilmu iklim." },
  { "en": "Klinik?", "id": "Balai pengobatan." },
  { "en": "Klinis?", "id": "Berdasarkan amatan." },
  { "en": "Klinisi?", "id": "Praktisi klinis." },
  { "en": "Klinometer?", "id": "Pengukur kemiringan." },
  { "en": "Klip?", "id": "Penjepit." },
  { "en": "Kliping?", "id": "Guntingan berita." },
  { "en": "Klir?", "id": "Jernih (Belanda)." },
  { "en": "Kliring?", "id": "Pertukaran cek." },
  { "en": "Klise?", "id": "Cetakan foto." },
  { "en": "Klistron?", "id": "Tabung elektron." },
  { "en": "Klitika?", "id": "Kata sandang." },
  { "en": "Kliyengan?", "id": "Pusing (Jawa)." },
  { "en": "Kloaka?", "id": "Lubang pembuangan." },
  { "en": "Klobot?", "id": "Kulit jagung." },
  { "en": "Klona?", "id": "Keturunan aseksual." },
  { "en": "Kloning?", "id": "Penggandaan genetik." },
  { "en": "Klonis?", "id": "Bergetar." },
  { "en": "Klonus?", "id": "Getaran otot." },
  { "en": "Klop?", "id": "Cocok." },
  { "en": "Klor?", "id": "Unsur kimia." },
  { "en": "Klorida?", "id": "Senyawa klor." },
  { "en": "Klorin?", "id": "Klor." },
  { "en": "Klorinasi?", "id": "Proses penambahan klor." },
  { "en": "Klorit?", "id": "Mineral." },
  { "en": "Klorobenzena?", "id": "Senyawa kimia." },
  { "en": "Klorofil?", "id": "Zat hijau daun." },
  { "en": "Kloroform?", "id": "Obat bius." },
  { "en": "Klorokuin?", "id": "Obat malaria." },
  { "en": "Kloroplas?", "id": "Bagian sel tumbuhan." },
  { "en": "Kloroprena?", "id": "Karet sintetis." },
  { "en": "Klos?", "id": "Gulungan benang." },
  { "en": "Kloset?", "id": "WC." },
  { "en": "Klub?", "id": "Perkumpulan." },
  { "en": "Kluntang-klantung?", "id": "Tidak menentu." },
  { "en": "Knop?", "id": "Tombol." },
  { "en": "Koagregasi?", "id": "Penggumpalan bersama." },
  { "en": "Koagulan?", "id": "Zat penggumpal." },
  { "en": "Koagulasi?", "id": "Penggumpalan." },
  { "en": "Koak?", "id": "Suara katak." },
  { "en": "Koaksi?", "id": "Kerja sama paksa." },
  { "en": "Koaksial?", "id": "Seporos." },
  { "en": "Koala?", "id": "Hewan Australia." },
  { "en": "Koalisi?", "id": "Gabungan." },
  { "en": "Koana?", "id": "Lubang hidung belakang." },
  { "en": "Kobah?", "id": "Gendang besar." },
  { "en": "Kobak?", "id": "Lubang." },
  { "en": "Kobalamin?", "id": "Vitamin B12." },
  { "en": "Kobalt?", "id": "Unsur kimia." },
  { "en": "Kobar?", "id": "Nyala." },
  { "en": "Kober?", "id": "Sempat (Jawa)." },
  { "en": "Koboi?", "id": "Gembala sapi." },
  { "en": "Kobok?", "id": "Cuci tangan (dalam wadah)." },
  { "en": "Kobra?", "id": "Ular." },
  { "en": "Kocar-kacir?", "id": "Cerai-berai." },
  { "en": "Kocek?", "id": "Saku." },
  { "en": "Kocilembik?", "id": "Kecil." },
  { "en": "Kocok?", "id": "Goyang." },
  { "en": "Kocor?", "id": "Pancur." },
  { "en": "Koda?", "id": "Bagian penutup musik." },
  { "en": "Kode?", "id": "Sandi." },
  { "en": "Kodein?", "id": "Obat batuk." },
  { "en": "Kodeks?", "id": "Naskah kuno." },
  { "en": "Kodepos?", "id": "Kode pos." },
  { "en": "Kodi?", "id": "Satuan jumlah (20)." },
  { "en": "Kodifikasi?", "id": "Penyusunan hukum." },
  { "en": "Kodikologi?", "id": "Ilmu naskah." },
  { "en": "Kodok?", "id": "Katak." },
  { "en": "Kodominan?", "id": "Sama dominan." },
  { "en": "Kodrat?", "id": "Sifat asal." },
  { "en": "Kodrati?", "id": "Alami." },
  { "en": "Koedukasi?", "id": "Pendidikan bersama." },
  { "en": "Koefisien?", "id": "Angka pengali." },
  { "en": "Koeksistensi?", "id": "Hidup berdampingan." },
  { "en": "Koenzim?", "id": "Bagian enzim." },
  { "en": "Koersi?", "id": "Paksaan." },
  { "en": "Kofaktor?", "id": "Zat pembantu enzim." },
  { "en": "Kofein?", "id": "Kafeina." },
  { "en": "Kognat?", "id": "Kerabat ayah." },
  { "en": "Kognisi?", "id": "Pemahaman." },
  { "en": "Kognitif?", "id": "Berkaitan pemahaman." },
  { "en": "Koh?", "id": "Kakak laki-laki (Tionghoa)." },
  { "en": "Kohabitasi?", "id": "Hidup bersama." },
  { "en": "Koheren?", "id": "Runtut." },
  { "en": "Koherensi?", "id": "Keruntutan." },
  { "en": "Kohesi?", "id": "Keterpaduan." },
  { "en": "Kohesif?", "id": "Terpadu." },
  { "en": "Kohlear?", "id": "Rumah siput (telinga)." },
  { "en": "Kohong?", "id": "Bau busuk." },
  { "en": "Kohor?", "id": "Pasukan Romawi." },
  { "en": "Koil?", "id": "Kumparan." },
  { "en": "Koin?", "id": "Uang logam." },
  { "en": "Koinsiden?", "id": "Terjadi bersamaan." },
  { "en": "Koinsidensi?", "id": "Kejadian bersamaan." },
  { "en": "Koit?", "id": "Sanggama (arkais)." },
  { "en": "Koitus?", "id": "Persanggamaan." },
  { "en": "Kojal-kajul?", "id": "Tergantung-gantung." },
  { "en": "Kojang?", "id": "Pohon." },
  { "en": "Kojoh?", "id": "Penuh (arkais)." },
  { "en": "Koju?", "id": "Kacang." },
  { "en": "Kok?", "id": "Mengapa." },
  { "en": "Koka?", "id": "Tanaman." },
  { "en": "Kokah?", "id": "Pohon." },
  { "en": "Kokain?", "id": "Narkotika." },
  { "en": "Kokainisme?", "id": "Kecanduan kokain." },
  { "en": "Kokang?", "id": "Siap tembak." },
  { "en": "Kokarde?", "id": "Lencana topi." },
  { "en": "Kokas?", "id": "Batu bara." },
  { "en": "Koki?", "id": "Juru masak." },
  { "en": "Kokila?", "id": "Burung (Sanskerta)." },
  { "en": "Kokoh?", "id": "Kuat." },
  { "en": "Koko?", "id": "Kakak laki-laki (Tionghoa)." },
  { "en": "Kokok?", "id": "Suara ayam." },
  { "en": "Kokokol?", "id": "Bengkok (arkais)." },
  { "en": "Kokon?", "id": "Kepompong." },
  { "en": "Kokot?", "id": "Penjepit." },
  { "en": "Kokpit?", "id": "Ruang pilot." },
  { "en": "Koksa?", "id": "Tulang panggul." },
  { "en": "Koktail?", "id": "Minuman campuran." },
  { "en": "Kokurikuler?", "id": "Kegiatan sekolah." },
  { "en": "Kokus?", "id": "Bakteri bulat." },
  { "en": "Kol?", "id": "Kubis." },
  { "en": "Kolaborasi?", "id": "Kerja sama." },
  { "en": "Kolaborator?", "id": "Rekan kerja." },
  { "en": "Kolagen?", "id": "Protein." },
  { "en": "Kolak?", "id": "Makanan manis." },
  { "en": "Kolam?", "id": "Empang." },
  { "en": "Kolang-kaling?", "id": "Buah aren." },
  { "en": "Kolaps?", "id": "Runtuh." },
  { "en": "Kolar?", "id": "Kerah (arkais)." },
  { "en": "Kolateral?", "id": "Jaminan tambahan." },
  { "en": "Kolator?", "id": "Pembanding naskah." },
  { "en": "Kold?", "id": "Dingin (Belanda)." },
  { "en": "Kolega?", "id": "Rekan." },
  { "en": "Kolegial?", "id": "Bersama." },
  { "en": "Kolegialitas?", "id": "Sifat kolegial." },
  { "en": "Koleksi?", "id": "Kumpulan." },
  { "en": "Kolektif?", "id": "Bersama-sama." },
  { "en": "Kolektivis?", "id": "Penganut kolektivisme." },
  { "en": "Kolektivisme?", "id": "Paham kebersamaan." },
  { "en": "Kolektivitas?", "id": "Kebersamaan." },
  { "en": "Kolektor?", "id": "Pengumpul." },
  { "en": "Kolembeng?", "id": "Kue." },
  { "en": "Kolenkima?", "id": "Jaringan tumbuhan." },
  { "en": "Kolera?", "id": "Penyakit." },
  { "en": "Kolese?", "id": "Sekolah tinggi." },
  { "en": "Kolesistitis?", "id": "Radang kantong empedu." },
  { "en": "Kolesterol?", "id": "Lemak darah." },
  { "en": "Koli?", "id": "Satuan barang." },
  { "en": "Kolibri?", "id": "Burung." },
  { "en": "Koligasi?", "id": "Ikatan kimia." },
  { "en": "Kolik?", "id": "Sakit perut." },
  { "en": "Kolimasi?", "id": "Penyelarasan cahaya." },
  { "en": "Kolina?", "id": "Vitamin." },
  { "en": "Kolintang?", "id": "Alat musik." },
  { "en": "Kolitis?", "id": "Radang usus besar." },
  { "en": "Kolofon?", "id": "Catatan akhir buku." },
  { "en": "Koloid?", "id": "Campuran." },
  { "en": "Koloidal?", "id": "Bersifat koloid." },
  { "en": "Kolok?", "id": "Ujian lisan." },
  { "en": "Kolokasi?", "id": "Pendampingan kata." },
  { "en": "Kolokium?", "id": "Seminar." },
  { "en": "Kolom?", "id": "Lajur." },
  { "en": "Kolon?", "id": "Usus besar." },
  { "en": "Kolone?", "id": "Barisan." },
  { "en": "Kolonel?", "id": "Pangkat militer." },
  { "en": "Koloni?", "id": "Daerah jajahan." },
  { "en": "Kolonial?", "id": "Bersifat penjajahan." },
  { "en": "Kolonialis?", "id": "Penjajah." },
  { "en": "Kolonialisme?", "id": "Paham penjajahan." },
  { "en": "Kolonis?", "id": "Pemukim." },
  { "en": "Kolonisasi?", "id": "Pembukaan pemukiman." },
  { "en": "Kolonoskopi?", "id": "Pemeriksaan usus." },
  { "en": "Kolor?", "id": "Celana dalam." },
  { "en": "Kolosal?", "id": "Sangat besar." },
  { "en": "Kolostomi?", "id": "Pembuatan lubang usus." },
  { "en": "Kolostrum?", "id": "Susu awal." },
  { "en": "Kolportir?", "id": "Penyebar bacaan." },
  { "en": "Kolt?", "id": "Pistol." },
  { "en": "Kolum?", "id": "Kolom." },
  { "en": "Kolumnis?", "id": "Penulis kolom." },
  { "en": "Koluvium?", "id": "Endapan." },
  { "en": "Koma?", "id": "Tanda baca." },
  { "en": "Komandan?", "id": "Pemimpin." },
  { "en": "Komandemen?", "id": "Perintah." },
  { "en": "Komanditer?", "id": "Sekutu pasif." },
  { "en": "Komando?", "id": "Perintah." },
  { "en": "Komaran?", "id": "Lampion." },
  { "en": "Komat-kamit?", "id": "Gerak bibir." },
  { "en": "Kombinasi?", "id": "Gabungan." },
  { "en": "Kombo?", "id": "Kelompok musik." },
  { "en": "Kombor?", "id": "Besar (pakaian)." },
  { "en": "Kombusio?", "id": "Luka bakar." },
  { "en": "Komedi?", "id": "Lawak." },
  { "en": "Komedian?", "id": "Pelawak." },
  { "en": "Komendur?", "id": "Komandan." },
  { "en": "Komeng?", "id": "Kecil (arkais)." },
  { "en": "Komensal?", "id": "Simbiosis." },
  { "en": "Komensalisme?", "id": "Jenis simbiosis." },
  { "en": "Komentar?", "id": "Ulasan." },
  { "en": "Komentator?", "id": "Pengulas." },
  { "en": "Komersial?", "id": "Bersifat dagang." },
  { "en": "Komet?", "id": "Bintang berekor." },
  { "en": "Komfor?", "id": "Nyaman (arkais)." },
  { "en": "Komidi?", "id": "Pertunjukan." },
  { "en": "Komik?", "id": "Cerita bergambar." },
  { "en": "Komikal?", "id": "Lucu." },
  { "en": "Komikus?", "id": "Penggambar komik." },
  { "en": "Kominfotel?", "id": "Komunikasi informasi telepon." },
  { "en": "Komis?", "id": "Roti." },
  { "en": "Komisariat?", "id": "Kantor komisaris." },
  { "en": "Komisaris?", "id": "Pejabat." },
  { "en": "Komisi?", "id": "Badan." },
  { "en": "Komisioner?", "id": "Anggota komisi." },
  { "en": "Komit?", "id": "Berjanji." },
  { "en": "Komite?", "id": "Panitia." },
  { "en": "Komitmen?", "id": "Janji." },
  { "en": "Komkoma?", "id": "Kunyit." },
  { "en": "Komodor?", "id": "Pangkat laut." },
  { "en": "Komodot?", "id": "Kadali." },
  { "en": "Kompak?", "id": "Padu." },
  { "en": "Komparasi?", "id": "Perbandingan." },
  { "en": "Komparatif?", "id": "Membandingkan." },
  { "en": "Komparator?", "id": "Alat banding." },
  { "en": "Kompartemen?", "id": "Ruang." },
  { "en": "Kompas?", "id": "Penunjuk arah." },
  { "en": "Kompatibel?", "id": "Cocok." },
  { "en": "Kompatriot?", "id": "Rekan senegara." },
  { "en": "Kompendium?", "id": "Ringkasan." },
  { "en": "Kompeni?", "id": "Perusahaan Belanda." },
  { "en": "Kompensasi?", "id": "Ganti rugi." },
  { "en": "Kompeten?", "id": "Cakap." },
  { "en": "Kompetensi?", "id": "Kecakapan." },
  { "en": "Kompetisi?", "id": "Perlombaan." },
  { "en": "Kompetitif?", "id": "Bersaing." },
  { "en": "Kompetitor?", "id": "Pesaing." },
  { "en": "Kompi?", "id": "Satuan tentara." },
  { "en": "Kompilasi?", "id": "Kumpulan." },
  { "en": "Kompilator?", "id": "Penyusun." },
  { "en": "Komplain?", "id": "Keluhan." },
  { "en": "Kompleks?", "id": "Rumit." },
  { "en": "Kompleksitas?", "id": "Kerumitan." },
  { "en": "Komplemen?", "id": "Pelengkap." },
  { "en": "Komplementer?", "id": "Saling melengkapi." },
  { "en": "Komplet?", "id": "Lengkap." },
  { "en": "Komplikasi?", "id": "Keruwetan." },
  { "en": "Komplimen?", "id": "Pujian." },
  { "en": "Komplot?", "id": "Sekongkol." },
  { "en": "Komplotan?", "id": "Gerombolan." },
  { "en": "Kompon?", "id": "Bagian (musik)." },
  { "en": "Komponen?", "id": "Unsur." },
  { "en": "Kompong?", "id": "Kudung (arkais)." },
  { "en": "Komponis?", "id": "Pencipta lagu." },
  { "en": "Kompor?", "id": "Alat masak." },
  { "en": "Kompos?", "id": "Pupuk." },
  { "en": "Komposisi?", "id": "Susunan." },
  { "en": "Komposit?", "id": "Gabungan." },
  { "en": "Komprador?", "id": "Perantara asing." },
  { "en": "Komprang?", "id": "Celana longgar." },
  { "en": "Komprehensif?", "id": "Menyeluruh." },
  { "en": "Kompres?", "id": "Tuala basah." },
  { "en": "Kompresi?", "id": "Pemampatan." },
  { "en": "Kompresor?", "id": "Alat pemampat." },
  { "en": "Kompromi?", "id": "Kesepakatan." },
  { "en": "Komputer?", "id": "Mesin hitung." },
  { "en": "Komputerisasi?", "id": "Penggunaan komputer." },
  { "en": "Komsatra?", "id": "Komunikasi sastra." },
  { "en": "Komunal?", "id": "Bersama." },
  { "en": "Komunalisme?", "id": "Paham komunal." },
  { "en": "Komune?", "id": "Kelompok hidup bersama." },
  { "en": "Komuni?", "id": "Sakramen." },
  { "en": "Komunikasi?", "id": "Hubungan." },
  { "en": "Komunikatif?", "id": "Mudah dipahami." },
  { "en": "Komunikan?", "id": "Penerima pesan." },
  { "en": "Komunikator?", "id": "Pemberi pesan." },
  { "en": "Komunike?", "id": "Pengumuman resmi." },
  { "en": "Komunir?", "id": "Pengomunike (arkais)." },
  { "en": "Komunis?", "id": "Penganut komunisme." },
  { "en": "Komunisme?", "id": "Paham politik." },
  { "en": "Komunitas?", "id": "Masyarakat." },
  { "en": "Komutasi?", "id": "Perubahan." },
  { "en": "Komutator?", "id": "Bagian motor listrik." },
  { "en": "Konan?", "id": "Kesulitan (Jepang)." },
  { "en": "Konco?", "id": "Kawan." },
  { "en": "Kondang?", "id": "Terkenal." },
  { "en": "Kondangan?", "id": "Menghadiri pesta." },
  { "en": "Konde?", "id": "Sanggul." },
  { "en": "Kondektur?", "id": "Penagih karcis." },
  { "en": "Kondensasi?", "id": "Pengembunan." },
  { "en": "Kondensator?", "id": "Kapasitor." },
  { "en": "Kondilus?", "id": "Tonjolan tulang." },
  { "en": "Kondisi?", "id": "Keadaan." },
  { "en": "Kondisional?", "id": "Bersyarat." },
  { "en": "Kondom?", "id": "Alat kontrasepsi." },
  { "en": "Kondominium?", "id": "Apartemen." },
  { "en": "Kondor?", "id": "Burung pemakan bangkai." },
  { "en": "Konduite?", "id": "Perilaku." },
  { "en": "Konduksi?", "id": "Hantaran panas." },
  { "en": "Konduktans?", "id": "Daya hantar listrik." },
  { "en": "Konduktif?", "id": "Menghantarkan." },
  { "en": "Konduktivitas?", "id": "Daya hantar." },
  { "en": "Konduktor?", "id": "Penghantar." },
  { "en": "Kondusif?", "id": "Mendukung." },
  { "en": "Koneksi?", "id": "Hubungan." },
  { "en": "Koneksitas?", "id": "Keterhubungan." },
  { "en": "Konektor?", "id": "Penghubung." },
  { "en": "Konfederasi?", "id": "Gabungan negara." },
  { "en": "Konfeksi?", "id": "Pakaian jadi." },
  { "en": "Konferensi?", "id": "Pertemuan." },
  { "en": "Konfesi?", "id": "Pengakuan dosa." },
  { "en": "Konfigurasi?", "id": "Susunan." },
  { "en": "Konfiks?", "id": "Imbuhan gabung." },
  { "en": "Konfirmasi?", "id": "Penegasan." },
  { "en": "Konfiskasi?", "id": "Penyitaan." },
  { "en": "Konflik?", "id": "Pertentangan." },
  { "en": "Konform?", "id": "Sesuai." },
  { "en": "Konformasi?", "id": "Bentuk." },
  { "en": "Konformis?", "id": "Mudah menyesuaikan." },
  { "en": "Konformitas?", "id": "Kesesuaian." },
  { "en": "Konfrontasi?", "id": "Pertentangan." },
  { "en": "Kongenital?", "id": "Bawaan lahir." },
  { "en": "Kongesti?", "id": "Kemacetan." },
  { "en": "Kongkalikong?", "id": "Persekongkolan." },
  { "en": "Kongko?", "id": "Berkumpul." },
  { "en": "Kongkoan?", "id": "Perkumpulan rahasia." },
  { "en": "Konglomerasi?", "id": "Penggabungan usaha." },
  { "en": "Konglomerat?", "id": "Pengusaha besar." },
  { "en": "Kongo?", "id": "Negara." },
  { "en": "Kongregasi?", "id": "Perkumpulan religius." },
  { "en": "Kongres?", "id": "Pertemuan besar." },
  { "en": "Kongruen?", "id": "Sama sebangun." },
  { "en": "Kongruensi?", "id": "Kesesuaian." },
  { "en": "Konifer?", "id": "Tumbuhan runjung." },
  { "en": "Konis?", "id": "Berbentuk kerucut." },
  { "en": "Konjugasi?", "id": "Tasrif." },
  { "en": "Konjungsi?", "id": "Kata hubung." },
  { "en": "Konjungter?", "id": "Kata penghubung." },
  { "en": "Konjungtiva?", "id": "Selaput mata." },
  { "en": "Konjungtivitis?", "id": "Radang mata." },
  { "en": "Konjungtur?", "id": "Keadaan ekonomi." },
  { "en": "Konkaf?", "id": "Cekung." },
  { "en": "Konklaf?", "id": "Rapat kardinal." },
  { "en": "Konklusi?", "id": "Kesimpulan." },
  { "en": "Konklusif?", "id": "Meyakinkan." },
  { "en": "Konkologi?", "id": "Ilmu kerang." },
  { "en": "Konkordansi?", "id": "Daftar kata." },
  { "en": "Konkordat?", "id": "Perjanjian." },
  { "en": "Konkret?", "id": "Nyata." },
  { "en": "Konkresi?", "id": "Pengerasan." },
  { "en": "Konkuren?", "id": "Sejalan." },
  { "en": "Konkurensi?", "id": "Persaingan." },
  { "en": "Konsekutif?", "id": "Berturut-turut." },
  { "en": "Konsekrasi?", "id": "Pengudusan." },
  { "en": "Konsekuen?", "id": "Taat asas." },
  { "en": "Konsekuensi?", "id": "Akibat." },
  { "en": "Konseli?", "id": "Penerima konseling." },
  { "en": "Konseling?", "id": "Bimbingan." },
  { "en": "Konselor?", "id": "Pembimbing." },
  { "en": "Konsensus?", "id": "Mufakat." },
  { "en": "Konsentrasi?", "id": "Pemusatan." },
  { "en": "Konsentrat?", "id": "Sari." },
  { "en": "Konsentrik?", "id": "Sepusat." },
  { "en": "Konsep?", "id": "Rancangan." },
  { "en": "Konsepsi?", "id": "Pemahaman." },
  { "en": "Konseptor?", "id": "Perancang." },
  { "en": "Konseptual?", "id": "Berkaitan konsep." },
  { "en": "Konser?", "id": "Pertunjukan musik." },
  { "en": "Konservasi?", "id": "Perlindungan." },
  { "en": "Konservasionis?", "id": "Pelindung alam." },
  { "en": "Konservatif?", "id": "Kolot." },
  { "en": "Konservatisme?", "id": "Paham kolot." },
  { "en": "Konservator?", "id": "Pengawet." },
  { "en": "Konservatori?", "id": "Sekolah musik." },
  { "en": "Konsesi?", "id": "Kelonggaran." },
  { "en": "Konsesif?", "id": "Menyatakan kelonggaran." },
  { "en": "Konsiderans?", "id": "Pertimbangan." },
  { "en": "Konsiderasi?", "id": "Pertimbangan." },
  { "en": "Konsili?", "id": "Rapat uskup." },
  { "en": "Konsiliasi?", "id": "Perdamaian." },
  { "en": "Konsinyasi?", "id": "Titip jual." },
  { "en": "Konsinyering?", "id": "Karantina kerja." },
  { "en": "Konsisten?", "id": "Taat asas." },
  { "en": "Konsistensi?", "id": "Ketaatan asas." },
  { "en": "Konsistori?", "id": "Dewan gereja." },
  { "en": "Konsol?", "id": "Meja." },
  { "en": "Konsolasi?", "id": "Penghiburan." },
  { "en": "Konsolidasi?", "id": "Penguatan." },
  { "en": "Konsonan?", "id": "Huruf mati." },
  { "en": "Konsonansi?", "id": "Kesesuaian bunyi." },
  { "en": "Konsorsium?", "id": "Gabungan usaha." },
  { "en": "Konspirasi?", "id": "Persekongkolan." },
  { "en": "Konspirator?", "id": "Penyekongkol." },
  { "en": "Konstan?", "id": "Tetap." },
  { "en": "Konstanta?", "id": "Nilai tetap." },
  { "en": "Konstantan?", "id": "Aloi." },
  { "en": "Konstatasi?", "id": "Penetapan." },
  { "en": "Konstatir?", "id": "Tetapkan." },
  { "en": "Konstelasi?", "id": "Rasi bintang." },
  { "en": "Konstipasi?", "id": "Sembelit." },
  { "en": "Konstituante?", "id": "Dewan penyusun UUD." },
  { "en": "Konstituen?", "id": "Pemilih." },
  { "en": "Konstitusi?", "id": "Undang-undang dasar." },
  { "en": "Konstitusional?", "id": "Sesuai UUD." },
  { "en": "Konstriksi?", "id": "Penyempitan." },
  { "en": "Konstriktor?", "id": "Otot penyempit." },
  { "en": "Konstruksi?", "id": "Bangunan." },
  { "en": "Konstruktif?", "id": "Membangun." },
  { "en": "Konstruktivisme?", "id": "Aliran seni." },
  { "en": "Konsul?", "id": "Pejabat luar negeri." },
  { "en": "Konsulat?", "id": "Kantor konsul." },
  { "en": "Konsulen?", "id": "Penasihat." },
  { "en": "Konsuler?", "id": "Berkaitan konsul." },
  { "en": "Konsultan?", "id": "Penasihat." },
  { "en": "Konsultasi?", "id": "Permintaan nasihat." },
  { "en": "Konsumen?", "id": "Pengguna." },
  { "en": "Konsumer?", "id": "Konsumen." },
  { "en": "Konsumerisme?", "id": "Paham konsumtif." },
  { "en": "Konsumtif?", "id": "Boros." },
  { "en": "Konsumsi?", "id": "Penggunaan." },
  { "en": "Kontak?", "id": "Hubungan." },
  { "en": "Kontaminasi?", "id": "Pencemaran." },
  { "en": "Kontan?", "id": "Tunai." },
  { "en": "Konte?", "id": "Bangsawan Eropa." },
  { "en": "Konteks?", "id": "Lingkungan." },
  { "en": "Kontekstual?", "id": "Sesuai konteks." },
  { "en": "Kontemplasi?", "id": "Perenungan." },
  { "en": "Kontemporer?", "id": "Masa kini." },
  { "en": "Konten?", "id": "Isi." },
  { "en": "Kontes?", "id": "Perlombaan." },
  { "en": "Kontestan?", "id": "Peserta." },
  { "en": "Kontinental?", "id": "Benua." },
  { "en": "Kontingen?", "id": "Perwakilan." },
  { "en": "Kontinu?", "id": "Berkelanjutan." },
  { "en": "Kontinuitas?", "id": "Keberlanjutan." },
  { "en": "Kontinum?", "id": "Rangkaian." },
  { "en": "Kontoid?", "id": "Bunyi konsonan." },
  { "en": "Kontra?", "id": "Lawan." },
  { "en": "Kontrabande?", "id": "Barang selundupan." },
  { "en": "Kontrabas?", "id": "Alat musik." },
  { "en": "Kontradiksi?", "id": "Pertentangan." },
  { "en": "Kontradiktif?", "id": "Bertentangan." },
  { "en": "Kontraindikasi?", "id": "Larangan." },
  { "en": "Kontrak?", "id": "Perjanjian." },
  { "en": "Kontraksi?", "id": "Pengerutan." },
  { "en": "Kontraktor?", "id": "Pemborong." },
  { "en": "Kontraktual?", "id": "Sesuai kontrak." },
  { "en": "Kontralto?", "id": "Suara wanita rendah." },
  { "en": "Kontras?", "id": "Perbedaan nyata." },
  { "en": "Kontrasepsi?", "id": "Pencegahan kehamilan." },
  { "en": "Kontraseptif?", "id": "Alat kontrasepsi." },
  { "en": "Kontribusi?", "id": "Sumbangan." },
  { "en": "Kontributor?", "id": "Penyumbang." },
  { "en": "Kontrol?", "id": "Pengawasan." },
  { "en": "Kontroversial?", "id": "Menimbulkan perdebatan." },
  { "en": "Kontroversi?", "id": "Perdebatan." },
  { "en": "Konveks?", "id": "Cembung." },
  { "en": "Konveksi?", "id": "Hantaran panas cairan." },
  { "en": "Konvektif?", "id": "Bersifat konveksi." },
  { "en": "Konvensi?", "id": "Kesepakatan." },
  { "en": "Konvensional?", "id": "Tradisional." },
  { "en": "Konvergen?", "id": "Memusat." },
  { "en": "Konvergensi?", "id": "Pemaduan." },
  { "en": "Konversasi?", "id": "Percakapan." },
  { "en": "Konversi?", "id": "Perubahan." },
  { "en": "Konvoi?", "id": "Iring-iringan." },
  { "en": "Konvolusi?", "id": "Lipatan." },
  { "en": "Konyak?", "id": "Minuman keras." },
  { "en": "Konyan?", "id": "Hantu (Jepang)." },
  { "en": "Konyol?", "id": "Lucu." },
  { "en": "Konyeh?", "id": "Lumat (arkais)." },
  { "en": "Kooperasi?", "id": "Kerja sama." },
  { "en": "Kooperatif?", "id": "Bersedia kerja sama." },
  { "en": "Kooperator?", "id": "Rekan kerja." },
  { "en": "Kooptasi?", "id": "Pemilihan anggota baru." },
  { "en": "Koordinasi?", "id": "Pengaturan." },
  { "en": "Koordinat?", "id": "Letak titik." },
  { "en": "Koordinator?", "id": "Pengatur." },
  { "en": "Kop?", "id": "Gelas bekam." },
  { "en": "Kopah?", "id": "Gumpal." },
  { "en": "Kopaiba?", "id": "Minyak." },
  { "en": "Kopal?", "id": "Damar." },
  { "en": "Kopan?", "id": "Gemetar (arkais)." },
  { "en": "Kopek?", "id": "Kupas." },
  { "en": "Kopel?", "id": "Pasangan." },
  { "en": "Koper?", "id": "Tas besar." },
  { "en": "Koperasi?", "id": "Badan usaha bersama." },
  { "en": "Kopi?", "id": "Minuman." },
  { "en": "Kopiah?", "id": "Peci." },
  { "en": "Kopilot?", "id": "Asisten pilot." },
  { "en": "Kopling?", "id": "Bagian mesin." },
  { "en": "Koplo?", "id": "Musik." },
  { "en": "Kopok?", "id": "Tuli." },
  { "en": "Kopra?", "id": "Daging kelapa kering." },
  { "en": "Koprak?", "id": "Suara." },
  { "en": "Kopral?", "id": "Pangkat militer." },
  { "en": "Koprok?", "id": "Permainan dadu." },
  { "en": "Koprofagia?", "id": "Makan tinja." },
  { "en": "Koprofili?", "id": "Suka tinja." },
  { "en": "Koptik?", "id": "Gereja Mesir." },
  { "en": "Kopula?", "id": "Kata kerja penghubung." },
  { "en": "Kopulasi?", "id": "Persanggamaan." },
  { "en": "Kopyok?", "id": "Kocok." },
  { "en": "Kopyor?", "id": "Kelapa." },
  { "en": "Kor?", "id": "Paduan suara." },
  { "en": "Koral?", "id": "Karang." },
  { "en": "Koralit?", "id": "Kerangka koral." },
  { "en": "Koram?", "id": "Orang banyak (Arab)." },
  { "en": "Koran?", "id": "Surat kabar." },
  { "en": "Korban?", "id": "Mangsa." },
  { "en": "Korbana?", "id": "Persembahan (Yahudi)." },
  { "en": "Korden?", "id": "Tirai." },
  { "en": "Kordial?", "id": "Ramah." },
  { "en": "Kordit?", "id": "Bahan peledak." },
  { "en": "Kordon?", "id": "Barisan penjaga." },
  { "en": "Korduroi?", "id": "Kain." },
  { "en": "Korek?", "id": "Gorek." },
  { "en": "Koreksi?", "id": "Perbaikan." },
  { "en": "Korektif?", "id": "Memperbaiki." },
  { "en": "Korektor?", "id": "Pemeriksa." },
  { "en": "Korelat?", "id": "Hubungan." },
  { "en": "Korelasi?", "id": "Hubungan timbal balik." },
  { "en": "Korelatif?", "id": "Saling berhubungan." },
  { "en": "Koreng?", "id": "Luka kering." },
  { "en": "Koreograf?", "id": "Pencipta tari." },
  { "en": "Koreografer?", "id": "Koreograf." },
  { "en": "Koreografi?", "id": "Seni tari." },
  { "en": "Kores?", "id": "Alat tulis." },
  { "en": "Koresponden?", "id": "Wartawan." },
  { "en": "Korespondensi?", "id": "Surat-menyurat." },
  { "en": "Koret?", "id": "Sedikit (Jawa)." },
  { "en": "Koridor?", "id": "Lorong." },
  { "en": "Korion?", "id": "Selaput janin." },
  { "en": "Kornea?", "id": "Bagian mata." },
  { "en": "Kornel?", "id": "Jagung (Belanda)." },
  { "en": "Korner?", "id": "Sudut (sepak bola)." },
  { "en": "Kornet?", "id": "Daging awetan." },
  { "en": "Koroid?", "id": "Lapisan mata." },
  { "en": "Korok?", "id": "Gali." },
  { "en": "Korona?", "id": "Mahkota." },
  { "en": "Koronal?", "id": "Bunyi." },
  { "en": "Koroner?", "id": "Pembuluh jantung." },
  { "en": "Korong?", "id": "Kampung (Minang)." },
  { "en": "Korosi?", "id": "Karat." },
  { "en": "Korosif?", "id": "Menyebabkan karat." },
  { "en": "Korporasi?", "id": "Perusahaan besar." },
  { "en": "Korporat?", "id": "Berkaitan korporasi." },
  { "en": "Korps?", "id": "Kesatuan." },
  { "en": "Korpulen?", "id": "Gemuk." },
  { "en": "Korpuskula?", "id": "Butir darah." },
  { "en": "Korsase?", "id": "Hiasan bunga." },
  { "en": "Korsel?", "id": "Korsleting." },
  { "en": "Korset?", "id": "Pakaian dalam." },
  { "en": "Korsleting?", "id": "Hubungan pendek listrik." },
  { "en": "Korteks?", "id": "Lapisan luar." },
  { "en": "Kortikulus?", "id": "Jamur." },
  { "en": "Kortikosteroid?", "id": "Hormon." },
  { "en": "Kortikosteron?", "id": "Hormon." },
  { "en": "Kortison?", "id": "Hormon." },
  { "en": "Korugator?", "id": "Otot." },
  { "en": "Korundum?", "id": "Mineral." },
  { "en": "Korup?", "id": "Busuk." },
  { "en": "Korupsi?", "id": "Penyalahgunaan wewenang." },
  { "en": "Koruptor?", "id": "Pelaku korupsi." },
  { "en": "Korve?", "id": "Kerja paksa." },
  { "en": "Korvet?", "id": "Kapal perang." },
  { "en": "Kosak-kasik?", "id": "Gelisah." },
  { "en": "Kosakata?", "id": "Perbendaharaan kata." },
  { "en": "Kosar?", "id": "Papan (arkais)." },
  { "en": "Kosek?", "id": "Alat pembersih." },
  { "en": "Kosen?", "id": "Rangka pintu." },
  { "en": "Kosinus?", "id": "Fungsi trigonometri." },
  { "en": "Kosmetik?", "id": "Alat rias." },
  { "en": "Kosmetolog?", "id": "Ahli kosmetik." },
  { "en": "Kosmetologi?", "id": "Ilmu kosmetik." },
  { "en": "Kosmis?", "id": "Antariksa." },
  { "en": "Kosmogoni?", "id": "Ilmu asal alam semesta." },
  { "en": "Kosmografi?", "id": "Ilmu peta langit." },
  { "en": "Kosmolinguistik?", "id": "Ilmu bahasa antariksa." },
  { "en": "Kosmolog?", "id": "Ahli kosmologi." },
  { "en": "Kosmologi?", "id": "Ilmu alam semesta." },
  { "en": "Kosmonaut?", "id": "Antariksawan." },
  { "en": "Kosmopolit?", "id": "Warga dunia." },
  { "en": "Kosmopolitan?", "id": "Bersifat kosmopolit." },
  { "en": "Kosmopolitanisme?", "id": "Paham kosmopolit." },
  { "en": "Kosmos?", "id": "Alam semesta." },
  { "en": "Kosponsor?", "id": "Sponsor bersama." },
  { "en": "Kostum?", "id": "Pakaian." },
  { "en": "Kota?", "id": "Bandar." },
  { "en": "Kotah?", "id": "Benteng (arkais)." },
  { "en": "Kotak?", "id": "Peti." },
  { "en": "Kotak-katik?", "id": "Gerak." },
  { "en": "Kotang?", "id": "Kutang." },
  { "en": "Kotangen?", "id": "Fungsi trigonometri." },
  { "en": "Kotaraja?", "id": "Ibu kota kerajaan." },
  { "en": "Kotek?", "id": "Suara ayam betina." },
  { "en": "Koteka?", "id": "Pakaian adat Papua." },
  { "en": "Koteng?", "id": "Sendiri (arkais)." },
  { "en": "Kotes?", "id": "Remah." },
  { "en": "Kotiledon?", "id": "Daun lembaga." },
  { "en": "Kotok?", "id": "Ayam (Jawa)." },
  { "en": "Kotor?", "id": "Najis." },
  { "en": "Kotoran?", "id": "Sampah." },
  { "en": "Kover?", "id": "Sampul." },
  { "en": "Kowad?", "id": "Korps Wanita Angkatan Darat." },
  { "en": "Kowal?", "id": "Korps Wanita Angkatan Laut." },
  { "en": "Kowan?", "id": "Uang (Tionghoa)." },
  { "en": "Koyam?", "id": "Ukuran berat." },
  { "en": "Koyan?", "id": "Koyam." },
  { "en": "Koyo?", "id": "Plester obat." },
  { "en": "Koyok?", "id": "Koyo." },
  { "en": "Kram?", "id": "Kejang otot." },
  { "en": "Krambil?", "id": "Kelapa (Jawa)." },
  { "en": "Kranial?", "id": "Tengkorak." },
  { "en": "Kraniologi?", "id": "Ilmu tengkorak." },
  { "en": "Kraniometri?", "id": "Pengukuran tengkorak." },
  { "en": "Kraniotomi?", "id": "Operasi tengkorak." },
  { "en": "Krank?", "id": "Gila (Belanda)." },
  { "en": "Krasan?", "id": "Betah." },
  { "en": "Krayon?", "id": "Pensil warna." },
  { "en": "Kreasi?", "id": "Ciptaan." },
  { "en": "Kreatif?", "id": "Mencipta." },
  { "en": "Kreativitas?", "id": "Daya cipta." },
  { "en": "Kreator?", "id": "Pencipta." },
  { "en": "Kredensial?", "id": "Surat kepercayaan." },
  { "en": "Kredibel?", "id": "Terpercaya." },
  { "en": "Kredibilitas?", "id": "Kepercayaan." },
  { "en": "Kredit?", "id": "Pinjaman." },
  { "en": "Kreditor?", "id": "Pemberi pinjaman." },
  { "en": "Kredo?", "id": "Pengakuan iman." },
  { "en": "Kreket?", "id": "Bunyi." },
  { "en": "Krem?", "id": "Krim." },
  { "en": "Kremasi?", "id": "Pembakaran mayat." },
  { "en": "Krematori?", "id": "Tempat kremasi." },
  { "en": "Krematorium?", "id": "Krematori." },
  { "en": "Kreolin?", "id": "Disinfektan." },
  { "en": "Kreolisasi?", "id": "Proses bahasa." },
  { "en": "Kreosol?", "id": "Senyawa." },
  { "en": "Kretin?", "id": "Dungu." },
  { "en": "Kribo?", "id": "Rambut keriting." },
  { "en": "Kricak?", "id": "Kerikil." },
  { "en": "Kriminal?", "id": "Kejahatan." },
  { "en": "Kriminalis?", "id": "Ahli kriminal." },
  { "en": "Kriminalisasi?", "id": "Proses pidana." },
  { "en": "Kriminalitas?", "id": "Tindak kejahatan." },
  { "en": "Kriminolog?", "id": "Ahli kriminologi." },
  { "en": "Kriminologi?", "id": "Ilmu kejahatan." },
  { "en": "Krio?", "id": "Beku (awalan)." },
  { "en": "Krioterapi?", "id": "Terapi dingin." },
  { "en": "Kripta?", "id": "Ruang bawah tanah." },
  { "en": "Kriptogam?", "id": "Tumbuhan tak berbiji." },
  { "en": "Kriptografi?", "id": "Ilmu sandi." },
  { "en": "Kriptogram?", "id": "Tulisan sandi." },
  { "en": "Kripton?", "id": "Unsur gas." },
  { "en": "Krisan?", "id": "Bunga." },
  { "en": "Krisis?", "id": "Kemelut." },
  { "en": "Krisma?", "id": "Sakramen." },
  { "en": "Krisoberil?", "id": "Mineral." },
  { "en": "Krisofil?", "id": "Suka emas." },
  { "en": "Krisolit?", "id": "Batu permata." },
  { "en": "Krisopras?", "id": "Batu permata." },
  { "en": "Kristen?", "id": "Agama." },
  { "en": "Kristal?", "id": "Hablur." },
  { "en": "Kristalisasi?", "id": "Penghabluran." },
  { "en": "Kristalografi?", "id": "Ilmu kristal." },
  { "en": "Kristaloid?", "id": "Mirip kristal." },
  { "en": "Kriteria?", "id": "Ukuran." },
  { "en": "Kriterium?", "id": "Kriteria." },
  { "en": "Kritik?", "id": "Celaan." },
  { "en": "Kritikus?", "id": "Pengkritik." },
  { "en": "Kritis?", "id": "Gawat." },
  { "en": "Kriya?", "id": "Kerajinan tangan." },
  { "en": "Krom?", "id": "Logam." },
  { "en": "Kromat?", "id": "Garam asam kromat." },
  { "en": "Kromatid?", "id": "Bagian kromosom." },
  { "en": "Kromatin?", "id": "Benang kromosom." },
  { "en": "Kromatofor?", "id": "Sel pigmen." },
  { "en": "Kromatografi?", "id": "Teknik pemisahan." },
  { "en": "Kromit?", "id": "Mineral." },
  { "en": "Kromium?", "id": "Unsur kimia." },
  { "en": "Kromofil?", "id": "Menyerap warna." },
  { "en": "Kromofob?", "id": "Tidak menyerap warna." },
  { "en": "Kromogen?", "id": "Penghasil warna." },
  { "en": "Kromong?", "id": "Gamelan." },
  { "en": "Kromosfer?", "id": "Lapisan matahari." },
  { "en": "Kromosom?", "id": "Pembawa gen." },
  { "en": "Kroni?", "id": "Kawan dekat." },
  { "en": "Kronik?", "id": "Catatan waktu." },
  { "en": "Kronis?", "id": "Menahun." },
  { "en": "Kronogram?", "id": "Penunjuk waktu." },
  { "en": "Kronolog?", "id": "Ahli kronologi." },
  { "en": "Kronologi?", "id": "Urutan waktu." },
  { "en": "Kronologis?", "id": "Menurut waktu." },
  { "en": "Kronometer?", "id": "Pengukur waktu." },
  { "en": "Kronoskop?", "id": "Alat ukur waktu singkat." },
  { "en": "Kros?", "id": "Lintas (Belanda)." },
  { "en": "Kroto?", "id": "Telur semut." },
  { "en": "Kru?", "id": "Awak." },
  { "en": "Kruistik?", "id": "Sulam silang." },
  { "en": "Krusial?", "id": "Penting." },
  { "en": "Krustasea?", "id": "Udang-udangan." },
  { "en": "Ku?", "id": "Aku (singkatan)." },
  { "en": "Kuaci?", "id": "Biji semangka kering." },
  { "en": "Kuadran?", "id": "Seperempat lingkaran." },
  { "en": "Kuadrat?", "id": "Pangkat dua." },
  { "en": "Kuadratur?", "id": "Penghitungan luas." },
  { "en": "Kuadriceps?", "id": "Otot paha." },
  { "en": "Kuadripartit?", "id": "Empat pihak." },
  { "en": "Kuadrilium?", "id": "Bilangan." },
  { "en": "Kuadrupleks?", "id": "Rangkap empat." },
  { "en": "Kuadruplet?", "id": "Kembar empat." },
  { "en": "Kuah?", "id": "Cairan masakan." },
  { "en": "Kuak?", "id": "Suara." },
  { "en": "Kual?", "id": "Ibu tiri." },
  { "en": "Kuala?", "id": "Muara sungai." },
  { "en": "Kualat?", "id": "Celaka." },
  { "en": "Kuali?", "id": "Wajan." },
  { "en": "Kualifikasi?", "id": "Persyaratan." },
  { "en": "Kualitas?", "id": "Mutu." },
  { "en": "Kualitatif?", "id": "Berdasarkan mutu." },
  { "en": "Kuantitas?", "id": "Jumlah." },
  { "en": "Kuantitatif?", "id": "Berdasarkan jumlah." },
  { "en": "Kuantor?", "id": "Penentu jumlah (logika)." },
  { "en": "Kuantum?", "id": "Satuan energi terkecil." },
  { "en": "Kuar?", "id": "Gali (arkais)." },
  { "en": "Kuari?", "id": "Tambang batu." },
  { "en": "Kuark?", "id": "Partikel." },
  { "en": "Kuarsa?", "id": "Mineral." },
  { "en": "Kuarsit?", "id": "Batuan." },
  { "en": "Kuart?", "id": "Satuan isi." },
  { "en": "Kuartal?", "id": "Triwulan." },
  { "en": "Kuartet?", "id": "Kelompok empat." },
  { "en": "Kuartir?", "id": "Markas." },
  { "en": "Kuarto?", "id": "Ukuran kertas." },
  { "en": "Kuas?", "id": "Alat cat." },
  { "en": "Kuasa?", "id": "Wewenang." },
  { "en": "Kuasar?", "id": "Benda langit." },
  { "en": "Kuasi?", "id": "Seolah-olah." },
  { "en": "Kuat?", "id": "Tangguh." },
  { "en": "Kuatir?", "id": "Khawatir." },
  { "en": "Kuau?", "id": "Burung." },
  { "en": "Kub?", "id": "Kubah (arkais)." },
  { "en": "Kubah?", "id": "Atap melengkung." },
  { "en": "Kubak?", "id": "Kupas." },
  { "en": "Kubang?", "id": "Genangan." },
  { "en": "Kubik?", "id": "Pangkat tiga." },
  { "en": "Kubil?", "id": "Penyakit kulit." },
  { "en": "Kubin?", "id": "Kelelawar pemakan buah." },
  { "en": "Kubis?", "id": "Kol." },
  { "en": "Kubisme?", "id": "Aliran seni." },
  { "en": "Kubit?", "id": "Cubitan." },
  { "en": "Kubra?", "id": "Besar (Arab)." },
  { "en": "Kubu?", "id": "Benteng." },
  { "en": "Kubul?", "id": "Pelepas (Arab)." },
  { "en": "Kubung?", "id": "Kungkang." },
  { "en": "Kubur?", "id": "Makam." },
  { "en": "Kubut?", "id": "Tutup (arkais)." },
  { "en": "Kucai?", "id": "Sayuran." },
  { "en": "Kucak?", "id": "Kocok (arkais)." },
  { "en": "Kucam?", "id": "Pudar." },
  { "en": "Kucandan?", "id": "Gurauan." },
  { "en": "Kucar-kacir?", "id": "Cerai-berai." },
  { "en": "Kucek?", "id": "Gosok." },
  { "en": "Kucil?", "id": "Singkir." },
  { "en": "Kucing?", "id": "Hewan peliharaan." },
  { "en": "Kucir?", "id": "Ikat rambut." },
  { "en": "Kucup?", "id": "Cium." },
  { "en": "Kucur?", "id": "Pancur." },
  { "en": "Kuda?", "id": "Hewan tunggangan." },
  { "en": "Kudai?", "id": "Keranjang." },
  { "en": "Kudang?", "id": "Puji (arkais)." },
  { "en": "Kudap?", "id": "Makan." },
  { "en": "Kudapan?", "id": "Makanan ringan." },
  { "en": "Kudeta?", "id": "Perebutan kekuasaan." },
  { "en": "Kudi?", "id": "Senjata." },
  { "en": "Kudis?", "id": "Penyakit kulit." },
  { "en": "Kudrat?", "id": "Kodrat." },
  { "en": "Kudrati?", "id": "Kodrati." },
  { "en": "Kudsi?", "id": "Suci (Arab)." },
  { "en": "Kuduk?", "id": "Tengkuk." },
  { "en": "Kudung?", "id": "Kerudung." },
  { "en": "Kudup?", "id": "Kuncup." },
  { "en": "Kudus?", "id": "Suci." },
  { "en": "Kue?", "id": "Penganan." },
  { "en": "Kuesioner?", "id": "Daftar pertanyaan." },
  { "en": "Kuetiau?", "id": "Mi pipih." },
  { "en": "Kufu?", "id": "Setara (Arab)." },
  { "en": "Kufur?", "id": "Ingkar." },
  { "en": "Kuini?", "id": "Buah mangga." },
  { "en": "Kuis?", "id": "Teka-teki." },
  { "en": "Kuit?", "id": "Sentuh (arkais)." },
  { "en": "Kuitansi?", "id": "Tanda terima." },
  { "en": "Kujarat?", "id": "Pedagang India." },
  { "en": "Kujung?", "id": "Tudung kepala." },
  { "en": "Kujur?", "id": "Tombak." },
  { "en": "Kujut?", "id": "Ikat (arkais)." },
  { "en": "Kukab?", "id": "Tutup (arkais)." },
  { "en": "Kukai?", "id": "Kue (arkais)." },
  { "en": "Kukang?", "id": "Hewan." },
  { "en": "Kuku?", "id": "Bagian jari." },
  { "en": "Kukuh?", "id": "Kuat." },
  { "en": "Kukuk?", "id": "Suara burung." },
  { "en": "Kukukbeluk?", "id": "Burung hantu." },
  { "en": "Kukul?", "id": "Bisul." },
  { "en": "Kukup?", "id": "Lumpur pantai." },
  { "en": "Kukur?", "id": "Alat parut." },
  { "en": "Kukuruyuk?", "id": "Suara ayam jantan." },
  { "en": "Kukus?", "id": "Masak uap." },
  { "en": "Kula?", "id": "Saya (Jawa halus)." },
  { "en": "Kulah?", "id": "Bak air." },
  { "en": "Kulai?", "id": "Terkulai." },
  { "en": "Kulak?", "id": "Beli untuk dijual." },
  { "en": "Kulakasar?", "id": "Baju zirah." }


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
            }, 4000);
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
