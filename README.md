<!DOCTYPE html>
<html lang="es" data-theme="dark">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="theme-color" content="#0f172a">
<title>MiTracke — Seguidor Académico</title>

<style>
:root[data-theme="dark"]{
  --bg:#0f172a;--card:#1e293b;--card2:#172033;--text:#f8fafc;
  --muted:#94a3b8;--accent:#3b82f6;--accent2:#2563eb;
  --success:#22c55e;--danger:#ef4444;--border:#334155;--gold:#f59e0b;
  --overlay:rgba(0,0,0,.75);
}
:root[data-theme="light"]{
  --bg:#f1f5f9;--card:#fff;--card2:#f8fafc;--text:#0f172a;
  --muted:#64748b;--accent:#2563eb;--accent2:#1d4ed8;
  --success:#16a34a;--danger:#dc2626;--border:#cbd5e1;--gold:#d97706;
  --overlay:rgba(0,0,0,.5);
}
*{box-sizing:border-box;margin:0;padding:0;font-family:Segoe UI,Tahoma,Verdana,sans-serif}
body{background:var(--bg);color:var(--text);min-height:100vh;overflow-x:hidden}
button,input,select{font:inherit}
button{cursor:pointer}
.app{display:flex;min-height:100vh}
.sidebar{width:280px;background:var(--card2);border-right:1px solid var(--border);padding:20px;display:flex;flex-direction:column;gap:20px;position:sticky;top:0;height:100vh;z-index:100}
.main{flex:1;padding:24px;max-width:1100px;margin:auto;width:100%}
.header,.card,.month{background:var(--card);border:1px solid var(--border);border-radius:12px}
.header{padding:16px 20px;display:flex;justify-content:space-between;align-items:center;gap:12px;margin-bottom:20px}
.brand{font-size:1.3rem;font-weight:800}.muted{color:var(--muted);font-size:.85rem}
.btn{border:0;border-radius:8px;padding:9px 14px;font-weight:700;background:var(--accent);color:#fff}
.btn:hover{background:var(--accent2)}
.btn.secondary{background:transparent;color:var(--text);border:1px solid var(--border)}
.btn.danger{background:var(--danger)}
.btn.gold{background:var(--gold)}
.btn:disabled{opacity:.5;cursor:not-allowed}
.clock,.sync{padding:7px 10px;border:1px solid var(--border);border-radius:8px;font-size:.82rem}
.sync.online{color:var(--success)}.sync.offline{color:var(--gold)}
.progress-grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(280px,1fr));gap:15px;margin-bottom:18px}
.card{padding:16px}.progress-info{display:flex;justify-content:space-between;font-weight:700;font-size:.9rem;margin-bottom:8px}
.track{height:11px;background:var(--bg);border:1px solid var(--border);border-radius:8px;overflow:hidden}
.fill{height:100%;width:0;background:linear-gradient(90deg,var(--accent),var(--success));transition:width .3s}
.trimesters{display:flex;gap:8px;overflow:auto;padding-bottom:8px;margin-bottom:18px}
.trim{white-space:nowrap;background:var(--card);color:var(--text);border:1px solid var(--border);border-radius:8px;padding:8px 13px;font-weight:700}
.trim.active{background:var(--accent);border-color:var(--accent);color:#fff}
.section-head{display:flex;justify-content:space-between;align-items:center;gap:10px;margin-bottom:15px}
.month{padding:15px;margin-bottom:18px}
.month-head{display:flex;justify-content:space-between;align-items:center;gap:15px;border-bottom:1px solid var(--border);padding-bottom:10px;margin-bottom:12px}
.month-title{color:var(--accent);font-weight:800}
.month-progress{width:220px;max-width:45%}
.weeks{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:12px}
.week{background:var(--bg);border:1px solid var(--border);border-radius:9px;padding:12px}
.week-head{display:flex;justify-content:space-between;margin-bottom:9px}
.subject{margin-bottom:9px}.subject-name{font-size:.82rem;font-weight:700;margin-bottom:5px}
.task{display:flex;gap:7px;align-items:center}
.task input[type=text]{flex:1;min-width:0;background:var(--card);border:1px solid var(--border);color:var(--text);padding:6px 8px;border-radius:6px;font-size:.8rem}
.task input[type=checkbox]{width:17px;height:17px;accent-color:var(--success)}
label{display:block;color:var(--muted);font-size:.82rem;margin-bottom:5px}
.input,.select{width:100%;padding:9px 10px;background:var(--bg);color:var(--text);border:1px solid var(--border);border-radius:7px}
.side-title{font-weight:800;font-size:1.05rem}.side-section{display:flex;flex-direction:column;gap:6px}
.spacer{margin-top:auto}
.modal{position:fixed;inset:0;background:var(--overlay);display:none;align-items:center;justify-content:center;z-index:1000;padding:20px}
.modal.show{display:flex}.modal-box{background:var(--card);border:1px solid var(--border);border-radius:15px;padding:24px;width:min(500px,100%);max-height:90vh;overflow:auto}
.modal-box h2{margin-bottom:8px}.modal-box p{color:var(--muted);font-size:.88rem;margin-bottom:18px}
.actions{display:flex;gap:10px;margin-top:18px}.actions>*{flex:1}
.subject-edit{margin-bottom:10px}
.mobile-menu{display:none;background:transparent;color:var(--text);border:0;font-size:1.4rem}
.empty{padding:30px;text-align:center;color:var(--muted)}
.badge{display:inline-block;padding:3px 7px;border-radius:5px;font-size:.72rem;font-weight:800;background:var(--gold);color:#fff}
.sealed{color:#fff;background:var(--danger);padding:5px 8px;border-radius:5px;font-size:.75rem;font-weight:800}
.toast{position:fixed;right:20px;bottom:20px;background:var(--card);border:1px solid var(--border);padding:12px 15px;border-radius:9px;display:none;z-index:2000}
@media(max-width:768px){
  .sidebar{position:fixed;left:-280px;transition:left .2s}
  .sidebar.open{left:0}.mobile-menu{display:block}.main{padding:14px}
  .header{align-items:flex-start}.header-right{flex-wrap:wrap;justify-content:flex-end}
  .month-progress{max-width:100%;width:180px}.month-head{align-items:flex-start}
}
</style>
</head>

<body>

<div class="app">
<aside class="sidebar" id="sidebar">
  <div class="side-title">Universidad Bicentenaria de Aragua</div>

  <div class="side-section">
    <label>Estudiante</label>
    <strong id="sideUser">Invitado</strong>
  </div>

  <div class="side-section">
    <label>Apariencia</label>
    <button class="btn secondary" id="themeBtn">🌓 Cambiar tema</button>
  </div>

  <div class="side-section">
    <label>Idioma</label>
    <select class="select" id="langSelect">
      <option value="es">Español</option>
      <option value="en">English</option>
      <option value="pt">Português</option>
    </select>
  </div>

  <div class="side-section">
    <label>Materias</label>
    <button class="btn secondary" id="subjectsBtn">✏️ Editar materias</button>
  </div>

  <div class="side-section">
    <label>Cuenta</label>
    <button class="btn secondary" id="accountBtn">👤 Cuenta</button>
  </div>

  <div class="spacer">
    <button class="btn danger" style="width:100%" id="resetBtn">Reiniciar trimestre</button>
  </div>
</aside>

<main class="main">
<header class="header">
  <div style="display:flex;align-items:center;gap:10px">
    <button class="mobile-menu" id="menuBtn">☰</button>
    <div>
      <div class="brand">MiTracke <span id="premiumBadge"></span></div>
      <div class="muted">Seguidor académico trimestral</div>
    </div>
  </div>
  <div class="header-right" style="display:flex;align-items:center;gap:8px">
    <div class="sync offline" id="syncStatus">● Local</div>
    <div class="clock" id="clock">--:-- --</div>
  </div>
</header>

<section class="progress-grid">
  <div class="card">
    <div class="progress-info"><span>Progreso del trimestre</span><span id="trimPct">0%</span></div>
    <div class="track"><div class="fill" id="trimFill"></div></div>
  </div>
  <div class="card">
    <div class="progress-info"><span>Progreso general</span><span id="globalPct">0%</span></div>
    <div class="track"><div class="fill" id="globalFill"></div></div>
  </div>
</section>

<nav class="trimesters" id="trimesters"></nav>

<div class="section-head">
  <h2 id="currentTitle">Trimestre 1</h2>
  <div>
    <span class="sealed" id="sealedBadge" style="display:none">🔒 SELLADO</span>
    <button class="btn secondary" id="sealBtn">✍️ Sellar trimestre</button>
  </div>
</div>

<div id="months"></div>
</main>
</div>

<!-- Modal bienvenida/cuenta -->
<div class="modal" id="welcomeModal">
  <div class="modal-box">
    <h2>Bienvenido a MiTracke</h2>
    <p>Esta beta usa almacenamiento local. La autenticación con servidor queda preparada para la siguiente fase.</p>
    <label>Nombre del estudiante</label>
    <input class="input" id="welcomeName" placeholder="Nombre completo">
    <div class="actions">
      <button class="btn secondary" id="welcomeCancel">Cerrar</button>
      <button class="btn" id="welcomeContinue">Continuar</button>
    </div>
  </div>
</div>

<!-- Editor de materias -->
<div class="modal" id="subjectsModal">
  <div class="modal-box">
    <h2>Editar materias</h2>
    <p>La edición queda guardada localmente. En producción estos datos pertenecerán a la cuenta del estudiante.</p>
    <div id="subjectsEditor"></div>
    <div class="actions">
      <button class="btn secondary" id="subjectsCancel">Cancelar</button>
      <button class="btn" id="subjectsSave">Guardar</button>
    </div>
  </div>
</div>

<!-- Cuenta futura -->
<div class="modal" id="accountModal">
  <div class="modal-box">
    <h2>Cuenta</h2>
    <p id="accountInfo">Modo local. Aquí se integrará el sistema real de autenticación.</p>
    <label>Correo electrónico — preparado para backend</label>
    <input class="input" id="accountEmail" type="email" placeholder="correo@ejemplo.com" disabled>
    <div class="actions">
      <button class="btn secondary" id="accountClose">Cerrar</button>
    </div>
  </div>
</div>

<div class="toast" id="toast"></div>

<script>
/*
============================================================
MiTracke — arquitectura preparada para versión final
============================================================

YA IMPLEMENTADO:
- UI responsive
- 11 trimestres
- meses/semanas/materias
- actividades
- progreso
- tema
- idiomas básicos
- persistencia local
- modo offline local

RESERVADO PARA SIGUIENTE FASE:
- autenticación real
- API/backend
- base de datos remota
- sincronización multi-dispositivo
- cola de cambios offline
- IndexedDB
- notificaciones
- empaquetado Windows/Android

IMPORTANTE:
Nunca colocar contraseñas reales, claves maestras o secretos
del servidor dentro de este archivo.
*/

// ==========================================================
// CONFIGURACIÓN
// ==========================================================

const CONFIG = {
  APP_VERSION: "1.0.0-beta",
  STORAGE_KEY: "mitracke_local_v1",

  // FUTURO:
  // Cambiar por la URL real del backend.
  API_BASE_URL: "",

  // FUTURO:
  // true cuando exista backend.
  REMOTE_SYNC_ENABLED: false
};

const DEFAULT_SUBJECTS = [
  "Ética y Cultura Universitaria",
  "Educación Física para la Salud y Deporte",
  "Geometría Analítica",
  "Identidad y Expresión Cultural",
  "Matemática I",
  "Lógica Matemática"
];

const DEFAULT_STATE = {
  schemaVersion: 1,
  user: {
    id: null,
    name: "",
    email: "",
    authenticated: false
  },
  currentTrimestre: 1,
  theme: "dark",
  lang: "es",
  subjects: [...DEFAULT_SUBJECTS],
  trimestresData: {},
  sealedTrimestres: {},

  // FUTURO OFFLINE SYNC
  sync: {
    lastSyncAt: null,
    pendingChanges: []
  }
};

let state = structuredClone(DEFAULT_STATE);

// ==========================================================
// STORAGE LOCAL
// ==========================================================

function loadState(){
  const raw = localStorage.getItem(CONFIG.STORAGE_KEY);

  if(!raw){
    state = structuredClone(DEFAULT_STATE);
    return;
  }

  try{
    state = {
      ...structuredClone(DEFAULT_STATE),
      ...JSON.parse(raw)
    };

    state.user = {
      ...structuredClone(DEFAULT_STATE.user),
      ...(state.user || {})
    };

    state.sync = {
      ...structuredClone(DEFAULT_STATE.sync),
      ...(state.sync || {})
    };

  }catch(error){
    console.error("No se pudo cargar el estado:", error);
    state = structuredClone(DEFAULT_STATE);
  }
}

function saveState(){
  localStorage.setItem(CONFIG.STORAGE_KEY, JSON.stringify(state));

  /*
    FUTURO:
    Después de guardar localmente:
      queueChange(...)
      syncPendingChanges()

    Esto permitirá trabajar sin Internet.
  */
}

// ==========================================================
// FUTURO: CAPA DE AUTENTICACIÓN
// ==========================================================

const AuthService = {

  async login(email, password){
    /*
      FUTURO:

      const response = await fetch(`${CONFIG.API_BASE_URL}/auth/login`, {
        method: "POST",
        headers: {"Content-Type":"application/json"},
        body: JSON.stringify({email, password})
      });

      // El servidor devolvería una sesión/token.
    */

    throw new Error("Autenticación remota todavía no configurada.");
  },

  async register(name, email, password){
    /*
      FUTURO:
      Registro de usuario en el backend.
    */

    throw new Error("Registro remoto todavía no configurado.");
  },

  async logout(){
    /*
      FUTURO:
      invalidar sesión/token.
    */
    state.user.authenticated = false;
    saveState();
  },

  async recoverPassword(email){
    /*
      FUTURO:
      solicitud de recuperación de contraseña.
    */
    throw new Error("Recuperación remota todavía no configurada.");
  }
};

// ==========================================================
// FUTURO: API
// ==========================================================

const ApiService = {

  async request(path, options = {}){
    if(!CONFIG.API_BASE_URL){
      throw new Error("API no configurada.");
    }

    const response = await fetch(
      `${CONFIG.API_BASE_URL}${path}`,
      {
        ...options,
        headers: {
          "Content-Type":"application/json",
          ...(options.headers || {})
        }
      }
    );

    if(!response.ok){
      throw new Error(`API ${response.status}`);
    }

    return response.json();
  },

  async getUserData(){
    return this.request("/me/data");
  },

  async saveUserData(data){
    return this.request("/me/data", {
      method: "PUT",
      body: JSON.stringify(data)
    });
  }
};

// ==========================================================
// FUTURO: COLA OFFLINE
// ==========================================================

function queueChange(type, payload){

  /*
    FUTURO:

    Cada modificación local se guarda como:

    {
      id: crypto.randomUUID(),
      type: "TASK_UPDATE",
      payload: {...},
      createdAt: Date.now()
    }

    Cuando vuelve Internet:
      syncPendingChanges()
  */

  state.sync.pendingChanges.push({
    id: crypto.randomUUID(),
    type,
    payload,
    createdAt: Date.now()
  });

  saveState();
}

async function syncPendingChanges(){

  if(!CONFIG.REMOTE_SYNC_ENABLED) return;
  if(!navigator.onLine) return;
  if(!state.user.authenticated) return;

  /*
    FUTURO:

    1. tomar pendingChanges
    2. enviarlos al servidor
    3. servidor confirma
    4. eliminar cambios confirmados
    5. resolver conflictos
    6. actualizar lastSyncAt
  */
}

// ==========================================================
// FUTURO: INDEXEDDB
// ==========================================================

const LocalDatabase = {

  async init(){
    /*
      FUTURO:

      IndexedDB será utilizado para almacenar:
      - actividades
      - materias
      - trimestres
      - cambios pendientes
      - configuración

      localStorage queda para preferencias pequeñas.
    */
  },

  async save(){
    /*
      FUTURO: persistencia estructurada en IndexedDB.
    */
  }
};

// ==========================================================
// CONECTIVIDAD
// ==========================================================

function updateConnectionStatus(){

  const element = document.getElementById("syncStatus");

  if(navigator.onLine){

    if(CONFIG.REMOTE_SYNC_ENABLED){
      element.textContent = "● Conectado";
      element.className = "sync online";
      syncPendingChanges();
    }else{
      element.textContent = "● Local / Internet";
      element.className = "sync online";
    }

  }else{
    element.textContent = "● Sin conexión — guardado local";
    element.className = "sync offline";
  }
}

window.addEventListener("online", updateConnectionStatus);
window.addEventListener("offline", updateConnectionStatus);

// ==========================================================
// UTILIDADES
// ==========================================================

function escapeHtml(value){
  return String(value ?? "")
    .replace(/&/g,"&amp;")
    .replace(/</g,"&lt;")
    .replace(/>/g,"&gt;")
    .replace(/"/g,"&quot;")
    .replace(/'/g,"&#039;");
}

function showToast(message){
  const toast = document.getElementById("toast");
  toast.textContent = message;
  toast.style.display = "block";

  clearTimeout(showToast.timer);
  showToast.timer = setTimeout(()=>{
    toast.style.display = "none";
  },2500);
}

// ==========================================================
// USUARIO LOCAL — TEMPORAL
// ==========================================================

function checkLocalUser(){

  if(state.user.name.trim()){
    document.getElementById("sideUser").textContent = state.user.name;
    return true;
  }

  document.getElementById("welcomeModal").classList.add("show");
  return false;
}

function continueWelcome(){

  const name = document.getElementById("welcomeName").value.trim();

  if(!name){
    showToast("Debes introducir tu nombre.");
    return;
  }

  state.user.name = name;
  saveState();

  document.getElementById("welcomeModal").classList.remove("show");
  document.getElementById("sideUser").textContent = name;

  renderAll();
}

function cancelWelcome(){
  document.getElementById("welcomeModal").classList.remove("show");
}

// ==========================================================
// TRIMESTRES
// ==========================================================

function selectTrimestre(number){

  if(number === 11){
    const ok = confirm("¿Deseas entrar al Trimestre 11? Es el último ciclo de la carrera.");
    if(!ok) return;
  }

  state.currentTrimestre = number;
  saveState();
  renderAll();
}

function renderTrimesters(){

  const container = document.getElementById("trimesters");
  container.innerHTML = "";

  for(let i=1;i<=11;i++){

    const button = document.createElement("button");

    button.className =
      "trim" + (state.currentTrimestre === i ? " active" : "");

    button.textContent = `Trimestre ${i}`;

    button.onclick = ()=>selectTrimestre(i);

    container.appendChild(button);
  }
}

// ==========================================================
// ACTIVIDADES
// ==========================================================

function taskKey(trim,month,week,subject){
  return `t${trim}_m${month}_w${week}_s${subject}`;
}

function getTask(trim,month,week,subject){

  const key = taskKey(trim,month,week,subject);

  return state.trimestresData[key] || {
    checked:false,
    text:""
  };
}

function saveTask(trim,month,week,subject,data){

  const key = taskKey(trim,month,week,subject);

  state.trimestresData[key] = {
    ...getTask(trim,month,week,subject),
    ...data,
    updatedAt: Date.now()
  };

  saveState();

  queueChange("TASK_UPDATE",{
    key,
    data:state.trimestresData[key]
  });

  updateProgress();
}

function toggleTask(trim,month,week,subject){

  const task = getTask(trim,month,week,subject);

  saveTask(trim,month,week,subject,{
    checked:!task.checked
  });
}

function saveTaskText(trim,month,week,subject,text){

  saveTask(trim,month,week,subject,{
    text
  });
}

// ==========================================================
// RENDER MESES
// ==========================================================

function renderMonths(){

  const container = document.getElementById("months");
  container.innerHTML = "";

  const trim = state.currentTrimestre;
  const sealed = Boolean(state.sealedTrimestres[trim]);

  for(let month=1;month<=3;month++){

    const monthElement = document.createElement("section");
    monthElement.className = "month";

    monthElement.innerHTML = `
      <div class="month-head">
        <div class="month-title">Mes ${month}</div>
        <div class="month-progress">
          <div class="progress-info">
            <span>Progreso</span>
            <span id="monthPct-${month}">0%</span>
          </div>
          <div class="track">
            <div class="fill" id="monthFill-${month}"></div>
          </div>
        </div>
      </div>
      <div class="weeks" id="weeks-${month}"></div>
    `;

    container.appendChild(monthElement);

    const weeks = monthElement.querySelector(`#weeks-${month}`);

    for(let week=1;week<=4;week++){

      const weekElement = document.createElement("div");
      weekElement.className = "week";

      weekElement.innerHTML = `
        <div class="week-head">
          <strong>Semana ${week}</strong>
          <span class="muted" id="weekPct-${month}-${week}">0%</span>
        </div>
      `;

      state.subjects.forEach((subject,index)=>{

        const task = getTask(trim,month,week,index);

        const row = document.createElement("div");
        row.className = "subject";

        row.innerHTML = `
          <div class="subject-name">${escapeHtml(subject)}</div>
          <div class="task">
            <input
              type="checkbox"
              ${task.checked ? "checked":""}
              ${sealed ? "disabled":""}
            >
            <input
              type="text"
              value="${escapeHtml(task.text)}"
              placeholder="Nombre de la actividad..."
              ${sealed ? "disabled":""}
            >
          </div>
        `;

        const checkbox = row.querySelector('input[type="checkbox"]');
        const text = row.querySelector('input[type="text"]');

        checkbox.addEventListener("change",()=>{
          toggleTask(trim,month,week,index);
        });

        text.addEventListener("change",()=>{
          saveTaskText(trim,month,week,index,text.value);
        });

        weekElement.appendChild(row);
      });

      weeks.appendChild(weekElement);
    }
  }

  document.getElementById("currentTitle").textContent =
    `Trimestre ${trim}`;

  document.getElementById("sealedBadge").style.display =
    sealed ? "inline-block":"none";

  document.getElementById("sealBtn").style.display =
    sealed ? "none":"inline-block";

  updateProgress();
}

// ==========================================================
// PROGRESO
// ==========================================================

function calculateProgressForTrim(trim){

  let total = 0;
  let checked = 0;

  for(let month=1;month<=3;month++){
    for(let week=1;week<=4;week++){

      state.subjects.forEach((_,subject)=>{

        total++;

        if(getTask(trim,month,week,subject).checked){
          checked++;
        }
      });
    }
  }

  return total ? Math.round(checked/total*100):0;
}

function updateProgress(){

  const trim = state.currentTrimestre;

  const trimProgress = calculateProgressForTrim(trim);

  document.getElementById("trimPct").textContent =
    `${trimProgress}%`;

  document.getElementById("trimFill").style.width =
    `${trimProgress}%`;

  let globalTotal = 0;
  let globalChecked = 0;

  for(let t=1;t<=11;t++){

    for(let month=1;month<=3;month++){

      for(let week=1;week<=4;week++){

        state.subjects.forEach((_,subject)=>{

          globalTotal++;

          if(getTask(t,month,week,subject).checked){
            globalChecked++;
          }
        });
      }
    }
  }

  const globalProgress =
    globalTotal ? Math.round(globalChecked/globalTotal*100):0;

  document.getElementById("globalPct").textContent =
    `${globalProgress}%`;

  document.getElementById("globalFill").style.width =
    `${globalProgress}%`;

  for(let month=1;month<=3;month++){

    let total=0,checked=0;

    for(let week=1;week<=4;week++){

      state.subjects.forEach((_,subject)=>{

        total++;

        if(getTask(trim,month,week,subject).checked){
          checked++;
        }
      });

      const weekPct =
        state.subjects.length
        ? Math.round(
            state.subjects.filter((_,subject)=>
              getTask(trim,month,week,subject).checked
            ).length /
            state.subjects.length * 100
          )
        : 0;

      const weekElement =
        document.getElementById(`weekPct-${month}-${week}`);

      if(weekElement)
        weekElement.textContent = `${weekPct}%`;
    }

    const monthPct =
      total ? Math.round(checked/total*100):0;

    const monthText =
      document.getElementById(`monthPct-${month}`);

    const monthFill =
      document.getElementById(`monthFill-${month}`);

    if(monthText) monthText.textContent = `${monthPct}%`;
    if(monthFill) monthFill.style.width = `${monthPct}%`;
  }
}

// ==========================================================
// SELLADO
// ==========================================================

function sealCurrentTrimester(){

  const trim = state.currentTrimestre;

  if(state.sealedTrimestres[trim]) return;

  const ok = confirm(
    "¿Seguro que quieres sellar este trimestre? " +
    "Las actividades quedarán bloqueadas."
  );

  if(!ok) return;

  state.sealedTrimestres[trim] = true;

  saveState();

  queueChange("TRIMESTER_SEALED",{
    trimester:trim
  });

  renderMonths();
}

// ==========================================================
// REINICIAR TRIMESTRE
// ==========================================================

function resetCurrentTrimester(){

  const trim = state.currentTrimestre;

  const ok = confirm(
    "Esto eliminará las actividades y marcas del trimestre actual. ¿Continuar?"
  );

  if(!ok) return;

  for(let month=1;month<=3;month++){
    for(let week=1;week<=4;week++){
      state.subjects.forEach((_,subject)=>{
        delete state.trimestresData[
          taskKey(trim,month,week,subject)
        ];
      });
    }
  }

  delete state.sealedTrimestres[trim];

  saveState();

  queueChange("TRIMESTER_RESET",{trimester:trim});

  renderAll();
}

// ==========================================================
// MATERIAS
// ==========================================================

function openSubjects(){

  const container =
    document.getElementById("subjectsEditor");

  container.innerHTML = "";

  state.subjects.forEach((subject,index)=>{

    const wrapper =
      document.createElement("div");

    wrapper.className = "subject-edit";

    wrapper.innerHTML = `
      <label>Materia ${index+1}</label>
      <input
        class="input"
        data-subject-index="${index}"
        value="${escapeHtml(subject)}"
      >
    `;

    container.appendChild(wrapper);
  });

  document.getElementById("subjectsModal").classList.add("show");
}

function saveSubjects(){

  const inputs =
    document.querySelectorAll(
      "#subjectsEditor input[data-subject-index]"
    );

  inputs.forEach(input=>{

    const index =
      Number(input.dataset.subjectIndex);

    const value =
      input.value.trim();

    if(value)
      state.subjects[index] = value;
  });

  saveState();

  queueChange("SUBJECTS_UPDATE",{
    subjects:state.subjects
  });

  document.getElementById("subjectsModal")
    .classList.remove("show");

  renderAll();
}

// ==========================================================
// TEMA
// ==========================================================

function toggleTheme(){

  state.theme =
    state.theme === "dark"
      ? "light"
      : "dark";

  document.documentElement
    .setAttribute("data-theme",state.theme);

  saveState();
}

// ==========================================================
// RELOJ LOCAL
// ==========================================================

function updateClock(){

  const now = new Date();

  let hours = now.getHours();

  const minutes =
    String(now.getMinutes()).padStart(2,"0");

  const ampm =
    hours >= 12 ? "PM":"AM";

  hours = hours % 12 || 12;

  document.getElementById("clock").textContent =
    `${String(hours).padStart(2,"0")}:${minutes} ${ampm}`;
}

setInterval(updateClock,1000);

// ==========================================================
// UI
// ==========================================================

function renderAll(){

  document.documentElement
    .setAttribute("data-theme",state.theme);

  document.getElementById("langSelect").value =
    state.lang;

  document.getElementById("sideUser").textContent =
    state.user.name || "Invitado";

  renderTrimesters();
  renderMonths();
  updateConnectionStatus();
}

document.getElementById("themeBtn")
  .addEventListener("click",toggleTheme);

document.getElementById("langSelect")
  .addEventListener("change",e=>{
    state.lang=e.target.value;
    saveState();
  });

document.getElementById("subjectsBtn")
  .addEventListener("click",openSubjects);

document.getElementById("subjectsSave")
  .addEventListener("click",saveSubjects);

document.getElementById("subjectsCancel")
  .addEventListener("click",()=>{
    document.getElementById("subjectsModal")
      .classList.remove("show");
  });

document.getElementById("accountBtn")
  .addEventListener("click",()=>{
    document.getElementById("accountModal")
      .classList.add("show");
  });

document.getElementById("accountClose")
  .addEventListener("click",()=>{
    document.getElementById("accountModal")
      .classList.remove("show");
  });

document.getElementById("welcomeContinue")
  .addEventListener("click",continueWelcome);

document.getElementById("welcomeCancel")
  .addEventListener("click",cancelWelcome);

document.getElementById("sealBtn")
  .addEventListener("click",sealCurrentTrimester);

document.getElementById("resetBtn")
  .addEventListener("click",resetCurrentTrimester);

document.getElementById("menuBtn")
  .addEventListener("click",()=>{
    document.getElementById("sidebar")
      .classList.toggle("open");
  });

// ==========================================================
// INICIALIZACIÓN
// ==========================================================

async function init(){

  loadState();

  await LocalDatabase.init();

  updateClock();

  renderAll();

  checkLocalUser();
}

init();
</script>
</body>
</html>
'''
