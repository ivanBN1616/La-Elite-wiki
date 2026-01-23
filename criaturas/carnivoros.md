---
title: Carnívoros 
description: 
published: true
date: 2026-01-23T01:18:22.746Z
tags: 
editor: markdown
dateCreated: 2026-01-23T01:18:22.746Z
---


# 🦖 Enciclopedia de Carnívoros - La Élite

Utiliza el panel lateral para filtrar las criaturas por Tier o Nombre. La información técnica se sincroniza con la base de datos oficial de ARK: Survival Ascended.

<html>
<div class="wiki-main-container">
    <div class="nav-panel">
        <div class="search-area">
            <input type="text" id="dinoSearch" onkeyup="filterList()" placeholder="🔍 Buscar dino o tier (S, A, B)...">
        </div>
        
        <div class="dino-list" id="dinoList">
            <button class="dino-item t-s" onclick="updateView('Giganotosaurus', 'S', 'Daño Melee (Top)')">
                <span class="t-indicator">S</span> <strong>Giganotosaurus</strong>
            </button>
            <button class="dino-item t-s" onclick="updateView('Carcharodontosaurus', 'S', 'Daño / Velocidad')">
                <span class="t-indicator">S</span> <strong>Carcharo</strong>
            </button>
            
            <button class="dino-item t-a" onclick="updateView('Rex', 'A', 'Vida / Daño Melee')">
                <span class="t-indicator">A</span> <strong>Tyrannosaurus</strong>
            </button>
            <button class="dino-item t-a" onclick="updateView('Yutyrannus', 'A', 'Stamina / Vida')">
                <span class="t-indicator">A</span> <strong>Yutyrannus</strong>
            </button>
            <button class="dino-item t-a" onclick="updateView('Therizinosaur', 'A', 'Daño / Recolección')">
                <span class="t-indicator">A</span> <strong>Therizino</strong>
            </button>

            <button class="dino-item t-b" onclick="updateView('Spinosaur', 'B', 'Daño (Bonus Agua)')">
                <span class="t-indicator">B</span> <strong>Spinosaurus</strong>
            </button>
            <button class="dino-item t-b" onclick="updateView('Thylacoleo', 'B', 'Daño Sangrado')">
                <span class="t-indicator">B</span> <strong>Thylacoleo</strong>
            </button>
            <button class="dino-item t-b" onclick="updateView('Baryonyx', 'B', 'Stamina / Agilidad')">
                <span class="t-indicator">B</span> <strong>Baryonyx</strong>
            </button>
        </div>
    </div>

    <div class="view-panel">
        <div class="info-header">
            <div id="dinoTitle">🦖 Selecciona una criatura</div>
            <div id="tierLabel" class="tier-tag">---</div>
            <div id="breedingFocus" class="focus-text"></div>
        </div>
        <iframe id="arkFrame" src="about:blank"></iframe>
    </div>
</div>

<style>
    .wiki-main-container {
        display: flex;
        height: 750px;
        background: #111;
        border: 2px solid #333;
        border-radius: 10px;
        overflow: hidden;
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }

    /* Navegación */
    .nav-panel {
        width: 280px;
        background: #1a1a1a;
        display: flex;
        flex-direction: column;
        border-right: 2px solid #333;
    }
    .search-area { padding: 15px; border-bottom: 1px solid #333; }
    #dinoSearch {
        width: 100%;
        padding: 10px;
        background: #000;
        border: 1px solid #ff4d4d;
        color: #fff;
        border-radius: 5px;
    }

    .dino-list { flex: 1; overflow-y: auto; padding: 10px; }
    .dino-item {
        width: 100%;
        display: flex;
        align-items: center;
        padding: 12px;
        margin-bottom: 6px;
        background: #252525;
        border: 1px solid #333;
        border-radius: 5px;
        color: #fff;
        cursor: pointer;
        transition: 0.2s;
    }
    .dino-item:hover { background: #333; border-color: #ff4d4d; transform: translateX(5px); }
    
    .t-indicator {
        width: 25px;
        height: 25px;
        display: flex;
        align-items: center;
        justify-content: center;
        border-radius: 3px;
        margin-right: 12px;
        font-weight: bold;
        font-size: 0.8em;
    }

    /* Tiers Colors */
    .t-s .t-indicator { background: #ff4d4d; color: #fff; box-shadow: 0 0 8px rgba(255,77,77,0.4); }
    .t-a .t-indicator { background: #ffa64d; color: #fff; }
    .t-b .t-indicator { background: #ffff4d; color: #000; }

    /* Visualizador */
    .view-panel { flex: 1; display: flex; flex-direction: column; background: #fff; }
    .info-header {
        background: #1a1a1a;
        color: #fff;
        padding: 15px 20px;
        display: flex;
        align-items: center;
        border-bottom: 3px solid #ff4d4d;
    }
    #dinoTitle { font-size: 1.3em; font-weight: bold; }
    .tier-tag {
        margin-left: 15px;
        padding: 3px 10px;
        border-radius: 4px;
        font-size: 0.8em;
        font-weight: bold;
        text-transform: uppercase;
    }
    .focus-text { margin-left: auto; font-size: 0.85em; color: #aaa; }

    #arkFrame { flex: 1; border: none; width: 100%; height: 100%; }
</style>

<script>
    function updateView(wikiId, tier, focus) {
        const frame = document.getElementById('arkFrame');
        const title = document.getElementById('dinoTitle');
        const tag = document.getElementById('tierLabel');
        const breeding = document.getElementById('breedingFocus');

        // Actualizar Contenido
        frame.src = "https://ark.wiki.gg/wiki/" + wikiId + "?useskin=fandom";
        title.innerText = "🦖 " + wikiId;
        tag.innerText = "Tier " + tier;
        breeding.innerText = "⭐ Foco: " + focus;

        // Estilo del Tag
        const colors = { 'S': '#ff4d4d', 'A': '#ffa64d', 'B': '#ffff4d' };
        tag.style.background = colors[tier] || '#444';
        tag.style.color = (tier === 'B') ? '#000' : '#fff';
    }

    function filterList() {
        let input = document.getElementById('dinoSearch').value.toLowerCase();
        let items = document.getElementsByClassName('dino-item');
        for (let i = 0; i < items.length; i++) {
            items[i].style.display = items[i].innerText.toLowerCase().includes(input) ? "flex" : "none";
        }
    }

    // Carga inicial automática
    setTimeout(() => {
        updateView('Giganotosaurus', 'S', 'Daño Melee (Top)');
    }, 500);
</script>
</html>
