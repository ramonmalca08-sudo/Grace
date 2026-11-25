
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Museo interactivo de compuestos inorgánicos</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Chakra+Petch:wght@600;700&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
<style>
:root{--bg:#f3f1fa;--sf:#fff;--tx:#1d1b2e;--mu:#5d5a73;--bd:#d9d5ec;--ac:#6d3fd6;--ac2:#0fa3a3;--ok:#1a8f4d;--bad:#c62f3b;box-sizing:border-box;padding-top:env(safe-area-inset-top,0px);padding-bottom:env(safe-area-inset-bottom,0px)}
@media (prefers-color-scheme:dark){:root:not([data-theme="light"]){--bg:#0f0d1a;--sf:#1a1730;--tx:#ece9fb;--mu:#a29fc0;--bd:#33305a;--ac:#9b7bff;--ac2:#3ed6d0;--ok:#4cd98a;--bad:#ff6b77}}
:root[data-theme="dark"]{--bg:#0f0d1a;--sf:#1a1730;--tx:#ece9fb;--mu:#a29fc0;--bd:#33305a;--ac:#9b7bff;--ac2:#3ed6d0;--ok:#4cd98a;--bad:#ff6b77}
html{scroll-padding-top:env(safe-area-inset-top,0px);height:100%}
*{box-sizing:border-box}
body{margin:0;height:100%;background:var(--bg);color:var(--tx);font:14px/1.45 Inter,system-ui,sans-serif;display:flex;flex-direction:column}
header{display:flex;align-items:center;gap:12px;padding:10px 16px;border-bottom:1px solid var(--bd);background:var(--sf)}
header h1{font:700 18px 'Chakra Petch',sans-serif;margin:0}header small{color:var(--mu)}
.logo{font-size:26px}
main{flex:1;display:flex;min-height:0}
#stage{flex:1;position:relative;background:radial-gradient(circle at 50% 40%,#2a2150,#0b0918 75%);min-width:0}
canvas{display:block;width:100%;height:100%;touch-action:none}
#badge{position:absolute;left:14px;top:12px;color:#fff;pointer-events:none}
#badge b{font:700 40px 'Chakra Petch',sans-serif;display:block;line-height:1}
#badge span{font-size:13px;padding:2px 10px;border-radius:99px;display:inline-block;margin-top:6px;color:#12102a;font-weight:600}
#hint{position:absolute;left:14px;bottom:12px;color:#cfc9f5;font-size:12px;background:#0007;padding:4px 10px;border-radius:99px}
#legend{position:absolute;right:12px;top:12px;display:flex;flex-direction:column;gap:4px;color:#fff;font-size:12px}
#legend i{display:inline-block;width:12px;height:12px;border-radius:50%;margin-right:6px;vertical-align:-2px}
aside{width:360px;overflow:auto;padding:12px;background:var(--bg);border-left:1px solid var(--bd);scrollbar-color:var(--ac) var(--sf)}
aside::-webkit-scrollbar{width:8px}aside::-webkit-scrollbar-thumb{background:linear-gradient(var(--ac),var(--ac2));border-radius:8px}aside::-webkit-scrollbar-track{background:var(--sf)}
.card{background:var(--sf);border:1px solid var(--bd);border-radius:12px;padding:12px;margin-bottom:12px}
.card h3{font:700 14px 'Chakra Petch',sans-serif;margin:0 0 8px;color:var(--ac)}
.room{margin-bottom:8px}.room>button{width:100%;text-align:left;font-weight:600}
.chips{display:flex;flex-wrap:wrap;gap:6px;margin:6px 0 2px}
button{font:inherit;cursor:pointer;background:var(--sf);color:var(--tx);border:1px solid var(--bd);border-radius:8px;padding:6px 10px;transition:transform .15s,border-color .15s}
button:hover{transform:translateY(-2px);border-color:var(--ac)}button:focus-visible,input:focus-visible{outline:2px solid var(--ac2);outline-offset:2px}
button.on{background:var(--ac);color:#fff;border-color:var(--ac)}
.row{display:flex;gap:8px;flex-wrap:wrap;align-items:center;margin:6px 0}
input[type=range]{flex:1;accent-color:var(--ac)}
dl{margin:0;display:grid;grid-template-columns:auto 1fr;gap:3px 10px}dt{color:var(--mu)}dd{margin:0;font-weight:500}
.how{background:color-mix(in srgb,var(--ac) 10%,transparent);border-radius:8px;padding:8px;margin-top:8px}
.opt{display:block;width:100%;text-align:left;margin:5px 0}.good{border-color:var(--ok)!important;color:var(--ok)}.bad{border-color:var(--bad)!important;color:var(--bad)}
#toast{position:fixed;left:50%;bottom:calc(20px + env(safe-area-inset-bottom,0px));transform:translateX(-50%);background:var(--ac);color:#fff;padding:8px 14px;border-radius:99px;opacity:0;transition:opacity .25s;pointer-events:none}
#err{position:absolute;inset:0;display:none;place-items:center;text-align:center;color:#fff;background:#0b0918;padding:20px}
@media(max-width:920px){main{flex-direction:column}#stage{flex:none;height:58vh}aside{width:100%;border-left:0;border-top:1px solid var(--bd)}}
@media(prefers-reduced-motion:reduce){*{transition:none!important}}
</style>
</head>
<body>
<header><span class="logo">🏛️</span><div><h1>Museo de compuestos inorgánicos</h1><small>Formula, nombra y clasifica · modelos moleculares y redes iónicas en 3D</small></div></header>
<main>
<div id="stage"><canvas id="cv"></canvas><div id="badge"></div><div id="legend"></div><div id="hint">Arrastra para girar · rueda para acercar · ← → cambia de pieza</div>
<div id="err"><div><p id="errm">No se pudo cargar la escena 3D. Revisa tu conexión.</p><button onclick="location.reload()">Reintentar</button></div></div></div>
<aside>
<div class="card"><h3>🚪 Salas del museo (familias)</h3><div id="rooms"></div></div>
<div class="card" id="info"></div>
<div class="card"><h3>🛠️ Herramientas</h3>
<div class="row"><label for="ex">Separar</label><input id="ex" type="range" min="0" max="100" value="0"><span id="exv">0%</span></div>
<div class="row"><button id="bRot" class="on">↻ Giro automático</button><button id="bQuiz">🎯 Modo reto</button><button id="bPng">📷 PNG</button><button id="bRst">⟲ Reiniciar</button></div>
<small style="color:var(--mu)">Teclas: ← → pieza · E separar · R giro · Q reto</small></div>
<div class="card" id="quiz" style="display:none"></div>
</aside>
</main>
<div id="toast"></div>
<script src="https://cdn.jsdelivr.net/npm/three@0.128.0/build/three.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/three@0.128.0/examples/js/controls/OrbitControls.js"></script>
<script>
const $=s=>document.querySelector(s);
const sub=f=>f.replace(/(\d+)/g,"<sub>$1</sub>");
const clamp=(v,a,b)=>Math.min(b,Math.max(a,v));
/* Elementos: color, radio visual (molecular), radio iónico en pm, nombre */
const EL={H:[0xf4f4f8,.3,0,"Hidrógeno"],C:[0x555b6a,.5,0,"Carbono"],N:[0x3b6fe0,.5,0,"Nitrógeno"],O:[0xe5383b,.5,140,"Oxígeno"],S:[0xf2c230,.7,0,"Azufre"],Cl:[0x3fbf5a,.7,181,"Cloro"],F:[0x9be564,.42,133,"Flúor"],Na:[0x9a62f0,.62,102,"Sodio"],Mg:[0xf08c2e,.6,72,"Magnesio"],Ca:[0x1fb8a8,.75,100,"Calcio"],Cs:[0xd946b0,.85,167,"Cesio"]};
const T=(h=>{const t=.63;return [[t,t,t],[t,-t,-t],[-t,t,-t],[-t,-t,t]]})();
const FAM=[
{n:"Óxidos básicos",c:"#f2a33a",rule:"Metal + oxígeno → óxido básico. Se cruzan las cargas: el número de oxidación del metal pasa a ser el subíndice del O y viceversa (y se simplifica).",items:[
 {f:"Na2O",s:"Monóxido de disodio",k:"Óxido de sodio",t:"Óxido sódico",cls:"Óxido básico · compuesto iónico",ch:"Na⁺ y O²⁻",how:"Na⁺ + O²⁻ → cruz de cargas → Na₂O¹ → Na₂O. Estructura antifluorita: cada O²⁻ rodeado por 8 Na⁺ y cada Na⁺ por 4 O²⁻.",m:{k:"fluor",a:"O",b:"Na"}},
 {f:"MgO",s:"Monóxido de magnesio",k:"Óxido de magnesio",t:"Óxido magnésico",cls:"Óxido básico · compuesto iónico",ch:"Mg²⁺ y O²⁻",how:"Mg²⁺ + O²⁻ → Mg₂O₂ → se simplifica → MgO. Red tipo NaCl, coordinación 6:6.",m:{k:"rock",a:"Mg",b:"O"}},
 {f:"CaO",s:"Monóxido de calcio",k:"Óxido de calcio",t:"Óxido cálcico (cal viva)",cls:"Óxido básico · compuesto iónico",ch:"Ca²⁺ y O²⁻",how:"Ca²⁺ + O²⁻ → Ca₂O₂ → CaO. Red tipo NaCl, coordinación 6:6.",m:{k:"rock",a:"Ca",b:"O"}}]},
{n:"Óxidos ácidos",c:"#e5383b",rule:"No metal + oxígeno → óxido ácido (anhídrido). Los prefijos griegos (mono, di, tri…) indican cuántos átomos hay.",items:[
 {f:"CO2",s:"Dióxido de carbono",k:"Óxido de carbono(IV)",t:"Anhídrido carbónico",cls:"Óxido ácido · covalente · apolar",ch:"C(+4), O(−2)",how:"C⁴⁺ y O²⁻ → C₂O₄ → simplificar → CO₂. Molécula lineal (180°).",m:{k:"mol",at:[["C",0,0,0],["O",-1.16,0,0],["O",1.16,0,0]],bd:[[0,1,2],[0,2,2]]}},
 {f:"SO2",s:"Dióxido de azufre",k:"Óxido de azufre(IV)",t:"Anhídrido sulfuroso",cls:"Óxido ácido · covalente · polar",ch:"S(+4), O(−2)",how:"S⁴⁺ + O²⁻ → S₂O₄ → SO₂. Molécula angular (≈119°).",m:{k:"mol",at:[["S",0,.36,0],["O",-1.23,-.36,0],["O",1.23,-.36,0]],bd:[[0,1,2],[0,2,1]]}},
 {f:"SO3",s:"Trióxido de azufre",k:"Óxido de azufre(VI)",t:"Anhídrido sulfúrico",cls:"Óxido ácido · covalente · apolar",ch:"S(+6), O(−2)",how:"S⁶⁺ + O²⁻ → S₂O₆ → SO₃. Molécula trigonal plana (120°).",m:{k:"mol",at:[["S",0,0,0],["O",0,1.42,0],["O",-1.23,-.71,0],["O",1.23,-.71,0]],bd:[[0,1,2],[0,2,2],[0,3,2]]}}]},
{n:"Hidróxidos",c:"#3b9bf0",rule:"Óxido básico + agua → hidróxido: metal + grupo OH⁻. El subíndice del OH va entre paréntesis si es mayor que 1.",items:[
 {f:"NaOH",s:"Hidróxido de sodio",k:"Hidróxido de sodio",t:"Hidróxido sódico (soda cáustica)",cls:"Hidróxido · base fuerte · iónico",ch:"Na⁺ y OH⁻",how:"Na⁺ + OH⁻ → NaOH (cargas iguales). El enlace O–H es covalente; Na⁺ se une por atracción iónica.",m:{k:"mol",at:[["Na",-1.9,0,0],["O",.5,0,0],["H",1.46,0,0]],bd:[[1,2,1]]}},
 {f:"Ca(OH)2",s:"Dihidróxido de calcio",k:"Hidróxido de calcio",t:"Hidróxido cálcico (cal apagada)",cls:"Hidróxido · base · iónico",ch:"Ca²⁺ y OH⁻",how:"Ca²⁺ + 2 OH⁻ → Ca(OH)₂. El paréntesis indica que el 2 afecta a todo el grupo OH.",m:{k:"mol",at:[["Ca",0,0,0],["O",-2.35,0,0],["H",-3.3,0,0],["O",2.35,0,0],["H",3.3,0,0]],bd:[[1,2,1],[3,4,1]]}}]},
{n:"Ácidos",c:"#2fc66b",rule:"Hidrácidos: H + no metal (grupos 16 y 17). Oxácidos: óxido ácido + agua. El H va primero en la fórmula.",items:[
 {f:"HCl",s:"Cloruro de hidrógeno",k:"Cloruro de hidrógeno",t:"Ácido clorhídrico (en agua)",cls:"Hidrácido · covalente polar",ch:"H(+1), Cl(−1)",how:"H⁺ + Cl⁻ → HCl. Disuelto en agua se llama ácido clorhídrico.",m:{k:"mol",at:[["Cl",-.4,0,0],["H",.87,0,0]],bd:[[0,1,1]]}},
 {f:"H2S",s:"Sulfuro de dihidrógeno",k:"Sulfuro de hidrógeno",t:"Ácido sulfhídrico (en agua)",cls:"Hidrácido · covalente polar",ch:"H(+1), S(−2)",how:"2 H⁺ + S²⁻ → H₂S. Molécula angular (≈92°).",m:{k:"mol",at:[["S",0,.46,0],["H",-.96,-.46,0],["H",.96,-.46,0]],bd:[[0,1,1],[0,2,1]]}},
 {f:"H2SO4",s:"Tetraoxidosulfato de dihidrógeno",k:"Tetraoxosulfato(VI) de hidrógeno",t:"Ácido sulfúrico",cls:"Oxácido · covalente polar",ch:"H(+1), S(+6), O(−2)",how:"SO₃ + H₂O → H₂SO₄ (óxido ácido + agua). S central tetraédrico: 2 enlaces S=O y 2 grupos S–OH.",m:{k:"mol",at:[["S",0,0,0],["O",.82,.82,.82],["O",.82,-.82,-.82],["O",-.9,.9,-.9],["O",-.9,-.9,.9],["H",-1.4,1.4,-1.4],["H",-1.4,-1.4,1.4]],bd:[[0,1,2],[0,2,2],[0,3,1],[0,4,1],[3,5,1],[4,6,1]]}}]},
{n:"Hidruros no metálicos",c:"#9aa6ff",rule:"H + no metal (grupos 14, 15 y el agua). Tienen nombres comunes aceptados: metano, amoníaco, agua.",items:[
 {f:"H2O",s:"Óxido de dihidrógeno",k:"—",t:"Agua",cls:"Hidruro/óxido · covalente polar",ch:"H(+1), O(−2)",how:"2 H + O → H₂O. Angular (104,5°): por eso es polar y disuelve sales.",m:{k:"mol",at:[["O",0,.3,0],["H",-.76,-.29,0],["H",.76,-.29,0]],bd:[[0,1,1],[0,2,1]]}},
 {f:"NH3",s:"Trihidruro de nitrógeno",k:"—",t:"Amoníaco",cls:"Hidruro · covalente polar",ch:"N(−3), H(+1)",how:"N + 3 H → NH₃. Pirámide trigonal (107°) con un par libre sobre el N.",m:{k:"mol",at:[["N",0,.38,0],["H",.94,0,0],["H",-.47,0,.81],["H",-.47,0,-.81]],bd:[[0,1,1],[0,2,1],[0,3,1]]}},
 {f:"CH4",s:"Tetrahidruro de carbono",k:"—",t:"Metano",cls:"Hidruro · covalente apolar",ch:"C(−4), H(+1)",how:"C + 4 H → CH₄. Tetraedro perfecto (109,5°), simétrico, por eso apolar.",m:{k:"mol",at:[["C",0,0,0],["H",...T[0]],["H",...T[1]],["H",...T[2]],["H",...T[3]]],bd:[[0,1,1],[0,2,1],[0,3,1],[0,4,1]]}}]},
{n:"Sales binarias",c:"#c77dff",rule:"Metal + no metal (sin O). Se nombra primero el no metal con terminación -uro y luego el metal. Forman redes iónicas, no moléculas.",items:[
 {f:"NaCl",s:"Monocloruro de sodio",k:"Cloruro de sodio",t:"Cloruro sódico (sal común)",cls:"Sal binaria · red iónica",ch:"Na⁺ y Cl⁻",how:"Na⁺ + Cl⁻ → NaCl. Red cúbica tipo sal de roca: cada ion tiene 6 vecinos de carga contraria (6:6).",m:{k:"rock",a:"Na",b:"Cl"}},
 {f:"CsCl",s:"Monocloruro de cesio",k:"Cloruro de cesio",t:"Cloruro césico",cls:"Sal binaria · red iónica",ch:"Cs⁺ y Cl⁻",how:"Cs⁺ + Cl⁻ → CsCl. Red cúbica centrada en el cuerpo: coordinación 8:8, el catión es grande.",m:{k:"cscl",a:"Cs",b:"Cl"}},
 {f:"CaF2",s:"Difluoruro de calcio",k:"Fluoruro de calcio",t:"Fluoruro cálcico (fluorita)",cls:"Sal binaria · red iónica",ch:"Ca²⁺ y F⁻",how:"Ca²⁺ + F⁻ → cruz de cargas → CaF₂. Estructura fluorita: Ca²⁺ rodeado por 8 F⁻ y cada F⁻ por 4 Ca²⁺.",m:{k:"fluor",a:"Ca",b:"F"}}]}
];
const ALL=FAM.flatMap((fm,fi)=>fm.items.map(it=>(it.fi=fi,it)));
const st={i:0,rot:true,ex:0,quiz:false,score:0,tries:0};
let R,S,C,CT,G,atoms=[],bonds=[],tw=null;
const cylG=()=>new THREE.CylinderGeometry(1,1,1,10);
function toast(t){const e=$("#toast");e.textContent=t;e.style.opacity=1;clearTimeout(toast.h);toast.h=setTimeout(()=>e.style.opacity=0,1400)}
function clearG(){while(G.children.length){const o=G.children.pop();o.geometry&&o.geometry.dispose();o.material&&o.material.dispose()}atoms=[];bonds=[]}
function addAtom(el,x,y,z,r){const m=new THREE.Mesh(new THREE.SphereGeometry(r,28,20),new THREE.MeshStandardMaterial({color:EL[el][0],roughness:.35,metalness:.15}));m.position.set(x,y,z);m.castShadow=true;G.add(m);atoms.push({m,b:new THREE.Vector3(x,y,z),el});return atoms.length-1}
function addBond(a,b,o,rad){const ms=[];for(let k=0;k<o;k++){const m=new THREE.Mesh(cylG(),new THREE.MeshStandardMaterial({color:0xcfd3e6,roughness:.4,metalness:.3}));m.userData.r=rad*(o>1?.7:1);G.add(m);ms.push(m)}bonds.push({a,b,o,ms})}
function layout(){const e=1+st.ex*1.1;atoms.forEach(a=>a.m.position.copy(a.b).multiplyScalar(e));
 const Y=new THREE.Vector3(0,1,0);
 bonds.forEach(bd=>{const pa=atoms[bd.a].m.position,pb=atoms[bd.b].m.position,d=pb.clone().sub(pa),L=d.length(),u=d.clone().normalize();
 const p=new THREE.Vector3().crossVectors(u,Math.abs(u.z)<.9?new THREE.Vector3(0,0,1):new THREE.Vector3(1,0,0)).normalize();
 bd.ms.forEach((m,k)=>{const off=bd.o>1?(k-(bd.o-1)/2)*.2:0;m.position.copy(pa).add(pb).multiplyScalar(.5).addScaledVector(p,off);m.scale.set(m.userData.r,L,m.userData.r);m.quaternion.setFromUnitVectors(Y,u);m.visible=st.ex<.98})})}
/* Redes iónicas: posiciones en coordenadas de celda; tipo 0 = a, tipo 1 = b */
function latt(m){const P=[];let sc,dn;
 if(m.k==="rock"){sc=1.7;dn=1;for(let i=0;i<4;i++)for(let j=0;j<4;j++)for(let k=0;k<4;k++)P.push([(i+j+k)%2,i-1.5,j-1.5,k-1.5])}
 else if(m.k==="cscl"){sc=2.5;dn=.866;for(let i=0;i<3;i++)for(let j=0;j<3;j++)for(let k=0;k<3;k++)P.push([0,i-1,j-1,k-1]);for(let i=0;i<2;i++)for(let j=0;j<2;j++)for(let k=0;k<2;k++)P.push([1,i-.5,j-.5,k-.5])}
 else{sc=4.2;dn=.433;const c=[0,1],f=[[.5,.5,0],[.5,.5,1],[.5,0,.5],[.5,1,.5],[0,.5,.5],[1,.5,.5]];c.forEach(i=>c.forEach(j=>c.forEach(k=>P.push([0,i-.5,j-.5,k-.5]))));f.forEach(q=>P.push([0,q[0]-.5,q[1]-.5,q[2]-.5]));[.25,.75].forEach(i=>[.25,.75].forEach(j=>[.25,.75].forEach(k=>P.push([1,i-.5,j-.5,k-.5]))))}
 const els=[m.a,m.b],rs=[EL[m.a][2],EL[m.b][2]],idx=P.map(p=>addAtom(els[p[0]],p[1]*sc,p[2]*sc,p[3]*sc,.72*dn*sc*rs[p[0]]/(rs[0]+rs[1])));
 for(let i=0;i<P.length;i++)for(let j=i+1;j<P.length;j++){if(P[i][0]===P[j][0])continue;const d=Math.hypot(P[i][1]-P[j][1],P[i][2]-P[j][2],P[i][3]-P[j][3]);if(Math.abs(d-dn)<.02)addBond(idx[i],idx[j],1,.05)}}
function build(){clearG();const it=ALL[st.i],m=it.m;
 if(m.k==="mol"){m.at.forEach(a=>addAtom(a[0],a[1],a[2],a[3],EL[a[0]][1]));m.bd.forEach(b=>addBond(b[0],b[1],b[2],.09))}else latt(m);
 const bx=new THREE.Box3().setFromObject(G),sz=bx.getSize(new THREE.Vector3()).length();
 const dist=Math.max(7,sz*1.5);tw={t:0,from:S.position.clone(),to:new THREE.Vector3(dist*.55,dist*.4,dist*.8)};
 layout();legend();badge();info();
}
function legend(){const s=[...new Set(atoms.map(a=>a.el))];$("#legend").innerHTML=s.map(e=>`<span><i style="background:#${EL[e][0].toString(16).padStart(6,"0")}"></i>${e} · ${EL[e][3]}</span>`).join("")}
function badge(){const it=ALL[st.i],fm=FAM[it.fi];$("#badge").innerHTML=`<b>${st.quiz?"¿?":sub(it.f)}</b><span style="background:${fm.c}">${fm.n}</span>`}
function info(){const it=ALL[st.i],fm=FAM[it.fi];
 document.querySelectorAll(".chip").forEach(c=>c.classList.toggle("on",+c.dataset.i===st.i));
 if(st.quiz){$("#info").innerHTML=`<h3>🔍 Pieza misteriosa</h3><p>Observa el modelo y la sala en la que está: <b>${fm.n}</b>. ¿Qué compuesto es?</p>`;quiz();return}
 $("#quiz").style.display="none";
 $("#info").innerHTML=`<h3>${sub(it.f)} · ${fm.n}</h3><dl><dt>Sistemática</dt><dd>${it.s}</dd><dt>Stock</dt><dd>${it.k}</dd><dt>Tradicional</dt><dd>${it.t}</dd><dt>Clasificación</dt><dd>${it.cls}</dd><dt>Iones / n.º ox.</dt><dd>${it.ch}</dd></dl><div class="how"><b>Cómo se formula:</b> ${it.how}</div><div class="how"><b>Regla de la sala:</b> ${fm.rule}</div>`}
function quiz(){const q=$("#quiz"),it=ALL[st.i];q.style.display="block";
 const pool=ALL.filter(x=>x!==it&&x.fi===it.fi),oth=ALL.filter(x=>x!==it&&x.fi!==it.fi).sort(()=>Math.random()-.5);
 const opts=[it,...pool.sort(()=>Math.random()-.5).slice(0,2),...oth].slice(0,4).sort(()=>Math.random()-.5);
 q.innerHTML=`<h3>🎯 Reto · aciertos ${st.score}/${st.tries}</h3>${opts.map(o=>`<button class="opt" data-f="${o.f}">${sub(o.f)} — ${o.t}</button>`).join("")}<div id="fb"></div>`;
 q.querySelectorAll(".opt").forEach(b=>b.onclick=()=>{if(q.dataset.done===it.f)return;q.dataset.done=it.f;st.tries++;const ok=b.dataset.f===it.f;if(ok)st.score++;
 b.classList.add(ok?"good":"bad");q.querySelectorAll(".opt").forEach(x=>{if(x.dataset.f===it.f)x.classList.add("good")});
 $("#fb").innerHTML=`<p>${ok?"✅ ¡Correcto!":"❌ Era "+sub(it.f)}</p><p><b>${it.s}</b> / ${it.k}<br>${it.how}</p><button id="nx">Siguiente ▶</button>`;
 $("#badge b").innerHTML=sub(it.f);$("#nx").onclick=()=>{q.dataset.done="";go(Math.floor(Math.random()*ALL.length))}})}
function go(i){st.i=(i+ALL.length)%ALL.length;st.ex=0;$("#ex").value=0;$("#exv").textContent="0%";build()}
function rooms(){$("#rooms").innerHTML=FAM.map((f,fi)=>`<div class="room"><button style="border-left:5px solid ${f.c}" data-r="${fi}">${f.n}</button><div class="chips">${ALL.map((it,i)=>it.fi===fi?`<button class="chip" data-i="${i}">${sub(it.f)}</button>`:"").join("")}</div></div>`).join("");
 $("#rooms").querySelectorAll(".chip").forEach(c=>c.onclick=()=>{st.quiz=false;$("#bQuiz").classList.remove("on");go(+c.dataset.i)});
 $("#rooms").querySelectorAll("[data-r]").forEach(b=>b.onclick=()=>{st.quiz=false;$("#bQuiz").classList.remove("on");go(ALL.findIndex(x=>x.fi===+b.dataset.r))})}
function setQuiz(v){st.quiz=v;$("#bQuiz").classList.toggle("on",v);if(v){st.score=0;st.tries=0;go(Math.floor(Math.random()*ALL.length));toast("Modo reto: adivina el compuesto")}else{$("#quiz").style.display="none";badge();info()}}
function setEx(v){st.ex=clamp(v,0,1);$("#ex").value=Math.round(st.ex*100);$("#exv").textContent=Math.round(st.ex*100)+"%";layout()}
function init(){
 if(!window.THREE||!THREE.OrbitControls)throw new Error("three");
 const cv=$("#cv");R=new THREE.WebGLRenderer({canvas:cv,antialias:true,preserveDrawingBuffer:true});R.setPixelRatio(Math.min(devicePixelRatio,2));R.shadowMap.enabled=true;
 S=new THREE.PerspectiveCamera(42,1,.1,200);S.position.set(5,4,8);
 C=new THREE.Scene();C.fog=new THREE.Fog(0x0b0918,14,40);
 C.add(new THREE.HemisphereLight(0xcfc4ff,0x1a1236,.9));
 const k=new THREE.DirectionalLight(0xffffff,1.1);k.position.set(6,9,7);k.castShadow=true;C.add(k);
 const rim=new THREE.PointLight(0x3ed6d0,.9,40);rim.position.set(-8,3,-6);C.add(rim);
 const fl=new THREE.Mesh(new THREE.CircleGeometry(9,48),new THREE.MeshStandardMaterial({color:0x2a2150,roughness:.6,metalness:.3}));fl.rotation.x=-Math.PI/2;fl.position.y=-4;fl.receiveShadow=true;C.add(fl);
 G=new THREE.Group();C.add(G);
 CT=new THREE.OrbitControls(S,cv);CT.enableDamping=true;CT.dampingFactor=.08;CT.minDistance=3;CT.maxDistance=30;
 const rs=()=>{const w=$("#stage").clientWidth,h=$("#stage").clientHeight;R.setSize(w,h,false);S.aspect=w/h;S.updateProjectionMatrix()};
 new ResizeObserver(rs).observe($("#stage"));rs();
 R.setAnimationLoop(()=>{if(tw){tw.t=Math.min(1,tw.t+.03);const e=tw.t*tw.t*(3-2*tw.t);S.position.lerpVectors(tw.from,tw.to,e);if(tw.t>=1)tw=null}
 CT.autoRotate=st.rot;CT.autoRotateSpeed=1.6;CT.update();R.render(C,S)});
 rooms();build();
}
$("#ex").oninput=e=>setEx(e.target.value/100);
$("#bRot").onclick=()=>{st.rot=!st.rot;$("#bRot").classList.toggle("on",st.rot)};
$("#bQuiz").onclick=()=>setQuiz(!st.quiz);
$("#bPng").onclick=()=>{const a=document.createElement("a");a.download="museo-"+ALL[st.i].f+".png";a.href=R.domElement.toDataURL("image/png");a.click();toast("Imagen guardada")};
$("#bRst").onclick=()=>{st.rot=true;$("#bRot").classList.add("on");st.quiz=false;$("#bQuiz").classList.remove("on");go(0);toast("Museo reiniciado")};
addEventListener("keydown",e=>{if(e.target.tagName==="INPUT")return;const k=e.key.toLowerCase();
 if(e.key==="ArrowRight")go(st.i+1);else if(e.key==="ArrowLeft")go(st.i-1);else if(k==="e")setEx(st.ex>.5?0:1);else if(k==="r")$("#bRot").click();else if(k==="q")$("#bQuiz").click()});
try{init()}catch(e){$("#err").style.display="grid"}
</script>
</body>
</html>
