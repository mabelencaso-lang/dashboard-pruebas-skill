---
name: "dashboard-pruebas"
description: "Arma un dashboard HTML de estudio con los patrones que más se repiten en pruebas pasadas de cualquier ramo, con el estilo fijo de Belén. Usar cuando suba pruebas y pida un dashboard."
---

# Dashboard de patrones de pruebas

La usuaria sube pruebas/exámenes pasados (y a veces ayudantías) de cualquier ramo y quiere un dashboard que muestre los patrones que más se repiten. No le pidas programa, clases ni que te diga los patrones: identifícalos tú desde los documentos. Si ella te da información extra (temario, fecha, qué entra), úsala.

## Pasos

1. Lee TODOS los documentos adjuntos completos, pregunta por pregunta (usa pdftotext u otra herramienta; si una tabla es imagen y no se lee, dilo en vez de inventar).
2. Toma como ANCLA la prueba más reciente.
3. Agrupa todas las preguntas en 6–10 bloques temáticos.
4. Para cada bloque calcula en cuántas pruebas aparece (ej. 5/6) y el rango de puntos (ej. 8–23 pts).
5. Ordena los bloques de más a menos importante (frecuencia + puntos + si salió en la más reciente).
6. Detecta ejercicios de ayudantía que se repitan igual o casi igual en alguna prueba. Si el parecido es muy fuerte (mismos números o mismo enunciado, no solo el mismo tema), agrégalo también a `REPEATS` (ver abajo); si es solo el mismo tipo de ejercicio, basta con mencionarlo en el `dsc` del ejercicio correspondiente.
7. Si el temario tiene categorías claras y con nombre propio (unidades, partes, módulos — lo que diga el programa del curso o los encabezados de las pruebas), asigna `cat`/`catlbl` a cada bloque. Si no hay una división así de clara, deja `cat` sin definir en todos los bloques — no la inventes.
8. Si calculas tú misma el resultado de un ejercicio porque no hay pauta oficial disponible, dilo: agrega `verified:false` a ese ejercicio en vez de presentarlo como si viniera de una pauta.
9. Construye el HTML con la plantilla de abajo. NO cambies el CSS ni el JavaScript; solo reemplaza los datos entre `[ ]` y las variables `EX`, `T`, `REPEATS`, `NOTES`, `FUENTES`.
10. Guarda el archivo como `dashboard_[ramo]_[evaluacion].html`. Si hay una carpeta conectada, guárdalo ahí junto a las pruebas; si no, entrégalo en el chat.
11. Al final, responde en el chat (no en otro archivo) con 3–5 líneas: los temas más repetidos y cualquier ejercicio que se repita textual.

## Cómo llenar los datos

- `EX`: una entrada por prueba revisada (+ una para ayudantías si hay). La más reciente primero con "★ ANCLA". Colores en este orden: `#d62828 #4aab6d #e08c3a #e05ca0 #9b5de5 #3a8fd4 #2ab8b8 #888`; ayudantías `#a3852a`.
- `T`: un objeto por bloque:
  - `n`: número del bloque.
  - `cat` / `catlbl` (OPCIONAL, ver paso 7): clave corta de la categoría (ej. `"u1"`) y su nombre visible (ej. `"Unidad 1"`). Si dos o más bloques comparten `cat`, el dashboard arma solo un índice de navegación arriba con las categorías agrupadas; con 0 o 1 categoría distinta, el índice no se muestra.
  - `t`: título del tema en MAYÚSCULAS.
  - `pts`: "PRIORIDAD [MÁXIMA/ALTA/MEDIA/BAJA] · [x/y] pruebas · [rango] pts".
  - `hot`: true si la prioridad es máxima o alta.
  - `p`: ids de las pruebas donde aparece.
  - `ej`: ejercicios del tema, cada uno con:
    - `tag`: `"tp"` = CLAVE (ejercicio central, con datos/tabla), `"ts"` = PRÁCTICA, `"tn"` = PATRÓN (algo que se repite siempre).
    - `dot`: id de la prueba.
    - `src`: "NombrePrueba — Pregunta X: de qué se trata" (+ " ★ ANCLA" si es la más reciente).
    - `file`: nombre EXACTO del archivo.
    - `dsc`: qué piden en 2–4 líneas, con datos clave, fórmulas y trampas. Puedes envolver números importantes en `<span class="num-em">...</span>` para que resalten.
    - `verified` (OPCIONAL, ver paso 8): pon `false` solo cuando el número/resultado lo calculaste tú porque no hay pauta — se marca automáticamente como "(cálculo propio, sin pauta oficial)". Omite este campo (o pon `true`) cuando el resultado viene de una pauta real.
