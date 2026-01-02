<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Fake Tweet Generator</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<script src="https://cdn.jsdelivr.net/npm/html2canvas@1.4.1/dist/html2canvas.min.js"></script>

<style>
:root{
  --card:#000;
  --text:#e7e9ea;
  --muted:#71767b;
}

body{
  margin:0;
  background:#0f1419;
  font-family:Arial,Helvetica,sans-serif;
  color:var(--text);
  display:flex;
  flex-direction:column;
  align-items:center;
}

/* CONTROLS */
.controls{
  width:95%;
  max-width:520px;
  background:#16181c;
  padding:14px;
  border-radius:12px;
  margin-top:14px;
}

.controls input,
.controls textarea,
.controls select,
.controls button{
  width:100%;
  margin-top:8px;
  padding:10px;
  border-radius:6px;
  border:none;
  background:#0f1419;
  color:#fff;
  font-size:14px;
}

.controls button{
  background:#1d9bf0;
  font-weight:bold;
}

/* TWEET */
#tweet{
  width:500px;
  max-width:95vw;
  background:var(--card);
  border-radius:16px;
  padding:16px;
  margin:20px 0;
}

/* CONTENT */
.header{
  display:flex;
}

.pfp{
  width:48px;
  height:48px;
  border-radius:50%;
  object-fit:cover;
  background:#999;
}

.info{
  margin-left:10px;
}

.name{
  font-weight:bold;
  display:flex;
  align-items:center;
  gap:4px;
}

.badge{
  width:16px;
  height:16px;
  display:none;
}

.username,
.footer,
.stats{
  color:var(--muted);
  font-size:14px;
}

.text{
  margin-top:12px;
  font-size:16px;
  line-height:1.45;
  white-space:pre-wrap;
}

.stats{
  margin-top:10px;
  display:flex;
  gap:16px;
}
</style>
</head>

<body>

<div class="controls">
  <input type="file" id="img">
  <input type="text" id="nameI" value="Elon Musk">
  <input type="text" id="userI" value="@elonmusk">
  <textarea id="textI">This is a fake tweet generated using an APK-safe generator.</textarea>
  <input type="text" id="dateI" value="10:45 AM · Jan 2, 2026">

  <input type="text" id="rI" value="1.2K Replies">
  <input type="text" id="rtI" value="8.6K Retweets">
  <input type="text" id="lI" value="42K Likes">

  <select id="theme">
    <option value="dark">Dark</option>
    <option value="dim">Dim</option>
    <option value="light">Light</option>
  </select>

  <select id="verify">
    <option value="no">Not Verified</option>
    <option value="yes">Verified</option>
  </select>

  <button onclick="download()">Save Tweet as PNG</button>
</div>

<div id="tweet">
  <div class="header">
    <img class="pfp" id="pfp">
    <div class="info">
      <div class="name">
        <span id="name">Elon Musk</span>
        <svg class="badge" id="badge" viewBox="0 0 24 24" fill="#1d9bf0">
          <path d="M22.5 12l-2.54-2.94.35-3.87-3.78-.88L14.7 1l-3.7 1.31-3.78.88.35 3.87L5.5 12l2.54 2.94-.35 3.87 3.78.88L14.7 23l3.7-1.31 3.78-.88-.35-3.87z"/>
        </svg>
      </div>
      <div class="username" id="user">@elonmusk</div>
    </div>
  </div>

  <div class="text" id="text"></div>
  <div class="footer" id="date"></div>

  <div class="stats">
    <span id="r"></span>
    <span id="rt"></span>
    <span id="l"></span>
  </div>
</div>

<script>
/* IMAGE */
img.onchange=e=>{
  const r=new FileReader();
  r.onload=()=>pfp.src=r.result;
  r.readAsDataURL(e.target.files[0]);
};

/* TEXT */
nameI.oninput=()=>name.innerText=nameI.value;
userI.oninput=()=>user.innerText=userI.value;
textI.oninput=()=>text.innerText=textI.value;
dateI.oninput=()=>date.innerText=dateI.value;
rI.oninput=()=>r.innerText=rI.value;
rtI.oninput=()=>rt.innerText=rtI.value;
lI.oninput=()=>l.innerText=lI.value;

/* INIT */
text.innerText=textI.value;
date.innerText=dateI.value;
r.innerText=rI.value;
rt.innerText=rtI.value;
l.innerText=lI.value;

/* THEME */
theme.onchange=()=>{
  if(theme.value==="light") set("#fff","#0f1419","#536471");
  else if(theme.value==="dim") set("#15202b","#e7e9ea","#8899a6");
  else set("#000","#e7e9ea","#71767b");
};

function set(c,t,m){
  document.documentElement.style.setProperty("--card",c);
  document.documentElement.style.setProperty("--text",t);
  document.documentElement.style.setProperty("--muted",m);
}

/* VERIFIED */
verify.onchange=()=>{
  badge.style.display=verify.value==="yes"?"inline":"none";
};

/* APK-SAFE ONE CLICK SAVE PNG */
function download(){
  html2canvas(tweet,{scale:2, backgroundColor:getComputedStyle(tweet).backgroundColor}).then(canvas=>{
    canvas.toBlob(blob=>{
      // Create a temporary link
      const a=document.createElement("a");
      a.href=URL.createObjectURL(blob);
      a.download="fake_tweet.png";
      document.body.appendChild(a);
      a.click();  // Trigger download
      a.remove(); // Clean up
    });
  });
}
</script>

</body>
</html>
