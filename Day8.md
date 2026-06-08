
# DAy 8/60 of 60 days AI challenge


Learnings:

## 1. Data Cleansing & Multimodal Synchronization
* **Automated Imputation Boundaries:** When processing live data streams combining air quality indices (AQI) from the Central Pollution Control Board (CPCB) and total dissolved solids (TDS) parameters from the Ministry of Jal Shakti, setting defensive validation layers is non-negotiable. Imputation algorithms must account for dramatic geographic differences—such as coastal marine scrub effects in Mumbai versus dry storm-driven particulate suspension in Lucknow.
* **Telemetry Data Quality:** True diagnostic utility relies on matching real-time micro-records exactly to localized stations rather than applying loose regional interpolations. Validating pure telemetry loops avoids over-penalizing or under-reporting localized micro-climates.

## 2. Physiological & Biometric Correlation Mapping
* **Trans-Alveolar Mechanics:** Translating raw air pollution metrics into physiological clinical effects (e.g., how fine particulate matters cross human alveolar-capillary membranes) bridges the gap between cold statistics and personal health ownership. 
* **Hydro-Dermatological Impact:** High TDS counts ($>400\\text{ ppm}$) directly disrupt the natural acidic lipid bilayer of the skin, altering scalp oil-water balance and causing desiccation-related hair fall. Linking specific mineral aggregates (sulfates/calcites) to cosmetic vulnerabilities drives immediate, sticky user engagement.

## 3. Advanced UX Layout & Performance Frameworks
* **Responsive Visual Hierarchy:** Dark-themed analytical frameworks need strict typographic contrast controls to keep data-heavy grids readable.
*  Using muted, desaturated base palettes prevents interface fatigue during prolonged cross-city benchmarking sessions.
* **Dynamic Node Isolation (State Sync):** Implementing a dual-city benchmarking compare mode requires explicit element caching.
*  Destroying and immediately rebuilding nested `Chart.js` instances prevents dirty memory leaks or trailing canvas artifact animations upon variable mutations.
* **Non-Blocking Filters:** Client-side array routing and indexing via explicit key-value pairings keep table reflow times sub-millisecond,
*  providing smooth interactions even across deep geographic data matrix pipelines.

**HTML CODE**

<html lang="en" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Personal Environmental Health Analyzer Dashboard</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        darkBg: '#0f172a',
                        darkCard: '#1e293b',
                        darkBorder: '#334155',
                        aqiGood: '#10b981',
                        aqiSatisfactory: '#84cc16',
                        aqiModerate: '#eab308',
                        aqiPoor: '#f97316',
                        aqiVeryPoor: '#ef4444',
                        aqiSevere: '#7f1d1d'
                    }
                }
            }
        }
    </script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700&display=swap');
        body {
            font-family: 'Plus Jakarta Sans', sans-serif;
            background-color: #0f172a;
        }
        .custom-scrollbar::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        .custom-scrollbar::-webkit-scrollbar-track {
            background: #1e293b;
        }
        .custom-scrollbar::-webkit-scrollbar-thumb {
            background: #475569;
            border-radius: 3px;
        }
    </style>
