<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ARKOS S.A. - Evaluación de Desempeño</title>
<script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
<style>
  :root {
    --primary-red: #8B0000;
    --dark-red: #660000;
    --light-red: #B22222;
    --bg-light: #FDF8F8;
    --card-bg: #FFFFFF;
    --text-dark: #2A2A2A;
    --text-muted: #666666;
    --border-color: #E5D5D5;
    --accent-blue: #1A5F7A;
  }

  * { box-sizing: border-box; }
  body {
    margin: 0;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background-color: var(--dark-red);
    color: var(--text-dark);
  }

  /* MODALES Y LOGIN */
  .overlay {
    position: fixed;
    top: 0; left: 0; width: 100%; height: 100%;
    background: linear-gradient(135deg, var(--dark-red), var(--primary-red));
    display: flex; justify-content: center; align-items: center;
    z-index: 1000;
  }
  .card-box {
    background: #fff; padding: 35px 40px; border-radius: 12px;
    width: 100%; max-width: 420px; box-shadow: 0 10px 25px rgba(0,0,0,0.4);
    text-align: center;
  }
  .card-box h2 { color: var(--primary-red); margin-bottom: 5px; margin-top: 0; font-size: 1.8rem; font-weight: bold; }
  .card-box p { color: var(--text-muted); font-size: 0.9rem; margin-bottom: 20px; }
  
  .form-group { text-align: left; margin-bottom: 15px; }
  .form-group label { display: block; font-size: 0.85rem; font-weight: bold; margin-bottom: 5px; color: var(--primary-red); }
  .form-group input, .form-group select, .form-group textarea {
    width: 100%; padding: 10px; border: 1px solid var(--border-color);
    border-radius: 6px; font-size: 0.95rem; outline: none; font-family: inherit;
  }
  .form-group input:focus, .form-group select:focus, .form-group textarea:focus { border-color: var(--primary-red); }
  
  .btn-action {
    width: 100%; padding: 12px; background: var(--primary-red); color: white;
    border: none; border-radius: 6px; font-weight: bold; font-size: 1rem;
    cursor: pointer; transition: background 0.3s; margin-top: 5px;
  }
  .btn-action:hover { background: var(--dark-red); }
  .btn-cancel { background: #e0e0e0; color: #333; margin-top: 8px; }
  .btn-cancel:hover { background: #d0d0d0; }

  .error-msg { color: var(--light-red); font-size: 0.85rem; margin-top: 10px; display: none; font-weight: bold; }
  .success-msg { color: #2F7D5A; font-size: 0.85rem; margin-top: 10px; display: none; font-weight: bold; }

  .forgot-container { margin-top: 18px; padding-top: 12px; border-top: 1px solid #eee; }
  .forgot-link {
    display: inline-block; font-size: 0.85rem;
    color: var(--primary-red); text-decoration: none; font-weight: 600; cursor: pointer;
  }
  .forgot-link:hover { text-decoration: underline; }

  /* APP PRINCIPAL */
  .app-container { display: none; background-color: var(--bg-light); min-height: 100vh; padding: 20px 0; }
  .main-content { max-width: 1000px; margin: 0 auto; padding: 0 20px; }
  
  .masthead {
    background: linear-gradient(135deg, var(--dark-red), var(--light-red));
    color: white; padding: 20px 30px; border-radius: 10px;
    display: flex; justify-content: space-between; align-items: center;
    margin-bottom: 25px; box-shadow: 0 4px 12px rgba(139, 0, 0, 0.2);
  }
  .brand-title h1 { margin: 0; font-size: 1.8rem; letter-spacing: 1px; }
  .brand-title p { margin: 5px 0 0 0; opacity: 0.85; font-size: 0.9rem; }
  .user-header-controls { display: flex; align-items: center; gap: 15px; }
  .user-badge { background: rgba(255,255,255,0.2); padding: 8px 15px; border-radius: 20px; font-size: 0.85rem; font-weight: 600; }
  
  .btn-logout {
    background: #ffffff; color: var(--primary-red); border: none; padding: 8px 16px;
    border-radius: 20px; font-weight: bold; font-size: 0.85rem; cursor: pointer;
    transition: all 0.2s ease;
  }
  .btn-logout:hover { background: #f0f0f0; transform: translateY(-1px); }

  .card {
    background: var(--card-bg); border-radius: 8px; border: 1px solid var(--border-color);
    padding: 20px 25px; margin-bottom: 20px; box-shadow: 0 2px 5px rgba(0,0,0,0.03);
  }
  .card h2 { color: var(--primary-red); font-size: 1.1rem; border-bottom: 2px solid var(--primary-red); padding-bottom: 8px; margin-top: 0; }
  .grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; }

  /* ESTILOS DE LA SECCIÓN DE INSTRUCCIONES */
  .instructions-card {
    background: #FFFDF9;
    border-left: 5px solid var(--primary-red);
    border-top: 1px solid var(--border-color);
    border-right: 1px solid var(--border-color);
    border-bottom: 1px solid var(--border-color);
    border-radius: 8px;
    padding: 20px 25px;
    margin-bottom: 25px;
    box-shadow: 0 2px 6px rgba(139, 0, 0, 0.05);
  }
  .instructions-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    cursor: pointer;
    user-select: none;
  }
  .instructions-title {
    color: var(--primary-red);
    font-size: 1.15rem;
    font-weight: bold;
    margin: 0;
    display: flex;
    align-items: center;
    gap: 8px;
  }
  .instructions-toggle-btn {
    background: rgba(139, 0, 0, 0.08);
    color: var(--primary-red);
    border: none;
    padding: 5px 12px;
    border-radius: 15px;
    font-size: 0.8rem;
    font-weight: bold;
    cursor: pointer;
    transition: background 0.2s;
  }
  .instructions-toggle-btn:hover {
    background: rgba(139, 0, 0, 0.15);
  }
  .instructions-content {
    margin-top: 15px;
    padding-top: 15px;
    border-top: 1px dashed var(--border-color);
    font-size: 0.88rem;
    line-height: 1.55;
    color: #333;
  }
  .instructions-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 15px;
    margin-top: 10px;
  }
  @media (max-width: 768px) {
    .instructions-grid { grid-template-columns: 1fr; }
  }
  .instruction-item {
    background: #FFFFFF;
    border: 1px solid #EFE6E6;
    border-radius: 6px;
    padding: 12px 15px;
  }
  .instruction-item strong {
    color: var(--primary-red);
    display: block;
    margin-bottom: 4px;
    font-size: 0.9rem;
  }
  .scale-list {
    margin: 6px 0 0 0;
    padding-left: 18px;
    font-size: 0.85rem;
  }
  .formula-box {
    background: #F8F9FA;
    border-left: 3px solid var(--accent-blue);
    padding: 8px 12px;
    margin-top: 6px;
    font-family: monospace;
    font-size: 0.83rem;
    color: #2c3e50;
    border-radius: 0 4px 4px 0;
  }

  table { width: 100%; border-collapse: collapse; margin-top: 10px; }
  th { background: var(--primary-red); color: white; padding: 10px; text-align: left; font-size: 0.85rem; }
  td { padding: 10px; border-bottom: 1px solid var(--border-color); font-size: 0.9rem; }
  td input { width: 100%; padding: 6px; border: 1px solid var(--border-color); border-radius: 4px; }

  .btn {
    padding: 10px 20px; border: none; border-radius: 6px; font-weight: bold;
    cursor: pointer; font-size: 0.9rem; transition: background 0.2s;
  }
  .btn-primary { background: var(--primary-red); color: white; }
  .btn-primary:hover { background: var(--dark-red); }
  .btn-secondary { background: #e0e0e0; color: #333; }
  .btn-secondary:hover { background: #d0d0d0; }

  .final-box {
    background: var(--bg-light); border: 2px solid var(--primary-red);
    border-radius: 8px; padding: 20px; text-align: center; margin-top: 15px;
  }
  .final-score { font-size: 2.5rem; font-weight: bold; color: var(--primary-red); }
  .final-concept { font-weight: bold; padding: 6px 16px; border-radius: 12px; color: white; display: inline-block; margin-top: 8px; }

  .badge-concept {
    font-size: 0.75rem; font-weight: bold; padding: 4px 10px; border-radius: 10px; color: white; display: inline-block;
  }
  .empty-history { text-align: center; color: var(--text-muted); font-style: italic; padding: 15px 0; }
  .comp-desc { font-size: 0.8rem; color: var(--text-muted); margin-top: 4px; display: block; }

  /* ESTILOS EXCLUSIVOS PARA EL REPORTE PDF GENERADO */
  .pdf-template {
    position: absolute;
    left: -9999px;
    top: 0;
    width: 700px;
    padding: 25px;
    background: #ffffff;
    color: #222222;
    font-family: Arial, sans-serif;
    box-sizing: border-box;
  }
  .pdf-header {
    border-bottom: 3px solid #8B0000;
    padding-bottom: 10px;
    margin-bottom: 15px;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  .pdf-header h1 { color: #8B0000; margin: 0; font-size: 20px; }
  .pdf-header p { margin: 3px 0 0 0; color: #555; font-size: 11px; }
  .pdf-section-title {
    background: #8B0000;
    color: #ffffff;
    padding: 5px 8px;
    font-size: 12px;
    font-weight: bold;
    margin-top: 14px;
    margin-bottom: 8px;
    border-radius: 2px;
  }
  .pdf-grid { width: 100%; border-collapse: collapse; margin-bottom: 10px; }
  .pdf-grid td { padding: 5px 8px; font-size: 11px; border: 1px solid #ccc; }
  .pdf-grid td.label { background: #f4f4f4; font-weight: bold; color: #8B0000; width: 30%; }
  .pdf-table { width: 100%; border-collapse: collapse; margin-bottom: 10px; }
  .pdf-table th { background: #eeeeee; color: #222; border: 1px solid #ccc; padding: 5px; font-size: 11px; text-align: left; }
  .pdf-table td { border: 1px solid #ddd; padding: 5px; font-size: 10px; }
  .pdf-box { border: 1px solid #ccc; background: #fafafa; padding: 8px; border-radius: 3px; font-size: 10px; margin-bottom: 10px; min-height: 35px; }
  .pdf-signatures { display: flex; justify-content: space-between; margin-top: 35px; }
  .pdf-sig-line { width: 30%; text-align: center; border-top: 1px solid #444; padding-top: 4px; font-size: 10px; font-weight: bold; }

  /* MODAL DE PROCESO DE DESCARGA Y NOTIFICACIÓN */
  .status-step {
    padding: 10px;
    border-radius: 6px;
    background: #f5f5f5;
    margin-bottom: 10px;
    font-size: 0.9rem;
    text-align: left;
    display: flex;
    align-items: center;
    gap: 10px;
  }
  .status-step.active {
    background: #EBF3ED;
    border: 1px solid #2F7D5A;
    color: #2F7D5A;
    font-weight: bold;
  }

  .pdf-capture-mode .no-pdf { display: none !important; }
</style>
</head>
<body>

  <!-- MODAL INFORMATIVO DE FINALIZACIÓN -->
  <div class="overlay" id="processModal" style="display: none;">
    <div class="card-box">
      <h2 id="processTitle">Generando Reporte</h2>
      <p id="processSubtitle">Enviando la evaluación al servidor...</p>

      <div class="status-step active" id="step1">
        <span>📤</span> 1. Enviando datos y generando el PDF...
      </div>
      <div class="status-step" id="step2">
        <span>✉️</span> 2. Enviando el correo desde Talento Humano...
      </div>

      <div style="margin-top: 15px; font-size: 0.8rem; color: #666; text-align: left; background: #f0f6ff; padding: 10px; border-radius: 6px; border: 1px solid #cfe0f7;">
        El PDF se genera y el correo se envía automáticamente desde <strong>talentohumano@arkos.com.co</strong>. No necesitas adjuntar ni enviar nada manualmente.
      </div>
      <div id="processError" style="display:none; margin-top: 8px; font-size: 0.8rem; color: #B54444; text-align: left; background: #fdf0f0; padding: 10px; border-radius: 6px; border: 1px solid #f0c9c9;"></div>
    </div>
  </div>

  <!-- LOGIN -->
  <div class="overlay" id="loginOverlay">
    <div class="card-box">
      <h2>ARKOS S.A.</h2>
      <p>Sistema de Evaluación de Desempeño</p>
      <form id="loginForm" onsubmit="handleLogin(event)">
        <div class="form-group">
          <label for="username">Usuario / Documento</label>
          <input type="text" id="username" required placeholder="Ingrese su usuario">
        </div>
        <div class="form-group">
          <label for="password">Contraseña</label>
          <input type="password" id="password" required placeholder="••••••••">
        </div>
        <button type="submit" class="btn-action">Iniciar Sesión</button>
        <div class="error-msg" id="loginError">Usuario o contraseña incorrectos</div>
      </form>
    </div>
  </div>

  <!-- APP PRINCIPAL -->
  <div class="app-container" id="appContainer">
    <div class="main-content">
      
      <div class="masthead">
        <div class="brand-title">
          <h1>ARKOS S.A.</h1>
          <p>Evaluación Integral de Desempeño Laboral</p>
        </div>
        <div class="user-header-controls">
          <span class="user-badge" id="userDisplay">Usuario</span>
          <button class="btn-logout" onclick="handleLogout()">Cerrar Sesión</button>
        </div>
      </div>

      <!-- SECCIÓN DE INSTRUCCIONES INTEGRADAS -->
      <div class="instructions-card no-pdf">
        <div class="instructions-header" onclick="toggleInstructions()">
          <h3 class="instructions-title">
            <span>📌</span> Instrucciones de Diligenciamiento de Compromisos Laborales
          </h3>
          <button type="button" class="instructions-toggle-btn" id="instructionsToggleBtn">Ocultar ▲</button>
        </div>
        <div class="instructions-content" id="instructionsBody">
          <p style="margin-top: 0; color: #555;">
            Siga cuidadosamente las siguientes pautas para la definición, medición y ponderación de cada compromiso laboral:
          </p>
          <div class="instructions-grid">
            <div class="instruction-item">
              <strong>1. Compromiso</strong>
              Describa el compromiso, actividad o tarea que se derive de las funciones del cargo, del plan operativo o de las actividades propias del área.<br>
              <em>Estructura obligatoria:</em> <code>verbo + objeto + condición</code>.
            </div>

            <div class="instruction-item">
              <strong>2. Indicador de Cumplimiento (Peso)</strong>
              Asigne a cada compromiso un porcentaje de ponderación de acuerdo con su impacto y contribución al logro de las metas y objetivos de la dependencia.<br>
              <strong>Nota:</strong> La sumatoria de la ponderación de todos los compromisos debe sumar exactamente <strong>100%</strong>.
            </div>

            <div class="instruction-item">
              <strong>3. Evidencia o Descripción del Compromiso</strong>
              Defina junto con el evaluado la forma de medir el cumplimiento mediante indicadores claros, verificables y objetivos.<br>
              <em>Ejemplo:</em> <code>(Número de visitas a clientes realizadas / Número de visitas a clientes programadas) × 100</code>.
            </div>

            <div class="instruction-item">
              <strong>4. Porcentaje de Logro</strong>
              Asigne una calificación entre 1% y 100% basada en las evidencias obtenidas, según la siguiente escala:
              <ul class="scale-list">
                <li><strong>Cumplimiento total:</strong> 100%</li>
                <li><strong>Cumplimiento alto:</strong> 90% a 99%</li>
                <li><strong>Cumplimiento parcial:</strong> 70% a 89%</li>
                <li><strong>Cumplimiento bajo:</strong> Menor al 70%</li>
              </ul>
            </div>

            <div class="instruction-item">
              <strong>5. Nota del Compromiso</strong>
              Se calcula automáticamente en escala de 0 a 5 según el porcentaje alcanzado:
              <div class="formula-box">Nota del Compromiso = (# cantidad de compromisos × Porcentaje de Logro) ÷ 100</div>
            </div>

            <div class="instruction-item">
              <strong>6. Puntaje y Sumatoria Final</strong>
              Se calcula automáticamente multiplicando el logro por el peso asignado:
              <div class="formula-box">Puntaje = (Porcentaje de Logro × Peso del Compromiso × 5) ÷ 100</div>
              <strong>Sumatoria de Puntajes:</strong> Corresponde a la calificación consolidada del componente de Compromisos Laborales (peso del <strong>70%</strong> en la evaluación de desempeño).
            </div>
          </div>
        </div>
      </div>

    </div>
  </div>

  <script>
    function toggleInstructions() {
      const body = document.getElementById('instructionsBody');
      const btn = document.getElementById('instructionsToggleBtn');
      if (body.style.display === 'none') {
        body.style.display = 'block';
        btn.innerText = 'Ocultar ▲';
      } else {
        body.style.display = 'none';
        btn.innerText = 'Mostrar ▼';
      }
    }
  </script>
</body>
</html>
