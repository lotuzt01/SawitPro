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
        
        /* Custom Scrollbar */
        ::-webkit-scrollbar { width: 6px; }
        ::-webkit-scrollbar-track { background: #f1f1f1; }
        ::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 10px; }
        ::-webkit-scrollbar-thumb:hover { background: #94a3b8; }
    </style>
</head>
<body class="text-slate-700">

    <!-- Modal Konfirmasi Hapus -->
    <div id="delete-modal" class="modal-overlay">
        <div class="bg-white p-8 rounded-3xl max-w-sm w-full mx-4 shadow-2xl text-center">
            <div class="text-rose-500 mb-4"><i class="fas fa-exclamation-triangle text-5xl"></i></div>
            <h3 class="text-xl font-black mb-2">Hapus Transaksi?</h3>
            <p class="text-slate-500 text-sm mb-6">Data ini akan dihapus permanen dari penyimpanan lokal browser Anda.</p>
            <div class="flex gap-3">
                <button onclick="closeDeleteModal()" class="flex-1 py-3 bg-slate-100 font-bold rounded-xl hover:bg-slate-200 transition-colors">Batal</button>
                <button id="confirm-delete-btn" class="flex-1 py-3 bg-rose-600 text-white font-bold rounded-xl hover:bg-rose-700 transition-colors">Hapus</button>
            </div>
        </div>
    </div>

    <!-- Layout Utama -->
    <div id="app-body">
        <!-- Sidebar Navigation -->
        <nav class="fixed left-0 top-0 h-full w-20 lg:w-64 bg-white border-r border-slate-200 z-50">
            <div class="p-6 flex flex-col h-full">
                <div class="flex items-center gap-3 mb-10">
                    <div class="bg-emerald-600 p-2 rounded-xl text-white shadow-lg shadow-emerald-200"><i class="fas fa-leaf text-xl"></i></div>
                    <span class="font-bold text-xl hidden lg:block text-emerald-900 tracking-tight">PalmCore<span class="text-emerald-500 font-black">ERP</span></span>
                </div>
                
                <div class="space-y-2 flex-1 overflow-y-auto pr-2">
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

        <!-- Main Content Area -->
        <main class="ml-20 lg:ml-64 p-4 lg:p-8 min-h-screen">
            
            <!-- Halaman Dashboard -->
            <div id="page-dashboard" class="page-content">
                <div class="mb-8">
                    <h1 class="text-2xl font-black text-slate-800">Ringkasan Operasional</h1>
                    <p class="text-slate-500 text-sm">Status stok dan kesehatan finansial terkini.</p>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
                    <div class="glass-panel p-6 rounded-3xl border-b-4 border-emerald-500 shadow-sm">
                        <p class="text-[10px] font-bold text-slate-400 uppercase tracking-widest mb-1">Stok Gudang (Kg)</p>
                        <h2 id="stok-val" class="text-3xl font-black text-slate-800">0</h2>
                    </div>
                    <div class="glass-panel p-6 rounded-3xl border-b-4 border-blue-500 shadow-sm">
                        <p class="text-[10px] font-bold text-slate-400 uppercase tracking-widest mb-1">Kas / Tunai Aktif</p>
                        <h2 id="cash-val" class="text-2xl font-black text-blue-600">Rp 0</h2>
                    </div>
                    <div class="glass-panel p-6 rounded-3xl border-b-4 border-amber-500 shadow-sm">
                        <p class="text-[10px] font-bold text-slate-400 uppercase tracking-widest mb-1">Total Investasi Modal</p>
                        <h2 id="modal-total-val" class="text-2xl font-black text-amber-600">Rp 0</h2>
                    </div>
                    <div class="bg-slate-900 p-6 rounded-3xl text-white shadow-xl">
                        <p class="text-[10px] font-bold text-emerald-400 uppercase tracking-widest mb-1">Laba Bersih Berjalan</p>
                        <h2 id="profit-val" class="text-2xl font-black text-emerald-400">Rp 0</h2>
                    </div>
                </div>
                
                <div class="glass-panel p-8 rounded-3xl h-[400px] shadow-sm">
                    <h3 class="font-bold text-slate-800 mb-4 flex items-center gap-2">
                        <i class="fas fa-chart-line text-emerald-500"></i> Tren Volume Transaksi (Kg)
                    </h3>
                    <canvas id="chartView"></canvas>
                </div>
            </div>

            <!-- Halaman Input Modal -->
            <div id="page-modal" class="page-content hidden">
                <div class="max-w-xl mx-auto glass-panel p-8 rounded-[2.5rem] border-t-8 border-amber-500 shadow-xl">
                    <h3 class="font-black text-2xl text-amber-900 mb-6">Input Modal Pemilik</h3>
                    <div class="space-y-6">
                        <div>
                            <label class="block text-[10px] font-black uppercase text-slate-400 mb-2">Nama Pelabur / Sumber</label>
                            <input id="m-sumber" type="text" placeholder="Contoh: Modal Awal Pemilik" class="w-full p

