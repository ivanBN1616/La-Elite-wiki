---
title: Carnívoros
description: 
published: true
date: 2026-01-23T01:15:10.279Z
tags: 
editor: markdown
dateCreated: 2026-01-23T01:15:10.279Z
---

<div class="wiki-container">
    <div class="side-nav">
        <div class="search-box">
            <input type="text" id="dinoInput" onkeyup="searchFunction()" placeholder="Buscar por nombre o tier...">
        </div>
        
        <div class="button-container" id="dinoMenu">
            <button class="dino-card tier-s" onclick="loadWiki('Giganotosaurus', 'S', 'Melee')">
                <span class="badge">S</span> <strong>Giganoto</strong>
            </button>
            <button class="dino-card tier-s" onclick="loadWiki('Carcharodontosaurus', 'S', 'Melee/Speed')">
                <span class="badge">S</span> <strong>Carcha</strong>
            </button>
            <button class="dino-card tier-a" onclick="loadWiki('Rex', 'A', 'Vida/Melee')">
                <span class="badge">A</span> <strong>T-Rex</strong>
            </button>
            <button class="dino-card tier-a" onclick="loadWiki('Yutyrannus', 'A', 'Stamina')">
                <span class="badge">A</span> <strong>Yutyrannus</strong>
            </button>
            <button class="dino-card tier-b" onclick="loadWiki('Spinosaur', 'B', 'Melee/Vida')">
                <span class="badge">B</span> <strong>Spinosaurus</strong>
            </button>
            <button class="dino-card tier-b" onclick="loadWiki('Thylacoleo', 'B', 'Melee')">
                <span class="badge">B</span> <strong>Thylacoleo</strong>
            </button>
            <button class="dino-card tier-c" onclick="loadWiki('Baryonyx', 'C', 'Stamina')">
                <span class="badge">C</span> <strong>Baryonyx</strong>
            </button>
        </div>
    </div>

    <div class="content-view">
        <div class="header-bar">
            <span id="nameDisplay">Selecciona un espécimen</span>
            <span id="tierDisplay" class="t-badge">---</span>
            <small id="statDisplay" style="color: #888; margin-left: 10px;"></small>
        </div>
        <iframe id="wikiFrame" src="about:blank"></iframe>
    </div>
</div>

<style>
    .wiki-container { display: flex; height: 80vh; background: #0d0d0d; border-radius: 8px; overflow: hidden; font-family: sans-serif; }
    
    /* Lateral */
    .side-nav { width: 260px; background: #1a1a1a; display: flex; flex-direction: column; border-right: 2px solid #333; }
    .search-box { padding: 15px; border-bottom: 1px solid #333; }
    #dinoInput { width: 100%; padding: 8px; background: #000; border: 1px solid #ff4d4d; color: #fff; border-radius: 4px; }
    
    .button-container { flex: 1; overflow-y: auto; padding: 10px; }
    .dino-card { 
        width: 100%; background: #222; border: 1px solid #333; color: #fff; 
        padding: 10px; margin-bottom: 8px; text-align: left; cursor: pointer;
        display: flex; align-items: center; border-radius: 4px; transition: 0.2s;
    }
    .dino-card:hover { background: #333; border-color: #ff4d4d; }
    .dino-card .badge { 
        width: 25px; height: 25px; display: flex; align-items: center; justify-content: center;
        border-radius: 3px; margin-right: 10px; font-weight: bold; font-size: 0.8em;
    }
    
    /* Colores Tier */
    .tier-s .badge { background: #ff4d4d; color: #fff; }
    .tier-a .badge { background: #ffa64d; color: #fff; }
    .tier-b .badge { background: #ffff4d; color: #000; }
    .tier-c .badge { background: #4dff88; color: #000; }

    /* Viewer */
    .content-view { flex: 1; display: flex; flex-direction: column; background: #fff; }
    .header-bar { background: #1a1a1a; color: #fff; padding: 12px 20px; border-bottom: 2px solid #ff4d4d; }
    .t-badge { padding: 2px 8px; border-radius: 4px; font-size: 0.7em; margin-left: 10px; font-weight: bold; background: #444; }
    #wikiFrame { flex: 1; border: none; }
</style>

<script>
    function loadWiki(id, tier, stat) {
        const frame = document.getElementById('wikiFrame');
        const nameDisp = document.getElementById('nameDisplay');
        const tierDisp = document.getElementById('tierDisplay');
        const statDisp = document.getElementById('statDisplay');
        
        frame.src = "https://ark.wiki.gg/wiki/" + id + "?useskin=fandom";
        nameDisp.innerText = "🦖 " + id;
        tierDisp.innerText = "TIER " + tier;
        statDisp.innerText = "| Foco: " + stat;
        
        // Color dinámico del badge en la cabecera
        const colors = { 'S': '#ff4d4d', 'A': '#ffa64d', 'B': '#ffff4d', 'C': '#4dff88' };
        tierDisp.style.background = colors[tier];
        tierDisp.style.color = (tier === 'B' || tier === 'C') ? '#000' : '#fff';
    }

    function searchFunction() {
        let input = document.getElementById('dinoInput').value.toLowerCase();
        let cards = document.getElementsByClassName('dino-card');
        for (let i = 0; i < cards.length; i++) {
            cards[i].style.display = cards[i].innerText.toLowerCase().includes(input) ? "flex" : "none";
        }
    }
    
    // Carga inicial
    window.onload = function() {
        loadWiki('Giganotosaurus', 'S', 'Melee');
    };
</script>

