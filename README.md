<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <link rel="stylesheet" href="styles.css">
    <title>Drawing app</title>

</head>
<body>
    <section class="container">
        <div id="toolbar">
            <h1>Draw.</h1>
            <label for="stroke">Stroke</label>
            <input id="stroke" mame="stroke" type="'color">
            <label for="lineWidth">Line Width</label>
            <input id="linewidth" name="lineWidth" type="number" value="5">
            <button id="clear">Clear</button>
        </div>
        <div class="drawing-board">
            <canvas id="drawing-board"></canvas>
        </div>
    </section>
    <script src="./index.js"></script>
</body>
</html>

body{
    margin: 0;
    padding: 0;
    height: 100%;
    overflow: hidden;
    color: white;
}

h1{
    background: #7F7FD5;
    background: -webkit-linear-gradient(to right, #91EAE4,#86A8E7,#7F7FD5);
    background: linear-gradient(to right,#91EAE4,#86A8E7,#7F7FD5);
    background-clip: text;
    -webkit-background-clip: text;
    -webkit-text-fill-color:transparent;

}
.container{
    height: 100%;
    display:flex;
}
#toolbar{
    display:flex;
    flex-direction: column;
    padding:5px;
    width: 70px;
    background-color: #202020;
}
#toolbar * {
    margin-bottom: 6px;
}
#toolbar label {
    font-size: 12px;
}

#toolbar input{
    width: 100%;
}

#toolbar button {
    background-color: #1565c0;
    border: none;
    border-radius: 4px;
    color:white;
    padding: 2px;
}


const canvas = document.getElementById('drawing-board');
const toolbar = document.getElementById('toolbar');
const ctx = canvas.getContext('2d');

const canvasOffsetX= canvas.offsetLeft;
const canvasOffsetY= canvas.offsetTop;

canvas.width= window.innerWidth- canvasOffsetX;
canvas.height= window.innerHeight- canvasOffsetY;

let isPainting= false;
let lineWidth = 5;
let startX;
let startY;

toolbar.addEventListener('click',e =>{
    if (e.target.id === 'clear') {
        ctx.clearRect(0,0, canvas.width, canvas.height);
    }
});

toolbar.addEventListener('change', e=> {
    if(e.target.id === 'stroke'){
        ctx.strokeStyle = e.target.value;
    }

    if(e.target.id === 'lineWidth'){
        lineWidth=e.target.value;
    }
}); 

const draw = (e)=>{
    if (!isPainting){
        return;
    }
    ctx.lineWidth= lineWidth;
    ctx.lineCap = 'round';
    ctx.lineTo(e.clientX-canvasOffsetX,e.clientY);
    ctx.stroke();    
}

canvas.addEventListener('mousedown',(e)=>{
    isPainting = true;
    startX= e.clientX;
    startY= e.clientY;
});

canvas.addEventListener('mouseup',e=>{
    isPainting=false;
    ctx.stroke();
    ctx.beginPath();
});

canvas.addEventListener('mousemove', draw);









