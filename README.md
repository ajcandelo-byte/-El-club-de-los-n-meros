<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Simulador de Teoría de Conjuntos - Teoremas & Álgebra</title>
  <style>
    /* Variables de Color Adaptables a tu Blog */
    :root {
      --primary: #0d6efd;       /* Azul Principal */
      --secondary: #6f42c1;     /* Violeta Teoremas */
      --success: #198754;       /* Verde Éxito */
      --danger: #dc3545;        /* Rojo Alerta */
      --bg-body: #f4f6f9;       /* Fondo General */
      --bg-card: #ffffff;       /* Tarjeta de Contenido */
      --border-color: #e3e6f0;   /* Bordes */
      --text-dark: #212529;
    }

   body {
      font-family: 'Segoe UI', system-ui, -apple-system, sans-serif;
      background-color: var(--bg-body);
      color: var(--text-dark);
      line-height: 1.6;
      margin: 0;
      padding: 20px;
    }

  .container {
      max-width: 850px;
      margin: 0 auto;
      background: var(--bg-card);
      border-radius: 12px;
      padding: 25px;
      box-shadow: 0 4px 15px rgba(0,0,0,0.08);
      border: 1px solid var(--border-color);
    }

  h1, h2 {
      text-align: center;
      color: var(--primary);
      margin-bottom: 10px;
    }

   .subtitle {
      text-align: center;
      color: #6c757d;
      font-size: 0.95em;
      margin-bottom: 25px;
    }

   .grid-inputs {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 15px;
      margin-bottom: 20px;
    }

   .form-group label {
      display: block;
      font-weight: 600;
      margin-bottom: 5px;
      color: #495057;
    }

   .form-group input, .form-group select {
      width: 100%;
      padding: 10px;
      border: 1px solid #ced4da;
      border-radius: 6px;
      box-sizing: border-box;
      font-size: 14px;
    }

  .btn-action {
      width: 100%;
      padding: 12px;
      background-color: var(--primary);
      color: #fff;
      border: none;
      border-radius: 6px;
      font-weight: bold;
      font-size: 15px;
      cursor: pointer;
      transition: background 0.2s;
    }

  .btn-action:hover {
      background-color: #0b5ed7;
    }

  .theorem-box {
      margin-top: 25px;
      padding: 20px;
      border-radius: 8px;
      background-color: #f8f9fa;
      border-left: 5px solid var(--secondary);
    }

   .badge-check {
      display: inline-block;
      padding: 4px 8px;
      border-radius: 4px;
      font-weight: bold;
      color: #fff;
    }
    .badge-success { background-color: var(--success); }
    .badge-danger { background-color: var(--danger); }

  .result-list {
      list-style: none;
      padding-left: 0;
    }

   .result-list li {
      padding: 8px 12px;
      margin-bottom: 6px;
      border-radius: 4px;
      background: #fff;
      border: 1px solid var(--border-color);
      font-family: monospace;
      font-size: 0.95em;
    }
  </style>
