---
title: Calculadora de crianza
description: 
published: true
date: 2026-01-23T01:59:25.578Z
tags: 
editor: markdown
dateCreated: 2026-01-23T01:59:25.578Z
---

# 🧬 Smart Breeding Online - La Élite (ASA)

Calcula instantáneamente si los stats de tu criatura son aptos para el meta del servidor.

---

<html>
<div class="asb-wrapper">
    <div class="asb-card input-panel">
        <h3 class="panel-title">📊 Entrada de Stats</h3>
        
        <div class="input-group">
            <label>Seleccionar Especie:</label>
            <select id="dinoType" onchange="calculatePoints()">
                <option value="rex">🦖 Tyrannosaurus (Rex)</option>
                <option value="giga">🌋 Giganotosaurus</option>
                <option value="stego">🛡️ Stegosaurus</option>
                <option value="carcha">⚔️ Carcharodontosaurus</option>
                <option value="therizino">🍃 Therizinosaurus</option>
            </select>
        </div>

        <div class="stats-grid">
            <div class="stat-row">
                <div class="stat-info">
                    <strong>Vida (HP)</strong>
                    <input type="number" id="hp_val" placeholder="0" oninput="calculatePoints()">
                </div>
                <div id="hp_pts" class="pts-result">0 pts</div>
            </div>

            <div class="stat-row">
                <div class="stat-info">
                    <strong>Energía (Stamina)</strong>
                    <input type="number" id="stam_val" placeholder="0" oninput="calculatePoints()">
                </div>
                <div id="stam_pts" class="pts-result">0 pts</div>
            </div>

            <div class="stat-row">
                <div class="stat-info">
                    <strong>Daño (Melee %)</strong>
                    <input type="number" id="melee_val" placeholder="100" oninput="calculatePoints()">
                </div>
                <div id="melee_pts" class="pts-result">0 pts</div>
            </div>
        </div>
    </div>

    <div class="asb-card result-panel">
        <h3 class="panel-title">🏆 Veredicto Élite</h3>
        <div id="evaluation-box" class="status-display">
            Introduce datos para analizar
        </div>
        
        <div class="ranking-list">
            <div class="rank-item"><span class="dot red"></span> 0-30: Descartar</div>
            <div class="rank-item"><span class="dot orange"></span> 31-44: Útil para inicio</div>
            <div class="rank-item"><span class="dot green"></span> 45+: Semental Élite</div>
        </div>
    </div>
</div>

<style>
    /* DISEÑO VISUAL DE LA HERRAMIENTA */
    .asb-wrapper {
        display: flex;
        flex-wrap: wrap;
        gap: 20px;
        background: #0a0a0a;
        padding: 20px;
        border-radius: 15px;
        border: 1px solid #333;
        font-family: 'Segoe UI', sans-serif;
    }
    
    .asb-card {
        flex: 1;
        min-width: 300px;
        background: #151518;
        padding: 20px;
        border-radius: 10px;
        border: 1px solid #222;
    }

    .panel-title {
        margin-top: 0;
        font-size: 1.1em;
        color: #ff4d4d;
        border-bottom: 2px solid #333;
        padding-bottom: 10px;
        margin-bottom: 20px;
    }

    .input-group label { display: block; margin-bottom: 8px; font-size: 0.9em; color: #aaa; }
    
    select, input {
        width: 100%;
        padding: 12px;
        background: #000;
        border: 1px solid #444;
        color: #fff;
        border-radius: 6px;
        font-size: 1em;
        outline: none;
    }
    
    input:focus { border-color: #ff4d4d; }

    .stats-grid { margin-top: 20px; }
    
    .stat-row {
        display: flex;
        justify-content: space-between;
        align-items: center;
        background: #1d1d21;
        padding: 10px 15px;
        margin-bottom: 10px;
        border-radius: 8px;
    }

    .stat-info strong { display: block; font-size: 0.8em; color: #ff4d4d; text-transform: uppercase; }
    .stat-info input { width: 120px; padding: 5px; margin-top: 5px; }

    .pts-result {
        font-size: 1.4em;
        font-weight: bold;
        color: #fff;
        text-shadow: 0 0 10px rgba(255,255,255,0.2);
    }

    /* ESTADO RESULTADO */
    .status-display {
        height: 120px;
        display: flex;
        align-items: center;
        justify-content: center;
        text-align: center;
        background: #000;
        border-radius: 8px;
        border: 2px dashed #333;
        font-weight: bold;
        padding: 15px;
        transition: 0.3s;
    }

    .ranking-list { margin-top: 25px; border-top: 1px solid #333; padding-top: 15px; }
    .rank-item { font-size: 0.85em; margin-bottom: 8px; display: flex; align-items: center; }
    .dot { width: 10px; height: 10px; border-radius: 50%; margin-right: 10px; }
    .red { background: #ff4d4d; }
    .orange { background: #ffa64d; }
    .green { background: #4dff88; }
</style>

<script>
    const dataASA = {
        rex: { h: 1100, s: 420, m: 100, hi: 220, si: 42, mi: 5 },
        giga: { h: 17000, s: 400, m: 100, hi: 40, si: 2, mi: 4 },
        stego: { h: 600, s: 300, m: 100, hi: 120, si: 30, mi: 4 },
        carcha: { h: 700, s: 400, m: 100, hi: 140, si: 40, mi: 5 },
        therizino: { h: 870, s: 300, m: 100, hi: 174, si: 30, mi: 5 }
    };

    function calculatePoints() {
        const type = document.getElementById('dinoType').value;
        const hVal = document.getElementById('hp_val').value || 0;
        const sVal = document.getElementById('stam_val').value || 0;
        const mVal = document.getElementById('melee_val').value || 0;
        
        const b = dataASA[type];
        
        let hpP = Math.floor((hVal - b.h) / b.hi);
        let stP = Math.floor((sVal - b.s) / b.si);
        let meP = Math.floor((mVal - b.m) / b.mi);

        document.getElementById('hp_pts').innerText = (hpP > 0 ? hpP : 0) + " pts";
        document.getElementById('stam_pts').innerText = (stP > 0 ? stP : 0) + " pts";
        document.getElementById('melee_pts').innerText = (meP > 0 ? meP : 0) + " pts";

        const box = document.getElementById('evaluation-box');
        let top = Math.max(hpP, stP, meP);

        if (top >= 45) {
            box.innerHTML = "<span style='color:#4dff88'>💎 EJEMPLAR ÉLITE<br>Apto para mutaciones TOP.</span>";
            box.style.borderColor = "#4dff88";
        } else if (top >= 31) {
            box.innerHTML = "<span style='color:#ffa64d'>✅ BUEN NIVEL<br>Base sólida para empezar.</span>";
            box.style.borderColor = "#ffa64d";
        } else {
            box.innerHTML = "<span style='color:#ff4d4d'>❌ STATS BAJOS<br>No recomendado para crianza.</span>";
            box.style.borderColor = "#ff4d4d";
        }
    }
</script>
</html>

