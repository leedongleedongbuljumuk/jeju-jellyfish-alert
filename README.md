# jeju-jellyfish-alert
 <!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>제주 푸른 해파리 실시간 이동 경로 추적 및 위험 알림 시스템</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Pretendard:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <style>
        body { font-family: 'Pretendard', sans-serif; }
        
        /* Glassmorphism Styles */
        .glass-panel {
            background: rgba(15, 23, 42, 0.85);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border: 1px solid rgba(56, 189, 248, 0.2);
        }

        /* Floating Jellyfish Character Motion */
        @keyframes jellyFloat {
            0% { transform: translateY(0px) rotate(0deg); }
            50% { transform: translateY(-8px) rotate(-3deg); }
            100% { transform: translateY(0px) rotate(0deg); }
        }
        .jelly-float-anim {
            animation: jellyFloat 3s ease-in-out infinite;
        }

        /* Custom Scrollbar */
        ::-webkit-scrollbar { width: 6px; }
        ::-webkit-scrollbar-track { background: #0f172a; }
        ::-webkit-scrollbar-thumb { background: #0284c7; border-radius: 4px; }
    </style>
</head>
<body class="bg-slate-950 text-slate-100 min-h-screen flex flex-col selection:bg-sky-500 selection:text-white">

    <!-- Header Navigation -->
    <header class="bg-slate-900/90 backdrop-blur-md border-b border-sky-500/20 sticky top-0 z-50 px-4 py-2.5">
        <div class="max-w-7xl mx-auto flex flex-col sm:flex-row justify-between items-center gap-3">
            <div class="flex items-center space-x-3 w-full sm:w-auto justify-between sm:justify-start">
                <div class="flex items-center space-x-3">
                    <!-- Cute Animated Character Icon -->
                    <div class="w-10 h-10 rounded-2xl bg-sky-500/20 border border-sky-400/40 flex items-center justify-center p-1 relative overflow-hidden shadow-lg shadow-sky-500/10">
                        <svg class="w-7 h-7 text-sky-400 jelly-float-anim" viewBox="0 0 100 100" fill="none" xmlns="http://www.w3.org/2000/svg">
                            <path d="M50 15C30 15 15 30 15 50C15 55 20 58 25 58C30 58 35 53 40 53C45 53 50 58 55 58C60 58 65 53 70 53C75 53 80 58 85 58C90 58 95 55 95 50C95 30 70 15 50 15Z" fill="currentColor" fill-opacity="0.85"/>
                            <circle cx="35" cy="38" r="4" fill="#0f172a"/>
                            <circle cx="65" cy="38" r="4" fill="#0f172a"/>
                            <path d="M25 58C25 75 30 85 28 92" stroke="currentColor" stroke-width="4.5" stroke-linecap="round"/>
                            <path d="M42 55C42 75 45 82 43 95" stroke="currentColor" stroke-width="4.5" stroke-linecap="round"/>
                            <path d="M58 55C58 75 55 82 57 95" stroke="currentColor" stroke-width="4.5" stroke-linecap="round"/>
                            <path d="M75 58C75 75 70 85 72 92" stroke="currentColor" stroke-width="4.5" stroke-linecap="round"/>
                        </svg>
                    </div>
                    <div>
                        <h1 class="font-extrabold text-base sm:text-lg text-sky-400 flex items-center gap-2">
                            제주 푸른 해파리 모니터링
                            <span class="text-[10px] font-bold px-2 py-0.5 rounded-full bg-sky-500/20 text-sky-300 border border-sky-500/30">실시간 예보</span>
                        </h1>
                        <p class="text-[11px] text-slate-400 hidden sm:block">공공 해양 데이터 & 실시간 위치 제보 연동 추적 시스템</p>
                    </div>
                </div>

                <!-- Admin Toggle Mobile -->
                <div class="sm:hidden flex items-center gap-1.5 bg-slate-800 px-2.5 py-1 rounded-xl border border-slate-700">
                    <span class="text-[10px] text-amber-300 font-bold"><i class="fa-solid fa-user-shield"></i> 관리자</span>
                    <label class="relative inline-flex items-center cursor-pointer">
                        <input type="checkbox" id="adminToggleMobile" class="sr-only peer" onchange="toggleAdminMode(this.checked)">
                        <div class="w-7 h-4 bg-slate-700 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-3 after:w-3 after:transition-all peer-checked:bg-amber-500"></div>
                    </label>
                </div>
            </div>

            <!-- Top Action Buttons -->
            <div class="flex items-center gap-2 w-full sm:w-auto justify-end">
                <div class="hidden sm:flex items-center gap-2 bg-slate-800 px-3 py-1.5 rounded-xl border border-slate-700">
                    <i class="fa-solid fa-user-shield text-amber-400 text-xs"></i>
                    <span class="text-xs font-semibold text-slate-200">안전관리자 모드</span>
                    <label class="relative inline-flex items-center cursor-pointer">
                        <input type="checkbox" id="adminToggleDesktop" class="sr-only peer" onchange="toggleAdminMode(this.checked)">
                        <div class="w-8 h-4.5 bg-slate-700 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-gray-300 after:border after:rounded-full after:h-3.5 after:w-3.5 after:transition-all peer-checked:bg-amber-500"></div>
                    </label>
                </div>

                <button onclick="openReportModal()" class="flex-1 sm:flex-none px-3.5 py-1.5 bg-sky-600 hover:bg-sky-500 text-white rounded-xl text-xs font-bold transition shadow-lg shadow-sky-600/30 flex items-center justify-center gap-1.5">
                    <i class="fa-solid fa-camera"></i> 현장 제보
                </button>
                <button onclick="openSosModal()" class="flex-1 sm:flex-none px-3.5 py-1.5 bg-rose-600 hover:bg-rose-500 text-white rounded-xl text-xs font-bold transition shadow-lg shadow-rose-600/30 animate-pulse flex items-center justify-center gap-1.5">
                    <i class="fa-solid fa-kit-medical"></i> 쏘임 사고 SOS
                </button>
            </div>
        </div>
    </header>

    <!-- Main Dynamic Recommendation Banner -->
    <section id="recommendationBanner" class="bg-gradient-to-r from-rose-900/90 via-rose-800/80 to-amber-900/90 border-b border-rose-500/40 px-4 py-3 shadow-xl transition-all duration-300">
        <div class="max-w-7xl mx-auto flex flex-col sm:flex-row items-center justify-between gap-2 text-center sm:text-left">
            <div class="flex items-center gap-3">
                <div id="bannerIcon" class="w-10 h-10 rounded-xl bg-rose-500/30 border border-rose-400/50 flex items-center justify-center text-rose-200 text-xl flex-shrink-0 animate-bounce">
                    🚨
                </div>
                <div>
                    <div class="text-[11px] font-extrabold uppercase tracking-wider text-rose-300 flex items-center gap-1 justify-center sm:justify-start">
                        <span id="selectedBeachTitle">제주시 조천읍 신흥해수욕장</span> 실시간 입수 안전 권고
                    </div>
                    <div id="bannerMessage" class="text-sm sm:text-base font-black text-white mt-0.5">
                        🚨 오늘은 해파리 밀집 구역입니다! 해수욕장에 가지 않는 것이 좋습니다.
                    </div>
                </div>
            </div>
            <div class="flex items-center gap-2">
                <span class="text-xs font-bold text-rose-200 bg-rose-950/80 px-3 py-1.5 rounded-xl border border-rose-500/40">
                    현재 위험도: <strong id="bannerRiskLabel" class="text-rose-400">위험 (입수 자제)</strong>
                </span>
            </div>
        </div>
    </section>

    <!-- Main Content Area -->
    <main class="flex-1 max-w-7xl w-full mx-auto p-2 sm:p-4 grid grid-cols-1 lg:grid-cols-4 gap-3 sm:gap-4">
        
        <!-- Left Dashboard Controls & Ocean Data -->
        <div class="lg:col-span-1 flex flex-col gap-3 order-2 lg:order-1">
            
            <!-- Beach Selector Card -->
            <div class="glass-panel rounded-2xl p-3.5 space-y-2">
                <label class="block text-xs font-extrabold text-sky-400 flex items-center justify-between">
                    <span><i class="fa-solid fa-map-pin mr-1"></i> 해수욕장 선택</span>
                    <span class="text-[10px] text-slate-400 font-normal">위치 클릭 시 이동</span>
                </label>
                <select id="beachSelect" onchange="selectBeach(this.value)" class="w-full bg-slate-900 border border-slate-700 rounded-xl px-3 py-2 text-xs text-slate-100 font-bold focus:outline-none focus:border-sky-400 transition">
                    <!-- Dynamic Populated -->
                </select>
            </div>

            <!-- Marine Conditions Card -->
            <div class="glass-panel rounded-2xl p-3.5 space-y-2.5">
                <div class="flex justify-between items-center border-b border-slate-800 pb-2">
                    <h3 class="text-xs font-bold text-slate-200 flex items-center gap-1.5">
                        <i class="fa-solid fa-water text-sky-400"></i> 실시간 해양 기상 정보
                    </h3>
                    <span class="text-[10px] text-slate-400">국립수산과학원 연동</span>
                </div>
                
                <div class="grid grid-cols-2 gap-2 text-xs">
                    <div class="bg-slate-800/60 p-2 rounded-xl border border-slate-700/50">
                        <div class="text-[10px] text-slate-400">연안 수온</div>
                        <div id="dataTemp" class="text-sm font-extrabold text-sky-400 mt-0.5">27.2 ℃</div>
                    </div>
                    <div class="bg-slate-800/60 p-2 rounded-xl border border-slate-700/50">
                        <div class="text-[10px] text-slate-400">풍향 / 풍속</div>
                        <div id="dataWind" class="text-xs font-extrabold text-slate-200 mt-0.5">남서풍 4.2m/s</div>
                    </div>
                    <div class="bg-slate-800/60 p-2 rounded-xl border border-slate-700/50">
                        <div class="text-[10px] text-slate-400">표층 유속</div>
                        <div id="dataCurrent" class="text-xs font-extrabold text-amber-400 mt-0.5">북동 0.8m/s</div>
                    </div>
                    <div class="bg-slate-800/60 p-2 rounded-xl border border-slate-700/50">
                        <div class="text-[10px] text-slate-400">주요 독성종</div>
                        <div id="dataJellyType" class="text-xs font-extrabold text-rose-400 mt-0.5 truncate">노무라입깃해파리</div>
                    </div>
                </div>
            </div>

            <!-- Admin Action Box (Hidden by default) -->
            <div id="adminAlertBox" class="hidden bg-amber-950/40 border-2 border-amber-500/60 rounded-2xl p-3.5 space-y-2 text-xs">
                <div class="flex items-center gap-1.5 text-amber-400 font-bold">
                    <i class="fa-solid fa-bullhorn animate-pulse"></i>
                    <span>안전요원 사전 대응 가이드</span>
                </div>
                <p class="text-slate-300 text-[11px] leading-normal">
                    예측 모델 결과 <strong class="text-amber-300">신흥해수욕장 3시간 이내</strong> 해파리 집단 유입 확률 89%입니다.
                </p>
                <div class="pt-1 flex gap-2">
                    <button onclick="alert('[차단망 설치 경보]\n신흥해수욕장 현장 차단망 수거/설치팀에 긴급 지시가 전송되었습니다.')" class="flex-1 py-1.5 bg-amber-500 hover:bg-amber-400 text-slate-950 font-bold rounded-lg text-[11px] transition">
                        차단망 우선 설치
                    </button>
                    <button onclick="alert('[입수 통제 방송]\n해수욕장 안내 방송 시스템으로 입수 금지 경보 방송을 송출합니다.')" class="flex-1 py-1.5 bg-slate-800 hover:bg-slate-700 text-amber-300 font-bold rounded-lg text-[11px] border border-amber-500/40 transition">
                        입수 통제 방송
                    </button>
                </div>
            </div>

            <!-- Beach List Overview -->
            <div class="glass-panel rounded-2xl p-3.5 flex-1 flex flex-col min-h-[220px]">
                <div class="flex justify-between items-center mb-2">
                    <h3 class="text-xs font-bold text-slate-200">제주 주요 해수욕장 위험도</h3>
                    <span class="text-[10px] text-slate-400">5개 거점</span>
                </div>
                <div class="space-y-1.5 overflow-y-auto max-h-[220px] pr-1" id="beachList">
                    <!-- Dynamic -->
                </div>
            </div>

        </div>

        <!-- Right Main Interactive Animated Canvas Map & Timeline -->
        <div class="lg:col-span-3 flex flex-col gap-3 order-1 lg:order-2">
            
            <div class="relative w-full h-[480px] sm:h-[520px] glass-panel rounded-2xl overflow-hidden shadow-2xl flex flex-col">
                
                <!-- Map Top Overlay Legend -->
                <div class="absolute top-3 left-3 z-20 bg-slate-950/90 backdrop-blur-md px-3 py-2 rounded-xl border border-slate-700 text-[11px] flex items-center gap-3 shadow-lg">
                    <span class="font-bold text-slate-300">위험도 범례:</span>
                    <span class="flex items-center gap-1"><span class="w-2.5 h-2.5 rounded-full bg-emerald-500"></span> 안전</span>
                    <span class="flex items-center gap-1"><span class="w-2.5 h-2.5 rounded-full bg-amber-500"></span> 주의</span>
                    <span class="flex items-center gap-1"><span class="w-2.5 h-2.5 rounded-full bg-rose-500 animate-pulse"></span> 위험</span>
                </div>

                <!-- Live Floating Jellyfish Badge -->
                <div class="absolute top-3 right-3 z-20 bg-rose-500/20 border border-rose-500/40 backdrop-blur-md px-3 py-1.5 rounded-xl text-[11px] text-rose-300 font-bold flex items-center gap-2 shadow-lg">
                    <span class="w-2 h-2 rounded-full bg-rose-500 animate-ping"></span>
                    <span id="liveJellyBadge">실시간 감지 독성 해파리 2개 군집 이동 중</span>
                </div>

                <!-- Custom Canvas Map (Zero External Image Dependency) -->
                <div class="relative flex-1 w-full h-full bg-slate-950 cursor-crosshair overflow-hidden">
                    <canvas id="jejuMapCanvas" class="w-full h-full block"></canvas>
                </div>

                <!-- Timeline Slider Control -->
                <div class="bg-slate-900/95 border-t border-slate-800 p-3 sm:p-4 z-20 relative">
                    <div class="flex flex-col sm:flex-row items-center justify-between gap-2 mb-2">
                        <div class="flex items-center gap-2">
                            <button id="playBtn" onclick="toggleTimelinePlay()" class="px-3.5 py-1.5 bg-sky-600 hover:bg-sky-500 text-white rounded-xl text-xs font-bold transition flex items-center gap-1.5 shadow-lg shadow-sky-600/30">
                                <i id="playIcon" class="fa-solid fa-play"></i>
                                <span id="playText">이동 경로 실시간 재생</span>
                            </button>
                            <span class="text-xs font-bold text-slate-300 hidden sm:inline">시간대별 예측 시뮬레이션</span>
                        </div>
                        <div class="text-[11px] font-bold text-sky-400 bg-sky-500/10 px-3 py-1 rounded-full border border-sky-500/30" id="timeDisplay">
                            현재 시각 (0시간 기준)
                        </div>
                    </div>
                    
                    <div class="relative flex items-center px-1 my-1">
                        <input type="range" id="timelineSlider" min="-3" max="6" value="0" step="1" 
                            class="w-full h-2.5 bg-slate-700 rounded-lg appearance-none cursor-pointer accent-sky-400 focus:outline-none"
                            oninput="onTimelineChange(this.value)">
                    </div>

                    <div class="flex justify-between text-[10px] text-slate-400 font-medium px-1">
                        <span>-3h (과거 궤적)</span>
                        <span class="text-sky-300 font-bold">현재 (0h)</span>
                        <span class="text-amber-400 font-semibold">+3h 예측</span>
                        <span class="text-rose-400 font-semibold">+6h 위험 도달</span>
                    </div>
                </div>

            </div>
        </div>

    </main>

    <!-- Modal 1: User Photo Reporting Modal -->
    <div id="reportModal" class="fixed inset-0 bg-slate-950/80 backdrop-blur-sm z-[1000] hidden flex items-center justify-center p-4">
        <div class="bg-slate-900 border border-slate-700 rounded-2xl w-full max-w-md p-5 shadow-2xl space-y-4">
            <div class="flex justify-between items-center border-b border-slate-800 pb-3">
                <h3 class="text-sm sm:text-base font-bold text-slate-100 flex items-center gap-2">
                    <i class="fa-solid fa-camera text-sky-400"></i> 현장 실시간 제보 등록
                </h3>
                <button onclick="closeReportModal()" class="text-slate-400 hover:text-white"><i class="fa-solid fa-xmark text-lg"></i></button>
            </div>

            <form id="reportForm" onsubmit="handleReportSubmit(event)" class="space-y-3 text-xs">
                <div>
                    <label class="block text-slate-300 mb-1 font-semibold">제보 위치 (해수욕장)</label>
                    <select id="reportBeach" class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3 py-2 text-slate-100 focus:outline-none focus:border-sky-400">
                        <option value="신흥해수욕장">제주시 조천읍 신흥해수욕장</option>
                        <option value="함덕해수욕장">제주시 조천읍 함덕해수욕장</option>
                        <option value="협재해수욕장">제주시 한림읍 협재해수욕장</option>
                        <option value="이호테우해수욕장">제주시 이호동 이호테우해수욕장</option>
                        <option value="중문색달해수욕장">서귀포시 중문색달해수욕장</option>
                    </select>
                </div>

                <div>
                    <label class="block text-slate-300 mb-1 font-semibold">GPS 위치 및 교차 인증</label>
                    <button type="button" onclick="verifyGps()" id="gpsBtn" class="w-full py-2 bg-slate-800 hover:bg-slate-700 border border-sky-500/40 text-sky-400 font-semibold rounded-xl flex items-center justify-center gap-2 transition">
                        <i class="fa-solid fa-location-crosshairs"></i> <span id="gpsStatus">현재 위치 GPS 인증 수행하기</span>
                    </button>
                </div>

                <div>
                    <label class="block text-slate-300 mb-1 font-semibold">해파리 종류 선택</label>
                    <select class="w-full bg-slate-800 border border-slate-700 rounded-xl px-3 py-2 text-slate-100 focus:outline-none focus:border-sky-400">
                        <option value="노무라입깃해파리">노무라입깃해파리 (강독성/적갈색)</option>
                        <option value="푸른우산관해파리">푸른우산관해파리 (강독성/푸른빛)</option>
                        <option value="보름달물해파리">보름달물해파리 (약독성/투명)</option>
                        <option value="기타/미상">기타/확인 불가</option>
                    </select>
                </div>

                <div>
                    <label class="block text-slate-300 mb-1 font-semibold">현장 사진 첨부 (필터링 중복 방지)</label>
                    <div class="border border-dashed border-slate-700 rounded-xl p-3 text-center hover:border-sky-400 cursor-pointer bg-slate-800/40">
                        <i class="fa-solid fa-cloud-arrow-up text-xl text-slate-400 mb-1"></i>
                        <div class="text-[11px] text-slate-400">촬영 사진 업로드 (위치 태그 자동 인식)</div>
                    </div>
                </div>

                <div class="pt-1">
                    <button type="submit" class="w-full py-2.5 bg-sky-600 hover:bg-sky-500 text-white font-bold rounded-xl text-xs transition shadow-lg shadow-sky-600/30">
                        공공데이터 상호 검증 후 제보 등록
                    </button>
                </div>
            </form>
        </div>
    </div>

    <!-- Modal 2: First Aid SOS Guide Modal -->
    <div id="sosModal" class="fixed inset-0 bg-slate-950/85 backdrop-blur-md z-[1000] hidden flex items-center justify-center p-4">
        <div class="bg-slate-900 border-2 border-rose-500/60 rounded-2xl w-full max-w-md p-5 shadow-2xl space-y-3.5 max-h-[90vh] overflow-y-auto">
            <div class="flex justify-between items-center border-b border-rose-500/30 pb-2.5">
                <h3 class="text-sm sm:text-base font-bold text-rose-400 flex items-center gap-2">
                    <i class="fa-solid fa-triangle-exclamation text-lg animate-bounce"></i> 쏘임 사고 긴급 행동 요령
                </h3>
                <button onclick="closeSosModal()" class="text-slate-400 hover:text-white"><i class="fa-solid fa-xmark text-lg"></i></button>
            </div>

            <div class="bg-rose-500/10 border border-rose-500/30 rounded-xl p-3 text-xs space-y-1">
                <div class="font-bold text-rose-300"><i class="fa-solid fa-circle-exclamation mr-1"></i> 최우선 응급 수칙!</div>
                <p class="text-slate-200 leading-relaxed">
                    물 밖으로 즉시 이동한 뒤, <strong class="text-amber-300 underline">바닷물(해수) 또는 생리식염수</strong>로 10분 이상 세척하세요.
                </p>
                <p class="text-rose-400 font-bold text-[11px] mt-1">
                    ✕ 수돗물, 생수, 알코올 세척 금지 (독성 낭포 폭발 위험)
                </p>
            </div>

            <div class="space-y-2 text-xs">
                <div class="flex items-start gap-2.5 bg-slate-800/60 p-2 rounded-xl border border-slate-700">
                    <div class="w-5 h-5 rounded-full bg-sky-500/20 text-sky-400 flex items-center justify-center font-bold text-[11px] flex-shrink-0">1</div>
                    <div>
                        <div class="font-bold text-slate-200">바닷물 충분히 세척</div>
                        <div class="text-slate-400 text-[11px]">쏘인 부위의 촉수를 바닷물로 조심히 씻어냅니다.</div>
                    </div>
                </div>

                <div class="flex items-start gap-2.5 bg-slate-800/60 p-2 rounded-xl border border-slate-700">
                    <div class="w-5 h-5 rounded-full bg-sky-500/20 text-sky-400 flex items-center justify-center font-bold text-[11px] flex-shrink-0">2</div>
                    <div>
                        <div class="font-bold text-slate-200">촉수 안전 제거</div>
                        <div class="text-slate-400 text-[11px]">맨손 절대 금지! 카드나 집게로 살살 긁어 떼어냅니다.</div>
                    </div>
                </div>

                <div class="flex items-start gap-2.5 bg-slate-800/60 p-2 rounded-xl border border-slate-700">
                    <div class="w-5 h-5 rounded-full bg-sky-500/20 text-sky-400 flex items-center justify-center font-bold text-[11px] flex-shrink-0">3</div>
                    <div>
                        <div class="font-bold text-slate-200">의무실 방문 치료</div>
                        <div class="text-slate-400 text-[11px]">가까운 해변 119 의무실에서 온찜질 및 약물 처방을 받습니다.</div>
                    </div>
                </div>
            </div>

            <div class="bg-slate-800 p-3 rounded-xl border border-slate-700 space-y-2 text-xs">
                <div class="font-bold text-slate-200 flex justify-between items-center">
                    <span><i class="fa-solid fa-hospital-user text-emerald-400 mr-1"></i> 현재 위치 주변 의무실</span>
                    <span class="text-emerald-400 font-semibold">신흥해변 의무실 120m</span>
                </div>
                <div class="flex gap-2 pt-1">
                    <button onclick="alert('119 수상구조대 종합의무실로 연결합니다.')" class="flex-1 py-2 bg-emerald-600 hover:bg-emerald-500 text-white font-bold rounded-lg text-xs flex items-center justify-center gap-1.5 transition">
                        <i class="fa-solid fa-phone"></i> 119 긴급 연결
                    </button>
                    <button onclick="closeSosModal()" class="px-3 py-2 bg-slate-700 hover:bg-slate-600 text-slate-200 font-semibold rounded-lg text-xs">
                        닫기
                    </button>
                </div>
            </div>
        </div>
    </div>

    <!-- Application JavaScript Code -->
    <script>
        // 제주 주요 해수욕장 기상 및 위험 데이터
        const beaches = [
            { 
                id: 'shineung', 
                name: '신흥해수욕장', 
                location: '제주시 조천읍',
                mapX: 0.65, mapY: 0.28, // Relative Canvas Coordinates (0~1)
                risk: '위험', 
                bannerMsg: '🚨 오늘은 해파리 밀집 구역입니다! 해수욕장에 가지 않는 것이 좋습니다.',
                jellyType: '노무라입깃해파리', 
                reports: 12, 
                temp: '27.5 ℃', wind: '남서풍 4.5m/s', current: '북동 0.9m/s'
            },
            { 
                id: 'hamdeok', 
                name: '함덕해수욕장', 
                location: '제주시 조천읍',
                mapX: 0.72, mapY: 0.30, 
                risk: '주의', 
                bannerMsg: '⚠️ 해파리 출몰 주의! 입수 시 래시가드를 착용하고 주의하세요.',
                jellyType: '노무라입깃해파리', 
                reports: 6, 
                temp: '27.2 ℃', wind: '남서풍 4.2m/s', current: '북동 0.8m/s'
            },
            { 
                id: 'hyeopjae', 
                name: '협재해수욕장', 
                location: '제주시 한림읍',
                mapX: 0.22, mapY: 0.52, 
                risk: '주의', 
                bannerMsg: '⚠️ 독성 푸른우산관해파리 출몰 주의! 물놀이 시 주변을 확인하세요.',
                jellyType: '푸른우산관해파리', 
                reports: 4, 
                temp: '26.8 ℃', wind: '남풍 3.8m/s', current: '북서 0.5m/s'
            },
            { 
                id: 'ihoteow', 
                name: '이호테우해수욕장', 
                location: '제주시 이호동',
                mapX: 0.48, mapY: 0.32, 
                risk: '안전', 
                bannerMsg: '✅ 현재 해파리 위험도가 낮아 안전한 물놀이가 가능합니다.',
                jellyType: '관해파리 (소수)', 
                reports: 1, 
                temp: '26.5 ℃', wind: '남서풍 3.2m/s', current: '북동 0.4m/s'
            },
            { 
                id: 'jungmun', 
                name: '중문색달해수욕장', 
                location: '서귀포시 중문',
                mapX: 0.42, mapY: 0.82, 
                risk: '안전', 
                bannerMsg: '✅ 현재 해파리 위험도가 낮아 안전한 물놀이가 가능합니다.',
                jellyType: '없음 (안전)', 
                reports: 0, 
                temp: '25.8 ℃', wind: '남동풍 2.8m/s', current: '동 0.3m/s'
            }
        ];

        // 해파리 유영 무리 데이터
        const jellyfishClusters = [
            { id: 1, startX: 0.58, startY: 0.25, name: '노무라입깃해파리 군집 A' },
            { id: 2, startX: 0.18, startY: 0.45, name: '푸른우산관해파리 군집 B' }
        ];

        let selectedBeach = beaches[0]; // Default: 신흥해수욕장
        let timeOffset = 0; // -3 ~ +6
        let isPlaying = false;
        let playInterval = null;

        // Canvas Mapping & Animation Engine
        let canvas, ctx;
        let animFrameId;
        let animTime = 0;

        function initCanvas() {
            canvas = document.getElementById('jejuMapCanvas');
            ctx = canvas.getContext('2d');
            resizeCanvas();
            window.addEventListener('resize', resizeCanvas);
            requestAnimationFrame(renderLoop);
        }

        function resizeCanvas() {
            if (!canvas) return;
            const rect = canvas.parentElement.getBoundingClientRect();
            canvas.width = rect.width * window.devicePixelRatio;
            canvas.height = rect.height * window.devicePixelRatio;
            ctx.scale(window.devicePixelRatio, window.devicePixelRatio);
        }

        // Main Animation Render Loop
        function renderLoop() {
            animTime += 0.03;
            drawMap();
            animFrameId = requestAnimationFrame(renderLoop);
        }

        // Draw Complete Jeju Vector Map & Jellyfish Characters
        function drawMap() {
            if (!canvas || !ctx) return;
            const w = canvas.width / window.devicePixelRatio;
            const h = canvas.height / window.devicePixelRatio;

            // Clear Background (Deep Ocean Blue)
            ctx.fillStyle = '#030712';
            ctx.fillRect(0, 0, w, h);

            // Draw Animated Coastal Ocean Waves
            ctx.lineWidth = 1;
            for (let i = 0; i < 6; i++) {
                ctx.strokeStyle = `rgba(56, 189, 248, ${0.05 + i * 0.02})`;
                ctx.beginPath();
                for (let x = 0; x < w; x += 20) {
                    const y = h * 0.2 + Math.sin(x * 0.01 + animTime + i) * (10 + i * 3) + i * 50;
                    if (x === 0) ctx.moveTo(x, y);
                    else ctx.lineTo(x, y);
                }
                ctx.stroke();
            }

            // Draw Jeju Island Base Shape (Custom Vector Polygon)
            const pts = [
                {x: 0.15, y: 0.50}, {x: 0.25, y: 0.38}, {x: 0.40, y: 0.30}, 
                {x: 0.60, y: 0.26}, {x: 0.78, y: 0.32}, {x: 0.88, y: 0.45}, 
                {x: 0.82, y: 0.62}, {x: 0.65, y: 0.75}, {x: 0.45, y: 0.85}, 
                {x: 0.28, y: 0.78}, {x: 0.18, y: 0.65}
            ];

            ctx.beginPath();
            pts.forEach((p, idx) => {
                const px = p.x * w;
                const py = p.y * h;
                if (idx === 0) ctx.moveTo(px, py);
                else {
                    const prev = pts[idx - 1];
                    const cx = (prev.x + p.x) / 2 * w;
                    const cy = (prev.y + p.y) / 2 * h;
                    ctx.quadraticCurveTo(prev.x * w, prev.y * h, cx, cy);
                }
            });
            ctx.closePath();

            // Island Gradient & Shadow
            const islandGrad = ctx.createRadialGradient(w*0.5, h*0.5, 20, w*0.5, h*0.5, w*0.4);
            islandGrad.addColorStop(0, '#1e293b');
            islandGrad.addColorStop(0.7, '#0f172a');
            islandGrad.addColorStop(1, '#090d16');

            ctx.fillStyle = islandGrad;
            ctx.shadowColor = 'rgba(56, 189, 248, 0.25)';
            ctx.shadowBlur = 20;
            ctx.fill();
            ctx.shadowBlur = 0;

            ctx.strokeStyle = '#38bdf8';
            ctx.lineWidth = 2;
            ctx.stroke();

            // Draw Mt. Halla Landmark
            ctx.fillStyle = 'rgba(255, 255, 255, 0.06)';
            ctx.beginPath();
            ctx.arc(w * 0.5, h * 0.53, w * 0.12, 0, Math.PI * 2);
            ctx.fill();
            ctx.fillStyle = '#475569';
            ctx.font = 'bold 12px Pretendard';
            ctx.textAlign = 'center';
            ctx.fillText('한라산 (1,947m)', w * 0.5, h * 0.54);

            // Draw Beaches & Risk Heatmap Circles
            beaches.forEach(b => {
                const bx = b.mapX * w;
                const by = b.mapY * h;
                const isSelected = (b.id === selectedBeach.id);

                // Risk Color
                let color = '#10b981'; // Safe
                if (b.risk === '위험') color = '#ef4444';
                else if (b.risk === '주의') color = '#f59e0b';

                // Heatmap Radius Animation
                const pulse = Math.sin(animTime * 3) * 4;
                const radius = (b.risk === '위험' ? 38 : (b.risk === '주의' ? 28 : 18)) + pulse;

                ctx.beginPath();
                ctx.arc(bx, by, radius, 0, Math.PI * 2);
                ctx.fillStyle = color + '33';
                ctx.fill();
                ctx.strokeStyle = color + '88';
                ctx.lineWidth = 1.5;
                ctx.stroke();

                // Selected Beach Ring Indicator
                if (isSelected) {
                    ctx.beginPath();
                    ctx.arc(bx, by, radius + 10, 0, Math.PI * 2);
                    ctx.strokeStyle = '#38bdf8';
                    ctx.lineWidth = 2.5;
                    ctx.setLineDash([4, 4]);
                    ctx.stroke();
                    ctx.setLineDash([]);
                }

                // Beach Marker Pin
                ctx.beginPath();
                ctx.arc(bx, by, 7, 0, Math.PI * 2);
                ctx.fillStyle = color;
                ctx.fill();
                ctx.strokeStyle = '#0f172a';
                ctx.lineWidth = 2;
                ctx.stroke();

                // Beach Title Label
                ctx.fillStyle = isSelected ? '#38bdf8' : '#e2e8f0';
                ctx.font = isSelected ? 'bold 13px Pretendard' : '11px Pretendard';
                ctx.fillText(b.name, bx, by - (radius + 6));
            });

            // Draw Floating Jellyfish Characters & Prediction Vectors
            jellyfishClusters.forEach(cluster => {
                // Wind & Current offset calculation based on timeline slider
                const dx = (timeOffset * 0.015);
                const dy = -(timeOffset * 0.008);
                const jx = (cluster.startX + dx) * w;
                const jy = (cluster.startY + dy) * h + Math.sin(animTime * 2 + cluster.id) * 4;

                // Draw Vector Trajectory Arrow (Dashed Line)
                const startX = cluster.startX * w;
                const startY = cluster.startY * h;
                const endX = (cluster.startX + 0.09) * w;
                const endY = (cluster.startY - 0.048) * h;

                ctx.beginPath();
                ctx.moveTo(startX, startY);
                ctx.lineTo(endX, endY);
                ctx.strokeStyle = '#f43f5e';
                ctx.lineWidth = 2;
                ctx.setLineDash([5, 5]);
                ctx.stroke();
                ctx.setLineDash([]);

                // Draw Anime-style Floating Jellyfish Character
                drawJellyfishCharacter(ctx, jx, jy, '#f43f5e', animTime);
            });
        }

        // Draw Cute Anime Jellyfish Character
        function drawJellyfishCharacter(ctx, x, y, color, t) {
            ctx.save();
            ctx.translate(x, y);

            // Floating bounce rotation
            const rot = Math.sin(t * 2) * 0.1;
            ctx.rotate(rot);

            // Outer Glow
            ctx.shadowColor = color;
            ctx.shadowBlur = 12;

            // Jellyfish Dome Cap
            ctx.beginPath();
            ctx.arc(0, 0, 16, Math.PI, 0, false);
            ctx.quadraticCurveTo(12, 6, 0, 4);
            ctx.quadraticCurveTo(-12, 6, -16, 0);
            ctx.fillStyle = color;
            ctx.fill();

            // Cute Eyes
            ctx.shadowBlur = 0;
            ctx.fillStyle = '#0f172a';
            ctx.beginPath();
            ctx.arc(-5, -4, 2, 0, Math.PI * 2);
            ctx.arc(5, -4, 2, 0, Math.PI * 2);
            ctx.fill();

            // Wavy Tentacles
            for (let i = -10; i <= 10; i += 5) {
                ctx.beginPath();
                ctx.moveTo(i, 4);
                const wave = Math.sin(t * 4 + i) * 3;
                ctx.quadraticCurveTo(i + wave, 14, i - wave, 22);
                ctx.strokeStyle = color;
                ctx.lineWidth = 2.5;
                ctx.stroke();
            }

            ctx.restore();
        }

        // Beach Selection Handler
        function selectBeach(beachId) {
            const found = beaches.find(b => b.id === beachId || b.name === beachId);
            if (!found) return;
            selectedBeach = found;

            document.getElementById('beachSelect').value = found.id;
            document.getElementById('selectedBeachTitle').innerText = `${found.location} ${found.name}`;
            document.getElementById('dataTemp').innerText = found.temp;
            document.getElementById('dataWind').innerText = found.wind;
            document.getElementById('dataCurrent').innerText = found.current;
            document.getElementById('dataJellyType').innerText = found.jellyType;

            // Update Top Recommendation Banner
            const banner = document.getElementById('recommendationBanner');
            const bannerMsg = document.getElementById('bannerMessage');
            const bannerRiskLabel = document.getElementById('bannerRiskLabel');
            const bannerIcon = document.getElementById('bannerIcon');

            bannerMsg.innerText = found.bannerMsg;

            if (found.risk === '위험') {
                banner.className = "bg-gradient-to-r from-rose-950 via-rose-900 to-amber-950 border-b border-rose-500/40 px-4 py-3 shadow-xl transition-all duration-300";
                bannerRiskLabel.innerText = "위험 (입수 자제)";
                bannerRiskLabel.className = "text-rose-400 font-extrabold";
                bannerIcon.innerText = "🚨";
            } else if (found.risk === '주의') {
                banner.className = "bg-gradient-to-r from-amber-950 via-amber-900 to-slate-900 border-b border-amber-500/40 px-4 py-3 shadow-xl transition-all duration-300";
                bannerRiskLabel.innerText = "주의 (래시가드 착용)";
                bannerRiskLabel.className = "text-amber-400 font-extrabold";
                bannerIcon.innerText = "⚠️";
            } else {
                banner.className = "bg-gradient-to-r from-emerald-950 via-slate-900 to-slate-950 border-b border-emerald-500/40 px-4 py-3 shadow-xl transition-all duration-300";
                bannerRiskLabel.innerText = "안전 (입수 가능)";
                bannerRiskLabel.className = "text-emerald-400 font-extrabold";
                bannerIcon.innerText = "✅";
            }

            renderBeachList();
        }

        // Timeline Slider Handler
        function onTimelineChange(val) {
            timeOffset = parseInt(val);
            const display = document.getElementById('timeDisplay');
            
            if (timeOffset === 0) {
                display.innerText = "현재 시각 (0시간 기준)";
                display.className = "text-[11px] font-bold text-sky-400 bg-sky-500/10 px-3 py-1 rounded-full border border-sky-500/30";
            } else if (timeOffset < 0) {
                display.innerText = `${Math.abs(timeOffset)}시간 전 과거 이동 궤적`;
                display.className = "text-[11px] font-bold text-slate-300 bg-slate-800 px-3 py-1 rounded-full border border-slate-700";
            } else {
                display.innerText = `+${timeOffset}시간 뒤 예측 이동 경로`;
                display.className = "text-[11px] font-bold text-rose-400 bg-rose-500/10 px-3 py-1 rounded-full border border-rose-500/30";
            }
        }

        // Auto Play Timeline Simulation
        function toggleTimelinePlay() {
            const playIcon = document.getElementById('playIcon');
            const playText = document.getElementById('playText');
            const slider = document.getElementById('timelineSlider');

            if (isPlaying) {
                clearInterval(playInterval);
                isPlaying = false;
                playIcon.className = "fa-solid fa-play";
                playText.innerText = "이동 경로 실시간 재생";
            } else {
                isPlaying = true;
                playIcon.className = "fa-solid fa-pause";
                playText.innerText = "일시정지";

                playInterval = setInterval(() => {
                    let cur = parseInt(slider.value);
                    if (cur >= 6) cur = -3;
                    else cur++;
                    slider.value = cur;
                    onTimelineChange(cur);
                }, 1200);
            }
        }

        // Render Left Beach Cards List
        function renderBeachList() {
            const container = document.getElementById('beachList');
            container.innerHTML = '';

            beaches.forEach(b => {
                let color = '#10b981';
                if (b.risk === '위험') color = '#ef4444';
                else if (b.risk === '주의') color = '#f59e0b';

                const isSel = (b.id === selectedBeach.id);

                const card = document.createElement('div');
                card.className = `p-2.5 rounded-xl border flex items-center justify-between cursor-pointer transition ${isSel ? 'bg-sky-500/20 border-sky-400' : 'bg-slate-800/40 border-slate-700/60 hover:bg-slate-800'}`;
                card.onclick = () => selectBeach(b.id);

                card.innerHTML = `
                    <div class="flex items-center gap-2.5">
                        <span class="w-3 h-3 rounded-full flex-shrink-0" style="background-color: ${color}; shadow: 0 0 8px ${color};"></span>
                        <div>
                            <div class="text-xs font-extrabold text-slate-100">${b.name}</div>
                            <div class="text-[10px] text-slate-400">${b.jellyType}</div>
                        </div>
                    </div>
                    <div class="text-right">
                        <span class="text-[10px] px-2 py-0.5 rounded-md font-bold" style="background-color:${color}22; color:${color}; border: 1px solid ${color}44;">${b.risk}</span>
                        <div class="text-[10px] text-slate-400 mt-1">제보 ${b.reports}건</div>
                    </div>
                `;
                container.appendChild(card);
            });
        }

        // Initialize Beach Select Options
        function initSelectOptions() {
            const select = document.getElementById('beachSelect');
            select.innerHTML = '';
            beaches.forEach(b => {
                const opt = document.createElement('option');
                opt.value = b.id;
                opt.innerText = `${b.name} (${b.risk})`;
                select.appendChild(opt);
            });
        }

        // Admin Toggle
        function toggleAdminMode(checked) {
            document.getElementById('adminToggleDesktop').checked = checked;
            document.getElementById('adminToggleMobile').checked = checked;
            const alertBox = document.getElementById('adminAlertBox');
            if (checked) alertBox.classList.remove('hidden');
            else alertBox.classList.add('hidden');
        }

        // Modals
        function openReportModal() { document.getElementById('reportModal').classList.remove('hidden'); }
        function closeReportModal() { document.getElementById('reportModal').classList.add('hidden'); }
        function openSosModal() { document.getElementById('sosModal').classList.remove('hidden'); }
        function closeSosModal() { document.getElementById('sosModal').classList.add('hidden'); }

        function verifyGps() {
            const btn = document.getElementById('gpsBtn');
            const status = document.getElementById('gpsStatus');
            btn.className = "w-full py-2 bg-emerald-500/20 border border-emerald-500/50 text-emerald-400 font-semibold rounded-xl flex items-center justify-center gap-2";
            status.innerText = "GPS 위치 인증 완료 (신흥해수욕장 부근)";
        }

        function handleReportSubmit(e) {
            e.preventDefault();
            const bName = document.getElementById('reportBeach').value;
            alert(`[제보 등록 완료]\n\nGPS 인증 및 공공 해양 데이터 교차 검증을 거쳐 ${bName} 실시간 해파리 제보가 성공적으로 등록되었습니다.`);
            
            const target = beaches.find(b => b.name === bName || b.id === bName);
            if (target) {
                target.reports += 1;
                renderBeachList();
            }
            closeReportModal();
        }

        window.onload = function() {
            initSelectOptions();
            initCanvas();
            selectBeach('shineung');
        };
    </script>
</body>
</html>
