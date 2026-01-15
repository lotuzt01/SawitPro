<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ADHIGROUPSAWINDO (AGS) - Offline Version</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap');
        body { font-family: 'Plus Jakarta Sans', sans-serif; background-color: #f8fafc; }
        .card { @apply bg-white p-6 rounded-3xl border border-slate-200 shadow-sm transition-all duration-300 hover:shadow-md; }
        .nav-btn { @apply flex items-center gap-3 px-5 py-4 rounded-2xl transition-all duration-200 font-bold text-sm w-full; }
        .nav-active { @apply bg-green-800 text-white shadow-lg scale-[1.02]; }
        .nav-inactive { @apply text-slate-500 hover:bg-slate-100; }
        .input-field { @apply w-full border border-slate-200 rounded-2xl px-4 py-3 focus:ring-2 focus:ring-green-500 outline-none text-sm bg-white shadow-sm transition-all; }
        .btn-action { @apply flex items-center justify-center gap-2 px-6 py-4 rounded-2xl font-black text-sm shadow-lg transition-all active:scale-95 disabled:opacity-50; }
        .form-label { @apply block text-[10px] font-black text-slate-400 uppercase mb-1.5 ml-1 tracking-wider; }
        .animate-fade-in { animation: fadeIn 0.4s ease-out; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body class="min-h-screen text-slate-800">

    <!-- LOGIN SCREEN -->
    <div id="login-screen" class="fixed inset-0 bg-[#064e3b] z-[500] flex items-center justify-center p-6">
        <div class="bg-white w-full max-w-md rounded-[3rem] p-10 shadow-2xl text-center">
            <div class="w-20 h-20 bg-green-700 rounded-[2rem] mx-auto flex items-center justify-center text-white font-black text-3xl mb-8 shadow-2xl">AGS</div>
            <h1 class="text-3xl font-black text-slate-900 tracking-tight">Enterprise Access</h1>
            <p class="text-slate-400 text-sm mb-10 font-medium">PIN: 1234 (Versi Offline)</p>
            <div class="space-y-6">
                <input type="password" id="login-pin" class="input-field text-center text-3xl tracking-[0.8em] font-black" placeholder="••••" maxlength="4">
                <button onclick="handleLogin()" class="w-full bg-green-700 text-white py-5 rounded-[1.5rem] font-black text-lg shadow-xl hover:bg-green-800 transition-all">MASUK</button>
            </div>
        </div>
    </div>

    <!-- APP CONTENT -->
    <div id="app-content" class="hidden flex flex-col md:flex-row min-h-screen">
        <aside class="w-full md:w-72 bg-white border-r border-slate-100 p-8 flex flex-col h-auto md:h-screen sticky top-0">
            <div class="flex items-center gap-4 mb-12">
                <div class="w-12 h-12 bg-green-800 rounded-2xl flex items-center justify-center text-white font-black text-2xl shadow-lg">A</div>
                <div>
                    <h1 class="text-lg font-black text-slate-900 leading-none">ADHIGROUP</h1>
                    <h1 class="text-sm font-bold text-green-600">SAWINDO</h1>
                </div>
            </div>
            
            <nav class="space-y-2 flex-grow">
                <button onclick="changeTab('dashboard')" id="tab-dashboard" class="nav-btn nav-active">📊 Dashboard</button>
                <button onclick="changeTab('beli')" id="tab-beli" class="nav-btn nav-inactive">🚜 Beli TBS</button>
                <button onclick="changeTab('jual')" id="tab-jual" class="nav-btn nav-inactive">🏭 Jual Pabrik</button>
                <button onclick="changeTab('biaya')" id="tab-biaya" class="nav-btn nav-inactive">💳 Biaya Ops</button>
                <button onclick="changeTab('laporan')" id="tab-laporan" class="nav-btn nav-inactive">📄 Laporan</button>
            </nav>

            <button onclick="handleLogout()" class="mt-8 text-xs font-black text-red-400 hover:text-red-600 p-4 rounded-2xl border border-dashed border-red-100">🚪 LOGOUT</button>
        </aside>

        <main class="flex-1 p-6 md:p-12 max-w-7xl mx-auto w-full">
            <!-- DASHBOARD -->
            <section id="view-dashboard" class="space-y-8 animate-fade-in">
                <h2 class="text-3xl font-black text-slate-900 tracking-tight">Ringkasan Bisnis</h2>
                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
                    <div class="card border-l-8 border-l-orange-500"><p class="form-label">BELI TBS</p><h3 id="dash-beli-rp" class="text-2xl font-black">Rp 0</h3></div>
                    <div class="card border-l-8 border-l-blue-500"><p class="form-label">JUAL PABRIK</p><h3 id="dash-jual-rp" class="text-2xl font-black">Rp 0</h3></div>
                    <div class="card border-l-8 border-l-red-500"><p class="form-label">BIAYA OPS</p><h3 id="dash-biaya-rp" class="text-2xl font-black">Rp 0</h3></div>
                    <div class="card bg-green-900 text-white"><p class="form-label text-green-400">LABA BERSIH</p><h3 id="dash-laba-rp" class="text-2xl font-black">Rp 0</h3></div>
                </div>
                <div class="card h-[400px]">
                    <canvas id="mainChart"></canvas>
                </div>
            </section>

            <!-- BELI -->
            <section id="view-beli" class="hidden space-y-6 animate-fade-in">
                <div class="card border-t-4 border-t-orange-500">
                    <h3 class="font-black text-xl mb-6">🚜 Input Beli TBS</h3>
                    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
                        <div><p class="form-label">Tanggal</p><input type="date" id="beli-tgl" class="input-field"></div>
                        <div><p class="form-label">Nama Petani</p><input type="text" id="beli-nama" class="input-field" placeholder="Nama Petani"></div>
                        <div><p class="form-label">Berat (Kg)</p><input type="number" id="beli-kg" class="input-field" placeholder="0"></div>
                        <div><p class="form-label">Harga/Kg</p><input type="number" id="beli-hrg" class="input-field" placeholder="0"></div>
                    </div>
                    <button onclick="saveData('beli')" class="btn-action bg-orange-600 text-white mt-6 w-full md:w-auto">Simpan Transaksi</button>
                </div>
                <div class="card overflow-x-auto p-0">
                    <table class="w-full text-sm"><thead class="bg-slate-50 text-slate-500 uppercase text-[10px] font-black tracking-widest"><tr><th class="p-4 text-left">Tgl</th><th class="p-4 text-left">Petani</th><th class="p-4 text-right">Kg</th><th class="p-4 text-right">Total</th><th class="p-4 text-center">Aksi</th></tr></thead><tbody id="table-beli" class="divide-y"></tbody></table>
                </div>
            </section>

            <!-- JUAL -->
            <section id="view-jual" class="hidden space-y-6 animate-fade-in">
                <div class="card border-t-4 border-t-blue-500">
                    <h3 class="font-black text-xl mb-6">🏭 Input Jual Pabrik</h3>
                    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
                        <div><p class="form-label">Tanggal</p><input type="date" id="jual-tgl" class="input-field"></div>
                        <div><p class="form-label">Nama Pabrik</p><input type="text" id="jual-nama" class="input-field" placeholder="Nama Pabrik"></div>
                        <div><p class="form-label">Berat (Kg)</p><input type="number" id="jual-kg" class="input-field" placeholder="0"></div>
                        <div><p class="form-label">Harga/Kg</p><input type="number" id="jual-hrg" class="input-field" placeholder="0"></div>
                    </div>
                    <button onclick="saveData('jual')" class="btn-action bg-blue-600 text-white mt-6 w-full md:w-auto">Simpan Penjualan</button>
                </div>
                <div class="card overflow-x-auto p-0">
                    <table class="w-full text-sm"><thead class="bg-slate-50 text-slate-500 uppercase text-[10px] font-black tracking-widest"><tr><th class="p-4 text-left">Tgl</th><th class="p-4 text-left">Pabrik</th><th class="p-4 text-right">Kg</th><th class="p-4 text-right">Total</th><th class="p-4 text-center">Aksi</th></tr></thead><tbody id="table-jual" class="divide-y"></tbody></table>
                </div>
            </section>

            <!-- BIAYA -->
            <section id="view-biaya" class="hidden space-y-6 animate-fade-in">
                <div class="card border-t-4 border-t-red-500">
                    <h3 class="font-black text-xl mb-6">💳 Biaya Operasional</h3>
                    <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4">
                        <div><p class="form-label">Tanggal</p><input type="date" id="biaya-tgl" class="input-field"></div>
                        <div>
                            <p class="form-label">Jenis Transaksi</p>
                            <select id="biaya-cat" class="input-field">
                                <option value="BBM">BBM (Solar/Bensin)</option>
                                <option value="Snack">Snack / Konsumsi</option>
                                <option value="Pajak">Pajak Kendaraan/Lainnya</option>
                                <option value="Pulsa/Listrik">Pulsa (Listrik/Air/Internet)</option>
                                <option value="Gaji">Gaji / Upah Karyawan</option>
                                <option value="SPTI">Biaya SPTI / Retribusi</option>
                                <option value="ATK">ATK / Keperluan Kantor</option>
                                <option value="PPh">PPh (Pajak Penghasilan)</option>
                                <option value="Servis">Servis / Maintenance</option>
                                <option value="Lainnya">Biaya Lain-lain</option>
                            </select>
                        </div>
                        <div><p class="form-label">Jumlah (Rp)</p><input type="number" id="biaya-jml" class="input-field" placeholder="0"></div>
                        <div><p class="form-label">Keterangan (Opsional)</p><input type="text" id="biaya-ket" class="input-field" placeholder="Catatan tambahan"></div>
                    </div>
                    <button onclick="saveData('biaya')" class="btn-action bg-red-600 text-white mt-6 w-full md:w-auto">Simpan Biaya</button>
                </div>
                <div class="card overflow-x-auto p-0">
                    <table class="w-full text-sm"><thead class="bg-slate-50 text-slate-500 uppercase text-[10px] font-black tracking-widest"><tr><th class="p-4 text-left">Tgl</th><th class="p-4 text-left">Kategori</th><th class="p-4 text-left">Ket</th><th class="p-4 text-right">Jumlah</th><th class="p-4 text-center">Aksi</th></tr></thead><tbody id="table-biaya" class="divide-y"></tbody></table>
                </div>
            </section>

            <!-- LAPORAN -->
            <section id="view-laporan" class="hidden space-y-6 animate-fade-in">
                <div class="flex flex-col md:flex-row justify-between items-center gap-4">
                    <div class="card p-4 flex gap-4 bg-white items-center w-full md:w-auto">
                        <span class="text-xs font-black text-slate-400">Filter Bulan:</span>
                        <input type="month" id="report-month" class="outline-none font-bold" onchange="renderAll()">
                    </div>
                    <button onclick="exportToExcel()" class="btn-action bg-green-700 text-white w-full md:w-auto">📊 DOWNLOAD EXCEL DETAIL</button>
                </div>
                <div class="card bg-slate-900 text-white overflow-hidden p-0">
                    <div class="p-8 flex flex-col md:flex-row justify-between items-center border-b border-slate-700 gap-4">
                        <h2 class="text-2xl font-black italic">LABA RUGI DETAIL</h2>
                        <div class="text-center md:text-right">
                            <p class="text-xs text-green-400 font-bold tracking-widest uppercase">Estimasi Laba Bersih</p>
                            <h2 id="rep-net-profit" class="text-3xl font-black">Rp 0</h2>
                        </div>
                    </div>
                    <table class="w-full text-left text-sm text-slate-300">
                        <thead class="bg-slate-800 text-[10px] uppercase font-black tracking-widest">
                            <tr><th class="p-6">Komponen Laporan</th><th class="p-6 text-right">Jumlah Total</th></tr>
                        </thead>
                        <tbody class="divide-y divide-slate-800 font-medium">
                            <tr><td class="p-6">Total Pendapatan (Penjualan Pabrik)</td><td id="rep-total-jual" class="p-6 text-right text-blue-400">Rp 0</td></tr>
                            <tr><td class="p-6">Total Harga Pokok (Pembelian Petani)</td><td id="rep-total-beli" class="p-6 text-right text-orange-400">- Rp 0</td></tr>
                            <tr><td class="p-6">Total Biaya Operasional (BBM, Gaji, dll)</td><td id="rep-total-biaya" class="p-6 text-right text-red-400">- Rp 0</td></tr>
                        </tbody>
                    </table>
                </div>
            </section>
        </main>
    </div>

    <script>
        // Data LocalStorage
        let db = JSON.parse(localStorage.getItem('ags_db')) || { beli: [], jual: [], biaya: [] };

        function saveDataLocal() {
            localStorage.setItem('ags_db', JSON.stringify(db));
            renderAll();
        }

        window.handleLogin = () => {
            const pin = document.getElementById('login-pin').value;
            if(pin === "1234") {
                document.getElementById('login-screen').classList.add('hidden');
                document.getElementById('app-content').classList.remove('hidden');
                renderAll();
            } else {
                alert("PIN Salah! Gunakan 1234");
            }
        };

        window.handleLogout = () => location.reload();

        window.changeTab = (id) => {
            ['dashboard', 'beli', 'jual', 'biaya', 'laporan'].forEach(t => {
                const view = document.getElementById('view-'+t);
                const tab = document.getElementById('tab-'+t);
                if(view) view.classList.add('hidden');
                if(tab) tab.className = 'nav-btn nav-inactive';
            });
            document.getElementById('view-'+id).classList.remove('hidden');
            document.getElementById('tab-'+id).className = 'nav-btn nav-active';
        };

        window.saveData = (type) => {
            const data = { id: Date.now(), tgl: document.getElementById(type+'-tgl').value };
            
            if(type === 'biaya') {
                data.cat = document.getElementById('biaya-cat').value;
                data.jml = Number(document.getElementById('biaya-jml').value) || 0;
                data.ket = document.getElementById('biaya-ket').value || "-";
                if(data.jml <= 0) return alert("Masukkan jumlah biaya!");
            } else {
                data.nama = document.getElementById(type+'-nama').value;
                data.kg = Number(document.getElementById(type+'-kg').value) || 0;
                data.hrg = Number(document.getElementById(type+'-hrg').value) || 0;
                if(!data.nama || data.kg <= 0 || data.hrg <= 0) return alert("Lengkapi data transaksi!");
            }
            
            db[type].push(data);
            saveDataLocal();
            
            // Clear inputs
            document.querySelectorAll(`#view-${type} input`).forEach(i => { 
                if(i.type !== 'date' && i.id !== 'biaya-cat') i.value = ""; 
            });
        };

        window.deleteItem = (type, id) => {
            if(confirm("Hapus data ini?")) {
                db[type] = db[type].filter(x => x.id !== id);
                saveDataLocal();
            }
        };

        let chart;
        function renderAll() {
            const month = document.getElementById('report-month').value;
            
            const b = db.beli.filter(x => x.tgl.startsWith(month));
            const j = db.jual.filter(x => x.tgl.startsWith(month));
            const c = db.biaya.filter(x => x.tgl.startsWith(month));

            const sB = b.reduce((a,v)=>a+(v.kg*v.hrg),0);
            const sJ = j.reduce((a,v)=>a+(v.kg*v.hrg),0);
            const sC = c.reduce((a,v)=>a+v.jml,0);

            // Update Dash
            document.getElementById('dash-beli-rp').innerText = "Rp " + sB.toLocaleString();
            document.getElementById('dash-jual-rp').innerText = "Rp " + sJ.toLocaleString();
            document.getElementById('dash-biaya-rp').innerText = "Rp " + sC.toLocaleString();
            document.getElementById('dash-laba-rp').innerText = "Rp " + (sJ - sB - sC).toLocaleString();

            // Update Report Tab
            document.getElementById('rep-total-beli').innerText = "Rp " + sB.toLocaleString();
            document.getElementById('rep-total-jual').innerText = "Rp " + sJ.toLocaleString();
            document.getElementById('rep-total-biaya').innerText = "Rp " + sC.toLocaleString();
            document.getElementById('rep-net-profit').innerText = "Rp " + (sJ-sB-sC).toLocaleString();

            // Render Tables
            renderTable('beli', b);
            renderTable('jual', j);
            renderTable('biaya', c);

            // Render Chart
            renderChart(month);
        }

        function renderTable(type, data) {
            const container = document.getElementById('table-'+type);
            container.innerHTML = data.sort((a,b)=>b.tgl.localeCompare(a.tgl)).map(x => {
                if(type === 'biaya') {
                    return `<tr>
                        <td class="p-4">${x.tgl}</td>
                        <td class="p-4 font-bold text-red-600">${x.cat}</td>
                        <td class="p-4 italic text-slate-500">${x.ket}</td>
                        <td class="p-4 text-right font-bold">Rp ${x.jml.toLocaleString()}</td>
                        <td class="p-4 text-center"><button class="text-red-300 hover:text-red-600" onclick="deleteItem('${type}',${x.id})">🗑️</button></td>
                    </tr>`;
                }
                return `<tr>
                    <td class="p-4">${x.tgl}</td>
                    <td class="p-4 font-bold uppercase">${x.nama}</td>
                    <td class="p-4 text-right">${x.kg.toLocaleString()}</td>
                    <td class="p-4 text-right font-bold text-slate-700">Rp ${(x.kg*x.hrg).toLocaleString()}</td>
                    <td class="p-4 text-center"><button class="text-red-300 hover:text-red-600" onclick="deleteItem('${type}',${x.id})">🗑️</button></td>
                </tr>`;
            }).join('');
        }

        function renderChart(month) {
            const ctx = document.getElementById('mainChart').getContext('2d');
            if(chart) chart.destroy();
            
            const daysInMonth = new Date(month.split('-')[0], month.split('-')[1], 0).getDate();
            const days = [...Array(daysInMonth)].map((_,i)=>`${month}-${(i+1).toString().padStart(2,'0')}`);
            
            chart = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: days.map(d => d.split('-')[2]),
                    datasets: [
                        { 
                            label: 'Pendapatan (Jual)', 
                            data: days.map(d => db.jual.filter(x=>x.tgl===d).reduce((a,v)=>a+(v.kg*v.hrg),0)), 
                            borderColor: '#1d4ed8', 
                            backgroundColor: 'rgba(29, 78, 216, 0.1)',
                            fill: true,
                            tension: 0.4 
                        },
                        { 
                            label: 'Pengeluaran (Beli)', 
                            data: days.map(d => db.beli.filter(x=>x.tgl===d).reduce((a,v)=>a+(v.kg*v.hrg),0)), 
                            borderColor: '#ea580c', 
                            borderDash: [5, 5],
                            tension: 0.4 
                        }
                    ]
                },
                options: { 
                    maintainAspectRatio: false,
                    plugins: { legend: { position: 'top' } },
                    scales: { y: { beginAtZero: true } }
                }
            });
        }

        window.exportToExcel = () => {
            const m = document.getElementById('report-month').value;
            const b = db.beli.filter(x=>x.tgl.startsWith(m));
            const j = db.jual.filter(x=>x.tgl.startsWith(m));
            const c = db.biaya.filter(x=>x.tgl.startsWith(m));

            const totalBeli = b.reduce((a,v)=>a+(v.kg*v.hrg),0);
            const totalJual = j.reduce((a,v)=>a+(v.kg*v.hrg),0);
            const totalBiaya = c.reduce((a,v)=>a+v.jml,0);

            const wb = XLSX.utils.book_new();

            // 1. Ringkasan
            const summary = [
                ["LAPORAN LABA RUGI ADHIGROUP SAWINDO"],
                ["Periode:", m],
                [],
                ["KOMPONEN", "JUMLAH (RP)"],
                ["Total Penjualan", totalJual],
                ["Total Pembelian TBS", totalBeli],
                ["Total Biaya Operasional", totalBiaya],
                ["----------------------", "----------"],
                ["LABA BERSIH", totalJual - totalBeli - totalBiaya]
            ];
            XLSX.utils.book_append_sheet(wb, XLSX.utils.aoa_to_sheet(summary), "Laba Rugi");

            // 2. Detail Pembelian
            const buyData = [["Tanggal", "Nama Petani", "Berat (Kg)", "Harga/Kg", "Total (Rp)"]].concat(b.map(x=>[x.tgl, x.nama, x.kg, x.hrg, x.kg*x.hrg]));
            XLSX.utils.book_append_sheet(wb, XLSX.utils.aoa_to_sheet(buyData), "Detail Pembelian");

            // 3. Detail Penjualan
            const sellData = [["Tanggal", "Nama Pabrik", "Berat (Kg)", "Harga/Kg", "Total (Rp)"]].concat(j.map(x=>[x.tgl, x.nama, x.kg, x.hrg, x.kg*x.hrg]));
            XLSX.utils.book_append_sheet(wb, XLSX.utils.aoa_to_sheet(sellData), "Detail Penjualan");

            // 4. Detail Biaya
            const costData = [["Tanggal", "Jenis Biaya", "Keterangan", "Jumlah (Rp)"]].concat(c.map(x=>[x.tgl, x.cat, x.ket, x.jml]));
            XLSX.utils.book_append_sheet(wb, XLSX.utils.aoa_to_sheet(costData), "Detail Biaya Ops");

            XLSX.writeFile(wb, `Laporan_AGS_Lengkap_${m}.xlsx`);
        };

        // Initialize Defaults
        const now = new Date();
        const localDate = now.toLocaleDateString('en-CA'); // YYYY-MM-DD
        ['beli-tgl','jual-tgl','biaya-tgl'].forEach(id => {
            const el = document.getElementById(id);
            if(el) el.value = localDate;
        });
        document.getElementById('report-month').value = localDate.slice(0, 7);
    </script>
</body>
</html>
