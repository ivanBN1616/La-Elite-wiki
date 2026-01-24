---
title: Registro de criaturas
description: 
published: true
date: 2026-01-24T02:06:51.646Z
tags: 
editor: markdown
dateCreated: 2026-01-24T02:06:51.646Z
---

# 🧬 Registro Automatizado de Sementales
> **Instrucciones:** En el juego, mira a tu dino, abre el menú circular y selecciona **"Exportar Dino"**. Luego, abre el archivo generado en tu carpeta de ARK, copia el contenido y pégalo aquí abajo.

---

## 📥 Paso 1: Pegar Exportación de ARK
<textarea id="arkExport" style="width: 100%; height: 150px; background: #1a1a1a; color: #4dff88; border: 1px solid #333; font-family: monospace; padding: 10px;" placeholder="Paste [DinoExport] content here..."></textarea>

<button onclick="procesarDino()" style="margin-top: 10px; background: #bc4dff; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer; font-weight: bold;">
  PROCESAR STATS ⚙️
</button>

---

## 📊 Paso 2: Resultado de Stats (Smart Breeding Style)
<div id="resultadoStats" style="display:none; background: #222; border: 1px solid #444; padding: 20px; border-radius: 10px;">
  <h3 id="resNombre" style="color: #00d4ff; margin-top: 0;"></h3>
  <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 10px;">
    <div style="color: #ff4d4d;">❤️ Vida: <span id="statVida">--</span></div>
    <div style="color: #4dff88;">⚡ Stamina: <span id="statStam">--</span></div>
    <div style="color: #ffff4d;">⚖️ Peso: <span id="statPeso">--</span></div>
    <div style="color: #ff884d;">⚔️ Daño: <span id="statDano">--</span></div>
  </div>
  <hr style="border: 0; border-top: 1px dashed #555;">
  <p><small>Copia el código de abajo para tu ficha personal:</small></p>
  <code id="codigoWiki" style="display: block; background: #000; padding: 5px; color: #ccc;"></code>
</div>

<script>
function procesarDino() {
  const raw = document.getElementById('arkExport').value;
  const stats = {};
  
  // Extracción simple de valores (Simulando parseo de archivo .ini)
  const lines = raw.split('\n');
  lines.forEach(line => {
    if(line.includes('DinoNameTag=')) stats.nombre = line.split('=')[1];
    if(line.includes('Health=')) stats.vida = Math.round(line.split('=')[1]);
    if(line.includes('MeleeDamage=')) stats.dano = Math.round(line.split('=')[1] * 100) + "%";
    // Nota: El export de ARK da valores raw, el Smart Breeding los traduce a puntos.
  });

  if(stats.nombre) {
    document.getElementById('resultadoStats').style.display = 'block';
    document.getElementById('resNombre').innerText = "Dino: " + stats.nombre;
    document.getElementById('statVida').innerText = stats.vida || "N/A";
    document.getElementById('statDano').innerText = stats.dano || "N/A";
    document.getElementById('codigoWiki').innerText = "| " + stats.nombre + " | " + (stats.vida || "0") + " | " + (stats.dano || "0") + " |";
  } else {
    alert("No se detectó un formato de exportación válido.");
  }
}
</script>

---

## 🏆 Paso 3: ¿Es un Top Stat?
Si los stats procesados arriba son mejores que los actuales, debes ir a la página de **[Top Stats](/tribu/top-stats)** y solicitar la actualización al administrador de crianza.

> [!TIP]
> **Integración con Smart Breeding:** Para una precisión total de "puntos", se recomienda usar la aplicación Smart Breeding y exportar el JSON. Esta herramienta web es para un registro rápido en la Wiki.

