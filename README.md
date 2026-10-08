<!DOCTYPE html>
<html lang="it">

<head>

<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Gestione Turni Reparto</title>

<style>

body{
font-family:Arial;
max-width:1200px;
margin:auto;
padding:20px;
background:#f5f5f5;
}

h1{
text-align:center;
color:#0078d4;
}

.card{
background:white;
padding:20px;
border-radius:10px;
margin-bottom:20px;
box-shadow:0px 2px 6px rgba(0,0,0,0.2);
}

table{
width:100%;
border-collapse:collapse;
background:white;
}

th{
background:#0078d4;
color:white;
padding:10px;
}

td{
border:1px solid #ccc;
padding:8px;
text-align:center;
}

input{
padding:6px;
width:95%;
}

button{
background:#0078d4;
color:white;
border:none;
padding:10px 20px;
border-radius:5px;
cursor:pointer;
margin-top:10px;
}

button:hover{
background:#005fa3;
}

</style>

</head>

<body>

<h1>PROGRAMMA TURNI REPARTO</h1>

<div class="card">

<h2>Periodo</h2>

Data Inizio

<input type="date" id="dataInizio">

<br><br>

Data Fine

<input type="date" id="dataFine">

</div>

<div class="card">

<h2>Assistente Settimanale</h2>

<input type="text" id="assistente" placeholder="Nome Assistente">

</div>

<div class="card">

<h2>Configurazione Gruppi</h2>

<table>

<tr>
<th>Gruppo</th>
<th>Nome</th>
</tr>

<tr><td>G1</td><td><input id="g1"></td></tr>
<tr><td>G2</td><td><input id="g2"></td></tr>
<tr><td>G3</td><td><input id="g3"></td></tr>
<tr><td>G4</td><td><input id="g4"></td></tr>
<tr><td>G5</td><td><input id="g5"></td></tr>
<tr><td>G6</td><td><input id="g6"></td></tr>
<tr><td>G7</td><td><input id="g7"></td></tr>
<tr><td>G8</td><td><input id="g8"></td></tr>

</table>

<button onclick="salvaGruppi()">Salva Nomi</button>

</div>

<div class="card">

<button onclick="generaTurni()">
Genera Calendario
</button>

<button onclick="window.print()">
Stampa Bacheca
</button>

</div>

<div class="card">

<table>

<thead>

<tr>
<th>Settimana</th>
<th>Assistente</th>
<th>Giovedì</th>
<th>Sabato</th>
<th>Domenica</th>
</tr>

</thead>

<tbody id="tabellaTurni">

</tbody>

</table>

</div>

<script>

function nomeGruppo(numero){

let nome=document.getElementById("g"+numero).value;

if(nome.trim()!=""){
return "G"+numero+" - "+nome;
}

return "G"+numero;

}

function salvaGruppi(){

for(let i=1;i<=8;i++){

localStorage.setItem(
"g"+i,
document.getElementById("g"+i).value
);

}

alert("Configurazione salvata");

}

window.onload=function(){

for(let i=1;i<=8;i++){

let dato=localStorage.getItem("g"+i);

if(dato){
document.getElementById("g"+i).value=dato;
}

}

};

function generaTurni(){

let html="";

const gruppi=[1,2,3,4,5,6,7,8];

let assistente=
document.getElementById("assistente").value;

for(let settimana=1; settimana<=8; settimana++){

let g1=gruppi[(settimana-1)%8];
let g2=gruppi[(settimana+2)%8];

let d1=gruppi[(settimana)%8];
let d2=gruppi[(settimana+3)%8];

let sabato="-";

if(settimana % 2 != 0){

let s1=gruppi[(settimana+1)%8];
let s2=gruppi[(settimana+4)%8];

sabato=
nomeGruppo(s1)+"<br>"+
nomeGruppo(s2);

}

html += `
<tr>

<td>${settimana}</td>

<td>${assistente}</td>

<td>
${nomeGruppo(g1)}<br>
${nomeGruppo(g2)}
</td>

<td>
${sabato}
</td>

<td>
${nomeGruppo(d1)}<br>
${nomeGruppo(d2)}
</td>

</tr>
`;

}

document.getElementById("tabellaTurni").innerHTML=html;

}

</script>

</body>
</html>
