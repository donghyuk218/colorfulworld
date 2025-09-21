<html lang="ko">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>신촌의 색 - colorfulworld sinchon.</title>
<style>
:root{--bg:#0b1020;--panel:#0e1428;--line:#1e293b;--muted:#90a4b8;--text:#e6edf3;--accent:#7dd3fc;}
*{box-sizing:border-box} html,body{height:100%}
body{margin:0;font-family:system-ui,-apple-system,Segoe UI,Roboto,Pretendard,'Noto Sans KR',sans-serif;
  background:radial-gradient(1200px 800px at 20% -20%,#0c1730 10%,#0b1020 60%,#090f1c 100%);color:var(--text)}
a{color:var(--accent)}
header{position:sticky;top:0;z-index:10;backdrop-filter:blur(10px);background:rgba(9,15,28,.65);border-bottom:1px solid var(--line)}
.container{max-width:1200px;margin:0 auto;padding:18px 20px}
h1{margin:0;font-size:26px;letter-spacing:.2px}
.subtitle{margin:6px 0 0;color:var(--muted);font-size:14px}
.tabs{display:flex;gap:8px;margin-top:12px;flex-wrap:wrap}
.tab{padding:8px 12px;border:1px solid var(--line);border-radius:10px;background:linear-gradient(180deg,#0a1428,#0a1326);color:var(--muted);cursor:pointer;user-select:none}
.tab.active{background:var(--accent);color:#041422;border-color:#a5f3fc;font-weight:600}
main{max-width:1200px;margin:0 auto;padding:18px 20px;display:grid;gap:14px}
.panel{border:1px solid var(--line);background:linear-gradient(180deg,#0b1222,#0b1326);border-radius:16px;padding:16px}
.grid{display:grid;grid-template-columns:1.1fr .9fr;gap:16px}
input,textarea,button,select{width:100%;padding:10px 12px;border-radius:10px;border:1px solid var(--line);background:#0a1426;color:var(--text);outline:none}
button{cursor:pointer}
button.primary{background:var(--accent);color:#072436;font-weight:700;border-color:#a5f3fc}
label{font-size:13px;color:var(--muted)}
.small{font-size:12px;color:var(--muted)}
hr{border:none;border-top:1px solid var(--line);margin:14px 0}
#preview{width:100%;border-radius:12px;border:1px solid var(--line);object-fit:cover;max-height:360px}
#palette{display:flex;gap:8px;flex-wrap:wrap;margin-top:8px}
.sw{width:36px;height:36px;border-radius:8px;border:1px solid rgba(255,255,255,.08);position:relative}
.sw span{position:absolute;bottom:-17px;left:0;right:0;margin:auto;text-align:center;font-size:10px;color:var(--muted)}
.cards{display:grid;grid-template-columns:repeat(auto-fill,minmax(260px,1fr));gap:12px}
.card{border:1px solid var(--line);border-radius:14px;padding:12px;background:linear-gradient(180deg,#0a1221,#091226)}
.card img{width:100%;height:180px;object-fit:cover;border-radius:10px;border:1px solid var(--line)}
.row{display:flex;gap:8px;align-items:center}
.tag{display:inline-block;padding:2px 8px;border-radius:999px;border:1px solid var(--line);color:var(--muted);font-size:11px}
#vizWrap{position:relative}
#vizCanvas{width:100%;height:560px;background:#0a1020;border-radius:14px;border:1px solid var(--line)}
#vizEmpty{position:absolute;inset:0;display:none;align-items:center;justify-content:center;color:var(--muted);font-size:14px}
.legend{display:flex;flex-wrap:wrap;gap:8px;margin-top:10px}
.legend .item{display:flex;align-items:center;gap:6px;border:1px solid var(--line);padding:6px 8px;border-radius:10px;font-size:12px}
.legend .dot{width:14px;height:14px;border-radius:4px;border:1px solid rgba(255,255,255,.15)}
footer{padding:18px;text-align:center;color:var(--muted)}
.notice{color:#86efac;font-size:12px}
kbd{background:#0a1428;border:1px solid var(--line);border-radius:6px;padding:2px 6px;font-size:12px}
</style>
</head>
<body>
<header>
  <div class="container">
    <h1>신촌의 색 — Color Commons</h1>
    <p class="subtitle">사진과 기록으로 모은 신촌의 색. 누구나 올리고, 모두 함께 보는 신촌의 색 아카이브.</p>
    <div class="tabs">
      <div class="tab active" data-tab="upload">새 기록</div>
      <div class="tab" data-tab="board">게시판</div>
      <div class="tab" data-tab="atlas">색 아카이브</div>
      <div class="tab" data-tab="data">백업/가져오기</div>
      <div class="tab" data-tab="help">도움말</div>
    </div>
  </div>
</header>

<main>
  <!-- Upload -->
  <section id="upload" class="panel">
    <div class="grid">
      <div>
        <label>사진 업로드</label>
        <input id="file" type="file" accept="image/*">
        <img id="preview" src="" alt="" style="margin-top:10px; display:none">
        <canvas id="canvas" style="display:none"></canvas>
        <div class="row" style="margin-top:10px; align-items:flex-end">
          <div style="width:120px">
            <label>팔레트 개수 (k)</label>
            <input id="k" type="number" min="3" max="8" value="5">
          </div>
          <button id="extract">색 추출</button>
          <span class="small">이미지에서 대표색을 찾습니다(k-means).</span>
        </div>
        <div id="palette"></div>
      </div>
      <div>
        <label>신촌의 색 이름 (예: 연세로 보랏빛 저녁)</label>
        <input id="title" placeholder="색 이름을 적어주세요">
        <label style="margin-top:8px">짧은 기록</label>
        <textarea id="note" rows="5" placeholder="그때의 공기, 말투, 소리… 무엇이 보였나요?"></textarea>
        <div class="row" style="margin-top:8px">
          <div style="flex:1">
            <label>날짜</label>
            <input id="date" type="date">
          </div>
          <div style="width:160px">
            <label>태그</label>
            <input id="tags" placeholder="예: 저녁,비,골목">
          </div>
        </div>
        <hr>
        <div class="row" style="justify-content:space-between">
          <span class="small">저장은 이 브라우저에 보관됩니다(<kbd>localStorage</kbd>).</span>
          <button class="primary" id="save">기록 저장</button>
        </div>
      </div>
    </div>
  </section>

  <!-- Board -->
  <section id="board" class="panel" style="display:none">
    <div class="row" style="justify-content:space-between; margin-bottom:10px">
      <div class="small">총 <span id="count">0</span>건</div>
      <div class="row">
        <input id="search" placeholder="제목/기록/태그 검색" style="width:220px">
        <select id="sort">
          <option value="new">최신순</option>
          <option value="old">오래된순</option>
          <option value="title">제목순</option>
        </select>
      </div>
    </div>
    <div id="cards" class="cards"></div>
  </section>

  <!-- Atlas -->
  <section id="atlas" class="panel" style="display:none">
    <h3>신촌의 색 아카이브(집계 시각화)</h3>
    <p class="small">여러 사람이 저장한 이미지의 팔레트를 모아 색 분포를 보여줍니다. 색은 12-bit로 양자화(#RGB) 후 빈도 기반으로 배치합니다.</p>
    <div id="vizWrap">
      <canvas id="vizCanvas"></canvas>
      <div id="vizEmpty">아직 집계할 색 팔레트가 없습니다. “색 추출” 후 저장해 보세요.</div>
    </div>
    <div id="legend" class="legend"></div>
  </section>

  <!-- Data -->
  <section id="data" class="panel" style="display:none">
    <div class="grid" style="grid-template-columns:1fr 1fr">
      <div>
        <h3>내보내기</h3>
        <p class="small">현재 저장된 모든 기록을 JSON으로 다운로드합니다.</p>
        <button id="export" class="primary">JSON 다운로드</button>
      </div>
      <div>
        <h3>가져오기</h3>
        <p class="small">다른 브라우저/기기의 백업 JSON을 불러옵니다(중복은 무시).</p>
        <input id="import" type="file" accept="application/json">
      </div>
    </div>
  </section>

  <!-- Help -->
  <section id="help" class="panel" style="display:none">
    <h3>도움말</h3>
    <ul>
      <li>“새 기록”에서 사진을 올리고 <b>색을 추출</b>한 뒤 제목과 기록을 적어 저장합니다.</li>
      <li>“게시판”에서 저장한 사진과 팔레트를 카드로 확인하고, 검색/정렬할 수 있습니다.</li>
      <li>“색 아카이브”는 저장된 카드의 팔레트를 모아 색 분포를 그리드로 시각화합니다.</li>
      <li>데이터는 브라우저 <kbd>localStorage</kbd>에 보관되며, “백업/가져오기”로 JSON 이동이 가능합니다.</li>
    </ul>
    <p class="notice">이미지 저장 실패 시, 이미지 크기를 줄여 재시도하거나 일부 기록을 삭제해 용량을 확보하세요.</p>
  </section>
</main>

<footer>© 2025 신촌의 색 </footer>

<script>
// ---------- Tabs ----------
const sections={upload:qs('#upload'),board:qs('#board'),atlas:qs('#atlas'),data:qs('#data'),help:qs('#help')};
document.querySelectorAll('.tab').forEach(t=>t.addEventListener('click',()=>{
  document.querySelectorAll('.tab').forEach(x=>x.classList.remove('active')); t.classList.add('active');
  Object.values(sections).forEach(s=>s.style.display='none'); sections[t.dataset.tab].style.display='block';
  if(t.dataset.tab==='board') renderBoard();
  if(t.dataset.tab==='atlas') drawAtlas();
}));

// ---------- Elements ----------
const fileEl=qs('#file'), preview=qs('#preview'), canvas=qs('#canvas'), ctx=canvas.getContext('2d',{willReadFrequently:true});
const kEl=qs('#k'), paletteEl=qs('#palette');
const titleEl=qs('#title'), noteEl=qs('#note'), dateEl=qs('#date'), tagsEl=qs('#tags');

// ---------- Image handling & palette ----------
let currentImageData=null;
fileEl.addEventListener('change',()=>{
  const f=fileEl.files[0]; if(!f) return;
  const url=URL.createObjectURL(f);
  const img=new Image();
  img.onload=()=>{
    const maxW=640, scale=Math.min(1, maxW/img.width);
    const w=Math.max(1, Math.round(img.width*scale)), h=Math.max(1, Math.round(img.height*scale));
    canvas.width=w; canvas.height=h; ctx.drawImage(img,0,0,w,h);
    try{ currentImageData=ctx.getImageData(0,0,w,h); }catch(e){ currentImageData=null; }
    preview.src=url; preview.style.display='block';
  };
  img.src=url;
});

qs('#extract').addEventListener('click',()=>{
  if(!currentImageData){ alert('이미지를 먼저 올려주세요.'); return; }
  const k=Math.max(3, Math.min(8, parseInt(kEl.value||'5',10)));
  const px=samplePixels(currentImageData,6);
  const centers=kmeans(px,k,12);
  const hexes=centers.map(c=>rgbToHex(c[0],c[1],c[2]));
  paletteEl.innerHTML='';
  hexes.forEach(h=>{ const el=document.createElement('div'); el.className='sw'; el.style.background=h; el.title=h; el.innerHTML=`<span>${h}</span>`; paletteEl.appendChild(el); });
});

// ---------- Save ----------
qs('#save').addEventListener('click',()=>{
  if(!preview.src){ alert('사진을 올려주세요.'); return; }
  const title=titleEl.value.trim(); if(!title){ alert('색 이름을 입력해주세요.'); return; }
  const note=noteEl.value.trim(); const date=dateEl.value; const tags=(tagsEl.value||'').split(',').map(s=>s.trim()).filter(Boolean);
  const palette=currentPalette();
  if(palette.length===0){ if(!confirm('팔레트를 추출하지 않았습니다. 추출 없이 저장할까요?')) return; }

  // 이미지 DataURL 생성(재시도 포함)
  let imageDataURL=null;
  try{ imageDataURL = canvas.toDataURL('image/jpeg',0.8); }
  catch(e){ imageDataURL = null; }
  // 크기 줄여 재시도(용량 문제 대비)
  if(imageDataURL && imageDataURL.length>1_500_000){ // 1.5MB 초과 시 추가 압축
    try{ imageDataURL = canvas.toDataURL('image/jpeg',0.7); }catch(e){}
  }

  const rec={ id:'rec_'+Date.now(), title, note, date, tags, palette, image:imageDataURL };

  const all=loadAll();
  all.unshift(rec);
  try{
    saveAll(all);
    alert('저장되었습니다.');
    resetForm(); renderBoard();
  }catch(err){
    // 저장 실패: 이미지 제거하고 다시 저장 폴백
    try{
      rec.image=null;
      all.shift(); all.unshift(rec);
      saveAll(all);
      alert('이미지 용량으로 인해 텍스트만 저장했습니다. 일부 기록 삭제 또는 작은 이미지로 다시 시도하세요.');
      resetForm(); renderBoard();
    }catch(e2){
      alert('저장에 실패했습니다. 이미지 크기를 줄이거나 일부 기록을 삭제한 후 다시 시도하세요.');
    }
  }
});

function resetForm(){
  titleEl.value=''; noteEl.value=''; dateEl.value=''; tagsEl.value='';
  fileEl.value=''; preview.src=''; preview.style.display='none'; paletteEl.innerHTML='';
  currentImageData=null;
}

// ---------- Board ----------
const searchEl=qs('#search'), sortEl=qs('#sort'), cardsEl=qs('#cards');
searchEl?.addEventListener('input', renderBoard);
sortEl?.addEventListener('change', renderBoard);

function renderBoard(){
  let data=loadAll();
  const q=(searchEl?.value||'').toLowerCase();
  if(q){
    data = data.filter(d=>(d.title||'').toLowerCase().includes(q) || (d.note||'').toLowerCase().includes(q) || (d.tags||[]).some(t=>t.toLowerCase().includes(q)));
  }
  const sort=sortEl?.value||'new';
  if(sort==='new') data.sort((a,b)=>(b.date||'').localeCompare(a.date||''));
  else if(sort==='old') data.sort((a,b)=>(a.date||'').localeCompare(b.date||''));
  else if(sort==='title') data.sort((a,b)=>(a.title||'').localeCompare(b.title||''));

  qs('#count').textContent=data.length;
  cardsEl.innerHTML='';
  data.forEach(d=>{
    const card=document.createElement('div'); card.className='card';
    const imgHtml = d.image ? `<img src="${d.image}" alt="">` : `<div style="height:180px;border:1px dashed var(--line);border-radius:10px;display:flex;align-items:center;justify-content:center;color:var(--muted)">이미지 없음</div>`;
    card.innerHTML=`
      ${imgHtml}
      <div class="row" style="justify-content:space-between; margin-top:8px">
        <b>${escapeHtml(d.title||'무제')}</b>
        <span class="tag">${d.date||''}</span>
      </div>
      <div class="small" style="margin:6px 0">${escapeHtml(d.note||'')}</div>
      <div class="row" style="gap:6px; flex-wrap:wrap; margin-top:6px">
        ${(d.palette||[]).map(c=>`<span class="sw" title="${c}" style="width:18px;height:18px;background:${c}"></span>`).join('')}
      </div>
      <div class="row" style="margin-top:8px; gap:6px">
        ${(d.tags||[]).map(t=>`<span class="tag">#${escapeHtml(t)}</span>`).join('')}
        <span style="flex:1"></span>
        <button onclick="delRec('${d.id}')">삭제</button>
      </div>
    `;
    cardsEl.appendChild(card);
  });
}

// ---------- Atlas Visualization ----------
function drawAtlas(){
  const canvas = qs('#vizCanvas'); const empty = qs('#vizEmpty'); const legend = qs('#legend');
  const dpr = window.devicePixelRatio || 1;
  const w = canvas.clientWidth || 800, h = canvas.clientHeight || 560;
  canvas.width = Math.round(w*dpr); canvas.height = Math.round(h*dpr);
  const g = canvas.getContext('2d'); g.setTransform(dpr,0,0,dpr,0,0);
  g.clearRect(0,0,w,h); legend.innerHTML='';

  const all = loadAll();
  const bins = new Map();
  let totalColors = 0;
  all.forEach(item => (item.palette||[]).forEach(hex => {
    if(!hex) return;
    const key = quantize12(hex).toLowerCase();
    if(!bins.has(key)) bins.set(key,{hex:key,count:0});
    bins.get(key).count++; totalColors++;
  }));

  if(totalColors===0){
    empty.style.display='flex';
    return;
  } else {
    empty.style.display='none';
  }

  const arr = Array.from(bins.values()).sort((a,b)=>b.count-a.count);

  const cols = Math.max(1, Math.ceil(Math.sqrt(arr.length)));
  const rows = Math.max(1, Math.ceil(arr.length/cols));
  const pad = 8, cellW = Math.floor((w - pad*2) / cols), cellH = Math.floor((h - pad*2) / rows);
  const cell = Math.max(10, Math.min(cellW, cellH));

  arr.forEach((bin,i)=>{
    const cx = pad + (i%cols)*cell;
    const cy = pad + Math.floor(i/cols)*cell;
    g.fillStyle = expand12(bin.hex);
    g.fillRect(cx, cy, cell-2, cell-2);
  });

  arr.slice(0,16).forEach(b=>{
    const el = document.createElement('div'); el.className='item';
    const dot = document.createElement('div'); dot.className='dot'; dot.style.background = expand12(b.hex);
    const label = document.createElement('div'); label.textContent = `${expand12(b.hex)} × ${b.count}`;
    el.appendChild(dot); el.appendChild(label); legend.appendChild(el);
  });
}

// ---------- Export/Import ----------
qs('#export').addEventListener('click',()=>{
  const data = loadAll();
  const blob = new Blob([JSON.stringify({version:1, exportedAt:new Date().toISOString(), items:data}, null, 2)], {type:'application/json'});
  const url = URL.createObjectURL(blob);
  const a=document.createElement('a'); a.href=url; a.download='shinchon-colors.json'; document.body.appendChild(a); a.click(); a.remove(); URL.revokeObjectURL(url);
});
qs('#import').addEventListener('change', async (e)=>{
  const f=e.target.files[0]; if(!f) return;
  try{
    const text = await f.text();
    const parsed = JSON.parse(text);
    const items = parsed.items || parsed;
    if(!Array.isArray(items)) throw new Error('형식 오류');
    const now = loadAll();
    const ids = new Set(now.map(x=>x.id));
    const merged = [...items.filter(x=>!ids.has(x.id)), ...now];
    saveAll(merged);
    alert(`가져오기가 완료되었습니다: ${items.length}건`);
    renderBoard(); drawAtlas();
  }catch(err){
    alert('가져오기에 실패했습니다.');
  }
});

// ---------- Storage ----------
function loadAll(){ try{ return JSON.parse(localStorage.getItem('shinchon-colors')||'[]'); }catch(e){ return []; } }
function saveAll(arr){ localStorage.setItem('shinchon-colors', JSON.stringify(arr)); }
function delRec(id){ if(!confirm('삭제하시겠습니까?')) return; const next=loadAll().filter(x=>x.id!==id); saveAll(next); renderBoard(); drawAtlas(); }

// ---------- Helpers ----------
function qs(sel){ return document.querySelector(sel); }
function escapeHtml(s){ return (s||'').replace(/[&<>\"']/g, m=>({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[m])); }

function samplePixels(imgData, step=6){
  const {data,width,height}=imgData||{}; const out=[];
  if(!data) return out;
  for(let y=0;y<height;y+=step){
    for(let x=0;x<width;x+=step){
      const i=(y*width+x)*4; const a=data[i+3]; if(a<128) continue;
      out.push([data[i],data[i+1],data[i+2]]);
    }
  }
  return out;
}
function kmeans(pixels, k=5, maxIter=12){
  if(!pixels.length) return Array.from({length:k},()=>[128,128,128]);
  const centers=[]; for(let i=0;i<k;i++){ centers.push(pixels[Math.floor(Math.random()*pixels.length)].slice()); }
  let labels=new Array(pixels.length).fill(0);
  for(let it=0; it<maxIter; it++){
    for(let i=0;i<pixels.length;i++){
      let best=0, min=1e9;
      for(let c=0;c<k;c++){
        const d=dist(pixels[i], centers[c]);
        if(d<min){min=d; best=c;}
      }
      labels[i]=best;
    }
    const sum=Array.from({length:k},()=>[0,0,0,0]);
    for(let i=0;i<pixels.length;i++){
      const c=labels[i]; sum[c][0]+=pixels[i][0]; sum[c][1]+=pixels[i][1]; sum[c][2]+=pixels[i][2]; sum[c][3]++;
    }
    for(let c=0;c<k;c++){
      if(sum[c][3]===0) continue;
      centers[c][0]=Math.round(sum[c][0]/sum[c][3]);
      centers[c][1]=Math.round(sum[c][1]/sum[c][3]);
      centers[c][2]=Math.round(sum[c][2]/sum[c][3]);
    }
  }
  return centers;
}
function dist(a,b){ const dr=a[0]-b[0], dg=a[1]-b[1], db=a[2]-b[2]; return dr*dr+dg*dg+db*db; }
function rgbToHex(r,g,b){ return "#"+[r,g,b].map(v=>v.toString(16).padStart(2,'0')).join(''); }
function currentPalette(){ return Array.from(document.querySelectorAll('#palette .sw')).map(el=>el.title).filter(Boolean); }
function quantize12(hex){
  const {r,g,b} = parseHex(hex); const rq=(r>>4).toString(16), gq=(g>>4).toString(16), bq=(b>>4).toString(16);
  return '#'+rq+gq+bq;
}
function expand12(rgb){ const r=rgb[1], g=rgb[2], b=rgb[3]; return '#'+r+r+g+g+b+b; }
function parseHex(h){
  if(!h) return {r:0,g:0,b:0};
  if(h.length===4){ const r=parseInt(h[1]+h[1],16), g=parseInt(h[2]+h[2],16), b=parseInt(h[3]+h[3],16); return {r,g,b}; }
  const r=parseInt(h.slice(1,3),16), g=parseInt(h.slice(3,5),16), b=parseInt(h.slice(5,7),16); return {r,g,b};
}

// Initialize
renderBoard();
</script>
</body>
</html>
