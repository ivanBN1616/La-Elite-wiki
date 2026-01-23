---
title: Hervivoros 
description: 
published: true
date: 2026-01-23T01:36:58.397Z
tags: 
editor: markdown
dateCreated: 2026-01-23T01:36:58.397Z
---

# 🍃 Enciclopedia de Herbívoros - La Élite

Directorio especializado en criaturas de recolección y tanques de asedio. Datos sincronizados con la fuente oficial de ARK ASA.

<html>
<div class="wiki-main-container">
    <div class="nav-panel">
        <div class="search-area">
            <input type="text" id="dinoSearch" onkeyup="filterList()" placeholder="🔍 Buscar herbívoro o tier...">
        </div>
        
        <div class="dino-list" id="dinoList">
            <button class="dino-item t-s" onclick="updateView('Stegosaurus', 'S', '❤️ Vida (Tanqueo)')">
                <span class="t-indicator">S</span> <strong>Stegosaurus</strong>
            </button>
            <button class="dino-item t-s" onclick="updateView('Therizinosaur', 'S', '⚔️ Daño / Recolección')">
                <span class="t-indicator">S</span> <strong>Therizino</strong>
            </button>
            <button class="dino-item t-s" onclick="updateView('Ankylosaurus', 'S', '⚔️ Daño (Metal)')">
                <span class="t-indicator">S</span> <strong>Ankylosaurus</strong>
            </button>
            
            <button class="dino-item t-a" onclick="updateView('Doedicurus', 'A', '⚖️ Peso / ❤️ Vida')">
                <span class="t-indicator">A</span> <strong>Doedicurus</strong>
            </button>
            <button class="dino-item t-a" onclick="updateView('Mammoth', 'A', '⚡ Stamina / Madera')">
                <span class="t-indicator">A</span> <strong>Mamut</strong>
            </button>
            <button class="dino-item t-a" onclick="updateView('Brontosaurus', 'A', '❤️ Vida / Base Móvil')">
                <span class="t-indicator">A</span> <strong>Brontosaurus</strong>
            </button>

            <button class="dino-item t-b" onclick="updateView('Megatherium', 'B', '⚔️ Daño (Bichos)')">
                <span class="t-indicator">B</span> <strong>Megatherium</strong>
            </button>
            <button class="dino-item t-c" onclick="updateView('Castoroides', 'C', '⚖️ Peso (Madera)')">
                <span class="t-indicator">C</span> <strong>Castoroides</strong>
            </button>
        </div>
    </div>

    <div class="view-panel">
        <div class="info-header">
            <div id="dinoTitle">🍃 Selecciona una criatura</div>
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
        background: #0a0a0a;
        border: 2px solid #2a4d2a;
        border-radius: 10px;
        overflow: hidden;
        font-family: sans-serif;
    }

    .nav-panel { width: 280px; background: #111; display: flex; flex-direction: column; border-right: 2px solid #2a4d2a; }
    .search-area { padding: 15px; border-bottom: 1px solid #222; }
    #dinoSearch { width: 100%; padding: 10px; background: #000; border: 1px solid #4dff88; color: #fff; border-radius: 5px; }

    .dino-list { flex: 1; overflow-y: auto; padding: 10px; }
    .dino-item {
        width: 100%; display: flex; align-items: center; padding: 12px; margin-bottom: 6px;
        background: #1a1a1a; border: 1px solid #333; border-radius: 5px; color: #fff; cursor: pointer; transition: 0.2s;
    }
    .dino-item:hover { background: #1d2a1d; border-color: #4dff88; transform: translateX(5px); }
    
    .t-indicator {
        width: 25px; height: 25px; display: flex; align-items: center; justify-content: center;
        border-radius: 3px; margin-right: 12px; font-weight: bold; font-size: 0.8em;
    }

    /* Tiers Colors Herbívoros */
    .t-s .t-indicator { background: #4dff88; color: #000; }
    .t-a .t-indicator { background: #a6ff4d; color: #000; }
    .t-b .t-indicator { background: #ffff4d; color: #000; }
    .t-c .t-indicator { background: #888; color: #fff; }

    .view-panel { flex: 1; display: flex; flex-direction: column; background: #fff; }
    .info-header { background: #111; color: #fff; padding: 15px 20px; display: flex; align-items: center; border-bottom: 3px solid #4dff88; }
    #dinoTitle { font-size: 1.3em; font-weight: bold; }
    .tier-tag { margin-left: 15px; padding: 3px 10px; border-radius: 4px; font-size: 0.8em; font-weight: bold; text-transform: uppercase; }
    .focus-text { margin-left: auto; font-size: 0.85em; color: #4dff88; }

    #arkFrame { flex: 1; border: none; width: 100%; height: 100%; }
</style>

<script>
    function updateView(wikiId, tier, focus) {
        const frame = document.getElementById('arkFrame');
        const title = document.getElementById('dinoTitle');
        const tag = document.getElementById('tierLabel');
        const breeding = document.getElementById('breedingFocus');

        frame.src = "https://ark.wiki.gg/wiki/" + wikiId + "?useskin=fandom";
        title.innerText = "🍃 " + wikiId;
        tag.innerText = "Tier " + tier;
        breeding.innerText = "🛡️ Foco Smart Breeding: " + focus;

        const colors = { 'S': '#4dff88', 'A': '#a6ff4d', 'B': '#ffff4d', 'C': '#888' };
        tag.style.background = colors[tier] || '#444';
        tag.style.color = (tier === 'S' || tier === 'A' || tier === 'B') ? '#000' : '#fff';
    }

    function filterList() {
        let input = document.getElementById('dinoSearch').value.toLowerCase();
        let items = document.getElementsByClassName('dino-item');
        for (let i = 0; i < items.length; i++) {
            items[i].style.display = items[i].innerText.toLowerCase().includes(input) ? "flex" : "none";
        }
    }

    setTimeout(() => { updateView('Stegosaurus', 'S', '❤️ Vida (Tanqueo)'); }, 500);
</script>
</html>

