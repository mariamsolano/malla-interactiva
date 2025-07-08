<!DOCTYPE html>
<html lang="es" data-theme="light">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Malla Interactiva - Ingeniería Electrónica y Telecomunicaciones</title>
  <style>
    :root {
      --bg-color-light: #eef1f5;
      --bg-color-dark: #121212;
      --text-color-light: #222;
      --text-color-dark: #e0e0e0;
      --card-color-light: #fff;
      --card-color-dark: #1e1e1e;
    }

    [data-theme="light"] {
      --bg-color: var(--bg-color-light);
      --text-color: var(--text-color-light);
      --card-color: var(--card-color-light);
    }

    [data-theme="dark"] {
      --bg-color: var(--bg-color-dark);
      --text-color: var(--text-color-dark);
      --card-color: var(--card-color-dark);
    }

    body {
      font-family: 'Segoe UI', sans-serif;
      background: var(--bg-color);
      margin: 0;
      padding: 2rem;
      color: var(--text-color);
    }
    h1 {
      text-align: center;
      margin-bottom: 2rem;
    }
    .grid {
      display: flex;
      gap: 1rem;
      overflow-x: auto;
      padding-bottom: 2rem;
    }
    .semestre {
      display: flex;
      flex-direction: column;
      gap: 0.5rem;
      background: var(--card-color);
      border-radius: 12px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.1);
      min-width: 260px;
      padding: 1rem;
    }
    .semestre h3 {
      text-align: center;
      border-bottom: 1px solid #ccc;
      padding-bottom: 0.5rem;
      margin-bottom: 1rem;
    }
    .materia {
      position: relative;
      padding: 1.5rem 0.75rem 1rem;
      border-radius: 10px;
      font-size: 0.85rem;
      cursor: pointer;
      user-select: none;
      border: 1px solid #888;
      transition: all 0.2s ease;
      box-shadow: 0 2px 6px rgba(0,0,0,0.1);
      overflow: hidden;
    }
    .materia:hover {
      transform: scale(1.02);
    }
    .materia .codigo, .materia .numero, .materia .creditos {
      position: absolute;
      background: rgba(255,255,255,0.7);
      padding: 2px 6px;
      border-radius: 4px;
      font-size: 0.7rem;
      font-weight: bold;
      z-index: 3;
    }
    [data-theme="dark"] .materia .codigo,
    [data-theme="dark"] .materia .numero,
    [data-theme="dark"] .materia .creditos {
      background: rgba(0,0,0,0.6);
    }
    .materia .numero {
      top: 4px;
      left: 4px;
    }
    .materia .codigo {
      top: 4px;
      right: 4px;
    }
    .materia .creditos {
      bottom: 4px;
      right: 4px;
    }
    .materia span.contenido {
      position: relative;
      z-index: 2;
      display: block;
      text-align: center;
      padding-top: 10px;
    }
    .materia[data-aprobada="true"]::after {
      content: "";
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background-image: linear-gradient(45deg, transparent 49%, var(--text-color) 50%, transparent 51%), linear-gradient(-45deg, transparent 49%, var(--text-color) 50%, transparent 51%);
      background-size: 100% 2px;
      background-repeat: no-repeat;
      background-position: center;
      z-index: 2;
    }
    .materia[data-bloqueada="true"]::before {
      content: "";
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      background: rgba(255,255,255,0.6);
      z-index: 4;
    }
    [data-theme="dark"] .materia[data-bloqueada="true"]::before {
      background: rgba(0,0,0,0.6);
    }
    .materia[data-bloqueada="true"] {
      cursor: not-allowed;
    }
    .materia[data-area="Matemáticas"] { background: #a1c4fd; }
    .materia[data-area="Física"] { background: #d3bfff; }
    .materia[data-area="Electricidad"] { background: #ffb3b3; }
    .materia[data-area="Electrónica"] { background: #ffd6a5; }
    .materia[data-area="Software"] { background: #caffbf; }
    .materia[data-area="Telecomunicaciones"] { background: #b5ead7; }
    .materia[data-area="Señales"] { background: #9bf6ff; }
    .materia[data-area="Humanísticas"] { background: #ffc6ff; }
    .materia[data-area="Proyectos"] { background: #fdffb6; }
    .materia[data-area="Electiva"] { background: #e0c3fc; }
  </style>
</head>
<body>
  <h1>Malla Interactiva - Ingeniería Electrónica y Telecomunicaciones</h1>
  <div class="grid" id="contenedor"></div>
  <script>
    const materias = [
      { numero: 1, codigo: 'MAT102.1', nombre: 'Álgebra Lineal', semestre: 1, area: 'Matemáticas', creditos: 3 },
      { numero: 2, codigo: 'MAT101.1', nombre: 'Cálculo Diferencial', semestre: 1, area: 'Matemáticas', creditos: 3 },
      { numero: 3, codigo: 'IAT101', nombre: 'Introducción a la Ingeniería', semestre: 1, area: 'Humanísticas', creditos: 2 },
      { numero: 4, codigo: 'BAI101', nombre: 'Introducción a los Circuitos Eléctricos', semestre: 1, area: 'Electricidad', creditos: 1 },
      { numero: 5, codigo: '21505', nombre: 'Lectura y Escritura', semestre: 1, area: 'Humanísticas', creditos: 2 },
      { numero: 6, codigo: 'M33530', nombre: 'Cálculo Integral', semestre: 2, area: 'Matemáticas', creditos: 3, prerequisitos: ['Cálculo Diferencial'] },
      { numero: 7, codigo: 'M33532', nombre: 'Circuitos de Corriente Directa', semestre: 2, area: 'Electricidad', creditos: 2, prerequisitos: ['Introducción a los Circuitos Eléctricos'] },
      { numero: 8, codigo: 'M34240', nombre: 'Ética', semestre: 2, area: 'Humanísticas', creditos: 2 },
      { numero: 9, codigo: 'M33531', nombre: 'Mecánica', semestre: 2, area: 'Física', creditos: 3 },
      { numero: 10, codigo: 'M33533', nombre: 'Programación Orientada a Objetos', semestre: 2, area: 'Software', creditos: 3 },
      { numero: 11, codigo: 'M33539', nombre: 'Cálculo Vectorial', semestre: 3, area: 'Matemáticas', creditos: 3, prerequisitos: ['Cálculo Integral'] },
      { numero: 12, codigo: 'M33541', nombre: 'Circuitos de Corriente Alterna', semestre: 3, area: 'Electricidad', creditos: 3, prerequisitos: ['Circuitos de Corriente Directa'] }
    ];

    const materiasPorSemestre = {};
    materias.forEach(m => {
      if (!materiasPorSemestre[m.semestre]) materiasPorSemestre[m.semestre] = [];
      materiasPorSemestre[m.semestre].push(m);
    });

    const contenedor = document.getElementById('contenedor');
    for (const semestre in materiasPorSemestre) {
      const columna = document.createElement('div');
      columna.className = 'semestre';
      const titulo = document.createElement('h3');
      titulo.textContent = `Semestre ${semestre}`;
      columna.appendChild(titulo);

      materiasPorSemestre[semestre].forEach(m => {
        const div = document.createElement('div');
        div.className = 'materia';
        div.dataset.area = m.area;
        div.dataset.nombre = m.nombre;
        div.innerHTML = `
          <div class="numero">${m.numero}</div>
          <div class="codigo">${m.codigo}</div>
          <div class="creditos">${m.creditos}</div>
          <span class="contenido">${m.nombre}</span>
        `;
        if (m.prerequisitos) {
          div.dataset.bloqueada = true;
        }
        div.addEventListener('click', () => alternarEstado(m, div));
        columna.appendChild(div);
      });

      contenedor.appendChild(columna);
    }

    function alternarEstado(materia, elemento) {
      if (elemento.dataset.bloqueada === 'true') return;
      const yaAprobada = elemento.dataset.aprobada === 'true';
      if (yaAprobada) {
        delete elemento.dataset.aprobada;
        volverABloquearDependientes(materia.nombre);
      } else {
        elemento.dataset.aprobada = true;
        desbloquearMaterias(materia.nombre);
      }
    }

    function desbloquearMaterias(nombreAprobado) {
      document.querySelectorAll('[data-bloqueada="true"]').forEach(el => {
        const mat = materias.find(m => m.nombre === el.dataset.nombre);
        if (mat && mat.prerequisitos.every(pr => estaAprobada(pr))) {
          el.dataset.bloqueada = false;
        }
      });
    }

    function volverABloquearDependientes(nombreDesmarcado) {
      document.querySelectorAll('.materia').forEach(el => {
        const mat = materias.find(m => m.nombre === el.dataset.nombre);
        if (mat && mat.prerequisitos && mat.prerequisitos.includes(nombreDesmarcado)) {
          el.dataset.bloqueada = true;
          delete el.dataset.aprobada;
          volverABloquearDependientes(mat.nombre);
        }
      });
    }

    function estaAprobada(nombre) {
      const el = document.querySelector(`.materia[data-nombre="${nombre}"]`);
      return el && el.dataset.aprobada === 'true';
    }
  </script>
</body>
</html>
