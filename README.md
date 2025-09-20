# colorfulworld
colorfulworldsinchon.
[신촌의_색_웹사이트_템플릿_color_atlas.html](https://github.com/user-attachments/files/22448445/_._._._color_atlas.html)
<!doctype html>
<html lang="ko">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>신촌의 색 Color Atlas</title>
  <meta name="description" content="신촌의 색을 수집하고 기록하는 사진·글 아카이브 / 지도와 게시판 포함" />
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Pretendard:wght@300;400;500;600;700&display=swap" rel="stylesheet">
  <script src="https://cdn.tailwindcss.com"></script>
  <!-- Leaflet for map -->
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" integrity="sha256-p4NxAoJBhIIN+hmNHrzRCf9tD/miZyoHS5obTRR9BMY=" crossorigin=""/>
  <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js" integrity="sha256-20nQCchB9co0qIjJZRGuk2/Z9VM+kNiyxNV1lvTlZBo=" crossorigin=""></script>
  <style>
    html, body { font-family: Pretendard, ui-sans-serif, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial, "Noto Sans"; }
    .card-shadow { box-shadow: 0 10px 25px rgba(0,0,0,.08); }
    .ring-accent { box-shadow: 0 0 0 3px rgba(14,165,233,0.35); }
    .sr-only { position:absolute; width:1px; height:1px; margin:-1px; padding:0; overflow:hidden; clip:rect(0,0,0,0); border:0; }
    .tab-active { background:#0ea5e9; color:#fff; }
    .palette-dot { width:14px; height:14px; border-radius:999px; border:1px solid rgba(255,255,255,.8); box-shadow:0 0 0 1px rgba(0,0,0,.08) inset; }
    #map { height: 520px; }
  </style>
</head>
<body class="min-h-screen bg-gradient-to-b from-white to-slate-50 text-slate-800">
  <!-- Header -->
  <header class="sticky top-0 z-40 bg-white/80 backdrop-blur border-b border-slate-200/60">
    <div class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8 py-3 flex items-center justify-between">
      <div class="flex items-center gap-3">
        <div class="h-9 w-9 rounded-xl bg-sky-500"></div>
        <div>
          <h1 class="text-lg font-semibold tracking-tight">신촌의 색 <span class="text-slate-400 text-base">Color Atlas</span></h1>
          <p class="text-xs text-slate-500">사진·지도·이야기로 수집하는 신촌의 색채 아카이브</p>
        </div>
      </div>
      <nav class="hidden md:flex items-center gap-2 text-sm">
        <button id="tab-gallery" class="px-3 py-1.5 rounded-lg hover:bg-slate-100 tab-active">갤러리</button>
        <button id="tab-map" class="px-3 py-1.5 rounded-lg hover:bg-slate-100">지도</button>
        <button id="tab-board" class="px-3 py-1.5 rounded-lg hover:bg-slate-100">이야기</button>
        <div class="w-px h-5 bg-slate-200 mx-1"></div>
        <button id="btn-add" class="px-3 py-1.5 rounded-lg bg-sky-500 text-white hover:bg-sky-600 transition">새 기록</button>
        <button id="btn-export" class="px-3 py-1.5 rounded-lg border border-slate-300 hover:bg-slate-100">내보내기</button>
        <label class="px-3 py-1.5 rounded-lg border border-slate-300 cursor-pointer hover:bg-slate-100">가져오기
          <input id="import-json" type="file" accept="application/json" class="sr-only" />
        </label>
      </nav>
    </div>
  </header>

  <!-- Controls -->
  <section class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8 mt-10 mb-6">
    <div class="grid md:grid-cols-3 gap-6 items-stretch">
      <div class="md:col-span-2 p-6 rounded-2xl card-shadow bg-white">
        <h2 class="text-2xl font-semibold mb-2">오늘의 신촌 색</h2>
        <p class="text-sm text-slate-600">사진을 올리면 대표색과 <b>다중 팔레트(클러스터링)</b>가 자동 추출됩니다. 기록에는 위치(지도 클릭)와 이야기 메모를 함께 남길 수 있습니다.</p>
        <div class="mt-4 flex items-center gap-3 text-xs text-slate-500">
          <span class="inline-flex items-center gap-2"><span class="h-2 w-2 rounded-full bg-emerald-500"></span>자연</span>
          <span class="inline-flex items-center gap-2"><span class="h-2 w-2 rounded-full bg-amber-500"></span>상업</span>
          <span class="inline-flex items-center gap-2"><span class="h-2 w-2 rounded-full bg-indigo-500"></span>야간</span>
          <span class="inline-flex items-center gap-2"><span class="h-2 w-2 rounded-full bg-rose-500"></span>감정</span>
        </div>
      </div>
      <div class="p-6 rounded-2xl bg-sky-50 border border-sky-100">
        <h3 class="font-semibold mb-2">검색/필터</h3>
        <div class="grid grid-cols-2 gap-3 text-sm">
          <input id="q" type="search" placeholder="키워드 검색" class="col-span-2 px-3 py-2 rounded-lg border border-slate-300 focus:outline-none focus:ring-2 focus:ring-sky-400" />
          <select id="tag" class="px-3 py-2 rounded-lg border border-slate-300 focus:outline-none focus:ring-2 focus:ring-sky-400">
            <option value="">전체 태그</option>
            <option>자연</option>
            <option>상업</option>
            <option>야간</option>
            <option>감정</option>
          </select>
          <select id="time" class="px-3 py-2 rounded-lg border border-slate-300 focus:outline-none focus:ring-2 focus:ring-sky-400">
            <option value="">시간대</option>
            <option>새벽</option>
            <option>아침</option>
            <option>낮</option>
            <option>저녁</option>
            <option>밤</option>
          </select>
        </div>
      </div>
    </div>
  </section>

  <!-- Tabs Content -->
  <main class="mx-auto max-w-7xl px-4 sm:px-6 lg:px-8 pb-24">
    <!-- Gallery -->
    <section id="view-gallery">
      <div id="gallery" class="grid sm:grid-cols-2 lg:grid-cols-3 gap-6"></div>
      <div id="empty" class="hidden text-center text-slate-500 mt-14">아직 기록이 없습니다. 오른쪽 상단의 <b>새 기록</b>을 눌러 시작해 보세요.</div>
    </section>

    <!-- Map -->
    <section id="view-map" class="hidden">
      <div class="rounded-2xl overflow-hidden border border-slate-200 card-shadow">
        <div id="map"></div>
      </div>
      <p class="text-xs text-slate-500 mt-2">지도의 타일/데이터는 외부 제공(오픈스트리트맵)으로, 네트워크 연결이 필요합니다.</p>
    </section>

    <!-- Board -->
    <section id="view-board" class="hidden">
      <div class="grid md:grid-cols-2 gap-6">
        <div class="p-6 rounded-2xl bg-white border border-slate-200 card-shadow">
          <h3 class="text-lg font-semibold mb-3">신촌 사람들 이야기 쓰기</h3>
          <div class="grid gap-3 text-sm">
            <input id="bName" class="px-3 py-2 rounded-lg border border-slate-300" placeholder="이름(또는 필명)" />
            <input id="bTitle" class="px-3 py-2 rounded-lg border border-slate-300" placeholder="제목" />
            <textarea id="bBody" rows="6" class="px-3 py-2 rounded-lg border border-slate-300" placeholder="이야기, 장소, 시간, 색의 기억 등"></textarea>
            <button id="bPost" class="px-4 py-2 rounded-lg bg-sky-500 text-white hover:bg-sky-600">등록</button>
            <p class="text-xs text-slate-500">개인정보는 수집하지 않으며, 게시글은 이 브라우저에만 저장됩니다.</p>
          </div>
        </div>
        <div>
          <h3 class="text-lg font-semibold mb-3">이야기 목록</h3>
          <div id="boardList" class="grid gap-4"></div>
          <div id="boardEmpty" class="text-slate-500 text-sm">아직 등록된 이야기가 없습니다.</div>
        </div>
      </div>
    </section>
  </main>

  <!-- Add Modal -->
  <dialog id="modal" class="w-full max-w-4xl rounded-2xl p-0 overflow-hidden shadow-2xl">
    <form id="form" method="dialog" class="bg-white">
      <div class="p-6 border-b border-slate-200 flex items-center justify-between">
        <h3 class="text-lg font-semibold">새 기록 추가</h3>
        <button type="button" id="close" class="p-2 rounded-lg hover:bg-slate-100">✕</button>
      </div>
      <div class="p-6 grid md:grid-cols-2 gap-6">
        <div>
          <label class="block text-sm font-medium mb-2">이미지</label>
          <div id="drop" class="aspect-[4/3] rounded-xl border-2 border-dashed border-slate-300 flex items-center justify-center text-slate-400 text-sm">이미지 끌어놓기 또는 클릭 업로드
            <input id="file" type="file" accept="image/*" class="sr-only" />
          </div>
          <canvas id="preview" class="mt-3 w-full rounded-xl hidden"></canvas>
          <div id="swatch" class="mt-3 hidden">
            <div class="text-sm text-slate-600 mb-1">자동 추출 대표색 & 팔레트</div>
            <div class="flex items-center gap-2" id="palette"></div>
            <div class="mt-2 text-xs text-slate-500" id="hex">#—</div>
          </div>
        </div>
        <div class="grid gap-4">
          <div>
            <label class="block text-sm font-medium mb-1">제목</label>
            <input id="title" required class="w-full px-3 py-2 rounded-lg border border-slate-300 focus:outline-none focus:ring-2 focus:ring-sky-400" placeholder="예) 연세로 비 온 뒤 아스팔트" />
          </div>
          <div class="grid grid-cols-2 gap-3">
            <div>
              <label class="block text-sm font-medium mb-1">장소(텍스트)</label>
              <input id="place" class="w-full px-3 py-2 rounded-lg border border-slate-300 focus:outline-none focus:ring-2 focus:ring-sky-400" placeholder="예) 연세로, 창천동" />
            </div>
            <div>
              <label class="block text-sm font-medium mb-1">시간대</label>
              <select id="timeInput" class="w-full px-3 py-2 rounded-lg border border-slate-300 focus:outline-none focus:ring-2 focus:ring-sky-400">
                <option>새벽</option><option>아침</option><option selected>낮</option><option>저녁</option><option>밤</option>
              </select>
            </div>
          </div>

          <div class="grid grid-cols-3 gap-3 items-end">
            <div>
              <label class="block text-sm font-medium mb-1">위도</label>
              <input id="lat" type="number" step="0.000001" class="w-full px-3 py-2 rounded-lg border border-slate-300" placeholder="37.559" />
            </div>
            <div>
              <label class="block text-sm font-medium mb-1">경도</label>
              <input id="lng" type="number" step="0.000001" class="w-full px-3 py-2 rounded-lg border border-slate-300" placeholder="126.936" />
            </div>
            <button type="button" id="pickMap" class="px-3 py-2 rounded-lg border border-slate-300 hover:bg-slate-100">지도에서 지정</button>
          </div>

          <div>
            <label class="block text-sm font-medium mb-1">태그</label>
            <div class="flex flex-wrap gap-2">
              <label class="inline-flex items-center gap-2 text-sm px-2 py-1.5 rounded-lg border border-slate-300 cursor-pointer"><input type="checkbox" class="tagChk" value="자연"/>자연</label>
              <label class="inline-flex items-center gap-2 text-sm px-2 py-1.5 rounded-lg border border-slate-300 cursor-pointer"><input type="checkbox" class="tagChk" value="상업"/>상업</label>
              <label class="inline-flex items-center gap-2 text-sm px-2 py-1.5 rounded-lg border border-slate-300 cursor-pointer"><input type="checkbox" class="tagChk" value="야간"/>야간</label>
              <label class="inline-flex items-center gap-2 text-sm px-2 py-1.5 rounded-lg border border-slate-300 cursor-pointer"><input type="checkbox" class="tagChk" value="감정"/>감정</label>
            </div>
          </div>
          <div>
            <label class="block text-sm font-medium mb-1">메모</label>
            <textarea id="note" rows="5" class="w-full px-3 py-2 rounded-lg border border-slate-300 focus:outline-none focus:ring-2 focus:ring-sky-400" placeholder="색의 느낌, 소리, 냄새, 그 순간의 이야기"></textarea>
          </div>
        </div>
      </div>
      <div class="p-6 border-t border-slate-200 flex items-center justify-end gap-3">
        <button type="button" id="resetBtn" class="px-4 py-2 rounded-lg border border-slate-300 hover:bg-slate-100">리셋</button>
        <button id="save" class="px-4 py-2 rounded-lg bg-sky-500 text-white hover:bg-sky-600">저장</button>
      </div>
    </form>
  </dialog>

  <!-- Map Picker Modal -->
  <dialog id="pickModal" class="w-full max-w-3xl rounded-2xl p-0 overflow-hidden shadow-2xl">
    <div class="p-3 bg-white border-b border-slate-200 flex items-center justify-between">
      <div class="text-sm">지도를 클릭하여 위치를 지정하세요 (신촌 일대)</div>
      <button id="pickClose" class="p-2 rounded-lg hover:bg-slate-100">✕</button>
    </div>
    <div id="pickMapView" style="height:420px"></div>
    <div class="p-3 bg-white border-t border-slate-200 text-right text-sm text-slate-600">좌표: <span id="pickCoord">—</span></div>
  </dialog>

  <template id="cardTpl">
    <article class="group overflow-hidden rounded-2xl bg-white card-shadow border border-slate-200/60">
      <div class="relative aspect-[4/3] overflow-hidden">
        <img class="w-full h-full object-cover" alt="" />
        <div class="absolute top-3 left-3 rounded-lg px-2 py-1 text-xs text-white/90" data-badge></div>
        <div class="absolute bottom-3 right-3 flex gap-1" data-chip></div>
      </div>
      <div class="p-4">
        <h4 class="font-semibold truncate" data-title></h4>
        <p class="text-xs text-slate-500 mt-1" data-meta></p>
        <p class="text-sm text-slate-700 mt-2 line-clamp-3" data-note></p>
        <div class="flex gap-2 flex-wrap mt-3" data-tags></div>
      </div>
    </article>
  </template>

  <template id="boardTpl">
    <article class="p-4 rounded-xl bg-white border border-slate-200 card-shadow">
      <h4 class="font-semibold" data-btitle></h4>
      <p class="text-xs text-slate-500 mt-0.5" data-bmeta></p>
      <p class="text-sm text-slate-700 mt-2" data-bbody></p>
    </article>
  </template>

  <script>
    // --- State ---
    const state = { items: [], board: [], filter: { q: '', tag: '', time: '' } };

    // --- Elements ---
    const gallery = document.getElementById('gallery');
    const empty = document.getElementById('empty');
    const modal = document.getElementById('modal');
    const form = document.getElementById('form');
    const fileInput = document.getElementById('file');
    const drop = document.getElementById('drop');
    const preview = document.getElementById('preview');
    const swatch = document.getElementById('swatch');
    const paletteWrap = document.getElementById('palette');
    const hexEl = document.getElementById('hex');

    const viewGallery = document.getElementById('view-gallery');
    const viewMap = document.getElementById('view-map');
    const viewBoard = document.getElementById('view-board');

    // Controls
    document.getElementById('tab-gallery').addEventListener('click', ()=> switchTab('gallery'));
    document.getElementById('tab-map').addEventListener('click', ()=> switchTab('map'));
    document.getElementById('tab-board').addEventListener('click', ()=> switchTab('board'));

    document.getElementById('btn-add').addEventListener('click', () => openModal());
    document.getElementById('close').addEventListener('click', () => modal.close());
    document.getElementById('resetBtn').addEventListener('click', resetModal);
    document.getElementById('save').addEventListener('click', onSave);
    document.getElementById('btn-export').addEventListener('click', onExport);
    document.getElementById('import-json').addEventListener('change', onImport);

    // Board
    document.getElementById('bPost').addEventListener('click', postBoard);

    // Filters
    document.getElementById('q').addEventListener('input', e => { state.filter.q = e.target.value.toLowerCase(); render(); });
    document.getElementById('tag').addEventListener('change', e => { state.filter.tag = e.target.value; render(); });
    document.getElementById('time').addEventListener('change', e => { state.filter.time = e.target.value; render(); });

    // Map picker
    const pickModal = document.getElementById('pickModal');
    const pickCoord = document.getElementById('pickCoord');
    let pickMap, pickMarker;
    document.getElementById('pickMap').addEventListener('click', () => {
      pickModal.showModal();
      setTimeout(()=>{
        if(!pickMap){
          pickMap = L.map('pickMapView').setView([37.559,126.936], 15);
          L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', { attribution: '&copy; OpenStreetMap' }).addTo(pickMap);
          pickMap.on('click', (e)=>{
            const { lat, lng } = e.latlng;
            if(pickMarker) pickMap.removeLayer(pickMarker);
            pickMarker = L.marker([lat,lng]).addTo(pickMap);
            document.getElementById('lat').value = lat.toFixed(6);
            document.getElementById('lng').value = lng.toFixed(6);
            pickCoord.textContent = lat.toFixed(6)+', '+lng.toFixed(6);
          });
        } else { pickMap.invalidateSize(); }
      },50);
    });
    document.getElementById('pickClose').addEventListener('click', ()=> pickModal.close());

    // Drag & Drop
    drop.addEventListener('click', () => fileInput.click());
    drop.addEventListener('dragover', e => { e.preventDefault(); drop.classList.add('ring-accent'); });
    drop.addEventListener('dragleave', () => drop.classList.remove('ring-accent'));
    drop.addEventListener('drop', e => { e.preventDefault(); drop.classList.remove('ring-accent'); handleFile(e.dataTransfer.files[0]); });
    fileInput.addEventListener('change', e => handleFile(e.target.files[0]));

    function switchTab(name){
      const tabs = ['gallery','map','board'];
      tabs.forEach(t=>{
        document.getElementById('view-'+t).classList.toggle('hidden', t!==name);
        document.getElementById('tab-'+t).classList.toggle('tab-active', t===name);
      });
      if(name==='map'){ initMainMap(); }
    }

    function openModal() { modal.showModal(); }

    function resetModal() {
      form.reset();
      preview.classList.add('hidden');
      swatch.classList.add('hidden');
      hexEl.textContent = '#—';
      paletteWrap.innerHTML = '';
      delete preview.dataset.src; delete preview.dataset.hex; delete preview.dataset.palette;
    }

    function handleFile(file) {
      if (!file) return;
      const reader = new FileReader();
      reader.onload = e => {
        const img = new Image();
        img.onload = () => {
          const ctx = preview.getContext('2d');
          const maxW = 900; const scale = Math.min(1, maxW / img.width);
          preview.width = Math.floor(img.width * scale);
          preview.height = Math.floor(img.height * scale);
          ctx.drawImage(img, 0, 0, preview.width, preview.height);
          preview.classList.remove('hidden');
          // Compute palette via k-means (k=5)
          const { avgHex, palette } = extractPalette(preview, 5, 8);
          swatch.classList.remove('hidden');
          paletteWrap.innerHTML = '';
          palette.forEach(hex=>{
            const d=document.createElement('div'); d.className='palette-dot'; d.style.background=hex; paletteWrap.appendChild(d);
          });
          hexEl.textContent = avgHex + '  ·  ' + palette.join(', ');
          preview.dataset.src = e.target.result;
          preview.dataset.hex = avgHex;
          preview.dataset.palette = JSON.stringify(palette);
        };
        img.src = e.target.result;
      };
      reader.readAsDataURL(file);
    }

    function extractPalette(canvas, k=5, iterations=8){
      const ctx = canvas.getContext('2d', { willReadFrequently: true });
      const { width, height } = canvas;
      const step = 8; const samples=[];
      try {
        const data = ctx.getImageData(0,0,width,height).data;
        for(let y=0;y<height;y+=step){
          for(let x=0;x<width;x+=step){
            const i=(y*width+x)*4; const r=data[i], g=data[i+1], b=data[i+2], a=data[i+3];
            if(a>200) samples.push([r,g,b]);
          }
        }
      } catch(e){ /* ignore CORS */ }
      if(samples.length===0) return { avgHex:'#999999', palette:['#999999'] };
      // init centers randomly
      const centers = Array.from({length:k}, ()=> samples[(Math.random()*samples.length)|0].slice());
      for(let it=0; it<iterations; it++){
        const buckets = Array.from({length:k}, ()=>({sum:[0,0,0], n:0}));
        for(const p of samples){
          let bi=0, bd=1e9; for(let i=0;i<k;i++){ const c=centers[i]; const d=(p[0]-c[0])**2+(p[1]-c[1])**2+(p[2]-c[2])**2; if(d<bd){bd=d;bi=i;} }
          const b=buckets[bi]; b.sum[0]+=p[0]; b.sum[1]+=p[1]; b.sum[2]+=p[2]; b.n++;
        }
        for(let i=0;i<k;i++){ if(buckets[i].n>0) centers[i]=buckets[i].sum.map(x=>Math.round(x/buckets[i].n)); }
      }
      const hex = v=> '#'+v.map(x=>x.toString(16).padStart(2,'0')).join('');
      const palette = centers.map(c=>hex(c));
      const avg = samples.reduce((a,p)=>[a[0]+p[0],a[1]+p[1],a[2]+p[2]],[0,0,0]).map(x=>Math.round(x/samples.length));
      const avgHex = hex(avg);
      return { avgHex, palette };
    }

    function onSave(e){
      e.preventDefault();
      const title = document.getElementById('title').value.trim(); if (!title) return;
      const place = document.getElementById('place').value.trim();
      const time = document.getElementById('timeInput').value;
      const note = document.getElementById('note').value.trim();
      const tags = [...document.querySelectorAll('.tagChk:checked')].map(i=>i.value);
      const src = preview.dataset.src || '';
      const hex = preview.dataset.hex || '#999999';
      const palette = preview.dataset.palette ? JSON.parse(preview.dataset.palette) : [];
      const lat = parseFloat(document.getElementById('lat').value);
      const lng = parseFloat(document.getElementById('lng').value);
      addItem({ title, place, time, note, tags, src, hex, palette, lat: isNaN(lat)?null:lat, lng: isNaN(lng)?null:lng, date: new Date().toISOString() });
      modal.close();
      resetModal();
    }

    function addItem(item){ state.items.unshift(item); persist(); render(); refreshMap(); }

    function render(){
      gallery.innerHTML = '';
      const { q, tag, time } = state.filter;
      const items = state.items.filter(it => {
        const matchQ = !q || (it.title+it.place+it.note).toLowerCase().includes(q);
        const matchTag = !tag || it.tags?.includes(tag);
        const matchTime = !time || it.time===time;
        return matchQ && matchTag && matchTime;
      });
      empty.classList.toggle('hidden', items.length>0);
      for(const it of items){
        const node = document.getElementById('cardTpl').content.firstElementChild.cloneNode(true);
        const img = node.querySelector('img'); img.src = it.src; img.alt = it.title;
        const badge = node.querySelector('[data-badge]');
        badge.textContent = it.time || '';
        badge.style.background = (it.hex||'#999') + 'cc';
        const chipWrap = node.querySelector('[data-chip]');
        (it.palette && it.palette.length? it.palette : [it.hex]).slice(0,5).forEach(h=>{ const d=document.createElement('div'); d.className='palette-dot'; d.style.background=h; chipWrap.appendChild(d); });
        node.querySelector('[data-title]').textContent = it.title;
        node.querySelector('[data-meta]').textContent = [it.place, new Date(it.date).toLocaleDateString('ko-KR')].filter(Boolean).join(' · ');
        node.querySelector('[data-note]').textContent = it.note;
        const tagsWrap = node.querySelector('[data-tags]');
        (it.tags||[]).forEach(t=>{ const span = document.createElement('span'); span.className = 'text-xs px-2 py-1 rounded-md border border-slate-300'; span.textContent = t; tagsWrap.appendChild(span); });
        gallery.appendChild(node);
      }
    }

    // --- Board ---
    function postBoard(){
      const name = document.getElementById('bName').value.trim() || '익명';
      const title = document.getElementById('bTitle').value.trim();
      const body = document.getElementById('bBody').value.trim();
      if(!title || !body) return;
      state.board.unshift({ name, title, body, date: new Date().toISOString() });
      document.getElementById('bName').value=''; document.getElementById('bTitle').value=''; document.getElementById('bBody').value='';
      persist(); renderBoard();
    }

    function renderBoard(){
      const wrap = document.getElementById('boardList');
      const emptyEl = document.getElementById('boardEmpty');
      wrap.innerHTML='';
      emptyEl.classList.toggle('hidden', state.board.length>0);
      for(const b of state.board){
        const node = document.getElementById('boardTpl').content.firstElementChild.cloneNode(true);
        node.querySelector('[data-btitle]').textContent = b.title;
        node.querySelector('[data-bmeta]').textContent = `${b.name} · ${new Date(b.date).toLocaleString('ko-KR')}`;
        node.querySelector('[data-bbody]').textContent = b.body;
        wrap.appendChild(node);
      }
    }

    // --- Map ---
    let mainMap, markersLayer;
    function initMainMap(){
      if(mainMap){ setTimeout(()=> mainMap.invalidateSize(), 50); return; }
      mainMap = L.map('map').setView([37.559,126.936], 14);
      L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', { attribution: '&copy; OpenStreetMap' }).addTo(mainMap);
      markersLayer = L.layerGroup().addTo(mainMap);
      refreshMap();
    }

    function refreshMap(){
      if(!markersLayer) return;
      markersLayer.clearLayers();
      const points = state.items.filter(i=> typeof i.lat==='number' && typeof i.lng==='number');
      points.forEach(it=>{
        const m = L.circleMarker([it.lat,it.lng], { radius:6, weight:1, color:'#333', fillColor: it.hex||'#3388ff', fillOpacity:0.9 });
        const pal = (it.palette && it.palette.length ? it.palette : [it.hex]).slice(0,5).map(h=>`<span style="display:inline-block;width:10px;height:10px;border-radius:50%;background:${h};margin-right:2px;border:1px solid #fff"></span>`).join('');
        m.bindPopup(`<b>${escapeHtml(it.title)}</b><br/>${escapeHtml(it.place||'')}<br/><small>${new Date(it.date).toLocaleString('ko-KR')}</small><div style="margin-top:6px">${pal}</div>`);
        m.addTo(markersLayer);
      });
    }

    function escapeHtml(str=''){ return str.replace(/[&<>"']/g, s=>({"&":"&amp;","<":"&lt;",
      ">":"&gt;","\"":"&quot;","'":"&#39;"}[s])); }

    // --- Persistence ---
    function persist(){ localStorage.setItem('shinchon-colors-pack', JSON.stringify({items:state.items, board:state.board})); }
    function restore(){
      try { const obj = JSON.parse(localStorage.getItem('shinchon-colors-pack')||'{}'); state.items = obj.items||[]; state.board = obj.board||[]; }
      catch(e){ state.items=[]; state.board=[]; }
      render(); renderBoard();
    }

    function onExport(){
      const blob = new Blob([JSON.stringify({items:state.items, board:state.board}, null, 2)], {type:'application/json'});
      const url = URL.createObjectURL(blob); const a=document.createElement('a'); a.href=url; a.download='shinchon-color-atlas.json'; a.click(); URL.revokeObjectURL(url);
    }

    function onImport(e){
      const file = e.target.files[0]; if(!file) return;
      const reader = new FileReader(); reader.onload = ev => {
        try { const obj = JSON.parse(ev.target.result); if(obj && (obj.items||obj.board)){ state.items = obj.items||[]; state.board = obj.board||[]; persist(); render(); renderBoard(); refreshMap(); } }
        catch(err){ alert('JSON 형식을 확인해 주세요.'); }
      }; reader.readAsText(file); e.target.value='';
    }

    // Init
    restore();
  </script>
</body>
</html>
