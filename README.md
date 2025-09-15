<html lang="pt-BR">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Meu Treino</title>
  <link rel="stylesheet" href="style.css">

  <!-- Fonte do Google -->
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap" rel="stylesheet">

  <style>
    
    h1 {
      text-align: center;
      margin-bottom: 20px;
    }

    select, input {
      margin: 5px 0;
      padding: 10px;
      font-size: 16px;
      border-radius: 8px;
      border: 1px solid #666;
    }

    .treino {
      background: rgb(51, 43, 43);
      padding: 20px;
      border-radius: 10px;
      margin-top: 15px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.4);
    }

    .exercicio {
      margin-bottom: 15px;
    }

    label {
      display: block;
      margin-bottom: 5px;
      font-weight: bold;
      color: #fff;
    }

    h3 {
      color: #ffeb3b;
      margin-top: 20px;
    }

    /* Caixinhas pequenas lado a lado */
    .pesos {
      display: flex;
      gap: 8px;
    }

    .pesos input {
      width: 70px;
      padding: 5px;
      text-align: center;
    }

    /* Feedback visual quando salva */
    .salvo {
      border: 2px solid #4caf50 !important;
      background: #e8f5e9;
      color: #000;
    }

    /* Responsividade */
    @media (max-width: 600px) {
      .pesos {
        flex-direction: column;
      }
      .pesos input {
        width: 100%;
      }
    }
  </style>
</head>
<body>
  <h1>TREINO SEMANAL <br>(Grazelda)</h1>

  <label for="dia">Selecione o dia:</label>
  <select id="dia" onchange="carregarTreino()">
    <option value="segunda">Segunda-feira</option>
    <option value="terca">Terça-feira</option>
    <option value="quarta">Quarta-feira</option>
    <option value="quinta">Quinta-feira</option>
    <option value="sexta">Sexta-feira</option>
    <option value="sabado">Sábado</option>
    <option value="domingo">Domingo</option>
  </select>
  <button id="btnduvidas">duvidas</button>

  <div id="treino" class="treino"></div>

<script>
    const treinos = {
      segunda: { peito: ["Supino Reto", "Cruxifixo"], ombro: ["Desenvolvimento", "Remada Alta Na Polia Baixa"], triceps: ["Tríceps Na Polia", "Francês No Halter"] },
      terca: { quadriceps: ["Agachamento No Smith", "Leg Press", "Cadeira Extensora", "Panturrilha Sentado"], abdominal: ["Abdômen"] },
      quarta: { cardio: ["Cardio"] },
      quinta: { costas: ["Puxada Alta", "Remada Baixa", "Pulldown"], biceps: ["Bíceps Na Polia", "Bíceps Scott"] },
      sexta: { pernas: ["Stiff", "Mesa Flexora", "Coice Na Polia", "Cadeira Abdutora", "Elevação Pélvica", "Panturrilha Em Pé"], abdominal: ["Abdômen"] },
      sabado: { descanso: ["Descanso"] },
      domingo: { descanso: ["Descanso"] }
    };

    function carregarTreino() {
      const dia = document.getElementById("dia").value;
      const treinoDiv = document.getElementById("treino");
      treinoDiv.innerHTML = "";

      if (treinos[dia].descanso) {
        treinoDiv.innerHTML = "<h3>Dia de descanso 😴</h3>";
        return;
      }

      for (const categoria in treinos[dia]) {
        treinoDiv.innerHTML += `<h3>${categoria.toUpperCase()}</h3>`;

        treinos[dia][categoria].forEach(exercicio => {
          treinoDiv.innerHTML += `
            <div class="exercicio">
              <label>${exercicio}</label>
              <div class="pesos">
                ${[1,2,3,4].map(i => {
                  const pesoSalvo = localStorage.getItem(`${dia}-${exercicio}-rep${i}`) || "";
                  return `<input type="text" value="${pesoSalvo}" onkeydown="formatarPeso(event, '${dia}', '${exercicio}', ${i}, this)">`;
                }).join("")}
              </div>
            </div>
          `;
        });
      }
    }

    function formatarPeso(event, dia, exercicio, rep, input) {
      if (event.key === "Enter") {
        let valor = input.value.replace("kg", "").trim();
        if (valor && !isNaN(valor)) {
          input.value = valor + " kg";
          localStorage.setItem(`${dia}-${exercicio}-rep${rep}`, input.value);

          // Feedback visual
          input.classList.add("salvo");
          setTimeout(() => input.classList.remove("salvo"), 600);

          // Próximo input
          const proximo = input.parentElement.querySelectorAll("input")[rep];
          if (proximo) proximo.focus();
        }
        event.preventDefault();
      }
    }

    window.onload = carregarTreino;

    document.getElementById("btnduvidas").onclick = function() {
  window.open("duvidas.html", "_blank"); // abre a página de dúvidas
};
</script>
</body>
</html>