- `REPEATS` (OPCIONAL, ver paso 6): arreglo de `{years:"2021 P1a → 2025 P1a", text:"qué se repitió y por qué es casi textual"}`. Solo para repeticiones muy fuertes entre evaluaciones distintas (no ejercicios de ayudantía parecidos entre sí). Si no encuentras ninguna, deja `const REPEATS=[];` — el recuadro no se muestra si está vacío.
- `NOTES`: 2–5 tips (ruta sugerida de ejercicios, fórmulas a memorizar, trampas), con el `n` del bloque como clave.
- `FUENTES`: string con los nombres de archivo que efectivamente usaste, separados por coma — se muestra al pie como aviso de que los números vienen de esos documentos.

El dashboard ya trae, sin que tengas que programar nada: modo oscuro/claro automático según el navegador, un círculo de check por ejercicio que la usuaria puede marcar como resuelto (se guarda en ese navegador, por dashboard), y un contador "x/y ✓" por bloque.

## Reglas

- Usa la notación del curso que aparezca en los documentos.
- No inventes números ni preguntas. Si calculaste tú un resultado porque no había pauta, dilo con `verified:false` (no lo escondas ni lo presentes como oficial).
- No agregues recuadros de aviso de colores ni secciones extra fuera de los bloques por tema, del índice de categorías (`cat`) y del recuadro de repeticiones (`REPEATS`) — esos tres son parte fija de la plantilla, no agregues otros.
- Escribe en español.

## Plantilla (copiar tal cual, solo cambiar los datos marcados en el bloque `// ===== DATOS =====`)

