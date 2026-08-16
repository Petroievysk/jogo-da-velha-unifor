<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Universidade de Fortaleza — Jogo da Velha</title>
<style>
  :root{
    --unifor-blue:#003366;
    --highlight:#00566b;
    --orange:#f28c28;
    --bg:#f4f6f9;
    --white:#ffffff;
    --text:#18344d;
    --line:#173f5f;
    --muted:#64717d;
    --shadow:0 10px 28px rgba(0,51,102,.10);
  }

  *{box-sizing:border-box}

  body{
    margin:0;
    min-height:100vh;
    font-family:Arial, Helvetica, sans-serif;
    background:var(--bg);
    color:var(--text);
  }

  .topbar{
    height:116px;
    background:#fff;
    display:flex;
    align-items:center;
    padding:0 32px;
    border-bottom:1px solid #e7ebef;
  }

  .brand{
    display:flex;
    align-items:center;
    gap:13px;
    color:var(--unifor-blue);
    font-weight:700;
    font-size:22px;
    letter-spacing:.2px;
  }

  .brand-mark{
    width:43px;
    height:48px;
    display:flex;
    align-items:center;
    justify-content:center;
    flex:0 0 auto;
    overflow:hidden;
  }

  .brand-mark img{
    width:43px;
    height:46px;
    object-fit:contain;
    display:block;
  }

  main{
    max-width:900px;
    margin:0 auto;
    padding:18px 20px 36px;
  }

  .title{
    text-align:center;
    color:var(--unifor-blue);
    font-size:35px;
    line-height:1.1;
    margin:0 0 16px;
    font-weight:800;
    letter-spacing:.4px;
  }

  .controls{
    display:flex;
    justify-content:center;
    gap:18px;
    flex-wrap:wrap;
    margin-bottom:17px;
  }

  .control-group{
    text-align:center;
  }

  .control-label{
    display:block;
    font-size:14px;
    margin-bottom:7px;
    color:#222;
    letter-spacing:.2px;
  }

  .segmented{
    display:flex;
  }

  .segmented button{
    appearance:none;
    border:1px solid #8da1b0;
    background:#f9fbfc;
    color:#163c58;
    padding:7px 13px;
    font-size:14px;
    cursor:pointer;
    min-height:33px;
    transition:.18s ease;
  }

  .segmented button:first-child{border-radius:5px 0 0 5px}
  .segmented button:last-child{border-radius:0 5px 5px 0}
  .segmented button + button{border-left:0}

  .segmented button.active{
    background:var(--unifor-blue);
    color:#fff;
    border-color:var(--unifor-blue);
    box-shadow:0 3px 8px rgba(0,51,102,.16);
  }

  .game-layout{
    display:flex;
    flex-direction:column;
    align-items:center;
  }

  .board-wrap{
    position:relative;
    margin-top:2px;
  }

  .board{
    width:265px;
    height:265px;
    display:grid;
    grid-template-columns:repeat(3,1fr);
    grid-template-rows:repeat(3,1fr);
    border:3px solid var(--line);
    border-radius:5px;
    overflow:hidden;
    background:#fff;
  }

  .cell{
    border:0;
    border-right:3px solid var(--line);
    border-bottom:3px solid var(--line);
    background:#fff;
    cursor:pointer;
    display:flex;
    align-items:center;
    justify-content:center;
    position:relative;
    transition:background .15s ease;
  }

  .cell:nth-child(3n){border-right:0}
  .cell:nth-child(n+7){border-bottom:0}

  .cell:not(.filled):hover{
    background:#f1f7fa;
  }

  .mark{
    font-size:63px;
    font-weight:400;
    line-height:1;
    user-select:none;
  }

  .mark.x{color:var(--unifor-blue)}
  .mark.o{color:var(--orange)}

  .win-cell{
    background:#eef5f8 !important;
  }

  .status-overlay{
    position:absolute;
    left:50%;
    top:50%;
    transform:translate(-50%,-50%);
    z-index:5;
    min-width:190px;
    text-align:center;
    color:#fff;
    background:rgba(32,32,32,.88);
    border-radius:5px;
    padding:9px 13px;
    font-size:16px;
    font-weight:700;
    box-shadow:0 5px 16px rgba(0,0,0,.18);
    display:none;
  }

  .status-overlay.show{display:block}

  .status-overlay .small{
    display:block;
    font-size:11px;
    margin-top:2px;
    opacity:.9;
    font-weight:500;
  }

  .score-area{
    width:510px;
    max-width:100%;
    display:grid;
    grid-template-columns:1fr 130px 1fr;
    align-items:center;
    gap:10px;
    margin-top:17px;
  }

  .score-card{
    min-height:89px;
    border:1px solid #dce3e8;
    background:rgba(255,255,255,.32);
    border-radius:5px;
    text-align:center;
    padding:10px 8px;
  }

  .score-name{
    font-size:14px;
    color:#222;
    margin-bottom:4px;
  }

  .score{
    font-size:17px;
    font-weight:700;
    color:var(--unifor-blue);
    line-height:1.15;
  }

  .score-card.o-card .score{
    color:var(--orange);
  }

  .round-info{
    text-align:center;
    font-size:15px;
    color:#222;
  }

  .round-number{
    display:block;
    font-size:20px;
    color:#333;
    margin:4px 0;
  }

  .round-format{
    font-size:12px;
    color:#222;
  }

  .turn-message{
    width:510px;
    max-width:100%;
    border:1px solid #dce3e8;
    background:rgba(255,255,255,.28);
    border-radius:5px;
    text-align:center;
    padding:10px;
    margin-top:10px;
    font-size:16px;
    color:#222;
    min-height:40px;
  }

  .turn-message.o-turn{
    color:var(--orange);
    font-weight:700;
  }

  .reset{
    width:510px;
    max-width:100%;
    margin-top:12px;
    border:0;
    border-radius:4px;
    background:var(--unifor-blue);
    color:#fff;
    padding:9px 16px;
    font-size:16px;
    cursor:pointer;
    transition:.18s ease;
  }

  .reset:hover{
    background:#004679;
    transform:translateY(-1px);
  }

  .reset:active{transform:translateY(0)}

  .result-note{
    text-align:center;
    margin-top:8px;
    min-height:18px;
    color:var(--muted);
    font-size:12px;
  }

  @media (max-width:600px){
    .topbar{height:88px;padding:0 18px}
    .brand{font-size:17px}
    .brand-mark{width:35px;height:40px}.brand-mark img{width:35px;height:39px}
    main{padding:18px 12px 28px}
    .title{font-size:29px}
    .board{width:min(82vw,265px);height:min(82vw,265px)}
    .score-area{grid-template-columns:1fr 105px 1fr;gap:5px}
    .mark{font-size:52px}
  }
