<!-- CLASIFICADOR INTERACTIVO DE NÚMEROS -->
<div class="simulador-box" style="max-width: 550px; margin: 30px auto; padding: 25px; border-radius: 12px; background-color: #f8f9fa; border: 1px solid #e9ecef; box-shadow: 0 4px 10px rgba(0,0,0,0.08);">
  <h3 style="margin-top:0; text-align: center; color: #0d6efd;">🎯 Clasificador Visual de Números</h3>
  <p style="font-size: 0.9em; color: #6c757d; text-align: center;">Ingresa un número o una expresión (ej: 5, -3, 0.75, pi, sqrt(2)) para ver a qué club pertenece.</p>
  
  <div class="form-group" style="margin-bottom: 15px;">
    <label for="numeroInput" style="display: block; font-weight: bold; margin-bottom: 5px;">Número o constante:</label>
    <input type="text" id="numeroInput" placeholder="Ej: 5, -12, 0.5, pi, sqrt(2)" style="width: 100%; padding: 10px; border-radius: 6px; border: 1px solid #ced4da; box-sizing: border-box;">
  </div>

  <button class="btn-calcular" onclick="clasificarNumero()" style="width: 100%; padding: 12px; background-color: #0d6efd; color: white; border: none; border-radius: 6px; font-weight: bold; cursor: pointer;">Clasificar Número</button>

  <div id="resultadoClasificacion" class="resultado-box" style="margin-top: 20px; padding: 16px; border-radius: 8px; background-color: #ffffff; border: 1px solid #dee2e6; display: none;">
    <h4 style="margin: 0 0 10px 0; color: #1a252c;">Pertenencia a los conjuntos:</h4>
    <ul id="listaConjuntos" style="padding-left: 20px; margin: 0; line-height: 1.8;">
    </ul>
  </div>
</div>

<script>
  function clasificarNumero() {
    const rawVal = document.getElementById('numeroInput').value.trim().toLowerCase();
    const resBox = document.getElementById('resultadoClasificacion');
    const lista = document.getElementById('listaConjuntos');
    lista.innerHTML = '';

    if (!rawVal) {
      alert("Por favor ingresa un número o expresión válida.");
      return;
    }

    let isNatural = false;
    let isInteger = false;
    let isRational = false;
    let isIrrational = false;
    let isReal = true;

    if (rawVal === 'pi' || rawVal === 'e' || rawVal.includes('sqrt')) {
      isIrrational = true;
    } else {
      const num = Number(rawVal);
      if (isNaN(num)) {
        alert("Expresión no reconocida. Prueba con un número, 'pi' o 'sqrt(X)'.");
        return;
      }

      if (Number.isInteger(num)) {
        isInteger = true;
        if (num > 0) {
          isNatural = true;
        }
        isRational = true;
      } else {
        isRational = true;
      }
    }

    const items = [
      { name: "Naturales (ℕ)", match: isNatural },
      { name: "Enteros (ℤ)", match: isInteger },
      { name: "Racionales (ℚ)", match: isRational },
      { name: "Irracionales (𝕀)", match: isIrrational },
      { name: "Reales (ℝ)", match: isReal }
    ];

    items.forEach(item => {
      const li = document.createElement('li');
      if (item.match) {
        li.innerHTML = `<strong>${item.name}:</strong> <span style="color: #198754; font-weight: bold;">✅ Pertenece</span>`;
      } else {
        li.innerHTML = `<strong>${item.name}:</strong> <span style="color: #dc3545;">❌ No pertenece</span>`;
      }
      lista.appendChild(li);
    });

    resBox.style.display = 'block';
  }
</script>