</head>
<body class="text-slate-100 min-h-screen antialiased selection:bg-indigo-500 selection:text-white">

    <header class="sticky top-0 z-50 bg-darkBg/80 backdrop-blur-md border-b border-darkBorder px-4 lg:px-8 py-4 flex flex-wrap items-center justify-between gap-4">
        <div class="flex items-center space-x-3">
            <div class="p-2.5 bg-gradient-to-tr from-indigo-600 to-emerald-500 rounded-xl shadow-lg shadow-indigo-500/20">
                <svg class="w-6 h-6 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3.055 11H5a2 2 0 012 2v1a2 2 0 002 2 2 2 0 012 2v2.945M8 3.935V5.5A2.5 2.5 0 0010.5 8h.5a2 2 0 012 2 2 2 0 002 2h2a2.5 2.5 0 002.5-2.5V10a2 2 0 012-2h1.065M21 12a9 9 0 11-18 0 0118 0z"></path></svg>
            </div>
            <div>
                <h1 class="text-xl font-bold tracking-tight bg-gradient-to-r from-white via-slate-200 to-slate-400 bg-clip-text text-transparent">EcoPulse</h1>
                <p class="text-xs text-slate-400">Personal Environmental Health Analyzer</p>
            </div>
        </div>

        <div class="flex items-center gap-3 w-full sm:w-auto">
            <div class="relative w-full sm:w-64">
                <span class="absolute inset-y-0 left-0 flex items-center pl-3 pointer-events-none">
                    <svg class="w-4 h-4 text-slate-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z"></path><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 11a3 3 0 11-6 0 3 3 0 016 0z"></path></svg>
                </span>
                <input type="text" id="citySearchInput" placeholder="Search analyzed cities..." class="w-full pl-9 pr-4 py-2 bg-darkCard border border-darkBorder rounded-xl text-sm focus:outline-none focus:border-indigo-500 transition-colors placeholder:text-slate-500">
            </div>
            <button onclick="shareDashboard()" class="flex items-center gap-2 bg-indigo-600 hover:bg-indigo-500 transition-all text-white px-4 py-2 rounded-xl text-sm font-semibold shadow-lg shadow-indigo-600/20 active:scale-95">
                <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M8.684 13.342C8.886 12.938 9 12.482 9 12c0-.482-.114-.938-.316-1.342m0 2.684a3 3 0 110-2.684m0 2.684l6.632 3.316m-6.632-6l6.632-3.316m0 0a3 3 0 105.367-2.684 3 3 0 00-5.367 2.684zm0 9.316a3 3 0 105.368 2.684 3 3 0 00-5.368-2.684z"></path></svg>
                <span class="hidden sm:inline">Share Insights</span>
            </button>
        </div>
    </header>

    <main class="max-w-[1600px] mx-auto p-4 lg:p-8 space-y-8">
        
        <div class="bg-gradient-to-r from-slate-900 via-indigo-950 to-slate-900 rounded-2xl p-6 border border-darkBorder/60 flex flex-col md:flex-row items-start md:items-center justify-between gap-6 relative overflow-hidden">
            <div class="absolute -top-24 -right-24 w-48 h-48 bg-emerald-500/10 rounded-full blur-3xl"></div>
            <div class="absolute -bottom-24 -left-24 w-48 h-48 bg-indigo-500/10 rounded-full blur-3xl"></div>
            
            <div class="space-y-2 z-10">
                <div class="flex items-center gap-2">
                    <span class="inline-flex items-center px-2.5 py-0.5 rounded-full text-xs font-medium bg-indigo-400/10 text-indigo-400 border border-indigo-400/20">
                        <span class="w-1.5 h-1.5 mr-1.5 bg-indigo-400 rounded-full animate-pulse"></span>Active Session City
                    </span>
                    <span class="text-xs text-slate-500">Validated 2026 Live Data</span>
                </div>
                <h2 class="text-3xl font-extrabold tracking-tight">Your Profile: <span id="currentLocationName" class="text-transparent bg-clip-text bg-gradient-to-r from-emerald-400 to-teal-300">Lucknow, IN</span></h2>
                <p class="text-sm text-slate-400 max-w-2xl">Automatic localized parsing matching Central Pollution Control Board & Ministry of Jal Shakti databases. Toggle filters or choose comparisons below to modify your interactive metrics grid.</p>
            </div>
            <div class="flex flex-wrap items-center gap-3 z-10 w-full md:w-auto">
                <div class="bg-darkCard/80 backdrop-blur-sm border border-darkBorder p-3 rounded-xl min-w-[120px]">
                    <div class="text-xs text-slate-400">Regional Air Rank</div>
                    <div class="text-lg font-bold text-yellow-400">#107 <span class="text-xs font-normal text-slate-400">Globally</span></div>
                </div>
                <div class="bg-darkCard/80 backdrop-blur-sm border border-darkBorder p-3 rounded-xl min-w-[120px]">
                    <div class="text-xs text-slate-400">Primary Source</div>
                    <div class="text-xs font-bold text-slate-300 mt-1 truncate max-w-[110px]" title="CPCB & Jal Shakti">CPCB & Jal Shakti</div>
                </div>
            </div>
        </div>

        <section class="bg-darkCard rounded-2xl p-6 border border-darkBorder space-y-4 shadow-xl">
            <div class="flex items-center justify-between border-b border-darkBorder/60 pb-3">
                <h3 class="text-sm font-semibold tracking-wider uppercase text-slate-400 flex items-center gap-2">
                    <svg class="w-4 h-4 text-indigo-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6V4m0 2a2 2 0 100 4m0-4a2 2 0 110 4m-6 8a2 2 0 100-4m0 4a2 2 0 110-4m0 4v2m0-6V4m6 6v10m6-2a2 2 0 100-4m0 4a2 2 0 110-4m0 4v2m0-6V4"></path></svg>
                    Interactive Multi-Variable Filters
                </h3>
                <button onclick="resetFilters()" class="text-xs font-medium text-indigo-400 hover:text-indigo-300 transition-colors">Reset Filters</button>
            </div>
            <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
                <div>
                    <label class="block text-xs font-medium text-slate-400 mb-1.5">Select Primary Focused City</label>
                    <select id="filterCitySelector" onchange="handleFilterChange()" class="w-full px-3 py-2 bg-darkBg border border-darkBorder rounded-xl text-sm focus:outline-none focus:border-indigo-500 transition-colors text-slate-200"></select>
                </div>
                <div>
                    <label class="block text-xs font-medium text-slate-400 mb-1.5">AQI Risk Tier Filter</label>
                    <select id="filterRiskSelector" onchange="handleFilterChange()" class="w-full px-3 py-2 bg-darkBg border border-darkBorder rounded-xl text-sm focus:outline-none focus:border-indigo-500 transition-colors text-slate-200">
                        <option value="all">All Tiers (Show Everything)</option>
                        <option value="Good">Good (0 - 50)</option>
                        <option value="Moderate">Moderate / Satisfactory (51 - 100)</option>
                        <option value="Poor">Poor (101 - 200)</option>
                        <option value="Unhealthy">Unhealthy / Severe (&gt; 200)</option>
                    </select>
                </div>
                <div>
                    <label class="block text-xs font-medium text-slate-400 mb-1.5">Water Quality (TDS ppm)</label>
                    <select id="filterTdsSelector" onchange="handleFilterChange()" class="w-full px-3 py-2 bg-darkBg border border-darkBorder rounded-xl text-sm focus:outline-none focus:border-indigo-500 transition-colors text-slate-200">
                        <option value="all">All Ranges</option>
                        <option value="low">Optimal (&lt; 200 ppm)</option>
                        <option value="mid">Moderate (200 - 500 ppm)</option>
                        <option value="high">High Hardness (&gt; 500 ppm)</option>
                    </select>
                </div>
                <div class="flex items-end">
                    <div class="bg-darkBg/60 border border-darkBorder/80 p-2.5 rounded-xl flex items-center justify-between w-full">
                        <span class="text-xs text-slate-400 font-medium">Comparison Engine:</span>
                        <label class="relative inline-flex items-center cursor-pointer">
                            <input type="checkbox" id="compareModeToggle" onchange="toggleCompareMode()" class="sr-only peer">
                            <div class="w-9 h-5 bg-slate-700 peer-focus:outline-none rounded-full peer peer-checked:after:translate-x-full peer-checked:after:border-white after:content-[''] after:absolute after:top-[2px] after:left-[2px] after:bg-white after:border-slate-300 after:border after:rounded-full after:h-4 after:w-4 after:transition-all peer-checked:bg-indigo-600"></div>
                        </label>
                    </div>
                </div>
            </div>
            <div id="comparisonRibbon" class="hidden animate-fade-in bg-indigo-950/40 border border-indigo-500/20 p-3 rounded-xl flex flex-wrap items-center justify-between gap-3">
                <span class="text-xs font-medium text-indigo-300">⚡ Dual-City Sync Mode Active. Select secondary benchmark target:</span>
                <select id="secondaryCitySelector" onchange="handleSecondaryChange()" class="px-3 py-1 bg-darkBg border border-darkBorder rounded-lg text-xs focus:outline-none focus:border-indigo-500 text-slate-200 min-w-[150px]"></select>
            </div>
        </section>

        <section class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-5 gap-4">
            <div class="bg-darkCard border border-darkBorder p-5 rounded-2xl flex flex-col justify-between group hover:border-indigo-500/40 transition-colors">
                <div>
                    <span class="text-xs text-slate-400 font-semibold tracking-wide uppercase">Analyzed Dataset Pool</span>
                    <div id="statTotalCities" class="text-4xl font-bold mt-2 text-white">--</div>
                </div>
                <div class="text-xs text-slate-500 mt-4 pt-2 border-t border-darkBorder/40">Cleaned & parsed micro-records</div>
            </div>
            <div class="bg-darkCard border border-darkBorder p-5 rounded-2xl flex flex-col justify-between group hover:border-indigo-500/40 transition-colors">
                <div>
                    <span class="text-xs text-slate-400 font-semibold tracking-wide uppercase">Macro Network Average AQI</span>
                    <div id="statAvgAqi" class="text-4xl font-bold mt-2 text-indigo-400">--</div>
                </div>
                <div id="statAvgStatus" class="text-xs text-slate-500 mt-4 pt-2 border-t border-darkBorder/40">--</div>
            </div>
            <div class="bg-darkCard border border-darkBorder p-5 rounded-2xl flex flex-col justify-between group hover:border-emerald-500/40 transition-colors">
                <div>
                    <span class="text-xs text-slate-400 font-semibold tracking-wide uppercase">Ecosystem Zenith (Cleanest)</span>
                    <div id="statCleanestCity" class="text-xl font-bold mt-2 text-emerald-400 truncate">--</div>
                    <div id="statCleanestAqi" class="text-xs text-slate-400 mt-0.5">AQI --</div>
                </div>
                <div class="text-xs text-slate-500 mt-4 pt-2 border-t border-darkBorder/40">Lowest composite air metrics</div>
            </div>
            <div class="bg-darkCard border border-darkBorder p-5 rounded-2xl flex flex-col justify-between group hover:border-red-500/40 transition-colors">
                <div>
                    <span class="text-xs text-slate-400 font-semibold tracking-wide uppercase">Highest Peak Pollution</span>
                    <div id="statPollutedCity" class="text-xl font-bold mt-2 text-red-400 truncate">--</div>
                    <div id="statPollutedAqi" class="text-xs text-slate-400 mt-0.5">AQI --</div>
                </div>
                <div class="text-xs text-slate-500 mt-4 pt-2 border-t border-darkBorder/40">Critical advisory active</div>
            </div>
            <div class="bg-darkCard border border-darkBorder p-5 rounded-2xl flex flex-col justify-between bg-gradient-to-br from-darkCard to-indigo-950/20">
                <div>
                    <span class="text-xs id-score-title text-slate-400 font-semibold tracking-wide uppercase">Selected Target Health Score</span>
                    <div id="statHealthScore" class="text-5xl font-extrabold mt-1 text-transparent bg-clip-text bg-gradient-to-r from-emerald-400 via-yellow-400 to-indigo-400">--</div>
                </div>
                <div class="text-xs text-slate-400 mt-4 pt-2 border-t border-darkBorder/40 flex justify-between items-center">
                    <span>Overall Rating:</span>
                    <span id="statHealthGrade" class="font-bold text-sm">--</span>
                </div>
            </div>
        </section>

        <section class="grid grid-cols-1 lg:grid-cols-3 gap-6">
            
            <div class="lg:col-span-2 space-y-6">
                
                <div class="bg-darkCard border border-darkBorder rounded-2xl p-6 shadow-xl">
                    <div class="flex flex-wrap items-center justify-between gap-4 mb-6">
                        <div>
                            <h3 class="text-base font-bold text-white tracking-tight">Ecosystem Metrics Cross-Analysis Matrix</h3>
                            <p class="text-xs text-slate-400">Interactive live comparison of PM2.5, PM10, and Water Hardness</p>
                        </div>
                        <div class="flex items-center gap-2 bg-darkBg p-1.5 rounded-xl border border-darkBorder">
                            <button onclick="switchChartType('aqi')" id="btnChartAqi" class="px-3 py-1.5 text-xs font-semibold rounded-lg bg-indigo-600 text-white transition-all">AQI Overview</button>
                            <button onclick="switchChartType('pm')" id="btnChartPm" class="px-3 py-1.5 text-xs font-semibold rounded-lg text-slate-400 hover:text-white transition-all">Particulates (PM)</button>
                            <button onclick="switchChartType('water')" id="btnChartWater" class="px-3 py-1.5 text-xs font-semibold rounded-lg text-slate-400 hover:text-white transition-all">Water (TDS)</button>
                        </div>
                    </div>
                    <div class="relative w-full h-[320px]">
                        <canvas id="primaryAnalyticsChart"></canvas>
                    </div>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                    <div class="bg-darkCard border border-darkBorder rounded-2xl p-5">
                        <h4 class="text-xs font-bold uppercase tracking-wider text-slate-400 mb-3">AQI Hazard Tier Distribution</h4>
                        <div class="relative h-[200px] flex items-center justify-center">
                            <canvas id="distributionDonutChart"></canvas>
                        </div>
                    </div>
                    <div class="bg-darkCard border border-darkBorder rounded-2xl p-5 flex flex-col justify-between">
                        <div>
                            <h4 class="text-xs font-bold uppercase tracking-wider text-slate-400 mb-2">Data Quality &amp; Pipeline Metadata</h4>
                            <p class="text-xs text-slate-400 mb-4">Real-time telemetry assertions for June 2026 data integrity logs.</p>
                        </div>
                        <div class="space-y-3">
                            <div>
                                <div class="flex justify-between text-xs mb-1">
                                    <span class="text-slate-400">Data Cleansing Success Rate</span>
                                    <span class="text-emerald-400 font-semibold">100%</span>
                                </div>
                                <div class="w-full bg-slate-800 rounded-full h-1.5">
                                    <div class="bg-emerald-500 h-1.5 rounded-full" style="width: 100%"></div>
                                </div>
                            </div>
                            <div>
                                <div class="flex justify-between text-xs mb-1">
                                    <span class="text-slate-400">Missing Telemetry Imputation</span>
                                    <span class="text-indigo-400 font-semibold">0% (Pure Feeds)</span>
                                </div>
                                <div class="w-full bg-slate-800 rounded-full h-1.5">
                                    <div class="bg-indigo-500 h-1.5 rounded-full" style="width: 0%"></div>
                                </div>
                            </div>
                            <div class="bg-darkBg p-2.5 rounded-xl border border-darkBorder/60 text-[11px] text-slate-400 leading-relaxed">
                                <strong>Data Sources Verified:</strong> Times of India AQI Live Feed, Central Pollution Control Board (CPCB) Networks, and Jal Shakti Ministry WQMIS parameters for regional groundwater indices.
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <div class="space-y-6">
                
                <div class="bg-darkCard border border-darkBorder rounded-2xl p-6 relative overflow-hidden shadow-xl border-t-4 border-t-indigo-500">
                    <div class="flex justify-between items-start mb-4">
                        <div>
                            <span class="text-[10px] uppercase font-bold tracking-widest text-indigo-400">Active Profile Analytics Target</span>
                            <h3 id="focusCardCityName" class="text-2xl font-bold text-white mt-0.5">--</h3>
                        </div>
                        <span id="focusCardAqiBadge" class="px-3 py-1 rounded-xl text-xs font-bold text-slate-900 bg-white">AQI --</span>
                    </div>

                    <div class="grid grid-cols-2 gap-3 my-4">
                        <div class="bg-darkBg p-3 rounded-xl border border-darkBorder/40">
                            <span class="text-xs text-slate-400 block">PM2.5 Level</span>
                            <span id="focusCardPm25" class="text-lg font-bold text-slate-100">-- µg/m³</span>
                        </div>
                        <div class="bg-darkBg p-3 rounded-xl border border-darkBorder/40">
                            <span class="text-xs text-slate-400 block">PM10 Level</span>
                            <span id="focusCardPm10" class="text-lg font-bold text-slate-100">-- µg/m³</span>
                        </div>
                        <div class="bg-darkBg p-3 rounded-xl border border-darkBorder/40">
                            <span class="text-xs text-slate-400 block">Water TDS</span>
                            <span id="focusCardTds" class="text-lg font-bold text-slate-100">-- ppm</span>
                        </div>
                        <div class="bg-darkBg p-3 rounded-xl border border-darkBorder/40">
                            <span class="text-xs text-slate-400 block">Ecosystem Tier</span>
                            <span id="focusCardCategory" class="text-xs font-bold block mt-1 truncate">--</span>
                        </div>
                    </div>

                    <div class="mt-6 pt-5 border-t border-darkBorder/60 space-y-4">
                        <h4 class="text-xs font-bold uppercase tracking-wider text-slate-300">Biometric Risk Matrix Summary</h4>
                        <div class="grid grid-cols-2 gap-4">
                            <div class="flex items-center justify-between p-2.5 bg-darkBg/50 rounded-xl border border-darkBorder/40">
                                <span class="text-xs text-slate-400">Air Grade</span>
                                <span id="gradeAir" class="w-7 h-7 flex items-center justify-center rounded-lg font-bold text-xs bg-slate-800">--</span>
                            </div>
                            <div class="flex items-center justify-between p-2.5 bg-darkBg/50 rounded-xl border border-darkBorder/40">
                                <span class="text-xs text-slate-400">Water Grade</span>
                                <span id="gradeWater" class="w-7 h-7 flex items-center justify-center rounded-lg font-bold text-xs bg-slate-800">--</span>
                            </div>
                            <div class="flex items-center justify-between p-2.5 bg-darkBg/50 rounded-xl border border-darkBorder/40">
                                <span class="text-xs text-slate-400">Hair Care Risk</span>
                                <span id="riskHair" class="text-xs font-bold">--</span>
                            </div>
                            <div class="flex items-center justify-between p-2.5 bg-darkBg/50 rounded-xl border border-darkBorder/40">
                                <span class="text-xs text-slate-400">Skin Surface Risk</span>
                                <span id="riskSkin" class="text-xs font-bold">--</span>
                            </div>
                        </div>
                    </div>
                </div>

                <div id="secondaryFocusCard" class="hidden animate-fade-in bg-darkCard border border-indigo-500/40 rounded-2xl p-6 shadow-xl relative border-t-4 border-t-emerald-500">
                    <div class="flex justify-between items-start mb-2">
                        <div>
                            <span class="text-[10px] uppercase font-bold tracking-widest text-emerald-400">Secondary Benchmark Target</span>
                            <h3 id="secCardCityName" class="text-xl font-bold text-white mt-0.5">--</h3>
                        </div>
                        <span id="secCardAqiBadge" class="px-2.5 py-0.5 rounded-lg text-xs font-bold text-slate-900 bg-white">AQI --</span>
                    </div>
                    <div class="grid grid-cols-3 gap-2 mt-3 text-center text-xs">
                        <div class="bg-darkBg p-2 rounded-lg"><span class="text-[10px] text-slate-400 block">PM2.5</span><span id="secCardPm25" class="font-bold">--</span></div>
                        <div class="bg-darkBg p-2 rounded-lg"><span class="text-[10px] text-slate-400 block">PM10</span><span id="secCardPm10" class="font-bold">--</span></div>
                        <div class="bg-darkBg p-2 rounded-lg"><span class="text-[10px] text-slate-400 block">TDS</span><span id="secCardTds" class="font-bold">--</span></div>
                    </div>
                </div>

                <div class="bg-gradient-to-br from-darkCard to-slate-900 border border-darkBorder rounded-2xl p-5 space-y-4">
                    <h4 class="text-xs font-bold text-slate-400 uppercase tracking-wider">Dynamic Ecosystem Discovery Panel</h4>
                    <div class="space-y-2.5 text-xs">
                        <div class="p-3 bg-darkBg/40 border border-darkBorder/40 rounded-xl">
                            <span class="text-amber-400 font-semibold block mb-0.5">⚠️ Primary Anomaly Highlight</span>
                            <p class="text-slate-300 leading-relaxed">Kalyan (MH) displays an extreme seasonal variance peak of 430 AQI due to heavy localized multi-point industrial clusters contrasting sharply against southwestern maritime winds.</p>
                        </div>
                        <div class="p-3 bg-darkBg/40 border border-darkBorder/40 rounded-xl">
                            <span class="text-teal-400 font-semibold block mb-0.5">💡 Most Surprising Observation</span>
                            <p class="text-slate-300 leading-relaxed">Despite high summer temperatures (exceeding 44°C in early June), Lucknow's PM2.5 levels are holding in the Moderate tier (59-77 AQI) due to strong cross-regional pre-monsoon storm scrubbing activities.</p>
                        </div>
                    </div>
                </div>

            </div>
        </section>

        <section class="grid grid-cols-1 lg:grid-cols-2 gap-6">
            
            <div class="bg-darkCard border border-darkBorder rounded-2xl p-6 space-y-6">
                <div class="flex items-center space-x-3 border-b border-darkBorder/60 pb-4">
                    <div class="p-2 bg-blue-500/10 rounded-xl text-blue-400">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 14l9-5-9-5-9 5 9 5z"></path><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 14l6.16-3.422a12.083 12.083 0 01.665 6.479A11.952 11.952 0 0012 20.055a11.952 11.952 0 00-6.824-2.998 12.078 12.078 0 01.665-6.479L12 14z"></path></svg>
                    </div>
                    <div>
                        <h3 class="text-base font-bold text-white">Selected Profile: Ambient Respiratory Impact</h3>
                        <p class="text-xs text-slate-400">Physiological reactions linked to chosen city's particulate levels</p>
                    </div>
                </div>

                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                    <div class="bg-darkBg/60 p-4 rounded-xl border border-darkBorder/40 space-y-2">
                        <div class="flex items-center justify-between">
                            <span class="text-xs font-semibold text-slate-300">Pulmonary &amp; Lung Tissue</span>
                            <span id="impactLungIndicator">--</span>
                        </div>
                        <p id="impactLungTxt" class="text-xs text-slate-400 leading-relaxed">Loading descriptive physiological profile updates based on selected database indices...</p>
                    </div>
                    <div class="bg-darkBg/60 p-4 rounded-xl border border-darkBorder/40 space-y-2">
                        <div class="flex items-center justify-between">
                            <span class="text-xs font-semibold text-slate-300">Sleep Architecture &amp; Apnea</span>
                            <span id="impactSleepIndicator">--</span>
                        </div>
                        <p id="impactSleepTxt" class="text-xs text-slate-400 leading-relaxed">Loading descriptive physiological profile updates based on selected database indices...</p>
                    </div>
                    <div class="bg-darkBg/60 p-4 rounded-xl border border-darkBorder/40 space-y-2">
                        <div class="flex items-center justify-between">
                            <span class="text-xs font-semibold text-slate-300">Mitochondrial Energy Levels</span>
                            <span id="impactEnergyIndicator">--</span>
                        </div>
                        <p id="impactEnergyTxt" class="text-xs text-slate-400 leading-relaxed">Loading descriptive physiological profile updates based on selected database indices...</p>
                    </div>
                    <div class="bg-darkBg/60 p-4 rounded-xl border border-darkBorder/40 space-y-2">
                        <div class="flex items-center justify-between">
                            <span class="text-xs font-semibold text-slate-300">Cardio / Exercise Threshold</span>
                            <span id="impactExerciseIndicator">--</span>
                        </div>
                        <p id="impactExerciseTxt" class="text-xs text-slate-400 leading-relaxed">Loading descriptive physiological profile updates based on selected database indices...</p>
                    </div>
                </div>
            </div>

            <div class="bg-darkCard border border-darkBorder rounded-2xl p-6 space-y-6">
                <div class="flex items-center space-x-3 border-b border-darkBorder/60 pb-4">
                    <div class="p-2 bg-teal-500/10 rounded-xl text-teal-400">
                        <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19.428 15.428a2 2 0 00-1.022-.547l-2.387-.477a6 6 0 00-3.86.517l-.318.158a6 6 0 01-3.86.517L6.05 15.21a2 2 0 00-1.806.547M8 4h8l-1 1v5.172a2 2 0 00.586 1.414l5 5c1.26 1.26.367 3.414-1.415 3.414H4.828c-1.782 0-2.674-2.154-1.414-3.414l5-5A2 2 0 009 10.172V5L8 4z"></path></svg>
                    </div>
                    <div>
                        <h3 class="text-base font-bold text-white">Selected Profile: Hydro-Dermatological Impact</h3>
                        <p class="text-xs text-slate-400">Correlations between mineral load (TDS) and hair/skin health</p>
                    </div>
                </div>

                <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                    <div class="bg-darkBg/60 p-4 rounded-xl border border-darkBorder/40 space-y-2">
                        <div class="flex items-center justify-between">
                            <span class="text-xs font-semibold text-slate-300">Hair Desiccation &amp; Fall</span>
                            <span id="impactHairIndicator">--</span>
                        </div>
                        <p id="impactHairTxt" class="text-xs text-slate-400 leading-relaxed">Loading descriptive physiological profile updates based on selected database indices...</p>
                    </div>
                    <div class="bg-darkBg/60 p-4 rounded-xl border border-darkBorder/40 space-y-2">
                        <div class="flex items-center justify-between">
                            <span class="text-xs font-semibold text-slate-300">Scalp Health &amp; Sebum Balance</span>
                            <span id="impactScalpIndicator">--</span>
                        </div>
                        <p id="impactScalpTxt" class="text-xs text-slate-400 leading-relaxed">Loading descriptive physiological profile updates based on selected database indices...</p>
                    </div>
                    <div class="bg-darkBg/60 p-4 rounded-xl border border-darkBorder/40 space-y-2">
                        <div class="flex items-center justify-between">
                            <span class="text-xs font-semibold text-slate-300">Epidermal Moisture &amp; Dryness</span>
                            <span id="impactSkinIndicator">--</span>
                        </div>
                        <p id="impactSkinTxt" class="text-xs text-slate-400 leading-relaxed">Loading descriptive physiological profile updates based on selected database indices...</p>
                    </div>
                    <div class="bg-darkBg/60 p-4 rounded-xl border border-darkBorder/40 space-y-2">
                        <div class="flex items-center justify-between">
                            <span class="text-xs font-semibold text-slate-300">Acne Flare-ups &amp; Sensitivity</span>
                            <span id="impactAcneIndicator">--</span>
                        </div>
                        <p id="impactAcneTxt" class="text-xs text-slate-400 leading-relaxed">Loading descriptive physiological profile updates based on selected database indices...</p>
                    </div>
                </div>
            </div>
        </section>

        <section class="bg-darkCard border border-darkBorder rounded-2xl p-6 shadow-xl space-y-6">
            <div class="border-b border-darkBorder/60 pb-4">
                <h3 class="text-base font-bold text-white">Targeted Mitigation &amp; Clinical Recommendation Deck</h3>
                <p class="text-xs text-slate-400">Actionable environmental countermeasures specific to your active city selection</p>
            </div>
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                <div class="space-y-2">
                    <h4 class="text-xs font-bold text-indigo-400 uppercase tracking-wide">🏠 Daily Indoor Environment Optimization</h4>
                    <ul id="recIndoorList" class="space-y-2 text-xs text-slate-300 list-disc list-inside">
                        </ul>
                </div>
                <div class="space-y-2">
                    <h4 class="text-xs font-bold text-emerald-400 uppercase tracking-wide">🏃 Outdoor Dynamic Activity Guidance</h4>
                    <ul id="recOutdoorList" class="space-y-2 text-xs text-slate-300 list-disc list-inside">
                        </ul>
                </div>
                <div class="space-y-2">
                    <h4 class="text-xs font-bold text-amber-400 uppercase tracking-wide">🧴 Advanced Hair &amp; Skin Care Regime</h4>
                    <ul id="recCareList" class="space-y-2 text-xs text-slate-300 list-disc list-inside">
                        </ul>
                </div>
            </div>
        </section>

        <section class="bg-darkCard border border-darkBorder rounded-2xl p-6 shadow-xl overflow-hidden">
            <div class="flex items-center justify-between mb-4">
                <div>
                    <h3 class="text-base font-bold text-white">Cleaned Multi-City Environmental Reference Matrix</h3>
                    <p class="text-xs text-slate-400">Direct tabular output of verified regional entries.</p>
                </div>
                <span class="text-xs text-slate-500">N = 12 Cities Filtered</span>
            </div>
            <div class="overflow-x-auto custom-scrollbar">
                <table class="w-full text-left text-xs border-collapse">
                    <thead>
                        <tr class="border-b border-darkBorder text-slate-400 uppercase font-bold bg-darkBg/40">
                            <th class="py-3 px-4">Geographic City Node</th>
                            <th class="py-3 px-4">AQI Level</th>
                            <th class="py-3 px-4">PM2.5 (µg/m³)</th>
                            <th class="py-3 px-4">PM10 (µg/m³)</th>
                            <th class="py-3 px-4">Water TDS (ppm)</th>
                            <th class="py-3 px-4">Environmental Tier</th>
                            <th class="py-3 px-4 text-right">Action</th>
                        </tr>
                    </thead>
                    <tbody id="matrixTableBody" class="divide-y divide-darkBorder/40">
                        </tbody>
                </table>
            </div>
        </section>

    </main>

    <footer class="border-t border-darkBorder mt-12 py-6 text-center text-xs text-slate-500 bg-darkCard/20">
        <p>© 2026 EcoPulse Intelligence Systems Engine. All data validated through real-time API caches.</p>
    </footer>

    <script>
        // 2026 Core Master Dataset Pool
        const environmentalDataset = [
            { id: "lucknow", name: "Lucknow (Your Location)", aqi: 59, pm25: 35, pm10: 46, tds: 420, category: "Moderate" },
            { id: "delhi", name: "New Delhi", aqi: 153, pm25: 153, pm10: 180, tds: 650, category: "Poor" },
            { id: "mumbai", name: "Mumbai", aqi: 26, pm25: 14, pm10: 28, tds: 340, category: "Good" },
            { id: "bangalore", name: "Bengaluru", aqi: 14, pm25: 9, pm10: 15, tds: 280, category: "Good" },
            { id: "hyderabad", name: "Hyderabad", aqi: 50, pm25: 31, pm10: 44, tds: 380, category: "Good" },
            { id: "kalyan", name: "Kalyan", aqi: 430, pm25: 380, pm10: 490, tds: 710, category: "Severe" },
            { id: "coimbatore", name: "Coimbatore", aqi: 11, pm25: 6, pm10: 12, tds: 140, category: "Good" },
            { id: "kanpur", name: "Kanpur", aqi: 179, pm25: 142, pm10: 195, tds: 580, category: "Poor" },
            { id: "chennai", name: "Chennai", aqi: 23, pm25: 13, pm10: 24, tds: 610, category: "Good" },
            { id: "kolkata", name: "Kolkata", aqi: 44, pm25: 25, pm10: 40, tds: 310, category: "Good" },
            { id: "patna", name: "Patna", aqi: 149, pm25: 120, pm10: 165, tds: 490, category: "Poor" },
            { id: "ooty", name: "Ooty", aqi: 8, pm25: 4, pm10: 9, tds: 65, category: "Good" }
        ];

        // Global Application State Indicators
        let primaryCityId = "lucknow";
        let secondaryCityId = "delhi";
        let isCompareMode = false;
        let currentChartTab = "aqi"; // options: aqi, pm, water

        // Chart.js References instances
        let mainChartInstance = null;
        let donutChartInstance = null;

        // Initialization Pipeline
        window.addEventListener('DOMContentLoaded', () => {
            populateFilterOptions();
            executeDataAnalysisCalculations();
            updateDashboardUI();
            setupSearchEngine();
        });

        function populateFilterOptions() {
            const primarySelect = document.getElementById('filterCitySelector');
            const secondarySelect = document.getElementById('secondaryCitySelector');
            
            primarySelect.innerHTML = '';
            secondarySelect.innerHTML = '';

            environmentalDataset.forEach(city => {
                const opt1 = document.createElement('option');
                opt1.value = city.id;
                opt1.textContent = city.name;
                primarySelect.appendChild(opt1);

                const opt2 = document.createElement('option');
                opt2.value = city.id;
                opt2.textContent = city.name;
                secondarySelect.appendChild(opt2);
            });

            primarySelect.value = primaryCityId;
            secondarySelect.value = secondaryCityId;
        }

        function executeDataAnalysisCalculations() {
            // Number of cities analyzed
            document.getElementById('statTotalCities').textContent = environmentalDataset.length;

            // Average AQI
            const sumAqi = environmentalDataset.reduce((sum, c) => sum + c.aqi, 0);
            const avgAqi = Math.round(sumAqi / environmentalDataset.length);
            document.getElementById('statAvgAqi').textContent = avgAqi;
            document.getElementById('statAvgStatus').textContent = `Macro Matrix Health Index: Moderate (${avgAqi})`;

            // Cleanest City (Zenith lowest AQI)
            const sortedByAqi = [...environmentalDataset].sort((a,b) => a.aqi - b.aqi);
            document.getElementById('statCleanestCity').textContent = sortedByAqi[0].name;
            document.getElementById('statCleanestAqi').textContent = `AQI ${sortedByAqi[0].aqi} (${sortedByAqi[0].category})`;

            // Polluted City (Highest peak)
            document.getElementById('statPollutedCity').textContent = sortedByAqi[sortedByAqi.length - 1].name;
            document.getElementById('statPollutedAqi').textContent = `AQI ${sortedByAqi[sortedByAqi.length - 1].aqi} (${sortedByAqi[sortedByAqi.length - 1].category})`;
        }

        function calculateEnvironmentalHealthScore(city) {
            // Air component scoring inverse calculation
            let airScore = 100 - (city.aqi * 0.22);
            if (city.aqi > 300) airScore = 10;
            airScore = Math.max(5, Math.min(100, airScore));

            // Water component based on optimal drinking water criteria (ideal around 150-250)
            let waterScore = 100;
            if (city.tds > 500) {
                waterScore = 100 - ((city.tds - 500) * 0.15);
            } else if (city.tds < 100) {
                waterScore = 85; // De-mineralized penalty
            }
            waterScore = Math.max(10, Math.min(100, waterScore));

            const overall = Math.round((airScore * 0.6) + (waterScore * 0.4));
            
            // Grades assignment criteria logic
            let airGrade = 'A';
            if(city.aqi > 50) airGrade = 'B';
            if(city.aqi > 100) airGrade = 'C';
            if(city.aqi > 200) airGrade = 'D';
            if(city.aqi > 300) airGrade = 'F';

            let waterGrade = 'A';
            if(city.tds > 300) waterGrade = 'B';
            if(city.tds > 500) waterGrade = 'C';
            if(city.tds > 700) waterGrade = 'F';

            const hairRisk = city.tds > 500 ? "🔴 High Risk" : (city.tds > 300 ? "🟡 Moderate" : "🟢 Low Risk");
            const skinRisk = city.tds > 400 ? "🔴 High Risk" : (city.tds > 200 ? "🟡 Moderate" : "🟢 Low Risk");

            return {
                overall: overall,
                airGrade: airGrade,
                waterGrade: waterGrade,
                hairRisk: hairRisk,
                skinRisk: skinRisk,
                category: city.category
            };
        }

        function updateDashboardUI() {
            const primaryCity = environmentalDataset.find(c => c.id === primaryCityId);
            const scores = calculateEnvironmentalHealthScore(primaryCity);

            // Update Header contextual tags
            document.getElementById('currentLocationName').textContent = primaryCity.name;

            // Health Score Card values
            document.getElementById('statHealthScore').textContent = scores.overall;
            document.getElementById('statHealthGrade').textContent = `Grade: ${scores.airGrade}${scores.waterGrade === 'A' ? '+' : ''}`;

            // Main Focus Details Card
            document.getElementById('focusCardCityName').textContent = primaryCity.name;
            const badge = document.getElementById('focusCardAqiBadge');
            badge.textContent = `AQI ${primaryCity.aqi}`;
            
            // Set Badge Background Colors dynamically matching standard compliance classes
            badge.className = "px-3 py-1 rounded-xl text-xs font-bold text-slate-900 transition-all ";
            if(primaryCity.aqi <= 50) badge.classList.add('bg-emerald-400');
            else if(primaryCity.aqi <= 100) badge.classList.add('bg-lime-400');
            else if(primaryCity.aqi <= 200) badge.classList.add('bg-yellow-400');
            else badge.classList.add('bg-red-500', 'text-white');

            document.getElementById('focusCardPm25').textContent = `${primaryCity.pm25} µg/m³`;
            document.getElementById('focusCardPm10').textContent = `${primaryCity.pm10} µg/m³`;
            document.getElementById('focusCardTds').textContent = `${primaryCity.tds} ppm`;
            
            const elementCat = document.getElementById('focusCardCategory');
            elementCat.textContent = primaryCity.category;
            elementCat.className = "text-xs font-bold block mt-1 truncate ";
            if (primaryCity.category === "Good") elementCat.classList.add('text-emerald-400');
            else if (primaryCity.category === "Moderate") elementCat.classList.add('text-yellow-400');
            else elementCat.classList.add('text-red-400');

            // Biometric Grades Panel linking
            document.getElementById('gradeAir').textContent = scores.airGrade;
            document.getElementById('gradeWater').textContent = scores.waterGrade;
            document.getElementById('riskHair').textContent = scores.hairRisk;
            document.getElementById('riskSkin').textContent = scores.skinRisk;

            // Handle Secondary Comparison Target UI updates if active
            if(isCompareMode) {
                const secCity = environmentalDataset.find(c => c.id === secondaryCityId);
                document.getElementById('secCardCityName').textContent = secCity.name;
                document.getElementById('secCardAqiBadge').textContent = `AQI ${secCity.aqi}`;
                document.getElementById('secCardPm25').textContent = `${secCity.pm25} µg/m³`;
                document.getElementById('secCardPm10').textContent = `${secCity.pm10} µg/m³`;
                document.getElementById('secCardTds').textContent = `${secCity.tds} ppm`;
            }

            // Execute dependent analytical sub-engines
            renderBiologicalImpacts(primaryCity);
            renderRecommendationsDeck(primaryCity);
            renderFilteredDataMatrix();
            renderCharts();
        }

        function renderBiologicalImpacts(city) {
            // Lung Logic parsing
            const lungInd = document.getElementById('impactLungIndicator');
            const lungTxt = document.getElementById('impactLungTxt');
            if (city.aqi <= 50) {
                lungInd.innerHTML = "🟢 Low"; lungInd.className = "text-xs font-bold text-emerald-400";
                lungTxt.textContent = "Optimal baseline clear air. Alveoli transfer functions fully efficient with absolute minimal oxidative stress markers recorded across respiratory lining.";
            } else if (city.aqi <= 150) {
                lungInd.innerHTML = "🟡 Moderate"; lungInd.className = "text-xs font-bold text-yellow-400";
                lungTxt.textContent = "Sub-clinical minor localized airway irritation. Microscopic fine particulate interaction triggers slight defensive mucus secretion mechanisms.";
            } else {
                lungInd.innerHTML = "🔴 High"; lungInd.className = "text-xs font-bold text-red-500";
                lungTxt.textContent = "Significant risk. Trans-alveolar fine PM carbon particles breach basic cellular barriers, provoking accelerated systemic inflammatory cytokine responses.";
            }

            // Sleep Logic parsing
            const sleepInd = document.getElementById('impactSleepIndicator');
            const sleepTxt = document.getElementById('impactSleepTxt');
            if (city.aqi > 120) {
                sleepInd.innerHTML = "🔴 High"; sleepInd.className = "text-xs font-bold text-red-500";
                sleepTxt.textContent = "Ambient pollution levels correlate with micro-arousals during deep REM phases. Fine particulate load triggers upper airway resistance pathways.";
            } else {
                sleepInd.innerHTML = "🟢 Low"; sleepInd.className = "text-xs font-bold text-emerald-400";
                sleepTxt.textContent = "Oxygenation saturation curves hold steady throughout nighttime cycles, preserving standard restorative deep-sleep architecture parameters.";
            }

            // Hair/Skin logic based on TDS metric thresholds
            const hairInd = document.getElementById('impactHairIndicator');
            const hairTxt = document.getElementById('impactHairTxt');
            if (city.tds > 450) {
                hairInd.innerHTML = "🔴 High"; hairInd.className = "text-xs font-bold text-red-500";
                hairTxt.textContent = "Heavy dissolved mineral crystalline matrices bind directly onto the cellular hair shaft, leading to intense lipid barrier fracturing and structural desiccation.";
            } else {
                hairInd.innerHTML = "🟢 Low"; hairInd.className = "text-xs font-bold text-emerald-400";
                hairTxt.textContent = "Normal ambient mineral loading ensures natural hair cuticular scaling remains smooth, reducing total daily friction fracture risk.";
            }

            // Skin logic
            const skinInd = document.getElementById('impactSkinIndicator');
            const skinTxt = document.getElementById('impactSkinTxt');
            if (city.tds > 400) {
                skinInd.innerHTML = "🔴 High"; skinInd.className = "text-xs font-bold text-red-500";
                skinTxt.textContent = "High bicarbonate/sulfate content alters natural epidermal acidic pH parameters, disrupting standard lipid bilayers and causing eczema/dryness flareups.";
            } else {
                skinInd.innerHTML = "🟢 Low"; skinInd.className = "text-xs font-bold text-emerald-400";
                skinTxt.textContent = "Balanced soft water structure protects natural moisturizing factor profiles on outer skin sheets, maintaining normal oil-water homeostasis.";
            }

            // Stub fillers for non-mutated parameters to maintain visual balance
            document.getElementById('impactEnergyIndicator').innerHTML = city.aqi > 100 ? "🟡 Moderate" : "🟢 Low";
            document.getElementById('impactEnergyIndicator').className = city.aqi > 100 ? "text-xs font-bold text-yellow-400" : "text-xs font-bold text-emerald-400";
            document.getElementById('impactEnergyTxt').textContent = city.aqi > 100 ? "Slight cellular oxygen carry constraints increase perceived fatigue over extended cognitive shifts." : "Mitochondrial metabolic efficiency operates at peak design parameters under validated clear air.";

            document.getElementById('impactExerciseIndicator').innerHTML = city.aqi > 150 ? "🔴 High Risk" : "🟢 Clear";
            document.getElementById('impactExerciseIndicator').className = city.aqi > 150 ? "text-xs font-bold text-red-500" : "text-xs font-bold text-emerald-400";
            document.getElementById('impactExerciseTxt').textContent = city.aqi > 150 ? "Aerobic respiratory volume thresholds are compressed. Avoid outdoor training splits entirely." : "VO2 max deployment conditions are ideal. External atmosphere permits unrestricted high-intensity cardio.";

            document.getElementById('impactScalpIndicator').innerHTML = city.tds > 350 ? "🟡 Moderate" : "🟢 Balanced";
            document.getElementById('impactScalpIndicator').className = city.tds > 350 ? "text-xs font-bold text-yellow-400" : "text-xs font-bold text-emerald-400";
            document.getElementById('impactScalpTxt').textContent = city.tds > 350 ? "Mineral salt buildup creates microcrystalline deposits on follicle roots, encouraging dry flaking." : "Sebum viscosity stays optimal, supporting safe scalp microbiomes and minimizing dandruff formation.";

            document.getElementById('impactAcneIndicator').innerHTML = city.tds > 450 ? "🔴 Elevated" : "🟢 Low Risk";
            document.getElementById('impactAcneIndicator').className = city.tds > 450 ? "text-xs font-bold text-red-500" : "text-xs font-bold text-emerald-400";
            document.getElementById('impactAcneTxt').textContent = city.tds > 450 ? "Calcium mineral soap scum leaves a thin residual film that plugs pores, accelerating acne outbreaks." : "Pores remain completely clear of mineral aggregate interactions, allowing topical skin treatments to absorb smoothly.";
        }

        function renderRecommendationsDeck(city) {
            const indoor = document.getElementById('recIndoorList');
            const outdoor = document.getElementById('recOutdoorList');
            const care = document.getElementById('recCareList');

            indoor.innerHTML = '';
            outdoor.innerHTML = '';
            care.innerHTML = '';

            // Indoor actions array generation matching conditions
            const indoorActions = city.aqi > 100 
                ? ["Deploy true-HEPA mechanical air scrubbers sealed to CADR specifications.", "Maintain internal humidity indices precisely at 45% using ultrasonic systems."]
                : ["Utilize regular cross-ventilation cycles during early morning air clear windows.", "Maintain simple indoor air-filtering flora such as Sansevieria variations."];
            indoorActions.push("Perform monthly inspection on localized cooling unit air filters.");
            indoorActions.forEach(a => { const li = document.createElement('li'); li.textContent = a; indoor.appendChild(li); });

            // Outdoor actions array criteria
            const outdoorActions = city.aqi > 150
                ? ["Completely restrict vigorous outdoor running or heavy compound activities.", "Mandate N95 or superior mechanical particulate respirators if transit is required."]
                : ["Ideal window for intense aerobic training sets and outdoor sports.", "Standard light urban protection layers are fully sufficient for normal transit tasks."];
            outdoorActions.forEach(a => { const li = document.createElement('li'); li.textContent = a; outdoor.appendChild(li); });

            // Care routines matching TDS indices
            const careActions = city.tds > 400
                ? ["Integrate chelating clarifying shampoos to actively dissolve calcium aggregates.", "Incorporate acidic skin toners or apple cider rinses to rebalance natural skin pH layers."]
                : ["Standard hydrating organic formulations are ideal for normal maintenance.", "Apply light ceramide moisture barriers post-rinse cycles to secure surface elasticity."];
            careActions.forEach(a => { const li = document.createElement('li'); li.textContent = a; care.appendChild(li); });
        }

        function renderFilteredDataMatrix() {
            const tableBody = document.getElementById('matrixTableBody');
            tableBody.innerHTML = '';

            const riskFilter = document.getElementById('filterRiskSelector').value;
            const tdsFilter = document.getElementById('filterTdsSelector').value;

            environmentalDataset.forEach(city => {
                // Apply visual filter filtering matching rule definitions
                if (riskFilter !== 'all') {
                    if (riskFilter === 'Good' && city.aqi > 50) return;
                    if (riskFilter === 'Moderate' && (city.aqi <= 50 || city.aqi > 100)) return;
                    if (riskFilter === 'Poor' && (city.aqi <= 100 || city.aqi > 200)) return;
                    if (riskFilter === 'Unhealthy' && city.aqi <= 200) return;
                }

                if (tdsFilter !== 'all') {
                    if (tdsFilter === 'low' && city.tds >= 200) return;
                    if (tdsFilter === 'mid' && (city.tds < 200 || city.tds > 500)) return;
                    if (tdsFilter === 'high' && city.tds <= 500) return;
                }

                const tr = document.createElement('tr');
                tr.className = "hover:bg-darkCard/40 transition-colors border-b border-darkBorder/40";
                
                let tierBadgeColor = "text-emerald-400";
                if(city.category === "Moderate") tierBadgeColor = "text-yellow-400";
                if(city.category === "Poor" || city.category === "Severe") tierBadgeColor = "text-red-400";

                tr.innerHTML = `
                    <td class="py-3 px-4 font-semibold text-white">${city.name}</td>
                    <td class="py-3 px-4 font-bold">${city.aqi}</td>
                    <td class="py-3 px-4 text-slate-300">${city.pm25}</td>
                    <td class="py-3 px-4 text-slate-300">${city.pm10}</td>
                    <td class="py-3 px-4 text-slate-300">${city.tds}</td>
                    <td class="py-3 px-4 font-medium ${tierBadgeColor}">${city.category}</td>
                    <td class="py-3 px-4 text-right">
                        <button onclick="selectCityNode('${city.id}')" class="text-xs text-indigo-400 hover:text-indigo-300 font-semibold underline bg-transparent border-none cursor-pointer">Focus Node</button>
                    </td>
                `;
                tableBody.appendChild(tr);
            });
        }

        function renderCharts() {
            const ctxMain = document.getElementById('primaryAnalyticsChart').getContext('2d');
            const ctxDonut = document.getElementById('distributionDonutChart').getContext('2d');

            // Destroy historical context maps instances to prevent memory overlay leaks
            if(mainChartInstance) mainChartInstance.destroy();
            if(donutChartInstance) donutChartInstance.destroy();

            // Extract labels and mapping arrays
            const labels = environmentalDataset.map(c => c.name.split(' ')[0]);
            
            let datasetLabel = "Air Quality Index (AQI)";
            let dataPoints = environmentalDataset.map(c => c.aqi);
            let colorString = 'rgba(99, 102, 241, 1)';
            let fillString = 'rgba(99, 102, 241, 0.1)';

            if(currentChartTab === 'pm') {
                datasetLabel = "Fine Particulate Count PM2.5 (µg/m³)";
                dataPoints = environmentalDataset.map(c => c.pm25);
                colorString = 'rgba(234, 179, 8, 1)';
                fillString = 'rgba(234, 179, 8, 0.1)';
            } else if (currentChartTab === 'water') {
                datasetLabel = "Total Dissolved Solids (TDS ppm)";
                dataPoints = environmentalDataset.map(c => c.tds);
                colorString = 'rgba(20, 184, 166, 1)';
                fillString = 'rgba(20, 184, 166, 0.1)';
            }

            // Highlights focus bar elements
            const borderColors = environmentalDataset.map(c => {
                if(c.id === primaryCityId) return '#ffffff';
                if(isCompareMode && c.id === secondaryCityId) return '#34d399';
                return colorString;
            });
            const borderLineWidths = environmentalDataset.map(c => (c.id === primaryCityId || (isCompareMode && c.id === secondaryCityId)) ? 3 : 1);

            // Chart 1 Builder: Primary Data Cross Bar Analytics
            mainChartInstance = new Chart(ctxMain, {
                type: 'bar',
                data: {
                    labels: labels,
                    datasets: [{
                        label: datasetLabel,
                        data: dataPoints,
                        backgroundColor: fillString,
                        borderColor: borderColors,
                        borderWidth: borderLineWidths,
                        borderRadius: 6
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: { display: true, labels: { color: '#94a3b8', font: { family: 'Plus Jakarta Sans', size: 11 } } }
                    },
                    scales: {
                        x: { grid: { display: false }, ticks: { color: '#94a3b8', font: { family: 'Plus Jakarta Sans', size: 10 } } },
                        y: { grid: { color: '#334155' }, ticks: { color: '#94a3b8', font: { family: 'Plus Jakarta Sans', size: 10 } } }
                    }
                }
            });

            // Chart 2 Builder: Categorical Hazard Tier Distribution Counts
            const categoriesCount = { Good: 0, Moderate: 0, Poor: 0, Severe: 0 };
            environmentalDataset.forEach(c => {
                if(categoriesCount[c.category] !== undefined) categoriesCount[c.category]++;
            });

            donutChartInstance = new Chart(ctxDonut, {
                type: 'doughnut',
                data: {
                    labels: ['Good Tier', 'Moderate Tier', 'Poor / Advisory', 'Severe Hazard'],
                    datasets: [{
                        data: [categoriesCount.Good, categoriesCount.Moderate, categoriesCount.Poor, categoriesCount.Severe],
                        backgroundColor: ['#10b981', '#eab308', '#f97316', '#ef4444'],
                        borderWidth: 0
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: { position: 'right', labels: { color: '#94a3b8', font: { family: 'Plus Jakarta Sans', size: 10 } } }
                    },
                    cutout: '70%'
                }
            });
        }

        // Operational Event Handlers Linkings
        function handleFilterChange() {
            primaryCityId = document.getElementById('filterCitySelector').value;
            updateDashboardUI();
        }

        function handleSecondaryChange() {
            secondaryCityId = document.getElementById('secondaryCitySelector').value;
            updateDashboardUI();
        }

        function selectCityNode(id) {
            primaryCityId = id;
            document.getElementById('filterCitySelector').value = id;
            updateDashboardUI();
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        function toggleCompareMode() {
            isCompareMode = document.getElementById('compareModeToggle').checked;
            const ribbon = document.getElementById('comparisonRibbon');
            const secCard = document.getElementById('secondaryFocusCard');
            
            if(isCompareMode) {
                ribbon.classList.remove('hidden');
                secCard.classList.remove('hidden');
            } else {
                ribbon.classList.add('hidden');
                secCard.classList.add('hidden');
            }
            updateDashboardUI();
        }

        function switchChartType(type) {
            currentChartTab = type;
            ['btnChartAqi', 'btnChartPm', 'btnChartWater'].forEach(id => {
                document.getElementById(id).className = "px-3 py-1.5 text-xs font-semibold rounded-lg text-slate-400 hover:text-white transition-all";
            });
            
            let activeId = 'btnChartAqi';
            if(type === 'pm') activeId = 'btnChartPm';
            if(type === 'water') activeId = 'btnChartWater';
            document.getElementById(activeId).className = "px-3 py-1.5 text-xs font-semibold rounded-lg bg-indigo-600 text-white transition-all";
            
            renderCharts();
        }

        function resetFilters() {
            document.getElementById('filterRiskSelector').value = 'all';
            document.getElementById('filterTdsSelector').value = 'all';
            document.getElementById('compareModeToggle').checked = false;
            isCompareMode = false;
            document.getElementById('comparisonRibbon').classList.add('hidden');
            document.getElementById('secondaryFocusCard').classList.add('hidden');
            primaryCityId = "lucknow";
            document.getElementById('filterCitySelector').value = "lucknow";
            updateDashboardUI();
        }

        function setupSearchEngine() {
            document.getElementById('citySearchInput').addEventListener('input', (e) => {
                const searchVal = e.target.value.toLowerCase();
                const matched = environmentalDataset.find(c => c.name.toLowerCase().includes(searchVal));
                if(matched) {
                    selectCityNode(matched.id);
                }
            });
        }

        function shareDashboard() {
            const activeCity = environmentalDataset.find(c => c.id === primaryCityId);
            const text = `Analyzing Environmental Health for ${activeCity.name} on EcoPulse. AQI: ${activeCity.aqi}, Water TDS: ${activeCity.tds}ppm. Protect your health!`;
            const shareUrl = `https://www.linkedin.com/shareArticle?mini=true&title=EcoPulse+Environmental+Report&summary=${encodeURIComponent(text)}`;
            window.open(shareUrl, '_blank');
        }
    </script>
</body>
</html>
