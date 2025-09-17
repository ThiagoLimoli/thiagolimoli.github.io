<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>LC Jurista - Controle de Empréstimos</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: linear-gradient(135deg, #1c1c1c, #2e2e2e);
      color: #f5f5f5;
      margin: 0;
      padding: 20px;
    }
    h1 {
      text-align: center;
      color: #ffd700;
      margin-bottom: 10px;
    }
    .resumo {
      display: flex;
      justify-content: space-around;
      margin-bottom: 15px;
      font-weight: bold;
      background: linear-gradient(135deg, #333, #444);
      padding: 10px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.6);
    }
    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 10px;
      border-radius: 12px;
      overflow: hidden;
      box-shadow: 0 0 15px rgba(0,0,0,0.8);
    }
    th, td {
      padding: 10px;
      text-align: center;
    }
    th {
      background: linear-gradient(135deg, #444, #222);
      color: #ffd700;
    }
    tr {
      background: #2c2c2c;
    }
    tr:nth-child(even) {
      background: #3a3a3a;
    }
    .inadimplente {
      background: linear-gradient(135deg, #5a0000, #330000) !important;
      color: #fff;
      font-weight: bold;
    }
    .adimplente {
      background: linear-gradient(135deg, #003300, #005500) !important;
      color: #d4ffd4;
    }
    button {
      padding: 5px 10px;
      margin: 2px;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      font-weight: bold;
    }
    button:hover {
      opacity: 0.9;
    }
    .btn-add { background: #0066cc; color: #fff; }
    .btn-pay { background: #ff8800; color: #fff; }
    .btn-quit { background: #00aa00; color: #fff; }
    .btn-react { background: #aa00aa; color: #fff; }
    .btn-png { background: #4444aa; color: #fff; }
    .btn-all { background: #ff0066; color: #fff; }
  </style>
</head>
<body>
  <h1>LC Jurista - Controle de Empréstimos</h1>

  <div class="resumo">
    <div>Total Emprestado: R$ <span id="totalEmprestado">0</span></div>
    <div>Total Retorno Juros: R$ <span id="totalJuros">0</span></div>
    <div>Total Retorno Diárias: R$ <span id="totalDiarias">0</span></div>
  </div>

  <button class="btn-add" onclick="adicionarCliente()">+ Adicionar Cliente</button>
  <button class="btn-all" onclick="gerarRelatorioGeral()">📊 Relatório Consolidado</button>

  <table id="tabelaClientes">
    <thead>
      <tr>
        <th>Nome</th>
        <th>Endereço</th>
        <th>Telefone</th>
        <th>Valor (R$)</th>
        <th>Juros 30% (R$)</th>
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

  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
  <script>
    let clientes = [
      {nome:"João da Silva",endereco:"Rua A, 123",telefone:"(11)98888-1111",valor:1000,data:"2025-08-20",historico:[],status:"ativo"},
      {nome:"Maria Oliveira",endereco:"Av. B, 456",telefone:"(11)97777-2222",valor:2000,data:"2025-08-15",historico:[],status:"ativo"},
      {nome:"Carlos Pereira",endereco:"Rua C, 789",telefone:"(21)96666-3333",valor:1500,data:"2025-07-30",historico:[],status:"ativo"},
      {nome:"Ana Souza",endereco:"Travessa D, 321",telefone:"(31)95555-4444",valor:2500,data:"2025-08-25",historico:[],status:"ativo"},
      {nome:"Lucas Martins",endereco:"Alameda E, 654",telefone:"(41)94444-5555",valor:3000,data:"2025-08-28",historico:[],status:"ativo"},
      {nome:"Pedro Lima",endereco:"Rua F, 111",telefone:"(51)93333-6666",valor:1200,data:"2025-07-10",historico:[],status:"ativo"},
      {nome:"Juliana Alves",endereco:"Av. G, 222",telefone:"(61)92222-7777",valor:1800,data:"2025-07-15",historico:[],status:"ativo"}
    ];

    function calcularScore(historico) {
      if (historico.length === 0) return 50;
      let pontos = 50;
      historico.forEach(p => {
        if (p.pagoNoPrazo) pontos += 10;
        else pontos -= 5;
      });
      return Math.max(0, Math.min(100, pontos));
    }

    function atualizarTabela() {
      const tbody = document.querySelector("#tabelaClientes tbody");
      tbody.innerHTML = "";

      let totalEmprestado = 0, totalJuros = 0, totalDiarias = 0;
      const hoje = new Date();

      clientes.sort((a,b) => calcularAtraso(b, hoje) - calcularAtraso(a, hoje));

      clientes.forEach((c, i) => {
        if (c.status !== "ativo") return;

        const valor = parseFloat(c.valor);
        const juros = valor * 0.3;
        const totalInicial = valor + juros;
        const dataInicial = new Date(c.data);
        const diasPassados = Math.floor((hoje - dataInicial) / (1000*60*60*24));
        let atraso = 0, multa = 0, totalAtual = totalInicial;

        if (diasPassados > 30) {
          atraso = diasPassados - 30;
          multa = atraso * 30;
          totalAtual += multa;
        }

        totalEmprestado += valor;
        totalJuros += juros;
        totalDiarias += multa;

        const tr = document.createElement("tr");
        tr.className = atraso > 0 ? "inadimplente" : "adimplente";
        tr.innerHTML = `
          <td contenteditable="true">${c.nome}</td>
          <td contenteditable="true">${c.endereco}</td>
          <td contenteditable="true">${c.telefone}</td>
          <td>${valor.toFixed(2)}</td>
          <td>${juros.toFixed(2)}</td>
          <td>${totalInicial.toFixed(2)}</td>
          <td>${dataInicial.toLocaleDateString("pt-BR")}</td>
          <td>${atraso}</td>
          <td>${multa.toFixed(2)}</td>
          <td>${totalAtual.toFixed(2)}</td>
          <td>${calcularScore(c.historico)}</td>
          <td>
            <button class="btn-pay" onclick="registrarPagamento(${i})">Pagamento</button>
            <button class="btn-quit" onclick="quitar(${i})">Quitar</button>
            <button class="btn-png" onclick="gerarPNG(${i})">PNG</button>
          </td>
        `;
        tbody.appendChild(tr);
      });

      document.getElementById("totalEmprestado").textContent = totalEmprestado.toFixed(2);
      document.getElementById("totalJuros").textContent = totalJuros.toFixed(2);
      document.getElementById("totalDiarias").textContent = totalDiarias.toFixed(2);
    }

    function calcularAtraso(cliente, hoje) {
      const dataInicial = new Date(cliente.data);
      const diasPassados = Math.floor((hoje - dataInicial)/(1000*60*60*24));
      return diasPassados > 30 ? diasPassados - 30 : 0;
    }

    function registrarPagamento(i) {
      const valorPago = parseFloat(prompt("Digite o valor pago pelo cliente:"));
      if (isNaN(valorPago) || valorPago <= 0) return;
      const cliente = clientes[i];
      const valorMinimo = cliente.valor * 0.3;

      if (valorPago >= valorMinimo) {
        if (confirm("Pagamento maior que 30% detectado. Deseja reiniciar os 30 dias?")) {
          cliente.data = new Date().toISOString().split("T")[0];
        }
        cliente.historico.push({valor:valorPago, pagoNoPrazo:true});
      } else {
        cliente.historico.push({valor:valorPago, pagoNoPrazo:false});
      }
      atualizarTabela();
    }

    function quitar(i) {
      if (confirm("Deseja realmente quitar esta dívida?")) {
        clientes[i].status = "quitado";
        atualizarTabela();
      }
    }

    function adicionarCliente() {
      const nome = prompt("Nome do cliente:");
      const endereco = prompt("Endereço do cliente:");
      const telefone = prompt("Telefone do cliente:");
      const valor = parseFloat(prompt("Valor emprestado:"));
      const data = prompt("Data inicial (AAAA-MM-DD):", new Date().toISOString().split("T")[0]);

      if (nome && endereco && telefone && !isNaN(valor)) {
        clientes.push({nome,endereco,telefone,valor,data,historico:[],status:"ativo"});
        atualizarTabela();
      }
    }

    function gerarPNG(i) {
      const cliente = clientes[i];
      const div = document.createElement("div");
      div.style.padding = "20px";
      div.style.background = "#fff";
      div.style.color = "#000";
      div.innerHTML = `
        <h2>Relatório de ${cliente.nome}</h2>
        <p>Endereço: ${cliente.endereco}</p>
        <p>Telefone: ${cliente.telefone}</p>
        <p>Valor emprestado: R$ ${cliente.valor}</p>
        <p>Data inicial: ${cliente.data}</p>
        <p>Status: ${cliente.status}</p>
        <p>Score: ${calcularScore(cliente.historico)}</p>
      `;
      document.body.appendChild(div);
      html2canvas(div).then(canvas => {
        const img = canvas.toDataURL("image/png");
        const novaAba = window.open();
        novaAba.document.write('<img src="'+img+'"/>');
        document.body.removeChild(div);
      });
    }

    function gerarRelatorioGeral() {
      const hoje = new Date();
      let totalEmprestado = 0, totalJuros = 0, totalDiarias = 0;

      clientes.forEach(c => {
        const valor = parseFloat(c.valor);
        const juros = valor * 0.3;
        const dataInicial = new Date(c.data);
        const diasPassados = Math.floor((hoje - dataInicial) / (1000*60*60*24));
        let multa = 0;
        if (diasPassados > 30) multa = (diasPassados - 30) * 30;

        totalEmprestado += valor;
        totalJuros += juros;
        totalDiarias += multa;
      });

      const div = document.createElement("div");
      div.style.padding = "20px";
      div.style.background = "#fff";
      div.style.color = "#000";

      let html = `<h2>Relatório Consolidado - ${hoje.toLocaleDateString("pt-BR")}</h2>`;
      html += `<p><b>Total Emprestado:</b> R$ ${totalEmprestado.toFixed(2)}</p>`;
      html += `<p><b>Total Retorno Juros:</b> R$ ${totalJuros.toFixed(2)}</p>`;
      html += `<p><b>Total Retorno Diárias:</b> R$ ${totalDiarias.toFixed(2)}</p>`;

      html += "<table border='1' cellpadding='5' cellspacing='0'><tr><th>Nome</th><th>Status</th><th>Valor</th><th>Data</th><th>Score</th></tr>";

      clientes.forEach(c => {
        html += `<tr>
          <td>${c.nome}</td>
          <td>${c.status}</td>
          <td>R$ ${c.valor}</td>
          <td>${c.data}</td>
          <td>${calcularScore(c.historico)}</td>
        </tr>`;
      });

      html += "</table>";
      div.innerHTML = html;

      document.body.appendChild(div);
      html2canvas(div).then(canvas => {
        const img = canvas.toDataURL("image/png");
        const novaAba = window.open();
        novaAba.document.write('<img src="'+img+'"/>');
        document.body.removeChild(div);
      });
    }

    atualizarTabela();
    setInterval(atualizarTabela, 60000);
  </script>
</body>
</html>
