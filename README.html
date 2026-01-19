<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PalmCore ERP - Financial Edition</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700&display=swap');
        body { font-family: 'Plus Jakarta Sans', sans-serif; background-color: #f8fafc; }
        .glass-panel { background: white; border: 1px solid #e2e8f0; }
        .nav-btn.active { background: #059669 !important; color: white !important; box-shadow: 0 10px 15px -3px rgba(5, 150, 105, 0.2); }
        .modal-overlay { position: fixed; inset: 0; background: rgba(0,0,0,0.5); display: none; align-items: center; justify-content: center; z-index: 100000; }
        .page-content { animation: fadeIn 0.3s ease-out; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
    </style>
</head>
<body class="text-slate-700">

    <!-- Modal Konfirmasi Hapus -->
    <div id="delete-modal" class="modal-overlay">
        <div class="bg-white p-8 rounded-3xl max-w-sm w-full mx-4 shadow-2xl text-center">
            <div class="text-rose-500 mb-4"><i class="fas fa-exclamation-triangle text-5xl"></i></div>
            <h3 class="text-xl font-black mb-2">Hapus Transaksi?</h3>
            <p class="text-slate-500 text-sm mb-6">Data ini akan dihapus kekal dari storan tempatan anda.</p>
            <div class="flex gap-3">
                <button onclick="closeDeleteModal()" class="flex-1 py-3 bg-slate-100 font-bold rounded-xl">Batal</button>
                <button id="confirm-delete-btn" class="flex-1 py-3 bg-rose-600 text-white font-bold rounded-xl">Hapus</button>
            </div>
        </div>
    </div>

    <!-- Layout Utama -->
    <div id="app-body">
        <nav class="fixed left-0 top-0 h-full w-20 lg:w-64 bg-white border-r border-slate-200 z-50">
            <div class="p-6 flex flex-col h-full">
                <div class="flex items-center gap-3 mb-10">
                    <div class="bg-emerald-600 p-2 rounded-xl text-white shadow-lg"><i class="fas fa-leaf text-xl"></i></div>
                    <span class="font-bold text-xl hidden lg:block text-emerald-900">PalmCore<span class="text-emerald-500 font-black">ERP</span></span>
                </div>
                
                <div class="space-y-2 flex-1 overflow-y-auto">
                    <button id="nav-dashboard" onclick="navTo('dashboard')" class="nav-btn active w-full flex items-center gap-4 p-3 rounded-xl text-slate-500 transition-all hover:bg-slate-50">
                        <i class="fas fa-chart-pie w-5"></i><span class="font-bold hidden lg:block text-sm">Dashboard</span>
                    </button>
                    <button id="nav-modal" onclick="navTo('modal')" class="nav-btn w-full flex items-center gap-4 p-3 rounded-xl text-slate-500 transition-all hover:bg-slate-50">
                        <i class="fas fa-vault w-5"></i><span class="font-bold hidden lg:block text-sm">Input Modal</span>
                    </button>
                    <button id="nav-beli" onclick="navTo('beli')" class="nav-btn w-full flex items-center gap-4 p-3 rounded-xl text-slate-500 transition-all hover:bg-slate-50">
                        <i class="fas fa-balance-scale w-5"></i><span class="font-bold hidden lg:block text-sm">Beli TBS</span>
                    </button>
                    <button id="nav-jual" onclick="navTo('jual')" class="nav-btn w-full flex items-center gap-4 p-3 rounded-xl text-slate-500 transition-all hover:bg-slate-50">
                        <i class="fas fa-truck-moving w-5"></i><span class="font-bold hidden lg:block text-sm">Jual (PKS)</span>
                    </button>
                    <button id="nav-biaya" onclick="navTo('biaya')" class="nav-btn w-full flex items-center gap-4 p-3 rounded-xl text-slate-500 transition-all hover:bg-slate-50">
                        <i class="fas fa-money-bill-wave w-5"></i><span class="font-bold hidden lg:block text-sm">Biaya</span>
                    </button>
                    <button id="nav-laporan" onclick="navTo('laporan')" class="nav-btn w-full flex items-center gap-4 p-3 rounded-xl text-slate-500 transition-all hover:bg-slate-50">
                        <i class="fas fa-file-invoice-dollar w-5"></i><span class="font-bold hidden lg:block text-sm">Laporan & Keuangan</span>
                    </button>
                </div>

                <div class="pt-4 border-t border-slate-100">
                    <button onclick="clearAllData()" class="w-full flex items-center gap-4 p-3 rounded-xl text-rose-500 hover:bg-rose-50 transition-colors">
                        <i class="fas fa-trash-alt w-5"></i><span class="font-bold hidden lg:block text-xs text-left">Reset Semua Data</span>
                    </button>
                </div>
            </div>
        </nav>

        <main class="ml-20 lg:ml-64 p-4 lg:p-8 min-h-screen">
            <!-- Dashboard -->
            <div id="page-dashboard" class="page-content">
                <h1 class="text-2xl font-black text-slate-800 mb-6">Ringkasan Operasional</h1>
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
                    <div class="glass-panel p-6 rounded-3xl border-b-4 border-emerald-500">
                        <p class="text-[10px] font-bold text-slate-400 uppercase mb-1">Stok (Kg)</p>
                        <h2 id="stok-val" class="text-3xl font-black text-slate-800">0</h2>
                    </div>
                    <div class="glass-panel p-6 rounded-3xl border-b-4 border-blue-500">
                        <p class="text-[10px] font-bold text-slate-400 uppercase mb-1">Tunai Aktif</p>
                        <h2 id="cash-val" class="text-2xl font-black text-blue-600">Rp 0</h2>
                    </div>
                    <div class="glass-panel p-6 rounded-3xl border-b-4 border-amber-500">
                        <p class="text-[10px] font-bold text-slate-400 uppercase mb-1">Total Modal</p>
                        <h2 id="modal-total-val" class="text-2xl font-black text-amber-600">Rp 0</h2>
                    </div>
                    <div class="bg-slate-900 p-6 rounded-3xl text-white shadow-xl">
                        <p class="text-[10px] font-bold text-emerald-400 uppercase mb-1">Laba Bersih</p>
                        <h2 id="profit-val" class="text-2xl font-black text-emerald-400">Rp 0</h2>
                    </div>
                </div>
                <div class="glass-panel p-8 rounded-3xl h-[400px]">
                    <canvas id="chartView"></canvas>
                </div>
            </div>

            <!-- Page Input Modal -->
            <div id="page-modal" class="page-content hidden">
                <div class="max-w-xl mx-auto glass-panel p-8 rounded-[2.5rem] border-t-8 border-amber-500 shadow-xl">
                    <h3 class="font-black text-2xl text-amber-900 mb-6">Input Modal Pemilik</h3>
                    <div class="space-y-6">
                        <input id="m-sumber" type="text" placeholder="Nama Pelabur / Sumber Modal" class="w-full p-4 bg-slate-50 border rounded-2xl font-bold">
                        <input id="m-nominal" type="number" placeholder="Nominal Modal (Rp)" class="w-full p-4 bg-amber-50 border-2 border-amber-100 rounded-2xl font-bold text-amber-700">
                        <button onclick="simpan('MODAL')" class="w-full py-5 bg-amber-600 hover:bg-amber-700 text-white font-black rounded-2xl transition-all shadow-xl">SIMPAN MODAL</button>
                    </div>
                </div>
            </div>

            <!-- Page Beli -->
            <div id="page-beli" class="page-content hidden">
                <div class="max-w-4xl mx-auto glass-panel p-8 rounded-[2.5rem] border-t-8 border-emerald-500 shadow-xl">
                    <h3 class="font-black text-2xl text-emerald-900 mb-6">Input TBS Masuk</h3>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-10">
                        <div class="space-y-5">
                            <input id="b-nama" type="text" placeholder="Nama Pemasok" class="w-full p-4 bg-slate-50 border rounded-2xl font-bold">
                            <div class="grid grid-cols-2 gap-4">
                                <input id="b-bruto" type="number" oninput="hitungBeli()" placeholder="Brutto" class="w-full p-4 bg-slate-50 border rounded-2xl font-bold">
                                <input id="b-tara" type="number" oninput="hitungBeli()" placeholder="Tarra" class="w-full p-4 bg-slate-50 border rounded-2xl font-bold text-rose-600">
                            </div>
                            <div class="grid grid-cols-2 gap-4">
                                <input id="b-persen" type="number" oninput="hitungBeli()" value="3" class="w-full p-4 bg-rose-50 border-2 border-rose-100 rounded-2xl font-bold text-rose-700">
                                <input id="b-harga" type="number" oninput="hitungBeli()" placeholder="Harga Beli" class="w-full p-4 bg-emerald-50 border-2 border-emerald-200 rounded-2xl font-bold text-emerald-700">
                            </div>
                        </div>
                        <div class="bg-emerald-900 rounded-[2.5rem] p-10 text-white flex flex-col justify-center text-center shadow-2xl">
                            <p class="text-xs font-bold text-emerald-400 mb-2 uppercase">Total Bayar</p>
                            <h2 id="res-total" class="text-4xl font-black mb-4">Rp 0</h2>
                            <p class="text-sm">Netto: <span id="res-netto">0</span> Kg</p>
                            <button onclick="simpan('BELI')" class="w-full py-5 mt-10 bg-emerald-500 hover:bg-emerald-400 rounded-2xl font-black">SIMPAN</button>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Page Jual -->
            <div id="page-jual" class="page-content hidden">
                <div class="max-w-4xl mx-auto glass-panel p-8 rounded-[2.5rem] border-t-8 border-blue-500 shadow-xl">
                    <h3 class="font-black text-2xl text-blue-900 mb-6">Penjualan ke PKS</h3>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-10">
                        <div class="space-y-5">
                            <input id="j-pabrik" type="text" placeholder="Nama Pabrik" class="w-full p-4 bg-slate-50 border rounded-2xl font-bold">
                            <input id="j-netto" type="number" oninput="hitungJual()" placeholder="Netto Pabrik (Kg)" class="w-full p-4 bg-slate-50 border rounded-2xl font-bold">
                            <input id="j-harga" type="number" oninput="hitungJual()" placeholder="Harga Jual /Kg" class="w-full p-4 bg-blue-50 border-2 border-blue-200 rounded-2xl font-bold text-blue-700">
                        </div>
                        <div class="bg-blue-900 rounded-[2.5rem] p-10 text-white flex flex-col justify-center text-center shadow-2xl">
                            <p class="text-xs font-bold text-blue-400 mb-2 uppercase">Total Terima</p>
                            <h2 id="res-jual-total" class="text-4xl font-black">Rp 0</h2>
                            <button onclick="simpan('JUAL')" class="w-full py-5 mt-10 bg-blue-500 hover:bg-blue-400 rounded-2xl font-black">SIMPAN</button>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Page Biaya -->
            <div id="page-biaya" class="page-content hidden">
                <div class="max-w-xl mx-auto glass-panel p-8 rounded-[2.5rem] border-t-8 border-rose-500 shadow-xl">
                    <h3 class="font-black text-2xl text-rose-900 mb-6">Input Biaya Operasional</h3>
                    <div class="space-y-6">
                        <input id="c-ket" type="text" placeholder="Keterangan Biaya" class="w-full p-4 bg-slate-50 border rounded-2xl font-bold">
                        <input id="c-nominal" type="number" placeholder="Nominal (Rp)" class="w-full p-4 bg-rose-50 border-2 border-rose-100 rounded-2xl font-bold text-rose-700">
                        <button onclick="simpan('BIAYA')" class="w-full py-5 bg-rose-600 hover:bg-rose-700 text-white font-black rounded-2xl transition-all">SIMPAN BIAYA</button>
                    </div>
                </div>
            </div>

            <!-- Page Laporan Keuangan -->
            <div id="page-laporan" class="page-content hidden">
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-8 mb-8">
                    <!-- Laporan Laba Rugi -->
                    <div class="glass-panel p-8 rounded-[2rem] shadow-sm">
                        <div class="flex items-center justify-between mb-6">
                            <h3 class="font-black text-xl text-slate-800">Laporan Laba Rugi</h3>
                            <i class="fas fa-file-invoice text-emerald-500 text-2xl"></i>
                        </div>
                        <div class="space-y-4 text-sm font-bold">
                            <div class="flex justify-between border-b pb-2"><span>Penjualan TBS</span><span id="pnl-jual" class="text-blue-600">Rp 0</span></div>
                            <div class="flex justify-between border-b pb-2 text-rose-500"><span>Pembelian TBS (HPP)</span><span id="pnl-beli">Rp 0</span></div>
                            <div class="flex justify-between border-b pb-2 font-black text-emerald-700 bg-emerald-50 px-2 py-1 rounded"><span>Laba Kotor</span><span id="pnl-kotor">Rp 0</span></div>
                            <div class="flex justify-between border-b pb-2 text-rose-500"><span>Biaya Operasional</span><span id="pnl-biaya">Rp 0</span></div>
                            <div class="flex justify-between pt-4 text-xl font-black text-slate-900"><span>Laba Bersih</span><span id="pnl-bersih">Rp 0</span></div>
                        </div>
                    </div>

                    <!-- Laporan Perubahan Modal -->
                    <div class="glass-panel p-8 rounded-[2rem] shadow-sm">
                        <div class="flex items-center justify-between mb-6">
                            <h3 class="font-black text-xl text-slate-800">Perubahan Modal</h3>
                            <i class="fas fa-seedling text-amber-500 text-2xl"></i>
                        </div>
                        <div class="space-y-4 text-sm font-bold">
                            <div class="flex justify-between border-b pb-2"><span>Modal Awal (Pelaburan)</span><span id="eq-awal" class="text-amber-600">Rp 0</span></div>
                            <div class="flex justify-between border-b pb-2"><span>Laba Bersih Berjalan</span><span id="eq-laba" class="text-emerald-600">Rp 0</span></div>
                            <div class="flex justify-between pt-6 text-xl font-black text-slate-900 border-t-2 border-slate-900"><span>Modal Akhir</span><span id="eq-akhir">Rp 0</span></div>
                        </div>
                    </div>
                </div>

                <!-- Riwayat Transaksi -->
                <div class="flex justify-between items-center mb-6">
                    <h2 class="text-2xl font-black">Riwayat Transaksi</h2>
                    <button onclick="exportExcel()" class="bg-emerald-600 text-white px-6 py-2 rounded-xl font-bold hover:bg-emerald-700">Export Excel</button>
                </div>
                <div class="glass-panel rounded-3xl overflow-hidden shadow-sm">
                    <div class="overflow-x-auto">
                        <table class="w-full text-sm text-left">
                            <thead class="bg-slate-50 border-b">
                                <tr>
                                    <th class="p-5 uppercase text-[10px] font-black text-slate-400">Tanggal</th>
                                    <th class="p-5 uppercase text-[10px] font-black text-slate-400">Tipe</th>
                                    <th class="p-5 uppercase text-[10px] font-black text-slate-400">Keterangan</th>
                                    <th class="p-5 uppercase text-[10px] font-black text-slate-400 text-right">Kg</th>
                                    <th class="p-5 uppercase text-[10px] font-black text-slate-400 text-right">Nilai (Rp)</th>
                                    <th class="p-5 uppercase text-[10px] font-black text-slate-400 text-center">Aksi</th>
                                </tr>
                            </thead>
                            <tbody id="table-log" class="divide-y divide-slate-50"></tbody>
                        </table>
                    </div>
                </div>
            </div>
        </main>
    </div>

    <div id="toast-container" class="fixed bottom-5 right-5 z-[200000] flex flex-col gap-2"></div>

    <script>
        let dataStore = JSON.parse(localStorage.getItem('sawit_erp_data')) || [];
        let pendingDeleteIdx = null;
        let mainChart = null;

        function saveData() {
            localStorage.setItem('sawit_erp_data', JSON.stringify(dataStore));
            renderAll();
        }

        function navTo(id) {
            document.querySelectorAll('.page-content').forEach(p => p.classList.add('hidden'));
            document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
            document.getElementById('page-' + id).classList.remove('hidden');
            document.getElementById('nav-' + id).classList.add('active');
            if(id === 'dashboard') initChart();
        }

        function hitungBeli() {
            const b = parseFloat(document.getElementById('b-bruto').value) || 0;
            const t = parseFloat(document.getElementById('b-tara').value) || 0;
            const p = parseFloat(document.getElementById('b-persen').value) || 0;
            const h = parseFloat(document.getElementById('b-harga').value) || 0;
            const netto = (b - t) - Math.round((b - t) * (p / 100));
            const total = netto * h;
            document.getElementById('res-netto').innerText = netto.toLocaleString();
            document.getElementById('res-total').innerText = 'Rp ' + total.toLocaleString();
            return { netto, total };
        }

        function hitungJual() {
            const n = parseFloat(document.getElementById('j-netto').value) || 0;
            const h = parseFloat(document.getElementById('j-harga').value) || 0;
            const total = n * h;
            document.getElementById('res-jual-total').innerText = 'Rp ' + total.toLocaleString();
            return { netto: n, total: total };
        }

        function showToast(msg, type = 'emerald') {
            const toast = document.createElement('div');
            toast.className = `bg-${type}-600 text-white p-4 rounded-xl shadow-2xl font-bold`;
            toast.innerText = msg;
            document.getElementById('toast-container').appendChild(toast);
            setTimeout(() => toast.remove(), 3000);
        }

        function simpan(tipe) {
            let entry = { tipe, ts: Date.now(), id: Date.now() };
            if(tipe === 'BELI') {
                const r = hitungBeli();
                entry.nama = document.getElementById('b-nama').value || 'Pemasok';
                entry.netto = r.netto; entry.total = r.total;
            } else if(tipe === 'JUAL') {
                const r = hitungJual();
                entry.nama = document.getElementById('j-pabrik').value || 'PKS';
                entry.netto = r.netto; entry.total = r.total;
            } else if(tipe === 'BIAYA') {
                entry.nama = document.getElementById('c-ket').value || 'Biaya';
                entry.total = parseFloat(document.getElementById('c-nominal').value) || 0;
                entry.netto = 0;
            } else if(tipe === 'MODAL') {
                entry.nama = document.getElementById('m-sumber').value || 'Pelabur';
                entry.total = parseFloat(document.getElementById('m-nominal').value) || 0;
                entry.netto = 0;
            }

            dataStore.unshift(entry);
            saveData();
            showToast('Data berhasil disimpan');
            document.querySelectorAll('input').forEach(i => { if(i.id !== 'b-persen') i.value = '' });
            hitungBeli(); hitungJual();
        }

        function openDeleteModal(id) {
            pendingDeleteIdx = id;
            document.getElementById('delete-modal').style.display = 'flex';
        }

        function closeDeleteModal() {
            document.getElementById('delete-modal').style.display = 'none';
        }

        document.getElementById('confirm-delete-btn').onclick = () => {
            dataStore = dataStore.filter(d => d.id !== pendingDeleteIdx);
            saveData();
            closeDeleteModal();
            showToast('Data dihapus', 'rose');
        };

        function clearAllData() {
            if(confirm("AWAS: Padam semua data transaksi? Tindakan ini tidak boleh diundur.")) {
                dataStore = [];
                saveData();
            }
        }

        function renderAll() {
            const table = document.getElementById('table-log');
            table.innerHTML = '';
            
            let totalBeli = 0, totalJual = 0, totalBiaya = 0, totalModal = 0, stokKg = 0, jualKg = 0;
            
            dataStore.forEach(d => {
                if(d.tipe==='BELI'){ totalBeli += d.total; stokKg += d.netto; }
                if(d.tipe==='JUAL'){ totalJual += d.total; stokKg -= d.netto; jualKg += d.netto; }
                if(d.tipe==='BIAYA'){ totalBiaya += d.total; }
                if(d.tipe==='MODAL'){ totalModal += d.total; }

                let color = 'text-slate-600';
                if(d.tipe==='BELI') color = 'text-emerald-600';
                if(d.tipe==='JUAL') color = 'text-blue-600';
                if(d.tipe==='BIAYA') color = 'text-rose-600';
                if(d.tipe==='MODAL') color = 'text-amber-600';

                table.insertAdjacentHTML('beforeend', `<tr class="hover:bg-slate-50">
                    <td class="p-5 text-xs font-bold text-slate-400">${new Date(d.ts).toLocaleDateString()}</td>
                    <td class="p-5 font-black ${color}">${d.tipe}</td>
                    <td class="p-5 font-bold">${d.nama}</td>
                    <td class="p-5 text-right font-bold">${d.netto ? d.netto.toLocaleString() : '-'}</td>
                    <td class="p-5 text-right font-black">Rp ${d.total.toLocaleString()}</td>
                    <td class="p-5 text-center">
                        <button onclick="openDeleteModal(${d.id})" class="text-slate-300 hover:text-rose-600"><i class="fas fa-trash-alt"></i></button>
                    </td>
                </tr>`);
            });

            // Financial Calculations
            const labaKotor = totalJual - totalBeli;
            const labaBersih = labaKotor - totalBiaya;
            const tunaiAktif = totalModal + totalJual - totalBeli - totalBiaya;
            const modalAkhir = totalModal + labaBersih;

            // Dashboard Updates
            document.getElementById('stok-val').innerText = stokKg.toLocaleString();
            document.getElementById('cash-val').innerText = 'Rp ' + tunaiAktif.toLocaleString();
            document.getElementById('modal-total-val').innerText = 'Rp ' + totalModal.toLocaleString();
            document.getElementById('profit-val').innerText = 'Rp ' + labaBersih.toLocaleString();
            document.getElementById('jual-val').innerText = jualKg.toLocaleString();

            // Laporan Keuangan Updates
            document.getElementById('pnl-jual').innerText = 'Rp ' + totalJual.toLocaleString();
            document.getElementById('pnl-beli').innerText = 'Rp ' + totalBeli.toLocaleString();
            document.getElementById('pnl-kotor').innerText = 'Rp ' + labaKotor.toLocaleString();
            document.getElementById('pnl-biaya').innerText = 'Rp ' + totalBiaya.toLocaleString();
            document.getElementById('pnl-bersih').innerText = 'Rp ' + labaBersih.toLocaleString();
            
            document.getElementById('eq-awal').innerText = 'Rp ' + totalModal.toLocaleString();
            document.getElementById('eq-laba').innerText = 'Rp ' + labaBersih.toLocaleString();
            document.getElementById('eq-akhir').innerText = 'Rp ' + modalAkhir.toLocaleString();
        }

        function exportExcel() {
            const ws = XLSX.utils.json_to_sheet(dataStore.map(d => ({ 
                Tanggal: new Date(d.ts).toLocaleDateString(), 
                Tipe: d.tipe, Nama: d.nama, Netto_Kg: d.netto || 0, Total_Rp: d.total 
            })));
            const wb = XLSX.utils.book_new(); 
            XLSX.utils.book_append_sheet(wb, ws, "Financial_Report");
            XLSX.writeFile(wb, `Laporan_Keuangan_Sawit_${Date.now()}.xlsx`);
        }

        function initChart() {
            const ctx = document.getElementById('chartView').getContext('2d');
            const sortedData = [...dataStore].filter(d => d.tipe === 'BELI' || d.tipe === 'JUAL').slice(0, 10).reverse();
            if(mainChart) mainChart.destroy();
            mainChart = new Chart(ctx, { 
                type: 'bar', 
                data: { 
                    labels: sortedData.map(d => new Date(d.ts).toLocaleDateString()), 
                    datasets: [{
                        label: 'Volume (Kg)', 
                        data: sortedData.map(d => d.netto), 
                        backgroundColor: sortedData.map(d => d.tipe === 'BELI' ? '#10b981' : '#3b82f6'), 
                        borderRadius: 8
                    }] 
                },
                options: { 
                    maintainAspectRatio: false,
                    scales: { y: { beginAtZero: true } }
                }
            });
        }

        window.onload = () => { renderAll(); initChart(); }
    </script>
</body>
</html>

