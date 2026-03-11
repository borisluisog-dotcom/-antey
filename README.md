santi
Finances
<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<title>Finanzas Inteligentes</title>

<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

<style>

body{
font-family:Arial;
background:#eef2f7;
padding:20px;
}

.container{
max-width:600px;
margin:auto;
background:white;
padding:20px;
border-radius:12px;
box-shadow:0 10px 25px rgba(0,0,0,0.1);
}

h1{
text-align:center;
}

input,select,button{
width:100%;
padding:10px;
margin-top:10px;
border-radius:6px;
border:1px solid #ddd;
}

button{
background:#2563eb;
color:white;
border:none;
font-weight:bold;
cursor:pointer;
}

.transaction{
display:flex;
justify-content:space-between;
padding:8px;
border-bottom:1px solid #eee;
}

.summary{
margin-top:15px;
padding:10px;
background:#f1f5f9;
border-radius:8px;
}

.chart{
margin-top:25px;
}

.quick{
display:flex;
gap:8px;
margin-top:10px;
}

.quick button{
flex:1;
background:#e2e8f0;
color:#111;
}

</style>

</head>

<body>

<div class="container">

<h1>💰 Finanzas AI</h1>

<input id="amount" type="number" placeholder="Monto">

<select id="type">
<option value="expense">Gasto</option>
<option value="income">Ingreso</option>
</select>

<input id="category" placeholder="Categoría">

<div class="quick">
<button onclick="setCategory('Comida')">🍔</button>
<button onclick="setCategory('Transporte')">🚗</button>
<button onclick="setCategory('Casa')">🏠</button>
<button onclick="setCategory('Ocio')">🎉</button>
</div>

<button onclick="addTransaction()">Agregar</button>

<div id="list"></div>

<div class="summary">

<p>Ingresos: $<span id="income">0</span></p>
<p>Gastos: $<span id="expense">0</span></p>
<p><b>Balance: $<span id="balance">0</span></b></p>

</div>

<div class="chart">
<canvas id="chart"></canvas>
</div>

</div>

<script>

let transactions = JSON.parse(localStorage.getItem("transactions")) || []

let chart

function setCategory(name){
document.getElementById("category").value=name
}

function save(){
localStorage.setItem("transactions",JSON.stringify(transactions))
}

function addTransaction(){

let amount = Number(document.getElementById("amount").value)
let type = document.getElementById("type").value
let category = document.getElementById("category").value || "Otros"

if(!amount)return

transactions.push({amount,type,category})

save()

render()

}

function render(){

let list=document.getElementById("list")

list.innerHTML=""

let income=0
let expense=0
let categories={}

transactions.forEach(t=>{

let div=document.createElement("div")

div.className="transaction"

div.innerHTML=`
<span>${t.category}</span>
<span>${t.type==="income" ? "+" : "-"}$${t.amount}</span>
`

list.appendChild(div)

if(t.type==="income") income+=t.amount
else{

expense+=t.amount

if(!categories[t.category]) categories[t.category]=0
categories[t.category]+=t.amount

}

})

document.getElementById("income").innerText=income
document.getElementById("expense").innerText=expense
document.getElementById("balance").innerText=income-expense

drawChart(categories)

}

function drawChart(data){

let ctx=document.getElementById("chart")

if(chart) chart.destroy()

chart=new Chart(ctx,{
type:"pie",
data:{
labels:Object.keys(data),
datasets:[{
data:Object.values(data)
}]
}
})

}

render()

</script>

</body>
</html>
