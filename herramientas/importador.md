---
title: Importador Genético
description: 
published: true
date: 2026-01-23T05:39:45.720Z
tags: 
editor: markdown
dateCreated: 2026-01-23T05:39:45.719Z
---

# 📥 Importador Genético de La Élite
Arrastra tu archivo de exportación de ASA (.ini) aquí para generar tu ficha de registro automáticamente.

---

<html>
<div class="import-container">
    <div id="drop-zone">
        <p>Arrastra aquí tu archivo de exportación (.ini)</p>
        <span>O haz clic para seleccionar</span>
        <input type="file" id="fileInput" accept=".ini" style="display:none">
    </div>

    <div id="result-area" style="display:none;">
        <h3>✅ Datos Extraídos</h3>
        <textarea id="output-md" readonly></textarea>
        <button onclick="copyToClipboard()">Copiar Formato Markdown</button>
    </div>
</div>

<style>
    .import-container { background: #111; padding: 25px; border-radius: 12px; border: 1px solid #333; color: white; }
    #drop-zone {
        border: 2px dashed #ff4d4d;
        padding: 40px;
        text-align: center;
        cursor: pointer;
        background: #0a0a0a;
        transition: 0.3s;
        border-radius: 8px;
    }
    #drop-zone:hover { background: #1a0a0a; border-color: #ff8888; }
    #result-area { margin-top: 25px; }
    textarea {
        width: 100%;
        height: 300px;
        background: #000;
        color: #4dff88;
        padding: 15px;
        font-family: monospace;
        border: 1px solid #333;
        border-radius: 6px;
        margin-bottom: 15px;
    }
    button {
        background: #ff4d4d;
        color: white;
        border: none;
        padding: 12px 25px;
        border-radius: 6px;
        cursor: pointer;
        font-weight: bold;
    }
    button:hover { background: #ff6666; }
</style>

<script>
    const dropZone = document.getElementById('drop-zone');
    const fileInput = document.getElementById('fileInput');
    const output = document.getElementById('output-md');
    const resultArea = document.getElementById('result-area');

    dropZone.onclick = () => fileInput.click();

    fileInput.onchange = e => handleFile(e.target.files[0]);

    dropZone.ondragover = e => { e.preventDefault(); dropZone.style.borderColor = '#4dff88'; };
    dropZone.ondragleave = () => dropZone.style.borderColor = '#ff4d4d';
    dropZone.ondrop = e => {
        e.preventDefault();
        handleFile(e.dataTransfer.files[0]);
    };

    function handleFile(file) {
        if (!file) return;
        const reader = new FileReader();
        reader.onload = e => parseIni(e.target.result, file.name);
        reader.readAsText(file);
    }

    function parseIni(content, filename) {
        const lines = content.split('\n');
        const stats = {};
        
        lines.forEach(line => {
            if (line.includes('=')) {
                const [key, val] = line.split('=');
                stats[key.trim()] = val.trim();
            }
        });

        // Mapeo de stats (Esto depende de cómo exporte ASA, ajustamos nombres comunes)
        const name = stats['CharacterName'] || 'Dino Desconocido';
        const type = stats['DinoClassName'] || 'Especie';
        const level = stats['CharacterLevel'] || '0';
        
        // Puntos Salvajes (Wild Levels) - ASA suele exportarlos como 'HealthPoints', etc.
        const hp = stats['HealthPoints'] || '0';
        const st = stats['StaminaPoints'] || '0';
        const ox = stats['OxygenPoints'] || '0';
        const fo = stats['FoodPoints'] || '0';
        const we = stats['WeightPoints'] || '0';
        const me = stats['MeleeDamagePoints'] || '0';
        const sp = stats['SpeedPoints'] || '0';

        let md = `# 🦖 Registro: ${name}\n`;
        md += `**Especie:** ${type} | **Nivel:** ${level}\n\n`;
        md += `| Stat | Puntos Salvajes (Wild) |\n`;
        md += `| :--- | :--- |\n`;
        md += `| ❤️ Vida | **${hp} pts** |\n`;
        md += `| ⚡ Energía | **${st} pts** |\n`;
        md += `| 🧪 Oxígeno | **${ox} pts** |\n`;
        md += `| 🍖 Comida | **${fo} pts** |\n`;
        md += `| ⚖️ Peso | **${we} pts** |\n`;
        md += `| ⚔️ Daño | **${me} pts** |\n`;
        md += `| 🏃 Vel. | **${sp} pts** |\n\n`;
        md += `--- \n*Ficha generada automáticamente desde: ${filename}*`;

        output.value = md;
        resultArea.style.display = 'block';
    }

    function copyToClipboard() {
        output.select();
        document.execCommand('copy');
        alert('¡Copiado! Ahora pégalo en tu página de usuario.');
    }
</script>
</html>