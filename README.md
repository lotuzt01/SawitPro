<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>PalmCore ERP - Pro Edition</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700&display=swap');
        body { font-family: 'Plus Jakarta Sans', sans-serif; background-color: #f1f5f9; }
        
        .glass-panel { background: white; border: 1px solid #cbd5e1; }
        
        /* High Contrast Nav */
        .nav-btn { color: #475569; font-weight: 700; transition: all 0.2s; }
        .nav-btn:hover { background: #e2e8f0; color: #0f172a; }
        .nav-btn.active { 
            background: #047857 !important; 
            color: #ffffff !important; 
            box-shadow: 0 4px 12px rgba(4, 120, 87, 0.3);
        }

        .modal-overlay { position: fixed; inset: 0; background: rgba(15, 23, 42, 0.7); display: none; align-items: center; justify-content: center; z-index: 100000; backdrop-filter: blur(4px); }
        
        .page-content { animation: fadeIn 0.3s ease-out; }
        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        
        /* Mobile friendly touch targets */
        input, button { min-height: 48px; }
        
        /* Status Badges */
        .badge-beli { background: #dcfce7; color: #166534; border: 1px solid #bbf7d0; }
        .badge-jual { background: #dbeafe; color: #1e40af; border: 1px solid #bfdbfe; }
        .badge-biaya { background: #fee2e2; color: #991b1b; border: 1px solid #fecaca; }
        .badge-modal { background: #fef3c7; color: #92400e; border: 1px solid #fde68a; }
    </style>
</head>
<body class="text-slate-800">

    <!-- Modal Konfirmasi Hapus -->
    <div id="delete-modal" class="modal-overlay">
        <div class="bg-white p-8 rounded-3xl max-w-sm w-full mx-4 shadow-2xl text-center border border-slate-200">
            <div class="text-rose-600 mb-4"><i class="fas fa-trash-alt text-5xl"></i></div>
            <h3 class="text-xl font-bold mb-2 text-slate-900">Hapus Data?</h3>
            <p class="text-slate-500 text-sm mb-6">Tindakan ini tidak dapat dibatalkan.</p>
            <div class="flex gap-3">
                <button onclick="closeDeleteModal()" class="flex-1 py-3 bg-slate-200 text-slate-800 font-bold rounded-xl">Batal</button>
                <button id="confirm-delete-btn" class="flex-1 py-3 bg-rose-600 text-white font-bold rounded-xl">Hapus</button>
            </div>
        </div>
    </div>

    <!-- Layout Utama -->
    <div id="app-body">
        <!-- Sidebar Navigation -->
        <nav class="fixed left-0 top-0 h-full w-20 lg:w-64 bg-white border-r border-slate-300 z-50 shadow-xl">
            <div class="p-4 lg:p-6 flex flex-col h-full">
                <div class="flex items-center gap-3 mb-8 justify-center lg:justify-start">
                    <div class="bg-emerald-700 p-2.5 rounded-xl text-white shadow-lg"><i class="fas fa-leaf text-xl"></i></div>
                    <span class="font-black text-xl hidden lg:block text-slate-900">PalmCore<span class="text-emerald-600">ERP</span></span>
                </div>
                
                <div class="space-y-1.5 flex-1 overflow-y-auto pr-1">
                    <button id="nav-dashboard" onclick="navTo('dashboard')" class="nav-btn active w-full flex items-center gap-4 p-3.5 rounded-xl">
                        <i class="fas fa-th-large w-5 text-lg"></i><span class="hidden lg:block">Dashboard</span>
                    </button>
                    <button id="nav-modal" onclick="navTo('modal')" class="nav-btn w-full flex items-center gap-4 p-3.5 rounded-xl">
                        <i class="fas fa-piggy-bank w-5 text-lg"></i><span class="hidden lg:block">Modal</span>
                    </button>
                    <button id="nav-beli" onclick="navTo('beli')" class="nav-btn w-full flex items-center gap-4 p-3.5 rounded-xl">
                        <i class="fas fa-shopping-cart w-5 text-lg"></i><span class="hidden lg:block">Beli TBS</span>
                    </button>
                    <button id="nav-jual" onclick="navTo('jual')" class="nav-btn w-full flex items-center gap-4 p-3.5 rounded-xl">
                        <i class="fas fa-truck w-5 text-lg"></i><span class="hidden lg:block">Jual PKS</span>
                    </button>
                    <button id="nav-biaya" onclick="navTo('biaya')" class="nav-btn w-full flex items-center gap-4 p-3.5 rounded-xl">
                        <i class="fas fa-wallet w-5 text-lg"></i><span class="hidden lg:block">Biaya Ops</span>
                    </button>
                    <button id="nav-laporan" onclick="navTo('laporan')" class="nav-btn w-full flex items-center gap-4 p-3.5 rounded-xl">
                        <i class="fas fa-print w-5 text-lg"></i><span class="hidden lg:block">Laporan</span>
                    </button>
                </div>

                <div class="pt-4 border-t border-slate-200">
                    <button onclick="clearAllData()" class="w-full flex items-center gap-4 p-3.5 rounded-xl text-rose-600 hover:bg-rose-50 font-bold transition-colors">
                        <i class="fas fa-power-off w-5"></i><span class="hidden lg:block text-xs">Reset Sistem</span>
                    </button>
                </div>
            </div>
        </nav>

        <!-- Main Content Area -->
        <main class="ml-20 lg:ml-64 p-4 lg:p-8 min-h-screen">
            
            <!-- Global Filter (Always Visible on Top) -->
            <div class="mb-6 glass-panel p-4 rounded-2xl border-l-4 border-emerald-600 flex flex-wrap items-center gap-4 shadow-sm">
                <div class="flex items-center gap-2">
                    <i class="fas fa-calendar-alt text-emerald-600"></i>
                    <span class="text-xs font-black uppercase text-slate-400">Filter Periode:</span>
                </div>
                <div class="flex items-center gap-2 flex-1 min-w-[300px]">
                    <input type="date" id="filter-start" onchange="renderAll()" class="bg-slate-50 border border-slate-300 p-2 rounded-lg text-sm font-bold focus:ring-2 ring-emerald-500 outline-none">
                    <span class="text-slate-400 font-bold">s/d</span>
                    <input type="date" id="filter-end" onchange="renderAll()" class="bg-slate-50 border border-slate-300 p-2 rounded-lg text-sm font-bold focus:ring-2 ring-emerald-500 outline-none">
                    <button onclick="resetFilter()" class="text-[10px] font-bold bg-slate-200 px-3 py-2 rounded-lg hover:bg-slate-300">RESET</button>
                </div>
            </div>

            <!-- Halaman Dashboard -->
            <div id="page-dashboard" class="page-content">
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-4 mb-8">
                    <div class="glass-panel p-6 rounded-3xl border-b-4 border-emerald-500 shadow-sm relative overflow-hidden">
                        <div class="absolute -right-2 -top-2 opacity-5 text-6xl"><i class="fas fa-box"></i></div>
                        <p class="text-[10px] font-black text-slate-400 uppercase tracking-widest mb-1">Stok Gudang (Kg)</p>
                        <h2 id="stok-val" class="text-3xl font-black text-slate-900">0</h2>
                    </div>
                    <div class="glass-panel p-6 rounded-3xl border-b-4 border-blue-600 shadow-sm relative overflow-hidden">
                        <div class="absolute -right-2 -top-2 opacity-5 text-6xl"><i class="fas fa-money-bill-wave"></i></div>
                        <p class="text-[10px] font-black text-slate-400 uppercase tracking-widest mb-1">Kas / Tunai</p>
                        <h2 id="cash-val" class="text-2xl font-black text-blue-700">Rp 0</h2>
                    </div>
                    <div class="glass-panel p-6 rounded-3xl border-b-4 border-amber-500 shadow-sm relative overflow-hidden">
                        <div class="absolute -right-2 -top-2 opacity-5 text-6xl"><i class="fas fa-vault"></i></div>
                        <p class="text-[10px] font-black text-slate-400 uppercase tracking-widest mb-1">Total Modal</p>
                        <h2 id="modal-total-val" class="text-2xl font-black text-amber-700">Rp 0</h2>
                    </div>
                    <div class="bg-slate-900 p-6 rounded-3xl text-white shadow-xl relative overflow-hidden border-b-4 border-emerald-400">
                        <div class="absolute -right-2 -top-2 opacity-10 text-6xl"><i class="fas fa-chart-line"></i></div>
                        <p class="text-[10px] font-bold text-emerald-400 uppercase tracking-widest mb-1">Laba Bersih</p>
                        <h2 id="profit-val" class="text-2xl font-black text-emerald-400">Rp 0</h2>
                    </div>
                </div>
                
                <div class="glass-panel p-6 lg:p-8 rounded-3xl h-[450px] shadow-sm">
                    <h3 class="font-black text-slate-900 mb-6 flex items-center gap-2">
                        <i class="fas fa-chart-area text-emerald-600"></i> Tren Volume & Arus Kas
                    </h3>
                    <div class="h-[350px]">
                        <canvas id="chartView"></canvas>
                    </div>
                </div>
            </div>

            <!-- Halaman Modal -->
            <div id="page-modal" class="page-content hidden">
                <div class="max-w-xl mx-auto glass-panel p-6 lg:p-10 rounded-[2.5rem] border-t-8 border-amber-500 shadow-2xl">
                    <h3 class="font-black text-3xl text-amber-900 mb-8">Input Modal Baru</h3>
                    <div class="space-y-6">
                        <div>
                            <label class="block text-[10px] font-black uppercase text-slate-500 mb-2 ml-1">Nama Sumber Modal</label>
                            <input id="m-sumber" type="text" placeholder="Contoh: Modal Awal Januari" class="w-full p-4 bg-slate-50 border-2 border-slate-200 rounded-2xl font-bold focus:border-amber-500 outline-none transition-all">
                        </div>
                        <div>
                            <label class="block text-[10px] font-black uppercase text-slate-500 mb-2 ml-1">Nominal (Rp)</label>
                            <input id="m-nominal" type="number" placeholder="0" class="w-full p-4 bg-amber-50 border-2 border-amber-200 rounded-2xl font-black text-amber-800 text-xl outline-none focus:ring-4 ring-amber-100">
                        </div>
                        <button onclick="simpan('MODAL')" class="w-full py-5 bg-amber-600 hover:bg-amber-700 text-white font-black rounded-2xl transition-all shadow-xl text-lg mt-4">
                            DAPATKAN MODAL
                        </button>
                    </div>
                </div>
            </div>

            <!-- Halaman Beli TBS -->
            <div id="page-beli" class="page-content hidden">
                <div class="max-w-5xl mx-auto glass-panel p-6 lg:p-10 rounded-[2.5rem] border-t-8 border-emerald-600 shadow-2xl">
                    <h3 class="font-black text-3xl text-emerald-900 mb-8">Transaksi Pembelian TBS</h3>
                    <div class="grid grid-cols-1 lg:grid-cols-2 gap-10">
                        <div class="space-y-6">
                            <div>
                                <label class="text-[10px] font-black text-slate-500 uppercase ml-2">Pemasok / Nama Petani</label>
                                <input id="b-nama" type="text" placeholder="Nama Lengkap" class="w-full p-4 bg-slate-50 border-2 border-slate-200 rounded-2xl font-bold outline-none focus:border-emerald-600">
                            </div>
                            <div class="grid grid-cols-2 gap-4">
                                <div>
                                    <label class="text-[10px] font-black text-slate-500 uppercase ml-2">Brutto (Kg)</label>
                                    <input id="b-bruto" type="number" oninput="hitungBeli()" placeholder="0" class="w-full p-4 bg-slate-50 border-2 border-slate-200 rounded-2xl font-bold outline-none focus:border-emerald-600">
                                </div>
                                <div>
                                    <label class="text-[10px] font-black text-rose-500 uppercase ml-2">Tarra (Kg)</label>
                                    <input id="b-tara" type="number" oninput="hitungBeli()" placeholder="0" class="w-full p-4 bg-slate-50 border-2 border-rose-100 rounded-2xl font-bold text-rose-600 outline-none focus:border-rose-500">
                                </div>
                            </div>
                            <div class="grid grid-cols-2 gap-4">
                                <div>
                                    <label class="text-[10px] font-black text-slate-500 uppercase ml-2">Potongan (%)</label>
                                    <input id="b-persen" type="number" oninput="hitungBeli()" value="3" class="w-full p-4 bg-slate-50 border-2 border-slate-200 rounded-2xl font-bold text-slate-700">
                                </div>
                                <div>
                                    <label class="text-[10px] font-black text-emerald-600 uppercase ml-2">Harga Beli /Kg</label>
                                    <input id="b-harga" type="number" oninput="hitungBeli()" placeholder="Rp" class="w-full p-4 bg-emerald-50 border-2 border-emerald-200 rounded-2xl font-black text-emerald-800 outline-none focus:ring-4 ring-emerald-100">
                                </div>
                            </div>
                        </div>
                        <div class="bg-emerald-900 rounded-[3rem] p-10 text-white flex flex-col justify-center text-center shadow-inner border-4 border-emerald-800">
                            <p class="text-xs font-bold text-emerald-400 mb-2 uppercase tracking-tighter">TOTAL YANG HARUS DIBAYAR</p>
                            <h2 id="res-total" class="text-5xl font-black mb-6">Rp 0</h2>
                            <div class="flex justify-center gap-4">
                                <span class="bg-emerald-800/50 px-4 py-2 rounded-xl text-sm font-bold border border-emerald-700">Netto: <span id="res-netto">0</span> Kg</span>
                            </div>
                            <button onclick="simpan('BELI')" class="w-full py-5 mt-10 bg-emerald-500 hover:bg-emerald-400 rounded-2xl font-black text-xl transition-all shadow-lg shadow-emerald-950">
                                CATAT PEMBELIAN
                            </button>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Halaman Laporan -->
            <div id="page-laporan" class="page-content hidden">
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-6 mb-8">
                    <!-- Laporan Laba Rugi -->
                    <div class="glass-panel p-8 rounded-[2rem] shadow-sm border-t-4 border-emerald-600">
                        <h3 class="font-black text-xl text-slate-900 mb-6 flex justify-between">
                            Laba Rugi <span class="text-xs font-bold text-slate-400 italic" id="pnl-period">Periode Terpilih</span>
                        </h3>
                        <div class="space-y-4">
                            <div class="flex justify-between border-b border-slate-100 pb-2">
                                <span class="text-slate-600 font-bold">Pendapatan Jual</span>
                                <span id="pnl-jual" class="text-blue-700 font-black">Rp 0</span>
                            </div>
                            <div class="flex justify-between border-b border-slate-100 pb-2">
                                <span class="text-slate-600 font-bold text-sm">HPP (Pembelian TBS)</span>
                                <span id="pnl-beli" class="text-rose-600 font-black">Rp 0</span>
                            </div>
                            <div class="flex justify-between bg-emerald-50 p-3 rounded-xl">
                                <span class="text-emerald-800 font-black">Laba Kotor</span>
                                <span id="pnl-kotor" class="text-emerald-800 font-black underline">Rp 0</span>
                            </div>
                            <div class="flex justify-between border-b border-slate-100 pb-2 pt-2">
                                <span class="text-slate-600 font-bold text-sm">Beban Operasional</span>
                                <span id="pnl-biaya" class="text-rose-600 font-black">Rp 0</span>
                            </div>
                            <div class="flex justify-between pt-4">
                                <span class="text-xl font-black text-slate-900">LABA BERSIH</span>
                                <span id="pnl-bersih" class="text-2xl font-black text-emerald-700">Rp 0</span>
                            </div>
                        </div>
                    </div>

                    <!-- Laporan Modal -->
                    <div class="glass-panel p-8 rounded-[2rem] shadow-sm border-t-4 border-amber-500">
                        <h3 class="font-black text-xl text-slate-900 mb-6">Posisi Keuangan</h3>
                        <div class="space-y-4">
                            <div class="flex justify-between border-b border-slate-100 pb-2">
                                <span class="text-slate-600 font-bold">Total Investasi Modal</span>
                                <span id="eq-awal" class="text-amber-700 font-black">Rp 0</span>
                            </div>
                            <div class="flex justify-between border-b border-slate-100 pb-2">
                                <span class="text-slate-600 font-bold">Akumulasi Laba</span>
                                <span id="eq-laba" class="text-emerald-600 font-black">Rp 0</span>
                            </div>
                            <div class="flex justify-between pt-10 border-t-2 border-slate-800 mt-6">
                                <span class="text-xl font-black text-slate-900 uppercase">Nilai Aset Modal</span>
                                <span id="eq-akhir" class="text-2xl font-black text-slate-900">Rp 0</span>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Tabel Riwayat -->
                <div class="flex justify-between items-center mb-6">
                    <h2 class="text-2xl font-black text-slate-900">Jurnal Transaksi</h2>
                    <button onclick="exportExcel()" class="bg-emerald-700 text-white px-6 py-3 rounded-xl font-black hover:bg-emerald-800 flex items-center gap-2 shadow-lg">
                        <i class="fas fa-file-download"></i> EXPORT EXCEL
                    </button>
                </div>
                
                <div class="glass-panel rounded-3xl overflow-hidden shadow-xl mb-20">
                    <div class="overflow-x-auto">
                        <table class="w-full text-sm text-left border-collapse">
                            <thead class="bg-slate-800 text-white">
                                <tr>
                                    <th class="p-4 uppercase text-[10px] font-black">Tgl</th>
                                    <th class="p-4 uppercase text-[10px] font-black">Kategori</th>
                                    <th class="p-4 uppercase text-[10px] font-black">Uraian</th>
                                    <th class="p-4 uppercase text-[10px] font-black text-right">Qty (Kg)</th>
                                    <th class="p-4 uppercase text-[10px] font-black text-right">Nilai Rp</th>
                                    <th class="p-4 uppercase text-[10px] font-black text-center">Opsi</th>
                                </tr>
                            </thead>
                            <tbody id="table-log" class="divide-y divide-slate-200 bg-white">
                                <!-- Data rows -->
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>

            <!-- Placeholder for other pages (simplified for logic) -->
            <div id="page-jual" class="page-content hidden">
                <div class="max-w-xl mx-auto glass-panel p-10 rounded-[2.5rem] border-t-8 border-blue-600 shadow-2xl text-center">
                    <h3 class="font-black text-3xl text-blue-900 mb-8">Penjualan PKS</h3>
                    <div class="space-y-6 text-left">
                        <input id="j-pabrik" type="text" placeholder="Nama Pabrik" class="w-full p-4 bg-slate-50 border-2 rounded-2xl font-bold">
                        <input id="j-netto" type="number" oninput="hitungJual()" placeholder="Netto Pabrik (Kg)" class="w-full p-4 bg-slate-50 border-2 rounded-2xl font-bold">
                        <input id="j-harga" type="number" oninput="hitungJual()" placeholder="Harga Jual /Kg" class="w-full p-4 bg-blue-50 border-2 border-blue-200 rounded-2xl font-black text-blue-800">
                        <div class="py-4 text-2xl font-black text-blue-700 border-y-2 border-blue-50">
                            Total: <span id="res-jual-total">Rp 0</span>
                        </div>
                        <button onclick="simpan('JUAL')" class="w-full py-5 bg-blue-600 text-white font-black rounded-2xl shadow-xl text-lg">POSTING PENJUALAN</button>
                    </div>
                </div>
            </div>

            <div id="page-biaya" class="page-content hidden">
                <div class="max-w-xl mx-auto glass-panel p-10 rounded-[2.5rem] border-t-8 border-rose-600 shadow-2xl">
                    <h3 class="font-black text-3xl text-rose-900 mb-8">Biaya Pengeluaran</h3>
                    <div class="space-y-6">
                        <input id="c-ket" type="text" placeholder="Keterangan (Gaji/BBM/Dll)" class="w-full p-4 bg-slate-50 border-2 rounded-2xl font-bold">
                        <input id="c-nominal" type="number" placeholder="Nominal Rp" class="w-full p-4 bg-rose-50 border-2 border-rose-200 rounded-2xl font-black text-rose-800">
                        <button onclick="simpan('BIAYA')" class="w-full py-5 bg-rose-600 text-white font-black rounded-2xl shadow-xl text-lg">CATAT BIAYA</button>
                    </div>
                </div>
            </div>

        </main>
    </div>

    <!-- Toast Container -->
    <div id="toast-container" class="fixed top-5 right-5 z-[200000] flex flex-col gap-2 pointer-events-none"></div>

    <script>
        let dataStore = JSON.parse(localStorage.getItem('sawit_pro_db')) || [];
        let pendingDeleteId = null;
        let mainChart = null;

        function navTo(id) {
            document.querySelectorAll('.page-content').forEach(p => p.classList.add('hidden'));
            document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
            document.getElementById('page-' + id).classList.remove('hidden');
            document.getElementById('nav-' + id).classList.add('active');
            if(id === 'dashboard') setTimeout(initChart, 100);
            renderAll();
        }

        function hitungBeli() {
            const b = parseFloat(document.getElementById('b-bruto').value) || 0;
            const t = parseFloat(document.getElementById('b-tara').value) || 0;
            const p = parseFloat(document.getElementById('b-persen').value) || 0;
            const h = parseFloat(document.getElementById('b-harga').value) || 0;
            const murni = b - t;
            const netto = Math.round(murni * (1 - p/100));
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

        function showToast(msg, color = 'emerald') {
            const toast = document.createElement('div');
            toast.className = `bg-${color}-700 text-white px-6 py-4 rounded-2xl shadow-2xl font-bold border-2 border-${color}-400 animate-bounce pointer-events-auto`;
            toast.innerHTML = `<i class="fas fa-info-circle mr-2"></i> ${msg}`;
            document.getElementById('toast-container').appendChild(toast);
            setTimeout(() => { toast.style.opacity = '0'; setTimeout(() => toast.remove(), 500); }, 3000);
        }

        function simpan(tipe) {
            let item = { id: Date.now(), ts: new Date().toISOString(), tipe };
            if(tipe === 'BELI') {
                const r = hitungBeli(); if(r.total <= 0) return alert('Input tidak valid');
                item.nama = document.getElementById('b-nama').value || 'Pemasok Umum';
                item.netto = r.netto; item.total = r.total;
            } else if(tipe === 'JUAL') {
                const r = hitungJual(); if(r.total <= 0) return alert('Input tidak valid');
                item.nama = document.getElementById('j-pabrik').value || 'PKS';
                item.netto = r.netto; item.total = r.total;
            } else if(tipe === 'BIAYA') {
                const n = parseFloat(document.getElementById('c-nominal').value) || 0; if(n <= 0) return alert('Input tidak valid');
                item.nama = document.getElementById('c-ket').value || 'Biaya Ops';
                item.netto = 0; item.total = n;
            } else if(tipe === 'MODAL') {
                const n = parseFloat(document.getElementById('m-nominal').value) || 0; if(n <= 0) return alert('Input tidak valid');
                item.nama = document.getElementById('m-sumber').value || 'Investasi Pemilik';
                item.netto = 0; item.total = n;
            }

            dataStore.unshift(item);
            localStorage.setItem('sawit_pro_db', JSON.stringify(dataStore));
            showToast(`Data ${tipe} Berhasil Disimpan!`);
            
            // Clear inputs
            document.querySelectorAll('input:not([type="date"])').forEach(i => { if(i.id !== 'b-persen') i.value = ''; });
            hitungBeli(); hitungJual();
            navTo('dashboard');
        }

        function resetFilter() {
            document.getElementById('filter-start').value = '';
            document.getElementById('filter-end').value = '';
            renderAll();
        }

        function openDeleteModal(id) {
            pendingDeleteId = id;
            document.getElementById('delete-modal').style.display = 'flex';
        }

        function closeDeleteModal() { document.getElementById('delete-modal').style.display = 'none'; }

        document.getElementById('confirm-delete-btn').onclick = () => {
            dataStore = dataStore.filter(d => d.id !== pendingDeleteId);
            localStorage.setItem('sawit_pro_db', JSON.stringify(dataStore));
            closeDeleteModal();
            renderAll();
            showToast('Data dihapus', 'rose');
        };

        function renderAll() {
            const start = document.getElementById('filter-start').value;
            const end = document.getElementById('filter-end').value;
            
            // Filter Data based on Global Date Range
            const filtered = dataStore.filter(d => {
                const dDate = d.ts.split('T')[0];
                if(start && dDate < start) return false;
                if(end && dDate > end) return false;
                return true;
            });

            const table = document.getElementById('table-log');
            table.innerHTML = '';
            
            let br = 0, jr = 0, exp = 0, mod = 0, kg = 0;
            
            filtered.forEach(d => {
                if(d.tipe==='BELI') { br += d.total; kg += d.netto; }
                if(d.tipe==='JUAL') { jr += d.total; kg -= d.netto; }
                if(d.tipe==='BIAYA') { exp += d.total; }
                if(d.tipe==='MODAL') { mod += d.total; }

                const badge = `badge-${d.tipe.toLowerCase()}`;
                table.insertAdjacentHTML('beforeend', `
                    <tr class="hover:bg-slate-50 transition-colors font-bold text-slate-700">
                        <td class="p-4 text-xs font-bold text-slate-400">${new Date(d.ts).toLocaleDateString('id-ID')}</td>
                        <td class="p-4"><span class="px-3 py-1 rounded-lg text-[10px] font-black uppercase ${badge}">${d.tipe}</span></td>
                        <td class="p-4 text-slate-900">${d.nama}</td>
                        <td class="p-4 text-right">${d.netto > 0 ? d.netto.toLocaleString() : '-'}</td>
                        <td class="p-4 text-right font-black text-slate-900">Rp ${d.total.toLocaleString()}</td>
                        <td class="p-4 text-center">
                            <button onclick="openDeleteModal(${d.id})" class="text-slate-300 hover:text-rose-600 transition-colors">
                                <i class="fas fa-trash-alt"></i>
                            </button>
                        </td>
                    </tr>
                `);
            });

            // Financial Calc
            const profit = (jr - br) - exp;
            const cash = (mod + jr) - br - exp;

            document.getElementById('stok-val').innerText = kg.toLocaleString();
            document.getElementById('cash-val').innerText = 'Rp ' + cash.toLocaleString();
            document.getElementById('modal-total-val').innerText = 'Rp ' + mod.toLocaleString();
            document.getElementById('profit-val').innerText = 'Rp ' + profit.toLocaleString();

            document.getElementById('pnl-jual').innerText = 'Rp ' + jr.toLocaleString();
            document.getElementById('pnl-beli').innerText = 'Rp ' + br.toLocaleString();
            document.getElementById('pnl-kotor').innerText = 'Rp ' + (jr - br).toLocaleString();
            document.getElementById('pnl-biaya').innerText = 'Rp ' + exp.toLocaleString();
            document.getElementById('pnl-bersih').innerText = 'Rp ' + profit.toLocaleString();
            document.getElementById('eq-awal').innerText = 'Rp ' + mod.toLocaleString();
            document.getElementById('eq-laba').innerText = 'Rp ' + profit.toLocaleString();
            document.getElementById('eq-akhir').innerText = 'Rp ' + (mod + profit).toLocaleString();
            
            if(mainChart) initChart(filtered);
        }

        function initChart(data = dataStore) {
            const ctx = document.getElementById('chartView').getContext('2d');
            const recent = [...data].reverse().slice(-10);
            
            if(mainChart) mainChart.destroy();
            mainChart = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: recent.map(r => new Date(r.ts).toLocaleDateString('id-ID', {day:'2-digit', month:'short'})),
                    datasets: [
                        {
                            label: 'Volume (Kg)',
                            data: recent.map(r => r.netto),
                            borderColor: '#059669',
                            backgroundColor: 'rgba(5, 150, 105, 0.1)',
                            fill: true,
                            tension: 0.4,
                            yAxisID: 'y'
                        },
                        {
                            label: 'Nilai Transaksi (Rp)',
                            data: recent.map(r => r.total),
                            borderColor: '#3b82f6',
                            borderDash: [5, 5],
                            tension: 0,
                            yAxisID: 'y1'
                        }
                    ]
                },
                options: {
                    maintainAspectRatio: false,
                    interaction: { mode: 'index', intersect: false },
                    plugins: { legend: { position: 'top', labels: { font: { weight: 'bold' } } } },
                    scales: {
                        y: { type: 'linear', position: 'left', grid: { display: false } },
                        y1: { type: 'linear', position: 'right', grid: { color: '#f1f5f9' } }
                    }
                }
            });
        }

        function exportExcel() {
            const data = dataStore.map(d => ({ 
                Tgl: new Date(d.ts).toLocaleDateString(), 
                Kategori: d.tipe, 
                Uraian: d.nama, 
                Kg: d.netto, 
                Nilai_Rp: d.total 
            }));
            const ws = XLSX.utils.json_to_sheet(data);
            const wb = XLSX.utils.book_new();
            XLSX.utils.book_append_sheet(wb, ws, "Laporan");
            XLSX.writeFile(wb, "PalmCore_ERP_Report.xlsx");
        }

        function clearAllData() {
            if(confirm("Hapus seluruh database aplikasi secara permanen?")) {
                localStorage.clear();
                dataStore = [];
                location.reload();
            }
        }

        window.onload = () => { renderAll(); initChart(); };
    </script>
</body>
</html>