```html
<!DOCTYPE html>
<html lang="es">
<head><meta charset="UTF-8"><meta name="viewport" content="width=device-width,initial-scale=1"><title>[RAMO] — Dashboard [EVALUACIÓN]</title>
<style>
@import url('https://fonts.googleapis.com/css2?family=IBM+Plex+Sans:wght@400;600;700;800&family=IBM+Plex+Mono:wght@500;600;700&display=swap');
:root{
  --paper:      #f4f1e8;
  --surface:    #fffefb;
  --surface-2:  #efece2;
  --ink:        #181a20;
  --ink-2:      #5b5d66;
  --ink-mute:   #8b8d93;
  --line:       #e4e1d5;
  --line-strong:#d3cfc0;
  --accent:     #1f5fae;
  --accent-soft:#e8f0fb;
  --gold:       #a9822c;
  --gold-soft:  #faf3e1;
  --danger:     #b23b3b;
  --danger-soft:#fbeaea;
  --good:       #2f7d5a;
  --good-soft:  #e9f5ee;
}
@media (prefers-color-scheme: dark){
  :root:not([data-theme="light"]){
    --paper:      #15161b;
    --surface:    #1d1f26;
    --surface-2:  #24262e;
    --ink:        #f0efe9;
    --ink-2:      #b7b6ae;
    --ink-mute:   #86868c;
    --line:       #30323c;
    --line-strong:#3c3e4a;
    --accent:     #6fa3e0;
    --accent-soft:#1f2c3d;
    --gold:       #d9b35f;
    --gold-soft:  #332a1a;
    --danger:     #e07a7a;
    --danger-soft:#3a2323;
    --good:       #7fc7a4;
    --good-soft:  #1c332a;
  }
}
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:'IBM Plex Sans','Helvetica Neue',Helvetica,Arial,sans-serif;background:var(--paper);color:var(--ink);padding:24px 20px 40px;max-width:1100px;margin:0 auto}
h1{font-size:21px;font-weight:800;letter-spacing:-.4px}
.sub{font-size:11.5px;color:var(--ink-mute);margin-top:5px;margin-bottom:16px;line-height:1.55}
.legend{display:flex;flex-wrap:wrap;gap:9px;margin-bottom:18px}
.leg{display:flex;align-items:center;gap:5px;font-size:11px;font-weight:600;color:var(--ink-2)}
.ldot{width:11px;height:11px;border-radius:50%;flex-shrink:0}

.callout{
  background:var(--surface); border:1px solid var(--line); border-left:3px solid var(--danger);
  border-radius:0 11px 11px 0; padding:12px 15px; margin-bottom:16px;
}
.ctitle{font-size:11.5px; font-weight:800; margin-bottom:8px; color:var(--danger)}
.repeat{font-size:11.5px; color:var(--ink-2); line-height:1.6; margin-bottom:6px}
.repeat:last-child{margin-bottom:0}
.repeat .years{font-family:'IBM Plex Mono',monospace; font-weight:700; color:var(--ink); display:block}
.repeat b{color:var(--danger)}

.toc{display:grid; grid-template-columns:repeat(auto-fit,minmax(180px,1fr)); gap:11px; margin-bottom:16px}
.toc-col{background:var(--surface); border:1px solid var(--line); border-radius:12px; padding:12px 13px}
.toc-col-head{font-size:10px; font-weight:800; letter-spacing:.04em; text-transform:uppercase; padding-bottom:8px; margin-bottom:8px; border-bottom:1px solid var(--line)}
.toc-item{display:flex; align-items:flex-start; gap:7px; padding:5px 0; cursor:pointer; font-size:11.5px; line-height:1.4; color:var(--ink-2)}
.toc-item:hover{color:var(--ink)}
.toc-item:hover .toc-num{background:var(--ink)}
.toc-num{font-family:'IBM Plex Mono',monospace; font-size:9px; font-weight:700; color:var(--surface); background:var(--ink-mute); border-radius:4px; width:16px; height:16px; flex-shrink:0; margin-top:1px; display:flex; align-items:center; justify-content:center; transition:background .15s}

.card{background:var(--surface); border:1px solid var(--line); border-radius:12px; margin-bottom:10px; overflow:hidden}
.card-head{display:flex; flex-wrap:wrap; align-items:center; gap:8px 11px; padding:13px 15px; cursor:pointer}
.card-head:hover{background:var(--surface-2)}
.num{font-family:'IBM Plex Mono',monospace; font-size:10.5px; font-weight:700; color:var(--surface); background:var(--ink); border-radius:5px; width:22px; height:22px; flex-shrink:0; display:flex; align-items:center; justify-content:center}
.catlbl{font-size:9.5px; font-weight:800; letter-spacing:.04em; border-radius:5px; padding:2px 6px; flex-shrink:0; text-transform:uppercase}
.ttl{flex:1; font-size:13px; font-weight:700; min-width:160px}
.dots{display:flex; gap:3px; flex-shrink:0}
.dot{width:10px; height:10px; border-radius:50%}
.dot-e{border:1.5px solid var(--line-strong); background:transparent}
.prog{font-family:'IBM Plex Mono',monospace; font-size:9.5px; font-weight:700; color:var(--ink-mute); background:var(--surface-2); border:1px solid var(--line-strong); border-radius:20px; padding:3px 8px; white-space:nowrap; flex-shrink:0; transition:color .15s,background .15s,border-color .15s}
.prog.full{color:var(--good); background:var(--good-soft); border-color:var(--good)}
.pts{font-family:'IBM Plex Mono',monospace; font-size:9.5px; font-weight:700; color:var(--gold); background:var(--gold-soft); border:1px solid var(--gold); border-radius:20px; padding:3px 9px; white-space:nowrap; flex-shrink:0}
.pts.hot{color:var(--danger); background:var(--danger-soft); border-color:var(--danger)}
.chev{font-size:10px; color:var(--ink-mute); transition:transform .15s; flex-shrink:0}
.chev.open{transform:rotate(180deg)}

.detail{display:none; border-top:1px solid var(--line); padding:2px 15px 13px}
.detail.open{display:block}
.row{display:flex; gap:10px; padding:10px 0; border-bottom:1px solid var(--line); align-items:flex-start; transition:opacity .15s}
.row:last-child{border-bottom:none}
.row.done{opacity:.5}
.rdot{width:8px; height:8px; border-radius:50%; flex-shrink:0; margin-top:5px}
.tag{font-family:'IBM Plex Mono',monospace; font-size:9px; font-weight:700; padding:2px 6px; border-radius:4px; flex-shrink:0; margin-top:2px; white-space:nowrap; text-transform:uppercase; letter-spacing:.02em}
.tp{background:var(--good-soft); color:var(--good); border:1px solid var(--good)}
.ts{background:var(--surface-2); color:var(--ink-2); border:1px solid var(--line-strong)}
.tn{background:var(--gold-soft); color:var(--gold); border:1px solid var(--gold)}
.src{font-size:12.5px; font-weight:700; color:var(--ink); line-height:1.4}
.row.done .src{text-decoration:line-through; text-decoration-color:var(--good)}
.fname{font-family:'IBM Plex Mono',monospace; font-size:10px; color:var(--gold); margin-top:2px}
.dsc{font-size:11.5px; color:var(--ink-2); margin-top:4px; line-height:1.55}
.dsc .num-em{font-family:'IBM Plex Mono',monospace; font-variant-numeric:tabular-nums; color:var(--ink); font-weight:600}
.unverified{font-style:italic; color:var(--ink-mute)}
.note{background:var(--surface-2); border-left:3px solid var(--gold); border-radius:0 8px 8px 0; padding:10px 13px; font-size:11.5px; color:var(--ink-2); margin-top:11px; line-height:1.6}
.note strong{color:var(--gold)}
.chk{width:19px; height:19px; border-radius:50%; border:1.6px solid var(--line-strong); flex-shrink:0; margin-left:auto; cursor:pointer; display:flex; align-items:center; justify-content:center; transition:background .15s,border-color .15s; -webkit-tap-highlight-color:transparent}
.chk:hover{border-color:var(--good)}
.chk::after{content:""; width:9px; height:9px; border-radius:50%; background:var(--good); transform:scale(0); transition:transform .12s}
.chk.done{border-color:var(--good); background:var(--good-soft)}
.chk.done::after{transform:scale(1)}
footer{margin-top:22px; font-size:10.5px; color:var(--ink-mute); line-height:1.6}
</style></head>
<body>
<h1>[RAMO] — Patrones [EVALUACIÓN]</h1>
<p class="sub">[N] bloques temáticos ordenados por prioridad · Ancla: [PRUEBA MÁS RECIENTE] · [N] pruebas revisadas · Haz clic en cada bloque para ver los ejercicios con el nombre exacto del archivo, y marca el círculo cuando ya lo resolviste — queda guardado en este navegador.</p>
<div class="legend" id="leg"></div>

<div class="callout" id="calloutBox" style="display:none">
  <div class="ctitle">🔁 Preguntas que se repitieron casi textuales</div>
  <div id="repeatsList"></div>
</div>

<div class="toc" id="toc"></div>

<div id="cards"></div>

<footer id="foot"></footer>

<script>
// ===== DATOS (lo único que cambia al llenar el dashboard) =====
const EX=[
  {id:"[P1]",lbl:"[Prueba más reciente] ★ ANCLA", c:"#d62828"},
  {id:"[P2]",lbl:"[Prueba anterior]",             c:"#4aab6d"},
];

const T=[
 {n:1,cat:"[c1]",catlbl:"[Nombre de la categoría]",t:"[TEMA MÁS IMPORTANTE]",pts:"PRIORIDAD MÁXIMA · [x/y] pruebas · [rango] pts",hot:true,
  p:["[P1]","[P2]"],
  ej:[
   {tag:"tp",dot:"[P1]",src:"[Prueba] — Pregunta [X]: [de qué se trata] ★ ANCLA",file:"[archivo.pdf]",
    dsc:"[Qué piden, datos clave y trampas, con <span class=\"num-em\">números</span> si corresponde]"},
  ]},
];

const REPEATS=[
  // {years:"[prueba vieja] → [prueba nueva]", text:"[qué se repitió y por qué es casi textual]"},
];

const NOTES={
 1:`<strong>[Tip]:</strong> [ruta sugerida, fórmula clave o trampa típica]`,
};

const FUENTES = "[lista de archivos usados, separados por coma]";

// ===== RENDER (no tocar) =====
const CM={}; EX.forEach(e=>CM[e.id]=e.c);
function hexToRgba(hex,a){
  const h=hex.replace('#','');
  const r=parseInt(h.substring(0,2),16), g=parseInt(h.substring(2,4),16), b=parseInt(h.substring(4,6),16);
  return `rgba(${r},${g},${b},${a})`;
}
function slugify(s){ return s.toLowerCase().normalize("NFD").replace(/[̀-ͯ]/g,"").replace(/[^a-z0-9]+/g,"_").slice(0,40); }
const STORE_KEY = "dash_chk_" + slugify(document.title);

const legEl=document.getElementById("leg");
EX.forEach(e=>{
  const d=document.createElement("div");d.className="leg";
  d.innerHTML=`<div class="ldot" style="background:${e.c}"></div><span style="font-family:'IBM Plex Mono',monospace;font-size:10px">${e.lbl}</span>`;
  legEl.appendChild(d);
});

if(REPEATS.length){
  document.getElementById("calloutBox").style.display="block";
  const rl=document.getElementById("repeatsList");
  REPEATS.forEach(r=>{
    const d=document.createElement("div");d.className="repeat";
    d.innerHTML=`<span class="years">${r.years}</span> <span>${r.text}</span>`;
    rl.appendChild(d);
  });
}

let DONE={};
try{ DONE=JSON.parse(localStorage.getItem(STORE_KEY)||"{}"); }catch(e){ DONE={}; }
function saveDone(){ try{ localStorage.setItem(STORE_KEY, JSON.stringify(DONE)); }catch(e){} }

function catColor(cat){
  const palette=["#1f5fae","#b2691f","#2f7d5a","#9b5de5","#c0392b","#2ab8b8"];
  const cats=[...new Set(T.filter(t=>t.cat).map(t=>t.cat))];
  const idx=cats.indexOf(cat);
  return palette[idx % palette.length];
}

const cardsEl=document.getElementById("cards");
T.forEach(t=>{
  const card=document.createElement("div");card.className="card";card.id="card-"+t.n;
  const dotsH=EX.map(e=>t.p.includes(e.id)
    ?`<div class="dot" style="background:${e.c}" title="${e.lbl}"></div>`
    :`<div class="dot dot-e" title="${e.lbl}"></div>`).join("");
  const head=document.createElement("div");head.className="card-head";
  const total=t.ej.length;
  const doneCount=t.ej.filter((ej,i)=>DONE[t.n+"_"+i]).length;
  const catH = t.cat ? `<span class="catlbl" style="background:${hexToRgba(catColor(t.cat),0.14)};color:${catColor(t.cat)}">${t.catlbl||t.cat}</span>` : "";
  head.innerHTML=`<div class="num">${t.n}</div>${catH}<div class="ttl">${t.t}</div><div class="dots">${dotsH}</div><div class="prog${doneCount===total?' full':''}">${doneCount}/${total} ✓</div><div class="pts${t.hot?' hot':''}">${t.pts}</div><div class="chev">▼</div>`;
  const progEl=head.querySelector(".prog");
  const det=document.createElement("div");det.className="detail";
  t.ej.forEach((ej,i)=>{
    const id=t.n+"_"+i;
    const isDone=!!DONE[id];
    const r=document.createElement("div");r.className="row"+(isDone?" done":"");
    const tagTxt=ej.tag==='tp'?'CLAVE':ej.tag==='tn'?'PATRÓN':'PRÁCTICA';
    const unver = ej.verified===false ? ` <span class="unverified">(cálculo propio, sin pauta oficial)</span>` : "";
    r.innerHTML=`
      <div class="rdot" style="background:${CM[ej.dot]||'#999'}"></div>
      <span class="tag ${ej.tag}">${tagTxt}</span>
      <div>
        <div class="src">${ej.src}</div>
        <div class="fname">📄 ${ej.file}</div>
        <div class="dsc">${ej.dsc}${unver}</div>
      </div>
      <div class="chk${isDone?' done':''}" data-id="${id}" title="Marcar como resuelto"></div>`;
    const chk=r.querySelector(".chk");
    chk.addEventListener("click",(ev)=>{
      ev.stopPropagation();
      const nowDone=!DONE[id];
      DONE[id]=nowDone;
      saveDone();
      chk.classList.toggle("done",nowDone);
      r.classList.toggle("done",nowDone);
      const newCount=t.ej.filter((_,j)=>DONE[t.n+"_"+j]).length;
      progEl.textContent=`${newCount}/${total} ✓`;
      progEl.classList.toggle("full",newCount===total);
    });
    det.appendChild(r);
  });
  if(NOTES[t.n]){
    const nt=document.createElement("div");nt.className="note";
    nt.innerHTML=NOTES[t.n];det.appendChild(nt);
  }
  head.addEventListener("click",()=>{
    det.classList.toggle("open");
    head.querySelector(".chev").classList.toggle("open");
  });
  card.appendChild(head);card.appendChild(det);cardsEl.appendChild(card);
});

const cats=[...new Set(T.filter(t=>t.cat).map(t=>t.cat))];
if(cats.length>=2){
  const tocEl=document.getElementById("toc");
  cats.forEach(cat=>{
    const items=T.filter(t=>t.cat===cat).sort((a,b)=>a.n-b.n);
    const label = items[0].catlbl || cat;
    const col=document.createElement("div");col.className="toc-col";
    const h=document.createElement("div");h.className="toc-col-head";h.style.color=catColor(cat);h.textContent=label;
    col.appendChild(h);
    items.forEach(t=>{
      const it=document.createElement("div");it.className="toc-item";
      const shortTitle=t.t.split(" (")[0].split(":")[0];
      it.innerHTML=`<div class="toc-num">${t.n}</div><span>${shortTitle}</span>`;
      it.addEventListener("click",()=>{
        const targetCard=document.getElementById("card-"+t.n);
        const targetDet=targetCard.querySelector(".detail");
        const targetChev=targetCard.querySelector(".chev");
        if(!targetDet.classList.contains("open")){ targetDet.classList.add("open"); targetChev.classList.add("open"); }
        targetCard.scrollIntoView({behavior:"smooth",block:"center"});
      });
      col.appendChild(it);
    });
    tocEl.appendChild(col);
  });
} else {
  document.getElementById("toc").style.display="none";
}

document.getElementById("foot").textContent = "Fuentes: " + FUENTES + ". Los números de cada ejercicio están tomados directamente de esos documentos.";
</script>
</body></html>
```
