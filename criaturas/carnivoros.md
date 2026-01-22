---
title: Carnívoros
description: 
published: true
date: 2026-01-22T23:31:30.690Z
tags: 
editor: markdown
dateCreated: 2026-01-22T23:31:30.690Z
---

<div class="elite-wiki-container">
    
    <aside class="sidebar">
        <div class="sidebar-header">
            <h3>🥩 Carnívoros</h3>
            <input type="text" id="dinoSearch" placeholder="🔍 Buscar depredador..." onkeyup="filterDinos()">
        </div>
        <div id="button-list" class="button-grid">
            </div>
    </aside>

    <main class="viewer">
        <div id="status-bar">
            <div class="info-main">
                <span id="current-dino">Cargando...</span>
                <span id="current-stat"></span>
            </div>
            <span id="current-tier" class="tier-badge">---</span>
        </div>
        <div class="iframe-wrapper">
            <iframe id="wiki-frame" name="wiki-frame" src="" allowfullscreen></iframe>
        </div>
    </main>
</div>

<style>
/* VARIABLES DE DISEÑO ELITE */
:root {
    --bg-dark: #0a0a0b;
    --bg-card: #141417;
    --accent: #ff4d4d;
    --text: #e0e0e0;
    --border: #2d2d32;
}

.elite-wiki-container {
    display: flex;
    gap: 0;
    height: 85vh;
    background: var(--bg-dark);
    border: 1px solid var(--border);
    border-radius: 12px;
    overflow: hidden;
    color: var(--text);
    font-family: 'Segoe UI', Roboto, sans-serif;
}

/* SIDEBAR */
.sidebar {
    flex: 0 0 300px;
    background: var(--bg-card);
    display: flex;
    flex-direction: column;
    border-right: 1px solid var(--border);
}

.sidebar-header {
    padding: 20px;
    background: rgba(0,0,0,0.2);
}

#dinoSearch {
    width: 100%;
    padding: 10px;
    background: #1e1e22;
    border: 1px solid var(--border);
    color: white;
    border-radius: 6px;
    outline: none;
}

#dinoSearch:focus { border-color: var(--accent); }

.button-grid {
    flex: 1;
    overflow-y: auto;
    padding: 10px;
}

/* BOTONES DINÁMICOS */
.dino-btn {
    width: 100%;
    background: #1e1e22;
    border: 1px solid var(--border);
    border-radius: 8px;
    padding: 12px;
    margin-bottom: 8px;
    color: white;
    cursor: pointer;
    transition: all 0.2s ease;
    text-align: left;
    display: flex;
    align-items: center;
    gap: 12px;
}

