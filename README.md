
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FuelTrack Pro | Kalkulator BBM Genset</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        :root {
            --bg: #f0f2f5;
            --card: #ffffff;
            --text: #2d3436;
            --primary: #1a73e8;
            --secondary: #00cec9;
            --accent: #6c5ce7;
            --input-bg: #f8f9fa;
            --warning: #ffbe76;
            --snl-blue: #21519b;
            --snl-red: #e31e24;
        }

        .dark-mode {
            --bg: #121212;
            --card: #1e1e1e;
            --text: #e0e0e0;
            --primary: #4dadff;
            --input-bg: #2d2d2d;
        }

        body { 
            font-family: 'Segoe UI', Roboto, sans-serif;
            background-color: var(--bg); 
            color: var(--text); 
            display: flex; 
            justify-content: center; 
            padding: 20px; 
            transition: 0.3s; 
            margin: 0;
        }

        .container { 
            background: var(--card);
            padding: 30px; 
            border-radius: 20px; 
            box-shadow: 0 10px 30px rgba(0,0,0,0.1); 
            width: 100%; 
            max-width: 480px;
        }

        .header { 
            display: flex;
            justify-content: space-between; 
            align-items: center; 
            margin-bottom: 25px; 
        }

        .logo-branding {
            text-align: center;
            margin-bottom: 15px;
        }

        .logo-branding img {
            max-width: 150px;
            height: auto;
        }

        h2 { margin: 0; font-size: 1.5rem; font-weight: 700; color: var(--snl-blue); }

        .theme-btn { 
            cursor: pointer;
            background: var(--input-bg); 
            border: none; 
            border-radius: 12px; 
            width: 45px; 
            height: 45px; 
            color: var(--text);
            transition: 0.3s;
        }

        .form-group { margin-bottom: 20px; }
        label { display: block; margin-bottom: 8px; font-weight: 600; font-size: 0.85rem; opacity: 0.8; }
        
        input, select { 
            width: 100%;
            padding: 12px 15px; 
            border: 1px solid rgba(0,0,0,0.1); 
            border-radius: 10px; 
            background: var(--input-bg); 
            color: var(--text); 
            font-size: 1rem;
            box-sizing: border-box;
            transition: 0.3s;
        }

        .specs-chip {
            background: rgba(33, 81, 155, 0.1);
            color: var(--snl-blue);
            padding: 12px;
            border-radius: 10px;
            font-size: 0.85rem;
            margin-bottom: 20px;
            border: 1px dashed var(--snl-blue);
            display: none;
        }

        .btn-group { display: flex; gap: 12px; margin-bottom: 25px; }
        .mode-btn { 
            flex: 1;
            padding: 15px; 
            border: none; 
            border-radius: 12px; 
            cursor: pointer; 
            font-weight: 700; 
            background: var(--input-bg); 
            color: var(--text);
            transition: 0.3s;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 5px;
        }

        .mode-btn.active { background: var(--snl-blue); color: white; }

        .section-card {
            background: var(--input-bg);
            padding: 20px;
            border-radius: 15px;
            margin-bottom: 20px;
        }

        .grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }

        .calculate-btn { 
            width: 100%;
            padding: 16px; 
            border: none; 
            border-radius: 12px; 
            background: linear-gradient(135deg, var(--snl-blue), var(--snl-red)); 
            color: white; 
            font-weight: bold; 
            cursor: pointer;
            transition: 0.3s;
        }

        .result-box { 
            margin-top: 25px;
            padding: 20px; 
            background: linear-gradient(135deg, var(--snl-blue), var(--snl-red)); 
            color: white;
            border-radius: 15px; 
            text-align: center;
            display: none;
        }

        .warning-box {
            background: rgba(255, 190, 118, 0.2);
            border-left: 4px solid var(--warning);
            color: var(--text);
            padding: 10px;
            margin-top: 15px;
            font-size: 0.8rem;
            border-radius: 5px;
            display: none;
            text-align: left;
        }

        .footer {
            text-align: center;
            margin-top: 25px;
            font-size: 0.9rem;
            font-weight: 900;
            letter-spacing: 2px;
            color: var(--snl-blue);
        }
        .footer span { color: var(--snl-red); }

        .hidden { display: none; }
    </style>
</head>
<body>

