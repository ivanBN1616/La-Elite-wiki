---
title: Importador Genético
description: 
published: true
date: 2026-01-23T05:47:40.987Z
tags: 
editor: markdown
dateCreated: 2026-01-23T05:39:45.719Z
---

```plaintext
<div class="elite-importer" style="background: #111; color: white; padding: 20px; border: 2px solid #333; border-radius: 10px; font-family: sans-serif;">
    <h2 style="color: #ff4d4d; margin-top: 0;">🧬 Importador Genético Élite</h2>
    <p style="font-size: 0.9em; color: #888;">Arrastra el archivo .ini de tu exportación de ASA abajo:</p>
    
    <div id="drop-zone" style="border: 2px dashed #ff4d4d; padding: 40px; text-align: center; border-radius: 8px; background: #000; cursor: pointer; margin-bottom: 20px;">
        <span id="status-text" style="font-weight: bold; color: #ff4d4d;">📁 CLIC O ARRASTRA ARCHIVO .INI</span>
        <input type="file" id="file-input" style="display: none;" accept=".ini">
    </div>

    <div id="output-container" style="display: none;">
        <div style="background: #1a1a1a; padding: 15px; border-radius: 5px; border: 1px solid #444;">
            <label style="display: block; font-size: 0.8em; color: #4dff88; margin-bottom: 5px;">CÓDIGO PARA TU FICHA (COPIAR Y PEGAR):</label>
            <textarea id="md-output" style="width: 100%; height: 200px; background: #000; color: #ccc; border: 1px solid #333; font-family: monospace; padding: 10px; font-size: 13px;"></textarea>
        </div>
        <button id="copy-btn" style="margin-top: 15px; width: 100%; padding: 12px; background: #ff4d4d; border: none; color: white; font-weight: bold; border-radius: 5px; cursor: pointer;">
            📋 COPIAR CÓDIGO GENERADO
        </button>
    </div>
</div>

<script>
    (function() {
        const dropZone = document.getElementById('drop-zone');
        const fileInput = document.getElementById('file-input');
        const outputContainer = document.getElementById('output-container');
        const mdOutput = document.getElementById('md-output');
        const copyBtn = document.getElementById('copy-btn');
        const statusText = document.getElementById('status-text');

        dropZone.addEventListener('click', () => fileInput.click());

        fileInput.addEventListener('change', (e) => handleFile(e.target.files[0]));

        dropZone.addEventListener('dragover', (e) => {
            e.preventDefault();
            dropZone.style.borderColor = '#4dff88';
        });

        dropZone.addEventListener('dragleave', () => {
            dropZone.style.borderColor = '#ff4d4d';
        });

        dropZone.addEventListener('drop', (e) => {
            e.preventDefault();
            handleFile(e.dataTransfer.files[0]);
        });

        function handleFile(file) {
            if (!file) return;
            const reader = new FileReader();
            reader.onload = (e) => {
                const text = e.target.result;
                const lines = text.split('\n');
                const stats = {};
                lines.forEach(l => {
                    const p = l.split('=');
                    if(p.length === 2) stats[p[0].trim()] = p[1].trim();
                });

                const name = stats['CharacterName'] || 'Dino';
                const level = stats['CharacterLevel'] || '0';
                
                let md = "### 🦖 Ficha de " + name + " (Lvl " + level + ")\n\n";
                md += "| Stat | Puntos (Wild) |\n";
                md += "| :--- | :--- |\n";
                md += "| ❤️ Vida | " + (stats['HealthPoints'] || 0) + " pts |\n";
                md += "| ⚡ Stamina | " + (stats['StaminaPoints'] || 0) + " pts |\n";
                md += "| ⚖️ Peso | " + (stats['WeightPoints'] || 0) + " pts |\n";
                md += "| ⚔️ Daño | " + (stats['MeleeDamagePoints'] || 0) + " pts |\n";
                md += "| 🏃 Vel. | " + (stats['SpeedPoints'] || 0) + " pts |\n\n";
                md += "---";

                mdOutput.value = md;
                outputContainer.style.display = 'block';
                statusText.innerText = "✅ ARCHIVO CARGADO";
                statusText.style.color = "#4dff88";
            };
            reader.readAsText(file);
        }

        copyBtn.addEventListener('click', () => {
            mdOutput.select();
            document.execCommand('copy');
            alert('¡Copiado! Pégalo en tu página personal.');
        });
    })();
</script>
```