<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cotizador Profesional - Ayala Music</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;600;700;900&display=swap');
        body { font-family: 'Inter', sans-serif; background-color: #f8fafc; }
        .card { background: white; border-radius: 1.5rem; box-shadow: 0 10px 25px -5px rgba(0,0,0,0.05); }
        .input-group { margin-bottom: 1.25rem; }
        label { display: block; font-size: 0.7rem; font-weight: 800; color: #64748b; margin-bottom: 0.4rem; text-transform: uppercase; letter-spacing: 0.05em; }
        
        input, textarea, select { 
            width: 100%; padding: 0.75rem; border: 2px solid #f1f5f9; border-radius: 0.75rem; 
            outline: none; transition: all 0.2s; font-weight: 600; font-size: 0.9rem;
        }
        input:focus, textarea:focus { border-color: #000; background-color: #fff; }
        
        .toggle-pill { display: flex; background: #f1f5f9; border-radius: 999px; padding: 3px; width: 100%; border: 1px solid #e2e8f0; }
        .toggle-pill button { flex: 1; padding: 6px 2px; border-radius: 999px; font-size: 9px; font-weight: 800; text-transform: uppercase; transition: all 0.2s; white-space: nowrap; }
        .toggle-pill button.selected { background: #000; color: white; box-shadow: 0 4px 6px rgba(0,0,0,0.1); }
        .toggle-pill button.inactive { color: #94a3b8; }

        .btn { width: 100%; padding: 1.25rem; border-radius: 1rem; font-weight: 800; text-transform: uppercase; letter-spacing: 0.1em; transition: all 0.3s; cursor: pointer; }
        .btn-primary { background-color: #000; color: white; }
        .btn-primary:hover { transform: translateY(-2px); box-shadow: 0 10px 15px -3px rgba(0,0,0,0.2); }
        
        .text-logo { font-weight: 900; letter-spacing: -0.02em; text-transform: uppercase; line-height: 1; }
        
        /* OPTIMIZACIÓN PARA IMPRESIÓN */
        @media print {
            .no-print { display: none !important; }
            body { background: white !important; padding: 0 !important; margin: 0 !important; }
            .card { box-shadow: none !important; border: none !important; padding: 0 !important; width: 100% !important; max-width: 100% !important; }
            #client-view { display: block !important; width: 100% !important; max-width: 100% !important; padding: 0 !important; margin: 0 !important; }
            .shadow-2xl { box-shadow: none !important; }
            @page { margin: 1cm; }
        }

        .animate-fade { animation: fadeIn 0.4s ease-out forwards; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body class="p-4 md:p-12">

    <!-- VISTA INTERNA (CONFIGURACIÓN) -->
    <div id="calculator-view" class="max-w-2xl mx-auto animate-fade no-print">
        <div class="text-center mb-10">
            <h1 class="text-logo text-2xl text-black">Ayala Music</h1>
            <p class="text-[9px] font-black text-slate-400 uppercase tracking-[0.4em] mt-2">Panel de Configuración</p>
        </div>

        <div class="grid md:grid-cols-2 gap-6">
            <div class="card p-6">
                <label class="border-b pb-2 mb-4">Ficha Técnica</label>
                
                <div class="input-group">
                    <label>Modelo del Bajo Quinto</label>
                    <input type="text" id="inp-modelo" placeholder="Ej: Clásico Imperial" value="Modelo Especial Ayala">
                </div>

                <div class="input-group">
                    <label>Maderas (Tapa, Cuerpo, Brazo)</label>
                    <textarea id="inp-maderas" rows="2" placeholder="Ej: Tapa de Abeto, Cuerpo de Palo Escrito...">Tapa de Pinabete Alemán, Cuerpo de Palo Escrito, Brazo de Cedro.</textarea>
                </div>

                <div class="input-group">
                    <label>Especificaciones Adicionales</label>
                    <textarea id="inp-specs" rows="2" placeholder="Ej: Diapasón de Ébano, Maquinaria de precisión...">Diapasón de ébano, fileteado artesanal, acabado en laca brillante.</textarea>
                </div>

                <div class="grid grid-cols-2 gap-4">
                    <div class="input-group">
                        <label>Fecha</label>
                        <input type="date" id="inp-fecha">
                    </div>
                    <div class="input-group">
                        <label>Tiempo de Entrega</label>
                        <input type="text" id="inp-entrega" placeholder="Ej: 4-6 meses" value="3 meses">
                    </div>
                </div>
            </div>

            <div class="card p-6">
                <label class="border-b pb-2 mb-4">Costos y Beneficios</label>
                
                <div class="input-group">
                    <label>Costo de Fabricación ($)</label>
                    <input type="number" id="costo-bajoquinto" value="15000" oninput="calcular()">
                </div>

                <div class="mb-6 p-4 bg-slate-50 border border-slate-200 rounded-2xl flex items-center justify-between">
                    <div>
                        <span class="text-[10px] font-black text-slate-800 uppercase">Habilitar 12 MSI</span>
                        <p class="text-[9px] text-slate-500 italic">Estrategia Clip</p>
                    </div>
                    <label class="relative inline-flex items-center cursor-pointer mb-0">
                        <input type="checkbox" id="check-meses" class="sr-only peer" onchange="calcular()">
                        <div class="w-10 h-5 bg-slate-200 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-slate-300 after:border after:rounded-full after:h-4 after:w-4 after:transition-all peer-checked:bg-black"></div>
                    </label>
                </div>

                <div class="space-y-4">
                    <div class="space-y-1">
                        <span class="text-[9px] font-black text-slate-400 uppercase tracking-widest">Estuche ($2,200)</span>
                        <div class="toggle-pill">
                            <button id="estuche-off" class="selected" onclick="setOpt('estuche', 'off')">No</button>
                            <button id="estuche-gratis" class="inactive" onclick="setOpt('estuche', 'gratis')">Gratis</button>
                            <button id="estuche-cobrado" class="inactive" onclick="setOpt('estuche', 'Cobrar')">Cobrar</button>
                        </div>
                    </div>

                    <div class="space-y-1">
                        <span class="text-[9px] font-black text-slate-400 uppercase tracking-widest">Pastilla ($4,000)</span>
                        <div class="toggle-pill">
                            <button id="pastilla-off" class="selected" onclick="setOpt('pastilla', 'off')">No</button>
                            <button id="pastilla-gratis" class="inactive" onclick="setOpt('pastilla', 'gratis')">Gratis</button>
                            <button id="pastilla-cobrado" class="inactive" onclick="setOpt('pastilla', 'cobrado')">Cobrar</button>
                        </div>
                    </div>

                    <div class="space-y-1">
                        <span class="text-[9px] font-black text-slate-400 uppercase tracking-widest">Envío ($500)</span>
                        <div class="toggle-pill">
                            <button id="envio-off" class="selected" onclick="setOpt('envio', 'off')">No</button>
                            <button id="envio-gratis" class="inactive" onclick="setOpt('envio', 'gratis')">Gratis</button>
                            <button id="envio-cobrado" class="inactive" onclick="setOpt('envio', 'cobrado')">Cobrar</button>
                        </div>
                    </div>
                </div>

                <div class="mt-6 p-4 bg-black text-white rounded-2xl text-center shadow-lg">
                    <span class="text-[9px] font-black text-slate-400 uppercase block mb-1">Precio sugerido al cliente</span>
                    <span id="resumen-total" class="text-xl font-black">$0.00</span>
                </div>
            </div>
        </div>

        <button class="btn btn-primary mt-8 shadow-xl" onclick="showClientView()">Generar Cotización Formal</button>
    </div>

    <!-- VISTA CLIENTE (COTIZACIÓN FORMAL) -->
    <div id="client-view" class="max-w-4xl mx-auto hidden animate-fade">
        <div class="no-print flex justify-between items-center mb-8 px-4">
            <button class="text-slate-400 text-xs font-black uppercase tracking-widest hover:text-black transition-all" onclick="hideClientView()">← Volver</button>
            <button class="bg-black text-white px-8 py-3 rounded-full text-xs font-black uppercase tracking-widest hover:scale-105 active:scale-95 transition-all shadow-xl flex items-center gap-2" onclick="imprimirPDF()">
                <span>🖨️</span> Imprimir / Guardar PDF
            </button>
        </div>

        <div class="card p-10 md:p-16 border-t-[16px] border-black shadow-2xl relative overflow-hidden bg-white" id="print-area">
            <!-- Header Propuesta -->
            <div class="flex flex-col md:flex-row justify-between gap-12 mb-12">
                <div class="max-w-sm">
                    <div class="mb-8">
                        <h1 class="text-logo text-3xl text-black">Ayala Music</h1>
                    </div>
                    <h2 class="text-4xl font-black text-black tracking-tighter leading-[0.9] mb-4 uppercase">Cotización de<br>Bajo Quinto</h2>
                    <p class="text-[10px] text-slate-400 font-bold uppercase tracking-[0.3em]" id="cli-fecha-display"></p>
                </div>
                
                <div class="text-left md:text-right flex-1">
                    <p class="text-[10px] text-slate-400 font-black uppercase tracking-widest mb-1">Inversión Total</p>
                    <p id="cli-total" class="text-6xl font-black text-black tracking-tighter mb-4">$0.00</p>
                    <div id="cli-badges" class="flex flex-wrap gap-2 justify-start md:justify-end">
                        <!-- Badges -->
                    </div>
                </div>
            </div>

            <div class="grid md:grid-cols-2 gap-12 mb-12 border-y border-slate-100 py-10">
                <!-- Ficha Técnica Visual -->
                <div>
                    <h3 class="text-xs font-black text-black uppercase tracking-widest mb-6 flex items-center gap-2">
                        <span class="w-2 h-2 bg-black rounded-full"></span> Detalles del Instrumento
                    </h3>
                    <div class="space-y-4">
                        <div>
                            <p class="text-[9px] text-slate-400 font-black uppercase">Modelo</p>
                            <p id="cli-modelo-display" class="font-bold text-slate-800"></p>
                        </div>
                        <div>
                            <p class="text-[9px] text-slate-400 font-black uppercase">Maderas Seleccionadas</p>
                            <p id="cli-maderas-display" class="text-sm text-slate-600 leading-relaxed"></p>
                        </div>
                        <div>
                            <p class="text-[9px] text-slate-400 font-black uppercase">Especificaciones</p>
                            <p id="cli-specs-display" class="text-sm text-slate-600 italic leading-relaxed"></p>
                        </div>
                    </div>
                </div>

                <!-- Detalles Entrega -->
                <div class="bg-slate-50 p-8 rounded-3xl border border-slate-100">
                    <h3 class="text-xs font-black text-black uppercase tracking-widest mb-6 flex items-center gap-2">
                        <span class="w-2 h-2 bg-black rounded-full"></span> Términos de Fabricación
                    </h3>
                    <div class="space-y-6">
                        <div class="flex justify-between items-center border-b border-slate-200 pb-4">
                            <span class="text-xs font-bold text-slate-500">Tiempo de Entrega:</span>
                            <span id="cli-entrega-display" class="text-xs font-black text-black uppercase"></span>
                        </div>
                        <div class="flex justify-between items-center border-b border-slate-200 pb-4">
                            <span class="text-xs font-bold text-slate-500">Garantía:</span>
                            <span class="text-xs font-black text-black uppercase">10 Semanas*</span>
                        </div>
                        <p class="text-[9px] text-slate-400 italic leading-tight">
                            *Garantía de 10 semanas contra defectos de fabricación. Aplican restricciones según el uso del instrumento. Precios sujetos a cambios sin previo aviso antes del anticipo inicial.
                        </p>
                    </div>
                </div>
            </div>

            <!-- Beneficios -->
            <div id="seccion-promociones" class="hidden">
                <div class="bg-black text-white text-[10px] font-black py-3 px-6 rounded-t-2xl text-center uppercase tracking-[0.2em]">
                    Beneficio Exclusivo Incluido
                </div>
                <div class="grid md:grid-cols-2 gap-0 border-2 border-black rounded-b-2xl overflow-hidden">
                    <div class="p-8 bg-white border-b-2 md:border-b-0 md:border-r-2 border-slate-100 flex flex-col items-center text-center">
                        <span class="text-3xl mb-3">🎁</span>
                        <h4 class="font-black text-black uppercase tracking-tighter">Estuche de Regalo</h4>
                        <p class="text-[10px] text-slate-500 mt-2">Protección rígida de alta resistencia incluida en su pago de contado.</p>
                    </div>
                    <div class="p-8 bg-slate-50 flex flex-col items-center text-center">
                        <span class="text-3xl mb-3">💳</span>
                        <h4 class="font-black text-black uppercase tracking-tighter">12 Meses Sin Int.</h4>
                        <p class="text-[10px] text-slate-500 mt-2">Difiere tu inversión de manera sencilla con cualquier tarjeta de crédito.</p>
                    </div>
                </div>
            </div>

            <div id="seccion-simple" class="hidden text-center p-8 border-2 border-dashed border-slate-200 rounded-3xl">
                <p class="text-slate-500 font-bold italic text-sm">Este proyecto incluye estuche rígido artesanal y ajuste profesional Ayala.</p>
            </div>

            <div class="mt-16 text-center pt-8 border-t border-slate-50">
                <p class="text-[10px] text-slate-300 font-black uppercase tracking-[0.8em]">Ayala Music — El Sonido del Maestro</p>
            </div>
        </div>
    </div>

    <script>
        // Inicializar fecha
        document.getElementById('inp-fecha').valueAsDate = new Date();

        let opts = {
            estuche: 'off',
            pastilla: 'off',
            envio: 'off'
        };

        function setOpt(key, val) {
            opts[key] = val;
            ['off', 'gratis', 'cobrado'].forEach(s => {
                const btn = document.getElementById(`${key}-${s}`);
                if (s === val) {
                    btn.classList.replace('inactive', 'selected');
                } else {
                    btn.classList.replace('selected', 'inactive');
                }
            });
            calcular();
        }

        function formatMoney(amount) {
            return new Intl.NumberFormat('es-MX', { style: 'currency', currency: 'MXN' }).format(amount);
        }

        function calcular() {
            const costo = parseFloat(document.getElementById('costo-bajoquinto').value) || 0;
            const tieneMeses = document.getElementById('check-meses').checked;

            let total = Math.round((costo * 1.30) / 100) * 100;

            if (opts.estuche === 'cobrado') total += 2200;
            if (opts.pastilla === 'cobrado') total += 4000;
            if (opts.envio === 'cobrado') total += 500;

            if (tieneMeses) {
                const comisionClip = (0.1277 + 0.036) * 1.16; 
                total = Math.ceil((total / (1 - comisionClip)) / 100) * 100;
            }

            document.getElementById('resumen-total').innerText = formatMoney(total);
            return total;
        }

        function showClientView() {
            const total = calcular();
            const tieneMeses = document.getElementById('check-meses').checked;
            
            document.getElementById('cli-modelo-display').innerText = document.getElementById('inp-modelo').value;
            document.getElementById('cli-maderas-display').innerText = document.getElementById('inp-maderas').value;
            document.getElementById('cli-specs-display').innerText = document.getElementById('inp-specs').value;
            document.getElementById('cli-entrega-display').innerText = document.getElementById('inp-entrega').value;
            
            const rawDate = new Date(document.getElementById('inp-fecha').value);
            const options = { year: 'numeric', month: 'long', day: 'numeric' };
            document.getElementById('cli-fecha-display').innerText = rawDate.toLocaleDateString('es-MX', options);

            document.getElementById('cli-total').innerText = formatMoney(total);
            
            const badgeContainer = document.getElementById('cli-badges');
            badgeContainer.innerHTML = '';

            const items = [
                { key: 'envio', label: 'Envío Asegurado' },
                { key: 'pastilla', label: 'Pastilla Pro' },
                { key: 'estuche', label: 'Estuche Rígido' }
            ];

            items.forEach(item => {
                const val = opts[item.key];
                if (val !== 'off') {
                    const badge = document.createElement('span');
                    const isGratis = val === 'gratis';
                    badge.className = `px-4 py-1.5 rounded-full text-[8px] font-black uppercase tracking-wider border-2 ${
                        isGratis ? 'border-emerald-100 bg-emerald-50 text-emerald-700' : 'border-slate-100 bg-white text-slate-500'
                    }`;
                    badge.innerText = isGratis ? `${item.label} (Regalo)` : `${item.label} (Incluido)`;
                    badgeContainer.appendChild(badge);
                }
            });

            if (tieneMeses) {
                document.getElementById('seccion-promociones').classList.remove('hidden');
                document.getElementById('seccion-simple').classList.add('hidden');
            } else {
                document.getElementById('seccion-promociones').classList.add('hidden');
                document.getElementById('seccion-simple').classList.remove('hidden');
            }

            document.getElementById('calculator-view').classList.add('hidden');
            document.getElementById('client-view').classList.remove('hidden');
            window.scrollTo(0,0);
        }

        function hideClientView() {
            document.getElementById('client-view').classList.add('hidden');
            document.getElementById('calculator-view').classList.remove('hidden');
        }

        function imprimirPDF() {
            window.focus();
            setTimeout(() => {
                window.print();
            }, 250);
        }

        calcular();
    </script>
</body>
</html>
