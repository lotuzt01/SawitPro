<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PalmCore ERP - Offline Edition</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700&display=swap');
        body { font-family: 'Plus Jakarta Sans', sans-serif; background-color: #f8fafc; }
        .glass-panel { background: white; border: 1px solid #e2e8f0; }
        .nav-btn.active { background: #059669 !important; color: white !important; box-shadow: 0 10px 15px -3px rgba(5, 150, 105, 0.2); }
        
        .page-content { display: none; animation: fadeIn 0.3s ease-out; }
        .page-content.active { display: block; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        /* Custom Scrollbar */
        ::-webkit-scrollbar { width: 6px; }
        ::-webkit-scrollbar-track { background: #f1f1f1; }
        ::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 10px; }
    </style>
</head>
<body class="text-slate-700">

    <!-- Sidebar -->
    <nav class="fixed left-0 top-0 h-full w-64 bg-white border-r border-slate-200 z-50">
        <div class="p-6">
            <div class="flex items-center gap-3 mb-10">
                <div class="bg-emerald-600 p-2 rounded-xl text-white shadow-lg shadow-emerald-200">
                    <i class="fas fa-leaf text-xl"></i>
                </div>
                <span class="font-bold text-xl text-emerald-900 tracking-tight">PalmCore<span class="text-emerald-500 font-black">ERP</span></span>
            </div>
            
            <div class="space-y-2">
                <button onclick="navTo('dashboard')" id="btn-dashboard" class="nav-btn active w-full flex items-center gap-4 p-4 rounded-2xl text-slate-500 transition-all hover:bg-slate-50">
                    <i class="fas fa-chart-pie w-5"></i><span class="font-bold text-sm">Dashboard</span>
                </button>
                <button onclick="navTo('beli')" id="btn-beli" class="nav-btn w-full flex items-center gap-4 p-4 rounded-2xl text-slate-500 transition-all hover:bg-slate-50">
                    <i class="fas fa-shopping-basket w-5"></i><span class="font-bold text-sm">Beli TBS</span>
                </button>
                <button onclick="navTo('jual')" id="btn-jual" class="nav-btn w-full flex items-center gap-4 p-4 rounded-2xl text-slate-500 transition-all hover:bg-slate-50">
                    <i class="fas fa-truck-moving w-5"></i><span class="font-bold text-sm">Jual (PKS)</span>
                </button>
                <button onclick="navTo('biaya')" id="btn-biaya" class="nav-btn w-full flex items-center gap-4 p-4 rounded-2xl text-slate-500 transition-all hover:bg-slate-50">
                    <i class="fas fa-money-bill-wave w-5"></i><span class="font-bold text-sm">Biaya Ops</span>
                </button>
                <button onclick="navTo('laporan')" id="btn-laporan" class="nav-btn w-full flex items-center gap-4 p-4 rounded-2xl text-slate-500 transition-all hover:bg-slate-50">
                    <i class="fas fa-file-invoice-dollar w-5"></i><span class="font-bold text-sm">Laporan</span>
                </button>
            </div>

            <div class="absolute bottom-8 left-6 right-6">
                <button onclick="clearData()" class="w-full flex items-center gap-3 p-3 text-rose-500 hover:bg-rose-50 rounded-xl transition-colors font-bold text-xs">
                    <i class="fas fa-sync-alt"></i> Reset Database
                </button>
            </div>
        </div>
    </nav>

    <!-- Main Content -->
    <main class="ml-64 p-8 min-h-screen">
        
        <!-- Dashboard Page -->
        <div id="page-dashboard" class="page-content active">
            <header class="mb-8">
                <h1 class="text-3xl font-black text-slate-800">Ringkasan Operasional</h1>
                <p class="text-slate-500">Pantau arus kas dan stok secara real-time.</p>
            </header>

            <div class="grid grid-cols-4 gap-6 mb-8">
                <div class="glass-panel p-6 rounded-3xl border-b-4 border-emerald-500 shadow-sm">
                    <p class="text-[10px] font-black text-slate-400 uppercase tracking-widest mb-1">Stok TBS di Gudang</p>
                    <h2 class="text-3xl font-black text-slate-800"><span id="stok-val">0</span> <span class="text-sm font-normal text-slate-400">Kg</span></h2>
                </div>
                <div class="glass-panel p-6 rounded-3xl border-b-4 border-blue-500 shadow-sm">
                    <p class="text-[10px] font-black text-slate-400 uppercase tracking-widest mb-1">Kas Tersedia</p>
                    <h2 id="kas-val" class="text-2xl font-black text-blue-600">Rp 0</h2>
                </div>
                <div class="glass-panel p-6 rounded-3xl border-b-4 border-amber-500 shadow-sm">
                    <p class="text-[10px] font-black text-slate-400 uppercase tracking-widest mb-1">Total Penjualan</p>
                    <h2 id="jual-val" class="text-2xl font-black text-amber-600">Rp 0</h2>
                </div>
                <div class="bg-slate-900 p-6 rounded-3xl text-white shadow-xl">
                    <p class="text-[10px] font-bold text-emerald-400 uppercase tracking-widest mb-1">Estimasi Laba</p>
                    <h2 id="profit-val" class="text-2xl font-black text-emerald-400">Rp 0</h2>
                </div>
            </div>

            <div class="glass-panel p-8 rounded-3xl h-[400px] shadow-sm">
                <h3 class="font-bold text-slate-800 mb-4">Grafik Volume Pembelian (Kg)</h3>
                <canvas id="chartView"></canvas>
            </div>
        </div>

        <!-- Form Beli -->
        <div id="page-beli" class="page-content">
            <div class="max-w-4xl glass-panel p-8 rounded-[2rem] shadow-xl border-t-8 border-emerald-500">
                <h3 class="text-2xl font-black text-emerald-900 mb-6">Transaksi Beli TBS</h3>
                <div class="grid grid-cols-2 gap-8">
                    <div class="space-y-4">
                        <input id="b-nama" type="text" placeholder="Nama Pemasok/Petani" class="w-full p-4 bg-slate-50 border rounded-2xl font-bold outline-emerald-500">
                        <div class="grid grid-cols-2 gap-4">
                            <input id="b-bruto" type="number" oninput="calcBeli()" placeholder="Brutto (Kg)" class="w-full p-4 bg-slate-50 border rounded-2xl font-bold">
                            <input id="b-tara" type="number" oninput="calcBeli()" placeholder="Tarra (Kg)" class="w-full p-4 bg-slate-50 border rounded-2xl font-bold">
                        </div>
                        <div class="grid grid-cols-2 gap-4">
                            <input id="b-persen" type="number" oninput="calcBeli()" value="3" class="w-full p-4 bg-rose-50 border-2 border-rose-100 rounded-2xl font-bold text-rose-700">
                            <input id="b-harga" type="number" oninput="calcBeli()" placeholder="Harga Rp/Kg" class="w-full p-4 bg-emerald-50 border-2 border-emerald-100 rounded-2xl font-bold text-emerald-700">
                        </div>
                    </div>
                    <div class="bg-emerald-900 rounded-3xl p-8 text-white flex flex-col justify-center text-center shadow-inner">
                        <p class="text-xs font-bold text-emerald-400 mb-1">TOTAL PEMBAYARAN</p>
                        <h2 id="res-total" class="text-4xl font-black mb-4">Rp 0</h2>
                        <p class="text-xs opacity-60">Netto: <span id="res-netto">0</span> Kg</p>
                        <button onclick="simpan('BELI')" class="mt-8 py-5 bg-emerald-500 hover:bg-emerald-400 rounded-2xl font-black transition-all">POSTING TRANSAKSI</button>
                    </div>
                </div>
            </div>
        </div>

        <!-- Halaman Lainnya (Jual, Biaya, Laporan) akan menyusul di logika JS -->
        <div id="page-jual" class="page-content">
            <div class="max-w-2xl glass-panel p-8 rounded-[2rem] shadow-xl border-t-8 border-blue-500">
                <h3 class="text-2xl font-black text-blue-900 mb-6">Penjualan ke PKS</h3>
                <div class="space-y-4">
                    <input id="j-pabrik" type="text" placeholder="Nama Pabrik" class="w-full p-4 bg-slate-50 border rounded-2xl font-bold">
                    <input id="j-netto" type="number" placeholder="Netto Pabrik (Kg)" class="w-full p-4 bg-slate-50 border rounded-2xl font-bold">
                    <input id="j-harga" type="number" placeholder="Harga Jual Rp/Kg" class="w-full p-4 bg-blue-50 border rounded-2xl font-bold">
                    <button onclick="simpan('JUAL')" class="w-full py-5 bg-blue-600 text-white rounded-2xl font-black text-xl shadow-lg">SIMPAN PENJUALAN</button>
                </div>
            </div>
        </div>

        <div id="page-biaya" class="page-content">
            <div class="max-w-xl glass-panel p-8 rounded-[2rem] shadow-xl border-t-8 border-rose-500">
                <h3 class="text-2xl font-black text-rose-900 mb-6">Biaya Operasional</h3>
                <div class="space-y-4">
                    <input id="c-ket" type="text" placeholder="Keterangan (Gaji, Solar, dll)" class="w-full p-4 bg-slate-50 border rounded-2xl font-bold">
                    <input id="c-nominal" type="number" placeholder="Nominal Rp" class="w-full p-4 bg-rose-50 border rounded-2xl font-bold">
                    <button onclick="simpan('BIAYA')" class="w-full py-5 bg-rose-600 text-white rounded-2xl font-black text-xl shadow-lg">SIMPAN BIAYA</button>
                </div>
            </div>
        </div>

        <div id="page-laporan" class="page-content">
            <div class="flex justify-between items-end mb-6">
                <div>
                    <h3 class="text-2xl font-black text-slate-800">Jurnal Transaksi</h3>
                    <p class="text-slate-400 text-sm">Data tersimpan di penyimpanan lokal browser.</p>
                </div>
                <button onclick="exportExcel()" class="bg-emerald-600 text-white px-6 py-3 rounded-xl font-bold hover:bg-emerald-700 transition-all flex items-center gap-2">
                    <i class="fas fa-file-excel"></i> Export Excel
                </button>
            </div>
            <div class="glass-panel rounded-[2rem] overflow-hidden shadow-sm">
                <table class="w-full text-left">
                    <thead class="bg-slate-50 border-b border-slate-100">
                        <tr>
                            <th class="p-6 text-xs font-black text-slate-400 uppercase">Waktu</th>
                            <th class="p-6 text-xs font-black text-slate-400 uppercase">Tipe</th>
                            <th class="p-6 text-xs font-black text-slate-400 uppercase">Keterangan</th>
                            <th class="p-6 text-xs font-black text-slate-400 uppercase text-right">Nilai (Rp)</th>
                        </tr>
                    </thead>
                    <tbody id="table-log"></tbody>
                </table>
            </div>
        </div>

    </main>

    <script>
        let dataStore = JSON.parse(localStorage.getItem('palmcore_db')) || [];
        let myChart = null;

        function navTo(id) {
            document.querySelectorAll('.page-content').forEach(p => p.classList.remove('active'));
            document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
            document.getElementById('page-' + id).classList.add('active');
            document.getElementById('btn-' + id).classList.add('active');
            if(id === 'dashboard') renderDashboard();
        }

        function calcBeli() {
            const b = parseFloat(document.getElementById('b-bruto').value) || 0;
            const t = parseFloat(document.getElementById('b-tara').value) || 0;
            const p = parseFloat(document.getElementById('b-persen').value) || 0;
            const h = parseFloat(document.getElementById('b-harga').value) || 0;
            const netto = (b - t) * (1 - p/100);
            const total = netto * h;
            document.getElementById('res-netto').innerText = Math.round(netto).toLocaleString();
            document.getElementById('res-total').innerText = 'Rp ' + Math.round(total).toLocaleString();
        }

        function simpan(tipe) {
            let item = { id: Date.now(), ts: new Date().toLocaleString(), tipe };
            if(tipe === 'BELI') {
                const b = parseFloat(document.getElementById('b-bruto').value) || 0;
                const t = parseFloat(document.getElementById('b-tara').value) || 0;
                const p = parseFloat(document.getElementById('b-persen').value) || 0;
                const h = parseFloat(document.getElementById('b-harga').value) || 0;
                item.netto = (b - t) * (1 - p/100);
                item.total = item.netto * h;
                item.nama = document.getElementById('b-nama').value || 'Petani';
            } else if(tipe === 'JUAL') {
                item.netto = parseFloat(document.getElementById('j-netto').value) || 0;
                item.harga = parseFloat(document.getElementById('j-harga').value) || 0;
                item.total = item.netto * item.harga;
                item.nama = document.getElementById('j-pabrik').value || 'PKS';
            } else if(tipe === 'BIAYA') {
                item.total = parseFloat(document.getElementById('c-nominal').value) || 0;
                item.nama = document.getElementById('c-ket').value || 'Biaya Umum';
            }

            dataStore.unshift(item);
            localStorage.setItem('palmcore_db', JSON.stringify(dataStore));
            alert('Data Berhasil Disimpan!');
            navTo('dashboard');
        }

        function renderDashboard() {
            let stok = 0, beli = 0, jual = 0, biaya = 0;
            const table = document.getElementById('table-log');
            table.innerHTML = '';

            dataStore.forEach(d => {
                if(d.tipe === 'BELI') { stok += d.netto; beli += d.total; }
                if(d.tipe === 'JUAL') { stok -= d.netto; jual += d.total; }
                if(d.tipe === 'BIAYA') { biaya += d.total; }

                table.insertAdjacentHTML('beforeend', `
                    <tr class="border-b border-slate-50 hover:bg-slate-50 transition-colors">
                        <td class="p-6 text-xs text-slate-400 font-medium">${d.ts}</td>
                        <td class="p-6"><span class="px-3 py-1 rounded-full text-[10px] font-black uppercase ${d.tipe==='BELI'?'bg-emerald-100 text-emerald-700':d.tipe==='JUAL'?'bg-blue-100 text-blue-700':'bg-rose-100 text-rose-700'}">${d.tipe}</span></td>
                        <td class="p-6 font-bold text-slate-700">${d.nama}</td>
                        <td class="p-6 text-right font-black text-slate-900">Rp ${Math.round(d.total).toLocaleString()}</td>
                    </tr>
                `);
            });

            document.getElementById('stok-val').innerText = Math.round(stok).toLocaleString();
            document.getElementById('kas-val').innerText = 'Rp ' + Math.round(jual - beli - biaya).toLocaleString();
            document.getElementById('jual-val').innerText = 'Rp ' + Math.round(jual).toLocaleString();
            document.getElementById('profit-val').innerText = 'Rp ' + Math.round(jual - beli - biaya).toLocaleString();
            
            updateChart();
        }

        function updateChart() {
            const ctx = document.getElementById('chartView').getContext('2d');
            const samples = dataStore.filter(d => d.tipe === 'BELI').slice(0, 7).reverse();
            if(myChart) myChart.destroy();
            myChart = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: samples.map(s => s.ts.split(',')[0]),
                    datasets: [{
                        label: 'Volume Masuk (Kg)',
                        data: samples.map(s => s.netto),
                        borderColor: '#10b981',
                        backgroundColor: 'rgba(16, 185, 129, 0.1)',
                        fill: true,
                        tension: 0.4
                    }]
                },
                options: { maintainAspectRatio: false }
            });
        }

        function clearData() {
            if(confirm('Hapus semua data permanen?')) {
                localStorage.removeItem('palmcore_db');
                dataStore = [];
                renderDashboard();
            }
        }

        function exportExcel() {
            const ws = XLSX.utils.json_to_sheet(dataStore);
            const wb = XLSX.utils.book_new();
            XLSX.utils.book_append_sheet(wb, ws, "Laporan");
            XLSX.writeFile(wb, "PalmCore_Report.xlsx");
        }

        window.onload = renderDashboard;
    </script>
</body>
</html>

