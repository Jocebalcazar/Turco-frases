# Turco-frases
Traductor gramatical
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Türkçe — Generador de Frases</title>
<style>
* { box-sizing: border-box; margin: 0; padding: 0; }
body { background:#0f0f13; color:#f0ebe0; font-family:system-ui,-apple-system,sans-serif; min-height:100vh; padding:22px 16px 60px; }
header { text-align:center; margin-bottom:28px; }
.eyebrow { font-size:10px; letter-spacing:4px; color:#e63e3e; text-transform:uppercase; margin-bottom:6px; }
h1 { font-size:24px; font-weight:700; }
h1 span { color:#e63e3e; }
.subtitle { font-size:12px; color:#6b6b80; margin-top:5px; }
.card { background:#17171e; border:1px solid #2a2a38; border-radius:12px; padding:16px; margin-bottom:12px; }
.lbl { font-size:10px; letter-spacing:3px; color:#6b6b80; text-transform:uppercase; display:block; margin-bottom:8px; }
select { width:100%; background:#0f0f13; border:1px solid #2a2a38; border-radius:8px; color:#f0ebe0; font-family:inherit; font-size:15px; padding:11px 12px; outline:none; appearance:none; -webkit-appearance:none; background-image:url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='12' height='8' viewBox='0 0 12 8'%3E%3Cpath d='M1 1l5 5 5-5' stroke='%236b6b80' stroke-width='1.5' fill='none' stroke-linecap='round'/%3E%3C/svg%3E"); background-repeat:no-repeat; background-position:right 12px center; cursor:pointer; margin-bottom:10px; }
select:focus { border-color:#e63e3e; }
.toggle-row { display:flex; gap:8px; margin-top:4px; }
.toggle-btn { flex:1; padding:10px; border-radius:8px; border:1px solid #2a2a38; background:#0f0f13; color:#6b6b80; font-family:inherit; font-size:13px; cursor:pointer; transition:all 0.15s; font-weight:500; }
.toggle-btn.active { background:rgba(230,62,62,0.15); border-color:#e63e3e; color:#f0ebe0; }
button#generar { width:100%; background:#e63e3e; border:none; border-radius:10px; color:#fff; font-family:inherit; font-size:16px; font-weight:700; padding:15px; cursor:pointer; margin-bottom:14px; }
button#generar:active { opacity:0.85; }
#resultado { display:none; }
.frase-es { font-size:13px; color:#6b6b80; font-style:italic; margin-bottom:8px; }
.frase-tr { font-family:monospace; font-size:24px; font-weight:700; line-height:1.3; color:#f0ebe0; margin-bottom:5px; }
.translit { font-size:13px; color:#6b6b80; margin-bottom:14px; }
.divider { height:1px; background:#2a2a38; margin:14px 0; }
.sec-lbl { font-size:10px; letter-spacing:3px; color:#e63e3e; text-transform:uppercase; margin-bottom:12px; }
.pieza { display:flex; align-items:flex-start; gap:10px; margin-bottom:11px; }
.token { font-family:monospace; font-size:13px; font-weight:700; color:#3ecf8e; background:rgba(62,207,142,0.08); border-radius:6px; padding:3px 8px; white-space:nowrap; flex-shrink:0; margin-top:1px; }
.info { font-size:13px; color:#6b6b80; line-height:1.5; }
.info strong { color:#f0ebe0; font-weight:600; }
.tag { display:inline-block; background:#1e1e2e; border:1px solid #2a2a38; border-radius:4px; font-size:10px; padding:1px 6px; color:#e63e3e; font-family:monospace; margin-left:5px; vertical-align:middle; }
.nota { background:rgba(230,62,62,0.07); border-left:3px solid #7a1f1f; border-radius:0 6px 6px 0; padding:10px 14px; font-size:12px; color:#6b6b80; line-height:1.6; margin-top:4px; }
.btn-nueva { width:100%; background:transparent; border:1px solid #2a2a38; border-radius:10px; color:#6b6b80; font-family:inherit; font-size:14px; padding:11px; cursor:pointer; margin-top:12px; }
.btn-nueva:active { border-color:#e63e3e; color:#f0ebe0; }
</style>
</head>
<body>

<header>
  <div class="eyebrow">Türkçe Dersi</div>
  <h1>Generador de <span>Frases</span></h1>
  <p class="subtitle">Construye frases con İMEK · Análisis morfológico automático · Sin internet</p>
</header>

<div class="card">
  <span class="lbl">Persona</span>
  <select id="persona">
    <option value="ben">Ben — Yo</option>
    <option value="sen">Sen — Tú</option>
    <option value="o">O — Él / Ella</option>
    <option value="biz">Biz — Nosotros</option>
    <option value="siz">Siz — Usted / Vosotros</option>
    <option value="onlar">Onlar — Ellos / Ellas</option>
  </select>
</div>

<div class="card">
  <span class="lbl">Adjetivo 1 (opcional)</span>
  <select id="adj1"><option value="">— ninguno —</option></select>
  <span class="lbl" style="margin-top:10px">Adjetivo 2 (opcional)</span>
  <select id="adj2"><option value="">— ninguno —</option></select>
</div>

<div class="card">
  <span class="lbl">Profesión o sustantivo (opcional)</span>
  <select id="sust"><option value="">— ninguno —</option></select>
</div>

<div class="card">
  <span class="lbl">Tipo de frase</span>
  <div class="toggle-row">
    <button class="toggle-btn active" id="btn-afirm" onclick="setTipo('afirm')">✓ Afirmativo</button>
    <button class="toggle-btn" id="btn-neg" onclick="setTipo('neg')">✗ Negativo (değil)</button>
  </div>
</div>

<button id="generar" onclick="generar()">Generar frase →</button>

<div class="card" id="resultado">
  <div class="sec-lbl">Traducción</div>
  <div class="frase-es" id="frase-es"></div>
  <div class="frase-tr" id="frase-tr"></div>
  <div class="translit" id="translit"></div>
  <div class="divider"></div>
  <div class="sec-lbl">Análisis morfológico</div>
  <div id="analisis"></div>
  <div id="nota-box"></div>
  <button class="btn-nueva" onclick="nueva()">← Nueva frase</button>
</div>

<script>
const PROFESIONES=[["avukat","abogado/a"],["doktor","médico/a"],["öğretmen","profesor/a"],["öğrenci","estudiante"],["asker","soldado"],["aşçı","cocinero/a"],["bakkal","tendero/a"],["balıkçı","pescador/a"],["bankacı","banquero/a"],["berber","barbero"],["çiftçi","agricultor/a"],["çoban","pastor/a"],["elektrikçi","electricista"],["fırıncı","panadero/a"],["garson","camarero/a"],["gazeteci","periodista"],["gemici","marinero/a"],["hademe","conserje"],["hâkim","juez/a"],["hemşire","enfermero/a"],["hoca","maestro/a"],["işçi","obrero/a"],["kasap","carnicero/a"],["kuaför","peluquero/a"],["kuyumcu","joyero/a"],["manav","verdulero/a"],["marangoz","carpintero/a"],["müdür","director/a"],["oyuncu","actor/actriz"],["pilot","piloto/a"],["polis","policía"],["memur","funcionario/a"],["rehber","guía"],["ressam","pintor/a"],["sanatçı","artista"],["sekreter","secretario/a"],["şâir","poeta"],["tamirci","mecánico/a"],["teknisyen","técnico/a"],["tercüman","traductor/a"],["terzi","sastre/a"],["veteriner","veterinario/a"],["yazar","escritor/a"],["fıstıkçı","vendedor/a de pistachos"],["anne","madre"],["baba","padre"],["kardeş","hermano/a"],["arkadaş","amigo/a"]];

const ADJETIVOS=[["atak","audaz"],["çekingen","tímido/a"],["becerikli","hábil"],["beceriksiz","torpe"],["çalışkan","trabajador/a"],["tembel","perezoso/a"],["dikkatli","prudente"],["dikkatsiz","imprudente"],["disiplinli","disciplinado/a"],["disiplinsiz","indisciplinado/a"],["güvenilir","confiable"],["güvenilmez","desconfiable"],["ikiyüzlü","hipócrita"],["samimi","sincero/a"],["kirli","sucio/a"],["temiz","limpio/a"],["özenli","cuidadoso/a"],["özensiz","descuidado/a"],["pasaklı","desaliñado/a"],["titiz","meticuloso/a"],["terbiyeli","educado/a"],["terbiyesiz","maleducado/a"],["anlayışlı","comprensivo/a"],["anlayışsız","incomprensivo/a"],["merhametli","compasivo/a"],["merhametsiz","despiadado/a"],["cömert","generoso/a"],["cimri","tacaño/a"],["kibirli","arrogante"],["mütevâzi","humilde"],["dürüst","honesto/a"],["sahtekar","estafador/a"],["cesur","valiente"],["korkak","cobarde"],["hızlı","rápido/a"],["yavaş","lento/a"],["yorgun","cansado/a"],["üzgün","triste"]];

const VOWELS=new Set(["a","e","ı","i","o","ö","u","ü"]);
const FRONT=new Set(["e","i","ö","ü"]);
const KETCAP=new Set(["f","s","t","k","ç","ş","h","p"]);
const MUTATION={p:"b",ç:"c",t:"d",k:"ğ"};
const NO_MUTAR=new Set(["doktor","pilot","teknisyen","veteriner","sekreter","garson","kuaför","rehber","müdür","kasap"]);

function lastVowel(w){for(let i=w.length-1;i>=0;i--)if(VOWELS.has(w[i]))return w[i];return "a";}
function arm2(w){return FRONT.has(lastVowel(w))?"e":"a";}
function arm4(w){const v=lastVowel(w);if(v==="a"||v==="ı")return"ı";if(v==="e"||v==="i")return"i";if(v==="o"||v==="u")return"u";return"ü";}
function endsVowel(w){return VOWELS.has(w[w.length-1]);}
function endsKetcap(w){return KETCAP.has(w[w.length-1]);}
function mutarRaiz(w){if(NO_MUTAR.has(w))return w;const l=w[w.length-1];return MUTATION[l]?w.slice(0,-1)+MUTATION[l]:w;}

function imek(word,persona){
  const v4=arm4(word);const isVoc=endsVowel(word);const isKet=endsKetcap(word)&&!NO_MUTAR.has(word);
  const base=isKet?mutarRaiz(word):word;
  switch(persona){
    case"ben":{const s=isVoc?`y${v4}m`:`${v4}m`;return{forma:base+s,sufijo:s,regla:isVoc?`vocal→+y+${v4}+m`:`cons→+${v4}+m`,muto:isKet};}
    case"biz":{const s=isVoc?`y${v4}z`:`${v4}z`;return{forma:base+s,sufijo:s,regla:isVoc?`vocal→+y+${v4}+z`:`cons→+${v4}+z`,muto:isKet};}
    case"sen":{const s=`s${v4}n`;return{forma:base+s,sufijo:s,regla:`+s+${v4}+n`,muto:isKet};}
    case"siz":{const s=`s${v4}n${v4}z`;return{forma:base+s,sufijo:s,regla:`+s+${v4}+n+${v4}+z`,muto:isKet};}
    case"o":{const s=isKet?`t${v4}r`:`d${v4}r`;return{forma:word+s,sufijo:s,regla:isKet?`sorda→+t+${v4}+r`:`+d+${v4}+r`,muto:false};}
    case"onlar":{const o=imek(word,"o");const pl=arm2(word)==="e"?"ler":"lar";return{forma:o.forma+pl,sufijo:o.sufijo+pl,regla:`İMEK(O)+${pl}`,muto:false};}
  }
}

function imekNeg(persona){
  const s={ben:"im",biz:"iz",sen:"sin",siz:"siniz",o:"dir",onlar:"dirler"};
  return{sufijo:s[persona]};
}

function poblar(id,lista){
  const sel=document.getElementById(id);
  lista.forEach(([v,l])=>{const o=document.createElement("option");o.value=v;o.textContent=`${v} — ${l}`;sel.appendChild(o);});
}
poblar("adj1",ADJETIVOS);poblar("adj2",ADJETIVOS);poblar("sust",PROFESIONES);

let tipo="afirm";
function setTipo(t){tipo=t;document.getElementById("btn-afirm").classList.toggle("active",t==="afirm");document.getElementById("btn-neg").classList.toggle("active",t==="neg");}
function nueva(){document.getElementById("resultado").style.display="none";}

const ES_VERBO={ben:"soy",sen:"eres",o:"es",biz:"somos",siz:"sois/es",onlar:"son"};
const ES_NEG={ben:"no soy",sen:"no eres",o:"no es",biz:"no somos",siz:"no sois/es",onlar:"no son"};
const ES_SUJ={ben:"Yo",sen:"Tú",o:"Él/Ella",biz:"Nosotros",siz:"Usted/Vosotros",onlar:"Ellos/Ellas"};
const PER_LABEL={ben:"Ben (yo)",sen:"Sen (tú)",o:"O (él/ella)",biz:"Biz (nosotros)",siz:"Siz",onlar:"Onlar"};

function generar(){
  const persona=document.getElementById("persona").value;
  const a1=document.getElementById("adj1").value;
  const a2=document.getElementById("adj2").value;
  const sv=document.getElementById("sust").value;
  if(!a1&&!a2&&!sv){alert("Elige al menos un adjetivo o una profesión.");return;}

  const piezas=[];
  let fraseTr=[];
  let esPartes=[];
  const adjs=[a1,a2].filter(Boolean);

  adjs.forEach(adj=>{
    const lbl=ADJETIVOS.find(([v])=>v===adj)?.[1]||adj;
    piezas.push({token:adj,explicacion:`${lbl} (adjetivo)`,regla:"sin cambio — adjetivos no declinan"});
    fraseTr.push(adj);esPartes.push(lbl);
  });

  if(sv){
    const lbl=PROFESIONES.find(([v])=>v===sv)?.[1]||sv;
    piezas.push({token:sv,explicacion:`${lbl} (sustantivo/profesión)`,regla:"raíz"});
    fraseTr.push(sv);esPartes.push(lbl);
  }

  const palabraImek=sv||(adjs.length>0?adjs[adjs.length-1]:null);
  if(!palabraImek)return;
  fraseTr.pop();

  const notas=[];

  if(tipo==="afirm"){
    const res=imek(palabraImek,persona);
    fraseTr.push(res.forma);
    if(res.muto){
      piezas.push({token:mutarRaiz(palabraImek),explicacion:`raíz mutada (${palabraImek[palabraImek.length-1]}→${MUTATION[palabraImek[palabraImek.length-1]]})`,regla:"ketçap-fıstıkçı şahap"});
      notas.push(`"${palabraImek}" termina en consonante del grupo FISTIKÇI ŞAHAP: muta antes del sufijo vocal.`);
    }
    piezas.push({token:res.sufijo,explicacion:`sufijo İMEK para ${PER_LABEL[persona]}`,regla:res.regla});
    document.getElementById("frase-es").textContent=`"${ES_SUJ[persona]} ${ES_VERBO[persona]} ${esPartes.join(" y ")}"`;
  } else {
    fraseTr.push(palabraImek);
    const neg=imekNeg(persona);
    fraseTr.push(`değil${neg.sufijo}`);
    piezas.push({token:"değil",explicacion:"negación (no)",regla:"invariable"});
    piezas.push({token:neg.sufijo,explicacion:`sufijo İMEK para ${PER_LABEL[persona]}`,regla:"armonía con 'i' de değil"});
    document.getElementById("frase-es").textContent=`"${ES_SUJ[persona]} ${ES_NEG[persona]} ${esPartes.join(" y ")}"`;
  }

  if(adjs.length>0&&sv) notas.push("El adjetivo siempre va ANTES del sustantivo en turco.");
  if(NO_MUTAR.has(palabraImek)) notas.push(`"${palabraImek}" es un préstamo: la consonante final NO muta.`);

  const tr=fraseTr.join(" ").replace(/ğ/g,"ğ(muda)").replace(/ş/g,"sh").replace(/ç/g,"ch").replace(/â/g,"a");
  document.getElementById("frase-tr").textContent=fraseTr.join(" ");
  document.getElementById("translit").textContent=`Pronunciación aprox.: ${tr}`;

  const analDiv=document.getElementById("analisis");
  analDiv.innerHTML="";
  piezas.forEach(p=>{
    const d=document.createElement("div");d.className="pieza";
    d.innerHTML=`<div class="token">${p.token}</div><div class="info"><strong>${p.explicacion}</strong>${p.regla?`<span class="tag">${p.regla}</span>`:""}</div>`;
    analDiv.appendChild(d);
  });

  document.getElementById("nota-box").innerHTML=notas.length?`<div class="divider"></div><div class="nota">💡 ${notas.join(" ")}</div>`:"";
  document.getElementById("resultado").style.display="block";
  document.getElementById("resultado").scrollIntoView({behavior:"smooth"});
}
</script>
</body>
</html>
