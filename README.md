<!DOCTYPE html>
<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <title>LC Jurista - Controle de Empréstimos</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f4f4f4;
      padding: 20px;
    }
    h1 {
      text-align: center;
      color: #222;
    }
    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 20px;
      background: #fff;
    }
    th, td {
      border: 1px solid #ccc;
      padding: 10px;
      text-align: center;
    }
    th {
      background: #333;
      color: #fff;
    }
    tr:nth-child(even) {
      background: #f9f9f9;
    }
    .atraso {
      color: red;
      font-weight: bold;
    }
    .btn {
      padding: 6px 10px;
      margin: 2px;
      border: none;
      cursor: pointer;
      border-radius: 4px;
    }
    .btn-add { background: green; color: #fff; }
    .btn-pay { background: blue; color: #fff; }
    .btn-reactivate { background: orange; color: #fff; }
    .btn-report { background: purple; color: #fff; margin-top: 10px; }
    .quitados {
      margin-top: 30px;
      background: #e8ffe8;
      padding: 15px;
      border: 1px solid #ccc;
    }
  </style>
</head>
<body>
  <h1>LC Jurista - Controle de Empréstimos</h1>

  <button class="btn btn-add" onclick="adicionarCliente()">Adicionar Cliente</button>
  <button class="btn btn-report" onclick="gerarRelatorio()">Gerar Relatório de Quitados</button>

  <table id="tabelaClientes">
    <thead>
      <tr>
        <th>Nome Completo</th>
        <th>Endereço</th>
        <th>Telefone</th>
        <th>Valor Emprestado (R$)</th>
        <th>Juros 30% (R$)</th>
        <th>Total Inicial (R$)</th>
        <th>Data Inicial</th>
        <th>Dias em Atraso</th>
        <th>Multa Diária (R$)</th>
        <th>Total Atualizado (R$)</th>
        <th>Ações</th>
        <th>Score</th>
      </tr>
    </thead>
    <tbody>
      <!-- Clientes fictícios -->
      <tr>
        <td>João da Silva</td>
        <td>Rua A, 123</td>
        <td>(11) 98888-1111</td>
        <td class="valor">1000</td>
        <td class="juros"></td>
        <td class="totalInicial"></td>
        <td class="data"></td>
        <td class="atraso"></td>
        <td class="multa"></td>
        <td class="totalAtual"></td>
        <td>
          <button class="btn btn-pay" onclick="registrarPagamento(this)">Registrar Pagamento</button>
        </td>
        <td class="score">-</td>
      </tr>
      <tr>
        <td>Maria Oliveira</td>
        <td>Av. B, 456</td>
        <td>(11) 97777-2222</td>
        <td class="valor">2000</td>
        <td class="juros"></td>
        <td class="totalInicial"></td>
        <td class="data"></td>
        <td class="atraso"></td>
        <td class="multa"></td>
        <td class="totalAtual"></td>
        <td>
          <button class="btn btn-pay" onclick="registrarPagamento(this)">Registrar Pagamento</button>
        </td>
        <td class="score">-</td>
      </tr>
      <tr>
        <td>Carlos Pereira</td>
        <td>Rua C, 789</td>
        <td>(21) 96666-3333</td>
        <td class="valor">1500</td>
        <td class="juros"></td>
        <td class="totalInicial"></td>
        <td class="data"></td>
        <td class="atraso"></td>
        <td class="multa"></td>
        <td class="totalAtual"></td>
        <td>
          <button class="btn btn-pay" onclick="registrarPagamento(this)">Registrar Pagamento</button>
        </td>
        <td class="score">-</td>
      </tr>
      <tr>
        <td>Ana Souza</td>
        <td>Travessa D, 321</td>
        <td>(31) 95555-4444</td>
        <td class="valor">2500</td>
        <td class="juros"></td>
        <td class="totalInicial"></td>
        <td class="data"></td>
        <td class="atraso"></td>
        <td class="multa"></td>
        <td class="totalAtual"></td>
        <td>
          <button class="btn btn-pay" onclick="registrarPagamento(this)">Registrar Pagamento</button>
        </td>
        <td class="score">-</td>
      </tr>
      <tr>
        <td>Lucas Martins</td>
        <td>Alameda E, 654</td>
        <td>(41) 94444-5555</td>
        <td class="valor">3000</td>
        <td class="juros"></td>
        <td class="totalInicial"></td>
        <td class="data"></td>
        <td class="atraso"></td>
        <td class="multa"></td>
        <td class="totalAtual"></td>
        <td>
          <button class="btn btn-pay" onclick="registrarPagamento(this)">Registrar Pagamento</button>
        </td>
        <td class="score">-</td>
      </tr>
      <!-- Dois clientes já vencidos para testes -->
      <tr>
        <td>Fernanda Lima</td>
        <td>Rua F, 999</td>
        <td>(51) 93333-6666</td>
        <td class="valor">1200</td>
        <td class="juros"></td>
        <td class="totalInicial"></td>
        <td class="data" data-inicial="2025-07-01"></td>
        <td class="atraso"></td>
        <td class="multa"></td>
        <td class="totalAtual"></td>
        <td>
          <button class="btn btn-pay" onclick="registrarPagamento(this)">Registrar Pagamento</button>
        </td>
        <td class="score">-</td>
      </tr>
      <tr>
        <td>Ricardo Gomes</td>
        <td>Av. G, 555</td>
        <td>(61) 92222-7777</td>
        <td class="valor">5000</td>
        <td class="juros"></td>
        <td class="totalInicial"></td>
        <td class="data" data-inicial="2025-06-25"></td>
        <td class="atraso"></td>
        <td class="multa"></td>
        <td class="totalAtual"></td>
        <td>
          <button class="btn btn-pay" onclick="registrarPagamento(this)">Registrar Pagamento</button>
        </td>
        <td class="score">-</td>
      </tr>
    </tbody>
  </table>

  <div class="quitados">
    <h2>Clientes Quitados</h2>
    <table id="tabelaQuitados">
      <thead>
        <tr>
          <th>Nome</th>
          <th>Score</th>
          <th>Ações</th>
        </tr>
      </thead>
      <tbody></tbody>
    </table>
  </div>

  <script>
    function formatarData(date) {
      return date.toLocaleDateString('pt-BR');
    }

    function calcularTabela() {
      const linhas = document.querySelectorAll("#tabelaClientes tbody tr");
      const hoje = new Date();

      linhas.forEach(linha => {
        const valor = parseFloat(linha.querySelector(".valor").textContent);
        const juros = valor * 0.3;
        const totalInicial = valor + juros;

        linha.querySelector(".juros").textContent = juros.toFixed(2);
        linha.querySelector(".totalInicial").textContent = totalInicial.toFixed(2);

        let dataCelula = linha.querySelector(".data");
        let dataInicial;
        if (!dataCelula.dataset.inicial) {
          const diasAtras = Math.floor(Math.random() * 30);
          dataInicial = new Date();
          dataInicial.setDate(hoje.getDate() - diasAtras);
          dataCelula.dataset.inicial = dataInicial.toISOString();
        } else {
          dataInicial = new Date(dataCelula.dataset.inicial);
        }
        dataCelula.textContent = formatarData(dataInicial);

        const diasPassados = Math.floor((hoje - dataInicial) / (1000 * 60 * 60 * 24));
        let atraso = 0;
        let multa = 0;
        let totalAtual = totalInicial;

        if (diasPassados > 30) {
          atraso = diasPassados - 30;
          multa = atraso * 30;
          totalAtual += multa;
        }

        linha.querySelector(".atraso").textContent = atraso > 0 ? atraso : "0";
        linha.querySelector(".multa").textContent = multa.toFixed(2);
        linha.querySelector(".totalAtual").textContent = totalAtual.toFixed(2);
      });
    }

    function registrarPagamento(botao) {
      const linha = botao.closest("tr");
      const nome = linha.cells[0].textContent;
      const totalAtual = parseFloat(linha.querySelector(".totalAtual").textContent);
      const totalInicial = parseFloat(linha.querySelector(".totalInicial").textContent);

      let valorPago = prompt(`Digite o valor pago por ${nome}:`, "0");
      if (!valorPago || isNaN(valorPago)) return;
      valorPago = parseFloat(valorPago);

      if (valorPago >= totalAtual) {
        alert(`${nome} quitou a dívida!`);
        moverParaQuitados(linha, 100); // score perfeito
      } else {
        if (valorPago >= totalInicial * 0.3) {
          if (confirm(`${nome} pagou mais de 30%. Deseja reiniciar os 30 dias de prazo?`)) {
            const hoje = new Date();
            linha.querySelector(".data").dataset.inicial = hoje.toISOString();
          }
        }
        alert(`${nome} pagou R$${valorPago.toFixed(2)}. Dívida atualizada.`);
        // recalcular dívida
        let novoValor = totalAtual - valorPago;
        linha.querySelector(".valor").textContent = novoValor.toFixed(2);
      }

      calcularTabela();
    }

    function moverParaQuitados(linha, score) {
      const tabelaQuitados = document.querySelector("#tabelaQuitados tbody");
      const nome = linha.cells[0].textContent;

      const novaLinha = document.createElement("tr");
      novaLinha.innerHTML = `
        <td>${nome}</td>
        <td>${score}</td>
        <td><button class="btn btn-reactivate" onclick="reativarCliente(this, '${nome}')">Reativar</button></td>
      `;
      tabelaQuitados.appendChild(novaLinha);

      linha.remove();
    }

    function reativarCliente(botao, nome) {
      const linhaQuitado = botao.closest("tr");
      linhaQuitado.remove();

      const tabela = document.querySelector("#tabelaClientes tbody");
      const novaLinha = document.createElement("tr");
      novaLinha.innerHTML = `
        <td>${nome}</td>
        <td>Endereço Novo</td>
        <td>(00) 90000-0000</td>
        <td class="valor">1000</td>
        <td class="juros"></td>
        <td class="totalInicial"></td>
        <td class="data"></td>
        <td class="atraso"></td>
        <td class="multa"></td>
        <td class="totalAtual"></td>
        <td><button class="btn btn-pay" onclick="registrarPagamento(this)">Registrar Pagamento</button></td>
        <td class="score">-</td>
      `;
      tabela.appendChild(novaLinha);
      calcularTabela();
    }

    function adicionarCliente() {
      const nome = prompt("Nome completo:");
      if (!nome) return;
      const endereco = prompt("Endereço:");
      const telefone = prompt("Telefone:");
      const valor = parseFloat(prompt("Valor emprestado:", "1000"));

      const tabela = document.querySelector("#tabelaClientes tbody");
      const novaLinha = document.createElement("tr");
      novaLinha.innerHTML = `
        <td>${nome}</td>
        <td>${endereco}</td>
        <td>${telefone}</td>
        <td class="valor">${valor}</td>
        <td class="juros"></td>
        <td class="totalInicial"></td>
        <td class="data"></td>
        <td class="atraso"></td>
        <td class="multa"></td>
        <td class="totalAtual"></td>
        <td><button class="btn btn-pay" onclick="registrarPagamento(this)">Registrar Pagamento</button></td>
        <td class="score">-</td>
      `;
      tabela.appendChild(novaLinha);
      calcularTabela();
    }

    function gerarRelatorio() {
      const linhas = document.querySelectorAll("#tabelaQuitados tbody tr");
      let relatorio = "Relatório de Quitados:\n";
      linhas.forEach(linha => {
        const nome = linha.cells[0].textContent;
        const score = linha.cells[1].textContent;
        relatorio += `Cliente: ${nome} | Score: ${score}\n`;
      });
      alert(relatorio);
    }

    calcularTabela();
    setInterval(calcularTabela, 60000); // atualiza a cada 1 min
  </script>
</body>
</html>
