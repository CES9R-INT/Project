
	<head>
    
	<meta charset="UTF-8">
    
	<title>Accès Sécurisé | SASU CESAR INTERNATIONAL</title>
    
	<script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    
	<script src="https://cdn.tailwindcss.com"></script>
    
	<link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
   
	<style>
      
		@import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800;900&display=swap');
        
        :root {
            --primary: #0f172a;
            --secondary: #64748b;
            --success: #22c55e;
            --danger: #ef4444;
            --bg: #ffffff;
            --surface: #f8fafc;
            --border: #e2e8f0;
        }

        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--bg);
            color: var(--primary);
            margin: 0;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
        }

        /* --- ÉCRAN DE VERROUILLAGE --- */
        #lock-screen {
            position: fixed;
            inset: 0;
            background: rgba(15, 23, 42, 0.98);
            backdrop-filter: blur(10px);
            z-index: 9999;
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            color: white;
            transition: opacity 0.5s ease, visibility 0.5s;
        }

        .login-box {
            text-align: center;
            width: 320px;
        }

        .login-box h1 { font-size: 1.5rem; margin-bottom: 2rem; font-weight: 800; letter-spacing: -1px; }

        .login-box input {
            width: 100%;
            padding: 12px;
            border-radius: 8px;
            border: 1px solid rgba(255,255,255,0.2);
            background: rgba(255,255,255,0.1);
            color: white;
            text-align: center;
            font-size: 1rem;
            margin-bottom: 1rem;
            outline: none;
            transition: border 0.3s;
        }

        .login-box input:focus { border-color: var(--success); }

        .login-box button {
            background: var(--success);
            color: var(--primary);
            border: none;
            padding: 12px 24px;
            border-radius: 8px;
            font-weight: 700;
            cursor: pointer;
            width: 100%;
            transition: transform 0.2s;
        }

        .login-box button:hover { transform: translateY(-2px); }
        
        #error-msg { color: var(--danger); font-size: 0.8rem; margin-top: 10px; display: none; }

        /* --- DASHBOARD --- */
        #main-content {
            opacity: 0;
            pointer-events: none;
            padding: 40px;
            flex: 1;
            display: flex;
            justify-content: center;
            transition: opacity 0.8s ease;
        }

        .unlocked #lock-screen { opacity: 0; visibility: hidden; }
        .unlocked #main-content { opacity: 1; pointer-events: auto; }
        .unlocked body { overflow: auto; }

        /* --- TABS --- */
        .container { width: 100%; max-width: 1100px; }
        .tabs-nav { display: flex; gap: 10px; margin-bottom: 30px; border-bottom: 2px solid var(--border); flex-wrap: wrap; }
        .tab-button { padding: 12px 24px; cursor: pointer; background: #f1f5f9; border: 1px solid var(--border); border-bottom: none; border-radius: 8px 8px 0 0; font-size: 14px; font-weight: 600; color: var(--secondary); transition: all 0.3s; }
        .tab-button:hover { background: #e2e8f0; }

        input[type="radio"] { display: none; }
        #tab1:checked ~ .tabs-nav label[for="tab1"], 
        #tab2:checked ~ .tabs-nav label[for="tab2"],
        #tab3:checked ~ .tabs-nav label[for="tab3"],
        #tab4:checked ~ .tabs-nav label[for="tab4"],
        #tab5:checked ~ .tabs-nav label[for="tab5"] { background: var(--primary); color: white; border-color: var(--primary); }

        .page-content { display: none; }
        #tab1:checked ~ #content1, 
        #tab2:checked ~ #content2,
        #tab3:checked ~ #content3,
        #tab4:checked ~ #content4,
        #tab5:checked ~ #content5 { display: block; }

        /* --- TABLES & GRAPHS --- */
        .header { display: flex; justify-content: space-between; align-items: flex-end; margin-bottom: 40px; border-bottom: 1px solid var(--border); padding-bottom: 20px; flex-wrap: wrap; gap: 15px; }
        .brand { font-size: 24px; font-weight: 800; letter-spacing: -1px; }
        .brand span { font-weight: 300; color: var(--secondary); margin-left: 10px; }
        .date-tag { font-size: 12px; font-weight: 600; color: var(--secondary); text-transform: uppercase; }

        .dashboard-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 60px; }
        @media (max-width: 900px) { .dashboard-grid { grid-template-columns: 1fr; gap: 30px; } }
        
        h2 { font-size: 14px; text-transform: uppercase; letter-spacing: 1px; color: var(--secondary); margin-bottom: 25px; }

        table { width: 100%; border-collapse: collapse; }
        td, th { padding: 14px 0; font-size: 14px; border-bottom: 1px solid #f1f5f9; text-align: left; }
        th { font-weight: 600; color: var(--secondary); }
        .val { text-align: right; font-family: 'Roboto Mono', monospace; font-weight: 500; }
        .neg { color: var(--danger); }
        .pos { color: var(--success); font-weight: 700; }
        .total-row { background-color: var(--surface); }

        .graph-container { background: var(--surface); padding: 30px; border-radius: 12px; border: 1px solid var(--border); }
        .bar-group { margin-bottom: 22px; }
        .bar-label { display: flex; justify-content: space-between; font-size: 12px; font-weight: 600; margin-bottom: 8px; }
        .bar-bg { width: 100%; height: 10px; background: #e2e8f0; border-radius: 20px; overflow: hidden; }
        .bar-fill { height: 100%; border-radius: 20px; transition: width 1.5s; }
        .fill-ca { background: var(--primary); width: 100%; }
        .fill-achats { background: var(--secondary); }
        .fill-cv { background: #3b82f6; }
        .fill-is { background: var(--danger); }
        .fill-net { background: var(--success); }

        /* --- SCI --- */
        .sci-container { background: white; padding: 25px; border-radius: 12px; }
        .kpi-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(150px, 1fr)); gap: 12px; margin-bottom: 20px; }
        .card { background: #fff; padding: 15px; border-radius: 8px; text-align: center; border: 1px solid #e2e8f0; border-top: 4px solid #3182ce; }
        .card span { font-size: 0.7rem; color: #718096; font-weight: bold; text-transform: uppercase; }
        .card b { display: block; font-size: 1.2rem; color: #2d3748; margin-top: 5px; }
        .chart-main { height: 350px; margin-bottom: 20px; position: relative; }
        .chart-sub { height: 250px; margin-bottom: 25px; padding: 15px; background: #fcfcfc; border-radius: 8px; border: 1px solid #eee; }
        .details-section { display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 20px; margin-bottom: 30px; }
        .sub-card { background: #fdfdfd; padding: 15px; border-radius: 8px; border: 1px solid #eee; }
        .renta-tag { background: #ebf8ff; color: #3182ce; padding: 2px 6px; border-radius: 4px; font-weight: bold; }
        .scenario-box { background: #f0f9ff; padding: 20px; border-radius: 12px; border: 1px solid #bee3f8; margin-top: 20px; overflow-x: auto; }
        .buy-event { background: #f0fff4; color: #2f855a; font-weight: bold; border-radius: 4px; padding: 4px 8px; font-size: 0.7rem; border: 1px solid #c6f6d5; display: inline-block; }

        .footer { 
            width: 100%;
            margin-top: auto; 
            font-size: 12px; 
            color: var(--secondary); 
            text-align: center; 
            border-top: 1px solid var(--border); 
            padding: 20px 0;
            background: var(--bg);
        }

        /* --- STYLES SUPPLEMENTAIRES --- */
        @media (max-width: 768px) {
            #main-content { padding: 20px; }
            .tab-button { padding: 8px 16px; font-size: 12px; }
        }
    </style>
</head>
<body>

    <div id="lock-screen">
        <div class="login-box">
            <h1>SASU CESAR INTERNATIONAL<br><span style="font-weight: 300; font-size: 0.9rem; color: #94a3b8;">FINANCIAL DATA ACCESS</span></h1>
            <input type="password" id="password" placeholder="Mot de passe" onkeypress="handleKeyPress(event)">
            <button onclick="checkPassword()">DÉVERROUILLER</button>
            <div id="error-msg">Code incorrect. Accès refusé.</div>
        </div>
    </div>

    <div id="main-content">
        <div class="container">
	    <input type="radio" id="tab5" name="dashboard-tabs">
	    <input type="radio" id="tab4" name="dashboard-tabs" checked>
            <input type="radio" id="tab1" name="dashboard-tabs"> 
            <input type="radio" id="tab2" name="dashboard-tabs">
            <input type="radio" id="tab3" name="dashboard-tabs">
            
            

            <div class="tabs-nav">
		<label for="tab4" class="tab-button">ORGANIGRAMME</label>
		<label for="tab5" class="tab-button">TABLEAU DE BORD</label>
		<label for="tab1" class="tab-button">EXERCICE 5</label>
                <label for="tab2" class="tab-button">EXERCICE 6.SEMESTRE 1</label>
                <label for="tab3" class="tab-button">T.S.G. ANALYSE</label>
                
            </div>

            <div id="content1" class="page-content">
                <div class="header">
                    <div class="brand">SASU CESAR INTERNATIONAL <span>Exercice 5</span></div>
                    <div class="date-tag">Rapport</div>
                </div>
                <div class="dashboard-grid">
                    <div class="table-section">
                        <h2>Compte de Résultat Simplifié</h2>
                        <table>
                            <thead>
                                <tr><th>Postes</th><th class="val">Montant (€)</th></tr>
                            </thead>
                            <tbody>
                                <tr><td>Chiffre d'Affaires (HT)</td><td class="val">1 568 156</td></tr>
                                <tr><td>Achats de Marchandises (HT)</td><td class="val neg">- 1 484 620</td></tr>
                                <tr><td>Charges Variables CV (HT)</td><td class="val neg">- 22 285</td></tr>
                                <tr><td><strong>Marge commerciale après CV (HT)</strong></td><td class="val"><strong>61 251</strong></td></tr>
                                <tr><td>Charges Fixes CF (HT)</td><td class="val neg">- 32 910</td></tr>
                                <tr><td><strong>Dont CF évitables (Investissement)</strong></td><td class="val"><strong>17 544</strong></td></tr>
                                <tr class="total-row"><td><strong>Résultat d'exploitation Rex (HT)</strong></td><td class="val pos">29 725</td></tr>
                            </tbody>
                        </table>
                    </div>
                    <div class="graph-section">
                        <h2>Analyse de la Structure</h2>
                        <div class="graph-container">
                            <div class="bar-group"><div class="bar-label"><span>Chiffre d'Affaires HT</span><span>100%</span></div><div class="bar-bg"><div class="bar-fill fill-ca"></div></div></div>
                            <div class="bar-group"><div class="bar-label"><span>Coût d'achat de Marchandises (HT)</span><span>94%</span></div><div class="bar-bg"><div class="bar-fill fill-is" style="width: 94%;"></div></div></div>
                            <div class="bar-group"><div class="bar-label"><span>Marge commerciale (HT)</span><span>3.90%</span></div><div class="bar-bg"><div class="bar-fill fill-cv" style="width: 3.90%;"></div></div></div>
                            <div class="bar-group"><div class="bar-label"><span>Charges fixes CF (HT)</span><span>2.59%</span></div><div class="bar-bg"><div class="bar-fill fill-is" style="width: 2.09%;"></div></div></div>
                            <div class="bar-group"><div class="bar-label"><span>Dont CF évitables (Investissement)</span><span>1.11%</span></div><div class="bar-bg"><div class="bar-fill fill-cv" style="width: 1.11%;"></div></div></div>
                            <div class="bar-group"><div class="bar-label"><span>Résultat d'exploitation Rex (HT)</span><span>1.89%</span></div><div class="bar-bg"><div class="bar-fill fill-net" style="width: 1.89%;"></div></div></div>
                        </div>
                    </div>
                </div>
            </div>

            <div id="content2" class="page-content">
                <div class="header">
                    <div class="brand">SASU CESAR INTERNATIONAL <span>Exercice 6 - Semestre 1</span></div>
                    <div class="date-tag">ESTIMATIONS</div>
                </div>
                <div class="dashboard-grid">
                    <div class="table-section">
                        <h2>Compte de Résultat Simplifié</h2>
                        <table>
                            <thead>
                                <tr><th>Postes</th><th class="val">Montant (€)</th></tr>
                            </thead>
                            <tbody>
                                <tr><td><strong>Chiffre d'Affaires brut (HT)</strong></td><td class="val"><strong>1 089 391,58</strong></td></tr>
                                <tr><td>Frais de vente (HT)</td><td class="val neg">- 23 071,00</td></tr>
                                <tr><td style="padding-left: 20px;">→ Vente de Marchandises (HT)</td><td class="val">1 066 391,43</td></tr>
                                <tr><td>Coût d'Acquisition de marchandises (HT)</td><td class="val neg">- 1 018 896,69</td></tr>
                                <tr><td>Frais d'achats CV (HT)</td><td class="val neg">- 6 765,00</td></tr>
                                <tr><td style="padding-left: 20px;">→ Achat de Marchandises (HT)</td><td class="val neg">- 1 012 131,82</td></tr>
                                <tr><td><strong>Marge commerciale (HT)</strong></td><td class="val"><strong>47 424,00</strong></td></tr>
                                <tr><td>Charges fixes CF (HT)</td><td class="val neg">- 17 159,00</td></tr>               
                                <tr class="total-row"><td><strong>Résultat d'exploitation Rex (HT)</strong></td><td class="val pos">30 265,00</td></tr>
                            </tbody>
                        </table>
                    </div>
                    <div class="graph-section">
                        <h2>Analyse de la Structure</h2>
                        <div class="graph-container">
                            <div class="bar-group"><div class="bar-label"><span>Chiffre d'affaires brut (HT)</span><span>100%</span></div><div class="bar-bg"><div class="bar-fill fill-ca"></div></div></div>
                            <div class="bar-group"><div class="bar-label"><span>Vente de Marchandises (HT)</span><span>97,88%</span></div><div class="bar-bg"><div class="bar-fill fill-achats" style="width: 97.88%;"></div></div></div>
                            <div class="bar-group"><div class="bar-label"><span>Coût d'acquisition de marchandises (HT)</span><span>93,53%</span></div><div class="bar-bg"><div class="bar-fill fill-is" style="width: 93.53%;"></div></div></div>
                            <div class="bar-group"><div class="bar-label"><span>Marge commerciale (HT)</span><span>4,35%</span></div><div class="bar-bg"><div class="bar-fill fill-cv" style="width: 4.35%;"></div></div></div>
                            <div class="bar-group"><div class="bar-label"><span>Frais fixes (HT)</span><span>1,58%</span></div><div class="bar-bg"><div class="bar-fill fill-is" style="width: 1.58%;"></div></div></div>
                            <div class="bar-group"><div class="bar-label"><span>Résultat d'exploitation Rex (HT)</span><span>2,78%</span></div><div class="bar-bg"><div class="bar-fill fill-net" style="width: 2.78%;"></div></div></div>
                        </div>
                    </div>
                </div>
            </div>

            <div id="content3" class="page-content">
                <div class="sci-container">
                    <div class="header">
                        <div class="brand">SCI-THE SHADY GROVE <span>| ANALYSE DE RENDEMENT</span></div>
                        <div class="date-tag">Wealth Management</div>
                    </div>

                    <div class="kpi-grid">
                        <div class="card"><span>Investissement</span><b>121 000 €</b></div>
                        <div class="card" style="border-top-color: #38a169;"><span>Loyer Mensuel (Base)</span><b>808 €</b></div>
                        <div class="card" style="border-top-color: #d69e2e;"><span>Cumul Cash (10 ans)</span><b>106 169 €</b></div>
                        <div class="card" style="border-top-color: #e53e3e;"><span>ROI Global (10 ans)</span><b>87,74 %</b></div>
                    </div>

                    <h2>1. Rentabilité & Cash-Flow Cumulé</h2>
                    <div class="chart-main">
                        <canvas id="chartCumul"></canvas>
                    </div>

                    <h2>2. Progression de la Rentabilité Annuelle (%)</h2>
                    <div class="chart-sub">
                        <canvas id="chartRentaLine"></canvas>
                    </div>

                    <div class="details-section">
                        <div class="sub-card">
                            <h3>Détail du Portefeuille Actuel</h3>
                            <table>
                                <thead>
                                    <tr><th>Bien</th><th>Prix Achat</th><th>Loyer Net</th><th>Renta.</th></tr>
                                </thead>
                                <tbody>
                                    <tr><td>Appartement A</td><td>73 000 €</td><td>550 €</td><td><span class="renta-tag">9,04 %</span></td></tr>
                                    <tr><td>Appartement B</td><td>48 000 €</td><td>258 €</td><td><span class="renta-tag">6,45 %</span></td></tr>
                                </tbody>
                                <tfoot>
                                    <tr class="total-row">
                                        <td>TOTAL SCI</td>
                                        <td>121 000 €</td>
                                        <td>808 €</td>
                                        <td>8,01 %</td>
                                    </tr>
                                </tfoot>
                            </table>
                        </div>
                        <div class="sub-card">
                            <h3>Paramètres de Projection</h3>
                            <ul style="font-size: 0.85rem; color: #4a5568; line-height: 1.6;">
                                <li>Indexation des loyers : <b>2% / an</b></li>
                                <li>Calcul basé sur capital investi (121k€)</li>
                                <li>Pas d'IS car 100% amortissement</li>
                                <li>Scénario dynamique ; Patrimoine net global année 10 (N+9). 323 000€. Données corrigées de l'inflation et exprimées en euros constants (base de l'année 2026).</li>
                            </ul>
                        </div>
                    </div>

                    <div class="scenario-box">
                        <h2 style="margin-top:0; border-left: 4px solid #2f855a; padding-left: 10px;">3. Scénario SCI autonome : Réinvestissement Dynamique</h2>
                        <div style="overflow-x: auto;">
                            <table style="margin-top:15px; min-width: 500px;">
                                <thead>
                                    <tr><th>Période</th><th>Nb Biens</th><th>Loyer Mensuel</th><th>Trésorerie</th><th>Action</th><tr>
                                </thead>
                                <tbody id="scenarioBody"></tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>

            <div id="content4" class="page-content">
                <div class="max-w-5xl w-full bg-white p-8 md:p-14 rounded-[2.5rem] shadow-[0_20px_60px_-15px_rgba(0,0,0,0.03)] border border-slate-100 relative overflow-x-auto mx-auto my-10" style="min-width: 550px;">
                    
                    <div class="text-center mb-10 relative z-20">
                        <p class="text-indigo-600 font-bold text-[10px] uppercase tracking-[0.4em] mb-3">Architecture Juridique</p>
                        <h1 class="text-2xl md:text-3xl font-extrabold text-slate-900 tracking-tight">SAS CESAR GROUP</h1>
                        <h2 class="text-sm text-slate-500 mt-1 font-light">Structure du Groupe</h2>
                        <div class="h-1 w-10 bg-slate-100 mx-auto mt-5 rounded-full"></div>
                    </div>

                    <div class="flex flex-col items-center w-full py-4 select-none">
                        
                        <div class="bg-[#0f172a] text-white p-5 px-8 rounded-2xl w-80 text-center shadow-lg z-20 relative">
                            <h3 class="font-extrabold text-lg tracking-tight uppercase">CESAR GROUP</h3>
                            <p class="text-indigo-400 text-[9px] mt-1 font-bold uppercase tracking-[0.2em] opacity-80">Holding de Tête</p>
                        </div>

                        <div class="w-0.5 h-10 bg-slate-300"></div>

                        <div class="w-full max-w-[46rem] flex flex-col items-center">
                            
                            <div class="w-[calc(100%-18rem)] border-t-2 border-slate-300 relative">
                                <div class="absolute -left-[1px] top-0 w-0.5 h-10 bg-slate-300"></div>
                                <div class="absolute -right-[1px] top-0 w-0.5 h-10 bg-slate-300"></div>
                                
                                <div class="absolute -left-6 -top-3.5 bg-white px-2.5 py-0.5 text-[10px] font-black text-slate-500 border border-slate-200 rounded-full shadow-sm">100 %</div>
                                <div class="absolute -right-8 -top-3.5 bg-white px-2.5 py-0.5 text-[10px] font-black text-slate-500 border border-slate-200 rounded-full shadow-sm">999 / 1000</div>
                            </div>

                            <div class="w-full flex justify-between mt-10 relative flex-wrap gap-8 justify-center">
                                
                                <div class="absolute top-1/2 left-[18rem] right-[18rem] flex items-center justify-center -translate-y-1/2 z-10 hidden md:flex">
                                    <div class="w-full border-t-2 border-dashed border-slate-300"></div>
                                    <div class="absolute bg-white px-3 py-1 text-[10px] font-black text-slate-500 border border-slate-200 rounded-full shadow-sm">1 / 1000</div>
                                </div>

                                <div class="w-72 flex justify-center z-20">
                                    <div class="bg-white border border-slate-200 p-6 rounded-2xl w-full text-center shadow-sm">
                                        <h3 class="font-bold text-slate-800 text-[13px] uppercase tracking-tight">CESAR INTERNATIONAL</h3>
                                        <p class="text-slate-400 text-[8px] mt-1.5 font-bold uppercase tracking-widest">Filiale d'Exploitation</p>
                                    </div>
                                </div>

                                <div class="w-72 flex justify-center z-20">
                                    <div class="bg-emerald-50/90 border border-emerald-200 p-6 rounded-2xl w-full text-center shadow-sm relative">
                                        <div class="absolute -top-3.5 left-1/2 -translate-x-1/2 bg-emerald-600 text-white text-[8px] px-3.5 py-1 rounded-full font-black uppercase tracking-widest shadow-sm whitespace-nowrap">Actif Immobilier</div>
                                        <h3 class="font-extrabold text-emerald-950 uppercase text-xs tracking-wider mt-1.5">THE SHADY GROVE</h3>
                                        <p class="text-emerald-600 text-[10px] font-bold mt-0.5 tracking-wider">SCI</p>
                                    </div>
                                </div>
                            </div>
                        </div>

                    </div>

                    <div class="mt-16 pt-6 border-t border-slate-50 flex justify-between items-center text-[9px] text-slate-400 font-bold uppercase tracking-wider">
                        <span>© 2026 CESAR GROUP</span>
                        <span class="opacity-60">Organigramme mis à jour</span>
                    </div>
                </div>
            </div>

            <div id="content5" class="page-content">
                <div class="max-w-6xl mx-auto space-y-8 bg-slate-50 p-4 md:p-8 rounded-3xl">
                    
                    <!-- Header Section -->
                    <header class="flex flex-col md:flex-row md:items-center justify-between gap-4">
                        <div>
                            <h1 class="text-3xl font-bold text-slate-800 tracking-tight">Tableau de Bord Stratégique</h1>
                            <p class="text-slate-500 mt-1">Analyse comparative des performances : CA vs REX</p>
                        </div>
                        <div class="flex items-center gap-2 bg-emerald-100 text-emerald-700 px-4 py-2 rounded-full font-semibold border border-emerald-200 shadow-sm">
                            <i class="fas fa-bullseye"></i>
                            <span>Croissance REX S1 : +1.8% vs Total EXO5</span>
                        </div>
                    </header>

                    <!-- Stats Cards -->
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
                        <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-200">
                            <div class="flex items-center gap-4 mb-4">
                                <div class="p-3 bg-blue-50 text-blue-600 rounded-xl">
                                    <i class="fas fa-chart-bar fa-lg"></i>
                                </div>
                                <h3 class="font-semibold text-slate-700">CA S1 EXO6</h3>
                            </div>
                            <p class="text-3xl font-black text-slate-900">1 089 391 €</p>
                            <p class="text-sm text-blue-600 mt-2 font-medium">
                                <i class="fas fa-arrow-trend-up"></i> 70% de l'an dernier déjà fait
                            </p>
                        </div>

                        <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-200">
                            <div class="flex items-center gap-4 mb-4">
                                <div class="p-3 bg-emerald-50 text-emerald-600 rounded-xl">
                                    <i class="fas fa-chart-line fa-lg"></i>
                                </div>
                                <h3 class="font-semibold text-slate-700">REX S1 EXO6</h3>
                            </div>
                            <p class="text-3xl font-black text-slate-900">30 265 €</p>
                            <p class="text-sm text-emerald-600 mt-2 font-medium">
                                <i class="fas fa-award"></i> Record historique en 6 mois
                            </p>
                        </div>

                        <div class="bg-white p-6 rounded-2xl shadow-sm border border-slate-200 flex flex-col justify-center">
                            <div class="relative pt-1">
                                <div class="flex mb-2 items-center justify-between">
                                    <span class="text-xs font-semibold inline-block py-1 px-2 uppercase rounded-full text-blue-600 bg-blue-100">
                                        Avancement CA
                                    </span>
                                    <span class="text-xs font-bold inline-block text-blue-600">70%</span>
                                </div>
                                <div class="overflow-hidden h-3 mb-4 text-xs flex rounded bg-slate-100">
                                    <div style="width: 70%" class="shadow-none flex flex-col text-center whitespace-nowrap text-white justify-center bg-blue-500"></div>
                                </div>
                                <p class="text-[10px] text-slate-400">Progression vs Volume Total EXO5</p>
                            </div>
                        </div>
                    </div>

                    <!-- Main Chart Section -->
                    <div class="bg-white p-8 rounded-3xl shadow-lg border border-slate-200">
                        <div class="flex flex-col md:flex-row justify-between items-start md:items-center mb-8 gap-4">
                            <h2 class="text-xl font-bold text-slate-800 flex items-center gap-2">
                                <i class="fas fa-chart-pie text-blue-500"></i>
                                Évolution du CA et du REX (Double Axe)
                            </h2>
                            <div class="flex gap-4 text-xs font-medium uppercase tracking-wider">
                                <span class="flex items-center gap-1.5 text-slate-400"><span class="w-3 h-3 bg-slate-200 rounded-sm"></span> Historique</span>
                                <span class="flex items-center gap-1.5 text-blue-600"><span class="w-3 h-3 bg-blue-500 rounded-sm"></span> Semestre</span>
                                <span class="flex items-center gap-1.5 text-emerald-600"><span class="w-3 h-3 bg-emerald-500 rounded-full"></span> REX</span>
                            </div>
                        </div>
                        
                        <div class="h-[480px] w-full relative">
                            <canvas id="financeChart"></canvas>
                        </div>
                        
                        <div class="mt-8 grid grid-cols-1 md:grid-cols-2 gap-4">
                            <div class="p-4 bg-emerald-50 rounded-2xl border border-emerald-100">
                                <h4 class="font-bold text-emerald-800 mb-2 flex items-center gap-2">
                                    <i class="fas fa-arrow-trend-up"></i>
                                    Analyse de la rentabilité
                                </h4>
                                <p class="text-sm text-emerald-700 leading-relaxed">
                                    Le REX du <strong>S1 EXO6 (30 265 €)</strong> dépasse déjà la performance annuelle de l'EXO5. Cela indique une structure de coûts optimisée et une marge opérationnelle en progression.
                                </p>
                            </div>
                            <div class="p-4 bg-blue-50 rounded-2xl border border-blue-100">
                                <h4 class="font-bold text-blue-800 mb-2 flex items-center gap-2">
                                    <i class="fas fa-chart-bar"></i>
                                    Analyse du Chiffre d'Affaires
                                </h4>
                                <p class="text-sm text-blue-700 leading-relaxed">
                                    Avec <strong>1,09 M€</strong> générés en 6 mois, l'entreprise change d'échelle. La projection à <strong>2,17 M€</strong> placerait l'exercice en croissance de +39% par rapport à l'an dernier.
                                </p>
                            </div>
                        </div>
                    </div>

                    <footer class="text-center text-slate-400 text-[10px] pb-8 uppercase tracking-widest">
                        Document d'Analyse Financière Provisoire • Basé sur Clôture S1 EXO6
                    </footer>
                </div>
            </div>

        </div>
    </div>

    <div class="footer">
        Document strictement confidentiel - SASU CESAR INTERNATIONAL Relations Institutions financières &copy; 2026
    </div>

    <script>
        // Gestion de l'écran de verrouillage
        function checkPassword() {
            const pass = document.getElementById('password').value;
            const error = document.getElementById('error-msg');
            if (pass === "CESAR66") {
                document.body.classList.add('unlocked');
                setTimeout(() => {
                    document.getElementById('lock-screen').style.display = 'none';
                    initCharts();
                    initFinanceChart();
                }, 500);
            } else {
                error.style.display = 'block';
                document.getElementById('password').value = '';
            }
        }

        function handleKeyPress(e) {
            if (e.key === "Enter") checkPassword();
        }

        // Initialisation des graphiques SCI
        function initCharts() {
            const totalInvesti = 121000;
            let loyerAnnuelStd = 9696;
            let cumulStd = 0;
            const labels = [];
            const dataRentaStd = [];
            const dataRoiStd = [];
            const dataCumulStd = [];

            let cashScenario = 0;
            let loyerMensuelScen = 808;
            let nbBiens = 2;
            const tableBody = document.getElementById('scenarioBody');
            if (tableBody) {
                tableBody.innerHTML = "";

                for (let i = 1; i <= 10; i++) {
                    labels.push("An " + i);
                    let rentaAn = (loyerAnnuelStd / totalInvesti) * 100;
                    cumulStd += loyerAnnuelStd;
                    dataRentaStd.push(rentaAn.toFixed(2));
                    dataRoiStd.push(((cumulStd / totalInvesti) * 100).toFixed(2));
                    dataCumulStd.push(Math.round(cumulStd));
                    loyerAnnuelStd *= 1.02;

                    cashScenario += (loyerMensuelScen * 12);
                    let event = "-";
                    if (cashScenario >= 50000) {
                        nbBiens++;
                        cashScenario -= 50000;
                        loyerMensuelScen += 258;
                        event = `<span class="buy-event">ACHAT BIEN ${nbBiens}</span>`;
                    }
                    tableBody.innerHTML += `<tr><td>Année ${i}</td><td><b>${nbBiens}</b></td><td>${Math.round(loyerMensuelScen).toLocaleString()} €</td><td>${Math.round(cashScenario).toLocaleString()} €</td><td>${event}</td></tr>`;
                    loyerMensuelScen *= 1.02;
                }
            }

            const chartCumulElement = document.getElementById('chartCumul');
            if (chartCumulElement) {
                new Chart(chartCumulElement, {
                    type: 'line',
                    data: {
                        labels: labels,
                        datasets: [
                            { label: 'ROI Cumulé (%)', data: dataRoiStd, borderColor: '#e53e3e', backgroundColor: 'rgba(229, 62, 62, 0.1)', fill: true, yAxisID: 'y' },
                            { label: 'Cash-flow Cumulé (€)', data: dataCumulStd, borderColor: '#d69e2e', backgroundColor: 'transparent', yAxisID: 'y1' }
                        ]
                    },
                    options: { 
                        responsive: true, 
                        maintainAspectRatio: false, 
                        scales: { 
                            y: { type: 'linear', position: 'left', title: { display: true, text: 'ROI (%)' } }, 
                            y1: { type: 'linear', position: 'right', grid: { drawOnChartArea: false }, title: { display: true, text: 'Cash-flow (€)' } } 
                        } 
                    }
                });
            }

            const chartRentaElement = document.getElementById('chartRentaLine');
            if (chartRentaElement) {
                new Chart(chartRentaElement, {
                    type: 'line',
                    data: {
                        labels: labels,
                        datasets: [{ label: 'Rentabilité annuelle (%)', data: dataRentaStd, borderColor: '#3182ce', borderWidth: 3, tension: 0.4, fill: false }]
                    },
                    options: { responsive: true, maintainAspectRatio: false, scales: { y: { min: 7, max: 11, title: { display: true, text: 'Rentabilité (%)' } } } }
                });
            }
        }

        // Initialisation du graphique financier du tableau de bord stratégique
        function initFinanceChart() {
            const ctx = document.getElementById('financeChart');
            if (!ctx) return;
            
            const labels = ['EXO1', 'EXO2', 'EXO3', 'EXO4', 'EXO5', 'EXO6 (S1)', 'EXO6 (Proj.)'];
            const caData = [139960, 278530, 458530, 988057, 1568156, 1089391, 2178782];
            const rexData = [15400, 21874, 48500, 19876, 29725, 30265, 60530];

            new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: labels,
                    datasets: [
                        {
                            label: 'Chiffre d\'Affaires',
                            data: caData,
                            yAxisID: 'yCA',
                            backgroundColor: (context) => {
                                const index = context.dataIndex;
                                if (index === 5) return '#3b82f6'; // S1
                                if (index === 6) return '#bfdbfe'; // Proj
                                return '#e2e8f0'; // Past
                            },
                            borderRadius: 8,
                            order: 2
                        },
                        {
                            label: 'Résultat d\'Exploitation',
                            data: rexData,
                            yAxisID: 'yREX',
                            type: 'line',
                            borderColor: '#059669',
                            borderWidth: 4,
                            pointBackgroundColor: '#fff',
                            pointBorderColor: '#059669',
                            pointBorderWidth: 2,
                            pointRadius: 6,
                            pointHoverRadius: 8,
                            tension: 0.4,
                            order: 1
                        }
                    ]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: { display: false },
                        tooltip: {
                            backgroundColor: '#fff',
                            titleColor: '#1e293b',
                            bodyColor: '#475569',
                            borderColor: '#e2e8f0',
                            borderWidth: 1,
                            padding: 12,
                            displayColors: true,
                            callbacks: {
                                label: function(context) {
                                    let label = context.dataset.label || '';
                                    if (label) label += ': ';
                                    if (context.parsed.y !== null) {
                                        label += new Intl.NumberFormat('fr-FR', { style: 'currency', currency: 'EUR', maximumFractionDigits: 0 }).format(context.parsed.y);
                                    }
                                    return label;
                                }
                            }
                        }
                    },
                    scales: {
                        x: {
                            grid: { display: false },
                            ticks: { color: '#64748b', font: { size: 11, weight: '500' } }
                        },
                        yCA: {
                            type: 'linear',
                            position: 'left',
                            grid: { color: '#f1f5f9' },
                            ticks: { 
                                color: '#64748b',
                                callback: (value) => (value / 1000) + 'k€'
                            }
                        },
                        yREX: {
                            type: 'linear',
                            position: 'right',
                            grid: { display: false },
                            ticks: { 
                                color: '#059669',
                                font: { weight: 'bold' },
                                callback: (value) => (value / 1000) + 'k€'
                            }
                        }
                    }
                }
            });
        }
    </script>
</body>
</html>
