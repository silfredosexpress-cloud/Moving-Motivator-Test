<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>Test Moving Motivators</title>
  <style>
    body {
      font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;
      background: linear-gradient(to bottom right, #ffe259, #ffa751);
      margin: 0;
      padding: 20px;
    }
    .container {
      background: transparent;
      padding: 40px 20px;
      max-width: 960px;
      margin: auto;
      border-radius: 15px;
    }
    h1 {
      text-align: center;
      color: #d700a6;
      margin-bottom: 30px;
      font-size: 1.8em;
    }
    .question {
      margin-bottom: 35px;
      padding: 25px;
      background: #fff0f7;
      border-left: 6px solid #d700a6;
      border-radius: 10px;
    }
    .question p {
      font-weight: bold;
      color: #333;
      margin-bottom: 12px;
    }
    .question label {
      display: block;
      margin-left: 15px;
      margin-bottom: 8px;
      font-weight: normal;
      color: #222;
      cursor: pointer;
      line-height: 1.4;
    }
    .question input[type="checkbox"] {
      margin-right: 10px;
      transform: scale(1.2);
    }
    .result {
      margin-top: 40px;
      text-align: center;
      font-size: 1.2em;
      color: #444;
      background: #ffe7f3;
      border: 2px solid #d700a6;
      border-radius: 12px;
      padding: 20px;
    }
    .result h2 {
      color: #d700a6;
      margin-bottom: 20px;
    }
    .ranking-box {
      background: #fff;
      border: 2px dashed #d700a6;
      border-radius: 12px;
      padding: 20px;
      margin-top: 20px;
    }
    button {
      display: block;
      margin: 40px auto 20px auto;
      padding: 14px 28px;
      font-size: 18px;
      background-color: #d700a6;
      color: white;
      border: none;
      border-radius: 8px;
      cursor: pointer;
      transition: background-color 0.3s ease, transform 0.2s;
    }
    button:hover {
      background-color: #b30085;
      transform: scale(1.03);
    }
  </style>
</head>
<body>
  <div class="container">
    <h1>Test Moving Motivators</h1>
    <form id="motivatorsForm">
      <div id="questions"></div>
      <button type="submit">Ver mis motivadores principales</button>
    </form>
    <div class="result" id="result"></div>
  </div>

  <script>
    const preguntasYRespuestas = [{
      pregunta: "¿Qué te motiva más en tu trabajo diario?",
      opciones: [
        "A. Tener libertad para tomar decisiones",
        "B. Saber que mi trabajo tiene sentido",
        "C. Aprender cosas nuevas constantemente",
        "D. Que se reconozca mi esfuerzo",
        "E. Que mis valores estén alineados con la empresa",
        "F. Tener metas claras",
        "G. Recibir retroalimentación continua",
        "H. Poder influir en decisiones importantes",
        "I. Tener relaciones cercanas con mis compañeros",
        "J. Sentirme valorado por lo que soy"
      ]
    },
    {
      pregunta: "¿Qué aspecto valoras más de tu equipo de trabajo?",
      opciones: [
        "A. Que comparta información de forma abierta",
        "B. Que me impulse a crecer profesionalmente",
        "C. Que exista un propósito común",
        "D. Que me permitan ser creativo",
        "E. Que se respeten los roles y procesos",
        "F. Que confíen en mi capacidad",
        "G. Que exista un liderazgo claro",
        "H. Que se celebren los logros",
        "I. Que haya buena comunicación",
        "J. Que se fomenten las relaciones humanas"
      ]
    },
    {
      pregunta: "¿Qué te hace sentir orgulloso de pertenecer a tu empresa?",
      opciones: [
        "A. Que me sienta valorado como persona",
        "B. Que se fomente el aprendizaje continuo",
        "C. Que me den libertad para tomar decisiones",
        "D. Que reconozcan mis aportaciones públicamente",
        "E. Que contribuyamos a un propósito más grande",
        "F. Que haya coherencia con mis valores",
        "G. Que me impulse a mejorar constantemente",
        "H. Que tenga estructuras claras y confiables",
        "I. Que me involucren en decisiones importantes",
        "J. Que tenga relaciones humanas profundas"
      ]
    }];

    const container = document.getElementById("questions");
    preguntasYRespuestas.forEach((item, index) => {
      const div = document.createElement("div");
      div.className = "question";
      const p = document.createElement("p");
      p.textContent = `${index + 1}. ${item.pregunta}`;
      div.appendChild(p);

      item.opciones.forEach(op => {
        const input = document.createElement("input");
        input.type = "checkbox";
        input.name = `q${index}`;
        input.value = op.charAt(0);
        const label = document.createElement("label");
        label.textContent = op;
        label.prepend(input);
        div.appendChild(label);
      });

      container.appendChild(div);
    });

    document.getElementById("motivatorsForm").addEventListener("submit", function(e) {
      e.preventDefault();
      const counts = { A: 0, B: 0, C: 0, D: 0, E: 0, F: 0, G: 0, H: 0, I: 0, J: 0 };
      preguntasYRespuestas.forEach((_, i) => {
        const seleccionados = document.querySelectorAll(`input[name=q${i}]:checked`);
        seleccionados.forEach(input => {
          counts[input.value]++;
        });
      });

      const motivadores = {
        A: "Aceptación",
        B: "Curiosidad",
        C: "Libertad",
        D: "Estatus",
        E: "Meta",
        F: "Honra",
        G: "Maestría",
        H: "Orden",
        I: "Poder",
        J: "Relaciones"
      };

      const sorted = Object.entries(counts).sort((a, b) => b[1] - a[1]);
      const top3 = sorted.slice(0, 3);

      let resultadoHTML = `<h2>Ranking de motivadores principales:</h2><div class='ranking-box'>`;
      top3.forEach(([letra, valor], idx) => {
        resultadoHTML += `<p><strong>${idx + 1}. ${motivadores[letra]}</strong> (${valor} votos)</p>`;
      });
      resultadoHTML += `</div>`;

      document.getElementById("result").innerHTML = resultadoHTML;
    });
  </script>
</body>
</html>
