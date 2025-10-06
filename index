<!DOCTYPE html>
<html lang="es" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dashboard del Proyecto: Plataforma de Órdenes de Compra</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/date-fns"></script>
    <script src="https://cdn.jsdelivr.net/npm/chartjs-adapter-date-fns"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <!-- Chosen Palette: Warm Neutrals -->
    <!-- Application Structure Plan: He diseñado la aplicación como un dashboard de una sola página con una barra de navegación lateral fija (en escritorio) para un acceso rápido y no lineal a la información. En lugar de forzar al usuario a desplazarse por un documento largo, puede saltar directamente a la sección de interés, como el 'Cronograma' o los 'Riesgos'. He agrupado secciones relacionadas como 'Alcance y Arquitectura' para proporcionar un contexto completo. La sección principal y más interactiva es el 'Cronograma', que transforma los datos de la tabla estática en un diagrama de Gantt visual y dinámico. Esta estructura fue elegida para mejorar la usabilidad y la exploración de datos, permitiendo a diferentes roles (gerentes, desarrolladores) encontrar rápidamente la información que necesitan. -->
    <!-- Visualization & Content Choices: 
        1. Report Info: Cronograma del Proyecto (Fases, Sprints, Fechas). Goal: Change. Viz/Presentation: Diagrama de Gantt interactivo. Interaction: Al hacer clic en una barra del gráfico, se muestra información detallada del sprint/fase en un panel de texto. Al pasar el cursor, se muestran las fechas. Justification: Un gráfico de Gantt es la forma estándar y más intuitiva de visualizar un cronograma de proyecto, y la interactividad permite explorar detalles sin saturar la vista principal. Library/Method: Chart.js (Gráfico de barras horizontales con eje de tiempo).
        2. Report Info: Riesgos Potenciales (Tabla). Goal: Organize/Compare. Viz/Presentation: Tabla HTML interactiva. Interaction: Botones de filtro que permiten a los usuarios mostrar riesgos por nivel de impacto (Alto, Medio). Justification: El filtrado permite a los usuarios centrarse en los riesgos más críticos, haciendo la tabla más manejable y útil para la toma de decisiones. Library/Method: Vanilla JavaScript para manipular clases de CSS (mostrar/ocultar filas).
        3. Report Info: Arquitectura de la Solución (Diagrama ASCII). Goal: Organize. Viz/Presentation: Diagrama de flujo construido con HTML/CSS. Interaction: Efectos de hover para resaltar los componentes del diagrama. Justification: Recrear el diagrama con HTML/CSS en lugar de una imagen lo mantiene ligero, adaptable y alineado con la restricción de no usar SVG. Library/Method: HTML semántico con Tailwind CSS (Flexbox, Grid, Borders).
        4. Report Info: Alcance (Dentro/Fuera). Goal: Inform. Viz/Presentation: Columnas lado a lado. Interaction: Ninguna, es informativo. Justification: La presentación en dos columnas es una forma clara y rápida de comparar lo que está incluido y lo que no. Library/Method: HTML con Tailwind CSS (Grid).
    -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #F8F9FA;
            color: #343A40;
        }
        .nav-link {
            transition: all 0.2s ease-in-out;
            border-left: 3px solid transparent;
        }
        .nav-link.active, .nav-link:hover {
            background-color: #E9ECEF;
            border-left-color: #007BFF;
            color: #0056b3;
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 1200px;
            margin-left: auto;
            margin-right: auto;
            height: 400px;
            max-height: 500px;
        }
        @media (min-width: 768px) {
            .chart-container {
                height: 500px;
                max-height: 600px;
            }
        }
        .architecture-box {
            border: 2px solid #6C757D;
            background-color: #FFFFFF;
            padding: 0.75rem;
            border-radius: 0.5rem;
            text-align: center;
            box-shadow: 0 2px 4px rgba(0,0,0,0.05);
            transition: all 0.2s ease-in-out;
        }
        .architecture-box:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
            border-color: #007BFF;
        }
        .connector {
            position: absolute;
            background-color: #6C757D;
            z-index: -1;
        }
        .risk-tag {
            padding: 0.25rem 0.75rem;
            border-radius: 9999px;
            font-size: 0.75rem;
            font-weight: 600;
        }
        .risk-high { background-color: #FECACA; color: #991B1B; }
        .risk-medium { background-color: #FEF08A; color: #854D0E; }
    </style>
</head>
<body class="flex flex-col md:flex-row">

    <aside class="w-full md:w-64 bg-white border-r border-gray-200 md:h-screen md:sticky md:top-0">
        <div class="p-6">
            <h1 class="text-xl font-bold text-gray-800">Proyecto OC</h1>
            <p class="text-sm text-gray-500">Plataforma de Compras</p>
        </div>
        <nav id="main-nav" class="flex flex-row md:flex-col p-3 md:p-0 md:mt-4">
            <a href="#overview" class="nav-link active font-medium text-gray-700 py-2.5 px-4 rounded-md w-full text-left">Visión General</a>
            <a href="#scope" class="nav-link font-medium text-gray-700 py-2.5 px-4 rounded-md w-full text-left">Alcance y Arquitectura</a>
            <a href="#timeline" class="nav-link font-medium text-gray-700 py-2.5 px-4 rounded-md w-full text-left">Cronograma Interactivo</a>
            <a href="#team" class="nav-link font-medium text-gray-700 py-2.5 px-4 rounded-md w-full text-left">Equipo y Riesgos</a>
            <a href="#next-steps" class="nav-link font-medium text-gray-700 py-2.5 px-4 rounded-md w-full text-left">Próximos Pasos</a>
        </nav>
    </aside>

    <main class="flex-1 p-6 md:p-10">
        
        <section id="overview" class="mb-16">
            <h2 class="text-3xl font-bold text-gray-800 mb-2">Visión General del Proyecto</h2>
            <p class="text-lg text-gray-600 mb-8">Esta sección resume el propósito, los desafíos y los beneficios clave de la implementación de la nueva plataforma de solicitudes de órdenes de compra. El objetivo es claro: transformar un proceso manual en una solución digital, eficiente e integrada.</p>

            <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
                <div class="bg-white p-6 rounded-lg shadow-md border border-gray-200">
                    <h3 class="text-xl font-semibold text-gray-700 mb-4">El Desafío Actual</h3>
                    <ul class="space-y-3 text-gray-600">
                        <li class="flex items-start"><span class="mr-2 text-red-500">🔴</span> Proceso manual, lento y propenso a errores.</li>
                        <li class="flex items-start"><span class="mr-2 text-red-500">🔴</span> Falta de visibilidad y trazabilidad de solicitudes.</li>
                        <li class="flex items-start"><span class="mr-2 text-red-500">🔴</span> Alto consumo de tiempo en tareas administrativas.</li>
                        <li class="flex items-start"><span class="mr-2 text-red-500">🔴</span> Dificultad para generar reportes y analizar datos.</li>
                    </ul>
                </div>

                <div class="bg-white p-6 rounded-lg shadow-md border border-gray-200">
                    <h3 class="text-xl font-semibold text-gray-700 mb-4">Beneficios Esperados</h3>
                    <ul class="space-y-3 text-gray-600">
                        <li class="flex items-start"><span class="mr-2 text-green-500">✅</span> Reducción significativa del tiempo de ciclo del proceso.</li>
                        <li class="flex items-start"><span class="mr-2 text-green-500">✅</span> Aumento de la productividad de los equipos involucrados.</li>
                        <li class="flex items-start"><span class="mr-2 text-green-500">✅</span> Visibilidad completa y en tiempo real del estado de las OCs.</li>
                        <li class="flex items-start"><span class="mr-2 text-green-500">✅</span> Toma de decisiones basada en datos con KPIs claros.</li>
                    </ul>
                </div>
            </div>
        </section>

        <section id="scope" class="mb-16">
            <h2 class="text-3xl font-bold text-gray-800 mb-2">Alcance y Arquitectura</h2>
            <p class="text-lg text-gray-600 mb-8">Aquí se define qué construye el proyecto y cómo se conectan sus componentes tecnológicos. Explore los límites de la solución y la interacción entre las herramientas de Power Platform y SAP.</p>
            
            <div class="bg-white p-6 rounded-lg shadow-md border border-gray-200 mb-8">
                <h3 class="text-xl font-semibold text-gray-700 mb-4">Alcance de la Solución</h3>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <div>
                        <h4 class="font-semibold text-green-600 mb-2">En Alcance</h4>
                        <ul class="list-disc list-inside space-y-2 text-gray-600">
                            <li>Formulario de creación de solicitudes en Power Apps.</li>
                            <li>Flujos de aprobación multinivel (Power Automate).</li>
                            <li>Notificaciones automáticas por correo electrónico.</li>
                            <li>Creación automática de OC en SAP vía script.</li>
                            <li>Roles de seguridad (solicitante, aprobador, admin).</li>
                            <li>Dashboard de seguimiento y KPIs en Power BI.</li>
                            <li>Capacitación y manuales de usuario.</li>
                        </ul>
                    </div>
                    <div>
                        <h4 class="font-semibold text-red-600 mb-2">Fuera de Alcance</h4>
                         <ul class="list-disc list-inside space-y-2 text-gray-600">
                            <li>Modificaciones complejas a módulos existentes de SAP.</li>
                            <li>Gestión de proveedores (altas/bajas/modificaciones).</li>
                            <li>Módulo de gestión de presupuestos.</li>
                            <li>Gestión de facturas y pagos.</li>
                            <li>Integración con otros sistemas ERP o de terceros.</li>
                        </ul>
                    </div>
                </div>
            </div>
            
            <div class="bg-white p-8 rounded-lg shadow-md border border-gray-200">
                <h3 class="text-xl font-semibold text-gray-700 mb-6">Arquitectura de la Solución</h3>
                <div class="flex flex-col items-center space-y-8">
                    <div class="flex items-center space-x-4">
                        <div class="architecture-box w-32">
                            <p class="font-semibold">Usuarios</p>
                        </div>
                        <p class="font-mono text-gray-500">--&gt;</p>
                        <div class="architecture-box w-48">
                            <p class="font-semibold">Power Apps</p>
                            <p class="text-xs text-gray-500">(Interfaz Web/Móvil)</p>
                        </div>
                         <p class="font-mono text-gray-500">--&gt;</p>
                         <div class="architecture-box w-48">
                            <p class="font-semibold">Dataverse</p>
                            <p class="text-xs text-gray-500">(Base de Datos)</p>
                        </div>
                    </div>
                     <div class="flex space-x-32">
                        <div class="w-px bg-gray-400 h-8"></div>
                        <div class="w-px bg-gray-400 h-8"></div>
                    </div>
                    <div class="flex items-center space-x-4">
                        <div class="architecture-box w-48">
                            <p class="font-semibold">Power Automate</p>
                            <p class="text-xs text-gray-500">(Flujos/Aprobaciones)</p>
                        </div>
                         <p class="font-mono text-gray-500">--&gt;</p>
                        <div class="architecture-box w-48">
                            <p class="font-semibold">SAP</p>
                            <p class="text-xs text-gray-500">(Crea OC)</p>
                        </div>
                    </div>
                     <div class="w-px bg-gray-400 h-8 ml-[-6rem]"></div>
                      <div class="architecture-box w-48 self-start ml-[-14rem]">
                            <p class="font-semibold">Power BI</p>
                            <p class="text-xs text-gray-500">(Reportes/KPIs)</p>
                        </div>
                </div>
            </div>
        </section>

        <section id="timeline" class="mb-16">
            <h2 class="text-3xl font-bold text-gray-800 mb-2">Cronograma Interactivo</h2>
            <p class="text-lg text-gray-600 mb-8">Visualice el plan del proyecto de 4 meses. Haga clic en cualquier barra para ver los hitos clave de esa fase o sprint en el panel de detalles a continuación. El área sombreada indica el período de vacaciones programado.</p>
            
            <div class="bg-white p-6 rounded-lg shadow-md border border-gray-200">
                <div class="chart-container">
                    <canvas id="timelineChart"></canvas>
                </div>
                <div id="timeline-details" class="mt-6 p-4 bg-blue-50 border border-blue-200 rounded-lg">
                    <h4 class="font-semibold text-lg text-blue-800 mb-2">Detalles de la Selección</h4>
                    <p class="text-blue-700">Haga clic en una barra del gráfico para ver los detalles aquí.</p>
                </div>
            </div>
        </section>

        <section id="team" class="mb-16">
            <h2 class="text-3xl font-bold text-gray-800 mb-2">Equipo y Riesgos</h2>
            <p class="text-lg text-gray-600 mb-8">Conozca los roles clave que impulsan el proyecto y los riesgos potenciales que gestionaremos. Utilice los filtros para centrarse en los riesgos de mayor impacto.</p>

            <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
                <div class="bg-white p-6 rounded-lg shadow-md border border-gray-200">
                    <h3 class="text-xl font-semibold text-gray-700 mb-4">Roles Clave</h3>
                    <div class="space-y-4">
                        <div class="p-3 bg-gray-50 rounded-md">
                            <p class="font-semibold text-gray-800">Dueño del Producto (Product Owner)</p>
                            <p class="text-sm text-gray-600">Define las prioridades y representa a los stakeholders.</p>
                        </div>
                        <div class="p-3 bg-gray-50 rounded-md">
                            <p class="font-semibold text-gray-800">Scrum Master / Project Manager</p>
                            <p class="text-sm text-gray-600">Facilita el proceso y elimina impedimentos.</p>
                        </div>
                        <div class="p-3 bg-gray-50 rounded-md">
                            <p class="font-semibold text-gray-800">Equipo de Desarrollo</p>
                            <p class="text-sm text-gray-600">Expertos en Power Platform, SAP (ABAP), y Analistas de Negocio/QA.</p>
                        </div>
                    </div>
                </div>

                <div class="bg-white p-6 rounded-lg shadow-md border border-gray-200">
                    <h3 class="text-xl font-semibold text-gray-700 mb-4">Riesgos Potenciales</h3>
                    <div class="flex space-x-2 mb-4">
                        <button class="risk-filter-btn active px-4 py-2 bg-blue-500 text-white rounded-md text-sm" data-filter="all">Todos</button>
                        <button class="risk-filter-btn px-4 py-2 bg-gray-200 text-gray-700 rounded-md text-sm" data-filter="Alto">Alto</button>
                        <button class="risk-filter-btn px-4 py-2 bg-gray-200 text-gray-700 rounded-md text-sm" data-filter="Medio">Medio</button>
                    </div>
                    <div id="risk-list" class="space-y-3">
                         <div class="risk-item p-3 border rounded-md" data-impact="Alto">
                            <div class="flex justify-between items-center">
                                <p class="font-medium text-gray-800">Complejidad en la integración con SAP</p>
                                <span class="risk-tag risk-high">Alto</span>
                            </div>
                            <p class="text-sm text-gray-600 mt-1">Mitigación: Involucrar al equipo SAP desde el Sprint 0 y realizar pruebas de concepto tempranas.</p>
                        </div>
                         <div class="risk-item p-3 border rounded-md" data-impact="Alto">
                            <div class="flex justify-between items-center">
                                <p class="font-medium text-gray-800">Baja adopción por parte de los usuarios</p>
                                <span class="risk-tag risk-high">Alto</span>
                            </div>
                             <p class="text-sm text-gray-600 mt-1">Mitigación: Diseño centrado en el usuario (UX), involucrar a usuarios clave y plan de capacitación robusto.</p>
                        </div>
                         <div class="risk-item p-3 border rounded-md" data-impact="Medio">
                            <div class="flex justify-between items-center">
                                <p class="font-medium text-gray-800">Cambios en los requerimientos</p>
                                <span class="risk-tag risk-medium">Medio</span>
                            </div>
                             <p class="text-sm text-gray-600 mt-1">Mitigación: Usar metodología Agile para gestionar y priorizar cambios con el Product Owner.</p>
                        </div>
                         <div class="risk-item p-3 border rounded-md" data-impact="Medio">
                            <div class="flex justify-between items-center">
                                <p class="font-medium text-gray-800">Disponibilidad de usuarios para pruebas</p>
                                <span class="risk-tag risk-medium">Medio</span>
                            </div>
                             <p class="text-sm text-gray-600 mt-1">Mitigación: Agendar sesiones de UAT con anticipación y comunicar su importancia.</p>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <section id="next-steps" class="mb-16">
            <h2 class="text-3xl font-bold text-gray-800 mb-2">Próximos Pasos</h2>
            <p class="text-lg text-gray-600 mb-8">Estas son las acciones inmediatas para poner en marcha el proyecto. La colaboración de todos es fundamental para cumplir con nuestros objetivos iniciales.</p>
            <div class="bg-white p-6 rounded-lg shadow-md border border-gray-200">
                <ol class="list-decimal list-inside space-y-4 text-gray-700 text-lg">
                    <li><span class="font-semibold">Validación del Equipo de Proyecto y Roles:</span> Confirmar a todos los participantes clave esta semana.</li>
                    <li><span class="font-semibold">Sesiones de Taller para Detallar Historias de Usuario:</span> Agendar reuniones con los departamentos involucrados.</li>
                    <li><span class="font-semibold">Configuración de Entornos de Desarrollo:</span> Iniciar la creación de los ambientes en Power Platform y SAP.</li>
                    <li><span class="font-semibold">Reunión de Kick-Off Técnico:</span> Alinear equipos de desarrollo de Power Platform y SAP.</li>
                </ol>
            </div>
        </section>
    </main>

<script>
document.addEventListener('DOMContentLoaded', () => {

    const timelineData = {
        sprints: [
            { id: 'sprint0', name: 'Sprint 0: Planificación', start: '2025-11-01', end: '2025-11-07', details: 'Kick-Off, Backlog inicial, Arquitectura definida.' },
            { id: 'sprint1', name: 'Sprint 1: Modelo de Datos', start: '2025-11-10', end: '2025-11-21', details: 'Modelo de datos y primer formulario funcional de solicitud.' },
            { id: 'sprint2', name: 'Sprint 2: Flujos de Aprobación', start: '2025-11-24', end: '2025-12-05', details: 'Flujos de aprobación y notificaciones implementados.' },
            { id: 'sprint3', name: 'Sprint 3: Aceptación y Roles', start: '2025-12-08', end: '2025-12-19', details: 'Módulo de aceptación para aprobadores y roles de seguridad.' },
            { id: 'sprint4', name: 'Sprint 4: Integración SAP', start: '2026-01-12', end: '2026-01-30', details: 'Integración con SAP completada y probada.' },
            { id: 'sprint5', name: 'Sprint 5: Power BI y UAT', start: '2026-02-02', end: '2026-02-13', details: 'Dashboards en Power BI y primera ronda de Pruebas de Usuario (UAT).' },
            { id: 'sprint6', name: 'Sprint 6: Go-Live', start: '2026-02-16', end: '2026-02-28', details: 'UAT Final, Capacitaciones y Go-Live (26 de Febrero).' }
        ],
        vacation: {
            start: '2025-12-22',
            end: '2026-01-09'
        }
    };

    const detailsPanel = document.getElementById('timeline-details');

    const timelineChartCtx = document.getElementById('timelineChart').getContext('2d');
    const timelineChart = new Chart(timelineChartCtx, {
        type: 'bar',
        data: {
            labels: timelineData.sprints.map(s => s.name),
            datasets: [{
                label: 'Duración del Sprint',
                data: timelineData.sprints.map(s => [s.start, s.end]),
                backgroundColor: 'rgba(0, 123, 255, 0.7)',
                borderColor: 'rgba(0, 123, 255, 1)',
                borderWidth: 1,
                borderRadius: 4,
                borderSkipped: false,
            }]
        },
        options: {
            indexAxis: 'y',
            responsive: true,
            maintainAspectRatio: false,
            scales: {
                x: {
                    type: 'time',
                    time: {
                        unit: 'week',
                        tooltipFormat: 'MMM dd, yyyy'
                    },
                    min: '2025-10-27',
                    max: '2026-03-03',
                    title: {
                        display: true,
                        text: 'Fecha'
                    }
                },
                y: {
                    beginAtZero: true
                }
            },
            plugins: {
                legend: {
                    display: false
                },
                tooltip: {
                    callbacks: {
                        title: function(context) {
                            return context[0].label;
                        },
                        label: function(context) {
                            const start = new Date(context.raw[0]);
                            const end = new Date(context.raw[1]);
                            return `De: ${start.toLocaleDateString('es-ES')} a ${end.toLocaleDateString('es-ES')}`;
                        }
                    }
                },
                annotation: {
                    annotations: {
                        vacationBox: {
                            type: 'box',
                            xMin: timelineData.vacation.start,
                            xMax: timelineData.vacation.end,
                            backgroundColor: 'rgba(108, 117, 125, 0.15)',
                            borderColor: 'rgba(108, 117, 125, 0.3)',
                            borderWidth: 1,
                            label: {
                                content: 'Vacaciones',
                                display: true,
                                position: 'start',
                                color: '#6c757d'
                            }
                        }
                    }
                }
            },
            onClick: (event, elements) => {
                if (elements.length > 0) {
                    const elementIndex = elements[0].index;
                    const selectedSprint = timelineData.sprints[elementIndex];
                    detailsPanel.innerHTML = `
                        <h4 class="font-semibold text-lg text-blue-800 mb-2">${selectedSprint.name}</h4>
                        <p class="text-blue-700">${selectedSprint.details}</p>
                    `;
                }
            }
        }
    });
    
    const riskFilterButtons = document.querySelectorAll('.risk-filter-btn');
    const riskItems = document.querySelectorAll('.risk-item');

    riskFilterButtons.forEach(button => {
        button.addEventListener('click', () => {
            const filter = button.dataset.filter;
            
            riskFilterButtons.forEach(btn => {
                btn.classList.remove('active', 'bg-blue-500', 'text-white');
                btn.classList.add('bg-gray-200', 'text-gray-700');
            });
            button.classList.add('active', 'bg-blue-500', 'text-white');
            button.classList.remove('bg-gray-200', 'text-gray-700');

            riskItems.forEach(item => {
                if (filter === 'all' || item.dataset.impact === filter) {
                    item.style.display = 'block';
                } else {
                    item.style.display = 'none';
                }
            });
        });
    });

    const navLinks = document.querySelectorAll('#main-nav a');
    const sections = document.querySelectorAll('main section');

    const observerOptions = {
        root: null,
        rootMargin: '0px',
        threshold: 0.3
    };

    const observer = new IntersectionObserver((entries) => {
        entries.forEach(entry => {
            if (entry.isIntersecting) {
                navLinks.forEach(link => {
                    link.classList.remove('active');
                    if (link.getAttribute('href').substring(1) === entry.target.id) {
                        link.classList.add('active');
                    }
                });
            }
        });
    }, observerOptions);

    sections.forEach(section => {
        observer.observe(section);
    });
});
</script>

</body>
</html>
