Quan Li Chi Tiêu Ca Nhan
Quản Lí Chi Tiêu Cá Nhân
<!DOCTYPE html>
<html lang="vi" class="h-full bg-slate-50">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
    <title>Quản Lý Chi Tiêu Cá Nhân - Android VNĐ</title>
    
    <!-- PWA & Mobile Meta Tags -->
    <meta name="theme-color" content="#2563eb">
    <meta name="mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <meta name="apple-mobile-web-app-title" content="Sổ Chi Tiêu">
    <link rel="icon" type="image/png" href="https://cdn-icons-png.flaticon.com/512/2953/2953361.png">

    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&display=swap" rel="stylesheet">
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Plus Jakarta Sans', 'sans-serif'],
                    },
                    colors: {
                        brand: {
                            50: '#eff6ff',
                            100: '#dbeafe',
                            500: '#3b82f6',
                            600: '#2563eb',
                            700: '#1d4ed8',
                        }
                    }
                }
            }
        }
    </script>
    
    <style>
        * {
            -webkit-tap-highlight-color: transparent;
        }
        ::-webkit-scrollbar {
            width: 4px;
            height: 4px;
        }
        ::-webkit-scrollbar-track {
            background: #f1f5f9;
        }
        ::-webkit-scrollbar-thumb {
            background: #cbd5e1;
            border-radius: 4px;
        }
        .recording-pulse {
            animation: pulse-red 1.5s infinite;
        }
        @keyframes pulse-red {
            0% { transform: scale(1); opacity: 1; }
            50% { transform: scale(1.08); opacity: 0.7; }
            100% { transform: scale(1); opacity: 1; }
        }
        .pb-safe {
            padding-bottom: env(safe-area-inset-bottom, 16px);
        }
    </style>
