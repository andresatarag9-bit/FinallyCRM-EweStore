<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta name="description" content="CRM MVP para EWE Store Tunja con Inteligencia Artificial simulada y gestión de Leads">
    <title>EWE Store Tunja - CRM Inteligente</title>
    <!-- Librería de Iconos (Phosphor Icons) -->
    <script src="https://unpkg.com/@phosphor-icons/web"></script>
    
    <style>
        /* --- VARIABLES Y RESET --- */
        :root {
            --primary: #00b894;       /* Verde principal */
            --primary-dark: #008f72;  /* Verde oscuro */
            --accent: #0984e3;        /* Azul acento */
            --bg: #f5f7fa;            /* Fondo general */
            --surface: #ffffff;       /* Fondo de tarjetas */
            --text: #2d3436;          /* Texto principal */
            --text-light: #636e72;    /* Texto secundario */
            --border: #dfe6e9;        /* Bordes */
            --danger: #d63031;        /* Rojo error */
            --warning: #fdcb6e;       /* Amarillo alerta */
            --success: #00b894;       /* Verde éxito */
            --shadow: 0 4px 6px rgba(0,0,0,0.05);
            --radius: 12px;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', system-ui, -apple-system, sans-serif; }

        body { background-color: var(--bg); color: var(--text); display: flex; height: 100vh; overflow: hidden; }

        /* --- SIDEBAR --- */
        aside {
            width: 260px;
            background: var(--surface);
            border-right: 1px solid var(--border);
            display: flex;
            flex-direction: column;
            padding: 1.5rem;
            z-index: 10;
        }

        .brand {
            font-size: 1.4rem;
            font-weight: 800;
            color: var(--primary);
            margin-bottom: 2rem;
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        nav button {
            background: none;
            border: none;
            width: 100%;
            padding: 0.8rem 1rem;
            text-align: left;
            cursor: pointer;
            color: var(--text-light);
            border-radius: var(--radius);
            font-weight: 600;
            display: flex;
            align-items: center;
            gap: 0.8rem;
            transition: all 0.2s;
            margin-bottom: 0.5rem;
        }

        nav button:hover, nav button.active { background-color: #e6fffa; color: var(--primary-dark); }
        nav button i { font-size: 1.2rem; }

        /* --- MAIN CONTENT --- */
        main { flex: 1; padding: 2rem; overflow-y: auto; position: relative; }

        header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 2rem; }
        
        h1 { font-size: 1.8rem; }

        .btn {
            padding: 0.6rem 1.2rem;
            border-radius: var(--radius);
            border: none;
            cursor: pointer;
            font-weight: 600;
            display: inline-flex;
            align-items: center;
            gap: 0.5rem;
            transition: 0.2s;
        }

        .btn-primary { background: var(--primary); color: white; }
        .btn-primary:hover { background: var(--primary-dark); }
        .btn-outline { background: transparent; border: 1px solid var(--border); color: var(--text); }
        .btn-outline:hover { border-color: var(--primary); color: var(--primary); }

        /* --- DASHBOARD STATS --- */
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 1.5rem;
            margin-bottom: 2rem;
        }

        .stat-card {
            background: var(--surface);
            padding: 1.5rem;
            border-radius: var(--radius);
            box-shadow: var(--shadow);
            border: 1px solid var(--border);
        }

        .stat-card h3 { font-size: 0.9rem; color: var(--text-light); margin-bottom: 0.5rem; }
        .stat-card .value { font-size: 1.8rem; font-weight: 700; color: var(--text); }
        .stat-card .trend { font-size: 0.8rem; margin-top: 0.5rem; display: block; }
        .trend.up { color: var(--success); }

        /* --- KANBAN BOARD --- */
        .kanban-board {
            display: flex;
            gap: 1rem;
            overflow-x: auto;
            height: calc(100vh - 250px);
            padding-bottom: 1rem;
        }

        .kanban-column {
            min-width: 300px;
            background: #eef2f5;
            border-radius: var(--radius);
            padding: 1rem;
            display: flex;
            flex-direction: column;
        }

        .column-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 1rem;
            padding-bottom: 0.5rem;
            border-bottom: 2px solid rgba(0,0,0,0.05);
        }

        .column-header h4 { font-size: 0.95rem; text-transform: uppercase; letter-spacing: 0.5px; }
        .badge { background: #dfe6e9; padding: 2px 8px; border-radius: 10px; font-size: 0.8rem; }

        .lead-card {
            background: var(--surface);
            padding: 1rem;
            border-radius: 8px;
            margin-bottom: 0.8rem;
            box-shadow: 0 2px 4px rgba(0,0,0,0.03);
            cursor: grab;
            border: 1px solid transparent;
            transition: all 0.2s;
            position: relative;
        }

        .lead-card:hover { transform: translateY(-2px); box-shadow: var(--shadow); border-color: var(--primary); }
        .lead-card:active { cursor: grabbing; }
        
        .lead-card.dragging { opacity: 0.5; border: 2px dashed var(--primary); }

        .card-top { display: flex; justify-content: space-between; margin-bottom: 0.5rem; }
        .lead-name { font-weight: 600; font-size: 0.95rem; }
        .lead-budget { font-size: 0.85rem; color: var(--text-light); background: #f1f2f6; padding: 2px 6px; border-radius: 4px; }
        .lead-model { font-size: 0.85rem; color: var(--primary); margin-bottom: 0.5rem; display: block; }
        
        .contact-row {
            display: flex;
            align-items: center;
            gap: 0.8rem;
            font-size: 0.75rem;
            color: #636e72;
            margin-bottom: 0.5rem;
        }

        .contact-row span { display: flex; align-items: center; gap: 4px; }

        .ai-score {
            display: flex;
            align-items: center;
            gap: 4px;
            font-size: 0.75rem;
            margin-top: 8px;
            padding-top: 8px;
            border-top: 1px solid #f1f2f6;
        }

        .score-dot { width: 8px; height: 8px; border-radius: 50%; }
        .score-high { background: var(--success); }
        .score-med { background: var(--warning); }
        .score-low { background: var(--danger); }

        /* --- AI PANEL --- */
        .ai-panel {
            background: linear-gradient(135deg, #2d3436 0%, #000000 100%);
            color: white;
            padding: 1.5rem;
            border-radius: var(--radius);
            margin-bottom: 2rem;
            display: flex;
            align-items: center;
            justify-content: space-between;
            box-shadow: 0 10px 20px rgba(0,0,0,0.2);
        }

        .ai-content h2 { font-size: 1.2rem; margin-bottom: 0.5rem; display: flex; align-items: center; gap: 0.5rem; }
        .ai-content p { color: #b2bec3; font-size: 0.9rem; }

        /* --- IMPORT SECTION --- */
        .import-zone {
            border: 2px dashed var(--primary);
            border-radius: var(--radius);
            padding: 4rem;
            text-align: center;
            background: #e6fffa;
            margin-top: 2rem;
            transition: 0.2s;
        }
        .import-zone:hover { background: #d1f2ea; }
        .import-zone i { font-size: 3rem; color: var(--primary); margin-bottom: 1rem; }

        /* --- TOAST NOTIFICATIONS --- */
        .toast-container {
            position: fixed;
            bottom: 20px;
            right: 20px;
            display: flex;
            flex-direction: column;
            gap: 10px;
            z-index: 100;
        }

        .toast {
            background: var(--surface);
            border-left: 4px solid var(--primary);
            padding: 1rem;
            border-radius: 4px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.15);
            display: flex;
            align-items: center;
            gap: 10px;
            animation: slideIn 0.3s ease;
            min-width: 250px;
        }

        @keyframes slideIn { from { transform: translateX(100%); opacity: 0; } to { transform: translateX(0); opacity: 1; } }

        /* --- MODAL --- */
        .modal-overlay {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: rgba(0,0,0,0.5); display: none; justify-content: center; align-items: center; z-index: 50;
        }
        .modal { background: var(--surface); padding: 2rem; border-radius: var(--radius); width: 450px; max-width: 90%; max-height: 90vh; overflow-y: auto;}
        .form-group { margin-bottom: 1rem; }
        .form-group label { display: block; margin-bottom: 0.5rem; font-size: 0.9rem; font-weight: 600; }
        .form-group input, .form-group select { width: 100%; padding: 0.6rem; border: 1px solid var(--border); border-radius: 6px; }
        
        .hidden { display: none !important; }
    </style>
</head>
<body>

    <!-- Sidebar de Navegación -->
    <aside>
        <div class="brand">
            <i class="ph ph-storefront"></i> EWE Store Tunja
        </div>
        <nav>
            <button class="active" onclick="showView('dashboard')">
                <i class="ph ph-squares-four"></i> Tablero
            </button>
            <button onclick="showView('leads')">
                <i class="ph ph-users-three"></i> Leads
            </button>
            <button onclick="showView('import')">
                <i class="ph ph-upload-simple"></i> Importar Datos
            </button>
            <button onclick="showView('automation')">
                <i class="ph ph-robot"></i> Automatización
            </button>
        </nav>
    </aside>

    <!-- Contenido Principal -->
    <main id="main-content">
        <!-- Vista: Dashboard -->
        <section id="view-dashboard">
            <header>
                <div>
                    <h1>Panel de Control</h1>
                    <p style="color: var(--text-light)">Gestión de ventas EWE Store Tunja</p>
                </div>
                <button class="btn btn-primary" onclick="openAddLeadModal()">
                    <i class="ph ph-plus"></i> Nuevo Lead
                </button>
            </header>

            <!-- Banner IA -->
            <div class="ai-panel">
                <div class="ai-content">
                    <h2><i class="ph ph-sparkle"></i> AI Assistant Activo</h2>
                    <p id="ai-recommendation">Analizando oportunidades... Se detectaron 3 leads con alta probabilidad de cierre hoy.</p>
                </div>
                <button class="btn btn-outline" style="color: white; border-color: rgba(255,255,255,0.3);" onclick="refreshAI()">Actualizar Análisis</button>
            </div>

            <!-- Estadísticas -->
            <div class="stats-grid">
                <div class="stat-card">
                    <h3>Total Leads</h3>
                    <div class="value" id="stat-total">0</div>
                    <span class="trend up"><i class="ph ph-trend-up"></i> +12% este mes</span>
                </div>
                <div class="stat-card">
                    <h3>Pipeline (COP)</h3>
                    <div class="value" id="stat-pipeline">$0</div>
                    <span class="trend">Valor potencial total</span>
                </div>
                <div class="stat-card">
                    <h3>Tasa de Conversión</h3>
                    <div class="value">18%</div>
                    <span class="trend up">+2% vs mes anterior</span>
                </div>
                <div class="stat-card">
                    <h3>Automatizaciones</h3>
                    <div class="value">3 Activas</div>
                    <span class="trend">Ahorro de 4h/semana</span>
                </div>
            </div>

            <h2>Oportunidades Destacadas</h2>
            <div class="kanban-board" id="dashboard-kanban">
                <!-- Contenido del Kanban generado por JS -->
            </div>
        </section>

        <!-- Vista: Importar -->
        <section id="view-import" class="hidden">
            <header>
                <h1>Importar Leads</h1>
            </header>
            <p style="margin-bottom: 1rem;">Alimenta tu CRM con datos de Excel, CSV o PDF.</p>
            
            <div class="import-zone" id="drop-zone">
                <i class="ph ph-file-arrow-up"></i>
                <h3>Arrastra tu archivo aquí</h3>
                <p style="color: var(--text-light); margin: 0.5rem 0;">Soporta .CSV (Funcional), .XLSX y .PDF (Simulado)</p>
                <input type="file" id="file-input" accept=".csv, .xlsx, .pdf" style="display:none">
                <button class="btn btn-primary" onclick="document.getElementById('file-input').click()">Seleccionar Archivo</button>
            </div>
            
            <div style="margin-top: 2rem;">
                <h4>Estado del proceso:</h4>
                <ul id="import-log" style="margin-top: 1rem; padding-left: 1rem; font-family: monospace; color: var(--text-light);">
                    <li>Esperando archivo...</li>
                </ul>
            </div>
        </section>

        <!-- Vista: Automatización -->
        <section id="view-automation" class="hidden">
            <header>
                <h1>Automatización de Procesos</h1>
            </header>
            
            <div class="stat-card" style="margin-bottom: 1rem;">
                <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 1rem;">
                    <h3><i class="ph ph-whatsapp-logo" style="color: #25D366;"></i> Mensaje de Bienvenida</h3>
                    <label style="display: flex; align-items: center; gap: 0.5rem; cursor: pointer;">
                        <input type="checkbox" checked onchange="toggleAutomation('whatsapp', this.checked)"> Activo
                    </label>
                </div>
                <p style="font-size: 0.9rem; color: var(--text-light);">Cuando un lead entra a la etapa "Nuevo", enviar un mensaje automático: "Hola [Nombre], gracias por tu interés en EWE Store Tunja. ¿Qué modelo buscas?"</p>
            </div>

            <div class="stat-card">
                <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 1rem;">
                    <h3><i class="ph ph-lightning" style="color: var(--warning);"></i> Alerta de Lead Caliente</h3>
                    <label style="display: flex; align-items: center; gap: 0.5rem; cursor: pointer;">
                        <input type="checkbox" checked onchange="toggleAutomation('hot-lead', this.checked)"> Activo
                    </label>
                </div>
                <p style="font-size: 0.9rem; color: var(--text-light);">Si la IA califica un lead con > 85 puntos, notificar inmediatamente al equipo de ventas.</p>
            </div>
        </section>
    </main>

    <!-- Modal: Agregar Lead -->
    <div class="modal-overlay" id="add-lead-modal">
        <div class="modal">
            <h2 style="margin-bottom: 1rem;">Agregar Nuevo Lead</h2>
            <form id="add-lead-form" onsubmit="handleAddLead(event)">
                <div class="form-group">
                    <label>Nombre Completo</label>
                    <input type="text" name="name" required placeholder="Ej: Juan Pérez">
                </div>
                
                <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 1rem;">
                    <div class="form-group">
                        <label>Teléfono / WhatsApp</label>
                        <input type="tel" name="phone" placeholder="Ej: 300 123 4567">
                    </div>
                    <div class="form-group">
                        <label>Correo Electrónico</label>
                        <input type="email" name="email" placeholder="ejemplo@email.com">
                    </div>
                </div>

                <div class="form-group">
                    <label>¿Dónde nos conoció?</label>
                    <select name="source">
                        <option value="instagram">Instagram</option>
                        <option value="facebook">Facebook</option>
                        <option value="whatsapp">WhatsApp</option>
                        <option value="google">Google / Maps</option>
                        <option value="store">Tienda Física</option>
                        <option value="other">Otro</option>
                    </select>
                </div>

                <div class="form-group">
                    <label>Modelo de Interés</label>
                    <select name="model">
                        <option value="TRX">TRX</option>
                        <option value="Strong">Strong</option>
                        <option value="Bee">Bee</option>
                        <option value="Raptor">Raptor</option>
                    </select>
                </div>
                <div class="form-group">
                    <label>Presupuesto (COP)</label>
                    <input type="number" name="budget" required placeholder="Ej: 3500000">
                </div>
                <div class="form-group">
                    <label>Estado</label>
                    <select name="status">
                        <option value="new">Nuevo</option>
                        <option value="contacted">Contactado</option>
                        <option value="negotiating">Negociación</option>
                    </select>
                </div>
                <div style="display: flex; gap: 1rem; justify-content: flex-end; margin-top: 1.5rem;">
                    <button type="button" class="btn btn-outline" onclick="closeAddLeadModal()">Cancelar</button>
                    <button type="submit" class="btn btn-primary">Guardar</button>
                </div>
            </form>
        </div>
    </div>

    <!-- Contenedor de Notificaciones -->
    <div class="toast-container" id="toast-container"></div>

    <script>
        // --- CONFIGURACIÓN Y UTILIDADES ---
        const formatCurrency = (amount) => {
            return new Intl.NumberFormat('es-CO', { style: 'currency', currency: 'COP', maximumFractionDigits: 0 }).format(amount);
        };

        // --- ESTADO DE LA APLICACIÓN ---
        let leads = [
            { id: 1, name: "Carlos Rodríguez", phone: "310 555 0101", email: "carlos@mail.com", source: "instagram", model: "TRX", budget: 4500000, status: "new", score: 65, aiAction: "Enviar catálogo PDF" },
            { id: 2, name: "Maria Gonzales", phone: "315 555 0202", email: "maria@mail.com", source: "facebook", model: "Bee", budget: 2800000, status: "contacted", score: 88, aiAction: "Ofrecer prueba en Tunja" },
            { id: 3, name: "Pedro Martínez", phone: "300 555 0303", email: "pedro@mail.com", source: "whatsapp", model: "Raptor", budget: 6000000, status: "negotiating", score: 92, aiAction: "Cierre urgente - 5% dto" },
            { id: 4, name: "Ana Silva", phone: "320 555 0404", email: "ana@mail.com", source: "google", model: "Strong", budget: 3200000, status: "new", score: 40, aiAction: "Nutrir con email marketing" }
        ];

        const columns = [
            { id: 'new', title: 'Nuevo' },
            { id: 'contacted', title: 'Contactado' },
            { id: 'negotiating', title: 'Negociación' },
            { id: 'won', title: 'Venta Cerrada' },
            { id: 'lost', title: 'Perdido' }
        ];

        const automations = {
            whatsapp: true,
            hotLead: true
        };

        // --- INICIALIZACIÓN ---
        function init() {
            renderStats();
            renderKanban();
            setupDragAndDrop();
            setupFileImport();
        }

        // --- NAVEGACIÓN ---
        function showView(viewName) {
            document.querySelectorAll('main > section').forEach(el => el.classList.add('hidden'));
            document.getElementById(`view-${viewName}`).classList.remove('hidden');
            document.querySelectorAll('nav button').forEach(btn => btn.classList.remove('active'));
            event.currentTarget.classList.add('active');
            
            if(viewName === 'dashboard') {
                renderKanban();
                renderStats();
            }
        }

        // --- RENDERIZADO (UI) ---
        function renderStats() {
            document.getElementById('stat-total').innerText = leads.length;
            const totalValue = leads.reduce((sum, lead) => sum + lead.budget, 0);
            document.getElementById('stat-pipeline').innerText = formatCurrency(totalValue);
        }

        function getSourceIcon(source) {
            switch(source) {
                case 'instagram': return 'ph-instagram-logo';
                case 'facebook': return 'ph-facebook-logo';
                case 'whatsapp': return 'ph-whatsapp-logo';
                case 'google': return 'ph-google-logo';
                case 'store': return 'ph-storefront';
                default: return 'ph-globe';
            }
        }

        function renderKanban() {
            const board = document.getElementById('dashboard-kanban');
            board.innerHTML = '';

            columns.forEach(col => {
                const colDiv = document.createElement('div');
                colDiv.className = 'kanban-column';
                colDiv.setAttribute('data-status', col.id);
                
                const colLeads = leads.filter(l => l.status === col.id);

                let cardsHTML = '';
                colLeads.forEach(lead => {
                    let scoreClass = lead.score > 80 ? 'score-high' : (lead.score > 50 ? 'score-med' : 'score-low');
                    const iconClass = getSourceIcon(lead.source);
                    
                    cardsHTML += `
                        <div class="lead-card" draggable="true" data-id="${lead.id}">
                            <div class="card-top">
                                <span class="lead-name">${lead.name}</span>
                                <span class="lead-budget">${formatCurrency(lead.budget)}</span>
                            </div>
                            <span class="lead-model">${lead.model}</span>
                            
                            <div class="contact-row">
                                <span title="Origen: ${lead.source}"><i class="ph ${iconClass}" style="font-weight:bold"></i></span>
                                <span title="Teléfono"><i class="ph ph-phone"></i> ${lead.phone || 'N/A'}</span>
                            </div>

                            <div class="ai-score">
                                <span class="score-dot ${scoreClass}"></span>
                                <span>IA Score: ${lead.score} - ${lead.aiAction}</span>
                            </div>
                        </div>
                    `;
                });

                colDiv.innerHTML = `
                    <div class="column-header">
                        <h4>${col.title}</h4>
                        <span class="badge">${colLeads.length}</span>
                    </div>
                    <div class="cards-container">${cardsHTML}</div>
                `;
                board.appendChild(colDiv);
            });

            setupDragAndDrop(); 
        }

        // --- DRAG AND DROP ---
        let draggedId = null;

        function setupDragAndDrop() {
            const cards = document.querySelectorAll('.lead-card');
            const columns = document.querySelectorAll('.kanban-column');

            cards.forEach(card => {
                card.addEventListener('dragstart', (e) => {
                    draggedId = parseInt(e.target.getAttribute('data-id'));
                    e.target.classList.add('dragging');
                });
                card.addEventListener('dragend', (e) => {
                    e.target.classList.remove('dragging');
                    draggedId = null;
                });
            });

            columns.forEach(col => {
                col.addEventListener('dragover', (e) => {
                    e.preventDefault();
                });

                col.addEventListener('drop', (e) => {
                    e.preventDefault();
                    const newStatus = col.getAttribute('data-status');
                    if (draggedId) {
                        moveLead(draggedId, newStatus);
                    }
                });
            });
        }

        function moveLead(id, newStatus) {
            const lead = leads.find(l => l.id === id);
            if (lead && lead.status !== newStatus) {
                lead.status = newStatus;
                checkAutomations(lead, newStatus);
                runAIScoring(lead);
                renderKanban();
                renderStats();
                showToast(`Lead movido a ${columns.find(c=>c.id===newStatus).title}`, 'success');
            }
        }

        // --- INTELIGENCIA ARTIFICIAL (SIMULADA) ---
        function runAIScoring(lead) {
            let score = 50;
            if(lead.budget > 4000000) score += 20;
            if(lead.status === 'negotiating') score += 30;
            score += Math.floor(Math.random() * 10); 
            if(score > 100) score = 100;

            lead.score = score;

            if (score > 90) lead.aiAction = "Prioridad Alta - Llamar Ya";
            else if (score > 75) lead.aiAction = "Enviar cotización detallada";
            else if (lead.status === 'new') lead.aiAction = "Enviar video de presentación";
            else lead.aiAction = "Seguimiento pasivo";
        }

        function checkAutomations(lead, newStatus) {
            if (automations.whatsapp && newStatus === 'new') {
                setTimeout(() => {
                    showToast(`🤖 [Auto] WhatsApp enviado a ${lead.phone || lead.name}`, 'success');
                }, 1500);
            }

            runAIScoring(lead);
            if (automations.hotLead && lead.score > 85) {
                setTimeout(() => {
                    showToast(`⚡ [IA] ¡Alerta! ${lead.name} es un Lead Caliente (${lead.score} pts).`, 'warning');
                }, 2500);
            }
        }

        function refreshAI() {
            const btn = event.target;
            const originalText = btn.innerText;
            btn.innerText = "Analizando...";
            
            leads.forEach(l => runAIScoring(l));
            renderKanban();
            
            setTimeout(() => {
                btn.innerText = originalText;
                showToast("Análisis de IA actualizado", 'success');
                const hotLeads = leads.filter(l => l.score > 80).length;
                document.getElementById('ai-recommendation').innerText = 
                    `Análisis completado. ${hotLeads} leads requieren atención inmediata para evitar perder la venta.`;
            }, 800);
        }

        function toggleAutomation(key, isActive) {
            automations[key] = isActive;
            showToast(`Automatización ${isActive ? 'activada' : 'desactivada'}`, 'info');
        }

        // --- IMPORTACIÓN DE ARCHIVOS ---
        function setupFileImport() {
            const dropZone = document.getElementById('drop-zone');
            const fileInput = document.getElementById('file-input');

            dropZone.addEventListener('dragover', (e) => {
                e.preventDefault();
                dropZone.style.backgroundColor = '#d1f2ea';
            });

            dropZone.addEventListener('dragleave', (e) => {
                e.preventDefault();
                dropZone.style.backgroundColor = '#e6fffa';
            });

            dropZone.addEventListener('drop', (e) => {
                e.preventDefault();
                dropZone.style.backgroundColor = '#e6fffa';
                const files = e.dataTransfer.files;
                if(files.length) processFile(files[0]);
            });

            fileInput.addEventListener('change', (e) => {
                if(e.target.files.length) processFile(e.target.files[0]);
            });
        }

        function processFile(file) {
            const log = document.getElementById('import-log');
            log.innerHTML = `<li>Leyendo archivo: ${file.name}...</li>`;

            setTimeout(() => {
                if (file.name.endsWith('.csv')) {
                    const reader = new FileReader();
                    reader.onload = function(e) {
                        const text = e.target.result;
                        const lines = text.split('\n').slice(1);
                        let count = 0;
                        
                        lines.forEach((line, index) => {
                            const cols = line.split(',');
                            if(cols.length >= 3) {
                                const name = cols[0]?.trim() || `Lead Importado ${index}`;
                                const budget = parseInt(cols[1]?.trim()) || 3000000;
                                const model = cols[2]?.trim() || "Genérico";
                                
                                leads.push({
                                    id: Date.now() + index,
                                    name: name,
                                    phone: "Sin dato",
                                    email: "Sin dato",
                                    source: "other",
                                    model: model,
                                    budget: budget,
                                    status: 'new',
                                    score: 0,
                                    aiAction: 'Pendiente análisis'
                                });
                                count++;
                            }
                        });
                        
                        log.innerHTML += `<li style="color: green">✓ CSV procesado: ${count} leads importados.</li>`;
                        runAIScoring(leads[leads.length-1]);
                        showToast(`Importación exitosa: ${count} leads`, 'success');
                        renderStats();
                    };
                    reader.readAsText(file);
                } else {
                    log.innerHTML += `<li>Detectado formato binario. Iniciando motor...</li>`;
                    setTimeout(() => {
                        const mockNames = ["Jorge Vélez", "Luisa Fernanda", "Carlos Duplat"];
                        mockNames.forEach((name, i) => {
                            leads.push({
                                id: Date.now() + i,
                                name: name,
                                phone: "300 000 000" + i,
                                email: "importado@crm.com",
                                source: "facebook",
                                model: "Raptor",
                                budget: 2500000,
                                status: 'new',
                                score: Math.floor(Math.random() * 60),
                                aiAction: 'Datos extraídos'
                            });
                        });
                        log.innerHTML += `<li style="color: green">✓ Extracción completada: 3 leads simulados.</li>`;
                        showToast("Extracción simulada completada", 'success');
                        renderStats();
                    }, 2000);
                }
            }, 1000);
        }

        // --- GESTIÓN DE MODALES ---
        function openAddLeadModal() {
            document.getElementById('add-lead-modal').style.display = 'flex';
        }
        function closeAddLeadModal() {
            document.getElementById('add-lead-modal').style.display = 'none';
        }
        function handleAddLead(e) {
            e.preventDefault();
            const formData = new FormData(e.target);
            
            const newLead = {
                id: Date.now(),
                name: formData.get('name'),
                phone: formData.get('phone'),
                email: formData.get('email'),
                source: formData.get('source'),
                model: formData.get('model'),
                budget: parseInt(formData.get('budget')),
                status: formData.get('status'),
                score: 50,
                aiAction: "Nuevo registro"
            };

            runAIScoring(newLead);
            leads.push(newLead);
            
            closeAddLeadModal();
            renderKanban();
            renderStats();
            showToast("Lead creado correctamente", 'success');
            e.target.reset();
        }

        // --- NOTIFICACIONES (TOASTS) ---
        function showToast(message, type = 'info') {
            const container = document.getElementById('toast-container');
            const toast = document.createElement('div');
            toast.className = 'toast';
            
            let icon = 'ph-info';
            if(type === 'success') { toast.style.borderLeftColor = 'var(--success)'; icon = 'ph-check-circle'; }
            if(type === 'warning') { toast.style.borderLeftColor = 'var(--warning)'; icon = 'ph-warning'; }
            
            toast.innerHTML = `<i class="ph ${icon}" style="font-size:1.2rem;"></i> <span>${message}</span>`;
            
            container.appendChild(toast);
            
            setTimeout(() => {
                toast.style.opacity = '0';
                toast.style.transform = 'translateX(100%)';
                setTimeout(() => toast.remove(), 300);
            }, 4000);
        }

        // Arrancar aplicación
        init();

    </script>
</body>
</html>
