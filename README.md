[index.html](https://github.com/user-attachments/files/27803357/index.html)
<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Matte Kar Çorabı - Karlı Yol Macerası</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Russo+One&family=Orbitron:wght@400;700;900&display=swap');
        *{margin:0;padding:0;box-sizing:border-box}
        body{background:#0a0e1a;overflow:hidden;font-family:'Orbitron',sans-serif;cursor:default;user-select:none}
        canvas{display:block}

        #startScreen{
            position:fixed;top:0;left:0;width:100%;height:100%;
            background:linear-gradient(180deg,#0d1b2a,#1b2a4a 40%,#2a3d6a);
            display:flex;flex-direction:column;align-items:center;justify-content:center;z-index:100;overflow:hidden;
        }
        #startScreen::before{
            content:'';position:absolute;top:0;left:0;width:100%;height:100%;
            background:radial-gradient(2px 2px at 20% 30%,#fff,transparent),radial-gradient(2px 2px at 40% 70%,#fff,transparent),
            radial-gradient(1px 1px at 60% 20%,#fff,transparent),radial-gradient(2px 2px at 80% 50%,#fff,transparent),
            radial-gradient(1px 1px at 10% 80%,#fff,transparent),radial-gradient(1px 1px at 70% 90%,#fff,transparent);
            animation:snowDrift 8s linear infinite;opacity:.6;
        }
        @keyframes snowDrift{0%{transform:translateY(-10px)}100%{transform:translateY(10px)}}
        .startLogo{width:200px;margin-bottom:15px;filter:drop-shadow(0 0 20px rgba(255,255,255,.3));z-index:2}
        .gameTitle{font-family:'Russo One',sans-serif;font-size:38px;color:#fff;text-shadow:0 0 30px rgba(100,180,255,.5);margin-bottom:8px;z-index:2;text-align:center;line-height:1.2}
        .gameSubtitle{font-size:14px;color:#8ab4f8;margin-bottom:35px;z-index:2;text-align:center;letter-spacing:3px}
        #startBtn{
            font-family:'Russo One',sans-serif;font-size:26px;padding:16px 65px;
            background:linear-gradient(135deg,#1a73e8,#4fc3f7);color:#fff;
            border:none;border-radius:50px;cursor:pointer;z-index:2;letter-spacing:3px;
            transition:all .3s;box-shadow:0 8px 32px rgba(26,115,232,.4);position:relative;overflow:hidden;
        }
        #startBtn::after{content:'';position:absolute;top:-50%;left:-50%;width:200%;height:200%;background:linear-gradient(45deg,transparent,rgba(255,255,255,.1),transparent);animation:shimmer 3s infinite}
        @keyframes shimmer{0%{transform:translateX(-100%) rotate(45deg)}100%{transform:translateX(100%) rotate(45deg)}}
        #startBtn:hover{transform:translateY(-3px) scale(1.05)}
        .instructions{margin-top:25px;color:rgba(255,255,255,.5);font-size:10px;z-index:2;text-align:center;line-height:2;letter-spacing:1px}
        .instructions span{color:#4fc3f7;font-weight:700}

        #stageScreen{
            position:fixed;top:0;left:0;width:100%;height:100%;
            background:rgba(5,10,20,.95);display:none;
            flex-direction:column;align-items:center;justify-content:center;z-index:100;backdrop-filter:blur(8px);
        }
        .stageTitle{font-family:'Russo One',sans-serif;font-size:22px;color:#8ab4f8;letter-spacing:4px;margin-bottom:8px}
        .stageName{font-family:'Russo One',sans-serif;font-size:48px;color:#fff;text-shadow:0 0 40px rgba(100,180,255,.5);margin-bottom:5px}
        .stageFlag{font-size:64px;margin-bottom:15px}
        .stageInfo{color:rgba(255,255,255,.5);font-size:12px;letter-spacing:2px;margin-bottom:30px}
        .stageScore{color:#ffd740;font-size:16px;letter-spacing:2px;margin-bottom:25px}
        #stageContinueBtn{
            font-family:'Russo One',sans-serif;font-size:20px;padding:14px 50px;
            background:linear-gradient(135deg,#1a73e8,#4fc3f7);color:#fff;
            border:none;border-radius:50px;cursor:pointer;letter-spacing:2px;transition:all .3s;
            box-shadow:0 8px 25px rgba(26,115,232,.4);
        }
        #stageContinueBtn:hover{transform:translateY(-2px) scale(1.05)}

        #gameOverScreen{
            position:fixed;top:0;left:0;width:100%;height:100%;
            background:rgba(5,10,20,.92);display:none;
            flex-direction:column;align-items:center;justify-content:center;z-index:100;backdrop-filter:blur(8px);
        }
        .goTitle{font-family:'Russo One',sans-serif;font-size:50px;color:#ff4444;text-shadow:0 0 40px rgba(255,50,50,.5);margin-bottom:25px}
        .goStats{color:#ccc;font-size:14px;margin-bottom:6px;letter-spacing:2px}
        .goStats .val{color:#4fc3f7;font-weight:700;font-size:20px}
        #restartBtn{
            font-family:'Russo One',sans-serif;font-size:20px;padding:14px 55px;
            background:linear-gradient(135deg,#e84118,#ff6348);color:#fff;
            border:none;border-radius:50px;cursor:pointer;margin-top:30px;letter-spacing:2px;transition:all .3s;
            box-shadow:0 8px 25px rgba(232,65,24,.4);
        }
        #restartBtn:hover{transform:translateY(-2px) scale(1.05)}

        #winScreen{
            position:fixed;top:0;left:0;width:100%;height:100%;
            background:rgba(5,15,30,.95);display:none;
            flex-direction:column;align-items:center;justify-content:center;z-index:100;backdrop-filter:blur(8px);
        }
        .winTitle{font-family:'Russo One',sans-serif;font-size:42px;color:#ffd740;text-shadow:0 0 40px rgba(255,215,64,.5);margin-bottom:10px}
        .winSub{font-family:'Russo One',sans-serif;font-size:18px;color:#69f0ae;margin-bottom:25px;letter-spacing:3px}

        .timeBonus{position:fixed;color:#ffd740;font-family:'Russo One',sans-serif;font-size:24px;pointer-events:none;z-index:60;text-shadow:0 0 15px rgba(255,215,64,.8);animation:floatUp 1s ease-out forwards}
        .speedBonus{position:fixed;color:#69f0ae;font-family:'Russo One',sans-serif;font-size:20px;pointer-events:none;z-index:60;text-shadow:0 0 15px rgba(105,240,174,.8);animation:floatUp 1s ease-out forwards}
        .warningText{position:fixed;color:#ff6b6b;font-family:'Russo One',sans-serif;font-size:18px;pointer-events:none;z-index:60;text-shadow:0 0 15px rgba(255,80,80,.8);animation:floatUp 1s ease-out forwards}
        @keyframes floatUp{0%{opacity:1;transform:translateY(0) scale(1)}100%{opacity:0;transform:translateY(-70px) scale(1.3)}}
        .collectFlash{position:fixed;top:0;left:0;width:100%;height:100%;background:radial-gradient(circle,rgba(255,215,64,.12),transparent 70%);pointer-events:none;z-index:55;animation:flash .3s ease-out forwards}
        @keyframes flash{0%{opacity:1}100%{opacity:0}}

        .laneIndicator{position:fixed;bottom:25px;left:50%;transform:translateX(-50%);display:none;gap:10px;z-index:50;pointer-events:none}
        .laneDot{width:10px;height:10px;border-radius:50%;background:rgba(255,255,255,.2);border:2px solid rgba(255,255,255,.3);transition:all .3s}
        .laneDot.active{background:#4fc3f7;border-color:#4fc3f7;box-shadow:0 0 10px rgba(79,195,247,.6)}

        .touchHints{position:fixed;bottom:60px;left:0;width:100%;display:none;justify-content:space-between;padding:0 30px;z-index:50;pointer-events:none}
        .touchArrow{width:50px;height:50px;border-radius:50%;background:rgba(255,255,255,.08);border:1px solid rgba(255,255,255,.15);display:flex;align-items:center;justify-content:center;font-size:20px;color:rgba(255,255,255,.3);pointer-events:auto}
        @media(hover:none)and(pointer:coarse){.touchHints{display:flex!important}}

        .leaderboard{margin-top:18px;width:280px}
        .lbTitle{color:#ffd740;font-size:12px;letter-spacing:3px;margin-bottom:8px;text-align:center}
        .lbRow{display:flex;justify-content:space-between;align-items:center;padding:5px 14px;border-radius:8px;margin-bottom:3px;font-size:11px}
        .lbRow.gold{background:rgba(255,215,64,.12);border:1px solid rgba(255,215,64,.25)}
        .lbRow.normal{background:rgba(255,255,255,.05);border:1px solid rgba(255,255,255,.08)}
        .lbRank{color:#ffd740;font-weight:700;width:24px}
        .lbName{color:#fff;font-weight:700;flex:1;text-align:center;letter-spacing:2px}
        .lbScore{color:#4fc3f7;font-weight:700;width:60px;text-align:right}
        .lbNew{color:#69f0ae;font-size:8px;margin-left:6px;animation:blink 1s infinite}
        @keyframes blink{0%,100%{opacity:1}50%{opacity:.3}}

        #nameScreen{
            position:fixed;top:0;left:0;width:100%;height:100%;
            background:rgba(5,10,20,.95);display:none;
            flex-direction:column;align-items:center;justify-content:center;z-index:110;backdrop-filter:blur(10px);
        }
        .nameTitle{font-family:'Russo One',sans-serif;font-size:28px;color:#ffd740;text-shadow:0 0 30px rgba(255,215,64,.4);margin-bottom:5px}
        .nameSub{font-size:12px;color:#8ab4f8;letter-spacing:2px;margin-bottom:25px}
        .nameInput{display:flex;gap:12px;margin-bottom:25px}
        .nameChar{width:50px;height:60px;background:rgba(255,255,255,.08);border:2px solid rgba(255,255,255,.2);border-radius:10px;
            display:flex;align-items:center;justify-content:center;font-family:'Russo One',sans-serif;font-size:32px;color:#fff;transition:all .2s}
        .nameChar.active{border-color:#4fc3f7;background:rgba(79,195,247,.12);box-shadow:0 0 15px rgba(79,195,247,.3)}
        .nameHint{color:rgba(255,255,255,.35);font-size:9px;letter-spacing:1px;margin-bottom:20px}
        #nameConfirmBtn{
            font-family:'Russo One',sans-serif;font-size:18px;padding:12px 45px;
            background:linear-gradient(135deg,#ffd740,#ffb300);color:#1a1a2e;
            border:none;border-radius:50px;cursor:pointer;letter-spacing:2px;transition:all .3s;
            box-shadow:0 6px 20px rgba(255,215,64,.3);
        }
        #nameConfirmBtn:hover{transform:translateY(-2px) scale(1.05)}

        #stageCompleteScreen{
            position:fixed;top:0;left:0;width:100%;height:100%;
            display:none;flex-direction:column;align-items:center;justify-content:center;z-index:90;
            pointer-events:none;
        }
        .scOverlay{
            background:rgba(5,10,25,.75);backdrop-filter:blur(4px);
            padding:35px 50px;border-radius:20px;border:1px solid rgba(255,255,255,.1);
            display:flex;flex-direction:column;align-items:center;
            animation:scaleIn .5s ease-out;
            pointer-events:auto;
        }
        @keyframes scaleIn{0%{opacity:0;transform:scale(.7)}100%{opacity:1;transform:scale(1)}}
        .scCheck{font-size:50px;margin-bottom:8px}
        .scTitle{font-family:'Russo One',sans-serif;font-size:26px;color:#69f0ae;text-shadow:0 0 20px rgba(105,240,174,.4);margin-bottom:4px}
        .scStage{font-size:13px;color:#8ab4f8;letter-spacing:3px;margin-bottom:18px}
        .scRow{display:flex;align-items:center;gap:10px;margin-bottom:6px}
        .scIcon{font-size:18px}
        .scLabel{color:rgba(255,255,255,.5);font-size:10px;letter-spacing:2px;width:80px;text-align:right}
        .scVal{color:#fff;font-size:18px;font-weight:700;font-family:'Russo One',sans-serif}
        .scVal.time{color:#4fc3f7}
        .scVal.score{color:#ffd740}
        .scVal.speed{color:#ff8a65}
        .scVal.socks{color:#69f0ae}
        .scNext{margin-top:18px;display:flex;align-items:center;gap:10px;padding:10px 20px;background:rgba(255,255,255,.06);border-radius:12px;border:1px solid rgba(255,255,255,.1)}
        .scNextFlag{font-size:28px}
        .scNextInfo{display:flex;flex-direction:column}
        .scNextLabel{color:rgba(255,255,255,.4);font-size:8px;letter-spacing:2px}
        .scNextName{color:#fff;font-family:'Russo One',sans-serif;font-size:16px}
        .scHint{color:rgba(255,255,255,.3);font-size:9px;margin-top:15px;letter-spacing:1px;animation:blink 1.5s infinite}

        #winFinalScreen{
            position:fixed;top:0;left:0;width:100%;height:100%;
            display:none;flex-direction:column;align-items:center;justify-content:center;
            z-index:100;
        }
        .wfBg{position:absolute;top:0;left:0;width:100%;height:100%;background:linear-gradient(180deg,#0a0e1a 0%,#0d1b2a 30%,#1a2a4a 60%,#0d1b2a 100%)}
        .wfContent{position:relative;z-index:2;display:flex;flex-direction:column;align-items:center;animation:scaleIn .8s ease-out}
        .wfHome{
            font-family:'Russo One',sans-serif;font-size:28px;color:#e0e0e0;
            margin-bottom:30px;letter-spacing:2px;text-align:center;
            animation:fadeSlide 1s ease-out .3s both;
        }
        @keyframes fadeSlide{0%{opacity:0;transform:translateY(20px)}100%{opacity:1;transform:translateY(0)}}
        .wfLogo{
            width:280px;margin-bottom:12px;
            filter:drop-shadow(0 0 30px rgba(255,255,255,.2));
            animation:fadeSlide 1s ease-out .6s both;
        }
        .wfSlogan{
            font-family:'Russo One',sans-serif;font-size:22px;
            color:#4fc3f7;letter-spacing:4px;text-align:center;
            text-shadow:0 0 25px rgba(79,195,247,.4);
            margin-bottom:25px;
            animation:fadeSlide 1s ease-out .9s both;
        }
        .wfDivider{width:200px;height:1px;background:linear-gradient(90deg,transparent,rgba(255,255,255,.2),transparent);margin-bottom:20px;animation:fadeSlide 1s ease-out 1s both}
        .wfFlag{font-size:20px;margin-bottom:18px;letter-spacing:8px;animation:fadeSlide 1s ease-out 1.1s both}
        .wfStats{display:flex;flex-direction:column;align-items:center;gap:4px;margin-bottom:12px;animation:fadeSlide 1s ease-out 1.3s both}
        .wfBtn{
            margin-top:18px;font-family:'Russo One',sans-serif;font-size:20px;padding:14px 55px;
            background:linear-gradient(135deg,#1a73e8,#4fc3f7);color:#fff;
            border:none;border-radius:50px;cursor:pointer;letter-spacing:2px;
            box-shadow:0 8px 25px rgba(26,115,232,.4);transition:all .3s;
            animation:fadeSlide 1s ease-out 1.5s both;
        }
        .wfBtn:hover{transform:translateY(-2px) scale(1.05)}

        .wfPodium{animation:fadeSlide 1s ease-out .5s both;margin-bottom:15px}
        .podiumScene{display:flex;flex-direction:column;align-items:center;position:relative}
        .podiumRacer{position:relative;width:50px;height:70px;margin-bottom:-5px;animation:racerBounce 1.5s ease-in-out infinite}
        @keyframes racerBounce{0%,100%{transform:translateY(0)}50%{transform:translateY(-6px)}}
        .racerHelmet{width:28px;height:22px;background:#c0392b;border-radius:14px 14px 8px 8px;position:absolute;top:0;left:11px;border:2px solid #962d22}
        .racerVisor{width:20px;height:6px;background:rgba(50,50,50,.8);border-radius:3px;position:absolute;top:10px;left:15px}
        .racerBody{width:24px;height:26px;background:#e74c3c;border-radius:6px;position:absolute;top:22px;left:13px;border:1px solid #c0392b}
        .racerArmL{width:8px;height:20px;background:#e74c3c;border-radius:4px;position:absolute;top:18px;left:3px;transform:rotate(30deg);border:1px solid #c0392b}
        .racerArmR{width:8px;height:20px;background:#e74c3c;border-radius:4px;position:absolute;top:18px;right:3px;transform:rotate(-30deg);border:1px solid #c0392b}
        .racerTrophy{position:absolute;top:-18px;left:50%;transform:translateX(-50%);font-size:24px;animation:trophyGlow 2s ease-in-out infinite}
        @keyframes trophyGlow{0%,100%{filter:drop-shadow(0 0 5px rgba(255,215,0,.3))}50%{filter:drop-shadow(0 0 15px rgba(255,215,0,.8))}}
        .podiumBlock{width:70px;height:45px;background:linear-gradient(180deg,#ffd740,#f9a825);border-radius:6px 6px 0 0;display:flex;align-items:center;justify-content:center;border:2px solid #f57f17;border-bottom:none}
        .podiumNum{font-family:'Russo One',sans-serif;font-size:24px;color:#5d4037;text-shadow:0 1px 0 rgba(255,255,255,.3)}
    </style>
</head>
<body>

<div id="startScreen">
    <img src="https://www.matteauto.com/wp-content/uploads/2024/11/Matte-Logo-Only-1-scaled-267x60.png" class="startLogo" alt="Matte Logo">
    <div class="gameTitle">KARLI YOL<br>MACERASI</div>
    <div class="gameSubtitle">KAR ÇORAPLARINI TOPLA • EVE GÜVENLE VAR</div>
    <button id="startBtn" onclick="startGame()">BAŞLA</button>
    <div class="instructions">
        <span>← →</span> veya <span>ekrana dokun/kaydır</span> ile şerit değiştir<br>
        <span>🛞 Kar Çorabı</span> topla → <span>+5 puan + hız artışı</span><br>
        <span>⚠️ Engellere / Çukurlara</span> çarpma → <span>GAME OVER</span><br>
        <span>3 ETAP:</span> İstanbul → Paris → Berlin
    </div>
</div>

<div id="stageScreen">
    <img src="https://www.matteauto.com/wp-content/uploads/2024/11/Matte-Logo-Only-1-scaled-267x60.png" class="startLogo" style="width:160px;margin-bottom:10px" alt="Matte Logo">
    <div class="stageTitle" id="stageLabel">ETAP 1</div>
    <div class="stageName" id="stageName">İSTANBUL</div>
    <div class="stageInfo" id="stageInfo">Karlı Boğaz Yolu</div>
    <div class="stageFlag" id="stageFlag" style="margin-bottom:10px">🇹🇷</div>
    <div style="display:flex;gap:18px;align-items:center;margin:15px 0 8px;z-index:2">
        <div style="text-align:center;opacity:.5;font-size:10px;letter-spacing:1px"><div style="font-size:28px;margin-bottom:2px">🇹🇷</div>ETAP 1</div>
        <div style="color:rgba(255,255,255,.3)">→</div>
        <div style="text-align:center;opacity:.5;font-size:10px;letter-spacing:1px"><div style="font-size:28px;margin-bottom:2px">🇫🇷</div>ETAP 2</div>
        <div style="color:rgba(255,255,255,.3)">→</div>
        <div style="text-align:center;opacity:.5;font-size:10px;letter-spacing:1px"><div style="font-size:28px;margin-bottom:2px">🇩🇪</div>ETAP 3</div>
    </div>
    <div id="stageHighlight" style="color:#4fc3f7;font-size:9px;letter-spacing:2px;margin-bottom:15px;z-index:2"></div>
    <div class="stageScore" id="stageScoreText"></div>
    <button id="stageContinueBtn" onclick="continueStage()">DEVAM</button>
</div>

<div id="gameOverScreen">
    <div class="goTitle">GAME OVER</div>
    <div class="goStats">ETAP <span class="val" id="goStage">1</span></div>
    <div class="goStats">TOPLAM PUAN <span class="val" id="goScore">0</span></div>
    <div class="goStats">TOPLANAN ÇORAP <span class="val" id="goSocks">0</span></div>
    <div class="goStats">MAX HIZ <span class="val" id="goSpeed">0</span> km/h</div>
    <div class="leaderboard" id="goLeaderboard"></div>
    <button id="restartBtn" onclick="restartGame()">TEKRAR OYNA</button>
</div>

<div id="winScreen" style="display:none"></div>

<div id="winFinalScreen">
    <div class="wfBg"></div>
    <canvas id="confettiCanvas" style="position:absolute;top:0;left:0;width:100%;height:100%;z-index:1;pointer-events:none"></canvas>
    <div class="wfContent">
        <div class="wfHome">Evine güvenle vardın.</div>
        <img src="https://www.matteauto.com/wp-content/uploads/2024/11/Matte-Logo-Only-1-scaled-267x60.png" class="wfLogo" alt="Matte">
        <div class="wfSlogan">Brings you home, Safely!</div>
        <div class="wfDivider"></div>
        <div class="wfPodium" id="wfPodium">
            <div class="podiumScene">
                <div class="podiumRacer">
                    <div class="racerHelmet"></div>
                    <div class="racerVisor"></div>
                    <div class="racerBody"></div>
                    <div class="racerArmL"></div>
                    <div class="racerArmR"></div>
                    <div class="racerTrophy">🏆</div>
                </div>
                <div class="podiumBlock">
                    <div class="podiumNum">1</div>
                </div>
            </div>
        </div>
        <div class="wfFlag">🇹🇷 → 🇫🇷 → 🇩🇪 ✅</div>
        <div class="wfStats">
            <div class="goStats">TOPLAM PUAN <span class="val" id="winScore">0</span></div>
            <div class="goStats">TOPLANAN ÇORAP <span class="val" id="winSocks">0</span></div>
            <div class="goStats">MAX HIZ <span class="val" id="winSpeed">0</span> km/h</div>
        </div>
        <div class="leaderboard" id="winLeaderboard"></div>
        <button onclick="restartGame()" class="wfBtn">TEKRAR OYNA</button>
    </div>
</div>

<div id="nameScreen">
    <div class="nameTitle">🏆 #1 YENİ REKOR!</div>
    <div class="nameSub">İSMİNİ GİR (3 HARF)</div>
    <div class="nameInput">
        <div class="nameChar active" id="nc0">A</div>
        <div class="nameChar" id="nc1">A</div>
        <div class="nameChar" id="nc2">A</div>
    </div>
    <div class="nameHint">↑↓ harf değiştir • ←→ karakter seç • ENTER onayla</div>
    <button id="nameConfirmBtn" onclick="confirmName()">ONAYLA</button>
</div>

<div id="stageCompleteScreen">
    <div class="scOverlay">
        <div style="color:rgba(255,255,255,.5);font-size:11px;letter-spacing:3px;margin-bottom:12px;font-family:'Orbitron',sans-serif">Brings you home, safely!</div>
        <div class="scCheck">✅</div>
        <div class="scTitle" id="scTitle">ETAP TAMAMLANDI!</div>
        <div class="scStage" id="scStage">İSTANBUL</div>
        <div class="scRow"><div class="scLabel">SÜRE</div><div class="scVal time" id="scTime">0s</div></div>
        <div class="scRow"><div class="scLabel">PUAN</div><div class="scVal score" id="scScore">0</div></div>
        <div class="scRow"><div class="scLabel">ÇORAP</div><div class="scVal socks" id="scSocks">0</div></div>
        <div class="scRow"><div class="scLabel">MAX HIZ</div><div class="scVal speed" id="scSpeed">0</div></div>
        <div class="scNext" id="scNextBox">
            <div class="scNextFlag" id="scNextFlag">🇫🇷</div>
            <div class="scNextInfo">
                <div class="scNextLabel">SONRAKİ ETAP</div>
                <div class="scNextName" id="scNextName">PARİS</div>
            </div>
        </div>
        <div class="scHint">Devam etmek için ENTER veya SPACE'e basın</div>
    </div>
</div>

<div class="touchHints" id="touchHints">
    <div class="touchArrow">◀</div>
    <div class="touchArrow">▶</div>
</div>

<div class="laneIndicator" id="laneIndicator">
    <div class="laneDot" id="lane0"></div>
    <div class="laneDot active" id="lane1"></div>
    <div class="laneDot" id="lane2"></div>
</div>

<canvas id="gameCanvas"></canvas>

<script>
const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');
function resize(){canvas.width=window.innerWidth;canvas.height=window.innerHeight}
resize();
window.addEventListener('resize',resize);

// ===== STAGES =====
const STAGES = [
    { name:'İSTANBUL', flag:'🇹🇷', subtitle:'Karlı Boğaz Yolu', length:1200, color1:'#1a2540', color2:'#2d3a55',
      roadColor:'#4a4f5b', snowColor:'#dfe6ed', treeColor:'#2d5a3f', sideType:'trees',
      sideColor1:'#c8d6e5', sideColor2:'#dfe6ed',
      waypoints:['Sultanahmet','Taksim','Beşiktaş','FSM Köprüsü','Kavacık','Beykoz','FİNİŞ'] },
    { name:'PARİS', flag:'🇫🇷', subtitle:'Champs-Élysées Rotası', length:1400, color1:'#1a2035', color2:'#252d42',
      roadColor:'#505565', snowColor:'#e0e5f0', treeColor:'#3a6a4f', sideType:'paris',
      sideColor1:'#b8c8b0', sideColor2:'#c8d8c0',
      waypoints:['Eyfel Kulesi','Arc de Triomphe','Louvre','Notre-Dame','Montmartre','Versailles','FİNİŞ'] },
    { name:'BERLİN', flag:'🇩🇪', subtitle:'Autobahn Kar Fırtınası', length:1600, color1:'#121a28', color2:'#1e2a3d',
      roadColor:'#555a65', snowColor:'#d0d8e2', treeColor:'#2a4a3a', sideType:'berlin',
      sideColor1:'#a8b5c0', sideColor2:'#bcc8d2',
      waypoints:['Brandenburg','Alexanderplatz','Potsdamer','Reichstag','Tiergarten','Tempelhof','FİNİŞ'] }
];

const MAX_SPEED = 5;        // Absolute max speed cap
const SLOWDOWN_INTERVAL = 2; // Slow down every 2 seconds without sock

const ROAD_WIDTH = 330;
const LANE_COUNT = 3;

function getLaneX(lane){
    return canvas.width/2 - ROAD_WIDTH/2 + (ROAD_WIDTH/LANE_COUNT)*lane + (ROAD_WIDTH/LANE_COUNT)/2;
}

// ===== GAME STATE =====
let G = {};
let car = {};
let keys={}, keyLock={};

function resetFullGame(){
    G = {
        running:false, finishing:false, finishTimer:0, stageTime:0,
        stageIndex:0, score:0, totalSocks:0, maxSpeedRecord:0,
        highScore:parseInt(localStorage.getItem('matteHS2')||'0'),
        // per-stage
        distance:0, socks:0, speed:3, baseSpeed:3, grip:10, slowdownTimer:0, lastSockTime:0,
        scrollY:0, lastTime:0, timerAccum:0,
        particles:[], snowflakes:[], obstacles:[], sockItems:[],
        carSlipX:0, slipTimer:0, slipForce:0, slipForceDur:0, slipForceDir:0,
        shakeX:0, shakeY:0, treesL:[], treesR:[], nextObs:500, nextSock:250,
        carTrail:[], speedDisplay:0
    };
    car = {x:0,y:0,width:38,height:68,currentLane:1,targetX:0};
}

function initStage(){
    let stage = STAGES[G.stageIndex];
    car.currentLane=1;
    car.x=getLaneX(1);
    car.y=canvas.height-150;
    car.targetX=car.x;

    G.distance=0; G.socks=0; G.speed=3; G.baseSpeed=3; G.grip=10; G.slowdownTimer=0; G.lastSockTime=0;
    G.finishing=false; G.finishTimer=0; G.stageTime=0;
    G.scrollY=0; G.timerAccum=0;
    G.particles=[]; G.obstacles=[]; G.sockItems=[];
    G.carSlipX=0; G.slipTimer=0; G.slipForce=0; G.slipForceDur=0;
    G.shakeX=0; G.shakeY=0; G.carTrail=[];
    G.nextObs=500; G.nextSock=250; G.speedDisplay=0;

    G.snowflakes=[];
    for(let i=0;i<100;i++) G.snowflakes.push({x:Math.random()*canvas.width,y:Math.random()*canvas.height,r:Math.random()*2.5+.5,speed:Math.random()*1.5+.8,wind:(Math.random()-.5)*.5,opacity:Math.random()*.35+.2});
    G.treesL=[]; G.treesR=[];
    for(let i=0;i<15;i++){
        G.treesL.push({y:i*100,x:-20-Math.random()*60,size:15+Math.random()*20});
        G.treesR.push({y:i*100,x:20+Math.random()*60,size:15+Math.random()*20});
    }
    updateLaneIndicator();
}

// ===== INPUT =====
window.addEventListener('keydown',e=>{
    e.preventDefault();

    // Name entry screen — intercept all keys
    if(nameState.active){
        handleNameInput(e.key);
        return;
    }

    // Enter or Space on menu screens
    if(e.key==='Enter'||e.key===' '){
        let vis=id=>getComputedStyle(document.getElementById(id)).display!=='none';
        if(vis('startScreen')){startGame();return}
        if(vis('stageCompleteScreen')){dismissStageComplete();return}
        if(vis('stageScreen')){continueStage();return}
        if(vis('gameOverScreen')){restartGame();return}
        if(vis('winFinalScreen')){restartGame();return}
        if(vis('winScreen')){restartGame();return}
    }

    if(!G.running)return;
    if(!keyLock[e.key]){
        keyLock[e.key]=true;
        if(e.key==='ArrowLeft'||e.key==='a'||e.key==='A'){if(car.currentLane>0){car.currentLane--;car.targetX=getLaneX(car.currentLane);updateLaneIndicator()}}
        if(e.key==='ArrowRight'||e.key==='d'||e.key==='D'){if(car.currentLane<2){car.currentLane++;car.targetX=getLaneX(car.currentLane);updateLaneIndicator()}}
    }
    keys[e.key]=true;
});
window.addEventListener('keyup',e=>{keys[e.key]=false;keyLock[e.key]=false});
window.addEventListener('dblclick',e=>e.preventDefault());

// ===== TOUCH / MOBILE CONTROLS =====
let touchStartX=0, touchStartY=0, touchActive=false;

window.addEventListener('touchstart',e=>{
    e.preventDefault();
    let t=e.touches[0];
    touchStartX=t.clientX;
    touchStartY=t.clientY;
    touchActive=true;

    // Tap on menu screens — use getComputedStyle for CSS-set displays
    if(!G.running && !G.finishing){
        let vis=id=>getComputedStyle(document.getElementById(id)).display!=='none';
        if(vis('startScreen')){startGame();return}
        if(vis('stageCompleteScreen')){dismissStageComplete();return}
        if(vis('stageScreen')){continueStage();return}
        if(vis('gameOverScreen')){restartGame();return}
        if(vis('winFinalScreen')){restartGame();return}
        if(vis('winScreen')){restartGame();return}
    }
},{passive:false});

window.addEventListener('touchmove',e=>{e.preventDefault()},{passive:false});

window.addEventListener('touchend',e=>{
    if(!touchActive)return;
    touchActive=false;
    let t=e.changedTouches[0];
    let dx=t.clientX-touchStartX;
    let dy=t.clientY-touchStartY;

    // Name entry screen — handle differently
    if(nameState.active){
        if(Math.abs(dy)>Math.abs(dx)&&Math.abs(dy)>20){
            handleNameInput(dy<0?'ArrowUp':'ArrowDown');
        } else if(Math.abs(dx)>20){
            handleNameInput(dx>0?'ArrowRight':'ArrowLeft');
        } else {
            confirmName();
        }
        return;
    }

    if(!G.running)return;

    // Swipe detection
    if(Math.abs(dx)>25){
        if(dx>0 && car.currentLane<2){
            car.currentLane++;car.targetX=getLaneX(car.currentLane);updateLaneIndicator();
        } else if(dx<0 && car.currentLane>0){
            car.currentLane--;car.targetX=getLaneX(car.currentLane);updateLaneIndicator();
        }
    }
},{passive:false});

// Also support simple left/right tap (no swipe needed)
canvas.addEventListener('touchstart',e=>{
    if(!G.running)return;
    let t=e.touches[0];
    let half=canvas.width/2;
    // Tap left half = go left, tap right half = go right
    if(t.clientX<half-30 && car.currentLane>0){
        car.currentLane--;car.targetX=getLaneX(car.currentLane);updateLaneIndicator();
    } else if(t.clientX>half+30 && car.currentLane<2){
        car.currentLane++;car.targetX=getLaneX(car.currentLane);updateLaneIndicator();
    }
},{passive:false});

function updateLaneIndicator(){
    for(let i=0;i<3;i++) document.getElementById('lane'+i).classList.toggle('active',i===car.currentLane);
}

// ===== DIFFICULTY — each stage harder =====
function getObsInterval(){
    let d=G.distance, si=G.stageIndex;
    // Stage 0 (Istanbul): easy, big gaps
    // Stage 1 (Paris): medium, tighter gaps
    // Stage 2 (Berlin): hard, very tight gaps
    let bases = [
        [650, 450, 320, 250],   // Istanbul — relaxed
        [450, 300, 220, 170],   // Paris — medium
        [350, 240, 170, 130]    // Berlin — intense
    ];
    let randoms = [
        [300, 200, 150, 120],
        [200, 150, 120, 100],
        [150, 120, 100, 80]
    ];
    let b=bases[si], r=randoms[si];
    let stLen=STAGES[si].length;
    let pct=d/stLen; // 0 to 1 progress
    if(pct<.2) return b[0]+Math.random()*r[0];
    if(pct<.5) return b[1]+Math.random()*r[1];
    if(pct<.8) return b[2]+Math.random()*r[2];
    return b[3]+Math.random()*r[3];
}

function getObsTypes(){
    let d=G.distance, si=G.stageIndex;
    let stLen=STAGES[si].length;
    let pct=d/stLen;

    // Stage 0 (Istanbul): starts easy, adds types gradually
    if(si===0){
        if(pct<.2)return['cone','pothole'];
        if(pct<.5)return['cone','pothole','rock'];
        return['cone','pothole','rock','barrier'];
    }
    // Stage 1 (Paris): starts with more types, adds ice
    if(si===1){
        if(pct<.2)return['cone','pothole','rock'];
        if(pct<.5)return['cone','pothole','rock','barrier'];
        return['cone','pothole','rock','barrier','ice'];
    }
    // Stage 2 (Berlin): all types from start, double obstacles possible
    if(pct<.2)return['cone','pothole','rock','barrier'];
    return['cone','pothole','rock','barrier','ice'];
}

// Berlin bonus: sometimes spawn 2 obstacles at once (different lanes)
function spawnObsBerlin(){
    spawnObs();
    if(G.stageIndex===2 && G.distance/STAGES[2].length > 0.4 && Math.random()<0.35){
        // Spawn a second obstacle in a different lane
        let usedLanes = G.obstacles.filter(o=>o.y<0).map(o=>o.lane);
        let freeLanes = [0,1,2].filter(l=>!usedLanes.includes(l));
        if(freeLanes.length>0){
            let lane=freeLanes[Math.floor(Math.random()*freeLanes.length)];
            if(!G.sockItems.some(s=>s.lane===lane&&s.y<200)){
                spawnObstacleInLane(lane);
            }
        }
    }
}

// ===== SPAWN =====
function spawnObstacleInLane(lane){
    let types=getObsTypes(),type=types[Math.floor(Math.random()*types.length)];
    let w,h;
    switch(type){
        case 'rock': w=35; h=30; break;
        case 'barrier': w=70; h=20; break;
        case 'ice': w=50; h=45; break;
        case 'pothole': w=48; h=38; break;
        case 'cone': w=20; h=25; break;
        default: w=35; h=30; type='rock';
    }
    G.obstacles.push({x:getLaneX(lane),y:-80,w,h,type,lane});
}

function spawnObs(){
    let lane=Math.floor(Math.random()*3);
    if(G.sockItems.some(s=>s.lane===lane&&s.y<200)){
        let alt=[0,1,2].filter(l=>l!==lane);
        lane=alt[Math.floor(Math.random()*alt.length)];
        if(G.sockItems.some(s=>s.lane===lane&&s.y<200))return;
    }
    spawnObstacleInLane(lane);
}

function spawnSock(){
    let lane=Math.floor(Math.random()*3);
    if(G.obstacles.some(o=>o.lane===lane&&o.y<150)){
        let alt=[0,1,2].filter(l=>l!==lane);
        lane=alt[Math.floor(Math.random()*alt.length)];
        if(G.obstacles.some(o=>o.lane===lane&&o.y<150)){
            let last=alt.find(l=>l!==lane);if(last!==undefined)lane=last;
        }
    }
    G.sockItems.push({x:getLaneX(lane),y:-60,size:44,pulse:0,glow:0,rotation:0,lane});
}

// ===== UPDATE =====
function update(dt){
    if(!G.running)return;
    let stage=STAGES[G.stageIndex];

    // ===== SLOWDOWN: every 2 sec without collecting sock, speed drops =====
    G.slowdownTimer += dt;
    if(G.slowdownTimer >= SLOWDOWN_INTERVAL){
        G.slowdownTimer = 0;
        G.baseSpeed = Math.max(2, G.baseSpeed - 0.3);
        G.speed = Math.max(2, G.speed - 0.4);
    }

    // Enforce absolute max speed cap
    G.speed = Math.min(G.speed, MAX_SPEED);
    G.baseSpeed = Math.min(G.baseSpeed, MAX_SPEED);

    // Auto-move forward at base speed minimum
    G.speed = Math.max(G.speed, G.baseSpeed);

    // Speed display (km/h feel)
    G.speedDisplay = G.speed * 18;
    G.maxSpeedRecord = Math.max(G.maxSpeedRecord, G.speedDisplay);

    // Distance
    G.distance += G.speed * dt * 8;
    G.scrollY += G.speed * 3;

    // Track stage elapsed time
    G.stageTime += dt;

    // Check stage complete — start finish animation
    if(G.distance >= stage.length && !G.finishing){
        G.finishing = true;
        G.running = false; // stop normal updates
        G.finishTimer = 0;
        G.score += Math.floor(G.distance);
        showStageComplete();
        return;
    }

    // Grip
    G.grip = Math.max(0, G.grip - dt*1.8);

    // Slip
    let gf=G.grip/100;
    let slipStr;
    if(gf<.15) slipStr=4;
    else if(gf<.35) slipStr=2.2*(1-gf);
    else if(gf<.6) slipStr=.7*(1-gf);
    else slipStr=.08*(1-gf);

    G.slipTimer+=dt;
    G.carSlipX = (Math.sin(G.slipTimer*2.5)*slipStr + Math.sin(G.slipTimer*5.7)*slipStr*.3 + Math.sin(G.slipTimer*1.1)*slipStr*.5) * G.speed*.2;

    if(G.slipForceDur>0){G.slipForceDur-=dt; G.carSlipX+=G.slipForce*G.slipForceDir; G.slipForce*=.96;}

    // Lane transition
    let laneSpd = 5 + gf*12;
    let diff=car.targetX-car.x;
    if(Math.abs(diff)>1) car.x+=diff*laneSpd*dt;
    else car.x=car.targetX;

    // Trail
    if(Math.random()<.3&&G.speed>1){
        G.carTrail.push({x:car.x-11,y:car.y+car.height/2,life:1});
        G.carTrail.push({x:car.x+11,y:car.y+car.height/2,life:1});
    }
    G.carTrail=G.carTrail.filter(t=>{t.life-=dt*.5;t.y+=G.speed*.5;return t.life>0});

    // Spawn
    // Stop spawning obstacles near finish (last 10%)
    let stageProgress = G.distance / STAGES[G.stageIndex].length;
    G.nextObs-=G.speed*3;
    if(G.nextObs<=0 && stageProgress < 0.90){spawnObsBerlin();G.nextObs=getObsInterval()}
    else if(G.nextObs<=0){G.nextObs=9999} // no more obstacles near finish
    G.nextSock-=G.speed*3;
    if(G.nextSock<=0){spawnSock();G.nextSock=380+Math.random()*420}

    // Update obstacles
    let cx=car.x+G.carSlipX;
    G.obstacles=G.obstacles.filter(o=>{
        o.y+=G.speed*3;
        // No collision near finish — let player cross safely
        if(stageProgress < 0.95){
            if(rectCol(cx-car.width/2+5,car.y-car.height/2+5,car.width-10,car.height-10,o.x-o.w/2,o.y-o.h/2,o.w,o.h)){
                G.shakeX=15;G.shakeY=15;
                gameOver();
                return false;
            }
        }
        return o.y<canvas.height+100;
    });

    // Update socks
    G.sockItems=G.sockItems.filter(s=>{
        s.y+=G.speed*3;s.pulse+=dt*4;s.glow=(Math.sin(s.pulse)+1)/2;s.rotation+=dt*1.5;
        if(Math.hypot(cx-s.x,car.y-s.y)<42){collectSock(s);return false}
        return s.y<canvas.height+100;
    });

    // Snow
    G.snowflakes.forEach(s=>{s.y+=s.speed+G.speed*2;s.x+=s.wind;if(s.y>canvas.height){s.y=-5;s.x=Math.random()*canvas.width}if(s.x<0)s.x=canvas.width;if(s.x>canvas.width)s.x=0});
    // Cap particles to prevent accumulation
    if(G.particles.length>80) G.particles=G.particles.slice(-80);
    G.particles=G.particles.filter(p=>{p.x+=p.vx;p.y+=p.vy+G.speed*1.5;p.life-=dt;p.size*=.98;return p.life>0});
    // Cap trail
    if(G.carTrail.length>40) G.carTrail=G.carTrail.slice(-40);
    // Cap obstacles that went offscreen
    if(G.obstacles.length>15) G.obstacles=G.obstacles.filter(o=>o.y<canvas.height+50);
    [...G.treesL,...G.treesR].forEach(t=>{t.y+=G.speed*3;if(t.y>canvas.height+50){t.y-=1500;t.size=15+Math.random()*20}});
    G.shakeX*=.9;G.shakeY*=.9;
}

function rectCol(x1,y1,w1,h1,x2,y2,w2,h2){return x1<x2+w2&&x1+w1>x2&&y1<y2+h2&&y1+h1>y2}

function collectSock(s){
    G.socks++; G.totalSocks++; G.score+=5;
    G.grip=Math.min(100,G.grip+30);
    // Speed boost — capped at MAX_SPEED
    G.baseSpeed=Math.min(MAX_SPEED,G.baseSpeed+.2);
    G.speed=Math.min(MAX_SPEED,G.speed+.25);
    G.slipForce=0;G.slipForceDur=0;
    // Reset slowdown timer — sock collected!
    G.slowdownTimer=0;

    for(let i=0;i<10;i++){
        let a=Math.random()*Math.PI*2,sp=2+Math.random()*4;
        G.particles.push({x:s.x,y:s.y,vx:Math.cos(a)*sp,vy:Math.sin(a)*sp,size:2+Math.random()*3,life:.4+Math.random()*.3,color:['#4fc3f7','#ffd740','#fff'][Math.floor(Math.random()*3)]});
    }
    showFloat(s.x,s.y,'+5','timeBonus');
    showFloat(s.x+30,s.y-10,'HIZLAN!','speedBonus');
    showFlash();
}

function showFloat(x,y,txt,cls){
    let el=document.createElement('div');el.className=cls;el.textContent=txt;el.style.left=x+'px';el.style.top=y+'px';document.body.appendChild(el);setTimeout(()=>el.remove(),1000);
}
function showFlash(){let el=document.createElement('div');el.className='collectFlash';document.body.appendChild(el);setTimeout(()=>el.remove(),300)}

// ===== DRAW =====
function draw(){
    ctx.save();
    ctx.translate(G.shakeX*(Math.random()-.5),G.shakeY*(Math.random()-.5));
    let stage=STAGES[G.stageIndex]||STAGES[0];

    // BG — subtle gradient (cached once per stage via 2 rects)
    ctx.fillStyle=stage.color1;
    ctx.fillRect(0,0,canvas.width,canvas.height/2);
    ctx.fillStyle=stage.color2;
    ctx.fillRect(0,canvas.height/2,canvas.width,canvas.height/2);

    let rcx=canvas.width/2, rl=rcx-ROAD_WIDTH/2, rr=rcx+ROAD_WIDTH/2;

    // Side ground with soft snow texture
    ctx.fillStyle=stage.sideColor2;
    ctx.fillRect(0,0,rl,canvas.height);
    ctx.fillRect(rr,0,canvas.width-rr,canvas.height);
    // Soft edge blend into road
    ctx.fillStyle=stage.sideColor1;
    ctx.fillRect(0,0,rl*0.4,canvas.height);
    ctx.fillRect(rr+(canvas.width-rr)*0.6,0,(canvas.width-rr)*0.4,canvas.height);

    let seed=Math.floor(G.scrollY/20);

    // Side decorations per stage
    if(stage.sideType==='trees'){
        G.treesL.forEach(t=>drawTree(rl+t.x,t.y,t.size,stage.treeColor));
        G.treesR.forEach(t=>drawTree(rr+t.x,t.y,t.size,stage.treeColor));
    } else if(stage.sideType==='paris'){
        G.treesL.forEach(t=>drawParisLamp(rl+t.x+10,t.y,t.size));
        G.treesR.forEach(t=>drawParisLamp(rr+t.x-10,t.y,t.size));
    } else if(stage.sideType==='berlin'){
        G.treesL.forEach(t=>drawBerlinPost(rl+t.x+10,t.y,t.size));
        G.treesR.forEach(t=>drawBerlinPost(rr+t.x-10,t.y,t.size));
    }

    // Road — two-tone for depth
    ctx.fillStyle=stage.roadColor;
    ctx.fillRect(rl,0,ROAD_WIDTH,canvas.height);
    // Road center lighter strip
    ctx.fillStyle='rgba(255,255,255,.03)';
    ctx.fillRect(rcx-20,0,40,canvas.height);

    // Snow/dirt accumulation on road shoulders
    ctx.fillStyle='rgba(200,210,220,.08)';
    ctx.fillRect(rl,0,18,canvas.height);
    ctx.fillRect(rr-18,0,18,canvas.height);

    // Road edges — snow bank with slight 3D
    ctx.fillStyle='#dce3ea';ctx.fillRect(rl-6,0,6,canvas.height);ctx.fillRect(rr,0,6,canvas.height);
    ctx.fillStyle='#c8d0d8';ctx.fillRect(rl-8,0,2,canvas.height);ctx.fillRect(rr+6,0,2,canvas.height);
    // Inner shadow on road edge
    ctx.fillStyle='rgba(0,0,0,.1)';ctx.fillRect(rl,0,2,canvas.height);ctx.fillRect(rr-2,0,2,canvas.height);

    // Lane lines
    ctx.strokeStyle='rgba(255,255,255,.35)';ctx.lineWidth=2;ctx.setLineDash([30,22]);
    for(let i=1;i<3;i++){let lx=rl+(ROAD_WIDTH/3)*i;ctx.beginPath();ctx.moveTo(lx,0);ctx.lineTo(lx,canvas.height);ctx.lineDashOffset=-G.scrollY*3;ctx.stroke()}
    ctx.setLineDash([]);

    // Finish line — uses scrollY-based positioning like all road objects
    // finishScrollMark = the scrollY value when car reaches finish
    // We place finish at a fixed "world Y" and let it scroll like obstacles
    let stLen=stage.length;
    let prog=G.distance/stLen;
    // Only render when close to finish (last 15%)
    if(prog > 0.85){
        // How many distance-units remain
        let remainDist = stLen - G.distance;
        // Scale: obstacles move G.speed*3 px per frame, distance grows G.speed*dt*8 per sec
        // At ~60fps, dt≈0.016, so per frame: dist grows G.speed*0.133, scroll moves G.speed*3
        // Ratio: 3/0.133 ≈ 22.5 screen-px per distance-unit
        let finishY = car.y - remainDist * 22.5;
        if(finishY > -80 && finishY < canvas.height + 50){
            let sq=15;
            for(let col=0;col<Math.ceil(ROAD_WIDTH/sq);col++){
                for(let row=0;row<3;row++){
                    ctx.fillStyle=(col+row)%2===0?'rgba(255,255,255,0.9)':'rgba(17,17,17,0.9)';
                    ctx.fillRect(rl+col*sq,finishY+row*sq,sq,sq);
                }
            }
            ctx.fillStyle='rgba(255,255,255,0.85)';
            ctx.font='bold 14px Orbitron';ctx.textAlign='center';
            ctx.fillText('FİNİŞ',rcx,finishY-10);

            // Paris: Eiffel Tower at finish
            if(G.stageIndex===1){
                drawEiffel(rcx, finishY-20);
            }
            // Berlin: Brandenburg Gate at finish
            if(G.stageIndex===2){
                drawBrandenburg(rcx, finishY-15);
            }
        }
    }

    // Trail
    G.carTrail.forEach(t=>{ctx.fillStyle=`rgba(60,65,75,${t.life*.3})`;ctx.fillRect(t.x-2,t.y,4,8);ctx.fillRect(t.x+20,t.y,4,8)});

    // Obstacles
    G.obstacles.forEach(o=>drawObstacle(o));

    // Socks
    G.sockItems.forEach(s=>drawTire(s));

    // Particles
    G.particles.forEach(p=>{ctx.globalAlpha=p.life;ctx.fillStyle=p.color;ctx.beginPath();ctx.arc(p.x,p.y,p.size,0,Math.PI*2);ctx.fill()});
    ctx.globalAlpha=1;

    // Car
    drawCar(car.x+G.carSlipX,car.y);

    // Snow
    G.snowflakes.forEach(s=>{ctx.fillStyle=`rgba(255,255,255,${s.opacity})`;ctx.beginPath();ctx.arc(s.x,s.y,s.r,0,Math.PI*2);ctx.fill()});

    // Atmosphere — subtle top/bottom/side darkening
    ctx.fillStyle='rgba(0,0,0,.12)';
    ctx.fillRect(0,0,canvas.width,20);
    ctx.fillStyle='rgba(0,0,0,.06)';
    ctx.fillRect(0,20,canvas.width,20);
    ctx.fillStyle='rgba(0,0,0,.08)';
    ctx.fillRect(0,canvas.height-20,canvas.width,20);
    // Side atmospheric fade
    ctx.fillStyle='rgba(0,0,0,.05)';
    ctx.fillRect(0,0,40,canvas.height);
    ctx.fillRect(canvas.width-40,0,40,canvas.height);

    // Low grip: just tint top bar red
    if(G.running&&G.grip<15){
        ctx.fillStyle='rgba(255,50,50,.08)';
        ctx.fillRect(0,0,canvas.width,4);
    }

    ctx.restore();

    // ===== HUD (drawn on canvas) =====
    if(G.running || G.finishing || G.distance > 0){
        drawHUD(stage);
        drawMiniMap(stage);
    }
}

function drawHUD(stage){
    let pad=18, y=20;

    // Left HUD
    let spdPct = Math.floor((G.speed / MAX_SPEED) * 100);
    let items=[
        {label:'PUAN',value:G.score,color:'#ffd740'},
        {label:'HIZ',value:Math.floor(G.speedDisplay)+' km/h',color: G.speed>=MAX_SPEED*0.95?'#ff4444':'#ff8a65'},
        {label:'ÇORAP',value:G.totalSocks,color:'#4fc3f7'},
        {label:'ETAP',value:(G.stageIndex+1)+'/3',color:'#69f0ae'}
    ];

    items.forEach((it,i)=>{
        let bx=pad, by=y+i*44;
        ctx.fillStyle='rgba(0,0,0,.5)';
        roundRect(bx,by,120,36,18);ctx.fill();
        ctx.fillStyle='rgba(255,255,255,.4)';ctx.font='8px Orbitron';ctx.textAlign='left';
        ctx.fillText(it.label,bx+12,by+13);
        ctx.fillStyle=it.color;ctx.font='bold 14px Orbitron';
        ctx.fillText(it.value,bx+12,by+28);
    });

    // Right top: logo
    // We'll draw text instead since image might not load
    ctx.fillStyle='rgba(0,0,0,.4)';
    roundRect(canvas.width-140,15,125,35,12);ctx.fill();
    ctx.fillStyle='#fff';ctx.font='bold 11px Orbitron';ctx.textAlign='center';
    ctx.fillText('MATTE',canvas.width-78,30);
    ctx.fillStyle='#8ab4f8';ctx.font='7px Orbitron';
    ctx.fillText('KAR ÇORABI',canvas.width-78,42);

    // Grip bar
    let gbx=canvas.width-140,gby=58;
    ctx.fillStyle='rgba(255,255,255,.3)';ctx.font='7px Orbitron';ctx.textAlign='center';
    ctx.fillText('YOL TUTUŞ',gbx+62,gby);
    ctx.fillStyle='rgba(0,0,0,.5)';
    roundRect(gbx,gby+4,125,8,4);ctx.fill();
    let gripW=Math.max(2,(G.grip/100)*121);
    ctx.fillStyle=G.grip<30?'#ff4444':(G.grip<60?'#ffd740':'#69f0ae');
    roundRect(gbx+2,gby+6,gripW,4,2);ctx.fill();

    // Progress bar bottom
    let prog=G.distance/STAGES[G.stageIndex].length;
    let pbW=200,pbH=6,pbX=canvas.width/2-pbW/2,pbY=canvas.height-15;
    ctx.fillStyle='rgba(0,0,0,.4)';
    roundRect(pbX,pbY,pbW,pbH,3);ctx.fill();
    ctx.fillStyle='#4fc3f7';
    roundRect(pbX,pbY,pbW*Math.min(1,prog),pbH,3);ctx.fill();
    ctx.fillStyle='rgba(255,255,255,.5)';ctx.font='7px Orbitron';ctx.textAlign='center';
    ctx.fillText(Math.floor(prog*100)+'%',canvas.width/2,pbY-3);
}

function drawMiniMap(stage){
    let mx=18, my=canvas.height/2-120, mw=36, mh=240;
    let wp=stage.waypoints;
    let prog=Math.min(1, G.distance/stage.length);

    // Map BG
    ctx.fillStyle='rgba(0,0,0,.55)';
    roundRect(mx-4,my-30,mw+8,mh+55,10);ctx.fill();

    // Stage label
    ctx.fillStyle='#8ab4f8';ctx.font='bold 7px Orbitron';ctx.textAlign='center';
    ctx.fillText(stage.flag+' ETAP '+(G.stageIndex+1),mx+mw/2,my-16);

    // Finish flag at TOP
    ctx.font='10px serif';ctx.textAlign='center';
    ctx.fillText('🏁',mx+mw/2,my-3);

    // Road line
    ctx.strokeStyle='rgba(255,255,255,.3)';ctx.lineWidth=3;
    ctx.beginPath();ctx.moveTo(mx+mw/2,my);ctx.lineTo(mx+mw/2,my+mh);ctx.stroke();

    // Road dashes
    ctx.strokeStyle='rgba(255,255,255,.15)';ctx.lineWidth=1;ctx.setLineDash([4,4]);
    ctx.beginPath();ctx.moveTo(mx+mw/2,my);ctx.lineTo(mx+mw/2,my+mh);ctx.stroke();
    ctx.setLineDash([]);

    // START label at bottom
    ctx.fillStyle='rgba(255,255,255,.4)';ctx.font='bold 6px Orbitron';ctx.textAlign='center';
    ctx.fillText('START',mx+mw/2,my+mh+14);

    // Waypoints — REVERSED: first waypoint at bottom, last (FİNİŞ) at top
    ctx.font='6px Orbitron';
    wp.forEach((name,i)=>{
        // i=0 is first waypoint (bottom), i=last is FİNİŞ (top)
        let wpProg = i/(wp.length-1); // 0 to 1
        let wy = my + mh - wpProg * mh; // bottom to top
        let passed = prog >= wpProg;
        ctx.fillStyle=passed?'#69f0ae':'rgba(255,255,255,.35)';
        ctx.beginPath();ctx.arc(mx+mw/2,wy,3,0,Math.PI*2);ctx.fill();
        ctx.fillStyle=passed?'rgba(255,255,255,.8)':'rgba(255,255,255,.3)';
        if(i%2===0){
            ctx.textAlign='left';
            ctx.fillText(name,mx+mw/2+8,wy+3);
        } else {
            ctx.textAlign='right';
            ctx.fillText(name,mx+mw/2-8,wy+3);
        }
    });

    // Car position — goes from BOTTOM to TOP
    let carMapY = my + mh - prog * mh;

    // Car icon (triangle pointing UP)
    ctx.fillStyle='#4fc3f7';
    ctx.shadowColor='#4fc3f7';ctx.shadowBlur=8;
    ctx.beginPath();
    ctx.moveTo(mx+mw/2, carMapY-6);
    ctx.lineTo(mx+mw/2-5, carMapY+4);
    ctx.lineTo(mx+mw/2+5, carMapY+4);
    ctx.closePath();ctx.fill();
    ctx.shadowBlur=0;

    // Pulsing ring around car
    let pulse=Math.sin(Date.now()*.005)*.5+.5;
    ctx.strokeStyle=`rgba(79,195,247,${.3+pulse*.4})`;
    ctx.lineWidth=1.5;
    ctx.beginPath();ctx.arc(mx+mw/2,carMapY,8+pulse*3,0,Math.PI*2);ctx.stroke();

    // Speed indicator near map
    ctx.fillStyle='rgba(255,255,255,.4)';ctx.font='6px Orbitron';ctx.textAlign='center';
    ctx.fillText(Math.floor(G.speedDisplay)+' km/h',mx+mw/2,my+mh+24);
}

function roundRect(x,y,w,h,r){
    ctx.beginPath();ctx.moveTo(x+r,y);ctx.lineTo(x+w-r,y);ctx.quadraticCurveTo(x+w,y,x+w,y+r);
    ctx.lineTo(x+w,y+h-r);ctx.quadraticCurveTo(x+w,y+h,x+w-r,y+h);
    ctx.lineTo(x+r,y+h);ctx.quadraticCurveTo(x,y+h,x,y+h-r);
    ctx.lineTo(x,y+r);ctx.quadraticCurveTo(x,y,x+r,y);ctx.closePath();
}

function pseudoR(s){let x=Math.sin(s*12.9898+78.233)*43758.5453;return x-Math.floor(x)}

function drawTree(x,y,sz,col){
    ctx.fillStyle=col;
    ctx.beginPath();ctx.moveTo(x,y-sz*1.5);ctx.lineTo(x-sz*.7,y);ctx.lineTo(x+sz*.7,y);ctx.closePath();ctx.fill();
    ctx.fillStyle='rgba(220,230,240,.7)';
    ctx.beginPath();ctx.moveTo(x,y-sz*1.8);ctx.lineTo(x-sz*.3,y-sz*1.1);ctx.lineTo(x+sz*.3,y-sz*1.1);ctx.closePath();ctx.fill();
    ctx.fillStyle='#5a3d2b';ctx.fillRect(x-3,y,6,sz*.3);
}

// Paris: trimmed trees + warm lampposts (classic Parisian boulevard)
function drawParisLamp(x,y,sz){
    // Round trimmed tree (platane)
    ctx.fillStyle='#3a6848';
    ctx.beginPath();ctx.arc(x,y-sz*1.1,sz*.5,0,Math.PI*2);ctx.fill();
    // Snow dusting on top
    ctx.fillStyle='rgba(230,240,250,.4)';
    ctx.beginPath();ctx.arc(x,y-sz*1.3,sz*.35,Math.PI,0,false);ctx.fill();
    // Trunk
    ctx.fillStyle='#6a5040';
    ctx.fillRect(x-2,y-sz*.6,4,sz*.6);
    // Lamppost next to tree
    ctx.fillStyle='#4a4a4a';
    ctx.fillRect(x+sz*.5-1,y-sz*1.4,2,sz*1.4);
    // Lamp glow
    ctx.fillStyle='rgba(255,210,100,.35)';
    ctx.beginPath();ctx.arc(x+sz*.5,y-sz*1.45,3.5,0,Math.PI*2);ctx.fill();
    ctx.fillStyle='rgba(255,210,100,.08)';
    ctx.beginPath();ctx.arc(x+sz*.5,y-sz*1.45,12,0,Math.PI*2);ctx.fill();
}

// Berlin: pine trees + concrete barriers (autobahn style)
function drawBerlinPost(x,y,sz){
    // Small pine tree
    ctx.fillStyle='#2a4a38';
    ctx.beginPath();ctx.moveTo(x,y-sz*1.4);ctx.lineTo(x-sz*.4,y-sz*.3);ctx.lineTo(x+sz*.4,y-sz*.3);ctx.closePath();ctx.fill();
    ctx.fillStyle='#2f5240';
    ctx.beginPath();ctx.moveTo(x,y-sz*1.7);ctx.lineTo(x-sz*.25,y-sz*.8);ctx.lineTo(x+sz*.25,y-sz*.8);ctx.closePath();ctx.fill();
    // Snow cap
    ctx.fillStyle='rgba(220,230,240,.5)';
    ctx.beginPath();ctx.moveTo(x,y-sz*1.7);ctx.lineTo(x-sz*.15,y-sz*1.2);ctx.lineTo(x+sz*.15,y-sz*1.2);ctx.closePath();ctx.fill();
    // Trunk
    ctx.fillStyle='#4a3828';
    ctx.fillRect(x-2,y-sz*.3,4,sz*.3);
    // Concrete road barrier
    ctx.fillStyle='rgba(160,170,180,.3)';
    ctx.fillRect(x+sz*.4,y-sz*.15,sz*.5,sz*.15);
}

function drawEiffel(x,y){
    ctx.save();ctx.translate(x,y);
    // Tower body — lattice iron
    ctx.strokeStyle='#5a4a3a';ctx.lineWidth=2;
    // Left leg
    ctx.beginPath();ctx.moveTo(-20,0);ctx.lineTo(-8,-50);ctx.lineTo(-5,-80);ctx.lineTo(-2,-110);ctx.stroke();
    // Right leg
    ctx.beginPath();ctx.moveTo(20,0);ctx.lineTo(8,-50);ctx.lineTo(5,-80);ctx.lineTo(2,-110);ctx.stroke();
    // Cross beams
    ctx.lineWidth=1.5;
    ctx.beginPath();ctx.moveTo(-14,-30);ctx.lineTo(14,-30);ctx.stroke();
    ctx.beginPath();ctx.moveTo(-8,-55);ctx.lineTo(8,-55);ctx.stroke();
    ctx.beginPath();ctx.moveTo(-4,-85);ctx.lineTo(4,-85);ctx.stroke();
    // Top spire
    ctx.lineWidth=1.5;
    ctx.beginPath();ctx.moveTo(0,-110);ctx.lineTo(0,-130);ctx.stroke();
    // Platform
    ctx.fillStyle='#5a4a3a';
    ctx.fillRect(-10,-52,20,4);
    ctx.fillRect(-5,-82,10,3);
    // Snow
    ctx.fillStyle='rgba(230,240,250,.5)';
    ctx.fillRect(-11,-53,22,2);
    ctx.fillRect(-6,-83,12,2);
    // Glow at top
    ctx.fillStyle='rgba(255,220,100,.5)';
    ctx.beginPath();ctx.arc(0,-130,3,0,Math.PI*2);ctx.fill();
    ctx.restore();
}

function drawBrandenburg(x,y){
    ctx.save();ctx.translate(x,y);
    // Columns
    ctx.fillStyle='#8a8070';
    for(let i=0;i<5;i++){
        let cx=-24+i*12;
        ctx.fillRect(cx-2,0,4,-50);
    }
    // Top beam
    ctx.fillStyle='#7a7060';
    ctx.fillRect(-28,-50,56,8);
    // Pediment
    ctx.beginPath();ctx.moveTo(-28,-50);ctx.lineTo(0,-68);ctx.lineTo(28,-50);ctx.closePath();ctx.fill();
    // Snow
    ctx.fillStyle='rgba(225,235,245,.5)';
    ctx.beginPath();ctx.moveTo(-26,-50);ctx.lineTo(0,-66);ctx.lineTo(26,-50);ctx.closePath();ctx.fill();
    // Quadriga on top
    ctx.fillStyle='#bfa850';
    ctx.fillRect(-5,-70,10,5);
    ctx.fillStyle='rgba(255,220,100,.4)';
    ctx.beginPath();ctx.arc(0,-72,4,0,Math.PI*2);ctx.fill();
    ctx.restore();
}

function drawObstacle(o){
    ctx.save();ctx.translate(o.x,o.y);
    switch(o.type){
        case'rock':
            ctx.fillStyle='#6b7280';ctx.beginPath();ctx.ellipse(0,0,o.w/2,o.h/2,0,0,Math.PI*2);ctx.fill();
            ctx.fillStyle='#9ca3af';ctx.beginPath();ctx.ellipse(-3,-3,o.w/3,o.h/3,.3,0,Math.PI*2);ctx.fill();
            ctx.fillStyle='rgba(220,230,240,.6)';ctx.beginPath();ctx.ellipse(0,-o.h/4,o.w/3,o.h/5,0,0,Math.PI);ctx.fill();break;
        case'barrier':
            ctx.fillStyle='#ef4444';ctx.fillRect(-o.w/2,-o.h/2,o.w,o.h);
            ctx.fillStyle='#fff';for(let i=0;i<4;i++)ctx.fillRect(-o.w/2+i*20+2,-o.h/2,7,o.h);break;
        case'ice':
            ctx.fillStyle='rgba(150,210,240,.5)';ctx.beginPath();ctx.ellipse(0,0,o.w/2,o.h/2,0,0,Math.PI*2);ctx.fill();
            ctx.strokeStyle='rgba(180,230,255,.6)';ctx.lineWidth=2;ctx.stroke();break;
        case'pothole':
            ctx.fillStyle='#2a2a2a';ctx.beginPath();ctx.ellipse(0,0,o.w/2+3,o.h/2+3,0,0,Math.PI*2);ctx.fill();
            ctx.fillStyle='#111';ctx.beginPath();ctx.ellipse(0,0,o.w/2,o.h/2,0,0,Math.PI*2);ctx.fill();
            ctx.fillStyle='#1a1a1a';ctx.beginPath();ctx.ellipse(0,0,o.w/3,o.h/3,0,0,Math.PI*2);ctx.fill();
            ctx.strokeStyle='#333';ctx.lineWidth=1.5;
            for(let i=0;i<6;i++){let a=(i/6)*Math.PI*2+.3,l=o.w/2+5+Math.random()*6;ctx.beginPath();ctx.moveTo(Math.cos(a)*o.w*.3,Math.sin(a)*o.h*.3);ctx.lineTo(Math.cos(a)*l,Math.sin(a)*l*.8);ctx.stroke()}
            ctx.strokeStyle='rgba(255,200,50,.25)';ctx.lineWidth=1;ctx.setLineDash([3,3]);
            ctx.beginPath();ctx.ellipse(0,0,o.w/2+6,o.h/2+6,0,0,Math.PI*2);ctx.stroke();ctx.setLineDash([]);break;
        case'cone':
            ctx.fillStyle='#f97316';ctx.beginPath();ctx.moveTo(0,-o.h/2);ctx.lineTo(-o.w/2,o.h/2);ctx.lineTo(o.w/2,o.h/2);ctx.closePath();ctx.fill();
            ctx.fillStyle='#fff';ctx.fillRect(-o.w/3,-2,o.w/1.5,4);break;
    }
    if(o.type!=='pothole'){ctx.fillStyle='rgba(0,0,0,.15)';ctx.beginPath();ctx.ellipse(4,o.h/2+3,o.w/2.5,5,0,0,Math.PI*2);ctx.fill()}
    ctx.restore();
}

function drawTire(s){
    ctx.save();ctx.translate(s.x,s.y);
    let ps=1+Math.sin(s.pulse)*.05;ctx.scale(ps,ps);
    let R=s.size/2;

    // Soft pickup glow
    ctx.fillStyle='rgba(200,60,40,.08)';
    ctx.beginPath();ctx.arc(0,0,R+14,0,Math.PI*2);ctx.fill();
    ctx.strokeStyle=`rgba(220,80,60,${.15+s.glow*.2})`;ctx.lineWidth=1;
    ctx.beginPath();ctx.arc(0,0,R+10+s.glow*4,0,Math.PI*2);ctx.stroke();

    ctx.save();ctx.rotate(s.rotation);

    // Ground shadow
    ctx.fillStyle='rgba(0,0,0,.1)';
    ctx.beginPath();ctx.ellipse(2,3,R,R*.85,0,0,Math.PI*2);ctx.fill();

    // === TIRE (black rubber edge visible around sock) ===
    ctx.fillStyle='#222';
    ctx.beginPath();ctx.arc(0,0,R,0,Math.PI*2);ctx.fill();
    // Tire sidewall — slightly lighter ring
    ctx.strokeStyle='#333';ctx.lineWidth=1.5;
    ctx.beginPath();ctx.arc(0,0,R-1.5,0,Math.PI*2);ctx.stroke();

    // === RED TEXTILE SOCK COVER (covers tread area) ===
    let sockOuter=R-3, sockInner=R*.22;

    // Main red textile body
    ctx.fillStyle='#c0392b';
    ctx.beginPath();ctx.arc(0,0,sockOuter,0,Math.PI*2);ctx.fill();

    // Textile weave texture — very subtle diagonal cross-hatch
    ctx.strokeStyle='rgba(160,30,20,.35)';ctx.lineWidth=.7;
    for(let i=0;i<8;i++){
        let a=(i/8)*Math.PI*2;
        let x1=Math.cos(a)*sockInner*1.5, y1=Math.sin(a)*sockInner*1.5;
        let x2=Math.cos(a)*sockOuter*.95, y2=Math.sin(a)*sockOuter*.95;
        ctx.beginPath();ctx.moveTo(x1,y1);ctx.lineTo(x2,y2);ctx.stroke();
    }
    // Concentric weave lines
    ctx.strokeStyle='rgba(170,35,25,.2)';ctx.lineWidth=.5;
    for(let r=sockInner+6;r<sockOuter-2;r+=6){
        ctx.beginPath();ctx.arc(0,0,r,0,Math.PI*2);ctx.stroke();
    }

    // Elastic edge band (darker red border where sock grips tire)
    ctx.strokeStyle='#8a1e15';ctx.lineWidth=2.5;
    ctx.beginPath();ctx.arc(0,0,sockOuter-1,0,Math.PI*2);ctx.stroke();
    // Inner elastic band around hub hole
    ctx.strokeStyle='#8a1e15';ctx.lineWidth=2;
    ctx.beginPath();ctx.arc(0,0,sockInner+1,0,Math.PI*2);ctx.stroke();

    // Fabric highlight — top-left light reflection on textile
    ctx.fillStyle='rgba(255,140,120,.12)';
    ctx.beginPath();ctx.arc(-R*.2,-R*.2,R*.35,0,Math.PI*2);ctx.fill();

    // "MATTE" branding strip across center
    ctx.fillStyle='rgba(255,255,255,.75)';
    ctx.font='bold 7px Arial';ctx.textAlign='center';ctx.textBaseline='middle';
    let brandY=0;
    // White text band background
    ctx.fillStyle='rgba(255,255,255,.12)';
    ctx.fillRect(-sockOuter*.7,brandY-5,sockOuter*1.4,10);
    ctx.fillStyle='rgba(255,255,255,.8)';
    ctx.fillText('MATTE',0,brandY);

    // Hub hole (shows dark wheel center through sock opening)
    ctx.fillStyle='#1a1a1a';
    ctx.beginPath();ctx.arc(0,0,sockInner,0,Math.PI*2);ctx.fill();
    // Lug nut hints
    ctx.fillStyle='#333';
    for(let i=0;i<4;i++){
        let a=(i/4)*Math.PI*2;
        ctx.beginPath();ctx.arc(Math.cos(a)*sockInner*.55,Math.sin(a)*sockInner*.55,1.5,0,Math.PI*2);ctx.fill();
    }

    ctx.restore(); // rotation

    // Floating arrow indicator
    let ay=-R-12-Math.sin(s.pulse*2)*3;
    ctx.fillStyle=`rgba(220,70,50,${.4+s.glow*.4})`;
    ctx.beginPath();ctx.moveTo(0,ay+7);ctx.lineTo(-5,ay);ctx.lineTo(5,ay);ctx.closePath();ctx.fill();

    ctx.restore();
}

function drawCar(x,y){
    ctx.save();ctx.translate(x,y);
    let diff=car.targetX-car.x,rot=Math.max(-.15,Math.min(.15,diff*.003));ctx.rotate(rot);

    // Soft car shadow
    ctx.fillStyle='rgba(0,0,0,.12)';ctx.beginPath();ctx.ellipse(4,6,car.width/2+6,car.height/2+5,0,0,Math.PI*2);ctx.fill();
    ctx.fillStyle='rgba(0,0,0,.18)';ctx.beginPath();ctx.ellipse(3,4,car.width/2+2,car.height/2+2,0,0,Math.PI*2);ctx.fill();

    // Car body — Saks blue with two-tone depth
    ctx.fillStyle='#4a8ab8';

    ctx.beginPath();
    ctx.moveTo(-car.width/2,car.height/2-8);ctx.lineTo(-car.width/2,-car.height/2+12);
    ctx.quadraticCurveTo(-car.width/2,-car.height/2,-car.width/2+8,-car.height/2);
    ctx.lineTo(car.width/2-8,-car.height/2);ctx.quadraticCurveTo(car.width/2,-car.height/2,car.width/2,-car.height/2+12);
    ctx.lineTo(car.width/2,car.height/2-8);ctx.quadraticCurveTo(car.width/2,car.height/2,car.width/2-8,car.height/2);
    ctx.lineTo(-car.width/2+8,car.height/2);ctx.quadraticCurveTo(-car.width/2,car.height/2,-car.width/2,car.height/2-8);
    ctx.closePath();ctx.fill();

    // Body highlight strip (left side lighter = light reflection)
    ctx.fillStyle='rgba(130,190,230,.2)';
    ctx.fillRect(-car.width/2+2,-car.height/2+10,car.width/3,car.height-20);
    // Body darker right side
    ctx.fillStyle='rgba(0,30,60,.12)';
    ctx.fillRect(car.width/6,-car.height/2+10,car.width/3,car.height-20);

    // Windshield
    ctx.fillStyle='rgba(160,210,250,.45)';
    ctx.beginPath();ctx.moveTo(-car.width/2+5,-car.height/2+14);ctx.lineTo(car.width/2-5,-car.height/2+14);ctx.lineTo(car.width/2-7,-car.height/2+26);ctx.lineTo(-car.width/2+7,-car.height/2+26);ctx.closePath();ctx.fill();
    // Rear window
    ctx.fillStyle='rgba(160,210,250,.3)';
    ctx.beginPath();ctx.moveTo(-car.width/2+6,car.height/2-22);ctx.lineTo(car.width/2-6,car.height/2-22);ctx.lineTo(car.width/2-5,car.height/2-13);ctx.lineTo(-car.width/2+5,car.height/2-13);ctx.closePath();ctx.fill();
    // Roof panel
    ctx.fillStyle='rgba(80,160,210,.2)';ctx.fillRect(-car.width/2+6,-5,car.width-12,14);

    ctx.fillStyle='#fef9c3';ctx.shadowColor='#fef08a';ctx.shadowBlur=12;
    ctx.fillRect(-car.width/2+2,-car.height/2,8,5);ctx.fillRect(car.width/2-10,-car.height/2,8,5);ctx.shadowBlur=0;

    ctx.fillStyle='#ef4444';ctx.shadowColor='#ef4444';ctx.shadowBlur=8;
    ctx.fillRect(-car.width/2+2,car.height/2-5,8,4);ctx.fillRect(car.width/2-10,car.height/2-5,8,4);ctx.shadowBlur=0;

    ctx.fillStyle='#1f2937';
    ctx.fillRect(-car.width/2-3,-car.height/2+8,6,14);ctx.fillRect(car.width/2-3,-car.height/2+8,6,14);
    ctx.fillRect(-car.width/2-3,car.height/2-22,6,14);ctx.fillRect(car.width/2-3,car.height/2-22,6,14);

    if(G.grip>50){
        ctx.strokeStyle='#ffd740';ctx.lineWidth=1.5;let co=(G.scrollY*3)%6;
        [-1,1].forEach(s=>{let wx=s*(car.width/2);
            for(let i=0;i<3;i++){let c1=-car.height/2+10+i*4+co%4;ctx.beginPath();ctx.moveTo(wx-3,c1);ctx.lineTo(wx+3,c1);ctx.stroke();
            let c2=car.height/2-20+i*4+co%4;ctx.beginPath();ctx.moveTo(wx-3,c2);ctx.lineTo(wx+3,c2);ctx.stroke()}
        });
    }

    if(G.running||G.finishing){
        ctx.fillStyle='rgba(254,249,195,.06)';
        ctx.beginPath();ctx.moveTo(-car.width/2+2,-car.height/2);ctx.lineTo(car.width/2-2,-car.height/2);ctx.lineTo(car.width/2+15,-car.height/2-60);ctx.lineTo(-car.width/2-15,-car.height/2-60);ctx.closePath();ctx.fill();
    }
    ctx.restore();

    if((G.running||G.finishing)&&G.speed>2){
        for(let i=0;i<2;i++)G.particles.push({x:x+(Math.random()-.5)*car.width,y:y+car.height/2+Math.random()*5,vx:(Math.random()-.5)*2,vy:Math.random()*2,size:2+Math.random()*3,life:.4+Math.random()*.3,color:'rgba(200,215,230,.6)'});
    }
}

// ===== LEADERBOARD =====
function getLeaderboard(){
    try{ return JSON.parse(localStorage.getItem('matteLeaderboard')||'[]'); }catch(e){ return []; }
}

function saveLeaderboard(lb){
    localStorage.setItem('matteLeaderboard',JSON.stringify(lb));
}

function addScore(name, score){
    let lb=getLeaderboard();
    lb.push({name:name, score:score});
    lb.sort((a,b)=>b.score-a.score);
    lb=lb.slice(0,5); // keep top 5
    saveLeaderboard(lb);
    return lb;
}

function getScoreRank(score){
    let lb=getLeaderboard();
    if(lb.length<5) return lb.length===0?1:lb.filter(e=>e.score>=score).length+1;
    for(let i=0;i<lb.length;i++){if(score>lb[i].score) return i+1;}
    return lb.length<5?lb.length+1:-1; // -1 means not in top 5
}

function isNewHighScore(score){
    return getScoreRank(score)===1;
}

function isInTop5(score){
    let rank=getScoreRank(score);
    return rank>=1&&rank<=5;
}

function renderLeaderboard(containerId, currentScore){
    let lb=getLeaderboard();
    let el=document.getElementById(containerId);
    if(lb.length===0){el.innerHTML='';return;}
    let html='<div class="lbTitle">🏆 EN İYİ 5 SKOR</div>';
    let medals=['🥇','🥈','🥉','4.','5.'];
    lb.forEach((entry,i)=>{
        let isThis=entry.score===currentScore&&entry._new;
        let cls=i===0?'lbRow gold':'lbRow normal';
        html+=`<div class="${cls}"><span class="lbRank">${medals[i]}</span><span class="lbName">${entry.name}</span><span class="lbScore">${entry.score}</span>${isThis?'<span class="lbNew">YENİ!</span>':''}</div>`;
    });
    el.innerHTML=html;
}

// ===== NAME ENTRY =====
let nameState={active:false, chars:['A','A','A'], pos:0, pendingScreen:''};

function showNameEntry(afterScreen){
    nameState={active:true, chars:['A','A','A'], pos:0, pendingScreen:afterScreen};
    for(let i=0;i<3;i++){
        document.getElementById('nc'+i).textContent='A';
        document.getElementById('nc'+i).classList.toggle('active',i===0);
    }
    document.getElementById('nameScreen').style.display='flex';
}

function updateNameDisplay(){
    for(let i=0;i<3;i++){
        document.getElementById('nc'+i).textContent=nameState.chars[i];
        document.getElementById('nc'+i).classList.toggle('active',i===nameState.pos);
    }
}

function handleNameInput(key){
    if(!nameState.active)return false;
    let letters='ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789';
    if(key==='ArrowUp'||key==='w'||key==='W'){
        let idx=letters.indexOf(nameState.chars[nameState.pos]);
        idx=(idx+1)%letters.length;
        nameState.chars[nameState.pos]=letters[idx];
        updateNameDisplay();return true;
    }
    if(key==='ArrowDown'||key==='s'||key==='S'){
        let idx=letters.indexOf(nameState.chars[nameState.pos]);
        idx=(idx-1+letters.length)%letters.length;
        nameState.chars[nameState.pos]=letters[idx];
        updateNameDisplay();return true;
    }
    if(key==='ArrowRight'||key==='d'||key==='D'){
        nameState.pos=Math.min(2,nameState.pos+1);
        updateNameDisplay();return true;
    }
    if(key==='ArrowLeft'||key==='a'||key==='A'){
        nameState.pos=Math.max(0,nameState.pos-1);
        updateNameDisplay();return true;
    }
    if(key==='Enter'||key===' '){
        confirmName();return true;
    }
    // Direct letter typing
    let upper=key.toUpperCase();
    if(letters.includes(upper)){
        nameState.chars[nameState.pos]=upper;
        if(nameState.pos<2)nameState.pos++;
        updateNameDisplay();return true;
    }
    return false;
}

function confirmName(){
    if(!nameState.active)return;
    nameState.active=false;
    let name=nameState.chars.join('');
    document.getElementById('nameScreen').style.display='none';

    // Save with _new flag for highlighting
    let lb=getLeaderboard();
    lb.push({name:name,score:G.score,_new:true});
    lb.sort((a,b)=>b.score-a.score);
    lb=lb.slice(0,5);
    // Clear old _new flags
    lb.forEach((e,i)=>{if(i>0||e.score!==G.score)delete e._new});
    saveLeaderboard(lb);

    // Show the pending screen
    if(nameState.pendingScreen==='gameover'){
        showGameOverFinal();
    } else {
        showWinFinal();
    }
}

// ===== STAGE COMPLETE OVERLAY =====
function showStageComplete(){
    let s=STAGES[G.stageIndex];
    let timeStr=G.stageTime.toFixed(1)+'s';
    document.getElementById('scTitle').textContent='ETAP '+(G.stageIndex+1)+' TAMAMLANDI!';
    document.getElementById('scStage').textContent=s.flag+' '+s.name;
    document.getElementById('scTime').textContent=timeStr;
    document.getElementById('scScore').textContent=G.score;
    document.getElementById('scSocks').textContent=G.socks;
    document.getElementById('scSpeed').textContent=Math.floor(G.maxSpeedRecord)+' km/h';

    let nextBox=document.getElementById('scNextBox');
    if(G.stageIndex < STAGES.length-1){
        let next=STAGES[G.stageIndex+1];
        nextBox.style.display='flex';
        document.getElementById('scNextFlag').textContent=next.flag;
        document.getElementById('scNextName').textContent=next.name;
    } else {
        nextBox.style.display='none';
    }

    document.getElementById('stageCompleteScreen').style.display='flex';
}

function dismissStageComplete(){
    document.getElementById('stageCompleteScreen').style.display='none';
    G.finishing=false;

    if(G.stageIndex < STAGES.length-1){
        G.stageIndex++;
        showStageScreen();
    } else {
        showWinScreen();
    }
}

// ===== SCREENS =====
function showStageScreen(){
    let s=STAGES[G.stageIndex];
    let si=G.stageIndex;
    document.getElementById('stageLabel').textContent='ETAP '+(si+1);
    document.getElementById('stageFlag').textContent=s.flag;
    document.getElementById('stageName').textContent=s.name;
    document.getElementById('stageInfo').textContent=s.subtitle;
    document.getElementById('stageScoreText').textContent=G.score>0?'TOPLAM PUAN: '+G.score:'';

    // Highlight current stage in the roadmap
    let dots=document.getElementById('stageScreen').querySelectorAll('[data-stage]');
    // Use JS to style the inline roadmap
    let roadmap=document.getElementById('stageScreen').querySelector('[style*="display:flex;gap:18px"]');
    if(roadmap){
        let items=roadmap.children;
        for(let i=0;i<items.length;i++){
            if(items[i].dataset && items[i].dataset.si!==undefined){
                // skip arrows
            }
        }
    }
    // Simpler: just update highlight text
    let names=['🇹🇷 İstanbul','🇫🇷 Paris','🇩🇪 Berlin'];
    document.getElementById('stageHighlight').textContent='▶ ŞU AN: '+names[si];

    document.getElementById('stageScreen').style.display='flex';
    document.getElementById('laneIndicator').style.display='none';
}

function continueStage(){
    document.getElementById('stageScreen').style.display='none';
    document.getElementById('laneIndicator').style.display='flex';
    initStage();
    G.running=true;G.lastTime=0;
}

function showWinScreen(){
    // score already added in stage complete check
    if(isNewHighScore(G.score)){
        showNameEntry('win');
    } else {
        if(isInTop5(G.score)){
            addScore('---',G.score);
        }
        showWinFinal();
    }
}

function showWinFinal(){
    document.getElementById('winScore').textContent=G.score;
    document.getElementById('winSocks').textContent=G.totalSocks;
    document.getElementById('winSpeed').textContent=Math.floor(G.maxSpeedRecord);
    renderLeaderboard('winLeaderboard',G.score);
    document.getElementById('winFinalScreen').style.display='flex';
    document.getElementById('laneIndicator').style.display='none';
    startConfetti();
}

// ===== CONFETTI =====
let confettiParts=[], confettiRunning=false;
function startConfetti(){
    let cc=document.getElementById('confettiCanvas');
    cc.width=window.innerWidth; cc.height=window.innerHeight;
    let cx=cc.getContext('2d');
    confettiParts=[];
    let colors=['#e74c3c','#f39c12','#ffd740','#2ecc71','#3498db','#9b59b6','#e91e63','#00bcd4','#ff5722','#4caf50'];
    for(let i=0;i<120;i++){
        confettiParts.push({
            x:Math.random()*cc.width,
            y:Math.random()*cc.height*-1 - 20,
            w:4+Math.random()*6,
            h:8+Math.random()*8,
            color:colors[Math.floor(Math.random()*colors.length)],
            vy:1.5+Math.random()*3,
            vx:(Math.random()-.5)*2,
            rot:Math.random()*Math.PI*2,
            rv:(Math.random()-.5)*.15,
            life:1
        });
    }
    confettiRunning=true;
    function animConfetti(){
        if(!confettiRunning)return;
        cx.clearRect(0,0,cc.width,cc.height);
        let alive=false;
        confettiParts.forEach(p=>{
            p.y+=p.vy;p.x+=p.vx;p.rot+=p.rv;
            p.vx+=(Math.random()-.5)*.1;
            if(p.y>cc.height+20){p.life=0;}
            if(p.life<=0)return;
            alive=true;
            cx.save();
            cx.translate(p.x,p.y);
            cx.rotate(p.rot);
            cx.fillStyle=p.color;
            cx.fillRect(-p.w/2,-p.h/2,p.w,p.h);
            cx.restore();
        });
        if(alive) requestAnimationFrame(animConfetti);
        else confettiRunning=false;
    }
    animConfetti();
}

function stopConfetti(){
    confettiRunning=false;
    let cc=document.getElementById('confettiCanvas');
    if(cc){let cx=cc.getContext('2d');cx.clearRect(0,0,cc.width,cc.height);}
}

function gameOver(){
    G.running=false;
    G.score+=Math.floor(G.distance);
    if(isNewHighScore(G.score)){
        setTimeout(()=>showNameEntry('gameover'),500);
    } else {
        if(isInTop5(G.score)){
            addScore('---',G.score);
        }
        setTimeout(()=>showGameOverFinal(),500);
    }
}

function showGameOverFinal(){
    document.getElementById('goStage').textContent=(G.stageIndex+1)+' - '+STAGES[G.stageIndex].name;
    document.getElementById('goScore').textContent=G.score;
    document.getElementById('goSocks').textContent=G.totalSocks;
    document.getElementById('goSpeed').textContent=Math.floor(G.maxSpeedRecord);
    renderLeaderboard('goLeaderboard',G.score);
    document.getElementById('gameOverScreen').style.display='flex';
    document.getElementById('laneIndicator').style.display='none';
}

function startGame(){
    document.getElementById('startScreen').style.display='none';
    resetFullGame();
    showStageScreen();
}

function restartGame(){
    document.getElementById('gameOverScreen').style.display='none';
    document.getElementById('winScreen').style.display='none';
    document.getElementById('winFinalScreen').style.display='none';
    stopConfetti();
    resetFullGame();
    showStageScreen();
}

// ===== FINISH ANIMATION =====
function updateFinishAnim(dt){
    if(!G.finishing) return;
    G.finishTimer += dt;

    // Clear obstacles and socks on first frame
    if(G.finishTimer < 0.05){
        G.obstacles=[];
        G.sockItems=[];
    }

    // Car keeps driving forward visually
    G.scrollY += G.speed * 3;
    G.speed = Math.max(G.speed * 0.995, 2);

    // Snow keeps falling
    G.snowflakes.forEach(s=>{s.y+=s.speed+G.speed*2;s.x+=s.wind;if(s.y>canvas.height){s.y=-5;s.x=Math.random()*canvas.width}if(s.x<0)s.x=canvas.width;if(s.x>canvas.width)s.x=0});

    // Particles fade out
    G.particles=G.particles.filter(p=>{p.x+=p.vx;p.y+=p.vy+G.speed*1.5;p.life-=dt;p.size*=.98;return p.life>0});

    // Trees scroll
    [...G.treesL,...G.treesR].forEach(t=>{t.y+=G.speed*3;if(t.y>canvas.height+50){t.y-=1500;t.size=15+Math.random()*20}});

    // Trail
    if(Math.random()<.2){
        G.carTrail.push({x:car.x-11,y:car.y+car.height/2,life:1});
        G.carTrail.push({x:car.x+11,y:car.y+car.height/2,life:1});
    }
    G.carTrail=G.carTrail.filter(t=>{t.life-=dt*.5;t.y+=G.speed*.5;return t.life>0});
}

// ===== LOOP =====
function gameLoop(ts){
    try{
        if(!G.lastTime)G.lastTime=ts;
        let dt=(ts-G.lastTime)/1000;G.lastTime=ts;
        if(dt>.1)dt=.1;
        if(G.running)update(dt);
        if(G.finishing)updateFinishAnim(dt);
        draw();
    }catch(err){
        console.error('Game loop error:', err);
        G.running=false;
        G.finishing=false;
        document.getElementById('gameOverScreen').style.display='flex';
    }
    requestAnimationFrame(gameLoop);
}

resetFullGame();
initStage();
G.speed=2;
requestAnimationFrame(gameLoop);
</script>
</body>
</html>
