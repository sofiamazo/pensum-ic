# pensum-ic
pensum
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Plan de Estudios - Sofía Mazo</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #fdfdfd;
      padding: 20px;
      color: #333;
    }
    h1 {
      text-align: center;
      margin-bottom: 10px;
    }
    #progreso {
      width: 80%;
      margin: 0 auto 10px;
      background: #eee;
      border-radius: 10px;
      overflow: hidden;
    }
    #barraProgreso {
      height: 25px;
      background: #81c784; /* verde suave */
      width: 0%;
      text-align: center;
      color: white;
      line-height: 25px;
      font-weight: bold;
    }
    #creditosContador {
      text-align: center;
      margin-bottom: 30px;
      font-size: 16px;
      font-weight: bold;
    }
    .semestre {
      display: inline-block;
      vertical-align: top;
      margin: 10px;
      background: white;
      padding: 10px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.05);
      width: 220px;
    }
    .materia {
      background: #f0f0f0;
      margin: 5px 0;
      padding: 8px;
      border-radius: 8px;
      cursor: pointer;
      transition: background 0.3s;
      font-size: 14px;
    }
    .bloqueada { background: #e0e0e0; cursor: not-allowed; }
    .disponible { background: #ffe0e6; }
    .vista { background: #d0f0d0; text-decoration: line-through; }
  </style>
</head>
<body>
  <h1>Plan de Estudios - Sofía Mazo</h1>
  <div id="progreso">
    <div id="barraProgreso">0%</div>
  </div>
  <div id="creditosContador">0 de 150 créditos completados</div>
  <div id="contenedor"></div>

  <script>
    const materias = [...]; // aquí va la lista completa (omitida aquí para brevedad)

    function crearInterfaz() {
      const contenedor = document.getElementById('contenedor');
      contenedor.innerHTML = "";
      let totalCreditos = 0;
      let vistosCreditos = 0;

      const maxSemestre = Math.max(...materias.map(m => m.semestre));
      for (let s = 1; s <= maxSemestre; s++) {
        const col = document.createElement('div');
        col.className = 'semestre';
        col.innerHTML = `<h3>Semestre ${s}</h3>`;

        materias.filter(m => m.semestre === s).forEach(m => {
          const div = document.createElement('div');
          div.className = 'materia';
          div.textContent = `${m.nombre} (${m.creditos} cr)`;
          div.dataset.nombre = m.nombre;
          totalCreditos += m.creditos;

          const estado = localStorage.getItem(m.nombre);
          if (estado === 'vista') {
            div.classList.add('vista');
            vistosCreditos += m.creditos;
          } else if (cumpleRequisitos(m)) {
            div.classList.add('disponible');
            div.onclick = () => marcarVista(m.nombre);
          } else {
            div.classList.add('bloqueada');
          }

          col.appendChild(div);
        });

        contenedor.appendChild(col);
      }

      actualizarProgreso(vistosCreditos, totalCreditos);
    }

    function actualizarProgreso(vistos, total) {
      const porcentaje = Math.round((vistos / total) * 100);
      const barra = document.getElementById('barraProgreso');
      barra.style.width = `${porcentaje}%`;
      barra.textContent = `${porcentaje}%`;

      const contador = document.getElementById('creditosContador');
      contador.textContent = `${vistos} de ${total} créditos completados`;
    }

    function cumpleRequisitos(materia) {
      return materia.requisitos.every(r => localStorage.getItem(r) === 'vista');
    }

    function marcarVista(nombre) {
      localStorage.setItem(nombre, 'vista');
      crearInterfaz();
    }

    crearInterfaz();
  </script>
</body>
</html>