<div class="container">
   

    <div class="header">
        <h2><i class="fas fa-gas-pump"></i> FuelCalc Pro</h2>
        <button id="themeToggle" class="theme-btn"><i id="themeIcon" class="fas fa-moon"></i></button>
    </div>

    <div class="form-group">
        <label>Unit Genset</label>
        <select id="gensetType" onchange="updateSpecs()">
            <option value="default">Pilih Kapasitas...</option>
            <option value="cummins300">Cummins (300 kVA)</option>
            <option value="previous">Cummins (350 kVA)</option>
            <option value="cummins400">Cummins (400 kVA)</option>
            <option value="cummins500">Cummins (500 kVA)</option>
            <option value="cummins750">Cummins (750 kVA)</option>
        </select>
    </div>

    <div id="gensetSpecs" class="specs-chip"></div>

    <div class="form-group">
        <label>Total Stok BBM (Liter)</label>
        <input type="number" id="totalFuel" placeholder="Contoh: 1000">
    </div>

    <div class="btn-group">
        <button id="plnBtn" class="mode-btn" onclick="toggleSection('pln')">
            <i class="fas fa-plug-circle-xmark"></i>
            <span>PLN OFF</span>
        </button>
        <button id="warmBtn" class="mode-btn" onclick="toggleSection('warm')">
            <i class="fas fa-fire-alt"></i>
            <span>WARMING</span>
        </button>
    </div>

    <div id="plnSection" class="hidden">
        <div class="section-card">
            <label>Load MDP (kW)</label>
            <div class="grid-2">
                <input type="number" id="loadMDP1" placeholder="MDP 1">
                <input type="number" id="loadMDP2" placeholder="MDP 2">
            </div>
            <label style="margin-top:15px">Runtime (Jam : Menit)</label>
            <div class="grid-2" style="margin-bottom:10px">
                <input type="number" id="engineHourBefore" placeholder="Jam Awal">
                <input type="number" id="engineMinBefore" placeholder="Min Awal">
            </div>
            <div class="grid-2">
                <input type="number" id="engineHourAfter" placeholder="Jam Akhir">
                <input type="number" id="engineMinAfter" placeholder="Min Akhir">
            </div>
        </div>
    </div>

    <div id="warmSection" class="hidden">
        <div class="section-card">
            <label>Durasi Pemanasan (Jam : Menit)</label>
            <div class="grid-2" style="margin-bottom:10px">
                <input type="number" id="engineHourBeforeWarm" placeholder="Jam Awal">
                <input type="number" id="engineMinBeforeWarm" placeholder="Min Awal">
            </div>
            <div class="grid-2">
                <input type="number" id="engineHourAfterWarm" placeholder="Jam Akhir">
                <input type="number" id="engineMinAfterWarm" placeholder="Min Akhir">
            </div>
        </div>
    </div>

    <div id="actionButtons" style="display:none">
        <button class="calculate-btn" onclick="calculate()">
            <i class="fas fa-calculator"></i> HITUNG SEKARANG
        </button>
    </div>

    <div id="result" class="result-box"></div>
    <div id="wetStackWarning" class="warning-box">
        <i class="fas fa-exclamation-triangle"></i> <strong>Peringatan Wet Stacking!</strong> Beban di bawah 30% berisiko merusak mesin.
    </div>

    <div class="footer">
        BY SNL <span>ACEH</span>
    </div>
</div>

