# pensum-ic
pensum
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Plan de Estudios - Investigación Criminal</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f4f4f4;
      padding: 20px;
    }
    h1 {
      text-align: center;
      color: #333;
    }
    .semestre {
      display: inline-block;
      vertical-align: top;
      margin: 10px;
      background: white;
      padding: 10px;
      border-radius: 10px;
      box-shadow: 0 0 10px rgba(0,0,0,0.1);
      width: 220px;
    }
    .materia {
      background: #eee;
      margin: 5px 0;
      padding: 8px;
      border-radius: 8px;
      cursor: pointer;
      transition: background 0.3s;
    }
    .bloqueada { background: #ccc; cursor: not-allowed; }
    .disponible { background: #fff8dc; }
    .vista { background: #c8e6c9; text-decoration: line-through; }
  </style>
</head>
<body>
  <h1>Plan de Estudios - Investigación Criminal</h1>
  <div id="contenedor"></div>

  <script>
    const materias = [
      { nombre: "Expresión Escrita", semestre: 1, creditos: 3, requisitos: [] },
      { nombre: "Actividad Deportiva o Cultural", semestre: 1, creditos: 1, requisitos: [] },
      { nombre: "Matemáticas Aplicadas a la Investigación Criminal", semestre: 1, creditos: 3, requisitos: [] },
      { nombre: "Teoría del Derecho y Constitución", semestre: 1, creditos: 3, requisitos: [] },
      { nombre: "Criminalística", semestre: 1, creditos: 3, requisitos: [] },
      { nombre: "Informática Forense", semestre: 1, creditos: 3, requisitos: [] },
      { nombre: "Cátedra Institucional Ciencia y Libertad", semestre: 2, creditos: 2, requisitos: [] },
      { nombre: "Química General", semestre: 2, creditos: 3, requisitos: [] },
      { nombre: "Teoría del Delito", semestre: 2, creditos: 3, requisitos: [] },
      { nombre: "Metodología de la Investigación", semestre: 2, creditos: 2, requisitos: [] },
      { nombre: "Diseño y Reconstrucción Forense", semestre: 2, creditos: 3, requisitos: ["Informática Forense"] },
      { nombre: "Ciencias Forenses", semestre: 2, creditos: 3, requisitos: ["Criminalística"] },
      { nombre: "Química Forense", semestre: 3, creditos: 3, requisitos: ["Química General"] },
      { nombre: "Física Forense", semestre: 3, creditos: 3, requisitos: ["Matemáticas Aplicadas a la Investigación Criminal"] },
      { nombre: "Derecho Penal Especial", semestre: 3, creditos: 3, requisitos: ["Teoría del Delito"] },
      { nombre: "Anatomía Humana", semestre: 3, creditos: 3, requisitos: ["Ciencias Forenses"] },
      { nombre: "Delitos Sexuales", semestre: 3, creditos: 1, requisitos: ["Teoría del Delito"] },
      { nombre: "Ética", semestre: 3, creditos: 1, requisitos: [] },
      { nombre: "Toxicología Forense", semestre: 4, creditos: 3, requisitos: ["Química Forense"] },
      { nombre: "Odontología Forense", semestre: 4, creditos: 3, requisitos: ["Anatomía Humana"] },
      { nombre: "Medicina Legal", semestre: 4, creditos: 3, requisitos: ["Anatomía Humana"] },
      { nombre: "Derecho Procesal Penal", semestre: 4, creditos: 2, requisitos: ["Derecho Penal Especial"] },
      { nombre: "Análisis Forense de Incendios y Explosivos", semestre: 4, creditos: 3, requisitos: ["Química Forense", "Física Forense"] },
      { nombre: "Libre Elección I", semestre: 4, creditos: 2, requisitos: [] },
      { nombre: "Antropología Forense", semestre: 5, creditos: 3, requisitos: ["Anatomía Humana"] },
      { nombre: "Pruebas Penales", semestre: 5, creditos: 3, requisitos: ["Derecho Procesal Penal"] },
      { nombre: "Lofoscopia", semestre: 5, creditos: 3, requisitos: ["Odontología Forense"] },
      { nombre: "Fotografía y Video Forense", semestre: 5, creditos: 3, requisitos: ["Ciencias Forenses"] },
      { nombre: "Criminología y Victimología", semestre: 5, creditos: 3, requisitos: ["Ciencias Forenses"] },
      { nombre: "Libre Elección II", semestre: 5, creditos: 2, requisitos: [] },
      { nombre: "Biología Forense", semestre: 6, creditos: 3, requisitos: ["Anatomía Humana"] },
      { nombre: "Topografía Forense", semestre: 6, creditos: 3, requisitos: ["Matemáticas Aplicadas a la Investigación Criminal"] },
      { nombre: "Derecho Internacional Humanitario", semestre: 6, creditos: 2, requisitos: ["Derecho Penal Especial"] },
      { nombre: "Inteligencia y Contrainteligencia", semestre: 6, creditos: 3, requisitos: ["Pruebas Penales"] },
      { nombre: "Estadística Criminal", semestre: 6, creditos: 3, requisitos: ["Matemáticas Aplicadas a la Investigación Criminal"] },
      { nombre: "Investigación Criminal I", semestre: 6, creditos: 3, requisitos: ["Criminología y Victimología"] },
      { nombre: "Libre Elección III", semestre: 6, creditos: 2, requisitos: [] },
      { nombre: "Grafología Forense", semestre: 7, creditos: 3, requisitos: ["Pruebas Penales"] },
      { nombre: "Balística y Hoplología", semestre: 7, creditos: 3, requisitos: ["Topografía Forense", "Física Forense"] },
      { nombre: "Morfología Facial Forense y Retrato Hablado", semestre: 7, creditos: 1, requisitos: ["Anatomía Humana"] },
      { nombre: "Seguridad y Análisis de Riesgo", semestre: 7, creditos: 3, requisitos: ["Investigación Criminal I"] },
      { nombre: "Investigación Criminal II", semestre: 7, creditos: 3, requisitos: ["Investigación Criminal I"] },
      { nombre: "Énfasis I", semestre: 7, creditos: 3, requisitos: ["Investigación Criminal I"] },
      { nombre: "Psicología Forense", semestre: 8, creditos: 3, requisitos: ["Criminología y Victimología"] },
      { nombre: "Delitos Informáticos", semestre: 8, creditos: 3, requisitos: ["Informática Forense", "Derecho Penal Especial"] },
      { nombre: "Criminalística de Campo", semestre: 8, creditos: 3, requisitos: ["Grafología Forense", "Balística y Hoplología", "Investigación Criminal II", "Psicología Forense"] },
      { nombre: "Genética Forense", semestre: 8, creditos: 2, requisitos: ["Biología Forense", "Química Forense"] },
      { nombre: "Microbiología y Entomología Forense", semestre: 8, creditos: 3, requisitos: ["Biología Forense"] },
      { nombre: "Crimen Transnacional", semestre: 8, creditos: 2, requisitos: ["Investigación Criminal II"] },
      { nombre: "Énfasis II", semestre: 8, creditos: 3, requisitos: ["Énfasis I"] },
      { nombre: "Documentología Forense", semestre: 9, creditos: 3, requisitos: ["Grafología Forense"] },
      { nombre: "Técnicas de Oralidad", semestre: 9, creditos: 2, requisitos: ["Derecho Procesal Penal"] },
      { nombre: "Técnicas de Entrevista e Interrogatorio", semestre: 9, creditos: 3, requisitos: ["Psicología Forense", "Investigación Criminal II"] },
      { nombre: "Psiquiatría Forense", semestre: 9, creditos: 3, requisitos: ["Psicología Forense"] },
      { nombre: "Práctica Profesional", semestre: 9, creditos: 3, requisitos: ["Énfasis II"] },
      { nombre: "Énfasis III", semestre: 9, creditos: 3, requisitos: ["Énfasis II"] }
    ];

    function crearInterfaz() {
      const contenedor = document.getElementById('contenedor');
      contenedor.innerHTML = "";
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

          const estado = localStorage.getItem(m.nombre);
          if (estado === 'vista') {
            div.classList.add('vista');
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
