# events-on4<!DOCTYPE html>
<html lang="en">
<head>
  <title>Event Services Selector</title>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <style>
    body { font-family: Arial,sans-serif; background:#f7f8fa; color: #222; }
    .container { max-width: 800px; margin: 36px auto 18px; background: #fff; padding: 32px 18px 28px; border-radius: 12px; box-shadow: 0 2px 10px #0002; }
    h1 { text-align: center; margin-bottom: 32px; }
    .tabs { display: flex; gap: 22px; margin-bottom: 29px; justify-content: center; }
    .tab-btn { background: #e3e7fa; border: none; border-radius: 8px; padding: 15px 18px; font-size: 1.08em; font-weight: bold; cursor: pointer; transition: background .2s, color .2s; }
    .tab-btn.selected { background: #376ade; color: #fff; }
    .step { margin-bottom: 22px; }
    .label { margin-bottom: 8px; font-weight: bold; display: block; }
    .input, select { padding: 8px 12px; border-radius: 6px; border: 1px solid #bbb; font-size: 1em; }
    .types-row { display: flex; gap: 10px; margin-bottom: 9px; }
    .type-btn, .photo-btn, .dj-btn { background: #eff0f5; border-radius: 8px; border: none; padding: 9px 17px; font-size: 1em; cursor: pointer; margin-right: 8px; margin-bottom: 8px; transition: background .18s, color .18s; }
    .type-btn.selected, .photo-btn.selected, .dj-btn.selected { background: #397adc; color: #fff; }
    .summary-box { background: #e3e7fa; padding: 21px; border-radius: 8px; margin-top: 17px; }
    .section-title { font-size: 1.08em; margin-bottom: 5px; text-decoration: underline; color: #333; }
    ul { padding-left: 21px; }
    @media (max-width:650px){ .container{padding:8px 3vw 10px;} .tabs,.types-row{flex-direction:column;} }
  </style>
</head>
<body>
  <div class="container">
    <h1>Event Services Selector</h1>
    <div class="tabs">
      <button class="tab-btn" onclick="showTab('conv')">Conventions</button>
      <button class="tab-btn" onclick="showTab('photo')">Photography & Videography</button>
      <button class="tab-btn" onclick="showTab('dj')">DJ & Dance/Singing</button>
    </div>
    <form id="convForm" style="display:none;">
      <div class="step">
        <span class="label">Budget (₹):</span>
        <input type="number" class="input" id="budget" placeholder="e.g. 100000" min="0" required>
      </div>
      <div class="step">
        <span class="label">Capacity:</span>
        <input type="number" class="input" id="capacity" placeholder="e.g. 500" min="0" required>
      </div>
      <div class="step">
        <span class="label">Type:</span>
        <div class="types-row">
          <button type="button" class="type-btn" onclick="selectConvType('Regular')">Regular (20-60K)</button>
          <button type="button" class="type-btn" onclick="selectConvType('Medium')">Medium</button>
          <button type="button" class="type-btn" onclick="selectConvType('Premium')">Premium (20-85K)</button>
        </div>
      </div>
      <button type="button" class="tab-btn" onclick="showConvSummary()">Show Summary</button>
    </form>
    <form id="photoForm" style="display:none;">
      <div class="step">
        <span class="label">Location:</span>
        <input type="text" class="input" id="photoLocation" required placeholder="Enter location">
      </div>
      <div class="step">
        <span class="label">Type:</span>
        <select id="photoType" class="input">
          <option value="Regular">Regular (20-15K)</option>
          <option value="Medium">Medium</option>
          <option value="Premium">Premium (20-85K)</option>
        </select>
      </div>
      <div class="step">
        <span class="label">Photo/Videography Services:</span>
        <div>
          <button type="button" class="photo-btn" onclick="togglePhotoSvc('Studio Album')">Studio Album</button>
          <button type="button" class="photo-btn" onclick="togglePhotoSvc('Pristine')">Pristine</button>
          <button type="button" class="photo-btn" onclick="togglePhotoSvc('Candid')">Candid</button>
        </div>
      </div>
      <button type="button" class="tab-btn" onclick="showPhotoSummary()">Show Summary</button>
    </form>
    <form id="djForm" style="display:none;">
      <div class="step">
        <span class="label">Location:</span>
        <input type="text" class="input" id="djLocation" required placeholder="Enter location">
      </div>
      <div class="step">
        <span class="label">Dance/Singing:</span>
        <div>
          <button type="button" class="dj-btn" onclick="toggleDJ('Mohini Dance (17000)')">Mohini Dance (17,000)</button>
          <button type="button" class="dj-btn" onclick="toggleDJ('Banesh Dance (20000)')">Banesh Dance (20,000)</button>
        </div>
      </div>
      <button type="button" class="tab-btn" onclick="showDJSummary()">Show Summary</button>
    </form>
    <div class="summary-box" id="summaryBox" style="display:none;">
      <div class="section-title">Summary:</div>
      <div id="summaryContent"></div>
    </div>
  </div>
<script>
  // Tabs logic
  function showTab(tab){
    document.getElementById('convForm').style.display = tab==='conv'?'block':'none';
    document.getElementById('photoForm').style.display = tab==='photo'?'block':'none';
    document.getElementById('djForm').style.display = tab==='dj'?'block':'none';
    document.getElementById('summaryBox').style.display = 'none';
    document.querySelectorAll('.tab-btn').forEach(b=>b.classList.remove('selected'));
    let idx={conv:0,photo:1,dj:2}[tab];
    document.querySelectorAll('.tab-btn')[idx].classList.add('selected');
    reset(tab);
  }
  // Conventions
  let convType=null;
  function selectConvType(t){
    convType=t;
    document.querySelectorAll('.type-btn').forEach(b=>b.classList.remove('selected'));
    Array.from(document.querySelectorAll('.type-btn')).find(b=>b.textContent.includes(t)).classList.add('selected');
  }
  function showConvSummary(){
    let budget=document.getElementById('budget').value;
    let capacity=document.getElementById('capacity').value;
    let html='<ul>';
    html+=`<li><b>Budget:</b> ₹${budget}</li>`;
    html+=`<li><b>Capacity:</b> ${capacity}</li>`;
    html+=`<li><b>Type:</b> ${convType}</li>`;
    html+='</ul>';
    document.getElementById('summaryContent').innerHTML=html;
    document.getElementById('summaryBox').style.display='';
    window.scrollTo({top:document.body.scrollHeight,behavior:"smooth"});
  }
  // Photography
  let photoSvcs={};
  function togglePhotoSvc(svc){
    photoSvcs[svc]=!photoSvcs[svc];
    document.querySelectorAll('.photo-btn').forEach(b=>{if(b.textContent===svc) b.classList.toggle('selected',!!photoSvcs[svc]);});
  }
  function showPhotoSummary(){
    let location=document.getElementById('photoLocation').value;
    let type=document.getElementById('photoType').value;
    let html='<ul>';
    html+=`<li><b>Location:</b> ${location}</li>`;
    html+=`<li><b>Type:</b> ${type}</li>`;
    let selected = Object.keys(photoSvcs).filter(k=>photoSvcs[k]);
    html+=`<li><b>Photo/Videography:</b> ${selected.length?selected.join(', '):'None'}</li>`;
    html+='</ul>';
    document.getElementById('summaryContent').innerHTML=html;
    document.getElementById('summaryBox').style.display='';
    window.scrollTo({top:document.body.scrollHeight,behavior:"smooth"});
  }
  // DJ
  let djSvcs={};
  function toggleDJ(opt){
    djSvcs[opt]=!djSvcs[opt];
    document.querySelectorAll('.dj-btn').forEach(b=>{if(b.textContent===opt) b.classList.toggle('selected',!!djSvcs[opt]);});
  }
  function showDJSummary(){
    let location=document.getElementById('djLocation').value;
    let html='<ul>';
    html+=`<li><b>Location:</b> ${location}</li>`;
    let selected = Object.keys(djSvcs).filter(k=>djSvcs[k]);
    html+=`<li><b>Dances/Singing:</b> ${selected.length?selected.join(', '):'None'}</li>`;
    html+='</ul>';
    document.getElementById('summaryContent').innerHTML=html;
    document.getElementById('summaryBox').style.display='';
    window.scrollTo({top:document.body.scrollHeight,behavior:"smooth"});
  }
  // Reset
  function reset(tab){
    if(tab==='conv'){convType=null;document.getElementById('budget').value='';document.getElementById('capacity').value='';document.querySelectorAll('.type-btn').forEach(b=>b.classList.remove('selected'));}
    if(tab==='photo'){photoSvcs={};document.getElementById('photoLocation').value='';document.getElementById('photoType').selectedIndex=0;document.querySelectorAll('.photo-btn').forEach(b=>b.classList.remove('selected'));}
    if(tab==='dj'){djSvcs={};document.getElementById('djLocation').value='';document.querySelectorAll('.dj-btn').forEach(b=>b.classList.remove('selected'));}
  }
</script>
</body>
</html>
