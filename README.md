<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Inmobiliaria PRO</title>

<style>

body{
margin:0;
font-family:Arial;
background:#f3f4f6;
transition:0.3s;
}

.dark{
background:#111827;
color:white;
}

header{
background:#1f2937;
color:white;
padding:15px;
text-align:center;
font-size:22px;
}

.container{
padding:20px;
}

input,button{
padding:10px;
border-radius:6px;
border:1px solid #ccc;
width:100%;
margin-top:5px;
}

button{
background:#16a34a;
color:white;
border:none;
cursor:pointer;
}

.grid{
display:grid;
grid-template-columns:repeat(auto-fit,minmax(220px,1fr));
gap:15px;
margin-top:20px;
}

.card{
background:white;
border-radius:10px;
box-shadow:0 4px 10px rgba(0,0,0,0.2);
overflow:hidden;
}

.dark .card{
background:#1f2937;
color:white;
}

.card img{
width:100%;
height:150px;
object-fit:cover;
}

.info{
padding:10px;
}

.precio{
color:#16a34a;
font-weight:bold;
}

.fav{
cursor:pointer;
color:red;
font-size:20px;
}

.section{
background:white;
padding:20px;
border-radius:10px;
margin-top:30px;
box-shadow:0 4px 10px rgba(0,0,0,0.2);
}

.dark .section{
background:#1f2937;
}

</style>
</head>

<body>

<header>
🏠 Inmobiliaria PRO
<button onclick="modoOscuro()">🌙</button>
</header>

<div class="container" id="login">

<h2>Login</h2>

<input id="user" placeholder="Usuario">
<input id="pass" type="password" placeholder="Contraseña">

<button onclick="login()">Entrar</button>
<button onclick="registro()">Registrarse</button>

</div>

<div class="container" id="app" style="display:none">

<input id="buscar" placeholder="Buscar casa..." onkeyup="mostrar()">

<div class="grid" id="lista"></div>

<div class="section">

<h3>💳 Simulador de crédito</h3>

<input id="precio" type="number" placeholder="Precio de la casa">
<input id="inicial" type="number" placeholder="Cuota inicial">
<input id="anos" type="number" placeholder="Años">
<input id="interes" type="number" placeholder="Interés anual (%)">

<button onclick="simular()">Calcular</button>

<h3 id="resultado"></h3>

</div>

<div class="section">

<h3>❤️ Favoritas</h3>

<div id="favoritas"></div>

</div>

<div class="section">

<h3>🏢 Publicar propiedad</h3>

<input id="nombreCasa" placeholder="Nombre">
<input id="precioCasa" type="number" placeholder="Precio">

<button onclick="agregarCasa()">Publicar</button>

</div>

<div class="section">

<h3>🗺 Ubicación</h3>

<iframe
width="100%"
height="250"
src="https://maps.google.com/maps?q=colombia&t=&z=5&ie=UTF8&iwloc=&output=embed">
</iframe>

</div>

</div>

<script>

let propiedades=JSON.parse(localStorage.getItem("propiedades"))||[];

if(propiedades.length===0){

for(let i=1;i<=20;i++){

propiedades.push({
id:i,
nombre:"Casa "+i,
precio:Math.floor(Math.random()*800000)+100000,
img:"https://picsum.photos/400/300?random="+i
});

}

}

let favoritas=[];

function login(){

document.getElementById("login").style.display="none";
document.getElementById("app").style.display="block";

mostrar();

}

function registro(){

alert("Usuario registrado (simulación)");

}

function mostrar(){

const texto=document.getElementById("buscar").value.toLowerCase();

const cont=document.getElementById("lista");

cont.innerHTML="";

propiedades
.filter(p=>p.nombre.toLowerCase().includes(texto))
.forEach(p=>{

cont.innerHTML+=`

<div class="card">

<img src="${p.img}">

<div class="info">

<h4>${p.nombre}</h4>

<div class="precio">$${p.precio}</div>

<div class="fav" onclick="fav(${p.id})">❤️</div>

</div>

</div>

`;

});

}

function fav(id){

const casa=propiedades.find(p=>p.id==id);

if(!favoritas.includes(casa)) favoritas.push(casa);

mostrarFav();

}

function mostrarFav(){

const cont=document.getElementById("favoritas");

cont.innerHTML="";

favoritas.forEach(f=>{

cont.innerHTML+=`<p>${f.nombre} - $${f.precio}</p>`;

});

}

function simular(){

const precio=parseFloat(document.getElementById("precio").value);

const inicial=parseFloat(document.getElementById("inicial").value);

const anos=parseFloat(document.getElementById("anos").value);

const interes=parseFloat(document.getElementById("interes").value)/100;

const prestamo=precio-inicial;

const meses=anos*12;

const tasa=interes/12;

const pago=(prestamo*tasa)/(1-Math.pow(1+tasa,-meses));

document.getElementById("resultado").innerText=
"Pago mensual estimado: $"+pago.toFixed(2);

}

function agregarCasa(){

const nombre=document.getElementById("nombreCasa").value;

const precio=document.getElementById("precioCasa").value;

const nueva={
id:Date.now(),
nombre:nombre,
precio:precio,
img:"https://picsum.photos/400/300?random="+Math.random()
};

propiedades.push(nueva);

localStorage.setItem("propiedades",JSON.stringify(propiedades));

mostrar();

}

function modoOscuro(){

document.body.classList.toggle("dark");

}

</script>

</body>
</html>
