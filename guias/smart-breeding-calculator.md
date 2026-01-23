---
title: Calculadora de breeding
description: 
published: true
date: 2026-01-23T01:55:54.398Z
tags: 
editor: markdown
dateCreated: 2026-01-23T01:55:54.398Z
---

# 🧬 Calculadora Smart Breeding - La Élite

Introduce los stats de tu criatura (recién tameada o recién nacida) para calcular sus **puntos salvajes**. Recuerda: En este servidor, lo que se hereda son los puntos, no el nivel.

---

<html>
<div class="asb-container">
    <div class="asb-config">
        <h3>1. Selecciona Criatura y Stats</h3>
        <label>Criatura:</label>
        <select id="dinoType" onchange="calculatePoints()">
            <option value="rex">T-Rex</option>
            <option value="giga">Giganotosaurus</option>
            <option value="stego">Stegosaurus</option>
            <option value="carcha">Carcharodontosaurus</option>
        </select>

        <div class="stat-input-grid">
            <div class="stat-field">
                <label>Vida (HP):</label>
                <input type="number" id="hp_val" placeholder="Ej: 12000" oninput="calculatePoints()">
                <span id="hp_pts" class="pts-badge">0 pts</span>
            </div>
            <div class="stat-field">
                <label>Energía (Stam):</label>
                <input type="number" id="stam_val" placeholder="Ej: 1500" oninput="calculatePoints()">
                <span id="stam_pts" class="pts-badge">0 pts</span>
            </div>
            <div class="stat-field">
                <label>Daño (Melee %):</label>
                <input type="number" id="melee_val" placeholder="Ej: 450" oninput="calculatePoints()">
                <span id="melee_pts" class="pts-badge">0 pts</span>
            </div>
        </div>
    </div>

    <div class="asb-results">
        <h3>2. Análisis de Potencial</h3>
        <div id="evaluation-box" class="eval-box">
            Esperando datos...
        </div>
        <div class="legend">
            <p><span style="color:#ff4d4d">■</span> 0-30 pts: Basura / Inicial</p>
            <p><span style="color:#ffa64d">■</span> 31-44 pts: Bueno / Aceptable</p>
            <p><span style="color:#4dff88">■</span> 45+ pts: Élite / Para Cría</p>
        </div>
    </div>
</div>

<style>
    .asb-container {
        display: flex;
        gap: 20px;
        background: #111;
        padding: 20px;
        border-radius: 12px;
        border: 1px solid #333;
        color: white;
        font-family: sans-serif;
    }
    .asb-config, .asb-results { flex: 1; }
    
    h3 { color: #ff4d4d; border-bottom: 1px solid #333; padding-bottom: 10px; }
    
    select, input {
        width: 100%;
        padding: 10px;
        margin: 10px 0;
        background: #000;
        border: 1px solid #444;
        color: white;
        border-radius: 5px;
    }

    .stat-input-grid { display: flex; flex-direction: column; gap: 15px; margin-top: 15px; }
    .stat-field { display: flex; align-items: center; gap: 10px; }
    .stat-field label { width: 120px; font-size: 0.9em; }
    
    .pts-badge {
        min-width: 60px;
        padding: 5px;
        background: #222;
        border-radius: 4px;
        text-align: center;
        font-weight: bold;
        font-size: 0.8em;
    }

    .eval-box {
        background: #000;
        padding: 20px;
        border-radius: 8px;
        min-height: 100px;
        display: flex;
        align-items: center;
        justify-content: center;
        text-align: center;
        border: 1px dashed #444;
        font-weight: bold;
        font-size: 1.2em;
    }

    .legend { margin-top: 20px; font-size: 0.8em; color: #888; }
</style>

<script>
    // Valores base simplificados para ASA (Simulados para la demo)
    const baseStats = {
        rex: { hp: 1100, stam: 420, melee: 100, hpInc: 220, stamInc: 42, meleeInc: 5 },
        giga: { hp: 17000, stam: 400, melee: 100, hpInc: 40, stamInc: 2, meleeInc: 4 },
        stego: { hp: 600, stam: 300, melee: 100, hpInc: 120, stamInc: 30, meleeInc: 4 }
    };

    function calculatePoints() {
        const type = document.getElementById('dinoType').value;
        const hpVal = document.getElementById('hp_val').value;
        const stamVal = document.getElementById('stam_val').value;
        const meleeVal = document.getElementById('melee_val').value;
        
        const base = baseStats[type];
        
        // Cálculo matemático inverso: (ValorActual - ValorBase) / IncrementoPorPunto
        let hpPts = Math.floor((hpVal - base.hp) / base.hpInc);
        let stamPts = Math.floor((stamVal - base.stam) / base.stamInc);
        let meleePts = Math.floor((meleeVal - base.melee) / base.meleeInc);

        // Mostrar puntos
        document.getElementById('hp_pts').innerText = (hpPts > 0 ? hpPts : 0) + " pts";
        document.getElementById('stam_pts').innerText = (stamPts > 0 ? stamPts : 0) + " pts";
        document.getElementById('melee_pts').innerText = (meleePts > 0 ? meleePts : 0) + " pts";

        // Evaluación
        const evalBox = document.getElementById('evaluation-box');
        let maxPts = Math.max(hpPts, stamPts, meleePts);

        if (maxPts >= 45) {
            evalBox.innerHTML = "<span style='color:#4dff88'>🔥 ¡ES ÉLITE!<br>Estat de nivel competitivo.</span>";
            evalBox.style.borderColor = "#4dff88";
        } else if (maxPts >= 31) {
            evalBox.innerHTML = "<span style='color:#ffa64d'>👍 BUEN DINO<br>Apto para empezar líneas.</span>";
            evalBox.style.borderColor = "#ffa64d";
        } else if (maxPts > 0) {
            evalBox.innerHTML = "<span style='color:#ff4d4d'>❌ DESECHABLE<br>Stats muy bajos para el meta.</span>";
            evalBox.style.borderColor = "#ff4d4d";
        }
    }
</script>
</html>

---

### ⚠️ Cómo funciona este bloque:
1.  **Ingeniería Inversa:** El script toma el valor que el usuario escribe y resta el valor base del dino de nivel 1. Luego lo divide por lo que gana cada dino por punto salvaje.
2.  **Validación de Meta:** Automáticamente le dice al usuario si el dino sirve para el servidor basándose en los puntos (Tier S = 45+).
3.  **Ligero:** No usa base de datos ni memoria del servidor, todo el cálculo se hace en el móvil/PC del usuario.

**¿Quieres que añada más dinosaurios a la lista del script o que profundicemos en la guía de cómo exportar los archivos `.ini` directamente a esta página?**