</head>
<body>

  <div class="container">
    <h1>🧠 Resolver Teoremas y Operaciones de Conjuntos</h1>
    <p class="subtitle">Basado en la axiomatización de Zermelo-Fraenkel (Hernández Hernández, 1998)</p>

    <!-- ENTRADA DE DATOS PARA UNIVERSO Y CONJUNTOS -->
   <div class="grid-inputs">
      <div class="form-group">
        <label for="setU">Universo (U):</label>
        <input type="text" id="setU" value="1, 2, 3, 4, 5, 6, 7, 8, 9, 10">
      </div>
      <div class="form-group">
        <label for="setA">Conjunto A:</label>
        <input type="text" id="setA" value="1, 2, 3, 4, 5">
      </div>
      <div class="form-group">
        <label for="setB">Conjunto B:</label>
        <input type="text" id="setB" value="4, 5, 6, 7">
      </div>
    </div>

  <button class="btn-action" onclick="solveSetsAndTheorems()">Calcular Operaciones y Validar Teoremas</button>

    <!-- RESULTADOS DE OPERACIONES BÁSICAS -->
   <div class="theorem-box">
      <h3>📊 Operaciones Fundamentales</h3>
      <ul class="result-list">
        <li><strong>A ∪ B (Unión):</strong> <span id="outUnion">-</span></li>
        <li><strong>A ∩ B (Intersección):</strong> <span id="outInter">-</span></li>
        <li><strong>A \ B (Diferencia):</strong> <span id="outDiffAB">-</span></li>
        <li><strong>Aᶜ (Complemento de A):</strong> <span id="outCompA">-</span></li>
        <li><strong>Bᶜ (Complemento de B):</strong> <span id="outCompB">-</span></li>
      </ul>
    </div>

      <!-- VERIFICACIÓN DE TEOREMAS Y LEYES -->
   <div class="theorem-box" style="border-left-color: var(--primary);">
      <h3>📜 Verificación de Teoremas del Álgebra de Conjuntos</h3>
      
  <div class="form-group">
        <label for="theoremSelect">Selecciona un Teorema a Demostrar con tus Datos:</label>
        <select id="theoremSelect" onchange="solveSetsAndTheorems()">
          <option value="demorgan1">Primera Ley de De Morgan: (A ∪ B)ᶜ = Aᶜ ∩ Bᶜ</option>
          <option value="demorgan2">Segunda Ley de De Morgan: (A ∩ B)ᶜ = Aᶜ ∪ Bᶜ</option>
          <option value="diff_identity">Teorema de la Diferencia: A \ B = A ∩ Bᶜ</option>
          <option value="complement_union">Ley del Complemento: A ∪ Aᶜ = U</option>
        </select>
      </div>

   <div id="theoremProofResult" style="margin-top: 15px;"></div>
    </div>
  </div>

  <!-- LÓGICA DE PROCESAMIENTO MATEMÁTICO EN JAVASCRIPT -->
  <script>
    function parseSet(str) {
      if (!str.trim()) return [];
      return Array.from(new Set(str.split(',').map(e => e.trim()).filter(e => e !== '')));
    }

    function formatSet(arr) {
      return arr.length === 0 ? '∅' : '{ ' + arr.sort().join(', ') + ' }';
    }

    function solveSetsAndTheorems() {
      const U = parseSet(document.getElementById('setU').value);
      const A = parseSet(document.getElementById('setA').value);
      const B = parseSet(document.getElementById('setB').value);

      // Operaciones Báicas
      const union = Array.from(new Set([...A, ...B]));
      const inter = A.filter(x => B.includes(x));
      const diffAB = A.filter(x => !B.includes(x));
      const compA = U.filter(x => !A.includes(x));
      const compB = U.filter(x => !B.includes(x));

      document.getElementById('outUnion').innerText = formatSet(union);
      document.getElementById('outInter').innerText = formatSet(inter);
      document.getElementById('outDiffAB').innerText = formatSet(diffAB);
      document.getElementById('outCompA').innerText = formatSet(compA);
      document.getElementById('outCompB').innerText = formatSet(compB);

      // Verificación de Teoremas
      const selectedTheorem = document.getElementById('theoremSelect').value;
      const proofDiv = document.getElementById('theoremProofResult');

      let lhs = [], rhs = [], formulaText = "", isValid = false;

      if (selectedTheorem === 'demorgan1') {
        formulaText = "(A ∪ B)ᶜ = Aᶜ ∩ Bᶜ";
        lhs = U.filter(x => !union.includes(x)); // (A ∪ B)ᶜ
        rhs = compA.filter(x => compB.includes(x)); // Aᶜ ∩ Bᶜ
      } else if (selectedTheorem === 'demorgan2') {
        formulaText = "(A ∩ B)ᶜ = Aᶜ ∪ Bᶜ";
        lhs = U.filter(x => !inter.includes(x)); // (A ∩ B)ᶜ
        rhs = Array.from(new Set([...compA, ...compB])); // Aᶜ ∪ Bᶜ
      } else if (selectedTheorem === 'diff_identity') {
        formulaText = "A \\ B = A ∩ Bᶜ";
        lhs = diffAB; // A \ B
        rhs = A.filter(x => compB.includes(x)); // A ∩ Bᶜ
      } else if (selectedTheorem === 'complement_union') {
        formulaText = "A ∪ Aᶜ = U";
        lhs = Array.from(new Set([...A, ...compA])); // A ∪ Aᶜ
        rhs = U; // Universo U
      }

      isValid = JSON.stringify(lhs.sort()) === JSON.stringify(rhs.sort());

      proofDiv.innerHTML = `
        <p><strong>Teorema evaluado:</strong> <code>${formulaText}</code></p>
        <p><strong>Lado Izquierdo (LHS):</strong> ${formatSet(lhs)}</p>
        <p><strong>Lado Derecho (RHS):</strong> ${formatSet(rhs)}</p>
        <p><strong>Resultado del Teorema:</strong> 
          <span class="badge-check ${isValid ? 'badge-success' : 'badge-danger'}">
            ${isValid ? '✅ TEOREMA DEMOSTRADO Y CUMPLIDO' : '❌ NO CUMPLE (Verificar Universo)'}
          </span>
        </p>
      `;
    }

    // Ejecución inicial al cargar
    solveSetsAndTheorems();
  </script>
</body>
</html>