.dino-btn:hover { background: #2a2a30; border-color: var(--accent); transform: translateX(5px); }
.dino-btn.active { background: #2d1a1a; border-color: var(--accent); box-shadow: inset 4px 0 0 var(--accent); }

.tier-indicator {
    width: 28px;
    height: 28px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-weight: bold;
    border-radius: 4px;
    font-size: 0.9em;
}

/* COLORES DE TIERS */
.tier-s .tier-indicator { background: #ff4d4d; color: white; }
.tier-a .tier-indicator { background: #ffa64d; color: white; }
.tier-b .tier-indicator { background: #ffff4d; color: black; }
.tier-c .tier-indicator { background: #4dff88; color: black; }

/* VISUALIZADOR */
.viewer {
    flex: 1;
    display: flex;
    flex-direction: column;
    background: #000;
}

#status-bar {
    padding: 15px 25px;
    background: #141417;
    border-bottom: 1px solid var(--border);
    display: flex;
    justify-content: space-between;
    align-items: center;
}

#current-dino { font-size: 1.2em; font-weight: bold; color: var(--accent); }
#current-stat { font-size: 0.85em; color: #888; margin-left: 15px; }

.tier-badge {
    padding: 4px 15px;
    border-radius: 20px;
    font-weight: bold;
    font-size: 0.8em;
    text-transform: uppercase;
}

.iframe-wrapper { flex: 1; width: 100%; background: #fff; }
#wiki-frame { width: 100%; height: 100%; border: none; }

/* Scrollbar Custom */
::-webkit-scrollbar { width: 6px; }
::-webkit-scrollbar-thumb { background: var(--border); border-radius: 10px; }
::-webkit-scrollbar-thumb:hover { background: var(--accent); }
</style>

<script>
// BASE DE DATOS JSON - CENTRALIZADA
const dbCriaturas = [
    { "id": "Giganotosaurus", "nombre": "Giganotosaurus", "stat": "⚔️ Melee (Top Stat)", "tier": "S", "wiki": "Giganotosaurus" },
    { "id": "Carcharodontosaurus", "nombre": "Carcha", "stat": "⚔️ Melee / 💨 Speed", "tier": "S", "wiki": "Carcharodontosaurus" },
    { "id": "Rex", "nombre": "T-Rex", "stat": "❤️ Vida / ⚔️ Melee", "tier": "A", "wiki": "Rex" },
    { "id": "Yutyrannus", "nombre": "Yutyrannus", "stat": "⚡ Stamina (2000+)", "tier": "A", "wiki": "Yutyrannus" },
    { "id": "Therizinosaurus", "nombre": "Therizino", "stat": "⚔️ Melee / ❤️ Vida", "tier": "A", "wiki": "Therizinosaur" },
    { "id": "Spinosaur", "nombre": "Spinosaur", "stat": "⚔️ Melee / ❤️ Vida", "tier": "B", "wiki": "Spinosaur" },
    { "id": "Thylacoleo", "nombre": "Thylacoleo", "stat": "❤️ Vida / ⚔️ Melee", "tier": "B", "wiki": "Thylacoleo" },
    { "id": "Baryonyx", "nombre": "Baryonyx", "stat": "❤️ Vida / ⚡ Stamina", "tier": "C", "wiki": "Baryonyx" }
];

const buttonList = document.getElementById('button-list');
const frame = document.getElementById('wiki-frame');
const labelDino = document.getElementById('current-dino');
const labelStat = document.getElementById('current-stat');
const labelTier = document.getElementById('current-tier');

// Lógica de carga
function loadDino(dino) {
    frame.src = `https://ark.wiki.gg/wiki/${dino.wiki}?useskin=fandom`;
    labelDino.innerText = `🦖 ${dino.nombre}`;
    labelStat.innerText = `Focus: ${dino.stat}`;
    labelTier.innerText = `Tier ${dino.tier}`;
    labelTier.className = `tier-badge tier-${dino.tier.toLowerCase()}`;
}

function render() {
    buttonList.innerHTML = "";
    // Ordenar por importancia (S > A > B)
    const sorted = dbCriaturas.sort((a, b) => a.tier.localeCompare(b.tier) || a.nombre.localeCompare(b.nombre));

    sorted.forEach((dino, index) => {
        const btn = document.createElement('button');
        btn.className = `dino-btn tier-${dino.tier.toLowerCase()}`;
        btn.innerHTML = `
            <span class="tier-indicator">${dino.tier}</span>
            <div>
                <strong>${dino.nombre}</strong><br>
                <small style="font-size:0.7em; opacity:0.6">${dino.stat}</small>
            </div>
        `;

        btn.onclick = () => {
            loadDino(dino);
            document.querySelectorAll('.dino-btn').forEach(b => b.classList.remove('active'));
            btn.classList.add('active');
        };

        buttonList.appendChild(btn);

        // Auto-cargar el primero de la lista al iniciar
        if (index === 0) {
            loadDino(dino);
            btn.classList.add('active');
        }
    });
}

function filterDinos() {
    let input = document.getElementById('dinoSearch').value.toLowerCase();
    let btns = document.getElementsByClassName('dino-btn');
    for (let btn of btns) {
        btn.style.display = btn.innerText.toLowerCase().includes(input) ? "flex" : "none";
    }
}

// Iniciar aplicación
document.addEventListener('DOMContentLoaded', render);
// Fallback por si DOMContentLoaded ya pasó
if (document.readyState === "complete" || document.readyState === "interactive") { render(); }
</script>

