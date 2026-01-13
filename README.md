# Rastreo-Ecocardiografico
Calculadora para rastreo ecocardiográfico
[Calculadora Monte Prototipo.html](https://github.com/user-attachments/files/24577617/Calculadora.Monte.Prototipo.html)
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculadora Monte - Cardiología</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    
    <style>
        /* ================= ESTILO MODO OSCURO (PERMANENTE) ================= */
        :root {
            --bg-color: #121212;
            --card-bg: #1e1e1e;
            --text-color: #e0e0e0;
            --primary: #4dabf7;  /* Azul claro para contraste */
            --accent: #22b8cf;   /* Cyan para detalles */
            --border-color: #333333;
            --input-bg: #2c2c2c;
            --shadow: 0 4px 20px rgba(0,0,0,0.5);
            
            /* Colores de Semáforo (Brillantes para fondo oscuro) */
            --c-red: #ff6b6b;
            --c-green: #51cf66;
            --c-orange: #fcc419;
        }

        body { 
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; 
            background-color: var(--bg-color); 
            color: var(--text-color); 
            margin: 0; 
            padding: 10px; 
        }
        
        .container { 
            max-width: 1000px; 
            margin: 0 auto; 
            background: var(--card-bg); 
            padding: 30px; 
            border-radius: 15px; 
            box-shadow: var(--shadow); 
        }

        /* HEADER Y LOGO */
        .header-container { text-align: center; margin-bottom: 30px; border-bottom: 2px solid var(--primary); padding-bottom: 20px; }
        .logo-area { margin-bottom: 15px; }
        .logo-img { max-height: 90px; width: auto; filter: drop-shadow(0 0 5px rgba(255,255,255,0.2)); } /* Sombra para resaltar logo */
        
        h1 { color: var(--primary); margin: 0; font-size: 1.8rem; text-transform: uppercase; letter-spacing: 1px; text-shadow: 0 2px 4px rgba(0,0,0,0.5); }
        h3 { color: var(--text-color); margin: 5px 0; font-weight: 500; font-size: 1.1rem; opacity: 0.9; }
        h4 { color: var(--accent); margin: 5px 0 0 0; font-weight: 700; font-size: 1rem; }

        /* SECCIONES */
        .input-section { 
            background: #252525; 
            border: 1px solid var(--border-color); 
            border-radius: 10px; 
            padding: 20px; 
            margin-bottom: 20px; 
            border-left: 4px solid var(--accent); 
        }
        .section-header { 
            font-weight: 700; 
            color: var(--primary); 
            text-transform: uppercase; 
            margin-bottom: 15px; 
            border-bottom: 1px solid var(--border-color); 
            padding-bottom: 5px; 
            font-size: 0.95rem; 
        }

        /* GRID E INPUTS */
        .grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: 20px; }
        .form-group { margin-bottom: 10px; }
        label { display: block; font-size: 0.85rem; font-weight: 600; margin-bottom: 5px; color: #b0b0b0; }
        
        input, select { 
            width: 100%; 
            padding: 12px; 
            border: 1px solid var(--border-color); 
            background-color: var(--input-bg);
            color: #ffffff;
            border-radius: 6px; 
            font-size: 1rem; 
            box-sizing: border-box; 
        }
        input:focus, select:focus { 
            outline: none; 
            border-color: var(--accent); 
            background-color: #383838;
            box-shadow: 0 0 8px rgba(34, 184, 207, 0.3); 
        }

        /* FEEDBACK */
        .feedback { font-size: 0.8rem; margin-top: 5px; font-weight: 700; min-height: 1.2rem; }
        .fb-high { color: var(--c-red); }
        .fb-low { color: var(--c-orange); }
        .fb-normal { color: var(--c-green); }

        /* BOTON */
        button { 
            display: block; 
            width: 100%; 
            padding: 18px; 
            background: linear-gradient(135deg, #1c7ed6 0%, #0056b3 100%); 
            color: white; 
            border: none; 
            border-radius: 8px; 
            font-size: 1.2rem; 
            font-weight: bold; 
            cursor: pointer; 
            margin-top: 30px; 
            box-shadow: 0 4px 15px rgba(0,0,0,0.3);
            transition: transform 0.2s, box-shadow 0.2s; 
        }
        button:hover { transform: translateY(-2px); box-shadow: 0 6px 20px rgba(0,0,0,0.5); filter: brightness(110%); }

        /* REPORTE */
        #reporte { display: none; margin-top: 40px; border-top: 3px solid var(--primary); padding-top: 20px; }
        .report-section { margin-bottom: 25px; }
        .report-title { 
            background: rgba(77, 171, 247, 0.15); 
            color: var(--primary); 
            padding: 10px 15px; 
            font-weight: bold; 
            font-size: 0.95rem; 
            border-left: 4px solid var(--primary);
            border-radius: 0 5px 5px 0;
            margin-bottom: 15px; 
        }
        .result-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; }
        .result-card { 
            background: #2a2a2a; 
            padding: 15px; 
            border-radius: 8px; 
            border-left: 4px solid #555; 
            box-shadow: 0 2px 8px rgba(0,0,0,0.2);
        }
        .lbl { font-size: 0.8rem; color: #aaa; display: block; margin-bottom: 4px; }
        .val { font-size: 1.3rem; font-weight: bold; color: #fff; margin: 3px 0; display: block; }
        .concl { font-size: 0.9rem; font-weight: 700; display: block; margin-top: 8px; padding-top: 8px; border-top: 1px solid #444; }

        /* Colores Reporte */
        .c-red { color: var(--c-red); border-left-color: var(--c-red); }
        .c-green { color: var(--c-green); border-left-color: var(--c-green); }
        .c-orange { color: var(--c-orange); border-left-color: var(--c-orange); }
        
        /* Utils */
        .hidden { display: none; }
        .sub-group { background: rgba(255,255,255,0.05); padding: 12px; border-radius: 6px; margin-top: 8px; border: 1px dashed #555; }
    </style>
</head>
<body>

<div class="container">
    <div class="header-container">
        <div class="logo-area">
            <img src="logo.png" alt="Logotipo IMSS / Cardiología" class="logo-img" onerror="this.style.display='none'; document.getElementById('fallback-icon').style.display='inline-block';">
            <i id="fallback-icon" class="fas fa-heartbeat fa-3x" style="color:var(--primary); display:none;"></i>
        </div>
        
        <h1>Rastreo Ecocardiográfico</h1>
        <h3>Servicio de Cardiología</h3>
        <h4>Calculadora Monte</h4>
    </div>

    <div class="input-section">
        <div class="section-header"><i class="fas fa-user-md"></i> Datos Generales y Vitales</div>
        <div class="grid">
            <div class="form-group">
                <label>Peso (kg)</label>
                <input type="number" id="peso" oninput="check(this, 0, 300, '', '')">
            </div>
            <div class="form-group">
                <label>Talla (cm)</label>
                <input type="number" id="talla" oninput="check(this, 50, 250, '', '')">
            </div>
            <div class="form-group">
                <label>Presión Art. Sistólica (mmHg)</label>
                <input type="number" id="pas" oninput="check(this, 90, 140, 'Hipotensión', 'Hipertensión')">
                <div class="feedback" id="fb_pas"></div>
            </div>
            <div class="form-group">
                <label>Presión Art. Diastólica (mmHg)</label>
                <input type="number" id="pad" oninput="check(this, 60, 90, 'Hipotensión', 'Hipertensión')">
                <div class="feedback" id="fb_pad"></div>
            </div>
            <div class="form-group">
                <label>Frecuencia Cardíaca (lpm)</label>
                <input type="number" id="fc" oninput="check(this, 60, 100, 'Bradicardia', 'Taquicardia')">
                <div class="feedback" id="fb_fc"></div>
            </div>
        </div>
    </div>

    <div class="input-section">
        <div class="section-header"><i class="fas fa-heart"></i> VI: Función Sistólica</div>
        <div class="grid">
            <div class="form-group">
                <label>Diámetro TSVI (cm)</label>
                <input type="number" id="d_tsvi" step="0.01" oninput="check(this, 1.8, 2.2, 'Bajo', 'Dilatado')">
                <div class="feedback" id="fb_d_tsvi"></div>
            </div>
            <div class="form-group">
                <label>IVT (VTI) TSVI (cm)</label>
                <input type="number" id="vti" step="0.1" oninput="check(this, 18, 22, 'Bajo Flujo', 'Alto Flujo')">
                <div class="feedback" id="fb_vti"></div>
            </div>
            <div class="form-group">
                <label>FEVI (%)</label>
                <select id="fevi">
                    <option value="60">Preservada (≥50%)</option>
                    <option value="45">Levemente Reducida (41-49%)</option>
                    <option value="35">Reducida (≤40%)</option>
                </select>
            </div>
        </div>
    </div>

    <div class="input-section">
        <div class="section-header"><i class="fas fa-wave-square"></i> VI: Función Diastólica</div>
        <div class="grid">
            <div class="form-group">
                <label>Onda E Temprana (cm/s)</label>
                <input type="number" id="onda_e" oninput="check(this, 50, 130, 'Baja', 'Alta')">
                <div class="feedback" id="fb_onda_e"></div>
            </div>
            <div class="form-group">
                <label>Onda A Tardía (cm/s)</label>
                <input type="number" id="onda_a">
            </div>
            <div class="form-group">
                <label>Relación e' Medial</label>
                <input type="number" id="e_med" step="0.1">
            </div>
            <div class="form-group">
                <label>Relación e' Lateral</label>
                <input type="number" id="e_lat" step="0.1" oninput="check(this, 0, 13, '', 'Incrementada')">
                <div class="feedback" id="fb_e_lat"></div>
            </div>
            <div class="form-group">
                <label>Tiempo Desaceleración (ms)</label>
                <input type="number" id="td" oninput="checkTD(this)">
                <div class="feedback" id="fb_td"></div>
            </div>
            <div class="form-group">
                <label>TRIVI (IVRT) (ms)</label>
                <input type="number" id="trivi" oninput="check(this, 60, 100, 'Relajación Alt.', 'Normal')">
                <div class="feedback" id="fb_trivi"></div>
            </div>
        </div>
    </div>

    <div class="input-section">
        <div class="section-header"><i class="fas fa-lungs"></i> Ventrículo Derecho</div>
        <div class="grid">
            <div class="form-group">
                <label>TAPSE (mm)</label>
                <input type="number" id="tapse" oninput="check(this, 17, 100, 'Disfunción Long.', 'Normal')">
                <div class="feedback" id="fb_tapse"></div>
            </div>
            <div class="form-group">
                <label>Onda S' Tricúspide (cm/s)</label>
                <input type="number" id="onda_s" oninput="check(this, 9.5, 100, 'Disfunción', 'Normal')">
                <div class="feedback" id="fb_onda_s"></div>
            </div>
            <div class="form-group">
                <label>FAC (%)</label>
                <input type="number" id="fac" oninput="check(this, 35, 100, 'Disfunción Circunf.', 'Normal')">
                <div class="feedback" id="fb_fac"></div>
            </div>
            <div class="form-group">
                <label>Cociente VD/VI</label>
                <input type="number" id="vd_vi" step="0.1" oninput="check(this, 0, 0.6, 'Normal', 'Dilatación VD')">
                <div class="feedback" id="fb_vd_vi"></div>
            </div>
            <div class="form-group">
                <label>Gradiente Regurg. Tricuspídea (mmHg)</label>
                <input type="number" id="grt">
            </div>
        </div>
    </div>

    <div class="input-section">
        <div class="section-header"><i class="fas fa-stethoscope"></i> Otros y Valvulopatías</div>
        <div class="grid">
            <div class="form-group">
                <label>Derrame Pericárdico</label>
                <select id="derrame_peri_opt" onchange="toggleField('peri_mm_group', this.value === 'presente')">
                    <option value="ausente">Ausente</option>
                    <option value="presente">Presente</option>
                </select>
                <div id="peri_mm_group" class="hidden sub-group">
                    <label>Separación (mm)</label>
                    <input type="number" id="peri_mm" placeholder="mm">
                </div>
            </div>
        </div>
        
        <h5 style="margin-bottom:10px; color:var(--primary); margin-top:15px; border-bottom:1px dashed var(--border-color); padding-bottom:5px;">Valvulopatías</h5>
        <div class="grid">
            <div class="form-group">
                <label>Válvula Mitral</label>
                <select id="v_mitral">
                    <option value="Normal">Normal</option>
                    <option value="Insuficiencia">Insuficiencia</option>
                    <option value="Estenosis">Estenosis</option>
                    <option value="Doble Lesión">Doble Lesión</option>
                </select>
            </div>
            <div class="form-group">
                <label>Válvula Aórtica</label>
                <select id="v_aortica">
                    <option value="Normal">Normal</option>
                    <option value="Insuficiencia">Insuficiencia</option>
                    <option value="Estenosis">Estenosis</option>
                    <option value="Doble Lesión">Doble Lesión</option>
                </select>
            </div>
            <div class="form-group">
                <label>Válvula Tricúspide</label>
                <select id="v_tricuspide">
                    <option value="Normal">Normal</option>
                    <option value="Insuficiencia">Insuficiencia</option>
                    <option value="Estenosis">Estenosis</option>
                    <option value="Doble Lesión">Doble Lesión</option>
                </select>
            </div>
            <div class="form-group">
                <label>Válvula Pulmonar</label>
                <select id="v_pulmonar">
                    <option value="Normal">Normal</option>
                    <option value="Insuficiencia">Insuficiencia</option>
                    <option value="Estenosis">Estenosis</option>
                    <option value="Doble Lesión">Doble Lesión</option>
                </select>
            </div>
        </div>
    </div>

    <div class="input-section">
        <div class="section-header"><i class="fas fa-water"></i> Congestión y VExUS</div>
        <div class="grid">
            <div class="form-group">
                <label>Diámetro VCI (mm)</label>
                <input type="number" id="vci" oninput="check(this, 0, 21, 'No dilatada', 'Dilatada')">
                <div class="feedback" id="fb_vci"></div>
            </div>
            <div class="form-group">
                <label>Colapsabilidad / Distensibilidad (%)</label>
                <input type="number" id="colapso" placeholder="%">
            </div>
            <div class="form-group">
                <label>Venas Hepáticas</label>
                <select id="v_hepatica">
                    <option value="0">Normal (S > D)</option>
                    <option value="1">Leve (S < D)</option>
                    <option value="2">Severo (S Invertida)</option>
                </select>
            </div>
            <div class="form-group">
                <label>Vena Porta</label>
                <select id="v_porta">
                    <option value="0">Normal (<30%)</option>
                    <option value="1">Leve (Pulsátil 30-49%)</option>
                    <option value="2">Severo (≥50%)</option>
                </select>
            </div>
            <div class="form-group">
                <label>Venas Renales</label>
                <select id="v_renal">
                    <option value="0">Normal (Continuo)</option>
                    <option value="1">Leve (Bifásico)</option>
                    <option value="2">Severo (Monofásico)</option>
                </select>
            </div>
        </div>
    </div>

    <div class="input-section">
        <div class="section-header"><i class="fas fa-lungs-virus"></i> USG Pulmonar</div>
        <div class="grid">
            <div class="form-group">
                <label>Perfil Pulmonar</label>
                <select id="perfil_pulm" onchange="toggleField('perfil_b_opts', this.value === 'B')">
                    <option value="A">Perfil A (Seco)</option>
                    <option value="B">Perfil B (Edema)</option>
                    <option value="C">Perfil C (Consolidado)</option>
                    <option value="AB">Perfil A/B (Asimétrico)</option>
                </select>
                <div id="perfil_b_opts" class="hidden sub-group">
                    <label>Severidad Líneas B</label>
                    <select id="b_severidad">
                        <option value="Leve (3-5 líneas)">Leve (3-5 líneas)</option>
                        <option value="Moderado (5-10 líneas)">Moderado (5-10 líneas)</option>
                        <option value="Severo (Confluentes)">Severo (Confluentes)</option>
                    </select>
                </div>
            </div>
            <div class="form-group">
                <label>Derrame Pleural</label>
                <select id="derrame_pleura_opt" onchange="toggleField('pleura_mm_group', this.value === 'presente')">
                    <option value="ausente">Ausente</option>
                    <option value="presente">Presente</option>
                </select>
                <div id="pleura_mm_group" class="hidden sub-group">
                    <label>Separación (mm)</label>
                    <input type="number" id="pleura_mm" placeholder="Para fórmula Balik">
                </div>
            </div>
        </div>
    </div>

    <button onclick="generarReporte()"><i class="fas fa-file-medical-alt"></i> GENERAR REPORTE CLÍNICO</button>

    <div id="reporte">
        <h2 style="text-align:center; color:var(--primary); text-transform:uppercase; letter-spacing:2px; margin-bottom:20px;">Resultados del Rastreo</h2>

        <div class="report-section">
            <div class="report-title">DATOS GENERALES Y SIGNOS VITALES</div>
            <div class="result-grid">
                <div class="result-card" id="card_asc"><span class="lbl">ASC (m²)</span><span class="val" id="r_asc">--</span></div>
                <div class="result-card" id="card_bmi"><span class="lbl">IMC</span><span class="val" id="r_bmi">--</span><span class="concl" id="c_bmi">--</span></div>
                <div class="result-card" id="card_pi"><span class="lbl">Peso Ideal (kg)</span><span class="val" id="r_pi">--</span></div>
                <div class="result-card" id="card_pam"><span class="lbl">PAM (mmHg)</span><span class="val" id="r_pam">--</span><span class="concl" id="c_pam">--</span></div>
                <div class="result-card" id="card_pp"><span class="lbl">Presión Pulso</span><span class="val" id="r_pp">--</span></div>
                <div class="result-card" id="card_shock"><span class="lbl">Índice Choque</span><span class="val" id="r_shock">--</span><span class="concl" id="c_shock">--</span></div>
            </div>
        </div>

        <div class="report-section">
            <div class="report-title">VI FUNCIÓN SISTÓLICA</div>
            <div class="result-grid">
                <div class="result-card" id="card_atsvi"><span class="lbl">Área TSVI (cm²)</span><span class="val" id="r_atsvi">--</span></div>
                <div class="result-card" id="card_gc"><span class="lbl">Gasto Cardíaco (L/min)</span><span class="val" id="r_gc">--</span><span class="concl" id="c_gc">--</span></div>
                <div class="result-card" id="card_ic"><span class="lbl">Índice Cardíaco (L/min/m²)</span><span class="val" id="r_ic">--</span><span class="concl" id="c_ic">--</span></div>
                <div class="result-card" id="card_fevi"><span class="lbl">FEVI</span><span class="val" id="r_fevi">--</span></div>
            </div>
            <div id="c_hemo_general" style="margin-top:10px; font-weight:bold; color:#4dabf7; text-align:center; background:rgba(77, 171, 247, 0.1); padding:10px; border-radius:5px; border:1px solid #4dabf7;"></div>
        </div>

        <div class="report-section">
            <div class="report-title">VI FUNCIÓN DIASTÓLICA</div>
            <div class="result-grid">
                <div class="result-card" id="card_e"><span class="lbl">Onda E</span><span class="val" id="r_e">--</span><span class="concl" id="c_e">--</span></div>
                <div class="result-card" id="card_ea"><span class="lbl">Relación E/A</span><span class="val" id="r_ea">--</span><span class="concl" id="c_ea">--</span></div>
                <div class="result-card" id="card_ee"><span class="lbl">E/e' Promedio</span><span class="val" id="r_ee">--</span><span class="concl" id="c_ee">--</span></div>
                <div class="result-card" id="card_pcp"><span class="lbl">PCP (Nagueh)</span><span class="val" id="r_pcp">--</span><span class="concl" id="c_pcp">--</span></div>
                <div class="result-card" id="card_trivi"><span class="lbl">TRIVI</span><span class="val" id="r_trivi">--</span><span class="concl" id="c_trivi">--</span></div>
            </div>
            <div id="c_diastole_final" style="margin-top:10px; font-weight:bold; text-align:center; padding:10px; border:1px solid #444; border-radius:5px;"></div>
        </div>

        <div class="report-section">
            <div class="report-title">VD FUNCIÓN SISTÓLICA</div>
            <div class="result-grid">
                <div class="result-card" id="card_vd_long"><span class="lbl">Función Longitudinal</span><span class="val" id="r_vd_long">--</span></div>
                <div class="result-card" id="card_vd_circ"><span class="lbl">Función Circunferencial</span><span class="val" id="r_vd_circ">--</span></div>
                <div class="result-card" id="card_vd_vi"><span class="lbl">Cociente VD/VI</span><span class="val" id="r_vd_vi">--</span><span class="concl" id="c_vd_vi">--</span></div>
            </div>
        </div>

        <div class="report-section">
            <div class="report-title">OTROS PARÁMETROS Y VExUS</div>
            <div class="result-grid">
                <div class="result-card" id="card_pvc"><span class="lbl">PVC Estimada</span><span class="val" id="r_pvc">--</span></div>
                <div class="result-card" id="card_psap"><span class="lbl">PSAP</span><span class="val" id="r_psap">--</span></div>
                <div class="result-card" id="card_peri"><span class="lbl">Derrame Pericárdico</span><span class="val" id="r_peri">--</span></div>
                <div class="result-card" id="card_valvulas" style="grid-column: span 2;"><span class="lbl">Valvulopatías</span><span class="val" id="r_valvulas" style="font-size:0.95rem; line-height:1.4;">--</span></div>
                <div class="result-card" id="card_vexus"><span class="lbl">VExUS Score</span><span class="val" id="r_vexus" style="font-size:1.4rem;">--</span></div>
            </div>
        </div>

        <div class="report-section">
            <div class="report-title">USG PULMONAR</div>
            <div class="result-grid">
                <div class="result-card"><span class="lbl">Perfil Pulmonar</span><span class="val" id="r_pulmon">--</span></div>
                <div class="result-card"><span class="lbl">Derrame Pleural (Vol. Balik)</span><span class="val" id="r_balik">--</span></div>
            </div>
        </div>

    </div>
</div>

<script>
// === LÓGICA DE FEEDBACK INMEDIATO ===
function check(input, min, max, lowTxt, highTxt) {
    let val = parseFloat(input.value);
    let id = "fb_" + input.id;
    let div = document.getElementById(id);
    if (!div) return;

    if (isNaN(val)) {
        div.innerText = "";
        return;
    }

    if (val < min) {
        div.innerText = lowTxt || "Bajo";
        div.className = "feedback fb-low";
    } else if (val > max) {
        div.innerText = highTxt || "Alto";
        div.className = "feedback fb-high";
    } else {
        div.innerText = "Normal";
        div.className = "feedback fb-normal";
    }
}

function checkTD(input) {
    let val = parseFloat(input.value);
    let div = document.getElementById("fb_td");
    if (!div) return;
    if (isNaN(val)) { div.innerText = ""; return; }
    if (val < 160) {
        div.innerText = "Restrictivo (P. Llenado Elevadas)";
        div.className = "feedback fb-high"; 
    } else if (val > 240) {
        div.innerText = "Relajación Lenta";
        div.className = "feedback fb-low"; 
    } else {
        div.innerText = "Normal";
        div.className = "feedback fb-normal"; 
    }
}

function toggleField(id, show) {
    document.getElementById(id).className = show ? "sub-group" : "hidden sub-group";
}

// === LÓGICA DE CÁLCULO Y REPORTE ===
function generarReporte() {
    const getVal = (id) => parseFloat(document.getElementById(id).value) || 0;
    const setTxt = (id, txt) => document.getElementById(id).innerText = txt;
    const setClass = (id, cls) => document.getElementById(id).className = "result-card " + cls;

    // 1. GENERALES
    let peso = getVal('peso');
    let talla = getVal('talla');
    let pas = getVal('pas');
    let pad = getVal('pad');
    let fc = getVal('fc');

    let asc = Math.sqrt((peso * talla) / 3600);
    let imc = peso / Math.pow(talla/100, 2);
    let p_ideal = 22 * Math.pow(talla/100, 2);
    let pam = pad + (pas - pad)/3;
    let pp = pas - pad;
    let shock = fc / pas;

    setTxt('r_asc', asc.toFixed(2));
    setTxt('r_bmi', imc.toFixed(1));
    setTxt('r_pi', p_ideal.toFixed(1));
    setTxt('r_pam', pam.toFixed(0));
    setTxt('r_pp', pp.toFixed(0));
    setTxt('r_shock', shock.toFixed(2));

    let c_bmi_txt = "";
    if (imc < 18.5) c_bmi_txt = "Bajo Peso";
    else if (imc < 25) c_bmi_txt = "Normal";
    else if (imc < 30) c_bmi_txt = "Sobrepeso";
    else if (imc < 35) c_bmi_txt = "Obesidad G1";
    else if (imc < 40) c_bmi_txt = "Obesidad G2";
    else c_bmi_txt = "Obesidad G3";
    setTxt('c_bmi', c_bmi_txt);

    setTxt('c_pam', pam < 65 ? "Hipotensión" : (pam > 105 ? "Hipertensión" : "Normotensión"));
    setClass('card_pam', pam < 65 ? "c-red" : (pam > 105 ? "c-orange" : "c-green"));

    let shock_conclusion = "Índice Normal";
    let shock_class = "c-green";
    if (shock > 0.7) { shock_conclusion = "Índice Elevado"; shock_class = "c-orange"; }
    if (pas < 90 && shock > 0.7) { shock_conclusion = "PROBABLE CHOQUE"; shock_class = "c-red"; }
    setTxt('c_shock', shock_conclusion);
    setClass('card_shock', shock_class);

    // 2. VI SISTOLICA
    let d_tsvi = getVal('d_tsvi');
    let vti = getVal('vti');
    let fevi_txt = document.getElementById('fevi').options[document.getElementById('fevi').selectedIndex].text;

    let area_tsvi = Math.PI * Math.pow(d_tsvi/2, 2);
    let gc = (area_tsvi * vti * fc) / 1000;
    let ic = gc / asc;

    setTxt('r_atsvi', area_tsvi.toFixed(1));
    setTxt('r_gc', gc.toFixed(1));
    setTxt('r_ic', ic.toFixed(1));
    setTxt('r_fevi', fevi_txt);

    const evalFlow = (val, min, max) => val < min ? "Bajo Gasto" : (val > max ? "Hiperdinámico" : "Gasto Normal");
    let conc_gc = evalFlow(gc, 4, 8);
    let conc_ic = evalFlow(ic, 2.5, 4.0);
    setTxt('c_gc', conc_gc);
    setTxt('c_ic', conc_ic);
    setClass('card_gc', conc_gc === "Gasto Normal" ? "c-green" : (conc_gc === "Bajo Gasto" ? "c-red" : "c-orange"));
    setClass('card_ic', conc_ic === "Gasto Normal" ? "c-green" : (conc_ic === "Bajo Gasto" ? "c-red" : "c-orange"));

    let hemo_txt = "Patrón Indeterminado";
    if (conc_gc.includes("Normal") && conc_ic.includes("Normal")) hemo_txt = "GASTO CARDÍACO NORMAL";
    else if (conc_gc.includes("Bajo") || conc_ic.includes("Bajo")) hemo_txt = "BAJO GASTO CARDÍACO";
    else if (conc_gc.includes("Hiper") || conc_ic.includes("Hiper")) hemo_txt = "ESTADO HIPERDINÁMICO";
    document.getElementById('c_hemo_general').innerText = hemo_txt;

    // 3. VI DIASTOLICA
    let e = getVal('onda_e');
    let a = getVal('onda_a');
    let e_lat = getVal('e_lat');
    let e_med = getVal('e_med');
    let trivi = getVal('trivi');
    
    let ea_ratio = a > 0 ? e/a : 0;
    let ee_avg = (e_med > 0 && e_lat > 0) ? (e_med + e_lat)/2 : (e_med || e_lat || 1);
    let ee_res = e / ee_avg;
    let pcp = (1.24 * ee_res) + 1.9;

    setTxt('r_e', e);
    setTxt('c_e', e < 50 ? "Baja" : (e > 130 ? "Alta" : "Normal"));
    setTxt('r_ea', ea_ratio.toFixed(1));
    let ea_conc = "Normal";
    if (ea_ratio < 0.8) ea_conc = "Patrón Alteración Relajación";
    else if (ea_ratio > 2.0) ea_conc = "Patrón Restrictivo";
    else if (ea_ratio >= 0.8 && ea_ratio <= 2.0) ea_conc = "Normal / Pseudonormal";
    setTxt('c_ea', ea_conc);
    setTxt('r_ee', ee_res.toFixed(1));
    setTxt('c_ee', ee_res > 14 ? "Inc. Presiones Llenado" : "Normal");
    setTxt('r_pcp', pcp.toFixed(1));
    
    let pcp_txt = "Normal"; let pcp_cls = "c-green";
    if (pcp > 25) { pcp_txt = "Edema Alveolar"; pcp_cls = "c-red"; }
    else if (pcp > 18) { pcp_txt = "Edema Intersticial"; pcp_cls = "c-orange"; }
    else if (pcp > 15) { pcp_txt = "Congestión Pulmonar"; pcp_cls = "c-orange"; }
    setTxt('c_pcp', pcp_txt); setClass('card_pcp', pcp_cls);

    setTxt('r_trivi', trivi);
    setTxt('c_trivi', trivi < 60 ? "Tiempo Acortado (P. Altas)" : (trivi > 100 ? "Relajación Lenta" : "Normal"));

    let dias_final = "Indeterminado";
    let presiones_altas = (ee_res > 14 || trivi < 60 || ea_ratio > 2);
    if (ea_ratio < 0.8 && !presiones_altas) dias_final = "Disfunción Diastólica Grado I (Sin elevación de presiones)";
    else if (presiones_altas) dias_final = "Disfunción Diastólica con ELEVACIÓN de Presiones de Llenado";
    else dias_final = "Sin evidencia clara de elevación de presiones de llenado";
    document.getElementById('c_diastole_final').innerText = dias_final;
    document.getElementById('c_diastole_final').style.color = presiones_altas ? "var(--c-red)" : "var(--c-green)";

    // 4. VD
    let tapse = getVal('tapse');
    let s_tri = getVal('onda_s');
    let fac = getVal('fac');
    let vd_vi = getVal('vd_vi');
    let grt = getVal('grt');

    let vd_long_ok = (tapse >= 17 && s_tri >= 9.5);
    setTxt('r_vd_long', vd_long_ok ? "Preservada" : "Reducida");
    setClass('card_vd_long', vd_long_ok ? "c-green" : "c-red");

    let fac_ok = fac >= 35;
    setTxt('r_vd_circ', fac_ok ? "Preservada" : "Reducida");
    setClass('card_vd_circ', fac_ok ? "c-green" : "c-red");

    setTxt('r_vd_vi', vd_vi);
    setTxt('c_vd_vi', vd_vi > 0.6 ? (vd_vi > 1 ? "Dilatación Severa (Riesgo TEP)" : "Dilatación VD") : "Normal");

    // 5. OTROS
    let vci = getVal('vci');
    let colapso = getVal('colapso');
    let pvc = (vci < 21 && colapso > 50) ? 3 : ((vci >= 21 && colapso < 50) ? 15 : 8);
    let psap = grt + pvc;

    setTxt('r_pvc', pvc + " mmHg");
    setTxt('r_psap', psap + " mmHg");

    let peri_opt = document.getElementById('derrame_peri_opt').value;
    let peri_mm = getVal('peri_mm');
    let peri_txt = "Ausente";
    if (peri_opt === 'presente') {
        if (peri_mm < 10) peri_txt = "Leve (<10mm)";
        else if (peri_mm < 20) peri_txt = "Moderado (10-20mm)";
        else peri_txt = "Severo (>20mm)";
    }
    setTxt('r_peri', peri_txt);

    let v_mit = document.getElementById('v_mitral').value;
    let v_aor = document.getElementById('v_aortica').value;
    let v_tri = document.getElementById('v_tricuspide').value;
    let v_pul = document.getElementById('v_pulmonar').value;
    let valvulas_res = "";
    let hay_valvulopatia = false;
    if (v_mit !== "Normal") { valvulas_res += "Mitral: " + v_mit + "<br>"; hay_valvulopatia = true; }
    if (v_aor !== "Normal") { valvulas_res += "Aórtica: " + v_aor + "<br>"; hay_valvulopatia = true; }
    if (v_tri !== "Normal") { valvulas_res += "Tricúspide: " + v_tri + "<br>"; hay_valvulopatia = true; }
    if (v_pul !== "Normal") { valvulas_res += "Pulmonar: " + v_pul + "<br>"; hay_valvulopatia = true; }
    if (!hay_valvulopatia) valvulas_res = "Sin valvulopatías aparentes";
    document.getElementById('r_valvulas').innerHTML = valvulas_res;
    setClass('card_valvulas', hay_valvulopatia ? 'c-orange' : 'c-green');

    // VExUS
    let hep = parseInt(document.getElementById('v_hepatica').value);
    let por = parseInt(document.getElementById('v_porta').value);
    let ren = parseInt(document.getElementById('v_renal').value);
    let score = 0;
    if (vci >= 20) { 
        let sev_count = (hep===2?1:0) + (por===2?1:0) + (ren===2?1:0);
        if (sev_count >= 2) score = 3;
        else if (sev_count === 1) score = 2;
        else score = 1; 
    }
    let v_text = ["Grado 0 (Sin Congestión)", "Grado 1 (Leve)", "Grado 2 (Moderada)", "Grado 3 (Severa)"];
    setTxt('r_vexus', v_text[score]);
    setClass('card_vexus', score >= 2 ? "c-red" : "c-green");

    let p_perfil = document.getElementById('perfil_pulm').value;
    let p_sev = document.getElementById('b_severidad').value;
    let p_txt = p_perfil === "B" ? "Perfil B - " + p_sev : "Perfil " + p_perfil;
    setTxt('r_pulmon', p_txt);

    let pleura_opt = document.getElementById('derrame_pleura_opt').value;
    let pleura_mm = getVal('pleura_mm');
    let balik = pleura_opt === 'presente' ? (pleura_mm * 20) + " ml aprox." : "0 ml";
    setTxt('r_balik', balik);

    document.getElementById('reporte').style.display = 'block';
    document.getElementById('reporte').scrollIntoView({behavior: 'smooth'});
}
</script>

</body>
</html>
