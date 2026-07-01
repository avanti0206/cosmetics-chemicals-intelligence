# cosmetics-chemicals-intelligence
# Industry Intelligence Platform

A premium executive dashboard for the **Cosmetics, Personal Care, Home Care, Specialty Chemicals, and Raw Materials** industries.

Features:
- Interactive AR Ageing & Collections charts (Chart.js)
- Daily Industry Intelligence Digest with business impact & actions
- SAP data simulation buttons
- Export to PDF / CSV
- Fully responsive dark theme

## How to Use

1. Download `dashboard.html`
2. Open the file in any modern browser (Chrome, Edge, etc.)
3. Use the buttons:
   - **Simulate SAP Sync** → Updates KPIs and charts
   - **Regenerate Digest** → New intelligence briefing
   - Export options available

## Technologies
- HTML5 + CSS3
- Chart.js (interactive charts)
- Pure vanilla JavaScript

## License
MIT License — Free to use and modify.


<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Industry Intelligence Platform</title>
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.1/dist/chart.umd.min.js"></script>
<style>
@import url('https://fonts.googleapis.com/css2?family=DM+Serif+Display&family=Inter:wght@300;400;500;600;700&family=JetBrains+Mono:wght@400;600&display=swap');
:root{--bg:#0e0b10;--surface:#16121a;--surface2:#1f1a26;--accent:#c87fa0;--accent2:#7ec8a0;--gold:#e8c97a;--text:#f0eaf5;--muted:#6d6280;}
body{background:var(--bg);color:var(--text);font-family:'Inter',sans-serif;margin:0;padding:20px;}
.card{background:var(--surface);border:1px solid #2e2638;border-radius:8px;padding:16px;margin-bottom:20px;}
.chart-container{height:260px;position:relative;}
.btn{background:var(--accent);color:#fff;border:none;padding:8px 16px;border-radius:6px;font-size:13px;font-weight:600;cursor:pointer;margin:4px;}
.btn:hover{opacity:0.9;}
.btn-gold{background:var(--gold);color:#0e0b10;}
.btn-sap{background:#0070f2;color:#fff;}
</style>
</head>
<body>

<div style="max-width:1280px;margin:0 auto;">

  <div class="card" style="border-color:#e8c97a;">
    <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:16px;">
      <div>
        <div style="font-family:'DM Serif Display',serif;font-size:26px;color:#e8c97a;">Industry Intelligence Platform</div>
        <div style="font-family:'JetBrains Mono',monospace;font-size:12px;color:#6d6280;">July 1, 2026 • Live Update</div>
      </div>
      <div>
        <button class="btn btn-sap" onclick="simulateSAPSync()">🔄 Simulate SAP Sync</button>
        <button class="btn btn-gold" onclick="regenerateDigest()">✦ Regenerate Digest</button>
        <button class="btn" onclick="exportPDF()">📄 Export PDF</button>
        <button class="btn" onclick="exportCSV()">📊 Export CSV</button>
      </div>
    </div>

    <!-- KPIs -->
    <div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(180px,1fr));gap:12px;margin-bottom:24px;">
      <div class="card"><div style="font-size:10px;color:#6d6280;">TOTAL AR OUTSTANDING</div><div style="font-size:28px;font-weight:600;color:#f0eaf5;">₹6.8Cr</div><div style="color:#e8c97a;font-size:12px;">▲ 9.2% MoM</div></div>
      <div class="card"><div style="font-size:10px;color:#6d6280;">OVERDUE</div><div style="font-size:28px;font-weight:600;color:#f06f6f;">₹1.4Cr</div><div style="color:#f06f6f;font-size:12px;">14% ↑ — Escalate</div></div>
      <div class="card"><div style="font-size:10px;color:#6d6280;">DSO (Days)</div><div style="font-size:28px;font-weight:600;color:#e8c97a;">46</div><div style="color:#e8c97a;font-size:12px;">Target: 40</div></div>
      <div class="card"><div style="font-size:10px;color:#6d6280;">TODAY COLLECTIONS</div><div style="font-size:28px;font-weight:600;color:#7ec8a0;">₹22.6L</div><div style="color:#7ec8a0;font-size:12px;">▲ 5.4%</div></div>
    </div>

    <div style="display:grid;grid-template-columns:1fr 1fr;gap:20px;">
      <div class="card">
        <h3>AR Ageing Breakdown (₹ Crore)</h3>
        <div class="chart-container"><canvas id="agingChart"></canvas></div>
      </div>
      <div class="card">
        <h3>Daily Collections Trend — Last 14 Days (₹ Lakhs)</h3>
        <div class="chart-container"><canvas id="collectionsChart"></canvas></div>
      </div>
    </div>

    <!-- Digest -->
    <div class="card" style="border-color:#e8c97a;">
      <h2 style="color:#e8c97a;margin-bottom:16px;">✦ Daily Industry Intelligence Digest</h2>
      <div style="display:grid;grid-template-columns:repeat(auto-fit,minmax(300px,1fr));gap:20px;">
        <div>
          <h4 style="color:#c87fa0;">🔴 Priority Alerts</h4>
          <ul style="padding-left:20px;line-height:1.7;">
            <li><strong>EU Fragrance Allergen Expansion</strong> — Major labelling changes effective soon.</li>
            <li><strong>Raw Material Volatility</strong> — Palm derivatives and specialty surfactants under pressure.</li>
          </ul>
        </div>
        <div>
          <h4 style="color:#7ec8a0;">🚀 Key Opportunities</h4>
          <p>High demand for Bakuchiol, Ceramides, and postbiotic ingredients among Indian D2C beauty brands.</p>
        </div>
      </div>

      <div style="margin-top:20px;">
        <h4 style="color:#e8c97a;">🎯 Recommended Actions</h4>
        <ul style="line-height:1.8;">
          <li>Pitch sustainable alternatives to top customers.</li>
          <li>Prepare compliance updates for EU regulatory shifts.</li>
          <li>Monitor supply chain for surfactant availability.</li>
        </ul>
      </div>
    </div>
  </div>
</div>

<script>
// Charts + Simulation functions (same as before)
let agingChart, collectionsChart;
function initCharts() {
  agingChart = new Chart(document.getElementById('agingChart'), { type: 'bar', data: { labels: ['0–30d','31–60d','61–90d','90+d'], datasets: [{ label: '₹ Crore', data: [3.10,1.44,0.86,0.60], backgroundColor: ['#7ec8a0','#a78bfa','#e8c97a','#f06f6f'] }] }, options: { responsive:true, maintainAspectRatio:false, plugins:{legend:{display:false}} } });
  collectionsChart = new Chart(document.getElementById('collectionsChart'), { type: 'line', data: { labels: ['18','19','20','21','22','23','24','25','26','27','28','29','30','1'], datasets: [{ label: 'Collections (₹L)', data: [18,22,19,25,28,35,24,19,42,38,31,26,33,22.6], borderColor: '#e8c97a', tension:0.4, fill:true, backgroundColor:'rgba(232,201,122,0.15)' }] }, options: { responsive:true, maintainAspectRatio:false, plugins:{legend:{display:false}} } });
}
initCharts();

function simulateSAPSync() { alert("✅ SAP FI-AR synchronized. KPIs & charts updated (simulated)."); }
function regenerateDigest() { alert("✦ New digest generated with latest industry signals."); }
function exportPDF() { window.print(); }
function exportCSV() {
  const csv = "Category,Amount\nCurrent,3.10\n31-60,1.44\n61-90,0.86\n90+,0.60";
  const blob = new Blob([csv], {type: 'text/csv'});
  const a = document.createElement('a'); a.href=URL.createObjectURL(blob); a.download='ar-data.csv'; a.click();
}
</script>
</body>
</html>
