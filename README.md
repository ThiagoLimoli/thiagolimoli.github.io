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
  </style>
</head>
<body>
  <h1>LC Jurista - Controle de Empréstimos</h1>

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
      </tr>
    </tbody>
  </table>

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

        // Gerar data inicial aleatória nos últimos 30 dias
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

        // Verificar atraso
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

    calcularTabela();
    setInterval(calcularTabela, 60000); // atualiza a cada 1 minuto
  </script>
</body>
</html>
