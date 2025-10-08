<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Home Game Ras Poker 2025</title>
<link rel="icon" href="https://cdn-icons-png.flaticon.com/512/3141/3141129.png">
<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-firestore-compat.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<style>
/* ====== Estilo Geral ====== */
body {
  font-family: "Trebuchet MS", Arial, sans-serif;
  margin:0; padding:0;
  background: url('https://i.imgur.com/3E9xL3c.jpg') no-repeat center center fixed;
  background-size: cover;
  color:#fff;
  display:flex; flex-direction:column; height:100vh; overflow:hidden;
}
h1,h2{margin:10px 0;text-align:center;}
button{cursor:pointer; font-weight:bold; border:none; border-radius:8px; padding:8px 16px; margin:2px; transition:0.3s;}
button:hover{transform:scale(1.05); opacity:0.9;}

/* ====== Layout Abas ====== */
.tabs{display:flex; justify-content:center; margin-top:10px; gap:10px;}
.tab-btn{background:#3e2723;color:#ffe082; padding:10px 20px; flex:1; text-align:center; font-size:14px;}
.tab-btn.active{background:#ffcc00;color:#3e2723;}
.tab-content{display:none; margin:0 auto; max-width:750px; flex:1; overflow-y:auto; padding:10px; transition:opacity 0.3s;}
.tab-content.active{display:block; opacity:1;}

/* ====== Ranking ====== */
.ranking{background:rgba(93,64,55,0.9); padding:15px; border-radius:15px; border:2px solid #ffcc00; box-shadow:0 4px 10px rgba(0,0,0,0.5);}
.ranking table{width:100%; border-collapse:collapse; font-size:14px;}
.ranking th, .ranking td{padding:8px; text-align:center; border:1px solid #a1887f; color:#000; word-break:break-word;}
.ranking th{background:#ffcc00; color:#000;}
.pos1_8 td{background:#81d4fa; color:#000;}
.pos9_17 td{background:#ff8a80; color:#000;}
td[contenteditable="true"]{background:rgba(255,236,179,0.3); border:1px dashed #ffcc00; border-radius:5px;}
.botoes-container{display:flex; justify-content:center; flex-wrap:wrap; gap:3px;}
.btn-pontos{padding:3px 8px; color:#fff; border-radius:6px; font-size:12px; font-weight:bold; transition:0.2s;}
.btn-pontos:hover{transform:scale(1.1);}
.btn-presenca{background:#4CAF50;}
.btn-segundo{background:#2196F3;}
.btn-terceiro{background:#9C27B0;}
.btn-quarto{background:#FF9800;}
.btn-quinto{background:#795548;}
.btn-campeao{background:#FF5722;}

/* ====== Comandas ====== */
table.comandas{width:100%; border-collapse:collapse; margin-top:10px; font-size:13px;}
table.comandas th{background:#ffcc00; color:#000;}
table.comandas td, table.comandas th{border:1px solid #888; padding:6px 8px; text-align:center; word-break:break-word;}
td[contenteditable="true"]{background:#fffae5; border:1px dashed #ffcc00; border-radius:5px;}
.product-cell{font-weight:bold; color:#fff; padding:4px 6px; border-radius:4px; display:inline-block; margin:1px;}
.product-btn{padding:2px 6px; margin:1px; border-radius:4px; font-weight:bold; font-size:12px; color:#fff; border:none; cursor:pointer; transition:0.2s;}
.product-btn:hover{transform:scale(1.1);}
.status-aberta{background:#81d4fa;}
.status-fechada{background:#ff8a80;}
.produtos-vendidos span{margin:0 2px; padding:2px 4px; border-radius:3px; color:#fff; font-weight:bold; display:inline-block; font-size:12px;}

/* ====== Premiação ====== */
.premiacao{background:rgba(59,42,27,0.9); padding:20px; border-radius:20px; box-shadow:0 4px 10px rgba(0,0,0,0.5);}
.premiacao input{width:100%; padding:12px; border-radius:12px; border:2px solid #d4af37; margin-bottom:10px; text-align:center; font-size:16px;}
.premiacao button{width:100%; padding:12px; border-radius:12px; margin-bottom:6px; background:#2e7d32; color:#fff; font-size:16px; transition:0.3s;}
.premiacao button:hover{transform:scale(1.05);}
#salvarQuinto{background:#c49102;}
#desfazerQuinto{background:#a93226;}
.resultado table{width:100%; border-collapse:collapse; margin-top:10px; font-size:14px;}
.resultado th, .resultado td{border:1px solid #d4af37; padding:8px; color:#000; font-weight:600;}
#mensagem{display:none; position:fixed; top:10%; left:50%; transform:translateX(-50%); background:rgba(212,175,55,0.95); color:#000; font-weight:bold; padding:12px 20px; border-radius:12px; z-index:999; animation:fadein 0.5s;}
@keyframes fadein{0%{opacity:0;}100%{opacity:1;}}

/* ====== Scroll interno suave ====== */
.tab-content::-webkit-scrollbar{width:8px;}
.tab-content::-webkit-scrollbar-thumb{background:rgba(255,204,0,0.6); border-radius:4px;}
.tab-content::-webkit-scrollbar-track{background:rgba(0,0,0,0.2); border-radius:4px;}
</style>
</head>
<body>

<h1>🏠 Home Game Ras Poker 2025</h1>

<div class="tabs">
  <button class="tab-btn active" data-tab="rankingTab">🏆 Ranking</button>
  <button class="tab-btn" data-tab="comandasTab">🍻 Comandas</button>
  <button class="tab-btn" data-tab="premiacaoTab">💰 Premiação</button>
</div>

<!-- ====== Ranking ====== -->
<div class="tab-content active" id="rankingTab">
  <div class="ranking">
    <button onclick="adicionarJogador()">➕ Adicionar Jogador</button>
    <button onclick="salvarRanking()">💾 Salvar Local</button>
    <table id="rankingTable">
      <tr><th>Posição</th><th>Nome</th><th>Pontos</th><th>Presenças</th><th>Vitórias</th><th>Adicionar Pontos</th></tr>
    </table>
  </div>
</div>

<!-- ====== Comandas ====== -->
<div class="tab-content" id="comandasTab">
  <h2>Comandas Abertas</h2>
  <button onclick="adicionarComanda()">➕ Nova Comanda</button>
  <table id="comandaAbertasTable" class="comandas">
    <tr><th>Nome</th><th>Número</th><th>Total</th><th>Finalizar</th><th>Produtos</th><th>Vendidos</th></tr>
  </table>
  <h2>Comandas Fechadas</h2>
  <table id="comandaFechadasTable" class="comandas">
    <tr><th>Nome</th><th>Número</th><th>Total</th><th>Comprovante PNG</th></tr>
  </table>
</div>

<!-- ====== Premiação ====== -->
<div class="tab-content" id="premiacaoTab">
  <div class="premiacao">
    <div id="mensagem">Quinto salvo com sucesso!</div>
    <label>Valor total arrecadado (R$):</label>
    <input type="number" id="valorTotal" placeholder="Digite o valor total">
    <button id="calcular">Calcular Divisão</button>
    <button id="salvarQuinto" disabled>Salvar o Quinto</button>
    <button id="desfazerQuinto" disabled>Desfazer Quinto</button>
    <div id="resultado" class="resultado"></div>
  </div>
</div>

<script>
/* ====== Firebase Config Genérica ====== */
const firebaseConfig = {
  apiKey: "SUA_API_KEY",
  authDomain: "SEU_AUTH_DOMAIN",
  projectId: "SEU_PROJECT_ID",
  storageBucket: "SEU_STORAGE_BUCKET",
  messagingSenderId: "SEU_MESSAGING_SENDER_ID",
  appId: "SEU_APP_ID"
};
firebase.initializeApp(firebaseConfig);
const db = firebase.firestore();

/* ====== Abas ====== */
document.querySelectorAll(".tab-btn").forEach(btn=>{
  btn.addEventListener("click", ()=>{
    document.querySelectorAll(".tab-btn").forEach(b=>b.classList.remove("active"));
    document.querySelectorAll(".tab-content").forEach(t=>t.classList.remove("active"));
    btn.classList.add("active");
    document.getElementById(btn.dataset.tab).classList.add("active");
  });
});

/* ====== Dados ====== */
let jogadores = JSON.parse(localStorage.getItem("rankingJogadores"))||[];
let comandas = JSON.parse(localStorage.getItem("comandas"))||[];
let premiosOriginais = {};
let quintoAdicionado = false;

/* ====== Ranking ====== */
function salvarRanking(){
  localStorage.setItem("rankingJogadores", JSON.stringify(jogadores));
  syncRankingFirestore();
}
function adicionarJogador(){
  let nome=prompt("Digite o nome do novo jogador:");
  if(nome){jogadores.push({nome:nome.trim(), pontos:0, presencas:0, vitorias:0}); atualizarRanking();}
}
function atualizarRanking(){
  const table=document.getElementById("rankingTable");
  table.innerHTML='<tr><th>Posição</th><th>Nome</th><th>Pontos</th><th>Presenças</th><th>Vitórias</th><th>Adicionar Pontos</th></tr>';
  jogadores.sort((a,b)=>b.pontos-a.pontos);
  jogadores.forEach((j,i)=>{
    let row=table.insertRow();
    row.innerHTML=`
      <td>${i+1}</td>
      <td contenteditable="true" oninput="jogadores[${i}].nome=this.innerText">${j.nome}</td>
      <td contenteditable="true" oninput="jogadores[${i}].pontos=parseInt(this.innerText)||0">${j.pontos}</td>
      <td contenteditable="true" oninput="jogadores[${i}].presencas=parseInt(this.innerText)||0">${j.presencas}</td>
      <td contenteditable="true" oninput="jogadores[${i}].vitorias=parseInt(this.innerText)||0">${j.vitorias}</td>
      <td class="botoes-container">
        <button class="btn-pontos btn-presenca" onclick="addPonto(${i},10)">+ Presença</button>
        <button class="btn-pontos btn-campeao" onclick="addPonto(${i},250)">Campeão</button>
        <button class="btn-pontos btn-segundo" onclick="addPonto(${i},150)">2º</button>
        <button class="btn-pontos btn-terceiro" onclick="addPonto(${i},100)">3º</button>
        <button class="btn-pontos btn-quarto" onclick="addPonto(${i},70)">4º</button>
        <button class="btn-pontos btn-quinto" onclick="addPonto(${i},50)">5º</button>
      </td>
    `;
  });
  salvarRanking();
}
function addPonto(idx, pts){
  jogadores[idx].pontos += pts;
  if(pts===10) jogadores[idx].presencas+=1;
  atualizarRanking();
}

/* ====== Comandas ====== */
function adicionarComanda(){
  let nome=prompt("Nome do cliente:");
  let numero=prompt("Número da comanda:");
  if(nome && numero){
    comandas.push({nome,n:numero,total:0,produtos:[],vendidos:[],status:"aberta"});
    atualizarComandas();
  }
}
function atualizarComandas(){
  const abertas = document.getElementById("comandaAbertasTable");
  const fechadas = document.getElementById("comandaFechadasTable");
  abertas.innerHTML='<tr><th>Nome</th><th>Número</th><th>Total</th><th>Finalizar</th><th>Produtos</th><th>Vendidos</th></tr>';
  fechadas.innerHTML='<tr><th>Nome</th><th>Número</th><th>Total</th><th>Comprovante PNG</th></tr>';
  comandas.forEach((c,i)=>{
    if(c.status==="aberta"){
      let row = abertas.insertRow();
      row.innerHTML=`
        <td>${c.nome}</td>
        <td>${c.n}</td>
        <td>${c.total}</td>
        <td><button onclick="finalizarComanda(${i})">✅</button></td>
        <td contenteditable="true" oninput="c.produtos=this.innerText.split(',').map(x=>x.trim())">${c.produtos.join(", ")}</td>
        <td class="produtos-vendidos">${c.vendidos.map(p=>`<span>${p}</span>`).join("")}</td>
      `;
    }else{
      let row = fechadas.insertRow();
      row.innerHTML=`
        <td>${c.nome}</td>
        <td>${c.n}</td>
        <td>${c.total}</td>
        <td><button onclick="exportComandaPNG(${i})">📸</button></td>
      `;
    }
  });
  localStorage.setItem("comandas", JSON.stringify(comandas));
  syncComandasFirestore();
}
function finalizarComanda(idx){
  comandas[idx].status="fechada";
  atualizarComandas();
}
function exportComandaPNG(idx){
  html2canvas(document.body).then(canvas=>{
    let a=document.createElement("a");
    a.href=canvas.toDataURL();
    a.download=`comanda_${comandas[idx].nome}.png`;
    a.click();
  });
}

/* ====== Premiação ====== */
document.getElementById("calcular").addEventListener("click",()=>{
  let total = parseFloat(document.getElementById("valorTotal").value)||0;
  let resultado = document.getElementById("resultado");
  let divisao = {
    campeao:250,
    segundo:150,
    terceiro:100,
    quarto:70,
    quinto:50,
    sexto:40,
    setimo:30,
    oitavo:20,
    nono:10
  };
  // Corrige diluição do quinto
  let somaFixo=250+150+100+70+50; 
  let restante=total-somaFixo; 
  let div = {...divisao};
  if(restante<0) restante=0; 
  div.quinto+=restante/5;
  premiosOriginais=div;
  quintoAdicionado=false;
  document.getElementById("salvarQuinto").disabled=false;
  document.getElementById("desfazerQuinto").disabled=false;
  resultado.innerHTML=`
    <table>
      <tr><th>Posição</th><th>Valor (R$)</th></tr>
      ${Object.keys(div).map(k=>`<tr><td>${k}</td><td>${div[k].toFixed(2)}</td></tr>`).join("")}
    </table>
  `;
});
document.getElementById("salvarQuinto").addEventListener("click",()=>{
  if(quintoAdicionado) return;
  let div = {...premiosOriginais};
  div.quinto=0;
  quintoAdicionado=true;
  document.getElementById("mensagem").style.display="block";
  setTimeout(()=>document.getElementById("mensagem").style.display="none",2000);
});
document.getElementById("desfazerQuinto").addEventListener("click",()=>{
  quintoAdicionado=false;
  document.getElementById("mensagem").style.display="none";
});

/* ====== Firebase Realtime ====== */
function syncRankingFirestore(){
  db.collection("ranking").doc("principal").set({jogadores})
    .then(()=>console.log("✅ Ranking Firestore atualizado"))
    .catch(err=>console.error(err));
}
function syncComandasFirestore(){
  db.collection("comandas").doc("principal").set({comandas})
    .then(()=>console.log("✅ Comandas Firestore atualizadas"))
    .catch(err=>console.error(err));
}
/* Escuta em tempo real */
db.collection("ranking").doc("principal").onSnapshot(doc=>{
  if(doc.exists){
    jogadores=doc.data().jogadores||[];
    atualizarRanking();
  }
});
db.collection("comandas").doc("principal").onSnapshot(doc=>{
  if(doc.exists){
    comandas=doc.data().comandas||[];
    atualizarComandas();
  }
});

/* Inicializa visualizações */
atualizarRanking();
atualizarComandas();
</script>

</body>
</html>