<script>
    let gensetData = {
        "default": { capacityKVA: 0, capacityKW: 0, table: {} },
        "cummins300": { capacityKVA: 300, capacityKW: 240, table: { 0: 6, 25: 14.3, 50: 28.6, 75: 42.9, 100: 57.3 } },
        "previous": { capacityKVA: 350, capacityKW: 280, table: { 0: 7, 25: 17.6, 50: 35.2, 75: 52.8, 100: 70.5 } },
        "cummins400": { capacityKVA: 400, capacityKW: 320, table: { 0: 8, 25: 18, 50: 36, 75: 54, 100: 72 } },
        "cummins500": { capacityKVA: 500, capacityKW: 400, table: { 0: 10, 25: 22.5, 50: 45, 75: 67.5, 100: 90 } },
        "cummins750": { 
            capacityKVA: 750, 
            capacityKW: 600, 
            table: { 0: 22, 5: 26, 25: 50, 50: 92, 75: 135, 100: 180 } 
        }
    };

    let activeSection = null;

    document.getElementById('themeToggle').addEventListener('click', function() {
        document.body.classList.toggle('dark-mode');
        const isDark = document.body.classList.contains('dark-mode');
        document.getElementById('themeIcon').className = isDark ? 'fas fa-sun' : 'fas fa-moon';
    });

    function updateSpecs() {
        const type = document.getElementById("gensetType").value;
        const specs = gensetData[type];
        const chip = document.getElementById("gensetSpecs");
        if(type !== "default") {
            const fullCons = specs.table[100];
            chip.innerHTML = `<i class="fas fa-info-circle"></i> ${specs.capacityKVA} kVA / ${specs.capacityKW} kW | Full Load: ${fullCons} L/H`;
            chip.style.display = 'block';
        } else { chip.style.display = 'none'; }
    }

    function toggleSection(section) {
        document.getElementById("plnSection").classList.add("hidden");
        document.getElementById("warmSection").classList.add("hidden");
        document.getElementById("plnBtn").classList.remove("active");
        document.getElementById("warmBtn").classList.remove("active");

        if (activeSection !== section) {
            document.getElementById(`${section}Section`).classList.remove("hidden");
            document.getElementById(`${section}Btn`).classList.add("active");
            document.getElementById("actionButtons").style.display = "block";
            activeSection = section;
        } else {
            document.getElementById("actionButtons").style.display = "none";
            activeSection = null;
        }
    }

    function calculate() {
        const type = document.getElementById("gensetType").value;
        const totalFuel = parseFloat(document.getElementById("totalFuel").value) || 0;
        
        if (type === "default" || totalFuel <= 0) {
            alert("Pilih tipe genset dan isi total BBM!");
            return;
        }
        
        const specs = gensetData[type];
        let totalCons = 0;
        let durMinutes = 0;
        let loadPercValue = 0;

        if (activeSection === "pln") {
            const h1 = parseFloat(document.getElementById("engineHourBefore").value) || 0;
            const m1 = parseFloat(document.getElementById("engineMinBefore").value) || 0;
            const h2 = parseFloat(document.getElementById("engineHourAfter").value) || 0;
            const m2 = parseFloat(document.getElementById("engineMinAfter").value) || 0;
            
            durMinutes = (h2 * 60 + m2) - (h1 * 60 + m1);
            let durHours = durMinutes / 60;
            
            const loadInput = (parseFloat(document.getElementById("loadMDP1").value) || 0) + (parseFloat(document.getElementById("loadMDP2").value) || 0);
            loadPercValue = (loadInput / specs.capacityKW) * 100;
            
            // Logika interpolasi konsumsi BBM berdasarkan tabel
            const points = Object.keys(specs.table).map(Number).sort((a,b) => a-b);
            let consPerHour = specs.table[100];
            
            for(let i=0; i < points.length - 1; i++) {
                if(loadPercValue >= points[i] && loadPercValue <= points[i+1]) {
                    const p1 = points[i], p2 = points[i+1];
                    const c1 = specs.table[p1], c2 = specs.table[p2];
                    consPerHour = c1 + (loadPercValue - p1) * (c2 - c1) / (p2 - p1);
                    break;
                }
            }

            totalCons = consPerHour * durHours;
            document.getElementById("wetStackWarning").style.display = (loadPercValue < 30) ? "block" : "none";
            
        } else {
            const h1 = parseFloat(document.getElementById("engineHourBeforeWarm").value) || 0;
            const m1 = parseFloat(document.getElementById("engineMinBeforeWarm").value) || 0;
            const h2 = parseFloat(document.getElementById("engineHourAfterWarm").value) || 0;
            const m2 = parseFloat(document.getElementById("engineMinAfterWarm").value) || 0;
            
            durMinutes = (h2 * 60 + m2) - (h1 * 60 + m1);
            let durHours = durMinutes / 60;
            
            // Mode warming menggunakan konsumsi Idle (0% load)
            totalCons = specs.table[0] * durHours;
            document.getElementById("wetStackWarning").style.display = "none";
        }

        if(durMinutes < 0) {
            alert("Waktu akhir harus lebih besar dari waktu awal!");
            return;
        }

        const resDiv = document.getElementById("result");
        resDiv.style.display = 'block';
        resDiv.innerHTML = `
            <div style="font-size:0.9rem; opacity:0.9">ESTIMASI SISA BBM</div>
            <div style="font-size:2rem; font-weight:800; margin:5px 0">${(totalFuel - totalCons).toFixed(2)} Liter</div>
            <div style="font-size:0.8rem; opacity:0.8; border-top:1px solid rgba(255,255,255,0.2); padding-top:10px; margin-top:10px">
                Beban: ${loadPercValue.toFixed(1)}% | Pemakaian: ${totalCons.toFixed(2)} L | Durasi: ${durMinutes} Menit
            </div>
        `;
    }
</script>
</body>
</html>
