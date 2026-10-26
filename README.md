# smart-notes-app
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Smart Notes</title>
<style>
*{box-sizing:border-box}
body{margin:0;background:#111421;color:#f3f4ff;font-family:Arial;padding:24px}
main{max-width:950px;margin:auto}
header{display:flex;justify-content:space-between;align-items:center;gap:12px;flex-wrap:wrap}
input,textarea{width:100%;padding:13px;background:#1a2235;color:white;border:1px solid #394560;border-radius:9px;margin:8px 0}
button{padding:10px 14px;background:#8c82ff;color:white;border:0;border-radius:8px;cursor:pointer}
.grid{display:grid;grid-template-columns:repeat(auto-fit,minmax(220px,1fr));gap:14px;margin-top:20px}
.note{padding:18px;border-radius:13px;background:#252d43;border:1px solid #394560;overflow-wrap:anywhere}
.note small{color:#aeb9d3}
</style>
</head>
<body>
<main>
<header><div><p>PERSONAL WORKSPACE</p><h1>Smart Notes ✎</h1></div><input id="search" placeholder="Search notes..." style="max-width:280px"></header>
<form id="form"><input id="title" placeholder="Note title" required><textarea id="body" rows="3" placeholder="Write your idea..." required><\/textarea><button>Save Note</button></form>
<p id="count"></p><section class="grid" id="list"></section>
</main>
<script>
const $=id=>document.getElementById(id);
let notes=JSON.parse(localStorage.smartNotes||"[]");
function esc(s){return String(s).replace(/[&<>"']/g,c=>({"&":"&amp;","<":"&lt;",">":"&gt;",'"':"&quot;","'":"&#39;"}[c]))}
function draw(){
const q=$("search").value.toLowerCase();
const filtered=notes.filter(n=>(n.title+" "+n.body).toLowerCase().includes(q));
$("count").textContent=notes.length+" saved notes";
$("list").innerHTML="";
filtered.forEach(n=>{
const article=document.createElement("article");article.className="note";
const date=document.createElement("small");date.textContent=n.date;
const h=document.createElement("h3");h.textContent=n.title;
const p=document.createElement("p");p.style.whiteSpace="pre-wrap";p.textContent=n.body;
const b=document.createElement("button");b.textContent="Delete";
b.onclick=()=>{notes=notes.filter(x=>x.id!==n.id);draw()};
article.append(date,h,p,b);$("list").appendChild(article);
});
if(!filtered.length)$("list").textContent="No matching notes.";
localStorage.smartNotes=JSON.stringify(notes);
}
$("form").onsubmit=e=>{
e.preventDefault();
notes.unshift({id:Date.now(),title:$("title").value,body:$("body").value,date:new Date().toLocaleDateString()});
$("title").value="";$("body").value="";draw();
};
$("search").oninput=draw;
draw();
</script>
</body>
</html>