</style>
</head>
<body>
<header class="topbar">
  <div class="brand">
    <div class="brand-mark" aria-hidden="true"><img src="data:image/png;base64,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" alt=""></div>
    <span>UNIVERSIDADE DE FORTALEZA</span>
  </div>
</header>

<main>
  <h1 class="title">JOGO DA VELHA</h1>

  <section class="controls" aria-label="Configurações da partida">
    <div class="control-group">
      <span class="control-label">MODO DE JOGO</span>
      <div class="segmented">
        <button type="button" id="mode2" class="active">2 JOGADORES</button>
        <button type="button" id="modeAI">COMPUTADOR (IA)</button>
      </div>
    </div>

    <div class="control-group">
      <span class="control-label">FORMATO DE PARTIDA</span>
      <div class="segmented">
        <button type="button" id="single" class="active">PARTIDA ÚNICA</button>
        <button type="button" id="best3">MELHOR DE 3</button>
      </div>
    </div>
  </section>

  <section class="game-layout">
    <div class="board-wrap">
      <div class="board" id="board" role="grid" aria-label="Tabuleiro do jogo da velha">
        <button class="cell" data-index="0" aria-label="Casa 1"></button>
        <button class="cell" data-index="1" aria-label="Casa 2"></button>
        <button class="cell" data-index="2" aria-label="Casa 3"></button>
        <button class="cell" data-index="3" aria-label="Casa 4"></button>
        <button class="cell" data-index="4" aria-label="Casa 5"></button>
        <button class="cell" data-index="5" aria-label="Casa 6"></button>
        <button class="cell" data-index="6" aria-label="Casa 7"></button>
        <button class="cell" data-index="7" aria-label="Casa 8"></button>
        <button class="cell" data-index="8" aria-label="Casa 9"></button>
      </div>
      <div class="status-overlay" id="overlay">
        <span id="overlayText"></span>
        <span class="small" id="overlaySub"></span>
      </div>
    </div>

    <div class="score-area">
      <div class="score-card">
        <div class="score-name">JOGADOR X</div>
        <div class="score" id="scoreX">0 – 0</div>
        <div class="round-format" id="scoreXLabel">Rounds</div>
        <div class="round-format" id="scoreXMode">Partida única</div>
      </div>

      <div class="round-info">
        <span>Rodada</span>
        <span class="round-number" id="roundNumber">1</span>
        <span class="round-format" id="bestLabel">Partida única</span>
      </div>

      <div class="score-card o-card">
        <div class="score-name" id="oName">JOGADOR O</div>
        <div class="score" id="scoreO">0 – 0</div>
        <div class="round-format" id="scoreOLabel">Rounds</div>
        <div class="round-format" id="scoreOMode">Partida única</div>
      </div>
    </div>

    <div class="turn-message" id="turnMessage">Vez do Jogador X</div>
    <button class="reset" id="reset">REINICIAR JOGO</button>
    <div class="result-note" id="resultNote"></div>
  </section>
