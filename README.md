<!DOCTYPE html>
<html lang="pt-br">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Comparação de Taxa de Retenção de Clientes</title>
<script src="https://cdn.plot.ly/plotly-latest.min.js"></script>
</head>
<body>

<h2>Comparação de Taxa de Retenção de Clientes</h2>

<div id="periodo1">
  <h3>Período 1</h3>
  <label for="dataInicio1">Data de Início:</label>
  <input type="date" id="dataInicio1" name="dataInicio1" required><br><br>
  <label for="dataFim1">Data de Fim:</label>
  <input type="date" id="dataFim1" name="dataFim1" required><br><br>
  <label for="clientesInicio1">Clientes no Início do Período:</label>
  <input type="number" id="clientesInicio1" name="clientesInicio1" required><br><br>
  <label for="novosClientes1">Novos Clientes no Período:</label>
  <input type="number" id="novosClientes1" name="novosClientes1" required><br><br>
  <label for="totalClientes1">Total de Clientes no Período:</label>
  <input type="number" id="totalClientes1" name="totalClientes1" required><br><br>
</div>

<button onclick="adicionarPeriodo()">Adicionar Período</button>
<button onclick="calcularTaxasRetencao()">Calcular Taxas de Retenção</button>

<div id="graficos"></div>

<script>
var quantPeriodos = 1;
var taxasRetencao = [];

function adicionarPeriodo() {
  quantPeriodos++;
  var novoPeriodoHTML = "<div id='periodo" + quantPeriodos + "'>";
  novoPeriodoHTML += "<h3>Período " + quantPeriodos + "</h3>";
  novoPeriodoHTML += "<label for='dataInicio" + quantPeriodos + "'>Data de Início:</label>";
  novoPeriodoHTML += "<input type='date' id='dataInicio" + quantPeriodos + "' name='dataInicio" + quantPeriodos + "' required><br><br>";
  novoPeriodoHTML += "<label for='dataFim" + quantPeriodos + "'>Data de Fim:</label>";
  novoPeriodoHTML += "<input type='date' id='dataFim" + quantPeriodos + "' name='dataFim" + quantPeriodos + "' required><br><br>";
  novoPeriodoHTML += "<label for='clientesInicio" + quantPeriodos + "'>Clientes no Início do Período:</label>";
  novoPeriodoHTML += "<input type='number' id='clientesInicio" + quantPeriodos + "' name='clientesInicio" + quantPeriodos + "' required><br><br>";
  novoPeriodoHTML += "<label for='novosClientes" + quantPeriodos + "'>Novos Clientes no Período:</label>";
  novoPeriodoHTML += "<input type='number' id='novosClientes" + quantPeriodos + "' name='novosClientes" + quantPeriodos + "' required><br><br>";
  novoPeriodoHTML += "<label for='totalClientes" + quantPeriodos + "'>Total de Clientes no Período:</label>";
  novoPeriodoHTML += "<input type='number' id='totalClientes" + quantPeriodos + "' name='totalClientes" + quantPeriodos + "' required><br><br>";
  novoPeriodoHTML += "</div>";
  document.getElementById("graficos").insertAdjacentHTML('beforebegin', novoPeriodoHTML);
}

function calcularTaxasRetencao() {
  taxasRetencao = [];
  var periodoLabels = [];
  var data = [];

  for (var i = 1; i <= quantPeriodos; i++) {
    var dataInicio = new Date(document.getElementById("dataInicio" + i).value);
    var dataFim = new Date(document.getElementById("dataFim" + i).value);
    var clientesInicio = parseInt(document.getElementById("clientesInicio" + i).value);
    var novosClientes = parseInt(document.getElementById("novosClientes" + i).value);
    var totalClientes = parseInt(document.getElementById("totalClientes" + i).value);
    var taxaRetencao = ((totalClientes - novosClientes) / clientesInicio) * 100;
    taxasRetencao.push(taxaRetencao.toFixed(2));
    periodoLabels.push("Período " + i + "<br>" + dataInicio.toLocaleDateString() + " a " + dataFim.toLocaleDateString());
  }

  var trace = {
    x: periodoLabels,
    y: taxasRetencao,
    type: 'bar',
    marker: {
      color: 'rgb(158,202,225)',
      line: {
        color: 'rgb(8,48,107)',
        width: 1.5
      }
    }
  };

  var data = [trace];

  var layout = {
    title: 'Comparação de Taxa de Retenção de Clientes',
    xaxis: {
      title: 'Períodos',
      tickfont: {
        size: 10
      }
    },
    yaxis: {
      title: 'Taxa de Retenção (%)'
    }
  };

  Plotly.newPlot('graficos', data, layout);
}
</script>

</body>
</html>
