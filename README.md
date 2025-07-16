# dia-del-amigo
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Ruleta Amigo Invisible</title>
  <link href="https://fonts.googleapis.com/css2?family=Luckiest+Guy&display=swap" rel="stylesheet">
  <style>
    body {
      margin: 0;
      padding: 0;
      font-family: 'Luckiest Guy', cursive;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      background: radial-gradient(circle, #ff9a9e, #fad0c4);
      flex-direction: column;
      text-align: center;
    }
    h1 {
      color: #fff;
      font-size: 3rem;
    }
    button {
      padding: 20px 40px;
      font-size: 2rem;
      background: #ff6f61;
      color: white;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      margin-top: 20px;
      box-shadow: 0 5px 15px rgba(0,0,0,0.3);
    }
    button:disabled {
      background: gray;
      cursor: not-allowed;
    }
    .resultado {
      font-size: 2.5rem;
      margin-top: 40px;
      color: #fff;
    }
  </style>
</head>
<body>
  <h1>🎉 Ruleta del Amigo Invisible 🎁</h1>
  <button id="boton">Girar Ruleta</button>
  <div class="resultado" id="resultado"></div>

  <audio id="sonido">
    <source src="https://www.soundjay.com/button/sounds/button-10.mp3" type="audio/mp3">
  </audio>

  <script>
    const nombres = ["agos", "mile", "bau", "cande", "carla", "abi"];
    const usados = JSON.parse(localStorage.getItem("usados")) || [];
    const miNombre = prompt("Ingresá tu nombre exactamente como está: agos, mile, bau, cande, carla o abi").toLowerCase();

    if (!nombres.includes(miNombre)) {
      document.getElementById("resultado").innerText = "Nombre no válido 🙃";
      document.getElementById("boton").disabled = true;
    } else if (localStorage.getItem(miNombre)) {
      document.getElementById("resultado").innerText = `Ya giraste la ruleta 🎯\nTe tocó: ${localStorage.getItem(miNombre)} 🎁`;
      document.getElementById("boton").disabled = true;
    }

    document.getElementById("boton").addEventListener("click", () => {
      const sonido = document.getElementById("sonido");
      sonido.play();
      
      // Opciones que no se repitan y que no seas vos mismo
      const disponibles = nombres.filter(n => !usados.includes(n) && n !== miNombre);
      
      if (disponibles.length === 0) {
        document.getElementById("resultado").innerText = "Ya no hay nombres disponibles 😅";
        return;
      }

      const elegido = disponibles[Math.floor(Math.random() * disponibles.length)];

      usados.push(elegido);
      localStorage.setItem("usados", JSON.stringify(usados));
      localStorage.setItem(miNombre, elegido);
      
      document.getElementById("resultado").innerText = `¡Te tocó ${elegido.toUpperCase()}! 🎁`;
      document.getElementById("boton").disabled = true;
    });
  </script>
</body>
</html>