</main>

<script>
(() => {
  "use strict";

  const boardEl = document.getElementById("board");
  const cells = [...document.querySelectorAll(".cell")];
  const mode2Btn = document.getElementById("mode2");
  const modeAIBtn = document.getElementById("modeAI");
  const singleBtn = document.getElementById("single");
  const best3Btn = document.getElementById("best3");
  const resetBtn = document.getElementById("reset");
  const scoreXEl = document.getElementById("scoreX");
  const scoreOEl = document.getElementById("scoreO");
  const oNameEl = document.getElementById("oName");
  const scoreXMode = document.getElementById("scoreXMode");
  const scoreOMode = document.getElementById("scoreOMode");
  const bestLabel = document.getElementById("bestLabel");
  const roundNumberEl = document.getElementById("roundNumber");
  const turnMessageEl = document.getElementById("turnMessage");
  const resultNoteEl = document.getElementById("resultNote");
  const overlayEl = document.getElementById("overlay");
  const overlayText = document.getElementById("overlayText");
  const overlaySub = document.getElementById("overlaySub");

  let mode = "2p";
  let format = "single";
  let board = Array(9).fill("");
  let currentPlayer = "X";
  let round = 1;
  let winsX = 0;
  let winsO = 0;
  let roundWinsX = 0;
  let roundWinsO = 0;
  let roundDraws = 0;
  let gameOver = false;
  let aiTimer = null;
  let audioCtx = null;

  const WIN_LINES = [
    [0,1,2],[3,4,5],[6,7,8],
    [0,3,6],[1,4,7],[2,5,8],
    [0,4,8],[2,4,6]
  ];

  function getAudioContext(){
    if(!audioCtx){
      const Ctx = window.AudioContext || window.webkitAudioContext;
      if(Ctx) audioCtx = new Ctx();
    }
    if(audioCtx && audioCtx.state === "suspended") audioCtx.resume();
    return audioCtx;
  }

  function tone(freq, duration, type="sine", volume=0.045, delay=0){
    const ctx = getAudioContext();
    if(!ctx) return;
    const osc = ctx.createOscillator();
    const gain = ctx.createGain();
    const now = ctx.currentTime + delay;
    osc.type = type;
    osc.frequency.setValueAtTime(freq, now);
    gain.gain.setValueAtTime(0.0001, now);
    gain.gain.exponentialRampToValueAtTime(volume, now + 0.008);
    gain.gain.exponentialRampToValueAtTime(0.0001, now + duration);
    osc.connect(gain);
    gain.connect(ctx.destination);
    osc.start(now);
    osc.stop(now + duration + 0.02);
  }

  function soundX(){
    tone(440, .10, "triangle", .035);
    tone(660, .12, "triangle", .025, .045);
  }

  function soundO(){
    tone(330, .12, "sine", .035);
    tone(495, .13, "sine", .025, .05);
  }

  function soundWin(){
    tone(523.25, .12, "triangle", .045);
    tone(659.25, .12, "triangle", .045, .12);
    tone(783.99, .20, "triangle", .05, .24);
  }

  function soundDraw(){
    tone(392, .16, "sine", .035);
    tone(349.23, .22, "sine", .03, .17);
  }

  function checkResult(state){
    for(const line of WIN_LINES){
      const [a,b,c] = line;
      if(state[a] && state[a] === state[b] && state[a] === state[c]){
        return {winner:state[a], line};
      }
    }
    if(state.every(Boolean)) return {winner:"draw", line:[]};
    return null;
  }

  function render(){
    cells.forEach((cell, i) => {
      cell.innerHTML = board[i] ? `<span class="mark ${board[i].toLowerCase()}">${board[i]}</span>` : "";
      cell.classList.toggle("filled", Boolean(board[i]));
      cell.disabled = Boolean(board[i]) || gameOver || (mode === "ai" && currentPlayer === "O");
    });

    scoreXEl.textContent = `${winsX} – ${winsO}`;
    scoreOEl.textContent = `${winsO} – ${winsX}`;

    const formatText = format === "best3" ? "Melhor de 3" : "Partida única";
    scoreXMode.textContent = formatText;
    scoreOMode.textContent = formatText;
    bestLabel.textContent = formatText;
    roundNumberEl.textContent = round;

    oNameEl.textContent = mode === "ai" ? "COMPUTADOR (IA)" : "JOGADOR O";

    if(!gameOver){
      turnMessageEl.textContent = currentPlayer === "X"
        ? "Vez do Jogador X"
        : (mode === "ai" ? "Vez do Computador (IA)" : "Vez do Jogador O");
      turnMessageEl.classList.toggle("o-turn", currentPlayer === "O");
    }
  }

  function makeMove(index, player){
    if(gameOver || board[index]) return;
    board[index] = player;
    player === "X" ? soundX() : soundO();
    render();

    const result = checkResult(board);
    if(result){
      finishRound(result);
      return;
    }

    currentPlayer = player === "X" ? "O" : "X";
    render();

    if(mode === "ai" && currentPlayer === "O" && !gameOver){
      clearTimeout(aiTimer);
      aiTimer = setTimeout(aiMove, 400);
    }
  }

  function aiMove(){
    if(gameOver || currentPlayer !== "O") return;
    const empty = board.map((v,i) => v ? null : i).filter(i => i !== null);
    if(!empty.length) return;
    const choice = empty[Math.floor(Math.random() * empty.length)];
    makeMove(choice, "O");
  }

  function finishRound(result){
    gameOver = true;

    if(result.winner === "draw"){
      roundDraws++;
      soundDraw();
      showOverlay("EMPATE!", "Rodada encerrada");
      resultNoteEl.textContent = "A rodada terminou em empate.";
    }else{
      const winnerName = result.winner === "X"
        ? "JOGADOR X"
        : (mode === "ai" ? "COMPUTADOR (IA)" : "JOGADOR O");

      if(result.winner === "X"){
        winsX++;
        roundWinsX++;
      }else{
        winsO++;
        roundWinsO++;
      }

      result.line.forEach(i => cells[i].classList.add("win-cell"));
      soundWin();
      showOverlay(`${winnerName} VENCEU!`, "Rodada encerrada");
      resultNoteEl.textContent = `${winnerName} venceu a rodada.`;
    }

    render();

    if(format === "best3" && (roundWinsX >= 2 || roundWinsO >= 2)){
      const seriesWinner = roundWinsX >= 2 ? "JOGADOR X"
        : (mode === "ai" ? "COMPUTADOR (IA)" : "JOGADOR O");
      setTimeout(() => {
        showOverlay(`${seriesWinner} VENCEU!`, "Melhor de 3 concluído");
        resultNoteEl.textContent = `Fim da melhor de 3: ${seriesWinner}. Clique em REINICIAR JOGO para zerar tudo.`;
      }, 900);
      return;
    }

    setTimeout(nextRound, 1200);
  }

  function showOverlay(main, sub){
    overlayText.textContent = main;
    overlaySub.textContent = sub;
    overlayEl.classList.add("show");
  }

  function hideOverlay(){
    overlayEl.classList.remove("show");
  }

  function nextRound(){
    if(format === "best3" && (roundWinsX >= 2 || roundWinsO >= 2)) return;

    round++;
    board = Array(9).fill("");
    currentPlayer = "X";
    gameOver = false;
    cells.forEach(c => c.classList.remove("win-cell"));
    hideOverlay();
    resultNoteEl.textContent = "";
    render();
  }

  function fullReset(){
    clearTimeout(aiTimer);
    board = Array(9).fill("");
    currentPlayer = "X";
    round = 1;
    winsX = 0;
    winsO = 0;
    roundWinsX = 0;
    roundWinsO = 0;
    roundDraws = 0;
    gameOver = false;
    cells.forEach(c => c.classList.remove("win-cell"));
    hideOverlay();
    resultNoteEl.textContent = "";
    render();
  }

  function changeMode(newMode){
    mode = newMode;
    mode2Btn.classList.toggle("active", mode === "2p");
    modeAIBtn.classList.toggle("active", mode === "ai");
    fullReset();
  }

  function changeFormat(newFormat){
    format = newFormat;
    singleBtn.classList.toggle("active", format === "single");
    best3Btn.classList.toggle("active", format === "best3");
    fullReset();
  }

  cells.forEach(cell => {
    cell.addEventListener("click", () => {
      const index = Number(cell.dataset.index);
      makeMove(index, currentPlayer);
    });
  });

  mode2Btn.addEventListener("click", () => changeMode("2p"));
  modeAIBtn.addEventListener("click", () => changeMode("ai"));
  singleBtn.addEventListener("click", () => changeFormat("single"));
  best3Btn.addEventListener("click", () => changeFormat("best3"));
  resetBtn.addEventListener("click", fullReset);

  // Inicialização: estado limpo, X começa.
  render();
})();
</script>
</body>
</html>
