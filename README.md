<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
<title>英语单词闯关大冒险</title>
<script src="https://cdn.tailwindcss.com"></script>
<style>
/* ========== 全局样式 ========== */
* { box-sizing: border-box; }
body {
  margin: 0; padding: 0;
  font-family: 'Comic Sans MS', 'KaiTi', 'STKaiti', sans-serif;
  overflow-x: hidden;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  min-height: 100vh;
  user-select: none;
  -webkit-user-select: none;
  -webkit-tap-highlight-color: transparent;
}

.screen {
  display: none;
  width: 100%; min-height: 100vh;
  flex-direction: column; align-items: center;
  padding: 10px;
}
.screen.active { display: flex; }

/* ========== 首页 ========== */
#start-screen {
  justify-content: center;
  background: linear-gradient(180deg, #ffecd2 0%, #fcb69f 50%, #ff9a9e 100%);
}
.start-title {
  font-size: clamp(1.8rem, 6vw, 3rem);
  color: #e74c3c;
  text-shadow: 3px 3px 0 #f39c12, 6px 6px 0 rgba(0,0,0,0.1);
  margin-bottom: 8px;
  animation: bounceTitle 1s ease infinite;
}
@keyframes bounceTitle {
  0%,100% { transform: translateY(0); }
  50% { transform: translateY(-15px); }
}
.start-subtitle {
  font-size: clamp(1rem, 3vw, 1.4rem);
  color: #8e44ad;
  margin-bottom: 30px;
}
.btn-start {
  background: linear-gradient(180deg, #f1c40f, #f39c12);
  border: 4px solid #e67e22;
  color: #fff;
  font-size: clamp(1.4rem, 5vw, 2rem);
  font-weight: bold;
  padding: 16px 60px;
  border-radius: 50px;
  cursor: pointer;
  box-shadow: 0 8px 0 #d35400, 0 10px 20px rgba(0,0,0,0.2);
  transition: all 0.1s;
  letter-spacing: 4px;
}
.btn-start:active {
  transform: translateY(4px);
  box-shadow: 0 4px 0 #d35400, 0 5px 10px rgba(0,0,0,0.2);
}
.level-dots {
  display: flex; gap: 20px; margin-top: 30px;
}
.level-dot {
  width: 35px; height: 35px;
  border-radius: 50%;
  background: #bdc3c7;
  display: flex; align-items: center; justify-content: center;
  font-size: 18px; color: #fff;
  border: 3px solid #95a5a6;
  transition: all 0.3s;
}
.level-dot.unlocked { background: #2ecc71; border-color: #27ae60; }
.level-dot.done { background: #f1c40f; border-color: #f39c12; }

/* ========== 游戏通用头部 ========== */
.game-header {
  width: 100%; max-width: 700px;
  display: flex; justify-content: space-between; align-items: center;
  background: rgba(255,255,255,0.9);
  border-radius: 20px;
  padding: 10px 20px;
  margin-bottom: 12px;
  box-shadow: 0 4px 15px rgba(0,0,0,0.1);
  flex-wrap: wrap; gap: 8px;
}
.game-header .level-name {
  font-size: 1.2rem; font-weight: bold; color: #6c5ce7;
}
.game-header .score-display {
  font-size: 1.1rem; font-weight: bold; color: #e74c3c;
  background: #fff3cd; padding: 4px 14px; border-radius: 20px;
}
.btn-reset {
  background: #ff7675; color: #fff; border: none;
  padding: 6px 18px; border-radius: 20px;
  font-size: 0.95rem; cursor: pointer; font-weight: bold;
  box-shadow: 0 3px 0 #d63031;
  transition: all 0.1s;
}
.btn-reset:active { transform: translateY(2px); box-shadow: 0 1px 0 #d63031; }

/* ========== 关卡1：翻牌记忆配对 ========== */
#level1-screen { background: linear-gradient(180deg, #a8e6cf 0%, #88d8b0 50%, #56ab91 100%); }
.card-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 10px;
  max-width: 650px; width: 100%;
  padding: 0 8px;
}
@media (max-width: 500px) {
  .card-grid { grid-template-columns: repeat(3, 1fr); gap: 8px; }
}
.memory-card {
  aspect-ratio: 1;
  perspective: 800px;
  cursor: pointer;
}
.memory-card .card-inner {
  width: 100%; height: 100%;
  position: relative;
  transform-style: preserve-3d;
  transition: transform 0.5s;
  border-radius: 14px;
}
.memory-card.flipped .card-inner { transform: rotateY(180deg); }
.memory-card.matched .card-inner { transform: rotateY(180deg); }
.memory-card.matched {
  animation: matchPop 0.5s ease forwards;
  pointer-events: none;
}
@keyframes matchPop {
  0% { transform: scale(1); opacity: 1; }
  50% { transform: scale(1.15); }
  100% { transform: scale(0); opacity: 0; }
}
.card-front, .card-back {
  position: absolute; inset: 0;
  border-radius: 14px;
  backface-visibility: hidden;
  display: flex; align-items: center; justify-content: center;
  border: 4px solid #fff;
}
.card-front {
  background: linear-gradient(135deg, #ff6b6b, #ee5a24);
  box-shadow: 0 4px 12px rgba(0,0,0,0.15);
}
.card-front::after {
  content: '⭐';
  font-size: clamp(1.8rem, 5vw, 2.5rem);
}
.card-back {
  background: #fff;
  transform: rotateY(180deg);
  box-shadow: 0 4px 12px rgba(0,0,0,0.15);
  flex-direction: column;
  padding: 6px;
}
.card-back .card-word {
  font-size: clamp(0.7rem, 2vw, 0.95rem);
  font-weight: bold;
  color: #2c3e50;
  text-align: center;
  word-break: break-word;
}

/* --- CSS 简笔画：教室物品 --- */
.drawing { width: 55px; height: 45px; position: relative; flex-shrink: 0; }
/* window */
.drawing.window {
  width: 50px; height: 40px;
  background: #87CEEB;
  border: 3px solid #795548;
  border-radius: 2px;
}
.drawing.window::before {
  content: '';
  position: absolute; top: 50%; left: 0; right: 0; height: 2px; background: #795548;
}
.drawing.window::after {
  content: '';
  position: absolute; left: 50%; top: 0; bottom: 0; width: 2px; background: #795548;
}
/* blackboard */
.drawing.blackboard {
  width: 50px; height: 36px;
  background: #2d5016;
  border: 3px solid #8d6e63;
  border-radius: 3px;
}
.drawing.blackboard::after {
  content: '';
  position: absolute; bottom: -8px; left: 50%; transform: translateX(-50%);
  width: 40px; height: 5px; background: #8d6e63; border-radius: 0 0 4px 4px;
}
/* door */
.drawing.door {
  width: 32px; height: 48px;
  background: #d4a574;
  border: 3px solid #8d6e63;
  border-radius: 5px 5px 0 0;
}
.drawing.door::after {
  content: '';
  position: absolute; right: 5px; top: 50%;
  width: 5px; height: 5px;
  background: #f1c40f; border-radius: 50%;
  border: 1px solid #8d6e63;
}
/* desk */
.drawing.desk {
  width: 48px; height: 30px;
  background: #d4a574;
  border: 2px solid #8d6e63;
  border-radius: 3px;
}
.drawing.desk::before {
  content: '';
  position: absolute; bottom: -12px; left: 6px;
  width: 4px; height: 12px; background: #8d6e63; border-radius: 1px;
  box-shadow: 30px 0 0 #8d6e63;
}
/* chair */
.drawing.chair {
  width: 30px; height: 46px; position: relative;
}
.drawing.chair::before {
  content: '';
  position: absolute; top: 0; left: 0; right: 0; height: 16px;
  background: #d4a574; border: 2px solid #8d6e63; border-radius: 4px;
}
.drawing.chair::after {
  content: '';
  position: absolute; top: 10px; right: 2px;
  width: 8px; height: 26px;
  background: #d4a574; border: 2px solid #8d6e63; border-radius: 3px;
}
/* schoolbag */
.drawing.schoolbag {
  width: 40px; height: 36px;
  background: #e74c3c;
  border: 3px solid #c0392b;
  border-radius: 8px 8px 12px 12px;
}
.drawing.schoolbag::before {
  content: '';
  position: absolute; top: -8px; left: 8px;
  width: 20px; height: 10px;
  border: 3px solid #c0392b;
  border-radius: 10px 10px 0 0;
  border-bottom: none;
  background: transparent;
}
/* have a look - magnifying glass */
.drawing.have-a-look {
  width: 40px; height: 40px;
  border: 4px solid #7f8c8d;
  border-radius: 50%;
  background: rgba(189,195,199,0.3);
}
.drawing.have-a-look::after {
  content: '';
  position: absolute; bottom: -8px; right: -6px;
  width: 4px; height: 18px;
  background: #7f8c8d; border-radius: 2px;
  transform: rotate(-45deg);
  transform-origin: top center;
}

/* ========== 关卡2：水果飞船 ========== */
#level2-screen { background: linear-gradient(180deg, #0c0c3d 0%, #1a1a5e 30%, #2d1b69 100%); }
.game-area {
  position: relative;
  width: 100%; max-width: 650px;
  height: 55vh; min-height: 350px;
  background: linear-gradient(180deg, #0a0a2e 0%, #16213e 100%);
  border-radius: 16px;
  overflow: hidden;
  border: 3px solid #4a69bd;
  box-shadow: 0 0 30px rgba(74,105,189,0.4);
  margin-bottom: 8px;
}
.fruit-block {
  position: absolute;
  width: clamp(44px, 10vw, 62px);
  height: clamp(44px, 10vw, 62px);
  border-radius: 12px;
  display: flex; flex-direction: column;
  align-items: center; justify-content: center;
  cursor: pointer;
  transition: opacity 0.2s;
  border: 2px solid rgba(255,255,255,0.5);
}
.fruit-block:hover { border-color: #fff; box-shadow: 0 0 15px rgba(255,255,255,0.5); }
.fruit-block.destroyed {
  animation: blockExplode 0.4s ease forwards;
  pointer-events: none;
}
@keyframes blockExplode {
  0% { transform: scale(1); opacity: 1; }
  100% { transform: scale(2); opacity: 0; }
}
.fruit-block .fw {
  font-size: clamp(0.55rem, 1.8vw, 0.7rem);
  font-weight: bold; color: #fff;
  text-shadow: 1px 1px 0 rgba(0,0,0,0.5);
  margin-top: 2px;
}
.spaceship-bar {
  display: flex; gap: 6px; flex-wrap: wrap; justify-content: center;
  max-width: 700px; width: 100%;
  background: rgba(255,255,255,0.1);
  border-radius: 16px; padding: 10px;
  margin-bottom: 8px;
}
.ship-word-btn {
  padding: 6px 12px;
  border-radius: 20px;
  font-weight: bold;
  font-size: clamp(0.65rem, 2vw, 0.85rem);
  cursor: pointer;
  border: 2px solid rgba(255,255,255,0.4);
  color: #fff;
  background: rgba(255,255,255,0.15);
  transition: all 0.2s;
}
.ship-word-btn:hover { background: rgba(255,255,255,0.3); }
.ship-word-btn.armed {
  background: #f1c40f !important;
  color: #2c3e50 !important;
  border-color: #f39c12 !important;
  animation: pulseArm 0.6s ease infinite;
  box-shadow: 0 0 20px rgba(241,196,15,0.7);
}
@keyframes pulseArm {
  0%,100% { box-shadow: 0 0 10px rgba(241,196,15,0.5); }
  50% { box-shadow: 0 0 25px rgba(241,196,15,0.9); }
}
.timer-display {
  font-size: 1.3rem; font-weight: bold; color: #e74c3c;
  background: rgba(0,0,0,0.4); padding: 4px 16px; border-radius: 20px;
}

/* --- CSS 简笔画：水果 --- */
.fruit-dwg {
  width: 28px; height: 28px; position: relative; flex-shrink: 0;
}
/* lemon */
.fruit-dwg.lemon {
  width: 30px; height: 22px;
  background: #f9e74a; border-radius: 50%;
  border: 1.5px solid #e7d730;
}
.fruit-dwg.lemon::after {
  content: '';
  position: absolute; left: -3px; top: 50%; transform: translateY(-50%);
  width: 4px; height: 4px; border-radius: 50%; background: #5a8f3a;
}
/* banana */
.fruit-dwg.banana {
  width: 32px; height: 14px;
  background: #ffe066; border-radius: 0 0 50% 50%;
  border: 1.5px solid #e7c030;
  transform: rotate(-20deg);
}
/* apple */
.fruit-dwg.apple {
  width: 26px; height: 26px;
  background: #e74c3c; border-radius: 50%;
  border: 1.5px solid #c0392b;
}
.fruit-dwg.apple::before {
  content: '';
  position: absolute; top: -4px; left: 50%; transform: translateX(-50%);
  width: 3px; height: 6px; background: #5d4037; border-radius: 1px;
}
.fruit-dwg.apple::after {
  content: '';
  position: absolute; top: -2px; left: calc(50% + 2px);
  width: 8px; height: 5px;
  background: #27ae60; border-radius: 50%;
}
/* watermelon */
.fruit-dwg.watermelon {
  width: 30px; height: 30px;
  background: #2d8c3c; border-radius: 50%;
  border: 1.5px solid #1e6d2b;
  overflow: hidden;
}
.fruit-dwg.watermelon::before {
  content: '';
  position: absolute; top: 4px; left: 6px; right: 6px;
  height: 2px; background: #1a5c20; border-radius: 50%;
  box-shadow: 0 6px 0 #1a5c20, 0 12px 0 #1a5c20;
}
/* pear */
.fruit-dwg.pear {
  width: 22px; height: 30px;
  background: #a8d86b; border-radius: 40% 40% 50% 50%;
  border: 1.5px solid #7ab648;
}
.fruit-dwg.pear::before {
  content: '';
  position: absolute; top: -3px; left: 50%; transform: translateX(-50%);
  width: 2px; height: 5px; background: #5d4037;
}
/* pineapple */
.fruit-dwg.pineapple {
  width: 24px; height: 28px;
  background: #f0a830; border-radius: 30% 30% 40% 40%;
  border: 1.5px solid #d4910a;
}
.fruit-dwg.pineapple::before {
  content: '';
  position: absolute; top: -10px; left: 50%; transform: translateX(-50%);
  width: 0; height: 0;
  border-left: 10px solid transparent;
  border-right: 10px solid transparent;
  border-bottom: 14px solid #27ae60;
}
/* peach */
.fruit-dwg.peach {
  width: 27px; height: 27px;
  background: #f8b8a0; border-radius: 50%;
  border: 1.5px solid #e8957a;
}
.fruit-dwg.peach::after {
  content: '';
  position: absolute; top: 4px; left: 50%; transform: translateX(-50%);
  width: 2px; height: 10px; background: #d4907a; border-radius: 1px;
}
/* orange */
.fruit-dwg.orange {
  width: 27px; height: 27px;
  background: #ffa502; border-radius: 50%;
  border: 1.5px solid #e08e0a;
}
.fruit-dwg.orange::after {
  content: '';
  position: absolute; bottom: -2px; left: 50%; transform: translateX(-50%);
  width: 6px; height: 4px; background: #e08e0a; border-radius: 50%;
}

/* ========== 关卡3：池塘捕鱼 ========== */
#level3-screen { background: linear-gradient(180deg, #a8d8ea 0%, #7ec8e3 30%, #4a90d9 100%); }
.pond {
  position: relative;
  width: 100%; max-width: 650px;
  height: 55vh; min-height: 380px;
  background: linear-gradient(180deg, #5dade2 0%, #2e86c1 40%, #1b4f72 100%);
  border-radius: 24px;
  overflow: hidden;
  border: 4px solid #85c1e9;
  box-shadow: 0 8px 32px rgba(0,0,0,0.2), inset 0 0 60px rgba(255,255,255,0.1);
  cursor: none;
  margin-bottom: 8px;
}
/* water ripple effect */
.pond::before {
  content: '';
  position: absolute; inset: 0;
  background: repeating-linear-gradient(
    0deg,
    transparent,
    transparent 40px,
    rgba(255,255,255,0.04) 40px,
    rgba(255,255,255,0.04) 42px
  );
  pointer-events: none; z-index: 1;
}
.fish {
  position: absolute;
  display: flex; align-items: center; justify-content: center;
  transition: none;
  z-index: 2;
}
.fish-body {
  width: clamp(50px, 12vw, 70px);
  height: clamp(34px, 8vw, 46px);
  border-radius: 45% 55% 50% 50%;
  display: flex; align-items: center; justify-content: center;
  position: relative;
  border: 2px solid rgba(0,0,0,0.2);
}
.fish-body .fish-label {
  font-size: clamp(0.55rem, 1.8vw, 0.75rem);
  font-weight: bold;
  color: #fff;
  text-shadow: 1px 1px 0 rgba(0,0,0,0.6);
  z-index: 1;
}
.fish-body::after {
  content: '';
  position: absolute; right: -12px; top: 50%; transform: translateY(-50%);
  width: 0; height: 0;
  border-top: 10px solid transparent;
  border-bottom: 10px solid transparent;
  border-left: 14px solid inherit;
}
.fish-body.tadpole-f {
  background: #3e2723; border-radius: 50%;
}
.fish-body.tadpole-f::after {
  content: '';
  position: absolute; left: -14px; top: 50%;
  width: 16px; height: 4px;
  background: #3e2723;
  border-radius: 2px;
  transform: translateY(-50%);
  border: none;
}
.fish-body.frog-f { background: #58b94a; }
.fish-body.frog-f::after { border-left-color: #58b94a; }
.fish-body.butterfly-f { background: #ff793f; border-radius: 50% 50% 30% 30%; }
.fish-body.goldfish-f { background: #ff6b35; }
.fish-body.goldfish-f::after { border-left-color: #ff6b35; }
.fish-body.duck-f { background: #f9ca24; }
.fish-body.duck-f::after { border-left-color: #f9ca24; }
.fish-body.tortoise-f { background: #6ab04c; border-radius: 50% 50% 40% 40%; }

.fish.caught {
  animation: fishCatch 0.5s ease forwards;
  pointer-events: none;
}
@keyframes fishCatch {
  0% { transform: scale(1); opacity: 1; }
  100% { transform: scale(2.5); opacity: 0; }
}

/* 渔网光标 */
.net-cursor {
  position: absolute;
  width: 60px; height: 60px;
  pointer-events: none;
  z-index: 10;
  transform: translate(-50%, -50%);
  display: none;
}
.net-cursor.visible { display: block; }
.net-hoop {
  width: 50px; height: 50px;
  border: 3px solid #8d6e63;
  border-radius: 50%;
  background: rgba(255,255,255,0.2);
  position: relative;
}
.net-hoop::before {
  content: '';
  position: absolute; top: -20px; left: 50%; transform: translateX(-50%);
  width: 4px; height: 24px;
  background: #a1887f; border-radius: 2px;
}
.net-hoop::after {
  content: '';
  position: absolute; inset: 4px;
  border: 2px dashed rgba(141,110,99,0.5);
  border-radius: 50%;
}
.target-word-bar {
  background: rgba(255,255,255,0.9);
  padding: 8px 20px;
  border-radius: 20px;
  font-size: 1.1rem;
  font-weight: bold;
  color: #2c3e50;
  margin-bottom: 8px;
  text-align: center;
}
.target-word-bar .listen-btn {
  background: #3498db; color: #fff; border: none;
  padding: 4px 12px; border-radius: 14px;
  cursor: pointer; font-size: 0.9rem;
  margin-left: 8px;
}

/* ========== 通关弹窗 ========== */
.modal-overlay {
  position: fixed; inset: 0;
  background: rgba(0,0,0,0.6);
  display: flex; align-items: center; justify-content: center;
  z-index: 100;
}
.modal-box {
  background: #fff;
  border-radius: 24px;
  padding: 30px 24px;
  text-align: center;
  max-width: 400px; width: 90%;
  box-shadow: 0 20px 60px rgba(0,0,0,0.3);
  animation: popIn 0.4s cubic-bezier(0.68, -0.55, 0.265, 1.55);
}
@keyframes popIn {
  0% { transform: scale(0.3); opacity: 0; }
  100% { transform: scale(1); opacity: 1; }
}
.modal-box h2 { font-size: 1.8rem; color: #e74c3c; margin-bottom: 10px; }
.modal-box .stars { font-size: 2.5rem; margin: 10px 0; }
.modal-btn {
  background: linear-gradient(180deg, #2ecc71, #27ae60);
  color: #fff; border: none;
  padding: 12px 36px; border-radius: 30px;
  font-size: 1.2rem; font-weight: bold; cursor: pointer;
  box-shadow: 0 4px 0 #1e8449;
  margin: 6px;
}
.modal-btn:active { transform: translateY(2px); box-shadow: 0 2px 0 #1e8449; }

/* ========== 电子奖状 ========== */
#certificate-screen {
  background: linear-gradient(135deg, #ffecd2 0%, #fcb69f 100%);
  justify-content: center;
}
.certificate {
  background: #fffef5;
  border: 6px double #f39c12;
  border-radius: 16px;
  padding: 30px 20px;
  max-width: 500px; width: 92%;
  text-align: center;
  box-shadow: 0 10px 40px rgba(0,0,0,0.15);
  position: relative;
}
.certificate::before {
  content: '🏆';
  position: absolute; top: -30px; left: 50%; transform: translateX(-50%);
  font-size: 3rem;
}
.certificate h2 {
  font-size: clamp(1.4rem, 5vw, 2rem);
  color: #e74c3c;
  margin: 16px 0 8px;
}
.certificate .big-score {
  font-size: clamp(2.5rem, 8vw, 4rem);
  font-weight: bold;
  color: #f39c12;
  text-shadow: 2px 2px 0 #e67e22;
}
.certificate .cert-stars { font-size: 2rem; margin: 8px 0; }
.cert-seal {
  display: inline-block;
  border: 3px solid #e74c3c;
  border-radius: 50%;
  padding: 8px 16px;
  color: #e74c3c;
  font-weight: bold;
  font-size: 0.85rem;
  transform: rotate(-15deg);
  margin-top: 10px;
  opacity: 0.8;
}

/* ========== 响应式调整 ========== */
@media (max-width: 400px) {
  .game-header { padding: 8px 12px; }
  .game-header .level-name { font-size: 1rem; }
  .card-grid { gap: 6px; }
  .pond { height: 45vh; min-height: 300px; }
  .game-area { height: 45vh; min-height: 300px; }
  .ship-word-btn { padding: 4px 8px; font-size: 0.6rem; }
}
</style>
</head>
<body>

<!-- ========== 首页 ========== -->
<div id="start-screen" class="screen active">
  <div class="start-title">🌟 英语闯关大冒险 🌟</div>
  <div class="start-subtitle">和小伙伴一起学单词吧！</div>
  <button class="btn-start" onclick="startGame()">开 始 闯 关</button>
  <div class="level-dots">
    <div class="level-dot unlocked" id="dot1">1</div>
    <div class="level-dot" id="dot2">🔒</div>
    <div class="level-dot" id="dot3">🔒</div>
  </div>
  <p style="color:#8e44ad;margin-top:12px;font-size:0.85rem;">总分：<b id="total-score-home">0</b></p>
</div>

<!-- ========== 关卡1：翻牌记忆配对 ========== -->
<div id="level1-screen" class="screen">
  <div class="game-header">
    <span class="level-name">🏫 第1关：教室单词翻牌配对</span>
    <span class="score-display">⭐ <span id="score1">0</span></span>
    <button class="btn-reset" onclick="resetLevel1()">🔄 重玩</button>
  </div>
  <div class="card-grid" id="card-grid"></div>
</div>

<!-- ========== 关卡2：水果飞船 ========== -->
<div id="level2-screen" class="screen">
  <div class="game-header">
    <span class="level-name">🚀 第2关：水果飞船大作战</span>
    <span class="timer-display">⏱ <span id="timer2">90</span>s</span>
    <span class="score-display">⭐ <span id="score2">0</span></span>
    <button class="btn-reset" onclick="resetLevel2()">🔄 重玩</button>
  </div>
  <div class="game-area" id="game-area">
    <div style="position:absolute;top:50%;left:50%;transform:translate(-50%,-50%);color:rgba(255,255,255,0.15);font-size:2rem;pointer-events:none;text-align:center;">
      🍎 🍋 🍌<br>水果来袭！
    </div>
  </div>
  <div class="spaceship-bar" id="spaceship-bar"></div>
</div>

<!-- ========== 关卡3：池塘捕鱼 ========== -->
<div id="level3-screen" class="screen">
  <div class="game-header">
    <span class="level-name">🎣 第3关：池塘捕鱼选词</span>
    <span class="score-display">🐟 进度：<span id="fish-progress">0/6</span></span>
    <span class="score-display">⭐ <span id="score3">0</span></span>
    <button class="btn-reset" onclick="resetLevel3()">🔄 重玩</button>
  </div>
  <div class="target-word-bar">
    🎧 听一听，找一找：<b id="target-word-text">---</b>
    <button class="listen-btn" onclick="speakTargetWord()">🔊 再听一遍</button>
  </div>
  <div class="pond" id="pond">
    <div class="net-cursor" id="net-cursor">
      <div class="net-hoop"></div>
    </div>
  </div>
</div>

<!-- ========== 通关弹窗（关卡间过渡） ========== -->
<div class="modal-overlay" id="modal-overlay" style="display:none;">
  <div class="modal-box">
    <h2 id="modal-title">🎉 太棒了！</h2>
    <div class="stars" id="modal-stars">⭐⭐⭐</div>
    <p id="modal-msg" style="color:#555;margin-bottom:16px;">本关得分已记录！</p>
    <button class="modal-btn" id="modal-btn" onclick="advanceLevel()">进入下一关 ➡️</button>
  </div>
</div>

<!-- ========== 通关失败弹窗（第2关计时结束） ========== -->
<div class="modal-overlay" id="fail-overlay" style="display:none;">
  <div class="modal-box">
    <h2>⏰ 时间到！</h2>
    <div class="stars">😢</div>
    <p style="color:#555;margin-bottom:16px;">水果方块还没清完呢，再试一次吧！</p>
    <button class="modal-btn" style="background:linear-gradient(180deg,#e74c3c,#c0392b);box-shadow:0 4px 0 #a93226;" onclick="resetLevel2()">🔄 重新挑战</button>
  </div>
</div>

<!-- ========== 电子奖状 ========== -->
<div id="certificate-screen" class="screen">
  <div class="certificate">
    <h2>🏅 通关荣誉证书 🏅</h2>
    <p style="color:#555;margin:4px 0;">恭喜你完成了全部三关挑战！</p>
    <div class="big-score" id="final-score">0</div>
    <p style="color:#e67e22;font-weight:bold;">总得分</p>
    <div class="cert-stars">🌟🌟🌟</div>
    <p style="color:#7f8c8d;font-size:0.9rem;">你是一名出色的英语小达人！</p>
    <div class="cert-seal">★ 优秀 ★</div>
    <br><br>
    <button class="modal-btn" onclick="restartAll()">🔄 重新开始</button>
  </div>
</div>

<script>
// ==================== 全局游戏状态 ====================
const state = {
  score: 0,
  currentLevel: 0,   // 0=start, 1=level1, 2=level2, 3=level3, 4=cert
  level1Complete: false,
  level2Complete: false,
  level3Complete: false,
};

// ==================== 单词配置（方便后续修改） ====================
const WORDS_LEVEL1 = [
  { word: 'window',    cn: '窗户' },
  { word: 'blackboard', cn: '黑板' },
  { word: 'door',      cn: '门' },
  { word: 'desk',      cn: '书桌' },
  { word: 'chair',     cn: '椅子' },
  { word: 'schoolbag', cn: '书包' },
  { word: 'have a look', cn: '看一看' },
];

const WORDS_LEVEL2 = [
  { word: 'lemon',     cn: '柠檬' },
  { word: 'banana',    cn: '香蕉' },
  { word: 'apple',     cn: '苹果' },
  { word: 'watermelon', cn: '西瓜' },
  { word: 'pear',      cn: '梨' },
  { word: 'pineapple', cn: '菠萝' },
  { word: 'peach',     cn: '桃子' },
  { word: 'orange',    cn: '橙子' },
];

const WORDS_LEVEL3 = [
  { word: 'tadpole',   cn: '蝌蚪' },
  { word: 'frog',      cn: '青蛙' },
  { word: 'butterfly', cn: '蝴蝶' },
  { word: 'goldfish',  cn: '金鱼' },
  { word: 'duck',      cn: '鸭子' },
  { word: 'tortoise',  cn: '乌龟' },
];

// ==================== 语音合成 ====================
function speakWord(word) {
  if (!window.speechSynthesis) return;
  speechSynthesis.cancel();
  const u = new SpeechSynthesisUtterance(word);
  u.lang = 'en-US';
  u.rate = 0.85;
  u.pitch = 1.1;
  speechSynthesis.speak(u);
}

// ==================== 屏幕切换 ====================
function showScreen(id) {
  document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
  const el = document.getElementById(id);
  if (el) el.classList.add('active');
}

function updateDots() {
  const d1 = document.getElementById('dot1');
  const d2 = document.getElementById('dot2');
  const d3 = document.getElementById('dot3');
  if (state.level1Complete) { d1.textContent = '✅'; d1.classList.add('done'); }
  if (state.level2Complete) { d2.textContent = '✅'; d2.classList.add('done'); }
  if (state.level3Complete) { d3.textContent = '✅'; d3.classList.add('done'); }
  if (state.level1Complete && !state.level2Complete) { d2.textContent = '2'; d2.classList.add('unlocked'); }
  if (state.level2Complete && !state.level3Complete) { d3.textContent = '3'; d3.classList.add('unlocked'); }
  document.getElementById('total-score-home').textContent = state.score;
}

function updateAllScoreDisplays() {
  document.getElementById('score1').textContent = state.score;
  document.getElementById('score2').textContent = state.score;
  document.getElementById('score3').textContent = state.score;
}

// ==================== 首页 ====================
function startGame() {
  state.currentLevel = 1;
  state.score = 0;
  state.level1Complete = false;
  state.level2Complete = false;
  state.level3Complete = false;
  updateAllScoreDisplays();
  updateDots();
  showScreen('level1-screen');
  initLevel1();
}

// ==================== 关卡1：翻牌记忆配对 ====================
let level1Cards = [];
let level1Flipped = [];
let level1Locked = false;
let level1MatchedCount = 0;

function initLevel1() {
  level1Cards = [];
  level1Flipped = [];
  level1Locked = false;
  level1MatchedCount = 0;
  updateAllScoreDisplays();

  // 为每个单词生成两张卡片：一张图片卡，一张单词卡
  WORDS_LEVEL1.forEach(w => {
    level1Cards.push({ word: w.word, type: 'picture', id: w.word + '_pic' });
    level1Cards.push({ word: w.word, type: 'word', id: w.word + '_txt' });
  });
  // 洗牌
  shuffleArray(level1Cards);

  const grid = document.getElementById('card-grid');
  grid.innerHTML = '';
  level1Cards.forEach((card, idx) => {
    const cardEl = document.createElement('div');
    cardEl.className = 'memory-card';
    cardEl.dataset.index = idx;
    cardEl.dataset.word = card.word;
    cardEl.dataset.type = card.type;

    const isPic = card.type === 'picture';
    const cssClass = card.word.replace(/\s+/g, '-'); // "have a look" → "have-a-look"

    cardEl.innerHTML = `
      <div class="card-inner">
        <div class="card-front"></div>
        <div class="card-back">
          ${isPic ? `<div class="drawing ${cssClass}"></div>` : ''}
          <span class="card-word">${isPic ? card.word : card.word}</span>
        </div>
      </div>
    `;
    cardEl.addEventListener('click', () => onCardClick(cardEl, idx));
    grid.appendChild(cardEl);
  });
}

function onCardClick(cardEl, idx) {
  if (level1Locked) return;
  if (cardEl.classList.contains('flipped')) return;
  if (cardEl.classList.contains('matched')) return;
  if (level1Flipped.length >= 2) return;

  cardEl.classList.add('flipped');
  level1Flipped.push({ el: cardEl, idx });

  if (level1Flipped.length === 2) {
    level1Locked = true;
    const [a, b] = level1Flipped;
    const ca = level1Cards[a.idx];
    const cb = level1Cards[b.idx];

    // 匹配条件：同一单词，且一图一词
    if (ca.word === cb.word && ca.type !== cb.type) {
      // 匹配成功
      setTimeout(() => {
        a.el.classList.add('matched');
        b.el.classList.add('matched');
        level1MatchedCount++;
        state.score += 10;
        updateAllScoreDisplays();
        speakWord(ca.word);

        setTimeout(() => {
          a.el.style.visibility = 'hidden';
          b.el.style.visibility = 'hidden';
        }, 500);

        if (level1MatchedCount === WORDS_LEVEL1.length) {
          setTimeout(() => level1Complete(), 800);
        }
        level1Flipped = [];
        level1Locked = false;
      }, 400);
    } else {
      // 不匹配，翻回去
      setTimeout(() => {
        a.el.classList.remove('flipped');
        b.el.classList.remove('flipped');
        level1Flipped = [];
        level1Locked = false;
      }, 700);
    }
  }
}

function level1Complete() {
  state.level1Complete = true;
  updateDots();
  showModal(
    '🎉 第一关通过！',
    '⭐⭐⭐',
    '教室里的单词都记住啦！准备好迎接水果挑战了吗？',
    '进入第二关 ➡️'
  );
}

function resetLevel1() {
  initLevel1();
}

// ==================== 关卡2：水果飞船大作战 ====================
let level2Blocks = [];
let level2ArmedWord = null;
let level2Timer = 90;
let level2TimerId = null;
let level2SpawnId = null;
let level2GameOver = false;
let level2TotalBlocks = 0;
let level2DestroyedBlocks = 0;
const LEVEL2_BLOCK_COUNT = 16; // 每种水果2个

function initLevel2() {
  level2Blocks = [];
  level2ArmedWord = null;
  level2Timer = 90;
  level2GameOver = false;
  level2TotalBlocks = LEVEL2_BLOCK_COUNT;
  level2DestroyedBlocks = 0;
  updateAllScoreDisplays();
  document.getElementById('timer2').textContent = level2Timer;

  // 清空游戏区域
  const area = document.getElementById('game-area');
  area.querySelectorAll('.fruit-block').forEach(b => b.remove());

  // 搭建飞船按钮
  const bar = document.getElementById('spaceship-bar');
  bar.innerHTML = '<span style="color:#fff;font-size:0.85rem;align-self:center;">🚀 点击选词 →</span>';
  WORDS_LEVEL2.forEach(w => {
    const btn = document.createElement('button');
    btn.className = 'ship-word-btn';
    btn.textContent = w.word;
    btn.dataset.word = w.word;
    btn.addEventListener('click', () => armWord(w.word, btn));
    bar.appendChild(btn);
  });

  // 隐藏失败弹窗
  document.getElementById('fail-overlay').style.display = 'none';

  // 启动倒计时
  clearInterval(level2TimerId);
  level2TimerId = setInterval(() => {
    level2Timer--;
    document.getElementById('timer2').textContent = level2Timer;
    if (level2Timer <= 0) {
      clearInterval(level2TimerId);
      if (!level2GameOver) level2Fail();
    }
  }, 1000);

  // 生成方块列表：每种水果2个
  let blockList = [];
  WORDS_LEVEL2.forEach(w => {
    blockList.push(w.word);
    blockList.push(w.word);
  });
  shuffleArray(blockList);

  // 逐个生成方块下落
  let spawnIdx = 0;
  level2SpawnId = setInterval(() => {
    if (level2GameOver) { clearInterval(level2SpawnId); return; }
    if (spawnIdx >= blockList.length) { clearInterval(level2SpawnId); return; }
    spawnFruitBlock(blockList[spawnIdx]);
    spawnIdx++;
  }, 1800);
}

function spawnFruitBlock(word) {
  if (level2GameOver) return;
  const area = document.getElementById('game-area');
  const block = document.createElement('div');
  block.className = 'fruit-block';
  block.dataset.word = word;

  const cssClass = word.replace(/\s+/g, '-');
  block.innerHTML = `
    <div class="fruit-dwg ${cssClass}"></div>
    <span class="fw">${word}</span>
  `;

  // 随机水平位置
  const areaW = area.clientWidth;
  const blockW = 62;
  const left = Math.random() * (areaW - blockW - 10) + 5;
  block.style.left = left + 'px';
  block.style.top = '-70px';
  block.style.background = getFruitBg(word);

  block.addEventListener('click', () => onBlockClick(block, word));
  area.appendChild(block);

  level2Blocks.push({ el: block, word, y: -70, speed: 0.4 + Math.random() * 0.5 });

  // 动画循环
  if (!level2Blocks._animating) {
    level2Blocks._animating = true;
    requestAnimationFrame(animateBlocks);
  }
}

function getFruitBg(word) {
  const colors = {
    lemon: 'rgba(249,231,74,0.25)',
    banana: 'rgba(255,224,102,0.25)',
    apple: 'rgba(231,76,60,0.25)',
    watermelon: 'rgba(45,140,60,0.25)',
    pear: 'rgba(168,216,107,0.25)',
    pineapple: 'rgba(240,168,48,0.25)',
    peach: 'rgba(248,184,160,0.25)',
    orange: 'rgba(255,165,2,0.25)',
  };
  return colors[word] || 'rgba(255,255,255,0.2)';
}

function animateBlocks() {
  if (level2GameOver) { level2Blocks._animating = false; return; }
  const area = document.getElementById('game-area');
  const areaH = area.clientHeight;

  let alive = false;
  level2Blocks.forEach(b => {
    if (b.el.classList.contains('destroyed')) return;
    alive = true;
    b.y += b.speed;
    b.el.style.top = b.y + 'px';
    // 超出底部即移除
    if (b.y > areaH + 50) {
      b.el.remove();
    }
  });

  if (alive) {
    requestAnimationFrame(animateBlocks);
  } else {
    level2Blocks._animating = false;
  }
}

function armWord(word, btnEl) {
  if (level2GameOver) return;
  // 切换武装状态
  document.querySelectorAll('.ship-word-btn').forEach(b => b.classList.remove('armed'));
  if (level2ArmedWord === word) {
    level2ArmedWord = null;
    return;
  }
  level2ArmedWord = word;
  btnEl.classList.add('armed');
}

function onBlockClick(blockEl, word) {
  if (level2GameOver) return;
  if (blockEl.classList.contains('destroyed')) return;
  if (!level2ArmedWord) return;

  if (level2ArmedWord === word) {
    // 命中！
    blockEl.classList.add('destroyed');
    state.score += 15;
    updateAllScoreDisplays();
    speakWord(word);
    level2DestroyedBlocks++;

    // 取消武装
    level2ArmedWord = null;
    document.querySelectorAll('.ship-word-btn').forEach(b => b.classList.remove('armed'));

    setTimeout(() => blockEl.remove(), 400);

    // 检查胜利
    if (level2DestroyedBlocks >= level2TotalBlocks) {
      level2GameOver = true;
      clearInterval(level2TimerId);
      clearInterval(level2SpawnId);
      setTimeout(() => level2Complete(), 600);
    }
  } else {
    // 打错了，取消武装
    level2ArmedWord = null;
    document.querySelectorAll('.ship-word-btn').forEach(b => b.classList.remove('armed'));
    // 方块闪红提示
    blockEl.style.borderColor = '#e74c3c';
    setTimeout(() => { blockEl.style.borderColor = 'rgba(255,255,255,0.5)'; }, 300);
  }
}

function level2Complete() {
  state.level2Complete = true;
  updateDots();
  clearInterval(level2TimerId);
  clearInterval(level2SpawnId);
  showModal(
    '🚀 第二关通过！',
    '⭐⭐⭐',
    '水果方块全部消灭！最后一关等着你！',
    '进入第三关 ➡️'
  );
}

function level2Fail() {
  level2GameOver = true;
  clearInterval(level2SpawnId);
  document.getElementById('fail-overlay').style.display = 'flex';
}

function resetLevel2() {
  clearInterval(level2TimerId);
  clearInterval(level2SpawnId);
  level2GameOver = true;
  level2Blocks._animating = false;
  document.getElementById('fail-overlay').style.display = 'none';
  initLevel2();
}

// ==================== 关卡3：池塘捕鱼 ====================
let level3Fish = [];
let level3TargetWord = null;
let level3Caught = [];
let level3AnimId = null;
let level3NetVisible = false;
let level3CaughtThisRound = false;
let level3CatchTimer = 0;

function initLevel3() {
  level3Fish = [];
  level3Caught = [];
  level3NetVisible = false;
  level3CaughtThisRound = false;
  level3CatchTimer = 0;
  updateAllScoreDisplays();
  document.getElementById('fish-progress').textContent = '0/6';

  const pond = document.getElementById('pond');
  // 清除旧鱼（保留net-cursor）
  pond.querySelectorAll('.fish').forEach(f => f.remove());

  // 隐藏渔网
  document.getElementById('net-cursor').classList.remove('visible');

  // 创建6条鱼
  WORDS_LEVEL3.forEach((w, i) => {
    const fishEl = document.createElement('div');
    fishEl.className = 'fish';
    fishEl.dataset.word = w.word;
    fishEl.innerHTML = `<div class="fish-body ${w.word}-f"><span class="fish-label">${w.word}</span></div>`;

    const pondW = pond.clientWidth;
    const pondH = pond.clientHeight;
    const x = 60 + Math.random() * (pondW - 140);
    const y = 50 + Math.random() * (pondH - 120);

    fishEl.style.left = x + 'px';
    fishEl.style.top = y + 'px';
    pond.appendChild(fishEl);

    level3Fish.push({
      el: fishEl,
      word: w.word,
      x, y,
      vx: (Math.random() - 0.5) * 1.8,
      vy: (Math.random() - 0.5) * 1.8,
      facingRight: true,
      caught: false,
    });
  });

  // 选择第一个目标单词
  pickNextTarget();

  // 绑定渔网移动事件
  bindNetEvents();

  // 启动鱼游动动画
  cancelAnimationFrame(level3AnimId);
  animateFish();
}

function pickNextTarget() {
  const remaining = WORDS_LEVEL3.filter(w => !level3Caught.includes(w.word));
  if (remaining.length === 0) {
    level3Complete();
    return;
  }
  const pick = remaining[Math.floor(Math.random() * remaining.length)];
  level3TargetWord = pick.word;
  level3CaughtThisRound = false;
  level3CatchTimer = 0;
  document.getElementById('target-word-text').textContent = pick.word + '（' + pick.cn + '）';
  document.getElementById('fish-progress').textContent = level3Caught.length + '/6';
  // 朗读目标单词
  setTimeout(() => speakWord(pick.word), 500);
}

function speakTargetWord() {
  if (level3TargetWord) speakWord(level3TargetWord);
}

function bindNetEvents() {
  const pond = document.getElementById('pond');
  if (pond._netBound) return;
  pond._netBound = true;

  const net = document.getElementById('net-cursor');
  pond._netEl = net;

  const moveNet = (e) => {
    if (state.currentLevel !== 3) return;
    e.preventDefault();
    const rect = pond.getBoundingClientRect();
    const clientX = e.touches ? e.touches[0].clientX : e.clientX;
    const clientY = e.touches ? e.touches[0].clientY : e.clientY;
    const x = clientX - rect.left;
    const y = clientY - rect.top;
    if (x >= 0 && x <= rect.width && y >= 0 && y <= rect.height) {
      net.style.left = x + 'px';
      net.style.top = y + 'px';
      net.classList.add('visible');
      level3NetVisible = true;
    }
  };

  const hideNet = () => {
    net.classList.remove('visible');
    level3NetVisible = false;
  };

  pond.addEventListener('mousemove', moveNet);
  pond.addEventListener('touchmove', moveNet, { passive: false });
  pond.addEventListener('mouseleave', hideNet);
  pond.addEventListener('touchend', hideNet);
}

function animateFish() {
  const pond = document.getElementById('pond');
  if (!pond || state.currentLevel !== 3) return;

  const pondW = pond.clientWidth;
  const pondH = pond.clientHeight;

  level3Fish.forEach(f => {
    if (f.caught) return;
    f.x += f.vx;
    f.y += f.vy;

    // 边界反弹
    if (f.x < 0) { f.x = 0; f.vx *= -1; f.facingRight = true; }
    if (f.x > pondW - 80) { f.x = pondW - 80; f.vx *= -1; f.facingRight = false; }
    if (f.y < 0) { f.y = 0; f.vy *= -1; }
    if (f.y > pondH - 60) { f.y = pondH - 60; f.vy *= -1; }

    // 随机转向
    if (Math.random() < 0.005) { f.vx *= -1; f.facingRight = !f.facingRight; }
    if (Math.random() < 0.005) { f.vy *= -1; }

    f.el.style.left = f.x + 'px';
    f.el.style.top = f.y + 'px';
    f.el.style.transform = f.facingRight ? 'scaleX(1)' : 'scaleX(-1)';

    // 检测渔网碰撞
    if (level3NetVisible && !f.caught && !level3CaughtThisRound && f.word === level3TargetWord) {
      const netEl = pond._netEl || document.getElementById('net-cursor');
      if (netEl && netEl.classList.contains('visible')) {
        const netRect = netEl.getBoundingClientRect();
        const fishRect = f.el.getBoundingClientRect();
        const netCX = netRect.left + netRect.width / 2;
        const netCY = netRect.top + netRect.height / 2;
        const fishCX = fishRect.left + fishRect.width / 2;
        const fishCY = fishRect.top + fishRect.height / 2;
        const dist = Math.hypot(netCX - fishCX, netCY - fishCY);

        if (dist < 45) {
          level3CatchTimer++;
        } else {
          level3CatchTimer = Math.max(0, level3CatchTimer - 1);
        }
        // 持续接触约0.5秒后捕获（约30帧）
        if (level3CatchTimer > 30) {
          catchFish(f);
        }
      }
    }
  });

  level3AnimId = requestAnimationFrame(animateFish);
}

function catchFish(fish) {
  fish.caught = true;
  level3CaughtThisRound = true;
  level3CatchTimer = 0;
  fish.el.classList.add('caught');
  state.score += 20;
  updateAllScoreDisplays();
  speakWord(fish.word);

  setTimeout(() => {
    fish.el.style.visibility = 'hidden';
    level3Caught.push(fish.word);
    document.getElementById('fish-progress').textContent = level3Caught.length + '/6';

    if (level3Caught.length >= WORDS_LEVEL3.length) {
      level3Complete();
    } else {
      pickNextTarget();
    }
  }, 500);
}

function level3Complete() {
  state.level3Complete = true;
  updateDots();
  cancelAnimationFrame(level3AnimId);
  document.getElementById('fish-progress').textContent = '6/6';
  // 显示奖状
  setTimeout(() => {
    showCertificate();
  }, 600);
}

function resetLevel3() {
  cancelAnimationFrame(level3AnimId);
  initLevel3();
}

// ==================== 弹窗与流程控制 ====================
function showModal(title, stars, msg, btnText) {
  document.getElementById('modal-title').textContent = title;
  document.getElementById('modal-stars').textContent = stars;
  document.getElementById('modal-msg').textContent = msg;
  document.getElementById('modal-btn').textContent = btnText;
  document.getElementById('modal-overlay').style.display = 'flex';
}

function advanceLevel() {
  document.getElementById('modal-overlay').style.display = 'none';
  if (state.currentLevel === 1) {
    state.currentLevel = 2;
    showScreen('level2-screen');
    initLevel2();
  } else if (state.currentLevel === 2) {
    // 清理关卡2的定时器
    clearInterval(level2TimerId);
    clearInterval(level2SpawnId);
    level2GameOver = true;
    level2Blocks._animating = false;
    state.currentLevel = 3;
    showScreen('level3-screen');
    initLevel3();
  }
}

function showCertificate() {
  state.currentLevel = 4;
  showScreen('certificate-screen');
  document.getElementById('final-score').textContent = state.score;
  // 庆祝动画
  launchConfetti();
}

function restartAll() {
  cancelAnimationFrame(level3AnimId);
  clearInterval(level2TimerId);
  clearInterval(level2SpawnId);
  level2GameOver = true;
  state.currentLevel = 0;
  showScreen('start-screen');
  updateDots();
  // 清除confetti
  document.querySelectorAll('.confetti-piece').forEach(c => c.remove());
}

// ==================== 简易彩纸庆祝效果 ====================
function launchConfetti() {
  const colors = ['#e74c3c','#f39c12','#2ecc71','#3498db','#9b59b6','#f1c40f','#e67e22','#1abc9c'];
  for (let i = 0; i < 80; i++) {
    const piece = document.createElement('div');
    piece.className = 'confetti-piece';
    piece.style.cssText = `
      position: fixed; top: -20px; left: ${Math.random() * 100}%;
      width: ${6 + Math.random() * 10}px; height: ${6 + Math.random() * 10}px;
      background: ${colors[Math.floor(Math.random() * colors.length)]};
      border-radius: ${Math.random() > 0.5 ? '50%' : '2px'};
      z-index: 9999; pointer-events: none;
      animation: confettiFall ${2 + Math.random() * 3}s ease-in forwards;
      animation-delay: ${Math.random() * 2}s;
      opacity: 0.9;
    `;
    document.body.appendChild(piece);
    setTimeout(() => piece.remove(), 5000);
  }
}
// 动态注入confetti动画
const confettiStyle = document.createElement('style');
confettiStyle.textContent = `
  @keyframes confettiFall {
    0% { transform: translateY(0) rotate(0deg); opacity: 1; }
    100% { transform: translateY(105vh) rotate(720deg); opacity: 0; }
  }
`;
document.head.appendChild(confettiStyle);

// ==================== 通用工具函数 ====================
function shuffleArray(arr) {
  for (let i = arr.length - 1; i > 0; i--) {
    const j = Math.floor(Math.random() * (i + 1));
    [arr[i], arr[j]] = [arr[j], arr[i]];
  }
}

// ==================== 窗口大小变化时重新调整 ====================
window.addEventListener('resize', () => {
  // 调整关卡2方块位置
  if (state.currentLevel === 2) {
    const area = document.getElementById('game-area');
    if (area && level2Blocks.length > 0) {
      level2Blocks.forEach(b => {
        if (b.el.classList.contains('destroyed')) return;
        const areaW = area.clientWidth;
        const currentLeft = parseFloat(b.el.style.left) || 0;
        if (currentLeft > areaW - 60) {
          b.el.style.left = (areaW - 70) + 'px';
        }
      });
    }
  }
  // 调整关卡3鱼的位置
  if (state.currentLevel === 3) {
    const pond = document.getElementById('pond');
    if (pond) {
      const pondW = pond.clientWidth;
      const pondH = pond.clientHeight;
      level3Fish.forEach(f => {
        f.x = Math.min(f.x, pondW - 80);
        f.y = Math.min(f.y, pondH - 60);
        f.el.style.left = f.x + 'px';
        f.el.style.top = f.y + 'px';
      });
    }
  }
});
</script>
</body>
</html>
