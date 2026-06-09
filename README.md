```html
<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Animal Adventure Quest - เกมผจญภัยเรียนรู้คำศัพท์สัตว์แสนสนุก</title>
    <!-- นำเข้า Tailwind CSS และ Google Fonts เพื่อความสวยงามและตอบสนองทุกหน้าจอ -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Kanit:wght@300;400;600;800&family=Fredoka+One&display=swap" rel="stylesheet">
    
    <!-- นำเข้าระบบเสียงสำรองชั้นนำระดับโลกเพื่อรับประกันเสียงออก 100% บนมือถือและแท็บเล็ตทุกเครื่อง -->
    <script src="https://code.responsivevoice.org/responsivevoice.js?key=8M37v0xX"></script>

    <style>
        body {
            font-family: 'Kanit', sans-serif;
            background-color: #0f172a;
            user-select: none;
            -webkit-user-select: none;
            overflow: hidden;
            margin: 0;
            padding: 0;
        }
        .fancy-font {
            font-family: 'Fredoka One', 'Kanit', sans-serif;
        }
        /* ตกแต่งปุ่มและพื้นหลังสไตล์เกมมือถือระดับพรีเมียม */
        .btn-gradient-green {
            background: linear-gradient(135deg, #4ade80, #22c55e);
            box-shadow: 0 4px 0 #15803d, 0 8px 15px rgba(34, 197, 94, 0.4);
            transition: all 0.1s ease;
        }
        .btn-gradient-green:active {
            transform: translateY(4px);
            box-shadow: 0 0px 0 #15803d, 0 4px 6px rgba(34, 197, 94, 0.4);
        }
        .btn-gradient-blue {
            background: linear-gradient(135deg, #60a5fa, #3b82f6);
            box-shadow: 0 4px 0 #1d4ed8, 0 8px 15px rgba(59, 130, 246, 0.4);
            transition: all 0.1s ease;
        }
        .btn-gradient-blue:active {
            transform: translateY(4px);
            box-shadow: 0 0px 0 #1d4ed8, 0 4px 6px rgba(59, 130, 246, 0.4);
        }
        .btn-gradient-yellow {
            background: linear-gradient(135deg, #fbbf24, #f59e0b);
            box-shadow: 0 4px 0 #b45309, 0 8px 15px rgba(245, 158, 11, 0.4);
            transition: all 0.1s ease;
        }
        .btn-gradient-yellow:active {
            transform: translateY(4px);
            box-shadow: 0 0px 0 #b45309, 0 4px 6px rgba(245, 158, 11, 0.4);
        }
        .btn-gradient-red {
            background: linear-gradient(135deg, #f87171, #ef4444);
            box-shadow: 0 4px 0 #b91c1c, 0 8px 15px rgba(239, 68, 68, 0.4);
            transition: all 0.1s ease;
        }
        .btn-gradient-red:active {
            transform: translateY(4px);
            box-shadow: 0 0px 0 #b91c1c, 0 4px 6px rgba(239, 68, 68, 0.4);
        }
        /* แอนิเมชันสำหรับฉากหลัง */
        @keyframes float {
            0%, 100% { transform: translateY(0px) rotate(0s); }
            50% { transform: translateY(-10px) rotate(2deg); }
        }
        .floating-element {
            animation: float 4s ease-in-out infinite;
        }
        /* ซ่อน Scrollbar ของบราวเซอร์ */
        ::-webkit-scrollbar {
            display: none;
        }
    </style>
</head>
<body class="flex items-center justify-center min-h-screen">

    <!-- หน้าต่างควบคุมเกมหลัก (Game Frame) เพื่อรองรับ Responsive และควบคุมอัตราส่วน 16:9 -->
    <div id="game-frame" class="relative w-full max-w-5xl aspect-[16/9] overflow-hidden bg-slate-900 rounded-2xl shadow-2xl border-4 border-slate-700">
        
        <!-- ==================== 1. หน้าจอหลัก (MAIN MENU) ==================== -->
        <div id="main-menu" class="absolute inset-0 z-30 flex flex-col justify-between p-6 bg-gradient-to-b from-[#1e3a1e] via-[#0f2e15] to-[#071a0b] text-white">
            <!-- ฉากหลังลอยๆ (ใบไม้ตกแต่ง) -->
            <div class="absolute inset-0 opacity-10 pointer-events-none">
                <div class="absolute top-10 left-10 text-6xl floating-element">🌿</div>
                <div class="absolute top-20 right-20 text-6xl floating-element" style="animation-delay: 1.5s;">🌳</div>
                <div class="absolute bottom-10 left-1/4 text-6xl floating-element" style="animation-delay: 2.5s;">🌺</div>
                <div class="absolute bottom-20 right-1/3 text-6xl floating-element" style="animation-delay: 0.5s;">🐒</div>
            </div>

            <!-- ส่วนบน: สถิติสูงสุดและการตั้งค่า -->
            <div class="flex justify-between items-center z-10">
                <div class="bg-black/40 px-4 py-2 rounded-full border border-green-500/30">
                    <span class="text-green-400 font-semibold">🏆 สถิติสูงสุด: </span>
                    <span id="best-score" class="fancy-font text-yellow-300">0</span>
                </div>
                <div class="flex gap-2">
                    <button id="btn-toggle-sound" class="w-12 h-12 rounded-full bg-slate-800 border-2 border-slate-600 flex items-center justify-center text-xl hover:bg-slate-700 active:scale-95 transition-all">🔊</button>
                    <button id="btn-shop" class="w-12 h-12 rounded-full bg-slate-800 border-2 border-slate-600 flex items-center justify-center text-xl hover:bg-slate-700 active:scale-95 transition-all">👕</button>
                </div>
            </div>

            <!-- ส่วนกลาง: โลโก้เกม -->
            <div class="text-center z-10 flex flex-col items-center">
                <div class="flex items-center gap-2 mb-2">
                    <span class="text-5xl">🦁</span>
                    <span class="text-5xl">🐘</span>
                    <span class="text-5xl">🦜</span>
                </div>
                <h1 class="fancy-font text-5xl md:text-6xl font-black text-transparent bg-clip-text bg-gradient-to-r from-yellow-400 via-green-400 to-emerald-300 tracking-wider drop-shadow-[0_5px_5px_rgba(0,0,0,0.8)]">
                    ANIMAL ADVENTURE
                </h1>
                <p class="fancy-font text-xl md:text-2xl text-yellow-300 tracking-widest font-semibold drop-shadow mt-1">QUEST</p>
                <p class="text-sm md:text-base text-green-300 mt-2 bg-emerald-950/80 px-4 py-1 rounded-full border border-emerald-500/20">ตะลุยป่าเวทมนตร์ ไขรหัสคำศัพท์เพื่อช่วยผองเพื่อนสัตว์ป่า!</p>
            </div>

            <!-- ส่วนล่าง: แผงควบคุมและเริ่มเกม -->
            <div class="grid grid-cols-3 gap-4 max-w-2xl mx-auto w-full z-10 mb-2">
                <button id="btn-how-to" class="btn-gradient-blue text-white py-3 px-4 rounded-xl font-bold text-lg border-b-4 tracking-wider flex items-center justify-center gap-2">
                    📖 วิธีเล่น
                </button>
                <button id="btn-play" class="btn-gradient-green text-white py-3 px-4 rounded-xl font-black text-xl border-b-4 tracking-wider flex items-center justify-center gap-2">
                    🎮 เริ่มเกม
                </button>
                <button id="btn-leaderboard" class="btn-gradient-yellow text-white py-3 px-4 rounded-xl font-bold text-lg border-b-4 tracking-wider flex items-center justify-center gap-2">
                    🏆 คะแนน
                </button>
            </div>
        </div>

        <!-- ==================== 2. หน้าจอวิธีเล่น (HOW TO PLAY) ==================== -->
        <div id="how-to-screen" class="absolute inset-0 z-40 hidden flex flex-col justify-between p-6 bg-slate-950/95 text-white">
            <h2 class="text-3xl font-black text-yellow-400 text-center flex items-center justify-center gap-2 mb-2">📖 วิธีการผจญภัยแสนสนุก</h2>
            
            <div class="grid grid-cols-1 md:grid-cols-3 gap-4 flex-grow overflow-y-auto pr-2 text-sm leading-relaxed max-h-[70%]">
                <div class="bg-slate-900 p-4 rounded-xl border border-blue-500/20">
                    <h3 class="font-bold text-blue-400 text-base mb-2">🎮 การควบคุมตัวละคร</h3>
                    <ul class="space-y-2 text-slate-300">
                        <li>• <b class="text-white">คอมพิวเตอร์:</b> กดปุ่ม <span class="bg-slate-800 px-2 py-0.5 rounded border border-slate-600">A</span>, <span class="bg-slate-800 px-2 py-0.5 rounded border border-slate-600">D</span> หรือปุ่ม <span class="bg-slate-800 px-2 py-0.5 rounded border border-slate-600">ลูกศร ซ้าย-ขวา</span> เพื่อเดิน</li>
                        <li>• <b class="text-white">กระโดด:</b> กดปุ่ม <span class="bg-slate-800 px-2 py-0.5 rounded border border-slate-600">W</span>, <span class="bg-slate-800 px-2 py-0.5 rounded border border-slate-600">Spacebar</span> หรือ <span class="bg-slate-800 px-2 py-0.5 rounded border border-slate-600">ลูกศรขึ้น</span></li>
                        <li>• <b class="text-white">มือถือ:</b> มีปุ่มบังคับและกระโดดจำลองบนหน้าจอ</li>
                    </ul>
                </div>
                <div class="bg-slate-900 p-4 rounded-xl border border-green-500/20">
                    <h3 class="font-bold text-green-400 text-base mb-2">🐘 การเรียนรู้คำศัพท์</h3>
                    <ul class="space-y-2 text-slate-300">
                        <li>• ผจญภัยไปเก็บสะสมเหรียญและดวงดาวตามด่านต่างๆ</li>
                        <li>• เดินชน <span class="text-yellow-400 font-bold">"กรงช่วยชีวิตสัตว์ 📦"</span> เพื่อพบกับคำถามวิเศษที่จะสอนภาษาอังกฤษ</li>
                        <li>• ตอบศัพท์ให้ถูกต้องเพื่อรับคะแนนและเพิ่มเลเวล</li>
                    </ul>
                </div>
                <div class="bg-slate-900 p-4 rounded-xl border border-red-500/20">
                    <h3 class="font-bold text-red-400 text-base mb-2">🦁 ระบบพลังชีวิตและบอส</h3>
                    <ul class="space-y-2 text-slate-300">
                        <li>• คุณมีพลังชีวิตเริ่มต้น <span class="text-red-500">❤️ 3 ดวง</span> ถ้าตอบคำศัพท์ผิด พลังจะลดลงทีละ 1 ดวง</li>
                        <li>• <b class="text-yellow-300">ด่านปะทะบอส:</b> ทุกๆ ด่านที่ 5 จะพบกับบอสสัตว์ป่าผู้พิทักษ์ด่าน ตอบคำถามให้ทันเวลาก่อนที่มันจะโจมตี!</li>
                    </ul>
                </div>
            </div>

            <button id="btn-close-howto" class="btn-gradient-red text-white py-2.5 px-6 rounded-xl font-bold text-lg mx-auto w-48 border-b-4">
                ย้อนกลับ
            </button>
        </div>

        <!-- ==================== 3. หน้าจอตารางคะแนน (LEADERBOARD) ==================== -->
        <div id="leaderboard-screen" class="absolute inset-0 z-40 hidden flex flex-col justify-between p-6 bg-slate-950/95 text-white">
            <h2 class="text-3xl font-black text-yellow-400 text-center flex items-center justify-center gap-2 mb-2">🏆 ตารางทำคะแนนสูงสุด</h2>
            
            <div class="bg-slate-900 rounded-xl p-4 border border-yellow-500/20 max-w-lg mx-auto w-full flex-grow overflow-y-auto mb-4">
                <div class="flex justify-between border-b border-slate-700 pb-2 mb-2 text-yellow-300 font-bold">
                    <span>อันดับ</span>
                    <span>ผู้ผจญภัย</span>
                    <span>คะแนนสูงสุด</span>
                </div>
                <div id="leaderboard-list" class="space-y-2">
                    <!-- โหลดมาจาก localStorage แบบ Dynamic -->
                </div>
            </div>

            <button id="btn-close-leaderboard" class="btn-gradient-red text-white py-2.5 px-6 rounded-xl font-bold text-lg mx-auto w-48 border-b-4">
                ย้อนกลับ
            </button>
        </div>

        <!-- ==================== 4. หน้าจอร้านค้าปลดล็อกสกิน (SHOP) ==================== -->
        <div id="shop-screen" class="absolute inset-0 z-40 hidden flex flex-col justify-between p-6 bg-slate-950/95 text-white">
            <div class="text-center">
                <h2 class="text-3xl font-black text-emerald-400 flex items-center justify-center gap-2">👕 ร้านค้าสกินนักสำรวจ</h2>
                <p class="text-sm text-yellow-300 mt-1">ใช้เหรียญที่ได้สะสมมาเพื่อปลดล็อกชุดพิเศษ!</p>
            </div>

            <!-- รายการสินค้าสกิน -->
            <div id="shop-items" class="grid grid-cols-4 gap-4 max-w-4xl mx-auto w-full my-4">
                <!-- สกิน 1: นักผจญภัยเริ่มต้น (ฟรี) -->
                <div class="bg-slate-900 border-2 border-slate-700 p-3 rounded-xl flex flex-col items-center justify-between text-center relative" id="skin-card-0">
                    <span class="text-4xl my-2">🤠</span>
                    <h3 class="font-bold text-sm">นักผจญภัยธรรมดา</h3>
                    <p class="text-xs text-slate-400 mt-1">เริ่มต้นฟรี</p>
                    <button class="w-full mt-3 py-1.5 rounded-lg text-xs font-bold bg-green-500 text-white" onclick="selectSkin(0)">สวมใส่</button>
                </div>
                <!-- สกิน 2: นักสำรวจป่า (10 เหรียญ) -->
                <div class="bg-slate-900 border-2 border-slate-700 p-3 rounded-xl flex flex-col items-center justify-between text-center relative" id="skin-card-1">
                    <span class="text-4xl my-2">🕵️‍♂️</span>
                    <h3 class="font-bold text-sm text-emerald-300">นักสำรวจ</h3>
                    <p class="text-xs text-yellow-300 mt-1">🟡 10 เหรียญ</p>
                    <button class="w-full mt-3 py-1.5 rounded-lg text-xs font-bold bg-slate-700 text-slate-200" id="btn-buy-skin-1" onclick="buyOrSelectSkin(1, 10)">ปลดล็อก</button>
                </div>
                <!-- สกิน 3: นักวิทยาศาสตร์ป่า (25 เหรียญ) -->
                <div class="bg-slate-900 border-2 border-slate-700 p-3 rounded-xl flex flex-col items-center justify-between text-center relative" id="skin-card-2">
                    <span class="text-4xl my-2">🥼</span>
                    <h3 class="font-bold text-sm text-blue-300">นักวิทยาศาสตร์</h3>
                    <p class="text-xs text-yellow-300 mt-1">🟡 25 เหรียญ</p>
                    <button class="w-full mt-3 py-1.5 rounded-lg text-xs font-bold bg-slate-700 text-slate-200" id="btn-buy-skin-2" onclick="buyOrSelectSkin(2, 25)">ปลดล็อก</button>
                </div>
                <!-- สกิน 4: ซูเปอร์ฮีโร่สัตว์ป่า (50 เหรียญ) -->
                <div class="bg-slate-900 border-2 border-slate-700 p-3 rounded-xl flex flex-col items-center justify-between text-center relative" id="skin-card-3">
                    <span class="text-4xl my-2">🦸‍♂️</span>
                    <h3 class="font-bold text-sm text-red-400 font-extrabold">ซูเปอร์ฮีโร่</h3>
                    <p class="text-xs text-yellow-300 mt-1">🟡 50 เหรียญ</p>
                    <button class="w-full mt-3 py-1.5 rounded-lg text-xs font-bold bg-slate-700 text-slate-200" id="btn-buy-skin-3" onclick="buyOrSelectSkin(3, 50)">ปลดล็อก</button>
                </div>
            </div>

            <!-- ส่วนล่าง: ยอดเหรียญและปุ่มปิด -->
            <div class="flex justify-between items-center max-w-xl mx-auto w-full">
                <div class="bg-black/40 px-5 py-2.5 rounded-full border border-yellow-500/30 text-yellow-300 font-black text-lg">
                    🟡 กระเป๋าเหรียญ: <span id="shop-wallet" class="fancy-font">0</span>
                </div>
                <button id="btn-close-shop" class="btn-gradient-red text-white py-2 px-6 rounded-xl font-bold text-lg border-b-4">
                    ตกลง
                </button>
            </div>
        </div>

        <!-- ==================== 5. หน้าต่างทับหน้าจอเมื่อเข้าสู่ส่วนตอบคำถาม (POPUP DIALOG - VOCABULARY) ==================== -->
        <div id="vocab-modal" class="absolute inset-0 z-50 hidden flex items-center justify-center p-6 bg-black/85 backdrop-blur-sm text-white">
            <div class="bg-gradient-to-br from-slate-900 to-indigo-950 p-6 rounded-2xl border-4 border-yellow-400 max-w-xl w-full flex flex-col items-center shadow-[0_0_50px_rgba(234,179,8,0.3)] transform scale-95 transition-transform duration-300">
                <span class="text-red-400 font-bold uppercase tracking-widest text-sm bg-red-950/60 border border-red-500/20 px-3 py-1 rounded-full mb-2 animate-pulse">📢 ช่วยชีวิตเพื่อนรักสัตว์ป่า!</span>
                
                <!-- รูปสัตว์ขนาดใหญ่ (Emoji / Canvas Vector) -->
                <div class="w-36 h-36 bg-slate-800/80 rounded-2xl border-2 border-yellow-400 flex items-center justify-center text-7xl shadow-inner relative group">
                    <span id="vocab-animal-emoji">🦆</span>
                    <button id="btn-vocab-speech-large" class="absolute bottom-2 right-2 w-10 h-10 bg-yellow-400 text-black hover:bg-yellow-300 font-black rounded-full flex items-center justify-center text-lg active:scale-90 transition-all shadow-md">🔊</button>
                </div>

                <!-- ข้อมูลคำศัพท์ -->
                <div class="text-center mt-4">
                    <h2 id="vocab-english-word" class="fancy-font text-4xl font-extrabold text-yellow-300 tracking-wider">DUCK</h2>
                    <p class="text-slate-300 text-sm mt-1">คำอ่าน: <span id="vocab-thai-reading" class="text-emerald-300 font-semibold text-lg">ดั๊ก</span></p>
                    <p class="text-slate-300 text-sm mt-1">ความหมาย: <span id="vocab-thai-meaning" class="text-white font-bold text-xl bg-indigo-900/50 px-3 py-0.5 rounded-md inline-block mt-1">เป็ด</span></p>
                </div>

                <!-- ประโยคตัวอย่างเพื่อส่งเสริมการศึกษา -->
                <div class="bg-indigo-950/80 border border-indigo-500/20 rounded-xl p-3 w-full my-3 text-center text-sm text-slate-300">
                    <b class="text-indigo-300 block mb-1">💡 ประโยคตัวอย่าง (Example sentence)</b>
                    <span id="vocab-example" class="italic font-light">"We can see a family of duck swimming happily in the pond."</span>
                </div>

                <!-- แผงปุ่มสำหรับคลาสเรียนลัด -->
                <div class="flex gap-4 w-full">
                    <button id="btn-vocab-speech-detail" class="btn-gradient-blue flex-1 text-white py-3 rounded-xl font-bold flex items-center justify-center gap-2 border-b-4">
                        🔊 ฟังเสียงภาษาอังกฤษ
                    </button>
                    <button id="btn-vocab-confirm" class="btn-gradient-green flex-1 text-white py-3 rounded-xl font-bold text-lg border-b-4">
                        เข้าใจแล้ว! ไปต่อ ➡️
                    </button>
                </div>
            </div>
        </div>

        <!-- ==================== 6. หน้าต่างปะทะมินิบอส (BOSS QUIZ SYSTEM) ==================== -->
        <div id="boss-modal" class="absolute inset-0 z-50 hidden flex items-center justify-center p-6 bg-black/90 text-white">
            <div class="bg-gradient-to-b from-slate-900 to-rose-950 p-6 rounded-2xl border-4 border-red-500 max-w-lg w-full flex flex-col items-center shadow-[0_0_50px_rgba(239,68,68,0.4)]">
                
                <!-- หัวข้อบอสและแถบเวลา -->
                <div class="text-center w-full mb-3">
                    <h2 class="fancy-font text-red-500 text-2xl font-black animate-bounce flex items-center justify-center gap-2">🔥 ด่านป้องกันตัวจากมินิบอสสัตว์ป่า! 🔥</h2>
                    <p id="boss-title" class="text-yellow-400 font-bold text-lg mt-1">บอส: ราชสีห์คำราม (Boss Lion)</p>
                    <!-- แถบเวลาจับเวลาถอยหลังในการทำโจทย์ -->
                    <div class="w-full bg-slate-800 h-4 rounded-full overflow-hidden border border-red-500/20 mt-3">
                        <div id="boss-timer-bar" class="bg-gradient-to-r from-red-500 to-amber-400 h-full w-full transition-all duration-100 ease-linear"></div>
                    </div>
                </div>

                <!-- ตัวบอส และ ภาพสัตว์ที่เป็นโจทย์ -->
                <div class="flex items-center gap-6 my-4 w-full justify-center">
                    <div class="w-24 h-24 bg-red-950 border border-red-500/30 rounded-2xl flex items-center justify-center text-6xl shadow-md">
                        <span id="boss-monster-emoji">🦁</span>
                    </div>
                    <div class="text-3xl font-black">⚔️ VS ⚔️</div>
                    <div class="w-24 h-24 bg-slate-800 border-2 border-yellow-400 rounded-2xl flex items-center justify-center text-6xl shadow-md">
                        <span id="boss-quest-emoji">🐱</span>
                    </div>
                </div>

                <!-- คำถามวิเศษ -->
                <div class="text-center mb-5">
                    <p class="text-slate-300 mb-1 text-sm">สัตว์ที่อยู่ฝั่งขวาภาษาอังกฤษคือคำว่าอะไร?</p>
                    <h3 id="boss-quest-thai" class="text-2xl font-extrabold text-yellow-300">"แมว"</h3>
                </div>

                <!-- รายการตัวเลือกคำตอบ 4 ข้อ -->
                <div class="grid grid-cols-2 gap-3 w-full" id="boss-choices-grid">
                    <button class="bg-slate-800 hover:bg-slate-700 active:scale-95 border-2 border-slate-600 p-3 rounded-xl font-bold text-lg text-slate-100 transition-all" onclick="answerBossQuest(0)">Dog</button>
                    <button class="bg-slate-800 hover:bg-slate-700 active:scale-95 border-2 border-slate-600 p-3 rounded-xl font-bold text-lg text-slate-100 transition-all" onclick="answerBossQuest(1)">Cat</button>
                    <button class="bg-slate-800 hover:bg-slate-700 active:scale-95 border-2 border-slate-600 p-3 rounded-xl font-bold text-lg text-slate-100 transition-all" onclick="answerBossQuest(2)">Rabbit</button>
                    <button class="bg-slate-800 hover:bg-slate-700 active:scale-95 border-2 border-slate-600 p-3 rounded-xl font-bold text-lg text-slate-100 transition-all" onclick="answerBossQuest(3)">Lion</button>
                </div>
            </div>
        </div>

        <!-- ==================== 7. หน้าจอผลลัพธ์แพ้/ชนะ (GAME OVER / STAGE COMPLETE SCREEN) ==================== -->
        <div id="game-over-screen" class="absolute inset-0 z-50 hidden flex flex-col justify-between p-6 bg-slate-950/95 text-white text-center">
            <div class="my-auto">
                <span id="game-over-emoji" class="text-7xl block animate-bounce">💀</span>
                <h2 id="game-over-title" class="fancy-font text-5xl font-black text-red-500 mt-4">GAME OVER</h2>
                <p id="game-over-subtitle" class="text-slate-300 mt-2 max-w-md mx-auto">อย่ายอมแพ้นะนักผจญภัยตัวน้อย! ความพยายามชนะทุกอุปสรรค ลองเรียนรู้ใหม่อีกสักครั้งกันเถอะ!</p>
                
                <!-- สถิติคะแนนที่ทำได้ -->
                <div class="grid grid-cols-4 gap-3 max-w-2xl mx-auto w-full mt-6 bg-slate-900/60 p-4 rounded-2xl border border-slate-700">
                    <div class="bg-slate-800/80 p-2.5 rounded-xl">
                        <span class="text-xs text-slate-400 block">🏆 ระดับเลเวล</span>
                        <span id="summary-level" class="fancy-font text-yellow-300 text-2xl font-black">1</span>
                    </div>
                    <div class="bg-slate-800/80 p-2.5 rounded-xl">
                        <span class="text-xs text-slate-400 block">✨ คะแนนรวม</span>
                        <span id="summary-score" class="fancy-font text-emerald-400 text-2xl font-black">0</span>
                    </div>
                    <div class="bg-slate-800/80 p-2.5 rounded-xl">
                        <span class="text-xs text-slate-400 block">🐘 ช่วยเหลือสำเร็จ</span>
                        <span id="summary-solved" class="fancy-font text-blue-400 text-2xl font-black">0 ตัว</span>
                    </div>
                    <div class="bg-slate-800/80 p-2.5 rounded-xl">
                        <span class="text-xs text-slate-400 block">🏅 เหรียญรางวัล</span>
                        <span id="summary-medal" class="fancy-font text-yellow-400 text-2xl font-black">Bronze</span>
                    </div>
                </div>
            </div>

            <!-- ปุ่มย้อนกลับไปเมนูหรือสู้ใหม่ -->
            <div class="flex gap-4 max-w-md mx-auto w-full mb-4">
                <button id="btn-return-menu" class="btn-gradient-blue flex-1 text-white py-3 rounded-xl font-bold text-lg border-b-4">
                    🏘️ หน้าหลัก
                </button>
                <button id="btn-retry" class="btn-gradient-green flex-1 text-white py-3 rounded-xl font-black text-lg border-b-4">
                    🔁 เล่นอีกครั้ง
                </button>
            </div>
        </div>

        <!-- ==================== 8. หน้าจอและคอนโทรลเลอร์เล่นเกม (GAMEPLAY VIEWPORT) ==================== -->
        <!-- ส่วนหัวแสดงสถานะแบบ Real-time -->
        <div id="game-ui" class="absolute top-0 inset-x-0 z-20 hidden p-3 flex justify-between items-center text-white bg-gradient-to-b from-black/80 to-transparent pointer-events-none">
            <!-- ซ้าย: หัวใจและด่านปัจจุบัน -->
            <div class="flex items-center gap-3">
                <div class="bg-black/50 px-3 py-1.5 rounded-full border border-red-500/30 flex items-center gap-1.5 text-base">
                    <span id="ui-hearts" class="tracking-wide">❤️ ❤️ ❤️</span>
                </div>
                <div class="bg-black/50 px-3 py-1.5 rounded-full border border-green-500/30 text-xs font-bold">
                    🌳 ด่าน: <span id="ui-stage-name" class="text-yellow-300">Forest Learning</span>
                </div>
            </div>

            <!-- ขวา: เลเวล คะแนน เหรียญ และปุ่มหยุดเกมชั่วคราว -->
            <div class="flex items-center gap-3 pointer-events-auto">
                <div class="bg-black/50 px-3 py-1.5 rounded-full border border-yellow-500/30 text-xs font-bold">
                    <span>🌟 เลเวล:</span> <span id="ui-level" class="text-yellow-400">1</span> | 
                    <span>คะแนน:</span> <span id="ui-score" class="text-emerald-300">0</span>
                </div>
                <div class="bg-black/50 px-3 py-1.5 rounded-full border border-amber-400/30 text-xs font-bold">
                    🟡 เหรียญ: <span id="ui-coins" class="text-yellow-400">0</span>
                </div>
                <button id="btn-pause" class="w-8 h-8 rounded-full bg-slate-800 hover:bg-slate-700 flex items-center justify-center text-sm border border-slate-600 font-bold active:scale-95">⏸️</button>
            </div>
        </div>

        <!-- อาร์เคดคริสตัลป๊อปอัปแจ้งความยินดีช่วงสั้นๆ (Notification Overlay) -->
        <div id="alert-msg" class="absolute top-1/4 left-1/2 -translate-x-1/2 -translate-y-1/2 z-40 hidden bg-emerald-500/90 text-white font-extrabold px-6 py-2.5 rounded-full border-2 border-yellow-300 shadow-xl pointer-events-none animate-bounce text-lg">
            ⭐ เก็บคำศัพท์สำเร็จ +10 คะแนน! ⭐
        </div>

        <!-- พื้นผิวแคนวาสสำหรับแสดงผลตัวเอนจิ้นเกม 2D Platformer -->
        <canvas id="gameCanvas" class="w-full h-full block bg-[#1e293b]"></canvas>

        <!-- ระบบปุ่มจำลองสำหรับมือถือ / สัมผัส (TOUCH CONTROLLER OVERLAY) -->
        <div id="touch-controls" class="absolute bottom-4 inset-x-4 z-20 hidden justify-between items-center pointer-events-none">
            <!-- ทิศทาง (ซ้าย-ขวา) -->
            <div class="flex gap-3 pointer-events-auto">
                <button id="btn-touch-left" class="w-16 h-16 rounded-full bg-slate-800/80 active:bg-slate-700/90 border-2 border-slate-600 text-3xl flex items-center justify-center text-white shadow-xl active:scale-90 transition-all select-none">◀️</button>
                <button id="btn-touch-right" class="w-16 h-16 rounded-full bg-slate-800/80 active:bg-slate-700/90 border-2 border-slate-600 text-3xl flex items-center justify-center text-white shadow-xl active:scale-90 transition-all select-none">▶️</button>
            </div>
            <!-- ปุ่มกระโดด -->
            <div class="pointer-events-auto">
                <button id="btn-touch-jump" class="w-20 h-20 rounded-full bg-green-500/80 active:bg-green-600/90 border-4 border-yellow-300 text-white text-xl flex items-center justify-center font-black shadow-xl active:scale-90 transition-all select-none">กระโดด</button>
            </div>
        </div>

    </div>

    <script>
        // ==========================================
        // ส่วนจัดการข้อมูลคำศัพท์ (ANIMAL VOCABULARY DATABASE)
        // ==========================================
        const vocabulary = [
            { en: "Dog", reading: "ด็อก", th: "สุนัข", emoji: "🐶", ex: "My dog likes to run and play in the garden." },
            { en: "Cat", reading: "แคท", th: "แมว", emoji: "🐱", ex: "The cat slept quietly on the comfortable sofa." },
            { en: "Tiger", reading: "ไท-เกอร์", th: "เสือ", emoji: "🐯", ex: "A tiger can run extremely fast in the wild jungle." },
            { en: "Lion", reading: "ไล-ออน", th: "สิงโต", emoji: "🦁", ex: "The lion is widely known as the king of the jungle." },
            { en: "Elephant", reading: "เอล-ละ-เฟินท์", th: "ช้าง", emoji: "🐘", ex: "An elephant is the biggest animal on the land." },
            { en: "Monkey", reading: "มัง-คี", th: "ลิง", emoji: "🐒", ex: "The clever monkey climbed the tree to pick bananas." },
            { en: "Rabbit", reading: "แรบ-บิท", th: "กระต่าย", emoji: "🐰", ex: "The little rabbit hopped quickly into the green field." },
            { en: "Horse", reading: "ฮอร์ส", th: "ม้า", emoji: "🐴", ex: "The cowboy loved riding his white horse every morning." },
            { en: "Cow", reading: "คาว", th: "วัว", emoji: "🐮", ex: "A cow eats grass and provides fresh milk for us." },
            { en: "Bear", reading: "แบร์", th: "หมี", emoji: "🐻", ex: "The brown bear looks for fresh fish in the cold river." },
            { en: "Panda", reading: "แพน-ด้า", th: "แพนด้า", emoji: "🐼", ex: "A giant panda enjoys eating green bamboo leaves all day." },
            { en: "Chicken", reading: "ชิค-เก็น", th: "ไก่", emoji: "🐔", ex: "The golden chicken woke everybody up early in the morning." },
            { en: "Duck", reading: "ดั๊ก", th: "เป็ด", emoji: "🦆", ex: "We can see a family of duck swimming happily in the pond." },
            { en: "Fish", reading: "ฟิช", th: "ปลา", emoji: "🐟", ex: "The tiny colorful fish swam quickly near the coral." },
            { en: "Whale", reading: "เวล", th: "วาฬ", emoji: "🐋", ex: "A blue whale is the largest ocean animal on Earth." },
            { en: "Dolphin", reading: "ดอล-ฟิน", th: "โลมา", emoji: "🐬", ex: "The cute dolphin leaped high above the ocean waves." },
            { en: "Penguin", reading: "เพน-กวิน", th: "เพนกวิน", emoji: "🐧", ex: "The little penguin can swim very fast in frozen waters." },
            { en: "Eagle", reading: "อี-เกิล", th: "นกอินทรี", emoji: "🦅", ex: "An eagle soared high in the beautiful blue sky." },
            { en: "Owl", reading: "เอาล์", th: "นกฮูก", emoji: "🦉", ex: "An owl usually stays awake and hunts food at night." },
            { en: "Frog", reading: "ฟร็อก", th: "กบ", emoji: "🐸", ex: "A little green frog jumped over the slippery wet leaf." },
            { en: "Snake", reading: "สเนก", th: "งู", emoji: "🐍", ex: "A long snake slid silently through the tall grass." },
            { en: "Zebra", reading: "ซี-บรา", th: "ม้าลาย", emoji: "🦓", ex: "A zebra has wonderful black and white stripes on its body." },
            { en: "Giraffe", reading: "จิ-ราฟ", th: "ยีราฟ", emoji: "🦒", ex: "A giraffe has a very long neck to reach high leaves." },
            { en: "Shark", reading: "ชาร์ก", th: "ฉลาม", emoji: "🦈", ex: "The great white shark swam fast under the deep sea." },
            { en: "Camel", reading: "แคม-เมล", th: "อูฐ", emoji: "🐫", ex: "The camel walked patiently across the hot sandy desert." },
            { en: "Kangaroo", reading: "แคน-กา-รู", th: "จิงโจ้", emoji: "🦘", ex: "A kangaroo carries its baby safely inside its front pouch." },
            { en: "Wolf", reading: "วูล์ฟ", th: "หมาป่า", emoji: "🐺", ex: "The wild wolf howled at the beautiful full moon." },
            { en: "Fox", reading: "ฟ็อกซ์", th: "สุนัขจิ้งจอก", emoji: "🦊", ex: "The smart little fox hid behind the thick bushes." },
            { en: "Deer", reading: "เดียร์", th: "กวาง", emoji: "🦌", ex: "A beautiful deer ran quickly through the quiet forest." },
            { en: "Crocodile", reading: "คร็อก-โค-ไดล์", th: "จระเข้", emoji: "🐊", ex: "The big crocodile rested silently by the muddy riverbanks." }
        ];

        // รายชื่อด่านทั้ง 5 ด่าน
        const stages = [
            { name: "Forest Learning", color: "#22c55e", bgGradient: ["#14532d", "#166534"] },
            { name: "Animal River", color: "#3b82f6", bgGradient: ["#1e3a8a", "#1d4ed8"] },
            { name: "Jungle Challenge", color: "#10b981", bgGradient: ["#064e3b", "#047857"] },
            { name: "Mountain Adventure", color: "#a855f7", bgGradient: ["#581c87", "#701a75"] },
            { name: "Animal Kingdom", color: "#eab308", bgGradient: ["#713f12", "#854d0e"] }
        ];

        // รายการรูปสกินตัวละครที่พร้อมปลดล็อก
        const skinsList = [
            { id: 0, label: "นักผจญภัยธรรมดา", emoji: "🤠", color: "#eab308", unlocked: true },
            { id: 1, label: "นักสำรวจป่า", emoji: "🕵️‍♂️", color: "#10b981", unlocked: false },
            { id: 2, label: "นักวิทยาศาสตร์ป่า", emoji: "🥼", color: "#3b82f6", unlocked: false },
            { id: 3, label: "ซูเปอร์ฮีโร่สัตว์ป่า", emoji: "🦸‍♂️", color: "#ef4444", unlocked: false }
        ];

        // ==========================================
        // ระบบเสียงประกอบสังเคราะห์ (WEB AUDIO API SYNTHESIZER)
        // ==========================================
        let audioCtx = null;
        let isSoundEnabled = true;
        let bgmInterval = null;

        function getAudioContext() {
            if (!audioCtx) {
                audioCtx = new (window.AudioContext || window.webkitAudioContext)();
            }
            if (audioCtx.state === 'suspended') {
                audioCtx.resume();
            }
            return audioCtx;
        }

        // เล่นเสียง Coin (ปิ๊ง)
        function playCoinSound() {
            if (!isSoundEnabled) return;
            try {
                const ctx = getAudioContext();
                const now = ctx.currentTime;
                const osc = ctx.createOscillator();
                const gain = ctx.createGain();
                
                osc.type = "sine";
                osc.frequency.setValueAtTime(587.33, now); // D5
                osc.frequency.setValueAtTime(880, now + 0.08); // A5
                
                gain.gain.setValueAtTime(0.1, now);
                gain.gain.exponentialRampToValueAtTime(0.01, now + 0.3);
                
                osc.connect(gain);
                gain.connect(ctx.destination);
                osc.start(now);
                osc.stop(now + 0.3);
            } catch (e) {}
        }

        // เล่นเสียง Star (วิ้ง)
        function playStarSound() {
            if (!isSoundEnabled) return;
            try {
                const ctx = getAudioContext();
                const now = ctx.currentTime;
                const osc = ctx.createOscillator();
                const gain = ctx.createGain();
                
                osc.type = "triangle";
                osc.frequency.setValueAtTime(987.77, now); // B5
                osc.frequency.setValueAtTime(1318.51, now + 0.05); // E6
                osc.frequency.setValueAtTime(1567.98, now + 0.1); // G6
                
                gain.gain.setValueAtTime(0.15, now);
                gain.gain.exponentialRampToValueAtTime(0.01, now + 0.4);
                
                osc.connect(gain);
                gain.connect(ctx.destination);
                osc.start(now);
                osc.stop(now + 0.4);
            } catch (e) {}
        }

        // เล่นเสียง Jump (ดึ๋ง)
        function playJumpSound() {
            if (!isSoundEnabled) return;
            try {
                const ctx = getAudioContext();
                const now = ctx.currentTime;
                const osc = ctx.createOscillator();
                const gain = ctx.createGain();
                
                osc.type = "sine";
                osc.frequency.setValueAtTime(200, now);
                osc.frequency.exponentialRampToValueAtTime(600, now + 0.15);
                
                gain.gain.setValueAtTime(0.15, now);
                gain.gain.exponentialRampToValueAtTime(0.01, now + 0.15);
                
                osc.connect(gain);
                gain.connect(ctx.destination);
                osc.start(now);
                osc.stop(now + 0.15);
            } catch (e) {}
        }

        // เล่นเสียงตอบถูก (เฉลิมฉลอง)
        function playCorrectSound() {
            if (!isSoundEnabled) return;
            try {
                const ctx = getAudioContext();
                const now = ctx.currentTime;
                const osc = ctx.createOscillator();
                const gain = ctx.createGain();
                
                osc.type = "triangle";
                osc.frequency.setValueAtTime(523.25, now); // C5
                osc.frequency.setValueAtTime(659.25, now + 0.1); // E5
                osc.frequency.setValueAtTime(783.99, now + 0.2); // G5
                osc.frequency.setValueAtTime(1046.50, now + 0.3); // C6
                
                gain.gain.setValueAtTime(0.2, now);
                gain.gain.exponentialRampToValueAtTime(0.01, now + 0.5);
                
                osc.connect(gain);
                gain.connect(ctx.destination);
                osc.start(now);
                osc.stop(now + 0.5);
            } catch (e) {}
        }

        // เล่นเสียงตอบผิด (ตึ่งตึ๊ง)
        function playWrongSound() {
            if (!isSoundEnabled) return;
            try {
                const ctx = getAudioContext();
                const now = ctx.currentTime;
                const osc = ctx.createOscillator();
                const gain = ctx.createGain();
                
                osc.type = "sawtooth";
                osc.frequency.setValueAtTime(220, now); // A3
                osc.frequency.setValueAtTime(146.83, now + 0.15); // D3
                
                gain.gain.setValueAtTime(0.15, now);
                gain.gain.exponentialRampToValueAtTime(0.01, now + 0.4);
                
                osc.connect(gain);
                gain.connect(ctx.destination);
                osc.start(now);
                osc.stop(now + 0.4);
            } catch (e) {}
        }

        // เล่นดนตรีแนว Retro 8-bit วนซ้ำเบาๆ ในปูมเบื้องหลัง
        function startBGM() {
            if (bgmInterval) clearInterval(bgmInterval);
            if (!isSoundEnabled) return;
            
            const notes = [261.63, 329.63, 392.00, 329.63, 293.66, 349.23, 440.00, 349.23]; // วนคอร์ด C และ Dm
            let idx = 0;
            
            bgmInterval = setInterval(() => {
                if (!isSoundEnabled) return;
                try {
                    const ctx = getAudioContext();
                    const now = ctx.currentTime;
                    const osc = ctx.createOscillator();
                    const gain = ctx.createGain();
                    
                    osc.type = "triangle";
                    osc.frequency.setValueAtTime(notes[idx % notes.length], now);
                    
                    gain.gain.setValueAtTime(0.03, now); // เสียงเบามากๆ ไม่กวนใจผู้เล่น
                    gain.gain.exponentialRampToValueAtTime(0.001, now + 0.4);
                    
                    osc.connect(gain);
                    gain.connect(ctx.destination);
                    osc.start(now);
                    osc.stop(now + 0.45);
                    idx++;
                } catch (e) {}
            }, 500);
        }

        function stopBGM() {
            if (bgmInterval) {
                clearInterval(bgmInterval);
                bgmInterval = null;
            }
        }

        // ==========================================
        // ระบบประมวลผลคำพูดสองระบบ (DUAL SPEECH ENGINE - BUG SOLVED)
        // ==========================================
        function speakWord(word) {
            // ระบบหลัก: ใช้ ResponsiveVoice API ซึ่งให้ผลเสียงชัดเจนและสม่ำเสมอในทุกอุปกรณ์
            if (typeof responsiveVoice !== 'undefined' && responsiveVoice.speak) {
                try {
                    responsiveVoice.cancel(); // ยกเลิกคิวเสียงเก่า
                    responsiveVoice.speak(word, "US English Male", {
                        pitch: 1.0,
                        rate: 0.85
                    });
                    console.log("Speech Output via ResponsiveVoice API success.");
                    return;
                } catch (e) {
                    console.error("ResponsiveVoice error, trying system TTS fallback...", e);
                }
            }

            // ระบบสำรอง: ใช้ Web Speech API ดั้งเดิมในกรณีที่เครือข่ายขัดข้อง
            if ('speechSynthesis' in window) {
                try {
                    window.speechSynthesis.cancel(); // รีเซ็ตเสียงค้าง

                    // ปลดล็อกสถานะบราวเซอร์หลับ
                    if (window.speechSynthesis.paused) {
                        window.speechSynthesis.resume();
                    }

                    const utterance = new SpeechSynthesisUtterance(word);
                    utterance.lang = 'en-US';
                    utterance.rate = 0.8;

                    // บังคับเลือกฐานเสียงภาษาอังกฤษที่ตรงที่สุด
                    const voices = window.speechSynthesis.getVoices();
                    const enVoice = voices.find(v => v.lang.startsWith('en-US') || v.lang.startsWith('en-'));
                    if (enVoice) {
                        utterance.voice = enVoice;
                    }

                    window.speechSynthesis.speak(utterance);
                    console.log("Speech Output via Web Speech API fallback success.");
                } catch (err) {
                    console.error("Web Speech API error: ", err);
                }
            } else {
                console.warn("เบราว์เซอร์นี้ไม่รองรับฐานเสียงคำศัพท์แบบสังเคราะห์");
            }
        }

        // ระบบปลดล็อค API ทันทีที่คลิกหน้าจอครั้งแรกเพื่อทำตามระเบียบรักษาความปลอดภัยของเบราว์เซอร์ยุคใหม่
        function initSpeechUnlock() {
            getAudioContext();
            speakWord(''); // กระตุ้นการเปิดระบบเสียงแบบว่าง
            window.removeEventListener('click', initSpeechUnlock);
            window.removeEventListener('touchstart', initSpeechUnlock);
        }
        window.addEventListener('click', initSpeechUnlock);
        window.addEventListener('touchstart', initSpeechUnlock);


        // ==========================================
        // ระบบจัดการข้อมูลรัฐภายในเกม (GAME STATE MANAGEMENT)
        // ==========================================
        let level = 1;
        let score = 0;
        let coins = 0;
        let lives = 3;
        let solvedVocabsCount = 0;
        let currentStageIdx = 0;
        let isGameRunning = false;
        let isPaused = false;
        let activeSkinId = 0;
        let unlockedSkins = [0]; // เก็บ ID ของสกินที่ปลดล็อกแล้ว
        let coinWallet = 0; // เหรียญถาวรที่ใช้ซื้อในร้านค้าสกิน

        // คลาสเก็บข้อมูลคะแนนสูงสุด
        let leaderboard = [];

        // โหลดข้อมูลจาก Local Storage เมื่อเปิดเกม
        function loadSaveData() {
            try {
                const savedBest = localStorage.getItem("animal_quest_best_score");
                if (savedBest) {
                    document.getElementById("best-score").innerText = savedBest;
                }
                const savedWallet = localStorage.getItem("animal_quest_wallet");
                if (savedWallet) {
                    coinWallet = parseInt(savedWallet);
                    document.getElementById("shop-wallet").innerText = coinWallet;
                }
                const savedSkins = localStorage.getItem("animal_quest_skins");
                if (savedSkins) {
                    unlockedSkins = JSON.parse(savedSkins);
                }
                const savedActiveSkin = localStorage.getItem("animal_quest_active_skin");
                if (savedActiveSkin) {
                    activeSkinId = parseInt(savedActiveSkin);
                }
                const savedLeaderboard = localStorage.getItem("animal_quest_leaderboard");
                if (savedLeaderboard) {
                    leaderboard = JSON.parse(savedLeaderboard);
                } else {
                    // มอบข้อมูลเริ่มต้นถ้านักเล่นเปิดหน้าแรก
                    leaderboard = [
                        { name: "ผู้กล้าภูผา", score: 500, date: "2026-06-01" },
                        { name: "สายลมจิ๋ว", score: 350, date: "2026-06-03" },
                        { name: "ใบไม้เขียว", score: 200, date: "2026-06-05" }
                    ];
                    localStorage.setItem("animal_quest_leaderboard", JSON.stringify(leaderboard));
                }
            } catch (e) {
                console.error("Local Storage is block by frame constraints", e);
            }
        }

        // เซฟข้อมูลการปลดล็อกและคะแนน
        function saveData() {
            try {
                localStorage.setItem("animal_quest_wallet", coinWallet);
                localStorage.setItem("animal_quest_skins", JSON.stringify(unlockedSkins));
                localStorage.setItem("animal_quest_active_skin", activeSkinId);
                
                const currentBest = parseInt(document.getElementById("best-score").innerText) || 0;
                if (score > currentBest) {
                    localStorage.setItem("animal_quest_best_score", score);
                    document.getElementById("best-score").innerText = score;
                }
            } catch (e) {}
        }

        // ปรับปรุงประวัติตารางทำสถิติสูงสุด
        function updateLeaderboard(finalScore) {
            try {
                const name = "ผู้ผจญภัยคนที่ " + (leaderboard.length + 1);
                leaderboard.push({ name: name, score: finalScore, date: new Date().toISOString().split('T')[0] });
                leaderboard.sort((a, b) => b.score - a.score);
                leaderboard = leaderboard.slice(0, 5); // เก็บแค่ Top 5
                localStorage.setItem("animal_quest_leaderboard", JSON.stringify(leaderboard));
            } catch (e) {}
        }

        // เรนเดอร์ตารางคะแนนลง HTML
        function renderLeaderboardHTML() {
            const listContainer = document.getElementById("leaderboard-list");
            listContainer.innerHTML = "";
            leaderboard.forEach((item, index) => {
                const medals = ["🥇", "🥈", "🥉", "🏅", "🏅"];
                const itemDiv = document.createElement("div");
                itemDiv.className = "flex justify-between items-center py-2 border-b border-slate-800 text-sm";
                itemDiv.innerHTML = `
                    <span class="flex items-center gap-2">
                        <span class="w-6 text-center">${medals[index]}</span>
                        <span class="${index === 0 ? 'text-yellow-300 font-extrabold' : 'text-white'}">${item.name}</span>
                    </span>
                    <span class="fancy-font text-emerald-400 font-bold">${item.score} แต้ม</span>
                `;
                listContainer.appendChild(itemDiv);
            });
        }

        // ==========================================
        // ระบบ Canvas Game Engine (PURE JAVASCRIPT)
        // ==========================================
        const canvas = document.getElementById("gameCanvas");
        const ctx = canvas.getContext("2d");

        // ปรับขนาดความละเอียดแคนวาสให้อยู่ที่ 800x450 พิกเซล (16:9) เพื่อให้สามารถสเกลการเรนเดอร์ได้อย่างสม่ำเสมอในทุกระดับความกว้างหน้าจอ
        const GAME_WIDTH = 800;
        const GAME_HEIGHT = 450;
        canvas.width = GAME_WIDTH;
        canvas.height = GAME_HEIGHT;

        // ตัวแปรคีย์บอร์ดอินพุต
        const keys = {};
        window.addEventListener("keydown", (e) => {
            keys[e.code] = true;
            if (["Space", "ArrowUp", "KeyW"].includes(e.code) && isGameRunning) {
                e.preventDefault(); // ป้องกันหน้าจอเบราว์เซอร์เลื่อน
            }
        });
        window.addEventListener("keyup", (e) => {
            keys[e.code] = false;
        });

        // คลาสตัวละครเอก (Player Entity)
        class Player {
            constructor() {
                this.width = 36;
                this.height = 54;
                this.x = 80;
                this.y = GAME_HEIGHT - 150;
                this.vx = 0;
                this.vy = 0;
                this.speed = 4.5;
                this.gravity = 0.5;
                this.jumpStrength = -10.5;
                this.isGrounded = false;
                
                // สำหรับจัดการแอนิเมชัน
                this.frameX = 0;
                this.tickCount = 0;
                this.ticksPerFrame = 6;
                this.facingRight = true;
                this.isMoving = false;
            }

            update() {
                // เคลื่อนไหวจากการกดแป้น
                this.isMoving = false;
                if (keys["KeyA"] || keys["ArrowLeft"] || touchLeftActive) {
                    this.vx = -this.speed;
                    this.facingRight = false;
                    this.isMoving = true;
                } else if (keys["KeyD"] || keys["ArrowRight"] || touchRightActive) {
                    this.vx = this.speed;
                    this.facingRight = true;
                    this.isMoving = true;
                } else {
                    this.vx = 0;
                }

                // สั่งกระโดด
                if ((keys["Space"] || keys["KeyW"] || keys["ArrowUp"] || touchJumpActive) && this.isGrounded) {
                    this.vy = this.jumpStrength;
                    this.isGrounded = false;
                    playJumpSound();
                    touchJumpActive = false; // เคลียร์สถานะทัชทันทีกันกดค้าง
                }

                // ประมวลแรงโน้มถ่วง
                this.vy += this.gravity;
                if (this.vy > 12) this.vy = 12; // เทอร์มินัลเวโลซิตี้ป้องกันตกรวดเร็วเกินไป

                // อัปเดตตำแหน่งแกน
                this.x += this.vx;
                this.y += this.vy;

                // ขีดจำกัดขอบจอด้านข้างแผนที่
                if (this.x < 0) this.x = 0;
                if (this.x > gameWorldWidth - this.width) this.x = gameWorldWidth - this.width;

                // ประมวลแอนิเมชันวิ่ง/นิ่ง
                if (this.isMoving) {
                    this.tickCount++;
                    if (this.tickCount > this.ticksPerFrame) {
                        this.tickCount = 0;
                        this.frameX = (this.frameX + 1) % 4; // แอนิเมชัน 4 เฟรม
                    }
                } else {
                    this.frameX = 0;
                }
            }

            draw() {
                ctx.save();
                ctx.translate(this.x, this.y);

                // หากหันซ้ายให้สลับแกนด้านใน
                if (!this.facingRight) {
                    ctx.translate(this.width, 0);
                    ctx.scale(-1, 1);
                }

                // กำหนดอิโมจิและเครื่องแต่งกายตามสกินที่สวมใส่
                const activeSkin = skinsList.find(s => s.id === activeSkinId) || skinsList[0];
                
                // วาดตัวละครเก๋ๆ สไตล์เกมส์ Vector นำสมัย
                // เงาขาล่าง
                ctx.fillStyle = "rgba(0,0,0,0.2)";
                ctx.beginPath();
                ctx.ellipse(this.width / 2, this.height - 4, 14, 4, 0, 0, Math.PI * 2);
                ctx.fill();

                // ร่างกายและเสื้อผ้าตามสีของสกิน
                ctx.fillStyle = activeSkin.color;
                ctx.beginPath();
                ctx.roundRect(4, 18, this.width - 8, this.height - 24, 8);
                ctx.fill();

                // หัว (อิโมจิ)
                ctx.font = "28px Arial";
                ctx.textAlign = "center";
                ctx.textBaseline = "middle";
                // ขยับหัวลอยขึ้นลงเบาๆ ตามแอนิเมชันเดิน
                const bobY = this.isMoving ? Math.sin(this.frameX * Math.PI / 2) * 2 : 0;
                ctx.fillText(activeSkin.emoji, this.width / 2, 14 + bobY);

                // ขาเคลื่อนไหวตอนวิ่ง
                ctx.strokeStyle = "#475569";
                ctx.lineWidth = 4;
                ctx.lineCap = "round";
                if (this.isMoving) {
                    const walkCycle = Math.sin(this.frameX * Math.PI / 2) * 6;
                    // ขาซ้าย
                    ctx.beginPath();
                    ctx.moveTo(12, this.height - 10);
                    ctx.lineTo(12 - walkCycle, this.height - 2);
                    ctx.stroke();
                    // ขาขวา
                    ctx.beginPath();
                    ctx.moveTo(this.width - 12, this.height - 10);
                    ctx.lineTo(this.width - 12 + walkCycle, this.height - 2);
                    ctx.stroke();
                } else {
                    // ขาตรงขณะยืนนิ่ง
                    ctx.beginPath();
                    ctx.moveTo(12, this.height - 10);
                    ctx.lineTo(12, this.height - 2);
                    ctx.moveTo(this.width - 12, this.height - 10);
                    ctx.lineTo(this.width - 12, this.height - 2);
                    ctx.stroke();
                }

                ctx.restore();
            }
        }

        // คลาสวัตถุพื้นผิว / บล็อกสิ่งกีดขวาง (Platform Entity)
        class Platform {
            constructor(x, y, w, h, isDamaging = false) {
                this.x = x;
                this.y = y;
                this.w = w;
                this.h = h;
                this.isDamaging = isDamaging; // สำหรับด่านที่มีแม่น้ำพิษหรือหนามแหลม
            }

            draw() {
                ctx.save();
                
                // เรนเดอร์ผิวสัมผัสตามประเภท
                if (this.isDamaging) {
                    // หนามหรือแม่น้ำกรดตามเฉดสีของแม่น้ำ
                    const gradient = ctx.createLinearGradient(this.x, this.y, this.x, this.y + this.h);
                    gradient.addColorStop(0, "#ef4444");
                    gradient.addColorStop(1, "#991b1b");
                    ctx.fillStyle = gradient;
                    ctx.fillRect(this.x, this.y, this.w, this.h);
                    
                    // ปลายแหลมชูขึ้นมาด้านบนสุด
                    ctx.fillStyle = "#f87171";
                    for (let px = this.x; px < this.x + this.w; px += 15) {
                        ctx.beginPath();
                        ctx.moveTo(px, this.y);
                        ctx.lineTo(px + 7.5, this.y - 8);
                        ctx.lineTo(px + 15, this.y);
                        ctx.fill();
                    }
                } else {
                    // บล็อกหญ้าด่านป่าทั่วไป
                    const currentStage = stages[currentStageIdx];
                    ctx.fillStyle = currentStage.color;
                    ctx.fillRect(this.x, this.y, this.w, 12); // ขอบหญ้าส่วนบนสุด

                    const mudGradient = ctx.createLinearGradient(this.x, this.y + 12, this.x, this.y + this.h);
                    mudGradient.addColorStop(0, "#475569");
                    mudGradient.addColorStop(1, "#1e293b");
                    ctx.fillStyle = mudGradient;
                    ctx.fillRect(this.x, this.y + 12, this.w, this.h - 12); // ส่วนดินและหินด่านล่าง
                }
                ctx.restore();
            }
        }

        // คลาสของสะสมทั่วไป: เหรียญสีทอง (Coin Entity)
        class Coin {
            constructor(x, y) {
                this.x = x;
                this.y = y;
                this.radius = 8;
                this.isCollected = false;
                this.bobOffset = Math.random() * Math.PI; // ลอยขึ้นลงพริ้วไหวไม่พร้อมกันเพื่อดูมีชีวิตชีวา
            }

            draw() {
                if (this.isCollected) return;
                ctx.save();
                const bob = Math.sin(Date.now() / 150 + this.bobOffset) * 3;
                
                // สารหล่อประกายแสงเหรียญ
                ctx.shadowBlur = 8;
                ctx.shadowColor = "#eab308";
                
                ctx.fillStyle = "#f59e0b";
                ctx.beginPath();
                ctx.arc(this.x, this.y + bob, this.radius, 0, Math.PI * 2);
                ctx.fill();

                ctx.strokeStyle = "#fef08a";
                ctx.lineWidth = 2;
                ctx.stroke();

                ctx.restore();
            }
        }

        // คลาสของสะสมพิเศษ: ดวงดาวดาวนำทาง (Star Entity)
        class Star {
            constructor(x, y) {
                this.x = x;
                this.y = y;
                this.w = 18;
                this.h = 18;
                this.isCollected = false;
            }

            draw() {
                if (this.isCollected) return;
                ctx.save();
                const pulse = 1 + Math.sin(Date.now() / 100) * 0.1;
                ctx.translate(this.x, this.y);
                ctx.scale(pulse, pulse);

                ctx.font = "18px Arial";
                ctx.textAlign = "center";
                ctx.textBaseline = "middle";
                ctx.fillText("⭐", 0, 0);
                ctx.restore();
            }
        }

        // คลาสกล่องกระดาษช่วยเหลือสัตว์ (Cage / Word Box Entity)
        class WordBox {
            constructor(x, y, vocabIndex) {
                this.x = x;
                this.y = y;
                this.w = 40;
                this.h = 40;
                this.vocabIndex = vocabIndex;
                this.isVisited = false;
            }

            draw() {
                if (this.isVisited) {
                    // วาดประตูกรงเปิดทิ้งไว้แสดงว่าสัตว์ป่าได้รับการช่วยแล้ว
                    ctx.save();
                    ctx.font = "28px Arial";
                    ctx.textAlign = "center";
                    ctx.textBaseline = "middle";
                    ctx.fillText("🔓", this.x + this.w / 2, this.y + this.h / 2);
                    ctx.restore();
                    return;
                }

                ctx.save();
                // ลักษณะเป็นกล่องของขวัญกรงสัตว์ขัดด้วยไม้คลาสสิก
                const grad = ctx.createLinearGradient(this.x, this.y, this.x, this.y + this.h);
                grad.addColorStop(0, "#f59e0b");
                grad.addColorStop(1, "#b45309");
                
                ctx.fillStyle = grad;
                ctx.beginPath();
                ctx.roundRect(this.x, this.y, this.w, this.h, 6);
                ctx.fill();

                // ขอบตะแกรงเหล็กปิดขังด้านใน
                ctx.strokeStyle = "#475569";
                ctx.lineWidth = 3;
                ctx.beginPath();
                ctx.moveTo(this.x + 10, this.y + 4);
                ctx.lineTo(this.x + 10, this.y + this.h - 4);
                ctx.moveTo(this.x + 20, this.y + 4);
                ctx.lineTo(this.x + 20, this.y + this.h - 4);
                ctx.moveTo(this.x + 30, this.y + 4);
                ctx.lineTo(this.x + 30, this.y + this.h - 4);
                ctx.stroke();

                // ป้ายเครื่องหมายคำถามวิเศษตรงกลางกรงล่อใจเด็กๆ
                ctx.fillStyle = "#ef4444";
                ctx.beginPath();
                ctx.arc(this.x + this.w / 2, this.y + this.h / 2, 8, 0, Math.PI * 2);
                ctx.fill();

                // ป้ายเครื่องหมายคำถาม
                ctx.font = "bold 12px Arial";
                ctx.fillStyle = "#ffffff";
                ctx.textAlign = "center";
                ctx.textBaseline = "middle";
                ctx.fillText("?", this.x + this.w / 2, this.y + this.h / 2);

                ctx.restore();
            }
        }

        // คลาสธงชัยปักตรงท้ายด่าน (Goal Flag Entity)
        class GoalFlag {
            constructor(x, y) {
                this.x = x;
                this.y = y;
                this.w = 32;
                this.h = 60;
            }

            draw() {
                ctx.save();
                // ด้ามเสาไม้
                ctx.fillStyle = "#94a3b8";
                ctx.fillRect(this.x, this.y, 4, this.h);
                
                // พื้นธงชัยสีส้มสะดุดตาพริ้วตามลม
                const wave = Math.sin(Date.now() / 200) * 3;
                ctx.fillStyle = "#f97316";
                ctx.beginPath();
                ctx.moveTo(this.x + 4, this.y);
                ctx.lineTo(this.x + this.w, this.y + 12 + wave);
                ctx.lineTo(this.x + 4, this.y + 24);
                ctx.closePath();
                ctx.fill();

                // รูปดาวในผืนธงชาติสุดกล้าหาญ
                ctx.font = "10px Arial";
                ctx.fillStyle = "#fff";
                ctx.fillText("⭐", this.x + 8, this.y + 14 + wave / 2);

                ctx.restore();
            }
        }

        // คลาสเอฟเฟกต์แสงเงาละอองอนุภาคสะเก็ดระยิบระยับ (Particle Effect)
        class Particle {
            constructor(x, y, color) {
                this.x = x;
                this.y = y;
                this.vx = (Math.random() - 0.5) * 5;
                this.vy = (Math.random() - 0.5) * 5;
                this.color = color;
                this.alpha = 1;
                this.size = Math.random() * 4 + 2;
                this.life = 1.0;
                this.decay = Math.random() * 0.03 + 0.015;
            }

            update() {
                this.x += this.vx;
                this.y += this.vy;
                this.alpha -= this.decay;
                if (this.alpha < 0) this.alpha = 0;
            }

            draw() {
                ctx.save();
                ctx.globalAlpha = this.alpha;
                ctx.fillStyle = this.color;
                ctx.beginPath();
                ctx.arc(this.x, this.y, this.size, 0, Math.PI * 2);
                ctx.fill();
                ctx.restore();
            }
        }

        // ==========================================
        // การสร้างแผนที่แบบกริตอัตโนมัติ (WORLD & LEVEL BUILDER)
        // ==========================================
        let player = new Player();
        let platforms = [];
        let coinsList = [];
        let starsList = [];
        let wordBoxes = [];
        let goalFlag = null;
        let particles = [];
        let gameWorldWidth = 2400; // แผนที่กว้าง 3 หน้าจอปกติ (ลื่นไหลเลื่อนข้าง)
        let cameraX = 0;

        // สุ่มรายคำศัพท์เฉพาะด่านมาวางตามกรงขัง
        function createWorldMap() {
            platforms = [];
            coinsList = [];
            starsList = [];
            wordBoxes = [];
            particles = [];
            cameraX = 0;

            // รีเซ็ตตัวละครกลับไปฝั่งซ้ายสุด
            player = new Player();
            player.x = 80;
            player.y = GAME_HEIGHT - 120;

            // กำหนดความยาวแผนที่ตามลำดับเลเวลปัจจุบัน (เลเวลเยอะ แผนที่ยาวขึ้น)
            gameWorldWidth = 1600 + (level % 5) * 400;

            // 1. วาดพื้นดินด้านล่างสุดทั้งหมดเป็นโครงฐานเวที
            platforms.push(new Platform(0, GAME_HEIGHT - 40, gameWorldWidth, 40));

            // 2. สร้างหลุมน้ำหรือจุดตกเหวพิษ (Water/Lava Pit) ท้าทายทักษะเด็กๆ
            const totalPits = 2 + Math.floor(level / 4);
            const pitPositions = [];
            for (let i = 0; i < totalPits; i++) {
                // หลุมห่างกันทีละช่วง
                const pitX = 400 + i * 400 + Math.random() * 100;
                if (pitX < gameWorldWidth - 300) {
                    pitPositions.push(pitX);
                }
            }

            // ตัดแต่งพื้นล่างให้เป็นหลุมอันตราย และใส่แผงหนามแหลมเข้าไปแทน
            pitPositions.forEach(px => {
                // ลบบล็อคพื้นล่างช่วงนั้นออกโดยแบ่งแพลตฟอร์มเป็นส่วนย่อย
                const lastPlatform = platforms.pop();
                if (lastPlatform) {
                    // แพลตฟอร์มฝั่งซ้ายของหลุม
                    platforms.push(new Platform(lastPlatform.x, lastPlatform.y, px - lastPlatform.x, 40));
                }
                // แพลตฟอร์มหนามอันตรายตรงกลางหลุม
                platforms.push(new Platform(px, GAME_HEIGHT - 30, 80, 30, true));
                // แพลตฟอร์มฝั่งขวาของหลุม
                platforms.push(new Platform(px + 80, GAME_HEIGHT - 40, gameWorldWidth - (px + 80), 40));
            });

            // 3. โปรยชั้นวางลอยฟ้า (Floating Platforms) เพื่อกระโดดไต่
            for (let px = 200; px < gameWorldWidth - 200; px += 180) {
                const py = GAME_HEIGHT - 120 - Math.random() * 80;
                const pw = 80 + Math.random() * 40;
                platforms.push(new Platform(px, py, pw, 20));

                // วางเหรียญหรือดาวตรงแท่นลอยฟ้าเหล่านี้
                if (Math.random() > 0.4) {
                    coinsList.push(new Coin(px + pw / 2, py - 15));
                } else {
                    starsList.push(new Star(px + pw / 2, py - 18));
                }
            }

            // 4. บรรจุกล่องกรงช่วยสัตว์ (Word Box) ทั่วดินแดน (ด่านละ 3 กรง)
            const chosenVocabIndices = [];
            while (chosenVocabIndices.length < 3) {
                const randIdx = Math.floor(Math.random() * vocabulary.length);
                if (!chosenVocabIndices.includes(randIdx)) {
                    chosenVocabIndices.push(randIdx);
                }
            }

            const step = gameWorldWidth / 4;
            for (let i = 0; i < 3; i++) {
                const boxX = step * (i + 1) + (Math.random() - 0.5) * 100;
                // วางกรงสัตว์ไว้บนพื้นที่มั่นคง
                wordBoxes.push(new WordBox(boxX, GAME_HEIGHT - 80, chosenVocabIndices[i]));
            }

            // 5. ปักป้ายธงชัยเส้นชัยท้ายด่านฝั่งขวาสุด
            goalFlag = new GoalFlag(gameWorldWidth - 100, GAME_HEIGHT - 100);
        }

        // ==========================================
        // ตรวจจับระบบฟิสิกส์การชนกัน (COLLISION DETECTION & RESOLUTION)
        // ==========================================
        function detectCollisions() {
            // สมมติให้อยู่บนอากาศก่อนเป็นสัจธรรม จากนั้นค่อยดักจับขอบชน
            player.isGrounded = false;

            // ตรวจการชนกับแพลตฟอร์มทั่วไป
            for (let p of platforms) {
                // ใช้การคำนวณกล่องตัดกัน (AABB Overlap)
                if (
                    player.x + player.width > p.x &&
                    player.x < p.x + p.w &&
                    player.y + player.height > p.y &&
                    player.y < p.y + p.h
                ) {
                    // หากชนบล็อกอันตราย (ตกเหวหนามแหลม)
                    if (p.isDamaging) {
                        handlePlayerHitDamage();
                        return;
                    }

                    // ยืนอยู่เหนือแท่น (ตกทะลุลงข้างล่าง)
                    if (player.vy > 0 && player.y + player.height - player.vy <= p.y + 12) {
                        player.y = p.y - player.height;
                        player.vy = 0;
                        player.isGrounded = true;
                    }
                }
            }

            // ตรวจการชนและเก็บเหรียญสีทอง
            for (let c of coinsList) {
                if (!c.isCollected &&
                    player.x + player.width > c.x - c.radius &&
                    player.x < c.x + c.radius &&
                    player.y + player.height > c.y - c.radius &&
                    player.y < c.y + c.radius
                ) {
                    c.isCollected = true;
                    score += 1;
                    coins += 1;
                    playCoinSound();
                    spawnParticles(c.x, c.y, "#eab308");
                    updateUIRealtime();
                }
            }

            // ตรวจการชนและเก็บดาวนำวิเศษ
            for (let s of starsList) {
                if (!s.isCollected &&
                    player.x + player.width > s.x - s.w/2 &&
                    player.x < s.x + s.w/2 &&
                    player.y + player.height > s.y - s.h/2 &&
                    player.y < s.y + s.h/2
                ) {
                    s.isCollected = true;
                    score += 20;
                    playStarSound();
                    spawnParticles(s.x, s.y, "#3b82f6");
                    showAlertNotification("⭐ เก็บดาวสำเร็จ +20 คะแนน!");
                    updateUIRealtime();
                }
            }

            // ตรวจชนกล่องกรงช่วยสัตว์ เพื่อเริ่มต้นบทเรียนคำศัพท์ (Vocab Quiz Dialog)
            for (let box of wordBoxes) {
                if (!box.isVisited &&
                    player.x + player.width > box.x &&
                    player.x < box.x + box.w &&
                    player.y + player.height > box.y &&
                    player.y < box.y + box.h
                ) {
                    triggerVocabLesson(box);
                }
            }

            // ตรวจพบการกระโดดแตะธงชัยท้ายด่าน สำเร็จบทเรียนด่านย่อย
            if (
                player.x + player.width > goalFlag.x &&
                player.x < goalFlag.x + goalFlag.w &&
                player.y + player.height > goalFlag.y &&
                player.y < goalFlag.y + goalFlag.h
            ) {
                handleStageClear();
            }
        }

        // กรณีชนสิ่งกีดขวางหรือหนามแหลม
        function handlePlayerHitDamage() {
            lives--;
            playWrongSound();
            spawnParticles(player.x + player.width/2, player.y + player.height/2, "#ef4444");
            
            if (lives <= 0) {
                triggerGameOver(false);
            } else {
                // พากลับจุดเซฟต้นแผนที่
                player.x = 80;
                player.y = GAME_HEIGHT - 150;
                player.vx = 0;
                player.vy = 0;
                updateUIRealtime();
                showAlertNotification("💥 ตกหนาม! เสียหัวใจ 1 ดวง 💔");
            }
        }

        // แสดงความยินดีช่วงสั้นๆ
        function showAlertNotification(msg) {
            const alertEl = document.getElementById("alert-msg");
            alertEl.innerText = msg;
            alertEl.classList.remove("hidden");
            setTimeout(() => {
                alertEl.classList.add("hidden");
            }, 1800);
        }

        // จัดการระเบิดกลุ่มละอองไฟประกายระยิบระยับ
        function spawnParticles(x, y, color) {
            for (let i = 0; i < 15; i++) {
                particles.push(new Particle(x, y, color));
            }
        }

        // ==========================================
        // ระบบจำลองทัชสกรีนควบคุมบนมือถือ (MOBILE RESPONSIVE CONTROLLER)
        // ==========================================
        let touchLeftActive = false;
        let touchRightActive = false;
        let touchJumpActive = false;

        function setupTouchListeners() {
            const btnLeft = document.getElementById("btn-touch-left");
            const btnRight = document.getElementById("btn-touch-right");
            const btnJump = document.getElementById("btn-touch-jump");

            // การซิงค์แตะปุ่มควบคุม ซ้าย-ขวา-กระโดด รองรับมัลติทัชสมบูรณ์แบบ
            btnLeft.addEventListener("touchstart", (e) => { e.preventDefault(); touchLeftActive = true; });
            btnLeft.addEventListener("touchend", (e) => { e.preventDefault(); touchLeftActive = false; });
            
            btnRight.addEventListener("touchstart", (e) => { e.preventDefault(); touchRightActive = true; });
            btnRight.addEventListener("touchend", (e) => { e.preventDefault(); touchRightActive = false; });

            btnJump.addEventListener("touchstart", (e) => { e.preventDefault(); touchJumpActive = true; });
            btnJump.addEventListener("touchend", (e) => { e.preventDefault(); touchJumpActive = false; });

            // ดักจับเพื่อให้เล่นบนจอแบบทัชได้ทันที
            if ('ontouchstart' in window) {
                document.getElementById("touch-controls").classList.remove("hidden");
                document.getElementById("touch-controls").classList.add("flex");
            }
        }

        // ==========================================
        // คลาสระบบการเรียนรู้ และ ป๊อปอัปคำศัพท์ (VOCABULARY LESSON MODAL)
        // ==========================================
        let activeQuizBox = null;

        function triggerVocabLesson(box) {
            // พักการอัปเดตแกนแอนิเมชันของฟิสิกส์ฉากหลังเพื่อความสงบในการติว
            isPaused = true;
            activeQuizBox = box;
            const item = vocabulary[box.vocabIndex];

            // กำหนดข้อมูลลงฟิลด์อินพุตป๊อปอัป
            document.getElementById("vocab-animal-emoji").innerText = item.emoji;
            document.getElementById("vocab-english-word").innerText = item.en.toUpperCase();
            document.getElementById("vocab-thai-reading").innerText = item.reading;
            document.getElementById("vocab-thai-meaning").innerText = item.th;
            document.getElementById("vocab-example").innerText = item.ex;

            // บังคับแสดงผลป๊อปอัปสไลด์มาข้างหน้า
            document.getElementById("vocab-modal").classList.remove("hidden");

            // บังคับออกเสียงภาษาอังกฤษคำศัพท์ทันทีเพื่อจูงใจการเรียนรู้
            speakWord(item.en);
        }

        // ลงทะเบียนฟังก์ชันปุ่มในหน้าป๊อปอัปศัพท์เพื่อช่วยให้น้องๆ ได้กดฟังซ้ำตามใจชอบ
        document.getElementById("btn-vocab-speech-large").addEventListener("click", () => {
            if (activeQuizBox) {
                const item = vocabulary[activeQuizBox.vocabIndex];
                speakWord(item.en);
            }
        });
        document.getElementById("btn-vocab-speech-detail").addEventListener("click", () => {
            if (activeQuizBox) {
                const item = vocabulary[activeQuizBox.vocabIndex];
                speakWord(item.en);
            }
        });

        // กดปุ่มปิด ยืนยันการเรียนรู้สะสมแต้ม
        document.getElementById("btn-vocab-confirm").addEventListener("click", () => {
            if (activeQuizBox) {
                activeQuizBox.isVisited = true;
                score += 10;
                solvedVocabsCount += 1;
                playCorrectSound();
                spawnParticles(player.x + 20, player.y, "#4ade80");
                
                // ปิดกล่องแชทและปลดล็อคเกมเพลย์หลัก
                document.getElementById("vocab-modal").classList.add("hidden");
                isPaused = false;
                activeQuizBox = null;
                updateUIRealtime();
                showAlertNotification("📚 เก่งมาก! ตอบถูกรับ +10 คะแนน 🎉");
            }
        });

        // ==========================================
        // ระบบประชันต่อสู้บอส (BOSS ENCOUNTER LEVEL SYSTEM)
        // ==========================================
        let bossTimer = null;
        let bossTimeLeft = 100; // เวลาเริ่มต้น 100%
        let currentBossQuest = null;
        let bossName = "";
        let bossEmoji = "";

        function checkAndTriggerBossStage() {
            // ทุกๆ 5 ระดับชั้น จะทำการเริ่มโหมดต่อสู้บอสแสนท้าทายแทนการวิ่งทั่วไป
            if (level % 5 === 0) {
                stopBGM();
                isPaused = true;
                
                // คัดสรรชนิดบอสตามเลเวลรอบนั้นๆ
                const bossPool = [
                    { name: "บอสสิงโตเจ้าป่า (Boss Lion)", emoji: "🦁" },
                    { name: "บอสพยัคฆ์ลายพาดกลอน (Boss Tiger)", emoji: "🐯" },
                    { name: "บอสจระเข้แม่น้ำเชี่ยว (Boss Crocodile)", emoji: "🐊" },
                    { name: "บอสฉลามจ้าวสมุทร (Boss Shark)", emoji: "🦈" }
                ];
                const selectedBoss = bossPool[(Math.floor(level / 5) - 1) % bossPool.length];
                bossName = selectedBoss.name;
                bossEmoji = selectedBoss.emoji;

                document.getElementById("boss-title").innerText = bossName;
                document.getElementById("boss-monster-emoji").innerText = bossEmoji;

                // จัดพิมพ์คำถามวิเศษประลองปัญญาจับเวลา
                generateBossVocabularyQuiz();

                document.getElementById("boss-modal").classList.remove("hidden");
                startBossTimer();
            }
        }

        function generateBossVocabularyQuiz() {
            // เลือกคำศัพท์สุ่มตัวหนึ่งเพื่อสร้างแบบทดสอบตัวเลือก
            const correctIndex = Math.floor(Math.random() * vocabulary.length);
            currentBossQuest = vocabulary[correctIndex];

            document.getElementById("boss-quest-emoji").innerText = currentBossQuest.emoji;
            document.getElementById("boss-quest-thai").innerText = `"${currentBossQuest.th}"`;

            // ดึงกลุ่มคำตอบลวงอีก 3 ข้อที่ไม่ตรงกับเฉลยหลัก
            const choices = [currentBossQuest.en];
            while (choices.length < 4) {
                const randItem = vocabulary[Math.floor(Math.random() * vocabulary.length)];
                if (!choices.includes(randItem.en)) {
                    choices.push(randItem.en);
                }
            }

            // สลับเรียงลำดับตัวเลือกใหม่กันการเดาซ้ำ
            choices.sort(() => Math.random() - 0.5);

            // วาดปุ่มกดให้เด็กๆ ลิ้มลอง
            const grid = document.getElementById("boss-choices-grid");
            grid.innerHTML = "";
            choices.forEach(choice => {
                const btn = document.createElement("button");
                btn.className = "bg-slate-800 hover:bg-slate-700 active:scale-95 border-2 border-slate-600 p-3 rounded-xl font-bold text-lg text-slate-100 transition-all";
                btn.innerText = choice;
                btn.onclick = () => selectBossAnswer(choice === currentBossQuest.en);
                grid.appendChild(btn);
            });
        }

        function startBossTimer() {
            bossTimeLeft = 100;
            const bar = document.getElementById("boss-timer-bar");
            bar.style.width = "100%";

            if (bossTimer) clearInterval(bossTimer);
            bossTimer = setInterval(() => {
                bossTimeLeft -= 2; // ลดลงทีละ 2% ทุกๆ 150ms (มีเวลาประมาณ 7-8 วินาทีต่อคำถาม)
                bar.style.width = bossTimeLeft + "%";

                if (bossTimeLeft <= 0) {
                    clearInterval(bossTimer);
                    // หมดเวลา = โดนโจมตีลบหัวใจทันที
                    selectBossAnswer(false);
                }
            }, 150);
        }

        function selectBossAnswer(isCorrect) {
            clearInterval(bossTimer);
            
            if (isCorrect) {
                score += 50;
                playCorrectSound();
                showAlertNotification("⚔️ โจมตีสำเร็จ! ชนะบอสได้รับ +50 คะแนน 🌟");
                
                // ปิดโหมดต่อสู้บอสและปลดล็อกเลเวลถัดไปได้เลย
                setTimeout(() => {
                    document.getElementById("boss-modal").classList.add("hidden");
                    isPaused = false;
                    handleStageClear();
                }, 1000);
            } else {
                lives--;
                playWrongSound();
                updateUIRealtime();
                
                if (lives <= 0) {
                    setTimeout(() => {
                        document.getElementById("boss-modal").classList.add("hidden");
                        triggerGameOver(false);
                    }, 1000);
                } else {
                    showAlertNotification("💥 โดนบอสโจมตี! เสียใจด้วยนะ เสียหัวใจ ❤️");
                    // สุ่มคำถามใหม่ทดแทนการตอบไม่ทัน
                    setTimeout(() => {
                        generateBossVocabularyQuiz();
                        startBossTimer();
                    }, 1500);
                }
            }
        }

        // ==========================================
        // การเลื่อนชั้น และ จัดระดับหน้าจอเคลียร์ด่าน (STAGE PROGRESSION)
        // ==========================================
        function handleStageClear() {
            // บันทึกสะสมเหรียญลงในบัญชีถาวรประจำกระเป๋า
            coinWallet += coins;
            saveData();

            // เลเวลขยับขึ้นก้าวหน้า
            level++;
            currentStageIdx = (currentStageIdx + 1) % stages.length;

            // ตรวจสอบรางวัลเหรียญยศตามเกณฑ์คะแนนสะสม
            checkAndTriggerBossStage();

            if (!isPaused) {
                // หากไม่ติดด่านต่อสู้บอส ให้เรนเดอร์สร้างแผนที่เวิร์ลป่าอันแสนงดงามอันใหม่
                createWorldMap();
                updateUIRealtime();
                showAlertNotification(`🎉 ผ่านด่านแล้ว! สู่ด่านต่อไป: ${stages[currentStageIdx].name} 🚀`);
                startBGM();
            }
        }

        // ปรับปรุงผลลัพธ์บนแถบข้อมูลสถิติหน้าจอหลักแบบ Real-time
        function updateUIRealtime() {
            document.getElementById("ui-level").innerText = level;
            document.getElementById("ui-score").innerText = score;
            document.getElementById("ui-coins").innerText = coins;
            document.getElementById("ui-stage-name").innerText = stages[currentStageIdx].name;

            // อัปเดตสถิติจำนวนหัวใจคงเหลือ
            let heartString = "";
            for (let i = 0; i < 3; i++) {
                if (i < lives) heartString += "❤️ ";
                else heartString += "🖤 ";
            }
            document.getElementById("ui-hearts").innerText = heartString;
        }

        // ==========================================
        // ระบบปิดฉากเกมและการมอบเหรียญเกียรติยศ (GAME OVER & REWARDS SYSTEM)
        // ==========================================
        function triggerGameOver(isVictory = false) {
            isGameRunning = false;
            stopBGM();

            // บันทึกสะสมกระเป๋าเหรียญ
            coinWallet += coins;
            saveData();
            updateLeaderboard(score);

            // แสดงหัวข้อที่เหมาะสม
            if (isVictory) {
                document.getElementById("game-over-emoji").innerText = "🏆";
                document.getElementById("game-over-title").innerText = "ชัยชนะสมบูรณ์แบบ!";
                document.getElementById("game-over-title").className = "fancy-font text-5xl font-black text-yellow-400 mt-4";
                document.getElementById("game-over-subtitle").innerText = "ยอดเยี่ยมมากนักผจญภัยตัวน้อย! คุณได้ช่วยชีวิตผองเพื่อนสัตว์ทั้งหมดในดินแดนเรียบร้อยแล้ว!";
            } else {
                document.getElementById("game-over-emoji").innerText = "💀";
                document.getElementById("game-over-title").innerText = "GAME OVER";
                document.getElementById("game-over-title").className = "fancy-font text-5xl font-black text-red-500 mt-4";
                document.getElementById("game-over-subtitle").innerText = "อย่ายอมแพ้นะนักผจญภัยตัวน้อย! ทุกความผิดพลาดคือการเรียนรู้ สู้ใหม่อีกรอบนะ!";
            }

            // คำนวณเหรียญตราเกียรติยศตามผลงานการเรียนสะสมคะแนน
            let medal = "Bronze Medal 🥉";
            if (score >= 400) medal = "Diamond Medal 💎";
            else if (score >= 250) medal = "Gold Medal 🥇";
            else if (score >= 100) medal = "Silver Medal 🥈";

            // ยิงข้อมูลใส่ลงตารางผลลัพธ์
            document.getElementById("summary-level").innerText = level;
            document.getElementById("summary-score").innerText = score;
            document.getElementById("summary-solved").innerText = solvedVocabsCount + " ตัว";
            document.getElementById("summary-medal").innerText = medal;

            // บังคับเปลี่ยนสลับเปิดหน้าแสดงผลลัพธ์
            document.getElementById("game-ui").classList.add("hidden");
            document.getElementById("touch-controls").classList.add("hidden");
            document.getElementById("game-over-screen").classList.remove("hidden");
        }

        // ==========================================
        // ส่วนควบคุมร้านค้าและการเปลี่ยนสกิน (CHARACTER SHOPPING & APPAREL)
        // ==========================================
        function updateShopUI() {
            document.getElementById("shop-wallet").innerText = coinWallet;
            
            // ปรับแต่งปุ่มแต่ละสกินตามฐานข้อมูลปลดล็อก
            skinsList.forEach(skin => {
                const btn = document.getElementById(`btn-buy-skin-${skin.id}`);
                const card = document.getElementById(`skin-card-${skin.id}`);
                
                if (btn) {
                    if (unlockedSkins.includes(skin.id)) {
                        btn.innerText = activeSkinId === skin.id ? "สวมใส่อยู่" : "สวมใส่";
                        btn.className = activeSkinId === skin.id 
                            ? "w-full mt-3 py-1.5 rounded-lg text-xs font-bold bg-green-500 text-white" 
                            : "w-full mt-3 py-1.5 rounded-lg text-xs font-bold bg-slate-700 hover:bg-slate-600 text-white";
                    } else {
                        btn.innerText = "ปลดล็อก";
                        btn.className = "w-full mt-3 py-1.5 rounded-lg text-xs font-bold bg-amber-500 hover:bg-amber-400 text-black";
                    }
                }

                // ใส่เอฟเฟกต์เด่นถ้าสกินนั้นกำลังเปิดใช้งานอยู่
                if (card) {
                    if (activeSkinId === skin.id) {
                        card.className = "bg-slate-900 border-2 border-green-500 p-3 rounded-xl flex flex-col items-center justify-between text-center relative shadow-[0_0_15px_rgba(34,197,94,0.3)]";
                    } else {
                        card.className = "bg-slate-900 border-2 border-slate-700 p-3 rounded-xl flex flex-col items-center justify-between text-center relative";
                    }
                }
            });
        }

        function buyOrSelectSkin(id, cost) {
            getAudioContext();
            if (unlockedSkins.includes(id)) {
                // สวมใส่ได้ทันทีเพราะเจ้าของเคยซื้อแล้ว
                activeSkinId = id;
                saveData();
                playStarSound();
                updateShopUI();
            } else {
                // ต้องจ่ายเหรียญเพื่อถอนกุญแจสกินนั้นๆ
                if (coinWallet >= cost) {
                    coinWallet -= cost;
                    unlockedSkins.push(id);
                    activeSkinId = id;
                    saveData();
                    playCorrectSound();
                    updateShopUI();
                    showAlertNotification(`👕 ปลดล็อกสกิน ${skinsList[id].label} สำเร็จแล้ว!`);
                } else {
                    playWrongSound();
                    showAlertNotification("❌ เหรียญไม่เพียงพอสะสมเพิ่มเติมระหว่างวิ่งนะ!");
                }
            }
        }

        // เชื่อมปุ่มสวมใส่เริ่มต้นให้ทำงานลื่นไหล
        function selectSkin(id) {
            activeSkinId = id;
            saveData();
            playStarSound();
            updateShopUI();
        }

        // ==========================================
        // วงจรรูปวาดภาพเคลื่อนไหว 60 FPS (ANIMATION & GAME LOOP)
        // ==========================================
        function gameLoop() {
            if (!isGameRunning) return;

            // คืนค่าอัปเดตฉากหากไม่ติดหน้าจอเรียนรู้
            if (!isPaused) {
                // อัปเดตข้อมูลนักสำรวจ
                player.update();

                // ตรวจจับแกนระนาบการไหลผ่านกล้อง (Camera Smooth Side-scrolling)
                // ดึงตำแหน่งตรงกลางจอภาพของตัวละครยึดแกนเป็นพิกัดกึ่งกลาง
                const targetCameraX = player.x - GAME_WIDTH / 2;
                cameraX += (targetCameraX - cameraX) * 0.1; // ความหนืดลื่นไหลสไตล์โมเดิร์น
                
                // จำกัดทิศทางมุมกล้องไม่ให้ออกนอกระดับพรมแดนโลก
                if (cameraX < 0) cameraX = 0;
                if (cameraX > gameWorldWidth - GAME_WIDTH) cameraX = gameWorldWidth - GAME_WIDTH;

                // ประมวลระบบฟิสิกส์วิชาชน
                detectCollisions();

                // อัปเดตอนุภาคสะเก็ดระยิบระยับ
                particles.forEach((p, idx) => {
                    p.update();
                    if (p.alpha <= 0) particles.splice(idx, 1);
                });
            }

            // --- ส่วนของขั้นตอนการเรนเดอร์ลงวาด Canvas (DRAWING PHASE) ---
            ctx.clearRect(0, 0, GAME_WIDTH, GAME_HEIGHT);

            // วาดพื้นหลังท้องฟ้าไล่ระดับอย่างละเมียดละไมตามสภาพด่านแต่ละแบบ
            const currentStage = stages[currentStageIdx];
            const skyGrad = ctx.createLinearGradient(0, 0, 0, GAME_HEIGHT);
            skyGrad.addColorStop(0, currentStage.bgGradient[0]);
            skyGrad.addColorStop(1, currentStage.bgGradient[1]);
            ctx.fillStyle = skyGrad;
            ctx.fillRect(0, 0, GAME_WIDTH, GAME_HEIGHT);

            // บรรจบมิติก้อนเมฆลอยละล่องด้านหลังเพื่อความมีมิติเชิงลึก (Parallax Layer)
            ctx.save();
            ctx.fillStyle = "rgba(255,255,255,0.06)";
            for (let i = 0; i < 6; i++) {
                const cloudX = (i * 400 - cameraX * 0.3) % (gameWorldWidth * 0.5) + 100;
                ctx.beginPath();
                ctx.arc(cloudX, 80 + i * 15, 45, 0, Math.PI * 2);
                ctx.arc(cloudX + 40, 80 + i * 15, 35, 0, Math.PI * 2);
                ctx.arc(cloudX - 40, 80 + i * 15, 35, 0, Math.PI * 2);
                ctx.fill();
            }
            ctx.restore();

            // นำผลลัพธ์เข้าสู่ระนาบพิกัดสไลด์ตามกล้อง (Offset translation)
            ctx.save();
            ctx.translate(-cameraX, 0);

            // วาดแท่นเหวและหนามสิ่งกีดขวาง
            platforms.forEach(p => p.draw());

            // วาดสะสมเหรียญ
            coinsList.forEach(c => c.draw());

            // วาดสะสมดวงดาวพิเศษ
            starsList.forEach(s => s.draw());

            // วาดกรงแชทคำศัพท์กล่องสัตว์ป่า
            wordBoxes.forEach(b => b.draw());

            // วาดธงชัยปักตรงท้ายด่าน
            goalFlag.draw();

            // วาดเอฟเฟกต์สะเก็ดไฟลื่นไหล
            particles.forEach(p => p.draw());

            // วาดร่างตัวละครนักผจญภัยยอดพริ้ว
            player.draw();

            ctx.restore();

            // สั่งทำเฟรมถัดไปด้วยมาตรฐานเฟรมเรตสากล
            requestAnimationFrame(gameLoop);
        }

        // ==========================================
        // การเชื่อมต่อปุ่มเมนู หน้าจอ และปุ่มจัดการทั่วไป (UI CONTROLS & NAVIGATION)
        // ==========================================
        function startGame() {
            getAudioContext();
            
            // รีเซ็ตค่าสถิติเมื่อสตาร์ตใหม่
            level = 1;
            score = 0;
            coins = 0;
            lives = 3;
            solvedVocabsCount = 0;
            currentStageIdx = 0;
            isPaused = false;

            // นำสกินปัจจุบันมาประกอบร่างสร้างด่าน
            createWorldMap();
            updateUIRealtime();

            // ซ่อนหน้าเมนูและเตรียมก้าวทะยานเข้าสู่โลก
            document.getElementById("main-menu").classList.add("hidden");
            document.getElementById("game-over-screen").classList.add("hidden");
            document.getElementById("game-ui").classList.remove("hidden");
            
            // แสดงทัชสกรีนบนจออุปกรณ์พกพา
            if ('ontouchstart' in window) {
                document.getElementById("touch-controls").classList.remove("hidden");
                document.getElementById("touch-controls").classList.add("flex");
            }

            isGameRunning = true;
            startBGM();
            gameLoop();
        }

        // ลงทะเบียนระบบรับแรงคลิกทุกส่วนหน้าจอหลัก
        document.getElementById("btn-play").addEventListener("click", () => {
            startGame();
        });

        document.getElementById("btn-how-to").addEventListener("click", () => {
            getAudioContext();
            document.getElementById("how-to-screen").classList.remove("hidden");
        });
        document.getElementById("btn-close-howto").addEventListener("click", () => {
            getAudioContext();
            document.getElementById("how-to-screen").classList.add("hidden");
        });

        document.getElementById("btn-leaderboard").addEventListener("click", () => {
            getAudioContext();
            renderLeaderboardHTML();
            document.getElementById("leaderboard-screen").classList.remove("hidden");
        });
        document.getElementById("btn-close-leaderboard").addEventListener("click", () => {
            getAudioContext();
            document.getElementById("leaderboard-screen").classList.add("hidden");
        });

        document.getElementById("btn-shop").addEventListener("click", () => {
            getAudioContext();
            updateShopUI();
            document.getElementById("shop-screen").classList.remove("hidden");
        });
        document.getElementById("btn-close-shop").addEventListener("click", () => {
            getAudioContext();
            document.getElementById("shop-screen").classList.add("hidden");
        });

        document.getElementById("btn-retry").addEventListener("click", () => {
            startGame();
        });
        document.getElementById("btn-return-menu").addEventListener("click", () => {
            getAudioContext();
            document.getElementById("game-over-screen").classList.add("hidden");
            document.getElementById("main-menu").classList.remove("hidden");
        });

        // เปิด/ปิดเสียง
        document.getElementById("btn-toggle-sound").addEventListener("click", () => {
            isSoundEnabled = !isSoundEnabled;
            document.getElementById("btn-toggle-sound").innerText = isSoundEnabled ? "🔊" : "🔇";
            if (isSoundEnabled) {
                getAudioContext();
                startBGM();
            } else {
                stopBGM();
            }
        });

        // ปุ่มหยุดพักด่านชั่วคราวขณะเล่น
        document.getElementById("btn-pause").addEventListener("click", () => {
            isPaused = !isPaused;
            document.getElementById("btn-pause").innerText = isPaused ? "▶️" : "⏸️";
            if (isPaused) {
                stopBGM();
            } else {
                startBGM();
            }
        });

        // ทำความสะอาดระบบและโหลดบันทึกเมื่อเปิดหน้าขึ้นครั้งแรกสุด
        window.addEventListener("load", () => {
            loadSaveData();
            setupTouchListeners();
        });
    </script>
</body>
</html>

```
