[GGFC MOMENTS_260202.html](https://github.com/user-attachments/files/25010723/GGFC.MOMENTS_260202.html)
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GGFC 베스트 모먼트</title>
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+KR:wght@300;500;700;900&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/flatpickr/dist/flatpickr.min.css">
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/flatpickr/dist/themes/dark.css">
    
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/flatpickr"></script>
    <script src="https://cdn.jsdelivr.net/npm/flatpickr/dist/l10n/ko.js"></script> <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.10.1/firebase-firestore.js"></script>

    <style>
        /* === 1. 테마 및 기본 설정 === */
        :root {
            --bg-color: #121212;
            --card-bg: #1e1e1e;
            --text-main: #f5f5f5;
            --text-sub: #b0b3b8;
            --primary: #3b82f6;
            --accent: #ffd700;
            --danger: #ef4444;
            --container-width: 1000px;
        }

        body { 
            font-family: 'Noto Sans KR', sans-serif; 
            background-color: var(--bg-color); 
            margin: 0; padding: 0; 
            color: var(--text-main); 
            user-select: none;
        }

        ::-webkit-scrollbar { width: 6px; }
        ::-webkit-scrollbar-track { background: var(--bg-color); }
        ::-webkit-scrollbar-thumb { background: #444; border-radius: 3px; }

        .container { 
            max-width: var(--container-width); 
            margin: 0 auto; 
            min-height: 100vh; 
            padding: 15px; 
            box-sizing: border-box;
        }

        /* === 2. 헤더 === */
        .pc-header {
            display: none; justify-content: space-between; align-items: center;
            padding: 10px 0; margin-bottom: 20px; border-bottom: 1px solid #333;
        }
        .logo { font-size: 18px; font-weight: 900; cursor: pointer; color: var(--text-main); }
        .logo span { color: var(--primary); margin-left:5px; }
        .pc-nav { display: flex; gap: 15px; }
        .pc-nav-btn {
            background: none; border: none; color: var(--text-sub); font-size: 14px; font-weight: 500; cursor: pointer;
        }
        .pc-nav-btn:hover, .pc-nav-btn.active { color: var(--primary); font-weight: 700; }

        .highlight-title {
            font-size: 20px; font-weight: 900; margin: 15px 0 15px 0;
            padding-left: 10px; border-left: 4px solid var(--primary);
        }

        /* === 3. 홈 화면 & 단체 사진 (요청 2: 너비 일치) === */
        .home-hero {
            text-align: center; padding: 30px 15px;
            background: linear-gradient(135deg, #1e1e1e 0%, #000000 100%);
            border-radius: 12px; margin-bottom: 20px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.4);
            /* 너비가 100%이므로 사진 박스와 자동 일치 */
        }
        .home-hero h1 { font-size: 22px; margin: 0 0 8px 0; font-weight: 900; }
        .home-hero p { color: var(--text-sub); margin: 0; font-size: 13px; }

        .team-photo-box {
            position: relative; 
            width: 100%; /* 가로 꽉 채움 */
            /* max-width 제거하여 home-hero와 동일하게 */
            height: 225px; 
            margin: 0 auto 20px auto; border-radius: 12px; overflow: hidden;
            border: 1px solid #333; background: #000; cursor: grab;
        }
        .team-photo-box:active { cursor: grabbing; }
        .team-photo-box img {
            width: 100%; height: 100%; object-fit: cover; object-position: 50% 50%;
            transition: object-position 0.1s; pointer-events: none;
        }
        .photo-edit-btn {
            position: absolute; bottom: 10px; right: 10px;
            background: rgba(0,0,0,0.6); color: white; border: 1px solid #555;
            border-radius: 50%; width: 32px; height: 32px; cursor: pointer;
            display: flex; align-items: center; justify-content: center; z-index: 10;
        }
        .photo-edit-btn:hover { background: var(--primary); border-color: var(--primary); }
        .drag-guide {
            position: absolute; top: 10px; left: 50%; transform: translateX(-50%);
            background: rgba(0,0,0,0.4); padding: 4px 10px; border-radius: 10px;
            color: #ddd; font-size: 11px; pointer-events: none; z-index: 5;
        }

        .home-search-box {
            position: relative; max-width: 400px; margin: 15px auto 0 auto; display: flex; gap: 8px;
        }
        .home-search-box input {
            padding: 10px 15px; border-radius: 30px; border: 1px solid #444; background: rgba(0,0,0,0.5);
            color: white; font-size: 14px; flex:1;
        }
        .home-search-box button {
            width: 40px; height: 40px; border-radius: 50%; background: var(--primary); border: none;
            color: white; cursor: pointer;
        }

        .menu-grid { display: grid; grid-template-columns: 1fr; gap: 10px; max-width: 500px; margin: 0 auto; }
        .menu-btn { 
            background: var(--card-bg); border: 1px solid #333; padding: 15px; border-radius: 10px; 
            color: var(--text-main); font-size: 15px; font-weight: bold; cursor: pointer; 
            display: flex; align-items: center; justify-content: space-between;
        }

        /* === 4. 갤러리 상단 === */
        .category-tabs { display: flex; gap: 8px; overflow-x: auto; padding-bottom: 5px; margin-bottom: 15px; scrollbar-width: none; }
        .category-tabs::-webkit-scrollbar { display: none; }
        .tab-btn {
            white-space: nowrap; padding: 8px 16px; border-radius: 20px; font-size: 13px; font-weight: bold;
            background: #2a2a2a; color: #888; border: 1px solid #333; cursor: pointer; transition: 0.2s;
        }
        .tab-btn:hover { color: white; background: #333; }
        .tab-btn.active { background: var(--primary); color: white; border-color: var(--primary); }

        .gallery-tools { margin-bottom: 15px; display: flex; flex-direction: column; gap: 10px; }
        .gallery-search-bar { display: flex; gap: 8px; }
        .gallery-search-bar input { flex: 1; padding: 10px; background: var(--card-bg); border: 1px solid #444; color: white; border-radius: 8px; }
        .gallery-search-bar button { padding: 0 15px; background: #333; color: white; border-radius: 8px; border: 1px solid #444; cursor: pointer; }

        .admin-filter-box {
            display: none; background: #252525; padding: 15px; border-radius: 8px; border: 1px solid #444;
            align-items: center; gap: 10px; flex-wrap: wrap;
        }
        .admin-filter-box.show { display: flex; }
        
        /* [요청 1] Flatpickr 달력 인풋 스타일 */
        input.calendar-input {
            background: #121212; border: 1px solid #555; color: white; padding: 8px; border-radius: 6px; font-family: inherit; cursor: pointer;
            width: 140px; text-align: center;
        }
        .filter-toggle-btn {
            font-size: 12px; background: none; border: 1px solid #444; color: #888; padding: 5px 10px; border-radius: 4px; cursor: pointer; margin-left: auto;
        }

        /* === 5. 리스트 뷰 & 카드 === */
        .video-list-container { display: flex; flex-direction: column; gap: 10px; }
        .list-item { 
            background: var(--card-bg); border-radius: 8px; padding: 12px 15px; 
            border: 1px solid #333; display: flex; align-items: center; justify-content: space-between;
            transition: 0.2s; gap: 15px;
        }
        .list-item:hover { transform: translateX(3px); border-color: #555; background: #252525; }
        
        .list-info { flex: 1; min-width: 0; display: flex; flex-direction: column; gap: 4px; }
        .list-title { 
            font-size: 14px; font-weight: bold; color: var(--text-main); text-decoration: none; 
            white-space: nowrap; overflow: hidden; text-overflow: ellipsis; display: block;
        }
        .list-title i { font-size: 11px; color: var(--primary); margin-left: 5px; opacity: 0.7; }
        .list-sub { display: flex; align-items: center; gap: 8px; font-size: 12px; color: #888; flex-wrap: wrap; }
        .protagonist { color: var(--accent); font-weight: 500; }
        .badge-cat { background: rgba(59,130,246,0.15); color: #93c5fd; padding: 2px 6px; border-radius: 4px; font-size: 11px; }
        .list-date { color: #666; font-size: 11px; margin-left: auto; }

        .list-actions { display: flex; align-items: center; gap: 10px; flex-shrink: 0; }
        .like-btn { 
            background: transparent; border: 1px solid #444; color: #ccc; 
            padding: 6px 12px; border-radius: 6px; cursor: pointer; font-size: 12px; font-weight: 500; 
            display: flex; align-items: center; gap: 4px; transition: 0.2s;
        }
        .like-btn.liked { background: #e0245e; color: white; border-color: #e0245e; }
        .delete-btn-icon { color: #555; cursor: pointer; font-size: 14px; padding: 5px; }
        .delete-btn-icon:hover { color: var(--danger); }
        .list-rank {
            width: 24px; height: 24px; border-radius: 50%; background: #333; color: white;
            display: flex; align-items: center; justify-content: center; font-size: 12px; font-weight: bold;
            margin-right: 5px; flex-shrink: 0;
        }
        .rank-1 { background: var(--accent); color: black; }
        .rank-2 { background: #c0c0c0; color: black; }
        .rank-3 { background: #cd7f32; color: black; }

        /* [요청 3] 기간별 순위 페이지 카드 */
        .period-grid { display: grid; grid-template-columns: 1fr; gap: 15px; }
        .period-card {
            background: var(--card-bg); border: 1px solid #444; padding: 20px; border-radius: 12px;
            cursor: pointer; transition: 0.2s; position: relative;
        }
        .period-card:hover { border-color: var(--primary); transform: translateY(-3px); }
        .period-title { font-size: 18px; font-weight: bold; margin-bottom: 5px; color: var(--text-main); }
        .period-date { font-size: 13px; color: #888; display: flex; align-items: center; gap: 5px; }
        .period-add-box {
            background: #252525; border: 1px dashed #555; padding: 20px; border-radius: 12px;
            text-align: center; color: #888; cursor: pointer; margin-bottom: 20px;
        }
        .period-add-box:hover { color: var(--primary); border-color: var(--primary); }
        
        /* 기간 생성 폼 */
        .create-period-form {
            display: none; background: #1a1a1a; padding: 20px; border-radius: 12px; border: 1px solid var(--primary);
            margin-bottom: 20px;
        }
        .create-period-form.show { display: block; animation: fadeIn 0.3s; }

        /* 기타 */
        .input-box { background: var(--card-bg); padding: 20px; border-radius: 12px; max-width: 600px; margin: 0 auto; border: 1px solid #333; }
        .chart-box { background: var(--card-bg); padding: 20px; border-radius: 12px; max-width: 600px; margin: 0 auto 20px auto; border: 1px solid #333; }
        .form-group { margin-bottom: 15px; }
        label { display: block; margin-bottom: 5px; font-size: 13px; color: #aaa; }
        input, select { width: 100%; padding: 10px; background: #121212; border: 1px solid #444; border-radius: 6px; color: white; box-sizing: border-box; }
        button.action-btn { width: 100%; background: var(--primary); padding: 12px; border: none; border-radius: 6px; color: white; font-weight: bold; cursor: pointer; }

        .view-section { display: none; }
        .view-section.active { display: block; }

        .nav-bar { display: flex; align-items: center; margin-bottom: 15px; padding-bottom: 10px; border-bottom: 1px solid #333; }
        .back-btn { background: none; border: none; color: #aaa; font-weight: bold; cursor: pointer; }
        .page-title { margin-left: auto; color: white; font-weight: bold; }

        @media (min-width: 768px) {
            .pc-header { display: flex; }
            .nav-bar { display: none; } 
            .menu-grid { grid-template-columns: 1fr 1fr; }
            .period-grid { grid-template-columns: 1fr 1fr; }
            .home-hero h1 { font-size: 32px; }
        }
        @keyframes fadeIn { from{opacity:0; transform:translateY(-10px);} to{opacity:1; transform:translateY(0);} }
    </style>
</head>
<body>

<div class="container">

    <header class="pc-header">
        <div class="logo" onclick="switchView('homeView')">⚽ GGFC <span>MOMENTS</span></div>
        <nav class="pc-nav">
            <button class="pc-nav-btn" onclick="switchView('registerView')">영상 등록</button>
            <button class="pc-nav-btn" onclick="switchView('galleryView')">전체 갤러리</button>
            <button class="pc-nav-btn" onclick="switchView('periodListView')">기간별 랭킹</button>
            <button class="pc-nav-btn" onclick="switchView('bestView')">명예의 전당</button>
            <button class="pc-nav-btn" onclick="switchView('statsView')">통계</button>
        </nav>
    </header>

    <div id="homeView" class="view-section active">
        <div class="team-photo-box" id="teamPhotoContainer">
            <div class="drag-guide">↕ 드래그하여 위치 조정</div>
            <img id="teamPhotoDisplay" src="https://via.placeholder.com/1000x225/000000/333333?text=GGFC+Team+Photo" alt="Team Photo">
            <input type="file" id="teamPhotoInput" accept="image/*" style="display:none;" onchange="handlePhotoUpload(this)">
            <button class="photo-edit-btn" onclick="triggerPhotoUpload()" title="사진 변경 (관리자)"><i class="fas fa-camera"></i></button>
        </div>

        <div class="home-hero">
            <h1>GGFC BEST MOMENTS</h1>
            <p>우리 팀의 빛나는 순간을 투표하세요!</p>
            <div class="home-search-box">
                <input type="text" id="homeSearchInput" placeholder="선수명, 제목 검색..." onkeyup="if(window.event.keyCode==13){searchFromHome()}">
                <button onclick="searchFromHome()"><i class="fas fa-search"></i></button>
            </div>
        </div>

        <div class="menu-grid">
            <button class="menu-btn" onclick="switchView('registerView')"><span>📝 영상 등록</span> <span>›</span></button>
            <button class="menu-btn" onclick="switchView('galleryView')"><span>📺 갤러리</span> <span>›</span></button>
            <button class="menu-btn" onclick="switchView('periodListView')"><span>🏆 기간별 랭킹</span> <span>›</span></button>
            <button class="menu-btn" onclick="switchView('bestView')"><span>🏅 명예의 전당</span> <span>›</span></button>
            <button class="menu-btn" onclick="switchView('statsView')"><span>📊 통계</span> <span>›</span></button>
        </div>
    </div>

    <div id="registerView" class="view-section">
        <div class="nav-bar"><button class="back-btn" onclick="switchView('homeView')">← 홈</button><span class="page-title">등록</span></div>
        <h2 class="highlight-title" style="display:none;" class="pc-only-title">📝 영상 등록</h2>
        <div class="input-box">
            <div class="form-group"><label>1. 등록자 이름</label><input type="text" id="userName" placeholder="예: 김매니저"></div>
            <div class="form-group"><label style="color:var(--accent);">2. 영상 속 주인공</label><input type="text" id="videoProtagonist" placeholder="예: 이강인"></div>
            <div class="form-group"><label>3. 카테고리</label>
                <select id="videoCategory">
                    <option value="" disabled selected>선택</option>
                    <option value="⚽ 골">⚽ 골</option><option value="🥅 어시스트">🥅 어시스트</option><option value="👍 퍼포먼스">👍 퍼포먼스</option>
                    <option value="🧤 선방">🧤 선방</option><option value="🪩 세레머니">🪩 세레머니</option><option value="🤝 페어 플레이">🤝 페어 플레이</option>
                    <option value="🍯 꿀잼 순간">🍯 꿀잼 순간</option>
                </select>
            </div>
            <div class="form-group"><label>4. 제목</label><input type="text" id="videoTitle" placeholder="한줄 설명"></div>
            <div class="form-group"><label>5. 유튜브 링크</label><input type="text" id="videoUrl" placeholder="링크 붙여넣기"></div>
            <button class="action-btn" id="addBtn" onclick="uploadVideo()">등록 완료</button>
        </div>
    </div>

    <div id="galleryView" class="view-section">
        <div class="nav-bar"><button class="back-btn" onclick="switchView('homeView')">← 홈</button><span class="page-title">갤러리</span></div>
        <h2 class="highlight-title">📺 갤러리</h2>
        <div class="category-tabs" id="categoryTabs"></div>
        <div class="gallery-tools">
            <div class="gallery-search-bar">
                <input type="text" id="gallerySearchInput" placeholder="검색어..." onkeyup="if(window.event.keyCode==13){searchGallery()}">
                <button onclick="searchGallery()"><i class="fas fa-search"></i></button>
            </div>
            <div style="display:flex; justify-content:flex-end;">
                <button class="filter-toggle-btn" onclick="toggleAdminFilter()"><i class="fas fa-filter"></i> 날짜 필터 (관리자)</button>
            </div>
            <div class="admin-filter-box" id="adminFilterBox">
                <span style="font-size:12px; color:#aaa;">기간:</span>
                <input type="text" class="calendar-input" id="filterStartDate" placeholder="시작일" onchange="renderGallery()">
                <span style="color:#aaa;">~</span>
                <input type="text" class="calendar-input" id="filterEndDate" placeholder="종료일" onchange="renderGallery()">
                <button style="background:#444; color:white; border:none; padding:8px 12px; border-radius:6px; font-size:11px; cursor:pointer;" onclick="resetDateFilter()">초기화</button>
            </div>
        </div>
        <div id="galleryList" class="video-list-container">
            <p style="text-align: center; color: #666; margin-top: 50px;">로딩 중...</p>
        </div>
    </div>

    <div id="periodListView" class="view-section">
        <div class="nav-bar"><button class="back-btn" onclick="switchView('homeView')">← 홈</button><span class="page-title">기간별 랭킹</span></div>
        <h2 class="highlight-title">🏆 기간별 랭킹</h2>
        
        <div class="period-add-box" onclick="togglePeriodForm()">
            <i class="fas fa-plus-circle"></i> 관리자: 새 기간(시즌) 카드 만들기
        </div>

        <div class="create-period-form" id="createPeriodForm">
            <h3 style="margin-top:0; color:var(--primary);">새 기간 생성</h3>
            <div class="form-group"><label>카드 제목 (예: 2024 상반기)</label><input type="text" id="newPeriodTitle"></div>
            <div class="form-group"><label>시작 날짜</label><input type="text" class="calendar-input" id="newPeriodStart" style="width:100%;"></div>
            <div class="form-group"><label>종료 날짜</label><input type="text" class="calendar-input" id="newPeriodEnd" style="width:100%;"></div>
            <div style="display:flex; gap:10px;">
                <button class="action-btn" onclick="createPeriod()">생성하기</button>
                <button class="action-btn" style="background:#555;" onclick="togglePeriodForm()">취소</button>
            </div>
        </div>

        <div id="periodCardList" class="period-grid">
            <p style="text-align: center; color: #666; width:100%;">불러오는 중...</p>
        </div>
    </div>

    <div id="periodDetailView" class="view-section">
        <div class="nav-bar"><button class="back-btn" onclick="switchView('periodListView')">← 목록</button><span class="page-title">상세 랭킹</span></div>
        <h2 class="highlight-title" id="periodDetailTitle">상세 랭킹</h2>
        <p id="periodDetailDate" style="color:#888; margin-bottom:20px; font-size:14px;"></p>
        <div id="periodVideoList" class="video-list-container"></div>
    </div>

    <div id="bestView" class="view-section">
        <div class="nav-bar"><button class="back-btn" onclick="switchView('homeView')">← 홈</button><span class="page-title">랭킹</span></div>
        <h2 class="highlight-title">🏆 명예의 전당 (Top 10)</h2>
        <div id="bestList" class="video-list-container">
            <p style="text-align: center; color: #666; margin-top: 50px;">집계 중...</p>
        </div>
    </div>

    <div id="statsView" class="view-section">
        <div class="nav-bar"><button class="back-btn" onclick="switchView('homeView')">← 홈</button><span class="page-title">통계</span></div>
        <h2 class="highlight-title">📊 현황</h2>
        <div class="chart-box">
            <h3 style="margin-top:0; color:var(--text-main); font-size:16px;">카테고리 비율</h3>
            <div style="height:250px;"><canvas id="categoryChart"></canvas></div>
            <p id="totalCount" style="color:var(--text-sub); font-size:12px; margin-top:10px;">총 0개</p>
        </div>
        <div class="chart-box">
            <h3 style="margin-top:0; color:var(--text-main); font-size:16px;">등록자 랭킹</h3>
            <div style="height:250px;"><canvas id="uploaderChart"></canvas></div>
        </div>
    </div>

</div>

<script>
    const firebaseConfig = {
      apiKey: "AIzaSyCgerolBaDkmbN75y1HJpxKrZgBd4q3kvE",
      authDomain: "ggfc-voting.firebaseapp.com",
      projectId: "ggfc-voting",
      storageBucket: "ggfc-voting.firebasestorage.app",
      messagingSenderId: "615904673843",
      appId: "1:615904673843:web:fd20f417aa324820232346"
    };
    if (!firebase.apps.length) firebase.initializeApp(firebaseConfig);
    const db = firebase.firestore();
    const collectionName = "ggfc_links_v2";
    const settingsCollection = "ggfc_settings";
    const periodCollection = "ggfc_periods";

    const getVoterId = () => {
        let id = localStorage.getItem('ggfcVoterId');
        if (!id) { id = 'user_' + Date.now() + Math.random().toString(36).substr(2, 5); localStorage.setItem('ggfcVoterId', id); }
        return id;
    };
    const currentVoterId = getVoterId();
    const getMyVotes = () => JSON.parse(localStorage.getItem('myVotedVideos') || "[]");

    let galleryUnsubscribe = null, periodUnsubscribe = null, bestUnsubscribe = null;
    let catChartInstance = null, uploaderChartInstance = null;
    let rawGalleryData = [];
    let currentSearchKeyword = "", currentCategory = "전체";
    let currentPeriodDates = { start: null, end: null };
    const CATEGORIES = ["전체", "⚽ 골", "🥅 어시스트", "👍 퍼포먼스", "🧤 선방", "🪩 세레머니", "🤝 페어 플레이", "🍯 꿀잼 순간", "기타"];

    window.onload = function() {
        renderCategoryTabs();
        loadTeamPhoto();
        initPhotoDrag();
        loadGalleryData();
        
        // [요청 1] Flatpickr 달력 초기화
        flatpickr(".calendar-input", {
            locale: "ko",
            dateFormat: "Y-m-d",
            theme: "dark"
        });
    };

    function renderCategoryTabs() {
        const tabContainer = document.getElementById('categoryTabs');
        let html = '';
        CATEGORIES.forEach(cat => {
            const activeClass = cat === currentCategory ? 'active' : '';
            html += `<button class="tab-btn ${activeClass}" onclick="selectCategory('${cat}')">${cat}</button>`;
        });
        tabContainer.innerHTML = html;
    }

    function selectCategory(cat) {
        currentCategory = cat;
        renderCategoryTabs();
        renderGallery();
    }

    function switchView(viewId) {
        document.querySelectorAll('.view-section').forEach(el => el.classList.remove('active'));
        document.getElementById(viewId).classList.add('active');
        document.querySelectorAll('.pc-nav-btn').forEach(btn => btn.classList.remove('active'));
        
        const navMap = { 'registerView':0, 'galleryView':1, 'periodListView':2, 'bestView':3, 'statsView':4 };
        if(navMap[viewId] !== undefined) {
             const btns = document.querySelectorAll('.pc-nav-btn');
             if(btns[navMap[viewId]]) btns[navMap[viewId]].classList.add('active');
        }

        const regTitle = document.querySelector('#registerView .highlight-title');
        if(regTitle) regTitle.style.display = (window.innerWidth >= 768) ? 'block' : 'none';

        if (bestUnsubscribe && viewId !== 'bestView') bestUnsubscribe();
        if (periodUnsubscribe && viewId !== 'periodListView') periodUnsubscribe();

        if (viewId === 'galleryView') renderGallery(); 
        else if (viewId === 'bestView') loadBestData();
        else if (viewId === 'statsView') loadStatsData();
        else if (viewId === 'periodListView') loadPeriods();
    }

    function loadTeamPhoto() {
        db.collection(settingsCollection).doc("home").get().then(doc => {
            if(doc.exists) {
                const data = doc.data();
                const img = document.getElementById('teamPhotoDisplay');
                if(data.teamPhotoUrl) img.src = data.teamPhotoUrl;
                if(data.photoPosition) img.style.objectPosition = `50% ${data.photoPosition}%`;
            }
        });
    }

    function triggerPhotoUpload() {
        const pwd = prompt("관리자 비밀번호를 입력하세요:");
        if(pwd === "ggfc1234") {
            document.getElementById('teamPhotoInput').click();
        } else if(pwd !== null) {
            alert("비밀번호가 틀렸습니다.");
        }
    }

    function handlePhotoUpload(input) {
        if (input.files && input.files[0]) {
            const file = input.files[0];
            if(file.size > 1024 * 1024) { alert("이미지 크기가 1MB를 초과합니다."); return; }
            const reader = new FileReader();
            reader.onload = function(e) {
                const base64String = e.target.result;
                db.collection(settingsCollection).doc("home").set({ teamPhotoUrl: base64String }, { merge: true })
                .then(() => {
                    document.getElementById('teamPhotoDisplay').src = base64String;
                    alert("사진이 변경되었습니다.");
                });
            };
            reader.readAsDataURL(file);
        }
    }

    function initPhotoDrag() {
        const container = document.getElementById('teamPhotoContainer');
        const img = document.getElementById('teamPhotoDisplay');
        let isDragging = false;
        let startY = 0;
        let initialPos = 50;

        container.addEventListener('mousedown', (e) => {
            isDragging = true;
            startY = e.clientY;
            const currentStyle = img.style.objectPosition || "50% 50%";
            initialPos = currentStyle.split(" ").length > 1 ? parseFloat(currentStyle.split(" ")[1]) : 50;
            container.style.cursor = 'grabbing';
        });

        window.addEventListener('mousemove', (e) => {
            if(!isDragging) return;
            e.preventDefault();
            const diff = e.clientY - startY;
            let newPos = initialPos - (diff * 0.2); 
            if(newPos < 0) newPos = 0;
            if(newPos > 100) newPos = 100;
            img.style.objectPosition = `50% ${newPos}%`;
        });

        window.addEventListener('mouseup', () => {
            if(isDragging) {
                isDragging = false;
                container.style.cursor = 'grab';
                const currentStyle = img.style.objectPosition || "50% 50%";
                const finalPos = currentStyle.split(" ").length > 1 ? parseFloat(currentStyle.split(" ")[1]) : 50;
                db.collection(settingsCollection).doc("home").set({ photoPosition: finalPos }, { merge: true });
            }
        });
    }

    function searchFromHome() {
        const val = document.getElementById('homeSearchInput').value.trim();
        if(!val) return alert("검색어를 입력하세요.");
        currentSearchKeyword = val;
        document.getElementById('gallerySearchInput').value = val;
        currentCategory = "전체";
        renderCategoryTabs();
        switchView('galleryView');
        document.getElementById('homeSearchInput').value = "";
    }
    function searchGallery() {
        currentSearchKeyword = document.getElementById('gallerySearchInput').value.trim();
        renderGallery();
    }

    function toggleAdminFilter() {
        const box = document.getElementById('adminFilterBox');
        if(box.classList.contains('show')) {
            box.classList.remove('show');
            resetDateFilter();
        } else {
            const pwd = prompt("관리자 비밀번호:");
            if(pwd === "ggfc1234") box.classList.add('show');
            else if(pwd !== null) alert("비밀번호 오류");
        }
    }
    function resetDateFilter() {
        document.getElementById('filterStartDate').value = "";
        document.getElementById('filterEndDate').value = "";
        renderGallery();
    }

    // [요청 3] 관리자: 기간 카드 생성
    function togglePeriodForm() {
        const form = document.getElementById('createPeriodForm');
        if(form.style.display === 'block') {
            form.style.display = 'none';
        } else {
            const pwd = prompt("관리자 비밀번호:");
            if(pwd === "ggfc1234") {
                form.style.display = 'block';
                form.classList.add('show');
            } else if(pwd !== null) {
                alert("비밀번호 오류");
            }
        }
    }

    function createPeriod() {
        const title = document.getElementById('newPeriodTitle').value;
        const start = document.getElementById('newPeriodStart').value;
        const end = document.getElementById('newPeriodEnd').value;
        
        if(!title || !start || !end) return alert("모든 항목을 입력해주세요.");
        
        db.collection(periodCollection).add({
            title: title, startDate: start, endDate: end,
            timestamp: firebase.firestore.FieldValue.serverTimestamp()
        }).then(() => {
            alert("기간이 생성되었습니다.");
            document.getElementById('newPeriodTitle').value = "";
            document.getElementById('newPeriodStart').value = "";
            document.getElementById('newPeriodEnd').value = "";
            document.getElementById('createPeriodForm').style.display = 'none';
        });
    }

    function loadPeriods() {
        const listDiv = document.getElementById('periodCardList');
        periodUnsubscribe = db.collection(periodCollection).orderBy("timestamp", "desc").onSnapshot(snapshot => {
            if(snapshot.empty) { listDiv.innerHTML = '<p style="text-align:center; color:#666; width:100%;">생성된 기간이 없습니다.</p>'; return; }
            let html = "";
            snapshot.forEach(doc => {
                const d = doc.data();
                html += `
                    <div class="period-card" onclick="openPeriod('${d.title}', '${d.startDate}', '${d.endDate}')">
                        <div class="period-title">${d.title}</div>
                        <div class="period-date"><i class="far fa-calendar-alt"></i> ${d.startDate} ~ ${d.endDate}</div>
                        <i class="fas fa-chevron-right" style="position:absolute; right:20px; top:50%; transform:translateY(-50%); color:#555;"></i>
                    </div>
                `;
            });
            listDiv.innerHTML = html;
        });
    }

    function openPeriod(title, start, end) {
        document.getElementById('periodDetailTitle').innerText = title + " 랭킹";
        document.getElementById('periodDetailDate').innerText = `기간: ${start} ~ ${end}`;
        
        currentPeriodDates.start = new Date(start);
        currentPeriodDates.end = new Date(end);
        currentPeriodDates.end.setHours(23, 59, 59);

        renderPeriodGallery();
        switchView('periodDetailView');
    }

    function renderPeriodGallery() {
        const listDiv = document.getElementById('periodVideoList');
        let filtered = rawGalleryData.map(doc => ({id: doc.id, ...doc.data()}))
        .filter(d => {
            if(!d.timestamp) return false;
            const vDate = d.timestamp.toDate();
            return vDate >= currentPeriodDates.start && vDate <= currentPeriodDates.end;
        });

        filtered.sort((a, b) => b.votes - a.votes);

        if(filtered.length === 0) { listDiv.innerHTML = '<p style="text-align:center;color:#666; margin-top:30px;">해당 기간에 등록된 영상이 없습니다.</p>'; return; }

        let html = "";
        let rank = 1;
        filtered.forEach(d => {
            const dummyDoc = { id: d.id, data: () => d };
            html += createListItemHtml(dummyDoc, rank++);
        });
        listDiv.innerHTML = html;
    }

    function createListItemHtml(doc, rank = null) {
        const d = doc.data();
        const id = doc.id;
        const myVotes = getMyVotes();
        const isVoted = myVotes.includes(id);

        const category = d.category || '기타'; 
        const protagonist = d.protagonist || d.userName;
        
        let dateStr = "";
        if(d.timestamp) {
            const date = d.timestamp.toDate();
            dateStr = `${date.getFullYear()}.${String(date.getMonth()+1).padStart(2,'0')}.${String(date.getDate()).padStart(2,'0')}`;
        }

        let btnClass = isVoted ? "like-btn liked" : "like-btn";
        let btnText = isVoted ? "완료" : "투표";
        let onClick = `onclick="castVote('${id}')"`;
        let heart = isVoted ? "♥" : "♡";

        let rankHtml = '';
        if (rank) {
            let rankClass = rank <= 3 ? `rank-${rank}` : '';
            rankHtml = `<div class="list-rank ${rankClass}">${rank}</div>`;
        }

        return `
            <div class="list-item">
                ${rankHtml}
                <div class="list-info">
                    <a href="${d.watchUrl}" target="_blank" class="list-title">
                        ${d.title} <i class="fas fa-external-link-alt"></i>
                    </a>
                    <div class="list-sub">
                        <span class="badge-cat">${category}</span>
                        <span class="protagonist">🏃 ${protagonist}</span>
                        <span>| 📝 ${d.userName}</span>
                        <span class="list-date">${dateStr}</span>
                    </div>
                </div>
                <div class="list-actions">
                    <button id="btn-${id}" class="${btnClass}" ${onClick}>
                        ${heart} ${btnText} <span style="margin-left:3px; font-weight:bold;">${d.votes}</span>
                    </button>
                    <i class="fas fa-trash-alt delete-btn-icon" onclick="deleteVideo('${id}')"></i>
                </div>
            </div>
        `;
    }

    function loadGalleryData() {
        galleryUnsubscribe = db.collection(collectionName).orderBy("timestamp", "desc").onSnapshot((snapshot) => {
            rawGalleryData = [];
            snapshot.forEach(doc => rawGalleryData.push(doc));
            if(document.getElementById('galleryView').classList.contains('active')) renderGallery();
            if(document.getElementById('periodDetailView').classList.contains('active')) renderPeriodGallery();
        });
    }

    function renderGallery() {
        const listDiv = document.getElementById('galleryList');
        if(rawGalleryData.length === 0) { listDiv.innerHTML = '<p style="text-align:center;color:#666; margin-top:30px;">등록된 영상이 없습니다.</p>'; return; }

        const startDateVal = document.getElementById('filterStartDate').value;
        const endDateVal = document.getElementById('filterEndDate').value;
        const start = startDateVal ? new Date(startDateVal) : null;
        const end = endDateVal ? new Date(endDateVal) : null;
        if(end) end.setHours(23, 59, 59);

        const filtered = rawGalleryData.filter(doc => {
            const d = doc.data();
            if (currentCategory !== "전체" && d.category !== currentCategory) {
                 if(currentCategory === "기타" && CATEGORIES.includes(d.category)) return false; 
                 if(currentCategory !== "기타" && d.category !== currentCategory) return false;
            }
            if (currentSearchKeyword) {
                const k = currentSearchKeyword.toLowerCase();
                const title = (d.title || "").toLowerCase();
                const p = (d.protagonist || "").toLowerCase();
                const u = (d.userName || "").toLowerCase();
                if (!title.includes(k) && !p.includes(k) && !u.includes(k)) return false;
            }
            if (start || end) {
                if(!d.timestamp) return false;
                const vDate = d.timestamp.toDate();
                if(start && vDate < start) return false;
                if(end && vDate > end) return false;
            }
            return true;
        });

        if (filtered.length === 0) { listDiv.innerHTML = `<p style="text-align:center;color:#666; margin-top:30px;">결과가 없습니다.</p>`; return; }
        let html = "";
        filtered.forEach(doc => html += createListItemHtml(doc));
        listDiv.innerHTML = html;
    }

    function loadBestData() {
        const listDiv = document.getElementById('bestList');
        bestUnsubscribe = db.collection(collectionName).orderBy("votes", "desc").onSnapshot((snapshot) => {
            if (snapshot.empty) { listDiv.innerHTML = '<p style="text-align:center;color:#666; margin-top:30px;">데이터 없음</p>'; return; }
            const cats = {};
            snapshot.forEach(doc => {
                const d = doc.data();
                if(d.votes === 0) return;
                let c = d.category || "기타";
                if(!CATEGORIES.includes(c)) c = "기타";
                if(!cats[c]) cats[c] = [];
                cats[c].push(doc);
            });
            let html = "";
            CATEGORIES.slice(1).forEach(cat => {
                if(cats[cat] && cats[cat].length > 0) {
                    html += `<div style="margin: 20px 0 10px 0; color:var(--primary); font-weight:bold; font-size:16px;">🏆 ${cat} Top 10</div>`;
                    cats[cat].slice(0, 10).forEach((doc, idx) => { html += createListItemHtml(doc, idx + 1); });
                }
            });
            listDiv.innerHTML = html || '<p style="text-align:center;color:#666; margin-top:30px;">아직 투표가 진행되지 않았습니다.</p>';
        });
    }

    function loadStatsData() {
        if (catChartInstance) catChartInstance.destroy();
        if (uploaderChartInstance) uploaderChartInstance.destroy();
        db.collection(collectionName).get().then((snapshot) => {
            const catCounts = {}; const uploaderCounts = {}; let total = 0;
            CATEGORIES.slice(1).forEach(c => catCounts[c] = 0);
            snapshot.forEach(doc => {
                const d = doc.data();
                const c = d.category && catCounts.hasOwnProperty(d.category) ? d.category : "기타";
                if(!catCounts[c]) catCounts[c] = 0; catCounts[c]++;
                const u = d.userName || "익명"; uploaderCounts[u] = (uploaderCounts[u] || 0) + 1; total++;
            });
            document.getElementById('totalCount').innerText = `총 등록수: ${total}`;
            Chart.defaults.color = '#ccc'; Chart.defaults.borderColor = '#444';
            const cLabels = Object.keys(catCounts).filter(k => catCounts[k] > 0);
            const cVals = cLabels.map(k => catCounts[k]);
            catChartInstance = new Chart(document.getElementById('categoryChart'), {
                type: 'doughnut',
                data: { labels: cLabels, datasets: [{ data: cVals, backgroundColor: ['#3b82f6','#ef4444','#f59e0b','#10b981','#8b5cf6','#ec4899','#6366f1','#9ca3af'], borderWidth:0 }] },
                options: { responsive:true, maintainAspectRatio:false, plugins:{ legend:{position:'right'} } }
            });
            const sortedUp = Object.entries(uploaderCounts).sort((a,b)=>b[1]-a[1]).slice(0,10);
            uploaderChartInstance = new Chart(document.getElementById('uploaderChart'), {
                type: 'bar',
                data: { labels: sortedUp.map(i=>i[0]), datasets: [{ label:'등록수', data: sortedUp.map(i=>i[1]), backgroundColor:'#3b82f6', borderRadius:4 }] },
                options: { responsive:true, maintainAspectRatio:false, indexAxis:'y', plugins:{legend:{display:false}}, scales:{x:{grid:{color:'#333'}}, y:{grid:{display:false}}} }
            });
        });
    }

    async function getWatchUrl(rawUrl) {
        let url = rawUrl.trim();
        if (url.includes('?si=')) url = url.split('?si=')[0];
        if (url.includes('&si=')) url = url.split('&si=')[0];
        try {
            const urlObj = new URL(url);
            const v = new URLSearchParams(urlObj.search).get('v');
            if (v) return { type: 'video', url: `https://www.youtube.com/watch?v=${v}` };
            if (urlObj.hostname === 'youtu.be') return { type: 'video', url: `https://www.youtube.com/watch?v=${urlObj.pathname.slice(1)}` };
            if (url.includes('/clip/')) {
                const res = await fetch(`https://noembed.com/embed?url=${encodeURIComponent(url)}`);
                const d = await res.json();
                if (d.url) return { type: 'clip', url: d.url };
            }
        } catch (e) {}
        return null;
    }

    async function uploadVideo() {
        const name = document.getElementById('userName').value.trim();
        const pro = document.getElementById('videoProtagonist').value.trim();
        const cat = document.getElementById('videoCategory').value;
        const tit = document.getElementById('videoTitle').value.trim();
        const url = document.getElementById('videoUrl').value.trim();

        if (!name || !pro || !cat || !tit || !url) return alert("모든 항목을 입력해주세요.");
        const btn = document.getElementById('addBtn');
        btn.disabled = true; btn.innerText = "처리중...";

        try {
            const vidData = await getWatchUrl(url);
            if (!vidData) throw new Error("유효한 유튜브 링크가 아닙니다.");
            await db.collection(collectionName).add({
                userName: name, protagonist: pro, category: cat, title: tit,
                watchUrl: vidData.url, type: vidData.type, votes: 0,
                timestamp: firebase.firestore.FieldValue.serverTimestamp()
            });
            alert("완료!");
            document.getElementById('videoTitle').value = '';
            document.getElementById('videoUrl').value = '';
            switchView('galleryView');
        } catch (e) { alert("Err: " + e.message); } 
        finally { btn.disabled = false; btn.innerText = "등록 완료"; }
    }

    function castVote(id) {
        const vRef = db.collection(collectionName).doc(id);
        const uRef = vRef.collection("votedBy").doc(currentVoterId);
        
        db.runTransaction(async t => {
            const d = await t.get(uRef);
            if(d.exists) {
                t.delete(uRef);
                t.update(vRef, { votes: firebase.firestore.FieldValue.increment(-1) });
                return "REMOVED";
            } else {
                t.set(uRef, { t: firebase.firestore.FieldValue.serverTimestamp() });
                t.update(vRef, { votes: firebase.firestore.FieldValue.increment(1) });
                return "ADDED";
            }
        }).then((res) => {
            let v = getMyVotes();
            const btn = document.getElementById(`btn-${id}`);
            const span = btn ? btn.querySelector('span') : null;
            let currentCount = span ? parseInt(span.innerText) : 0;
            if(res === "ADDED") {
                if(!v.includes(id)) { v.push(id); localStorage.setItem('myVotedVideos', JSON.stringify(v)); }
                if(btn) { btn.classList.add('liked'); btn.innerHTML = `♥ 완료 <span style="margin-left:3px; font-weight:bold;">${currentCount + 1}</span>`; }
            } else {
                const newV = v.filter(vid => vid !== id);
                localStorage.setItem('myVotedVideos', JSON.stringify(newV));
                if(btn) { btn.classList.remove('liked'); btn.innerHTML = `♡ 투표 <span style="margin-left:3px; font-weight:bold;">${Math.max(0, currentCount - 1)}</span>`; }
            }
        }).catch(e => { console.error(e); });
    }

    function deleteVideo(id) {
        if(prompt("비밀번호:")==="ggfc1234") {
            if(confirm("삭제?")) db.collection(collectionName).doc(id).delete().then(()=>alert("삭제됨"));
        } else { alert("비번 오류"); }
    }
</script>
</body>
</html>
