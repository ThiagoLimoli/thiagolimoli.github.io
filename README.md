<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<title>LC Jurista - Sistema Completo de Empréstimos</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<style>
body {font-family: Arial; background: linear-gradient(135deg,#1c1c1c,#2e2e2e); color:#f5f5f5; margin:0; padding:20px;}
h1 {text-align:center;color:#ffd700;margin-bottom:10px;}
.resumo {display:flex;justify-content:space-around;margin-bottom:15px;font-weight:bold;
  background:linear-gradient(135deg,#333,#444);padding:10px;border-radius:10px;box-shadow:0 0 10px rgba(0,0,0,0.6);}
table {width:100%;border-collapse:collapse;margin-top:10px;border-radius:12px;overflow:hidden;box-shadow:0 0 15px rgba(0,0,0,0.8);}
th,td {padding:10px;text-align:center;}
th {background:linear-gradient(135deg,#444,#222);color:#ffd700;cursor:pointer;}
tr {background:#2c2c2c;}
tr:nth-child(even) {background:#3a3a3a;}
.inadimplente {background:linear-gradient(135deg,#5a0000,#330000)!important;color:#fff;font-weight:bold;}
.adimplente {background:linear-gradient(135deg,#003300,#005500)!important;color:#d4ffd4;}
.reativado {background:linear-gradient(135deg,#003366,#005599)!important;color:#fff;}
button {padding:5px 10px;margin:2px;border:none;border-radius:6px;cursor:pointer;font-weight:bold;}
button:hover {opacity:0.9;}
.btn-add{background:#0066cc;color:#fff;}
.btn-pay{background:#ff8800;color:#fff;}
.btn-quit{background:#00aa00;color:#fff;}
.btn-react{background:#aa00aa;color:#fff;}
.btn-png{background:#4444aa;color:#fff;}
.btn-all{background:#ff0066;color:#fff;}
#loginOverlay{position:fixed;top:0;left:0;width:100%;height:100%;background:rgba(0,0,0,0.95);display:flex;align-items:center;justify-content:center;z-index:1000;}
#loginBox{background:#222;padding:30px;border-radius:12px;box-shadow:0 0 20px #000;text-align:center;}
#loginBox input{margin:5px;padding:5px;width:200px;}
#charts {display:flex;justify-content:space-around;margin-top:20px;flex-wrap:wrap;}
canvas{background:#fff;border-radius:10px;padding:10px;margin:10px;}
.filtros {margin-bottom:10px;text-align:center;}
.darkMode {background: #121212; color: #fff;}
</style>
</head>
<body>

<div id="loginOverlay">
  <div id="loginBox">
    <h2>Login LC Jurista</h2>
    <input type="text" id="user" placeholder="Usuário"><br>
    <input type="password" id="pass" placeholder="Senha"><br>
    <button onclick="login()">Entrar</button>
  </div>
</div>

<h1>LC Jurista - Sistema Completo de Empréstimos</h1>

<div class="resumo">
  <div>Total Emprestado: R$ <span id="totalEmprestado">0</span></div>
  <div>Total Retorno Juros: R$ <span id="totalJuros">0</span></div>
  <div>Total Retorno Diárias: R$ <span id="totalDiarias">0</span></div>
  <div>Total Pago: R$ <span id="totalPago">0</span></div>
</div>

<div class="filtros">
  <input type="text" id="buscaCliente" placeholder="Buscar cliente..." onkeyup="filtrar()">
  <button onclick="toggleTheme()">🌗 Tema Claro/Escuro</button>
</div>

<button class="btn-add" onclick="adicionarCliente()">+ Adicionar Cliente</button>
<button class="btn-all" onclick="gerarRelatorioGeral()">📊 Relatório Consolidado</button>
<button class="btn-all" onclick="exportExcel()">📥 Exportar Excel</button>

<table id="tabelaClientes">
<thead>
<tr>
<th onclick="ordenarTabela(0)">Nome</th>
<th>CPF</th>
<th>Endereço</th>
<th>Telefone</th>
<th onclick="ordenarTabela(4)">Valor (R$)</th>
<th>Juros (%)</th>
<th>Total Inicial (R$)</th>
<th>Data Inicial</th>
<th>Dias Atraso</th>
<th>Multa (R$)</th>
<th>Total Atual (R$)</th>
<th>Score</th>
<th>Ações</th>
</tr>
</thead>
<tbody></tbody>
</table>

<div id="charts">
  <canvas id="graficoStatus" width="400" height="300"></canvas>
  <canvas id="graficoScore" width="400" height="300"></canvas>
</div>

<script>
/* ===== LOGIN ===== */
function login() {
  const user=document.getElementById('user').value;
  const pass=document.getElementById('pass').value;
  if(user==='lcjurista' && pass==='1234'){document.getElementById('loginOverlay').style.display='none';}
  else{alert('Usuário ou senha incorretos');}
}

/* ===== DADOS ===== */
let clientes=[
  {nome:"João da Silva",cpf:"12345678909",endereco:"Rua A, 123",telefone:"(11)98888-1111",emprestimos:[{valor:1000,juros:30,data:"2025-08-20",historico:[]}],status:"ativo",obs:""},
  {nome:"Maria Oliveira",cpf:"98765432100",endereco:"Av. B, 456",telefone:"(11)97777-2222",emprestimos:[{valor:2000,juros:30,data:"2025-08-15",historico:[]}],status:"ativo",obs:""}
];

let dark=false;

/* ===== VALIDAÇÃO CPF ===== */
function validarCPF(cpf) {
  cpf = cpf.replace(/[^\d]+/g,''); if(cpf.length!==11) return false;
  if (/^(\d)\1+$/.test(cpf)) return false;
  let soma=0; for(let i=0;i<9;i++) soma+=parseInt(cpf.charAt(i))*(10-i);
  let resto=(soma*10)%11; if(resto===10) resto=0; if(resto!==parseInt(cpf.charAt(9))) return false;
  soma=0; for(let i=0;i<10;i++) soma+=parseInt(cpf.charAt(i))*(11-i);
  resto=(soma*10)%11; if(resto===10) resto=0; if(resto!==parseInt(cpf.charAt(10))) return false;
  return true;
}
function cpfDuplicado(cpf){return clientes.some(c=>c.cpf===cpf);}

/* ===== SCORE ===== */
function calcularScore(historico){
  if(historico.length===0) return 50;
  let pontos=50; historico.forEach(p=>{if(p.pagoNoPrazo)pontos+=10;else pontos-=5;});
  return Math.max(0,Math.min(100,pontos));
}

/* ===== CALCULO ATRASO ===== */
function calcularAtraso(cliente,hoje){
  let atrasoTotal=0;
  cliente.emprestimos.forEach(e=>{
    const dataInicial=new Date(e.data);
    const diasPassados=Math.floor((hoje-dataInicial)/(1000*60*60*24));
    if(diasPassados>30) atrasoTotal+=diasPassados-30;
  });
  return atrasoTotal;
}

/* ===== ATUALIZAR TABELA ===== */
function atualizarTabela(){
  const tbody=document.querySelector("#tabelaClientes tbody");
  tbody.innerHTML="";
  let totalEmprestado=0,totalJuros=0,totalDiarias=0,totalPago=0;
  const hoje=new Date();
  clientes.sort((a,b)=>calcularAtraso(b,hoje)-calcularAtraso(a,hoje));
  clientes.forEach((c,i)=>{
    let valorTotal=0,jurosTotal=0,multaTotal=0,totalAtual=0,scoreTotal=0,diasAtraso=0;
    c.emprestimos.forEach(e=>{
      const valor=parseFloat(e.valor);
      const juros=valor*(e.juros/100);
      const totalInicial=valor+juros;
      const dataInicial=new Date(e.data);
      const diasPassados=Math.floor((hoje-dataInicial)/(1000*60*60*24));
      let multa=0; if(diasPassados>30) multa=(diasPassados-30)*30;
      valorTotal+=valor; jurosTotal+=juros; multaTotal+=multa; totalAtual+=totalInicial+multa; diasAtraso+=diasPassados>30?diasPassados-30:0;
      e.historico.forEach(h=>totalPago+=h.valor);
    });
    const tr=document.createElement("tr");
    if(c.status==="quitado") tr.className="adimplente";
    else if(c.status==="reativado") tr.className="reativado";
    else tr.className=diasAtraso>0?"inadimplente":"adimplente";
    tr.innerHTML=`
      <td contenteditable="true">${c.nome}</td>
      <td>${c.cpf}</td>
      <td contenteditable="true">${c.endereco}</td>
      <td contenteditable="true">${c.telefone}</td>
      <td>${valorTotal.toFixed(2)}</td>
      <td>${c.emprestimos.map(e=>e.juros).join(",")}</td>
      <td>${(valorTotal+jurosTotal).toFixed(2)}</td>
      <td>${c.emprestimos.map(e=>new Date(e.data).toLocaleDateString("pt-BR")).join(", ")}</td>
      <td>${diasAtraso}</td>
      <td>${multaTotal.toFixed(2)}</td>
      <td>${totalAtual.toFixed(2)}</td>
      <td>${calcularScore([].concat(...c.emprestimos.map(e=>e.historico)))}</td>
      <td>
        ${c.status!=="quitado"?`<button class="btn-pay" onclick="registrarPagamento(${i})">Pagamento</button>`:""}
        ${c.status!=="quitado"?`<button class="btn-quit" onclick="quitar(${i})">Quitar</button>`:""}
        ${c.status==="quitado"?`<button class="btn-react" onclick="reativar(${i})">Reativar</button>`:""}
        <button class="btn-png" onclick="gerarPNG(${i})">PNG</button>
      </td>`;
    tbody.appendChild(tr);
    totalEmprestado+=valorTotal; totalJuros+=jurosTotal; totalDiarias+=multaTotal;
  });
  document.getElementById("totalEmprestado").textContent=totalEmprestado.toFixed(2);
  document.getElementById("totalJuros").textContent=totalJuros.toFixed(2);
  document.getElementById("totalDiarias").textContent=totalDiarias.toFixed(2);
  document.getElementById("totalPago").textContent=totalPago.toFixed(2);
  atualizarGraficos();
}

/* ===== FUNÇÕES PAGAMENTO ===== */
function registrarPagamento(i){
  const cliente=clientes[i];
  const valorPago=parseFloat(prompt("Digite o valor pago pelo cliente:"));
  if(isNaN(valorPago)||valorPago<=0) return;
  const emprestimoAtual=cliente.emprestimos[cliente.emprestimos.length-1];
  if(valorPago>=emprestimoAtual.valor*(emprestimoAtual.juros/100)){
    if(confirm("Pagamento maior que juros detectado. Reiniciar 30 dias?")) emprestimoAtual.data=new Date().toISOString().split("T")[0];
    emprestimoAtual.historico.push({valor:valorPago,pagoNoPrazo:true});
  } else emprestimoAtual.historico.push({valor:valorPago,pagoNoPrazo:false});
  atualizarTabela();
}
function quitar(i){ if(confirm("Deseja quitar esta dívida?")) {clientes[i].status="quitado"; atualizarTabela();} }
function reativar(i){
  const cliente=clientes[i];
  const novoValor=parseFloat(prompt("Digite o novo valor do empréstimo para reativação:"));
  if(isNaN(novoValor)||novoValor<=0) return alert("Valor inválido");
  cliente.emprestimos.push({valor:novoValor,juros:30,data:new Date().toISOString().split("T")[0],historico:[]});
  cliente.status="reativado";
  atualizarTabela();
}

/* ===== ADICIONAR CLIENTE ===== */
function adicionarCliente(){
  const nome=prompt("Nome do cliente:");
  const cpf=prompt("CPF do cliente:");
  if(!validarCPF(cpf)) return alert("CPF inválido");
  if(cpfDuplicado(cpf)) return alert("CPF já existe no sistema");
  const endereco=prompt("Endereço do cliente:");
  const telefone=prompt("Telefone do cliente:");
  const valor=parseFloat(prompt("Valor emprestado:"));
  const juros=parseFloat(prompt("Juros (%) do empréstimo:","30"));
  const data=prompt("Data inicial (AAAA-MM-DD):",new Date().toISOString().split("T")[0]);
  if(nome && endereco && telefone && !isNaN(valor) && !isNaN(juros)){
    clientes.push({nome,cpf,endereco,telefone,emprestimos:[{valor,juros,data,historico:[]}],status:"ativo",obs:""});
    atualizarTabela();
  }
}

/* ===== RELATORIOS E PNG ===== */
function gerarPNG(i){
  const cliente=clientes[i];
  const div=document.createElement("div");
  div.style.padding="20px"; div.style.background="#fff"; div.style.color="#000";
  div.innerHTML=`<h2>Relatório de ${cliente.nome}</h2>
    <p>CPF: ${cliente.cpf}</p>
    <p>Endereço: ${cliente.endereco}</p>
    <p>Telefone: ${cliente.telefone}</p>
    <p>Empréstimos:</p>
    <ul>${cliente.emprestimos.map(e=>`<li>Valor: R$ ${e.valor}, Juros: ${e.juros}%, Data: ${e.data}</li>`).join('')}</ul>`;
  document.body.appendChild(div);
  html2canvas(div).then(canvas=>{const img=canvas.toDataURL("image/png");const novaAba=window.open();novaAba.document.write('<img src="'+img+'"/>');document.body.removeChild(div);});
}

function gerarRelatorioGeral(){
  const hoje=new Date();let totalEmprestado=0,totalJuros=0,totalDiarias=0;
  clientes.forEach(c=>c.emprestimos.forEach(e=>{
    const valor=parseFloat(e.valor); const juros=valor*(e.juros/100);
    const dataInicial=new Date(e.data); const diasPassados=Math.floor((hoje-dataInicial)/(1000*60*60*24));
    let multa=0;if(diasPassados>30)multa=(diasPassados-30)*30;
    totalEmprestado+=valor; totalJuros+=juros; totalDiarias+=multa;
  }));
  const div=document.createElement("div"); div.style.padding="20px"; div.style.background="#fff"; div.style.color="#000";
  let html=`<h2>Relatório Consolidado - ${hoje.toLocaleDateString("pt-BR")}</h2>`;
  html+=`<p><b>Total Emprestado:</b> R$ ${totalEmprestado.toFixed(2)}</p>`;
  html+=`<p><b>Total Retorno Juros:</b> R$ ${totalJuros.toFixed(2)}</p>`;
  html+=`<p><b>Total Retorno Diárias:</b> R$ ${totalDiarias.toFixed(2)}</p>`;
  html+="<table border='1' cellpadding='5' cellspacing='0'><tr><th>Nome</th><th>CPF</th><th>Status</th><th>Valor</th><th>Data</th><th>Score</th></tr>";
  clientes.forEach(c=>{html+=`<tr><td>${c.nome}</td><td>${c.cpf}</td><td>${c.status}</td><td>${c.emprestimos.map(e=>"R$ "+e.valor).join(", ")}</td><td>${c.emprestimos.map(e=>new Date(e.data).toLocaleDateString("pt-BR")).join(", ")}</td><td>${calcularScore([].concat(...c.emprestimos.map(e=>e.historico)))}</td></tr>`;});
  html+="</table>"; div.innerHTML=html;
  document.body.appendChild(div);
  html2canvas(div).then(canvas=>{const img=canvas.toDataURL("image/png");const novaAba=window.open();novaAba.document.write('<img src="'+img+'"/>');document.body.removeChild(div);});
}

/* ===== FILTROS ===== */
function filtrar(){const filtro=document.getElementById('buscaCliente').value.toLowerCase();
  document.querySelectorAll("#tabelaClientes tbody tr").forEach(tr=>{tr.style.display=tr.cells[0].innerText.toLowerCase().includes(filtro)?'':'';});
}

/* ===== ORDENAR ===== */
function ordenarTabela(col){const tabela=document.getElementById("tabelaClientes"); let rows=[...tabela.tBodies[0].rows];
  rows.sort((a,b)=>parseFloat(a.cells[col].innerText)-parseFloat(b.cells[col].innerText));
  rows.forEach(r=>tabela.tBodies[0].appendChild(r));
}

/* ===== EXPORTAR EXCEL ===== */
function exportExcel(){const wb=XLSX.utils.book_new();const ws=XLSX.utils.table_to_sheet(document.getElementById("tabelaClientes"));
  XLSX.utils.book_append_sheet(wb,ws,"Clientes"); XLSX.writeFile(wb,"RelatorioLCJurista.xlsx");
}

/* ===== GRAFICOS ===== */
let chartStatus, chartScore;
function atualizarGraficos(){
  const ativos=clientes.filter(c=>c.status==="ativo" || c.status==="reativado").length;
  const quitados=clientes.filter(c=>c.status==="quitado").length;
  const inadimplentes=clientes.filter(c=>calcularAtraso(c,new Date())>0 && (c.status==="ativo" || c.status==="reativado")).length;
  const scores=clientes.map(c=>calcularScore([].concat(...c.emprestimos.map(e=>e.historico))));
  if(chartStatus) chartStatus.destroy(); if(chartScore) chartScore.destroy();
  const ctx1=document.getElementById("graficoStatus").getContext("2d");
  chartStatus=new Chart(ctx1,{type:'pie',data:{labels:['Ativos/Reativados','Quitados','Inadimplentes'],datasets:[{data:[ativos,quitados,inadimplentes],backgroundColor:['#00aa00','#00ffff','#ff0000']}]},options:{responsive:true}});
  const ctx2=document.getElementById("graficoScore").getContext("2d");
  chartScore=new Chart(ctx2,{type:'bar',data:{labels:clientes.map(c=>c.nome),datasets:[{label:'Score',data:scores,backgroundColor:'#ffd700'}]},options:{responsive:true,scales:{y:{min:0,max:100}}}});
}

/* ===== TEMA ===== */
function toggleTheme(){dark=!dark; document.body.classList.toggle('darkMode');}

atualizarTabela();
setInterval(atualizarTabela,60000);
</script>
</body>
</html>