</head>
<body class="font-sans antialiased text-slate-800 min-h-full flex flex-col pb-20 sm:pb-0 select-none">

    <!-- Top Navigation -->
    <header class="bg-white border-b border-slate-200 sticky top-0 z-30 shadow-sm">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between h-16 items-center">
                <div class="flex items-center gap-3">
                    <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-blue-600 to-indigo-600 flex items-center justify-center text-white shadow-md shadow-blue-500/20">
                        <i class="fa-solid font-bold fa-wallet text-xl"></i>
                    </div>
                    <div>
                        <h1 class="font-bold text-lg leading-tight text-slate-900">Sổ Chi Tiêu VNĐ</h1>
                        <p class="text-[11px] text-slate-500 font-medium">Tối ưu cho thiết bị di động</p>
                    </div>
                </div>
                
                <div class="flex items-center gap-2">
                    <button onclick="openTransactionModal()" class="inline-flex items-center gap-2 bg-blue-600 hover:bg-blue-700 text-white px-4 py-2.5 rounded-xl font-semibold text-xs sm:text-sm transition-all shadow-md shadow-blue-600/20 active:scale-95">
                        <i class="fa-solid fa-plus"></i>
                        <span class="hidden sm:inline">Thêm khoản chi</span>
                        <span class="sm:hidden">Thêm</span>
                    </button>
                </div>
            </div>
        </div>
    </header>

    <!-- Main Container -->
    <main class="flex-1 max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-4 sm:py-6 space-y-5">

        <!-- Statistics Summary Cards -->
        <section class="grid grid-cols-2 lg:grid-cols-4 gap-3 sm:gap-4">
            <!-- Today Expense Card -->
            <div class="bg-white p-4 rounded-2xl border border-slate-200 shadow-sm relative overflow-hidden flex flex-col justify-between">
                <div class="flex items-center justify-between mb-2">
                    <span class="text-[11px] font-bold uppercase tracking-wider text-slate-500">Hôm nay</span>
                    <div class="w-7 h-7 rounded-lg bg-emerald-50 text-emerald-600 flex items-center justify-center">
                        <i class="fa-solid fa-calendar-day text-xs"></i>
                    </div>
                </div>
                <div>
                    <div id="stat-today" class="text-lg sm:text-2xl font-extrabold text-slate-900">0 ₫</div>
                    <p id="stat-today-count" class="text-[11px] text-slate-400 mt-0.5">0 giao dịch</p>
                </div>
                <div class="absolute bottom-0 left-0 right-0 h-1 bg-emerald-500"></div>
            </div>

            <!-- Month Expense Card -->
            <div class="bg-white p-4 rounded-2xl border border-slate-200 shadow-sm relative overflow-hidden flex flex-col justify-between">
                <div class="flex items-center justify-between mb-2">
                    <span class="text-[11px] font-bold uppercase tracking-wider text-slate-500">Tháng này</span>
                    <div class="w-7 h-7 rounded-lg bg-blue-50 text-blue-600 flex items-center justify-center">
                        <i class="fa-solid fa-calendar-days text-xs"></i>
                    </div>
                </div>
                <div>
                    <div id="stat-month" class="text-lg sm:text-2xl font-extrabold text-slate-900">0 ₫</div>
                    <p id="stat-month-count" class="text-[11px] text-slate-400 mt-0.5">0 giao dịch</p>
                </div>
                <div class="absolute bottom-0 left-0 right-0 h-1 bg-blue-500"></div>
            </div>

            <!-- Year Expense Card -->
            <div class="bg-white p-4 rounded-2xl border border-slate-200 shadow-sm relative overflow-hidden flex flex-col justify-between">
                <div class="flex items-center justify-between mb-2">
                    <span class="text-[11px] font-bold uppercase tracking-wider text-slate-500">Năm nay</span>
                    <div class="w-7 h-7 rounded-lg bg-indigo-50 text-indigo-600 flex items-center justify-center">
                        <i class="fa-solid fa-calendar-check text-xs"></i>
                    </div>
                </div>
                <div>
                    <div id="stat-year" class="text-lg sm:text-2xl font-extrabold text-slate-900">0 ₫</div>
                    <p id="stat-year-count" class="text-[11px] text-slate-400 mt-0.5">0 giao dịch</p>
                </div>
                <div class="absolute bottom-0 left-0 right-0 h-1 bg-indigo-500"></div>
            </div>

            <!-- Media Attachments Count -->
            <div onclick="filterByMediaShortcut()" class="bg-white p-4 rounded-2xl border border-slate-200 shadow-sm relative overflow-hidden flex flex-col justify-between cursor-pointer hover:border-purple-300 hover:shadow-md transition-all active:scale-95 group">
                <div class="flex items-center justify-between mb-2">
                    <span class="text-[11px] font-bold uppercase tracking-wider text-slate-500 group-hover:text-purple-600 transition-colors">Bằng chứng Ảnh/Video</span>
                    <div class="w-7 h-7 rounded-lg bg-purple-50 text-purple-600 flex items-center justify-center group-hover:bg-purple-100 transition-colors">
                        <i class="fa-solid fa-camera text-xs"></i>
                    </div>
                </div>
                <div>
                    <div id="stat-media" class="text-lg sm:text-2xl font-extrabold text-slate-900 group-hover:text-purple-700 transition-colors">0</div>
                    <p class="text-[11px] text-slate-400 mt-0.5">Nhấn để lọc xem hóa đơn</p>
                </div>
                <div class="absolute bottom-0 left-0 right-0 h-1 bg-purple-500"></div>
            </div>
        </section>

        <!-- Content Area: List & Analytics Grid -->
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-5">

            <!-- Expense List & Filters -->
            <div class="lg:col-span-2 space-y-4">
                
                <!-- Filter Toolbar -->
                <div class="bg-white p-4 rounded-2xl border border-slate-200 shadow-sm space-y-3">
                    <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-3">
                        <h2 class="font-bold text-slate-900 text-sm sm:text-base flex items-center gap-2">
                            <i class="fa-solid fa-receipt text-blue-600"></i>
                            Danh sách chi tiêu
                        </h2>
                        <!-- Search Box -->
                        <div class="relative flex-1 sm:max-w-xs">
                            <i class="fa-solid fa-magnifying-glass absolute left-3 top-1/2 -translate-y-1/2 text-slate-400 text-xs"></i>
                            <input type="text" id="search-input" oninput="renderTransactions()" placeholder="Tìm theo tên khoản chi..." class="w-full pl-9 pr-3 py-2 bg-slate-50 border border-slate-200 rounded-xl text-xs focus:outline-none focus:border-blue-500 transition-colors">
                        </div>
                    </div>

                    <!-- Filter Controls -->
                    <div class="grid grid-cols-2 sm:grid-cols-4 gap-2 pt-2 border-t border-slate-100">
                        <div>
                            <label class="block text-[10px] font-bold uppercase text-slate-400 mb-1">Thời gian</label>
                            <select id="filter-period" onchange="handlePeriodChange()" class="w-full bg-slate-50 border border-slate-200 text-slate-700 text-xs rounded-xl px-2.5 py-2 focus:outline-none focus:border-blue-500">
                                <option value="all">Tất cả</option>
                                <option value="day">Theo Ngày</option>
                                <option value="month">Theo Tháng</option>
                                <option value="year">Theo Năm</option>
                            </select>
                        </div>

                        <!-- Date inputs -->
                        <div id="filter-date-container" class="hidden">
                            <label class="block text-[10px] font-bold uppercase text-slate-400 mb-1">Chọn ngày/tháng</label>
                            <input type="date" id="filter-date-val" onchange="renderTransactions()" class="w-full bg-slate-50 border border-slate-200 text-slate-700 text-xs rounded-xl px-2 py-1.5 focus:outline-none focus:border-blue-500">
                        </div>

                        <div>
                            <label class="block text-[10px] font-bold uppercase text-slate-400 mb-1">Danh mục</label>
                            <select id="filter-category" onchange="renderTransactions()" class="w-full bg-slate-50 border border-slate-200 text-slate-700 text-xs rounded-xl px-2.5 py-2 focus:outline-none focus:border-blue-500">
                                <option value="all">Tất cả danh mục</option>
                                <option value="Ăn uống">🍲 Ăn uống</option>
                                <option value="Di chuyển">🚗 Di chuyển</option>
                                <option value="Mua sắm">🛍️ Mua sắm</option>
                                <option value="Hóa đơn">⚡ Hóa đơn</option>
                                <option value="Giải trí">🎬 Giải trí</option>
                                <option value="Sức khỏe">💊 Sức khỏe</option>
                                <option value="Khác">📌 Khác</option>
                            </select>
                        </div>

                        <div>
                            <label class="block text-[10px] font-bold uppercase text-slate-400 mb-1">Tệp đính kèm</label>
                            <select id="filter-media" onchange="renderTransactions()" class="w-full bg-slate-50 border border-slate-200 text-slate-700 text-xs rounded-xl px-2.5 py-2 focus:outline-none focus:border-blue-500">
                                <option value="all">Tất cả</option>
                                <option value="has_media">Có Ảnh/Video</option>
                                <option value="no_media">Không có</option>
                            </select>
                        </div>
                    </div>
                </div>

                <!-- Transaction Summary for Filtered View -->
                <div class="bg-blue-50/80 border border-blue-100 rounded-xl px-4 py-2.5 flex items-center justify-between text-xs text-blue-800 font-medium">
                    <div>
                        <span>Tổng chi hiển thị:</span>
                        <span id="filtered-count" class="font-bold ml-1 text-blue-900">0 khoản</span>
                    </div>
                    <div id="filtered-total" class="font-extrabold text-sm text-blue-900">0 ₫</div>
                </div>

                <!-- Transactions List Container -->
                <div id="transaction-list" class="space-y-2.5">
                    <!-- Dynamic transactions will render here -->
                </div>

                <!-- Empty state -->
                <div id="empty-state" class="hidden bg-white p-8 text-center rounded-2xl border border-slate-200">
                    <div class="w-16 h-16 bg-slate-100 text-slate-400 rounded-full flex items-center justify-center mx-auto mb-3">
                        <i class="fa-solid fa-wallet text-2xl"></i>
                    </div>
                    <h3 class="font-bold text-slate-700 text-sm">Chưa có giao dịch chi tiêu</h3>
                    <p class="text-xs text-slate-400 mt-1 max-w-xs mx-auto">Chạm nút "Thêm khoản chi" để bắt đầu ghi lại các khoản tiêu dùng hàng ngày tính bằng VNĐ.</p>
                </div>

            </div>

            <!-- Analytics & Charts Sidebar -->
            <div class="space-y-4">
                <div class="bg-white p-5 rounded-2xl border border-slate-200 shadow-sm">
                    <h2 class="font-bold text-slate-900 mb-3 flex items-center gap-2 text-sm sm:text-base">
                        <i class="fa-solid fa-chart-pie text-indigo-600"></i>
                        Tỷ lệ Chi Tiêu Theo Danh Mục
                    </h2>
                    <div class="relative flex items-center justify-center aspect-square max-h-[240px] mx-auto">
                        <canvas id="categoryChart"></canvas>
                    </div>
                    <div id="chart-legend" class="mt-4 space-y-2 text-xs divide-y divide-slate-100">
                        <!-- Dynamic legend items -->
                    </div>
                </div>

                <!-- Backup & Utility Card -->
                <div class="bg-slate-900 text-white p-5 rounded-2xl shadow-sm space-y-3">
                    <div class="flex items-center justify-between">
                        <div class="flex items-center gap-2">
                            <i class="fa-solid fa-hard-drive text-emerald-400 text-lg"></i>
                            <h3 class="font-bold text-sm">Bộ Nhớ & Tiện Ích</h3>
                        </div>
                        <span id="storage-usage-tag" class="text-[10px] bg-slate-800 text-emerald-400 font-mono px-2 py-0.5 rounded-full border border-slate-700">0 MB</span>
                    </div>
                    <p class="text-[11px] text-slate-400">Ghi chú nhanh & chụp hóa đơn lưu trữ trực tiếp trên thiết bị di động.</p>
                    
                    <div class="grid grid-cols-2 gap-2 pt-1">
                        <button onclick="openCalculator()" class="bg-slate-800 hover:bg-slate-700 active:bg-slate-700 border border-slate-700 text-xs font-semibold py-2.5 px-3 rounded-xl flex items-center justify-center gap-1.5 transition-colors">
                            <i class="fa-solid fa-calculator text-blue-400"></i> Máy Tính
                        </button>
                        <button onclick="openNotesModal()" class="bg-slate-800 hover:bg-slate-700 active:bg-slate-700 border border-slate-700 text-xs font-semibold py-2.5 px-3 rounded-xl flex items-center justify-center gap-1.5 transition-colors">
                            <i class="fa-solid fa-note-sticky text-amber-400"></i> Ghi Chú
                        </button>
                    </div>

                    <div class="grid grid-cols-2 gap-2">
                        <button onclick="openQuickCameraExpense()" class="bg-slate-800 hover:bg-slate-700 active:bg-slate-700 border border-slate-700 text-xs font-semibold py-2.5 px-3 rounded-xl flex items-center justify-center gap-1.5 transition-colors text-indigo-300">
                            <i class="fa-solid fa-camera text-indigo-400"></i> Chụp Chi Tiêu
                        </button>
                        <button onclick="confirmClearAllData()" class="bg-red-500/10 hover:bg-red-500/20 active:bg-red-500/20 text-red-400 border border-red-500/20 text-xs font-semibold py-2.5 px-3 rounded-xl flex items-center justify-center gap-1.5 transition-colors">
                            <i class="fa-solid fa-trash-can"></i> Xóa Dữ Liệu
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </main>

    <!-- Modal: Notes Management -->
    <div id="notes-modal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-white w-full max-w-md rounded-2xl shadow-2xl overflow-hidden flex flex-col max-h-[85vh]">
            <!-- Header -->
            <div class="px-5 py-3.5 border-b border-slate-100 flex items-center justify-between bg-slate-50/50">
                <div class="flex items-center gap-2">
                    <div class="w-8 h-8 rounded-lg bg-amber-100 text-amber-600 flex items-center justify-center">
                        <i class="fa-solid fa-note-sticky text-sm"></i>
                    </div>
                    <h3 class="font-bold text-slate-900 text-base">Ghi Chú Cá Nhân</h3>
                </div>
                <button onclick="closeNotesModal()" class="text-slate-400 hover:text-slate-600 w-8 h-8 rounded-full bg-slate-100 flex items-center justify-center transition-colors">
                    <i class="fa-solid fa-xmark text-sm"></i>
                </button>
            </div>

            <!-- Add Note Form -->
            <div class="p-4 border-b border-slate-100 bg-slate-50/30">
                <form onsubmit="handleAddNote(event)" class="space-y-2">
                    <textarea id="new-note-text" required rows="2" placeholder="Nhập ghi chú mới (ví dụ: Cần mua sữa, Dự trù tiền điện tháng này...)" class="w-full px-3 py-2 bg-white border border-slate-200 rounded-xl text-xs sm:text-sm focus:outline-none focus:border-amber-500 resize-none"></textarea>
                    <div class="flex justify-end">
                        <button type="submit" class="px-4 py-2 bg-amber-500 hover:bg-amber-600 active:scale-95 text-white font-bold text-xs rounded-xl shadow-md shadow-amber-500/20 transition-all flex items-center gap-1.5">
                            <i class="fa-solid fa-plus"></i> Thêm ghi chú
                        </button>
                    </div>
                </form>
            </div>

            <!-- Notes List -->
            <div id="notes-list" class="p-4 overflow-y-auto space-y-2.5 flex-1 max-h-[400px]">
                <!-- Dynamic notes will be rendered here -->
            </div>
        </div>
    </div>

    <!-- Modal: Add/Edit Expense -->
    <div id="transaction-modal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 flex items-end sm:items-center justify-center p-0 sm:p-4 hidden">
        <div class="bg-white w-full max-w-lg rounded-t-3xl sm:rounded-2xl shadow-2xl overflow-hidden max-h-[92vh] flex flex-col transition-all transform">
            
            <!-- Modal Header -->
            <div class="px-5 py-3.5 border-b border-slate-100 flex items-center justify-between bg-slate-50/50">
                <div class="flex items-center gap-2">
                    <div class="w-2 h-6 bg-blue-600 rounded-full"></div>
                    <h3 id="modal-title" class="font-bold text-slate-900 text-base">Thêm khoản chi mới (VNĐ)</h3>
                </div>
                <button onclick="closeTransactionModal()" class="text-slate-400 hover:text-slate-600 w-8 h-8 rounded-full bg-slate-100 flex items-center justify-center transition-colors">
                    <i class="fa-solid fa-xmark text-sm"></i>
                </button>
            </div>

            <!-- Modal Body Form -->
            <form id="expense-form" onsubmit="handleFormSubmit(event)" class="p-5 overflow-y-auto space-y-4 flex-1">
                <input type="hidden" id="edit-id" value="">
                
                <!-- Amount Field -->
                <div>
                    <label class="block text-xs font-bold uppercase tracking-wider text-slate-600 mb-1">Số tiền chi (VNĐ) <span class="text-red-500">*</span></label>
                    <div class="relative">
                        <input type="number" id="form-amount" required min="1000" step="1000" placeholder="0" class="w-full pl-4 pr-12 py-3 bg-slate-50 border border-slate-200 rounded-xl text-xl font-extrabold text-slate-900 focus:outline-none focus:border-blue-500">
                        <span class="absolute right-4 top-1/2 -translate-y-1/2 text-slate-500 font-bold text-sm">₫</span>
                    </div>

                    <!-- Quick VNĐ Amount Buttons for Android Touch Input -->
                    <div class="flex flex-wrap gap-1.5 mt-2">
                        <button type="button" onclick="addAmountPreset(10000)" class="px-2.5 py-1 bg-slate-100 hover:bg-slate-200 text-slate-700 text-[11px] font-semibold rounded-lg transition-colors">+10k</button>
                        <button type="button" onclick="addAmountPreset(20000)" class="px-2.5 py-1 bg-slate-100 hover:bg-slate-200 text-slate-700 text-[11px] font-semibold rounded-lg transition-colors">+20k</button>
                        <button type="button" onclick="addAmountPreset(50000)" class="px-2.5 py-1 bg-slate-100 hover:bg-slate-200 text-slate-700 text-[11px] font-semibold rounded-lg transition-colors">+50k</button>
                        <button type="button" onclick="addAmountPreset(100000)" class="px-2.5 py-1 bg-slate-100 hover:bg-slate-200 text-slate-700 text-[11px] font-semibold rounded-lg transition-colors">+100k</button>
                        <button type="button" onclick="addAmountPreset(200000)" class="px-2.5 py-1 bg-slate-100 hover:bg-slate-200 text-slate-700 text-[11px] font-semibold rounded-lg transition-colors">+200k</button>
                        <button type="button" onclick="addAmountPreset(500000)" class="px-2.5 py-1 bg-slate-100 hover:bg-slate-200 text-slate-700 text-[11px] font-semibold rounded-lg transition-colors">+500k</button>
                        <button type="button" onclick="resetAmountInput()" class="px-2.5 py-1 bg-red-50 hover:bg-red-100 text-red-600 text-[11px] font-semibold rounded-lg transition-colors">Đặt lại</button>
                    </div>
                </div>

                <!-- Title / Note -->
                <div>
                    <label class="block text-xs font-bold uppercase tracking-wider text-slate-600 mb-1">Mục chi tiêu / Ghi chú <span class="text-red-500">*</span></label>
                    <input type="text" id="form-title" required placeholder="Ví dụ: Ăn trưa phở bò, Mua quà sinh nhật, Đổ xăng xe..." class="w-full px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-xs sm:text-sm focus:outline-none focus:border-blue-500">
                </div>

                <!-- Category & Date Grid -->
                <div class="grid grid-cols-1 sm:grid-cols-2 gap-3">
                    <div>
                        <label class="block text-xs font-bold uppercase tracking-wider text-slate-600 mb-1">Phân loại <span class="text-red-500">*</span></label>
                        <select id="form-category" required class="w-full px-3 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-xs sm:text-sm focus:outline-none focus:border-blue-500">
                            <option value="Ăn uống">🍲 Ăn uống</option>
                            <option value="Di chuyển">🚗 Di chuyển</option>
                            <option value="Mua sắm">🛍️ Mua sắm</option>
                            <option value="Hóa đơn">⚡ Hóa đơn & Tiện ích</option>
                            <option value="Giải trí">🎬 Giải trí</option>
                            <option value="Sức khỏe">💊 Sức khỏe</option>
                            <option value="Khác">📌 Khác</option>
                        </select>
                    </div>

                    <div>
                        <label class="block text-xs font-bold uppercase tracking-wider text-slate-600 mb-1">Thời gian chi <span class="text-red-500">*</span></label>
                        <input type="datetime-local" id="form-datetime" required class="w-full px-3 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-xs sm:text-sm focus:outline-none focus:border-blue-500">
                    </div>
                </div>

                <!-- Photo & Video Attachment Section -->
                <div class="border-t border-slate-100 pt-3">
                    <label class="block text-xs font-bold uppercase tracking-wider text-slate-600 mb-2">Đính kèm ảnh / video hóa đơn (Camera Điện Thoại)</label>

                    <!-- Preview box -->
                    <div id="media-preview-box" class="hidden mb-3 relative rounded-xl overflow-hidden border border-slate-200 bg-slate-950 group">
                        <div id="media-preview-content" class="max-h-40 flex items-center justify-center"></div>
                        <button type="button" onclick="clearAttachedMedia()" class="absolute top-2 right-2 bg-red-600 text-white rounded-full w-7 h-7 flex items-center justify-center text-xs shadow-md hover:bg-red-700 transition-colors">
                            <i class="fa-solid fa-trash"></i>
                        </button>
                    </div>

                    <!-- Media Input Actions -->
                    <div id="media-action-buttons" class="grid grid-cols-3 gap-2">
                        <button type="button" onclick="openCameraModal('photo')" class="flex flex-col items-center justify-center p-2.5 border border-dashed border-slate-300 rounded-xl hover:bg-blue-50 hover:border-blue-400 text-slate-700 text-[11px] font-semibold transition-all">
                            <i class="fa-solid fa-camera text-blue-600 text-base mb-1"></i> Camera Trực Tiếp
                        </button>
                        <button type="button" onclick="openCameraModal('video')" class="flex flex-col items-center justify-center p-2.5 border border-dashed border-slate-300 rounded-xl hover:bg-indigo-50 hover:border-indigo-400 text-slate-700 text-[11px] font-semibold transition-all">
                            <i class="fa-solid fa-video text-indigo-600 text-base mb-1"></i> Quay Video
                        </button>
                        <label class="flex flex-col items-center justify-center p-2.5 border border-dashed border-slate-300 rounded-xl hover:bg-slate-100 text-slate-700 text-[11px] font-semibold cursor-pointer transition-all">
                            <i class="fa-solid fa-image text-slate-500 text-base mb-1"></i> Thư Viện Ảnh
                            <input type="file" id="form-file-input" accept="image/*,video/*" capture="environment" onchange="handleFileUpload(event)" class="hidden">
                        </label>
                    </div>
                    <input type="hidden" id="form-media-data" value="">
                    <input type="hidden" id="form-media-type" value="">
                </div>

                <!-- Submit Buttons -->
                <div class="pt-3 border-t border-slate-100 flex items-center justify-end gap-2">
                    <button type="button" onclick="closeTransactionModal()" class="px-4 py-2.5 text-xs font-semibold text-slate-600 hover:bg-slate-100 rounded-xl transition-colors">Hủy</button>
                    <button type="submit" class="px-5 py-2.5 bg-blue-600 hover:bg-blue-700 active:scale-95 text-white font-bold text-xs sm:text-sm rounded-xl shadow-md shadow-blue-600/20 transition-all">Lưu khoản chi</button>
                </div>
            </form>
        </div>
    </div>

    <!-- Modal: Camera Capture -->
    <div id="camera-modal" class="fixed inset-0 bg-slate-950/95 z-50 flex items-center justify-center p-3 hidden">
        <div class="bg-slate-900 w-full max-w-lg rounded-2xl overflow-hidden flex flex-col border border-slate-800 shadow-2xl">
            <!-- Header -->
            <div class="px-4 py-3 border-b border-slate-800 flex justify-between items-center text-white">
                <span id="camera-modal-title" class="font-bold text-xs sm:text-sm flex items-center gap-2">
                    <i class="fa-brands fa-android text-emerald-400"></i> Camera Điện Thoại
                </span>
                <div class="flex items-center gap-2">
                    <button type="button" onclick="switchCameraFacing()" title="Chuyển Camera Trước/Sau" class="text-slate-300 hover:text-white bg-slate-800 hover:bg-slate-700 px-2.5 py-1 rounded-lg text-xs font-semibold flex items-center gap-1 transition-colors">
                        <i class="fa-solid fa-rotate"></i> Đổi Camera
                    </button>
                    <button onclick="closeCameraModal()" class="text-slate-400 hover:text-white w-8 h-8 rounded-full flex items-center justify-center">
                        <i class="fa-solid fa-xmark text-lg"></i>
                    </button>
                </div>
            </div>

            <!-- Camera Viewfinder -->
            <div class="relative bg-black flex items-center justify-center min-h-[300px] overflow-hidden">
                <video id="camera-stream" autoplay playsinline muted class="w-full h-full object-cover max-h-[55vh]"></video>
                <canvas id="camera-canvas" class="hidden"></canvas>
                
                <!-- Video recording overlay -->
                <div id="recording-badge" class="hidden absolute top-3 left-3 bg-red-600 text-white text-xs font-bold px-3 py-1 rounded-full flex items-center gap-2 recording-pulse shadow-lg">
                    <span class="w-2 h-2 rounded-full bg-white"></span>
                    <span>Đang quay (<span id="recording-timer">00:00</span>)</span>
                </div>
            </div>

            <!-- Camera Controls -->
            <div class="p-4 bg-slate-900 flex items-center justify-center gap-3">
                <button type="button" id="btn-take-photo" onclick="takePhoto()" class="bg-white hover:bg-slate-200 text-slate-900 font-extrabold px-5 py-2.5 rounded-full text-xs sm:text-sm shadow-md flex items-center gap-2 transition-transform active:scale-95">
                    <i class="fa-solid fa-camera"></i> Chụp Hóa Đơn
                </button>

                <button type="button" id="btn-start-video" onclick="startVideoRecording()" class="bg-red-600 hover:bg-red-700 text-white font-extrabold px-5 py-2.5 rounded-full text-xs sm:text-sm shadow-md flex items-center gap-2 transition-transform active:scale-95">
                    <i class="fa-solid fa-circle text-xs"></i> Bắt Đầu Quay
                </button>

                <button type="button" id="btn-stop-video" onclick="stopVideoRecording()" class="hidden bg-slate-700 hover:bg-slate-600 text-white font-extrabold px-5 py-2.5 rounded-full text-xs sm:text-sm shadow-md flex items-center gap-2">
                    <i class="fa-solid fa-square text-xs"></i> Dừng Quay
                </button>
            </div>
        </div>
    </div>

    <!-- Modal: Lightbox Media Preview -->
    <div id="lightbox-modal" class="fixed inset-0 bg-slate-950/90 backdrop-blur-md z-50 flex items-center justify-center p-3 hidden" onclick="closeLightboxModal()">
        <div class="relative max-w-3xl max-h-[90vh] w-full flex flex-col items-center justify-center" onclick="event.stopPropagation()">
            <button onclick="closeLightboxModal()" class="absolute -top-12 right-0 text-white bg-slate-800/80 hover:bg-slate-700 w-10 h-10 rounded-full flex items-center justify-center shadow-lg">
                <i class="fa-solid fa-xmark text-lg"></i>
            </button>
            <div id="lightbox-container" class="w-full flex items-center justify-center rounded-2xl overflow-hidden bg-black/40 border border-slate-800">
                <!-- Media content injected here -->
            </div>
        </div>
    </div>

    <!-- Modal: Financial Calculator -->
    <div id="calculator-modal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-slate-900 text-white w-full max-w-xs rounded-3xl p-5 shadow-2xl border border-slate-800 space-y-4">
            <div class="flex items-center justify-between border-b border-slate-800 pb-2">
                <div class="flex items-center gap-2 text-blue-400 font-bold text-sm">
                    <i class="fa-solid fa-calculator"></i> Máy Tính Chi Tiêu
                </div>
                <button onclick="closeCalculator()" class="text-slate-400 hover:text-white w-7 h-7 rounded-full flex items-center justify-center">
                    <i class="fa-solid fa-xmark"></i>
                </button>
            </div>

            <div class="bg-slate-950 p-3 rounded-2xl text-right">
                <div id="calc-expression" class="text-xs text-slate-400 min-h-[16px]"></div>
                <div id="calc-display" class="text-2xl font-extrabold text-white truncate">0</div>
            </div>

            <div class="grid grid-cols-4 gap-2">
                <button onclick="calcInput('C')" class="col-span-2 bg-red-500/20 text-red-400 font-bold py-3 rounded-xl hover:bg-red-500/30">AC</button>
                <button onclick="calcInput('/')" class="bg-slate-800 text-blue-400 font-bold py-3 rounded-xl hover:bg-slate-700">÷</button>
                <button onclick="calcInput('*')" class="bg-slate-800 text-blue-400 font-bold py-3 rounded-xl hover:bg-slate-700">×</button>

                <button onclick="calcInput('7')" class="bg-slate-800 font-bold py-3 rounded-xl hover:bg-slate-700">7</button>
                <button onclick="calcInput('8')" class="bg-slate-800 font-bold py-3 rounded-xl hover:bg-slate-700">8</button>
                <button onclick="calcInput('9')" class="bg-slate-800 font-bold py-3 rounded-xl hover:bg-slate-700">9</button>
                <button onclick="calcInput('-')" class="bg-slate-800 text-blue-400 font-bold py-3 rounded-xl hover:bg-slate-700">-</button>

                <button onclick="calcInput('4')" class="bg-slate-800 font-bold py-3 rounded-xl hover:bg-slate-700">4</button>
                <button onclick="calcInput('5')" class="bg-slate-800 font-bold py-3 rounded-xl hover:bg-slate-700">5</button>
                <button onclick="calcInput('6')" class="bg-slate-800 font-bold py-3 rounded-xl hover:bg-slate-700">6</button>
                <button onclick="calcInput('+')" class="bg-slate-800 text-blue-400 font-bold py-3 rounded-xl hover:bg-slate-700">+</button>

                <button onclick="calcInput('1')" class="bg-slate-800 font-bold py-3 rounded-xl hover:bg-slate-700">1</button>
                <button onclick="calcInput('2')" class="bg-slate-800 font-bold py-3 rounded-xl hover:bg-slate-700">2</button>
                <button onclick="calcInput('3')" class="bg-slate-800 font-bold py-3 rounded-xl hover:bg-slate-700">3</button>
                <button onclick="calcInput('=')" class="row-span-2 bg-blue-600 text-white font-bold py-3 rounded-xl hover:bg-blue-500">=</button>

                <button onclick="calcInput('0')" class="col-span-2 bg-slate-800 font-bold py-3 rounded-xl hover:bg-slate-700">0</button>
                <button onclick="calcInput('000')" class="bg-slate-800 font-bold py-3 rounded-xl hover:bg-slate-700 text-xs">000</button>
            </div>

            <button onclick="applyCalculatorToForm()" class="w-full bg-emerald-600 hover:bg-emerald-500 text-white font-bold py-2.5 rounded-xl text-xs flex items-center justify-center gap-1.5 transition-colors">
                <i class="fa-solid fa-check"></i> Áp dụng vào số tiền chi
            </button>
        </div>
    </div>

    <!-- Custom Modal Dialog / Confirmation Overlay -->
    <div id="confirm-modal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-white max-w-sm w-full rounded-2xl p-5 shadow-2xl space-y-4 text-center">
            <div id="confirm-icon" class="w-12 h-12 bg-red-100 text-red-600 rounded-full flex items-center justify-center mx-auto text-xl">
                <i class="fa-solid fa-triangle-exclamation"></i>
            </div>
            <div>
                <h4 id="confirm-title" class="font-bold text-slate-900 text-base">Xác nhận</h4>
                <p id="confirm-message" class="text-xs text-slate-500 mt-1">Bạn có chắc chắn muốn thực hiện thao tác này?</p>
            </div>
            <div class="flex items-center justify-center gap-2 pt-2">
                <button id="confirm-btn-cancel" class="flex-1 py-2.5 bg-slate-100 hover:bg-slate-200 text-slate-700 font-semibold text-xs rounded-xl transition-colors">Hủy</button>
                <button id="confirm-btn-ok" class="flex-1 py-2.5 bg-red-600 hover:bg-red-700 text-white font-bold text-xs rounded-xl shadow-md transition-colors">Đồng ý</button>
            </div>
        </div>
    </div>

    <!-- Toast Alert Container -->
    <div id="toast-container" class="fixed top-4 right-4 z-50 flex flex-col gap-2 pointer-events-none max-w-xs w-full"></div>

    <script>
        // Global App State
        let db = null;
        let transactions = [];
        let notes = [];
        let isQuickCameraMode = false;
        let categoryChart = null;
        let cameraStream = null;
        let mediaRecorder = null;
        let recordedChunks = [];
        let recordTimerInterval = null;
        let recordSeconds = 0;
        let currentCameraFacing = 'environment';
        let activeCameraMode = 'photo'; // 'photo' or 'video'

        // Category Colors Mapping
        const CATEGORY_COLORS = {
            'Ăn uống': '#ef4444',
            'Di chuyển': '#f59e0b',
            'Mua sắm': '#ec4899',
            'Hóa đơn': '#3b82f6',
            'Giải trí': '#8b5cf6',
            'Sức khỏe': '#10b981',
            'Khác': '#64748b'
        };

        const CATEGORY_ICONS = {
            'Ăn uống': '🍲',
            'Di chuyển': '🚗',
            'Mua sắm': '🛍️',
            'Hóa đơn': '⚡',
            'Giải trí': '🎬',
            'Sức khỏe': '💊',
            'Khác': '📌'
        };

        // IndexedDB Initialization
        function initDB() {
            return new Promise((resolve, reject) => {
                const request = indexedDB.open("VNDSpendTrackerDB", 2);
                request.onupgradeneeded = (e) => {
                    const database = e.target.result;
                    if (!database.objectStoreNames.contains("transactions")) {
                        const store = database.createObjectStore("transactions", { keyPath: "id" });
                        store.createIndex("timestamp", "timestamp", { unique: false });
                    }
                    if (!database.objectStoreNames.contains("notes")) {
                        database.createObjectStore("notes", { keyPath: "id" });
                    }
                };
                request.onsuccess = (e) => {
                    db = e.target.result;
                    resolve(db);
                };
                request.onerror = (e) => {
                    console.error("IndexedDB initialization error:", e);
                    showToast("Không thể mở cơ sở dữ liệu bộ nhớ!", "error");
                    reject(e);
                };
            });
        }

        // DB Operations for Notes
        function getAllNotesFromDB() {
            return new Promise((resolve, reject) => {
                if (!db) return resolve([]);
                const transaction = db.transaction(["notes"], "readonly");
                const store = transaction.objectStore("notes");
                const request = store.getAll();
                request.onsuccess = () => resolve(request.result || []);
                request.onerror = (e) => reject(e);
            });
        }

        function saveNoteToDB(item) {
            return new Promise((resolve, reject) => {
                if (!db) return reject("Database not ready");
                const transaction = db.transaction(["notes"], "readwrite");
                const store = transaction.objectStore("notes");
                const request = store.put(item);
                request.onsuccess = () => resolve();
                request.onerror = (e) => reject(e);
            });
        }

        function deleteNoteFromDB(id) {
            return new Promise((resolve, reject) => {
                if (!db) return reject("Database not ready");
                const transaction = db.transaction(["notes"], "readwrite");
                const store = transaction.objectStore("notes");
                const request = store.delete(id);
                request.onsuccess = () => resolve();
                request.onerror = (e) => reject(e);
            });
        }

        // DB Operations for Transactions
        function getAllTransactionsFromDB() {
            return new Promise((resolve, reject) => {
                if (!db) return resolve([]);
                const transaction = db.transaction(["transactions"], "readonly");
                const store = transaction.objectStore("transactions");
                const request = store.getAll();
                request.onsuccess = () => resolve(request.result || []);
                request.onerror = (e) => reject(e);
            });
        }

        function saveTransactionToDB(item) {
            return new Promise((resolve, reject) => {
                if (!db) return reject("Database not ready");
                const transaction = db.transaction(["transactions"], "readwrite");
                const store = transaction.objectStore("transactions");
                const request = store.put(item);
                request.onsuccess = () => resolve();
                request.onerror = (e) => reject(e);
            });
        }

        function deleteTransactionFromDB(id) {
            return new Promise((resolve, reject) => {
                if (!db) return reject("Database not ready");
                const transaction = db.transaction(["transactions"], "readwrite");
                const store = transaction.objectStore("transactions");
                const request = store.delete(id);
                request.onsuccess = () => resolve();
                request.onerror = (e) => reject(e);
            });
        }

        function clearAllDataFromDB() {
            return new Promise((resolve, reject) => {
                if (!db) return reject("Database not ready");
                const transaction = db.transaction(["transactions"], "readwrite");
                const store = transaction.objectStore("transactions");
                const request = store.clear();
                request.onsuccess = () => resolve();
                request.onerror = (e) => reject(e);
            });
        }

        window.addEventListener('DOMContentLoaded', async () => {
            try {
                await initDB();
                await loadTransactions();
                await loadNotes();
                initChart();
                updateStorageUsageTag();
            } catch (err) {
                console.error("Initialization error:", err);
            }
        });

        async function loadTransactions() {
            transactions = await getAllTransactionsFromDB();
            transactions.sort((a, b) => b.timestamp - a.timestamp);
            renderTransactions();
            updateStats();
            updateChart();
        }

        function formatVND(amount) {
            return new Intl.NumberFormat('vi-VN', { style: 'currency', currency: 'VND' }).format(amount).replace('₫', '₫');
        }

        function renderTransactions() {
            const listEl = document.getElementById('transaction-list');
            const emptyEl = document.getElementById('empty-state');
            const period = document.getElementById('filter-period').value;
            const filterDateVal = document.getElementById('filter-date-val').value;
            const category = document.getElementById('filter-category').value;
            const mediaFilter = document.getElementById('filter-media').value;
            const searchQuery = document.getElementById('search-input').value.trim().toLowerCase();

            let filtered = transactions.filter(t => {
                // Category filter
                if (category !== 'all' && t.category !== category) return false;

                // Media filter
                if (mediaFilter === 'has_media' && !t.mediaData) return false;
                if (mediaFilter === 'no_media' && t.mediaData) return false;

                // Search query
                if (searchQuery && !t.title.toLowerCase().includes(searchQuery)) return false;

                // Period filter
                if (period !== 'all' && filterDateVal) {
                    const tDate = new Date(t.timestamp);
                    const selectedDate = new Date(filterDateVal);

                    if (period === 'day') {
                        return tDate.toDateString() === selectedDate.toDateString();
                    } else if (period === 'month') {
                        return tDate.getFullYear() === selectedDate.getFullYear() &&
                               tDate.getMonth() === selectedDate.getMonth();
                    } else if (period === 'year') {
                        return tDate.getFullYear() === selectedDate.getFullYear();
                    }
                }

                return true;
            });

            // Update Filter Total Summary
            const totalSum = filtered.reduce((acc, curr) => acc + (Number(curr.amount) || 0), 0);
            document.getElementById('filtered-count').innerText = `${filtered.length} khoản`;
            document.getElementById('filtered-total').innerText = formatVND(totalSum);

            if (filtered.length === 0) {
                listEl.innerHTML = '';
                emptyEl.classList.remove('hidden');
                return;
            }

            emptyEl.classList.add('hidden');
            listEl.innerHTML = filtered.map(item => {
                const dateObj = new Date(item.timestamp);
                const dateFormatted = dateObj.toLocaleDateString('vi-VN', {
                    day: '2-digit', month: '2-digit', year: 'numeric'
                });
                const timeFormatted = dateObj.toLocaleTimeString('vi-VN', {
                    hour: '2-digit', minute: '2-digit'
                });

                const catIcon = CATEGORY_ICONS[item.category] || '📌';
                const catColor = CATEGORY_COLORS[item.category] || '#64748b';

                let mediaBadge = '';
                if (item.mediaData) {
                    const isVideo = item.mediaType && item.mediaType.startsWith('video');
                    mediaBadge = `
                        <button onclick="openLightbox('${item.id}')" class="inline-flex items-center gap-1 px-2 py-0.5 rounded-md bg-purple-50 text-purple-700 border border-purple-200 text-[10px] font-semibold hover:bg-purple-100 transition-colors">
                            <i class="fa-solid ${isVideo ? 'fa-video' : 'fa-image'} text-purple-600"></i>
                            <span>${isVideo ? 'Xem Video' : 'Xem Hóa Đơn'}</span>
                        </button>
                    `;
                }

                return `
                    <div class="bg-white p-3.5 rounded-2xl border border-slate-200 shadow-sm flex items-center justify-between gap-3 hover:border-slate-300 transition-all">
                        <div class="flex items-center gap-3 min-w-0">
                            <div class="w-10 h-10 rounded-xl flex-shrink-0 flex items-center justify-center text-lg text-white font-bold" style="background-color: ${catColor}">
                                ${catIcon}
                            </div>
                            <div class="min-w-0">
                                <h4 class="font-bold text-slate-900 text-xs sm:text-sm truncate">${escapeHTML(item.title)}</h4>
                                <div class="flex items-center gap-2 mt-0.5 flex-wrap">
                                    <span class="text-[10px] font-semibold text-slate-500 bg-slate-100 px-2 py-0.5 rounded-md">${escapeHTML(item.category)}</span>
                                    <span class="text-[10px] text-slate-400 font-medium">${dateFormatted} • ${timeFormatted}</span>
                                    ${mediaBadge}
                                </div>
                            </div>
                        </div>
                        <div class="flex items-center gap-2">
                            <div class="text-right">
                                <div class="font-extrabold text-slate-900 text-sm sm:text-base">${formatVND(item.amount)}</div>
                            </div>
                            <div class="flex items-center gap-1 border-l border-slate-100 pl-2">
                                <button onclick="editTransaction('${item.id}')" title="Sửa khoản chi" class="w-7 h-7 rounded-lg text-slate-400 hover:text-blue-600 hover:bg-blue-50 flex items-center justify-center text-xs transition-colors">
                                    <i class="fa-solid fa-pen"></i>
                                </button>
                                <button onclick="confirmDeleteTransaction('${item.id}')" title="Xóa khoản chi" class="w-7 h-7 rounded-lg text-slate-400 hover:text-red-600 hover:bg-red-50 flex items-center justify-center text-xs transition-colors">
                                    <i class="fa-solid fa-trash-can"></i>
                                </button>
                            </div>
                        </div>
                    </div>
                `;
            }).join('');
        }

        function updateStats() {
            const now = new Date();
            const todayStr = now.toDateString();
            const currentMonth = now.getMonth();
            const currentYear = now.getFullYear();

            let todaySum = 0, todayCnt = 0;
            let monthSum = 0, monthCnt = 0;
            let yearSum = 0, yearCnt = 0;
            let mediaCnt = 0;

            transactions.forEach(t => {
                const amount = Number(t.amount) || 0;
                const d = new Date(t.timestamp);

                if (t.mediaData) mediaCnt++;

                if (d.getFullYear() === currentYear) {
                    yearSum += amount;
                    yearCnt++;

                    if (d.getMonth() === currentMonth) {
                        monthSum += amount;
                        monthCnt++;

                        if (d.toDateString() === todayStr) {
                            todaySum += amount;
                            todayCnt++;
                        }
                    }
                }
            });

            document.getElementById('stat-today').innerText = formatVND(todaySum);
            document.getElementById('stat-today-count').innerText = `${todayCnt} giao dịch`;

            document.getElementById('stat-month').innerText = formatVND(monthSum);
            document.getElementById('stat-month-count').innerText = `${monthCnt} giao dịch`;

            document.getElementById('stat-year').innerText = formatVND(yearSum);
            document.getElementById('stat-year-count').innerText = `${yearCnt} giao dịch`;

            document.getElementById('stat-media').innerText = mediaCnt;
        }

        function initChart() {
            const ctx = document.getElementById('categoryChart').getContext('2d');
            categoryChart = new Chart(ctx, {
                type: 'doughnut',
                data: {
                    labels: [],
                    datasets: [{
                        data: [],
                        backgroundColor: [],
                        borderWidth: 2,
                        borderColor: '#ffffff'
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: { display: false },
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    return ` ${context.label}: ${formatVND(context.raw)}`;
                                }
                            }
                        }
                    },
                    cutout: '70%'
                }
            });
        }

        function updateChart() {
            if (!categoryChart) return;

            const categoryTotals = {};
            transactions.forEach(t => {
                const cat = t.category || 'Khác';
                categoryTotals[cat] = (categoryTotals[cat] || 0) + (Number(t.amount) || 0);
            });

            const labels = Object.keys(categoryTotals);
            const data = Object.values(categoryTotals);
            const colors = labels.map(l => CATEGORY_COLORS[l] || '#64748b');

            categoryChart.data.labels = labels;
            categoryChart.data.datasets[0].data = data;
            categoryChart.data.datasets[0].backgroundColor = colors;
            categoryChart.update();

            // Render custom legend
            const legendEl = document.getElementById('chart-legend');
            const total = data.reduce((a, b) => a + b, 0);

            if (labels.length === 0) {
                legendEl.innerHTML = `<div class="text-center py-4 text-slate-400 text-xs">Chưa có dữ liệu thống kê</div>`;
                return;
            }

            legendEl.innerHTML = labels.map((label, idx) => {
                const val = data[idx];
                const pct = total > 0 ? ((val / total) * 100).toFixed(1) : 0;
                const color = colors[idx];
                const icon = CATEGORY_ICONS[label] || '📌';

                return `
                    <div class="flex items-center justify-between py-1.5 text-xs">
                        <div class="flex items-center gap-2">
                            <span class="w-3 h-3 rounded-full flex-shrink-0" style="background-color: ${color}"></span>
                            <span class="font-medium text-slate-700">${icon} ${escapeHTML(label)}</span>
                        </div>
                        <div class="flex items-center gap-2">
                            <span class="font-bold text-slate-900">${formatVND(val)}</span>
                            <span class="text-[10px] text-slate-400 font-mono font-semibold w-10 text-right">${pct}%</span>
                        </div>
                    </div>
                `;
            }).join('');
        }

        function openTransactionModal(item = null) {
            const modal = document.getElementById('transaction-modal');
            const form = document.getElementById('expense-form');
            form.reset();
            clearAttachedMedia();

            if (item) {
                document.getElementById('modal-title').innerText = "Sửa khoản chi (VNĐ)";
                document.getElementById('edit-id').value = item.id;
                document.getElementById('form-amount').value = item.amount;
                document.getElementById('form-title').value = item.title;
                document.getElementById('form-category').value = item.category;

                const d = new Date(item.timestamp);
                const localIso = new Date(d.getTime() - (d.getTimezoneOffset() * 60000)).toISOString().slice(0, 16);
                document.getElementById('form-datetime').value = localIso;

                if (item.mediaData) {
                    setAttachedMedia(item.mediaData, item.mediaType || 'image/jpeg');
                }
            } else {
                document.getElementById('modal-title').innerText = "Thêm khoản chi mới (VNĐ)";
                document.getElementById('edit-id').value = "";
                
                const d = new Date();
                const localIso = new Date(d.getTime() - (d.getTimezoneOffset() * 60000)).toISOString().slice(0, 16);
                document.getElementById('form-datetime').value = localIso;
            }

            modal.classList.remove('hidden');
        }

        function closeTransactionModal() {
            document.getElementById('transaction-modal').classList.add('hidden');
        }

        function addAmountPreset(val) {
            const input = document.getElementById('form-amount');
            const current = Number(input.value) || 0;
            input.value = current + val;
        }

        function resetAmountInput() {
            document.getElementById('form-amount').value = '';
        }

        async function handleFormSubmit(e) {
            e.preventDefault();
            const editId = document.getElementById('edit-id').value;
            const amount = Number(document.getElementById('form-amount').value);
            const title = document.getElementById('form-title').value.trim();
            const category = document.getElementById('form-category').value;
            const datetimeVal = document.getElementById('form-datetime').value;
            const mediaData = document.getElementById('form-media-data').value;
            const mediaType = document.getElementById('form-media-type').value;

            if (!amount || amount <= 0 || !title || !datetimeVal) {
                showToast("Vui lòng điền đầy đủ các thông tin bắt buộc!", "warning");
                return;
            }

            const timestamp = new Date(datetimeVal).getTime();
            const item = {
                id: editId || ('tx_' + Date.now() + '_' + Math.random().toString(36).substr(2, 4)),
                amount: amount,
                title: title,
                category: category,
                timestamp: timestamp,
                mediaData: mediaData || null,
                mediaType: mediaType || null
            };

            try {
                await saveTransactionToDB(item);
                closeTransactionModal();
                await loadTransactions();
                updateStorageUsageTag();
                showToast(editId ? "Đã cập nhật khoản chi!" : "Đã thêm khoản chi mới!", "success");
            } catch (err) {
                console.error("Save error:", err);
                showToast("Lỗi khi lưu dữ liệu!", "error");
            }
        }

        async function loadNotes() {
            notes = await getAllNotesFromDB();
            notes.sort((a, b) => b.createdAt - a.createdAt);
            renderNotes();
        }

        function openNotesModal() {
            renderNotes();
            document.getElementById('notes-modal').classList.remove('hidden');
        }

        function closeNotesModal() {
            document.getElementById('notes-modal').classList.add('hidden');
        }

        async function handleAddNote(e) {
            e.preventDefault();
            const input = document.getElementById('new-note-text');
            const text = input.value.trim();
            if (!text) return;

            const note = {
                id: 'note_' + Date.now() + '_' + Math.random().toString(36).substr(2, 4),
                text: text,
                createdAt: Date.now()
            };

            await saveNoteToDB(note);
            input.value = '';
            await loadNotes();
            showToast("Đã lưu ghi chú mới!", "success");
        }

        async function confirmDeleteNote(id) {
            try {
                await deleteNoteFromDB(id);
                await loadNotes();
                showToast("Đã xóa ghi chú!", "success");
            } catch (err) {
                showToast("Lỗi khi xóa ghi chú!", "error");
            }
        }

        function renderNotes() {
            const listEl = document.getElementById('notes-list');
            if (notes.length === 0) {
                listEl.innerHTML = `
                    <div class="text-center py-8 text-slate-400 text-xs">
                        <i class="fa-regular fa-note-sticky text-2xl mb-2 block text-amber-300"></i>
                        Chưa có ghi chú nào. Hãy nhập nội dung ở trên để tạo mới.
                    </div>
                `;
                return;
            }

            listEl.innerHTML = notes.map(n => {
                const dateStr = new Date(n.createdAt).toLocaleString('vi-VN', {
                    day: '2-digit', month: '2-digit', year: 'numeric',
                    hour: '2-digit', minute: '2-digit'
                });

                return `
                    <div class="bg-amber-50/60 border border-amber-200/80 p-3 rounded-xl flex items-start justify-between gap-3 group transition-all">
                        <div class="flex-1 min-w-0">
                            <p class="text-xs sm:text-sm text-slate-800 font-medium whitespace-pre-wrap break-words">${escapeHTML(n.text)}</p>
                            <span class="text-[10px] text-amber-700/70 font-semibold mt-1 block">${dateStr}</span>
                        </div>
                        <button onclick="confirmDeleteNote('${n.id}')" title="Xóa ghi chú" class="text-slate-400 hover:text-red-600 w-7 h-7 rounded-lg hover:bg-red-50 flex items-center justify-center text-xs transition-colors">
                            <i class="fa-solid fa-trash-can"></i>
                        </button>
                    </div>
                `;
            }).join('');
        }

        async function openQuickCameraExpense() {
            isQuickCameraMode = true;
            await openCameraModal('photo');
        }

        function editTransaction(id) {
            const item = transactions.find(t => t.id === id);
            if (item) {
                openTransactionModal(item);
            }
        }

        function confirmDeleteTransaction(id) {
            showConfirmDialog(
                "Xóa khoản chi",
                "Bạn có chắc chắn muốn xóa giao dịch này khỏi sổ chi tiêu?",
                async () => {
                    try {
                        await deleteTransactionFromDB(id);
                        await loadTransactions();
                        showToast("Đã xóa khoản chi!", "success");
                    } catch (err) {
                        showToast("Lỗi khi xóa khoản chi", "error");
                    }
                }
            );
        }

        async function openCameraModal(mode = 'photo') {
            activeCameraMode = mode;
            const modal = document.getElementById('camera-modal');
            const title = document.getElementById('camera-modal-title');
            const btnPhoto = document.getElementById('btn-take-photo');
            const btnStartVid = document.getElementById('btn-start-video');
            const btnStopVid = document.getElementById('btn-stop-video');

            if (mode === 'photo') {
                title.innerHTML = `<i class="fa-solid fa-camera text-blue-400"></i> Chụp Ảnh Hóa Đơn`;
                btnPhoto.classList.remove('hidden');
                btnStartVid.classList.add('hidden');
                btnStopVid.classList.add('hidden');
            } else {
                title.innerHTML = `<i class="fa-solid fa-video text-indigo-400"></i> Quay Video Hóa Đơn`;
                btnPhoto.classList.add('hidden');
                btnStartVid.classList.remove('hidden');
                btnStopVid.classList.add('hidden');
            }

            modal.classList.remove('hidden');
            await startCameraStream();
        }

        async function startCameraStream() {
            const video = document.getElementById('camera-stream');
            stopCameraStream();

            try {
                const constraints = {
                    video: {
                        facingMode: currentCameraFacing,
                        width: { ideal: 1280 },
                        height: { ideal: 720 }
                    },
                    audio: activeCameraMode === 'video'
                };
                cameraStream = await navigator.mediaDevices.getUserMedia(constraints);
                video.srcObject = cameraStream;
            } catch (err) {
                console.error("Camera access error:", err);
                showToast("Không thể truy cập Camera. Vui lòng kiểm tra quyền ứng dụng!", "warning");
                closeCameraModal();
            }
        }

        function stopCameraStream() {
            if (cameraStream) {
                cameraStream.getTracks().forEach(track => track.stop());
                cameraStream = null;
            }
        }

        function switchCameraFacing() {
            currentCameraFacing = (currentCameraFacing === 'environment') ? 'user' : 'environment';
            startCameraStream();
        }

        function closeCameraModal() {
            stopVideoRecordingTimer();
            if (mediaRecorder && mediaRecorder.state !== 'inactive') {
                mediaRecorder.stop();
            }
            stopCameraStream();
            document.getElementById('camera-modal').classList.add('hidden');
        }

        function takePhoto() {
            const video = document.getElementById('camera-stream');
            const canvas = document.getElementById('camera-canvas');
            if (!video.videoWidth) return;

            canvas.width = video.videoWidth;
            canvas.height = video.videoHeight;
            const ctx = canvas.getContext('2d');
            ctx.drawImage(video, 0, 0, canvas.width, canvas.height);

            const dataUrl = canvas.toDataURL('image/jpeg', 0.8);

            if (isQuickCameraMode) {
                isQuickCameraMode = false;
                closeCameraModal();
                openTransactionModal();
                setAttachedMedia(dataUrl, 'image/jpeg');
                showToast("Đã chụp ảnh! Vui lòng nhập số tiền chi tiêu.", "success");
                setTimeout(() => {
                    const amountInput = document.getElementById('form-amount');
                    if (amountInput) amountInput.focus();
                }, 100);
            } else {
                setAttachedMedia(dataUrl, 'image/jpeg');
                closeCameraModal();
                showToast("Đã chụp và đính kèm hóa đơn!", "success");
            }
        }

        function startVideoRecording() {
            if (!cameraStream) return;
            recordedChunks = [];

            try {
                mediaRecorder = new MediaRecorder(cameraStream);
                mediaRecorder.ondataavailable = (e) => {
                    if (e.data.size > 0) recordedChunks.push(e.data);
                };
                mediaRecorder.onstop = () => {
                    const blob = new Blob(recordedChunks, { type: 'video/webm' });
                    const reader = new FileReader();
                    reader.onloadend = () => {
                        setAttachedMedia(reader.result, 'video/webm');
                        showToast("Đã quay xong video đính kèm!", "success");
                        closeCameraModal();
                    };
                    reader.readAsDataURL(blob);
                };

                mediaRecorder.start();
                document.getElementById('btn-start-video').classList.add('hidden');
                document.getElementById('btn-stop-video').classList.remove('hidden');
                document.getElementById('recording-badge').classList.remove('hidden');

                recordSeconds = 0;
                document.getElementById('recording-timer').innerText = "00:00";
                recordTimerInterval = setInterval(() => {
                    recordSeconds++;
                    const m = String(Math.floor(recordSeconds / 60)).padStart(2, '0');
                    const s = String(recordSeconds % 60).padStart(2, '0');
                    document.getElementById('recording-timer').innerText = `${m}:${s}`;
                }, 1000);

            } catch (err) {
                console.error("Recording start error:", err);
                showToast("Trình duyệt không hỗ trợ quay video trực tiếp", "error");
            }
        }

        function stopVideoRecording() {
            if (mediaRecorder && mediaRecorder.state !== 'inactive') {
                mediaRecorder.stop();
            }
            stopVideoRecordingTimer();
        }

        function stopVideoRecordingTimer() {
            if (recordTimerInterval) {
                clearInterval(recordTimerInterval);
                recordTimerInterval = null;
            }
            document.getElementById('recording-badge').classList.add('hidden');
        }

        function handleFileUpload(e) {
            const file = e.target.files[0];
            if (!file) return;

            const reader = new FileReader();
            reader.onload = (evt) => {
                setAttachedMedia(evt.target.result, file.type);
                showToast("Đã đính kèm tệp từ thiết bị!", "success");
            };
            reader.readAsDataURL(file);
        }

        function setAttachedMedia(dataUrl, type) {
            document.getElementById('form-media-data').value = dataUrl;
            document.getElementById('form-media-type').value = type;

            const previewBox = document.getElementById('media-preview-box');
            const previewContent = document.getElementById('media-preview-content');
            const actionBtns = document.getElementById('media-action-buttons');

            if (type && type.startsWith('video')) {
                previewContent.innerHTML = `<video src="${dataUrl}" controls class="max-h-40 w-full object-contain rounded-lg"></video>`;
            } else {
                previewContent.innerHTML = `<img src="${dataUrl}" alt="Hóa đơn" class="max-h-40 w-full object-contain rounded-lg">`;
            }

            previewBox.classList.remove('hidden');
            actionBtns.classList.add('hidden');
        }

        function clearAttachedMedia() {
            document.getElementById('form-media-data').value = '';
            document.getElementById('form-media-type').value = '';
            document.getElementById('form-file-input').value = '';

            document.getElementById('media-preview-box').classList.add('hidden');
            document.getElementById('media-preview-content').innerHTML = '';
            document.getElementById('media-action-buttons').classList.remove('hidden');
        }

        function openLightbox(id) {
            const item = transactions.find(t => t.id === id);
            if (!item || !item.mediaData) return;

            const container = document.getElementById('lightbox-container');
            const modal = document.getElementById('lightbox-modal');

            if (item.mediaType && item.mediaType.startsWith('video')) {
                container.innerHTML = `<video src="${item.mediaData}" controls autoplay class="max-h-[80vh] w-auto rounded-xl"></video>`;
            } else {
                container.innerHTML = `<img src="${item.mediaData}" alt="Hóa đơn" class="max-h-[80vh] w-auto object-contain rounded-xl">`;
            }

            modal.classList.remove('hidden');
        }

        function closeLightboxModal() {
            const container = document.getElementById('lightbox-container');
            container.innerHTML = '';
            document.getElementById('lightbox-modal').classList.add('hidden');
        }

        function filterByMediaShortcut() {
            document.getElementById('filter-media').value = 'has_media';
            renderTransactions();
            window.scrollTo({
                top: document.getElementById('transaction-list').offsetTop - 100,
                behavior: 'smooth'
            });
        }

        function handlePeriodChange() {
            const period = document.getElementById('filter-period').value;
            const container = document.getElementById('filter-date-container');
            const dateInput = document.getElementById('filter-date-val');

            if (period === 'all') {
                container.classList.add('hidden');
            } else {
                container.classList.remove('hidden');
                if (!dateInput.value) {
                    dateInput.value = new Date().toISOString().slice(0, 10);
                }
            }
            renderTransactions();
        }

        let calcExpr = '';

        function openCalculator() {
            calcExpr = '';
            updateCalcDisplay();
            document.getElementById('calculator-modal').classList.remove('hidden');
        }

        function closeCalculator() {
            document.getElementById('calculator-modal').classList.add('hidden');
        }

        function calcInput(char) {
            if (char === 'C') {
                calcExpr = '';
            } else if (char === '=') {
                try {
                    const sanitized = calcExpr.replace(/[^0-9+\-*/.]/g, '');
                    calcExpr = String(eval(sanitized) || 0);
                } catch {
                    calcExpr = 'Lỗi';
                }
            } else {
                if (calcExpr === 'Lỗi') calcExpr = '';
                calcExpr += char;
            }
            updateCalcDisplay();
        }

        function updateCalcDisplay() {
            document.getElementById('calc-expression').innerText = calcExpr;
            try {
                const val = eval(calcExpr.replace(/[^0-9+\-*/.]/g, ''));
                document.getElementById('calc-display').innerText = val ? formatVND(val) : (calcExpr || '0');
            } catch {
                document.getElementById('calc-display').innerText = calcExpr || '0';
            }
        }

        function applyCalculatorToForm() {
            try {
                const val = Math.round(eval(calcExpr.replace(/[^0-9+\-*/.]/g, '')));
                if (val && val > 0) {
                    openTransactionModal();
                    document.getElementById('form-amount').value = val;
                    closeCalculator();
                    showToast("Đã nhập số tiền từ máy tính!", "success");
                } else {
                    showToast("Số tiền không hợp lệ!", "warning");
                }
            } catch {
                showToast("Lỗi tính toán!", "error");
            }
        }

        function confirmClearAllData() {
            showConfirmDialog(
                "Xóa toàn bộ dữ liệu?",
                "Tất cả các khoản chi và hóa đơn đã lưu sẽ bị xóa vĩnh viễn khỏi thiết bị.",
                async () => {
                    try {
                        await clearAllDataFromDB();
                        await loadTransactions();
                        showToast("Đã xóa sạch toàn bộ dữ liệu!", "success");
                    } catch (err) {
                        showToast("Lỗi khi xóa dữ liệu!", "error");
                    }
                }
            );
        }

        async function updateStorageUsageTag() {
            if (!navigator.storage || !navigator.storage.estimate) return;
            try {
                const estimate = await navigator.storage.estimate();
                const mbUsed = (estimate.usage / (1024 * 1024)).toFixed(2);
                document.getElementById('storage-usage-tag').innerText = `${mbUsed} MB`;
            } catch (e) {
                console.error("Storage estimation error:", e);
            }
        }

        function showToast(message, type = 'info') {
            const container = document.getElementById('toast-container');
            const toast = document.createElement('div');

            const bgMap = {
                success: 'bg-emerald-600 text-white',
                error: 'bg-red-600 text-white',
                warning: 'bg-amber-500 text-white',
                info: 'bg-slate-800 text-white'
            };

            const iconMap = {
                success: 'fa-circle-check',
                error: 'fa-circle-xmark',
                warning: 'fa-triangle-exclamation',
                info: 'fa-circle-info'
            };

            toast.className = `p-3 rounded-xl shadow-xl flex items-center gap-2.5 text-xs font-semibold transform transition-all duration-300 translate-y-2 opacity-0 pointer-events-auto ${bgMap[type] || bgMap.info}`;
            toast.innerHTML = `<i class="fa-solid ${iconMap[type]} text-sm"></i> <span>${escapeHTML(message)}</span>`;

            container.appendChild(toast);

            setTimeout(() => {
                toast.classList.remove('translate-y-2', 'opacity-0');
            }, 10);

            setTimeout(() => {
                toast.classList.add('opacity-0', 'translate-y-2');
                setTimeout(() => toast.remove(), 300);
            }, 3000);
        }

        function showConfirmDialog(title, message, onOk) {
            const modal = document.getElementById('confirm-modal');
            document.getElementById('confirm-title').innerText = title;
            document.getElementById('confirm-message').innerText = message;

            const btnOk = document.getElementById('confirm-btn-ok');
            const btnCancel = document.getElementById('confirm-btn-cancel');

            const cleanup = () => {
                modal.classList.add('hidden');
                btnOk.onclick = null;
                btnCancel.onclick = null;
            };

            btnOk.onclick = () => {
                cleanup();
                if (onOk) onOk();
            };

            btnCancel.onclick = () => {
                cleanup();
            };

            modal.classList.remove('hidden');
        }

        function escapeHTML(str) {
            if (!str) return '';
            return str.replace(/[&<>'"]/g, 
                tag => ({ '&': '&amp;', '<': '&lt;', '>': '&gt;', "'": '&#39;', '"': '&quot;' }[tag] || tag)
            );
        }
    </script>
</body>
</html>
