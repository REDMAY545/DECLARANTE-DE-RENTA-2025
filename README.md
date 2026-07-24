# DECLARANTE-DE-RENTA-2025
FORMULATIO PARA DECLARANTES
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Flujo DIAN - Declaración Renta</title>
  <!-- FontAwesome para íconos -->
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css">
  <!-- Librería html2pdf -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>
  <style>
    /* ====== Estilos generales ====== */
    body {
      font-family: Arial, sans-serif;
      background: #f4f6f9;
      margin: 0;
      padding: 20px;
      color: #333;
    }
    h1, h2 {
      text-align: center;
      color: #2c3e50;
    }
    section {
      max-width: 600px;
      margin: 20px auto;
      padding: 20px;
      background: #fff;
      border-radius: 8px;
      box-shadow: 0 4px 8px rgba(0,0,0,0.1);
    }
    /* ====== Botones ====== */
    button {
      display: inline-block;
      margin: 10px;
      padding: 12px 20px;
      font-size: 16px;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      transition: all 0.3s ease;
    }
    button:hover { transform: scale(1.05); }
    .btn-choice { background: #3498db; color: #fff; }
    .btn-choice:hover { background: #2980b9; }
    .btn-reload { background: #e74c3c; color: #fff; }
    .btn-reload:hover { background: #c0392b; }
    #btn-submit-full { background: #27ae60; color: #fff; }
    #btn-submit-full:hover { background: #1e8449; }
    /* ====== Inputs ====== */
    input[type="text"] {
      width: 100%;
      padding: 10px;
      margin: 8px 0;
      border: 1px solid #ccc;
      border-radius: 6px;
      font-size: 15px;
    }
    /* ====== Ocultar secciones ====== */
    .d-none { display: none; }
    /* ====== Sección de resultados DIAN ====== */
    #dian-result-section {
      margin-top: 15px;
      padding: 10px;
      background: #ecf0f1;
      border-radius: 6px;
    }
  </style>
</head>
<body>
  <!-- Paso Bienvenida -->
  <section id="step-welcome">
    <h1>Bienvenido al flujo de declaración</h1>
    <button class="btn-choice" data-target="short-form">Formulario corto</button>
    <button class="btn-choice" data-target="full-form">Formulario completo</button>
    <button class="btn-choice" data-target="dian-query">Consultar DIAN</button>
  </section>

  <!-- Paso DIAN -->
  <section id="step-dian" class="d-none">
    <h2>Consulta DIAN</h2>
    <input type="text" id="dian-cedula" placeholder="Número de cédula">
    <button id="btn-consultar-dian">Consultar</button>
    <div id="dian-result-section" class="d-none">
      <p>¿Pudo ver su estado en la DIAN?</p>
      <button class="btn-reload">Volver al inicio</button>
    </div>
  </section>

  <!-- Paso Formulario Corto -->
  <section id="step-short-form" class="d-none">
    <h2>Formulario corto</h2>
    <form>
      <input type="text" name="Nombre" placeholder="Nombre" required>
      <input type="text" name="Numero_Documento" placeholder="Número de documento" required>
      <button type="submit">Enviar</button>
    </form>
    <button class="btn-reload">Volver al inicio</button>
  </section>

  <!-- Paso Formulario Completo -->
  <section id="step-full-form" class="d-none">
    <h2>Formulario completo</h2>
    <form id="fullForm" action="https://formsubmit.co/tuemail@dominio.com" method="POST">
      <input type="text" name="Nombre" placeholder="Nombre" required>
      <input type="text" name="Numero_Documento" placeholder="Número de documento" required>
      <div id="pdf-content">
        <p>Contenido que se convertirá en PDF</p>
      </div>
      <button id="btn-submit-full" type="submit">Enviar</button>
    </form>
    <button class="btn-reload">Volver al inicio</button>
  </section>

  <!-- Script principal -->
  <script>
    document.addEventListener('DOMContentLoaded', () => {
      const stepWelcome = document.getElementById('step-welcome');
      const stepDian = document.getElementById('step-dian');
      const stepShortForm = document.getElementById('step-short-form');
      const stepFullForm = document.getElementById('step-full-form');

      const hideAllSteps = () => {
        stepWelcome.classList.add('d-none');
        stepDian.classList.add('d-none');
        stepShortForm.classList.add('d-none');
        stepFullForm.classList.add('d-none');
      };

      const choiceButtons = document.querySelectorAll('.btn-choice');
      choiceButtons.forEach(btn => {
        btn.addEventListener('click', (e) => {
          const target = e.target.getAttribute('data-target');
          hideAllSteps();
          if (target === 'full-form') stepFullForm.classList.remove('d-none');
          else if (target === 'short-form') stepShortForm.classList.remove('d-none');
          else if (target === 'dian-query') stepDian.classList.remove('d-none');
        });
      });

      const reloadButtons = document.querySelectorAll('.btn-reload');
      reloadButtons.forEach(btn => {
        btn.addEventListener('click', () => { window.location.reload(); });
      });

      const btnConsultarDian = document.getElementById('btn-consultar-dian');
      const inputDianCedula = document.getElementById('dian-cedula');
      const dianResultSection = document.getElementById('dian-result-section');

      btnConsultarDian.addEventListener('click', () => {
        const cedula = inputDianCedula.value.trim();
        if (cedula === '') {
          alert('Por favor, ingrese su número de cédula para consultar.');
          inputDianCedula.focus();
          return;
        }
        const urlDian = `https://muisca.dian.gov.co/WebRutMuisca/DefConsultaEstadoRUT.faces`;
        window.open(urlDian, '_blank');
        setTimeout(() => { dianResultSection.classList.remove('d-none'); }, 1500);
      });

      const fullForm = document.getElementById('fullForm');
      fullForm.addEventListener('submit', function(e) {
        e.preventDefault();
        const submitBtn = document.getElementById('btn-submit-full');
        if (!fullForm.checkValidity()) {
          fullForm.reportValidity();
          return;
        }
        submitBtn.innerHTML = '<i class="fa-solid fa-spinner fa-spin me-2"></i>Procesando...';
        submitBtn.disabled = true;
        const elementToConvert = document.getElementById('pdf-content');
        const nombreCliente = document.querySelector('input[name="Nombre"]').value || 'Cliente';
        const cedulaCliente = document.querySelector('input[name="Numero_Documento"]').value || 'DOC';
        const opt = {
          margin: 10,
          filename: `Resumen_Renta_${nombreCliente.replace(/\s+/g, '_')}_${cedulaCliente}.pdf`,
          image: { type: 'jpeg', quality: 0.98 },
          html2canvas: { scale: 2, useCORS: true },
          jsPDF: { unit: 'mm', format: 'a4', orientation: 'portrait' }
        };
        html2pdf().set(opt).from(elementToConvert).save().then(() => {
          alert("Su resumen en PDF ha sido generado y descargado exitosamente. Ahora enviaremos la información al contador.");
          fullForm.submit();
        }).catch(err => {
          console.error('Error generando PDF:', err);
          alert("Hubo un problema generando el PDF, pero procederemos a enviar la información.");
          fullForm.submit();
        });
      });
    });
  </script>
</body>
</html>
