<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>El Club de los Números & Simulador ZFC</title>
  <style>
    :root {
      --primary: #0d6efd;
      --primary-dark: #0b5ed7;
      --bg-light: #f8f9fa;
      --text-dark: #212529;
      --border: #e9ecef;
    }
    body {
      font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
      line-height: 1.6;
      color: var(--text-dark);
      max-width: 800px;
      margin: 0 auto;
      padding: 20px;
      background-color: #fff;
    }
    h1, h2 { text-align: center; color: var(--primary); }
    .card {
      background: var(--bg-light);
      border: 1px solid var(--border);
      border-radius: 10px;
      padding: 20px;
      margin-bottom: 25px;
      box-shadow: 0 4px 6px rgba(0,0,0,0.05);
    }
    .form-group { margin-bottom: 15px; }
    label { font-weight: bold; display: block; margin-bottom: 5px; }
    input[type="text"] {
      width: 100%;
      padding: 10px;
      border: 1px solid #ced4da;
      border-radius: 6px;
      box-sizing: border-box;
      font-size: 16px;
    }
    button {
      width: 100%;
      padding: 12px;
      background: var(--primary);
      color: white;
      border: none;
      border-radius: 6px;
      font-weight: bold;
      font-size: 16px;
      cursor: pointer;
      transition: background 0.2s;
    }
    button:hover { background: var(--primary-dark); }
    .results {
      margin-top: 15px;
      background: white;
      padding: 15px;
      border-radius: 6px;
      border: 1px solid #dee2e6;
    }
    .status-true { color: #198754; font-weight: bold; }
    .status-false { color: #dc3545; }
    ul { list-style: none; padding-left: 0; }
    li { padding: 6px 0; border-bottom: 1px solid var(--border); }
  </style>
</head>
<body>

  <h1>El club de los números: ¿Quién es quién en ℝ, ℤ, ℚ, 𝕀 y ℝ?</h1>
  <p style="text-align: center;">Ingresa cualquier número o expresión para comprobar dinámicamente a qué conjuntos numéricos pertenece.</p>

  <div class="card">
    <h2>🎯 Clasificador Dinámico de Números</h2>
    <div class="form-group">
      <label for="numberInput">Ingresa un valor (Ej: 5, -12, 3/4, 0.333, pi, e, sqrt(2)):</label>
      <input type="text" id="numberInput" placeholder="Escribe aquí tu número...">
    </div>
    <button onclick="evaluateNumber()">Clasificar Elemento</button>

   <div id="outputContainer" class="results" style="display:none;">
      <h3>Resultado de Pertenencia:</h3>
      <ul id="resultsList"></ul>
    </div>
  </div>

  <script>
    function parseInput(val) {
      val = val.trim().toLowerCase();
      if (!val) return null;

      // Evaluación de Constantes Irracionales conocidas
      if (val === 'pi' || val === 'π') return { value: Math.PI, isIrrational: true };
      if (val === 'e') return { value: Math.E, isIrrational: true };
      if (val.startsWith('sqrt(') && val.endsWith(')')) {
        let inside = parseFloat(val.replace('sqrt(', '').replace(')', ''));
        if (!isNaN(inside)) {
          let sqrtVal = Math.sqrt(inside);
          let isPerfectSquare = Number.isInteger(sqrtVal);
          return { value: sqrtVal, isIrrational: !isPerfectSquare };
        }
      }

      // Evaluación de Fracciones (a/b)
      if (val.includes('/')) {
        let parts = val.split('/');
        if (parts.length === 2) {
          let num = parseFloat(parts[0]);
          let den = parseFloat(parts[1]);
          if (!isNaN(num) && !isNaN(den) && den !== 0) {
            return { value: num / den, isFraction: true };
          }
        }
      }

      // Evaluar números decimales o enteros estándar
      let num = parseFloat(val);
      if (!isNaN(num)) {
        return { value: num, isStandard: true };
      }

      return null;
    }

    function evaluateNumber() {
      const inputStr = document.getElementById('numberInput').value;
      const parsed = parseInput(inputStr);
      const list = document.getElementById('resultsList');
      const container = document.getElementById('outputContainer');
      
      list.innerHTML = '';

      if (!parsed) {
        alert("Entrada no válida. Ingresa un entero, decimal, fracción (ej: 3/4), pi, e, o sqrt(X).");
        return;
      }

      let val = parsed.value;
      let isNat = Number.isInteger(val) && val > 0;
      let isInt = Number.isInteger(val);
      let isRat = parsed.isFraction || (!parsed.isIrrational && (isInt || !Number.isInteger(val)));
      let isIrr = !!parsed.isIrrational;
      let isReal = true; // Todo número procesado en esta escala es Real

      // Excepción para flotantes de JavaScript que representan enteros
      if (Math.abs(val - Math.round(val)) < 1e-9) {
        isInt = true;
        if (val > 0) isNat = true;
        isRat = true;
        isIrr = false;
      }

      const categories = [
        { name: "Naturales (ℕ)", match: isNat },
        { name: "Enteros (ℤ)", match: isInt },
        { name: "Racionales (ℚ)", match: isRat },
        { name: "Irracionales (𝕀)", match: isIrr },
        { name: "Reales (ℝ)", match: isReal }
      ];

      categories.forEach(item => {
        let li = document.createElement('li');
        let status = item.match 
          ? `<span class="status-true">✅ Pertenece</span>` 
          : `<span class="status-false">❌ No Pertenece</span>`;
        li.innerHTML = `<strong>${item.name}:</strong> ${status}`;
        list.appendChild(li);
      });

      container.style.display = 'block';
    }
  </script>
</body>
</html>
