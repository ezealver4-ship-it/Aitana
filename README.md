# Aitana
El mundo de aitana
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Universo de Luna Aitana</title>

<style>
body{
margin:0;
overflow:hidden;
background:black;
font-family:Arial, sans-serif;
color:white;
}

/* FONDO ESTRELLAS */
#estrellas{
position:fixed;
width:100%;
height:100%;
background:black;
overflow:hidden;
z-index:0;
}

.estrella{
position:absolute;
width:2px;
height:2px;
background:white;
border-radius:50%;
opacity:0.8;
animation:parpadeo 2s infinite alternate;
}

@keyframes parpadeo{
from{opacity:0.3;}
to{opacity:1;}
}

/* PANTALLA INICIO */
#inicio{
position:fixed;
width:100%;
height:100%;
display:flex;
flex-direction:column;
justify-content:center;
align-items:center;
z-index:10;
background:rgba(0,0,0,0.9);
text-align:center;
}

#inicio h1{
font-size:22px;
margin-bottom:20px;
}

#abrirBtn{
padding:15px 25px;
font-size:18px;
border:none;
border-radius:30px;
background:#ff4da6;
color:white;
}

/* UNIVERSO */
#universo{
position:absolute;
width:100%;
height:100%;
display:none;
overflow:hidden;
touch-action:none;
z-index:1;
}

/* NOMBRE CENTRAL ORBITANDO */
#nombreOrbita{
position:absolute;
top:50%;
left:50%;
transform:translate(-50%,-50%);
width:200px;
height:200px;
animation:rotar 20s linear infinite;
}

#nombreOrbita span{
position:absolute;
width:100%;
text-align:center;
top:0;
font-size:18px;
color:#ffccff;
}

/* ORBITAS FOTOS */
.orbita{
position:absolute;
top:50%;
left:50%;
transform-origin:center center;
animation:rotar linear infinite;
}

.foto{
position:absolute;
width:90px;
height:90px;
border-radius:50%;
object-fit:cover;
cursor:pointer;
}

/* MODAL */
#modal{
position:fixed;
top:0;
left:0;
width:100%;
height:100%;
background:rgba(0,0,0,0.95);
display:none;
justify-content:center;
align-items:center;
z-index:20;
}

#modal img{
max-width:90%;
max-height:90%;
border-radius:20px;
}

@keyframes rotar{
from{transform:rotate(0deg);}
to{transform:rotate(360deg);}
}
</style>
</head>

<body>

<div id="estrellas"></div>

<div id="inicio">
<h1>¿Quieres conocer mi universo?</h1>
<button id="abrirBtn">Tocar para abrir</button>
</div>

<div id="universo">

<div id="nombreOrbita">
<span>Luna Aitana</span>
</div>

</div>

<div id="modal" onclick="cerrarModal()">
<img id="imgGrande">
</div>

<audio id="musica" src="musica.mp3"></audio>

<script>

/* CREAR ESTRELLAS */
const estrellas = document.getElementById("estrellas");
for(let i=0;i<200;i++){
let star=document.createElement("div");
star.className="estrella";
star.style.top=Math.random()*100+"%";
star.style.left=Math.random()*100+"%";
star.style.animationDuration=(1+Math.random()*3)+"s";
estrellas.appendChild(star);
}

const fotos=[
"foto1.jpg","foto2.jpg","foto3.jpg",
"foto4.jpg","foto5.jpg","foto6.jpg",
"foto7.jpg","foto8.jpg","foto9.jpg"
];

const universo=document.getElementById("universo");

/* CREAR ORBITAS */
fotos.forEach((src,i)=>{
let orbita=document.createElement("div");
orbita.className="orbita";
orbita.style.width=(220 + i*40)+"px";
orbita.style.height=(220 + i*40)+"px";
orbita.style.marginLeft=-(110 + i*20)+"px";
orbita.style.marginTop=-(110 + i*20)+"px";
orbita.style.animationDuration=(25 + i*5)+"s";

let img=document.createElement("img");
img.src=src;
img.className="foto";
img.style.top="0px";
img.style.left="50%";
img.style.transform="translateX(-50%)";

img.onclick=()=>abrirModal(src);

orbita.appendChild(img);
universo.appendChild(orbita);
});

/* ABRIR UNIVERSO */
document.getElementById("abrirBtn").onclick=()=>{
document.getElementById("inicio").style.display="none";
universo.style.display="block";
document.getElementById("musica").play();
};

/* MODAL */
function abrirModal(src){
document.getElementById("imgGrande").src=src;
document.getElementById("modal").style.display="flex";
}

function cerrarModal(){
document.getElementById("modal").style.display="none";
}

/* ZOOM */
let scale=1;
let startDist=0;

universo.addEventListener("touchstart",e=>{
if(e.touches.length==2){
startDist=getDist(e);
}
});

universo.addEventListener("touchmove",e=>{
if(e.touches.length==2){
let newDist=getDist(e);
scale*=newDist/startDist;
universo.style.transform=`scale(${scale})`;
startDist=newDist;
}
});

function getDist(e){
let dx=e.touches[0].clientX - e.touches[1].clientX;
let dy=e.touches[0].clientY - e.touches[1].clientY;
return Math.sqrt(dx*dx + dy*dy);
}

</script>

</body>
</html>
