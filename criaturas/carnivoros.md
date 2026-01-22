---
title: Carnívoros 
description: 
published: true
date: 2026-01-22T23:27:18.724Z
tags: 
editor: markdown
dateCreated: 2026-01-22T23:27:18.723Z
---

# 🥩 Directorio de Carnívoros (Elite Tier System)

<div class="wiki-layout">
    <aside class="dino-selector">
        <input type="text" id="dinoSearch" placeholder="🔍 Filtrar por nombre, stat o tier..." onkeyup="filterDinos()">
        <div id="button-list" class="button-grid"></div>
    </aside>

    <main class="wiki-display">
        <div id="status-bar">
            <span id="current-dino">Selecciona una criatura</span> 
            <span id="current-tier" class="tier-badge">---</span>
        </div>
        <iframe id="wiki-frame" src="about:blank"></iframe>
    </main>
</div>

<script>
// JSON DE CRIATURAS CON SISTEMA DE TIERS
const dbCriaturas = [
    { "id": "Giganotosaurus", "nombre": "Giganotosaurus", "stat": "⚔️ Melee (Top)", "tier": "S", "wiki": "Giganotosaurus" },
    { "id": "Carcharodontosaurus", "nombre": "Carcha", "stat": "⚔️ Melee / 💨 Speed", "tier": "S", "wiki": "Carcharodontosaurus" },
    { "id": "Rex", "nombre": "T-Rex", "stat": "❤️ Vida / ⚔️ Melee", "tier": "A", "wiki": "Rex" },
    { "id": "Yutyrannus", "nombre": "Yutyrannus", "stat": "⚡ Stamina (2k+)", "tier": "A", "wiki": "Yutyrannus" },
    { "id": "Spinosaur", "nombre": "Spinosaur", "stat": "⚔️ Melee / ❤️ Vida", "tier": "B", "wiki": "Spinosaur" },
    { "id": "Thylacoleo", "nombre": "Thylacoleo", "stat": "❤️ Vida / ⚔️ Melee", "tier": "B", "wiki": "Thylacoleo" },
    { "id": "Baryonyx", "nombre": "Baryonyx", "stat": "❤️ Vida / ⚡ Stam", "tier": "C", "wiki": "Baryonyx" }
];

const buttonList = document.getElementById('button-list');
const frame = document.getElementById('wiki-frame');
const labelDino = document.getElementById('current-dino');
const labelTier = document.getElementById('current-tier');

function render() {
    buttonList.innerHTML = "";
    // Ordenar por Tier (S -> A -> B...) y luego por nombre
    dbCriaturas.sort((a, b) => a.tier.localeCompare(b.tier) || a.nombre.localeCompare(b.nombre)).forEach(dino => {
        const btn = document.createElement('button');
        btn.className = `dino-btn tier-${dino.tier.toLowerCase()}`;
        btn.innerHTML = `
            <div class="btn-content">
                <span class="tier-indicator">${dino.tier}</span>
                <div>
                    <strong>${dino.nombre}</strong><br>
                    <small>${dino.stat}</small>
                </div>
            </div>`;
        
        btn.onclick = () => {
            frame.src = `https://ark.wiki.gg/wiki/${dino.wiki}?useskin=fandom`;
            labelDino.innerText = `🦖 ${dino.nombre}`;
            labelTier.innerText = `TIER ${dino.tier}`;
            labelTier.className = `tier-badge tier-${dino.tier.toLowerCase()}`;
            
            document.querySelectorAll('.dino-btn').forEach(b => b.classList.remove('active'));
            btn.classList.add('active');
        };
        buttonList.appendChild(btn);
    });
}

function filterDinos() {
    let input = document.getElementById('dinoSearch').value.toLowerCase();
    let btns = document.getElementsByClassName('dino-btn');
    for (let btn of btns) {
        btn.style.display = btn.innerText.toLowerCase().includes(input) ? "block" : "none";
    }
}

render();
</script>

<style>
/* Estructura Base */
.wiki-layout { display: flex; gap: 15px; height: 85vh; background: #0a0a0a; padding: 10px; border-radius: 12px; }
.dino-selector { flex: 1; background: #111; padding: 15px; border-radius: 10px; overflow-y: auto; border: 1px solid #222; min-width: 280px; }
.wiki-display { flex: 4; display: flex; flex-direction: column; background: #000; border-radius: 10px; overflow: hidden; }

/* Buscador */
#dinoSearch { width: 100%; padding: 12px; margin-bottom: 15px; background: #1a1a1a; border: 1px solid #444; color: white; border-radius: 6px; }

/* Botones y Tiers */
.button-grid { display: flex; flex-direction: column; gap: 8px; }
.dino-btn { background: #1a1a1a; color: #fff; border: 1px solid #333; padding: 10px; text-align: left; cursor: pointer; border-radius: 6px; transition: 0.2s; }
.btn-content { display: flex; align-items: center; gap: 12px; }

.tier-indicator { 
    width: 30px; height: 30px; display: flex; align-items: center; justify-content: center; 
    font-weight: bold; border-radius: 4px; font-size: 1.1em;
}

/* Colores de Tiers */
.tier-s .tier-indicator { background: #ff0000; color: white; box-shadow: 0 0 10px #ff000066; }
.tier-a .tier-indicator { background: #ff8000; color: white; }
.tier-b .tier-indicator { background: #ffff00; color: black; }
.tier-c .tier-indicator { background: #00ff00; color: black; }

.dino-btn:hover { transform: translateX(5px); background: #222; }
.dino-btn.active { border-left: 5px solid #fff; background: #252525; }

/* Status Bar */
#status-bar { background: #1a1a1a; color: #fff; padding: 12px 20px; display: flex; justify-content: space-between; align-items: center; border-bottom: 2px solid #333; }
.tier-badge { padding: 4px 12px; border-radius: 20px; font-size: 0.8em; font-weight: bold; }
#wiki-frame { width: 100%; flex-grow: 1; border: none; background: #fff; }
</style>

