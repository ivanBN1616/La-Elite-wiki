---
title: Importador Genético
description: 
published: true
date: 2026-01-23T05:43:20.447Z
tags: 
editor: markdown
dateCreated: 2026-01-23T05:39:45.719Z
---

# 📥 Importador Genético de La Élite

Arrastra tu archivo `.ini` de exportación (localizado en `ARKSA/Saved/DinoExports`) para obtener el código de tu ficha.

<html>
<div style="background: #111; padding: 20px; border-radius: 10px; border: 1px solid #333; font-family: sans-serif; color: white;">
    
    <div id="drop-area" style="border: 2px dashed #ff4d4d; padding: 30px; text-align: center; border-radius: 10px; cursor: pointer; background: #0a0a0a;">
        <p style="margin: 0; font-size: 1.1em;">📁 Arrastra tu archivo de exportación aquí</p>
        <p style="color: #888; font-size: 0.8em; margin-top: 10px;">O haz clic para buscarlo en tu PC</p>
        <input type="file" id="fileSelector" style="display: none;" accept=".ini">
    </div>

    <div id="preview-section" style="display: none; margin-top: 20px;">
        <h4 style="color: #4dff88; margin-bottom: 10px;">✅ Código Generado:</h4>
        <div style="background: #000; padding: 15px; border-radius: 5px; border: 1px solid #444;">
            <pre id="md-result" style="white-space: pre-wrap; color: #ccc; font-family: monospace; font-size: 0.9em; margin: 0;"></pre>
        </div>
        <button id="copy-btn" style="margin-top: 15px; background: #ff4d4d; color: white; border: none; padding: 10px 20px; border-radius: 5px; cursor: pointer; font-weight: bold;">
            Copiar Código
        </button>
    </div>
</div>

<script>
    const area = document.getElementById('drop-area');
    const selector = document.getElementById('fileSelector');
    const resultBox = document.getElementById('preview-section');
    const resultText = document.getElementById('md-result');
    const copyBtn = document.getElementById('copy-btn');

    area.onclick = () => selector.click();
    selector.onchange = (e) => processFile(e.target.files[0]);

    area.ondragover = (e) => { e.preventDefault(); area.style.backgroundColor = '#1a1111'; };
    area.ondragleave = () => area.style.backgroundColor = '#0a0a0a';
    area.ondrop = (e) => {
        e.preventDefault();
        area.style.backgroundColor = '#0a0a0a';
        processFile(e.dataTransfer.files[0]);
    };

    function processFile(file) {
        if (!file) return;
        const reader = new FileReader();
        reader.onload = (e) => {
            const content = e.target.result;
            const lines = content.split('\n');
            const data = {};
            
            lines.forEach(line => {
                const parts = line.split('=');
                if (parts.length === 2) data[parts[0].trim()] = parts[1].trim();
            });

            // Mapeo de datos del .ini de ASA
            const nombre = data['CharacterName'] || 'Sin Nombre';
            const especie = data['DinoClassName'] || 'Desconocido';
            const lv = data['CharacterLevel'] || '0';
            
            // Construcción del texto MD
            let finalMD = "## 🦖 Ficha de " + nombre + "\n";
            finalMD += "**Especie:** " + especie + " | **Nivel:** " + lv + "\n\n";
            finalMD += "| Stat | Puntos (Wild) |\n";
            finalMD += "| :--- | :--- |\n";
            finalMD += "| ❤️ Vida | " + (data['HealthPoints'] || 0) + " pts |\n";
            finalMD += "| ⚡ Stamina | " + (data['StaminaPoints'] || 0) + " pts |\n";
            finalMD += "| ⚖️ Peso | " + (data['WeightPoints'] || 0) + " pts |\n";
            finalMD += "| ⚔️ Daño | " + (data['MeleeDamagePoints'] || 0) + " pts |\n";
            finalMD += "| 🏃 Vel. | " + (data['SpeedPoints'] || 0) + " pts |\n\n";
            finalMD += "---";

            resultText.innerText = finalMD;
            resultBox.style.display = 'block';
        };
        reader.readAsText(file);
    }

    copyBtn.onclick = () => {
        const text = resultText.innerText;
        navigator.clipboard.writeText(text).then(() => {
            alert("¡Copiado! Ahora pégalo en tu página de usuario.");
        });
    };
</script>
</html>