# webtur
Website visit Now 
<!DOCTYPE html>
<html lang="bn">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>বাজার দর - বাংলাদেশের কাঁচামাল বাজারদর</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Firebase SDK -->
    <script src="https://www.gstatic.com/firebasejs/9.6.0/firebase-app-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.6.0/firebase-auth-compat.js"></script>
    <script src="https://www.gstatic.com/firebasejs/9.6.0/firebase-firestore-compat.js"></script>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; -webkit-tap-highlight-color: transparent; }
        :root { --primary: #2e7d32; --primary-dark: #1b5e20; --secondary: #f9a825; --light: #f5f5f5; --dark: #333; --danger: #d32f2f; --success: #388e3c; --warning: #ffa000; --gray: #757575; --border-radius: 12px; --shadow: 0 4px 12px rgba(0, 0, 0, 0.1); }
        body { background: #f5f5f5; color: var(--dark); min-height: 100vh; padding-bottom: 80px; }
        .container { max-width: 100%; margin: 0 auto; padding: 0 10px; }
        header { background: linear-gradient(to right, var(--primary), var(--primary-dark)); color: white; padding: 15px 10px; position: sticky; top: 0; z-index: 100; box-shadow: var(--shadow); display: flex; align-items: center; justify-content: space-between; }
        .date-display { font-size: 12px; background: rgba(255, 255, 255, 0.2); padding: 4px 8px; border-radius: 20px; }
        .user-profile { display: flex; align-items: center; gap: 8px; cursor: pointer; position: relative; }
        .user-avatar { width: 32px; height: 32px; border-radius: 50%; background-color: white; color: var(--primary); display: flex; align-items: center; justify-content: center; font-weight: bold; }
        .user-menu { position: absolute; top: 40px; right: 0; background-color: white; border-radius: var(--border-radius); box-shadow: var(--shadow); padding: 10px 0; width: 180px; z-index: 110; display: none; }
        .user-menu.active { display: block; }
        .user-menu-item { padding: 8px 15px; font-size: 14px; cursor: pointer; transition: background-color 0.3s; color: var(--dark); }
        .user-menu-item:hover { background-color: #f5f5f5; }
        .user-menu-item i { margin-right: 8px; width: 20px; text-align: center; color: var(--primary); }
        .search-section { background-color: white; padding: 15px 10px; box-shadow: var(--shadow); position: sticky; top: 66px; z-index: 90; }
        .search-container { display: flex; gap: 8px; }
        .search-container input { flex: 1; padding: 10px 12px; border: 2px solid #e0e0e0; border-radius: var(--border-radius); font-size: 14px; outline: none; transition: border-color 0.3s; }
        .search-container input:focus { border-color: var(--primary); }
        .search-container button { background-color: var(--primary); color: white; border: none; border-radius: var(--border-radius); padding: 0 15px; font-size: 14px; cursor: pointer; min-width: 50px; }
        .section-title { font-size: 16px; font-weight: 600; margin: 15px 0 10px; color: var(--primary-dark); display: flex; align-items: center; gap: 6px; }
        .section-title i { font-size: 18px; }
        .categories { display: grid; grid-template-columns: repeat(2, 1fr); gap: 10px; margin-bottom: 15px; }
        .category-card { background-color: white; border-radius: var(--border-radius); padding: 12px; text-align: center; box-shadow: var(--shadow); cursor: pointer; transition: box-shadow 0.2s; }
        .category-card.active { box-shadow: 0 0 0 2px var(--primary); }
        .category-icon { width: 40px; height: 40px; margin: 0 auto 8px; background-color: var(--light); border-radius: 50%; display: flex; align-items: center; justify-content: center; font-size: 20px; color: var(--primary); }
        .category-name { font-size: 13px; font-weight: 500; }
        .products-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 12px; }
        .product-card { border: 1px solid #e0e0e0; border-radius: var(--border-radius); overflow: hidden; background: white; cursor: pointer; }
        .product-image { height: 120px; background-color: #f5f5f5; display: flex; align-items: center; justify-content: center; }
        .product-image img { width: 100%; height: 100%; object-fit: cover; }
        .product-info { padding: 12px; }
        .product-name { font-size: 14px; font-weight: 600; margin-bottom: 6px; color: var(--dark); display: -webkit-box; -webkit-line-clamp: 2; -webkit-box-orient: vertical; overflow: hidden; }
        .product-meta { display: flex; justify-content: space-between; margin-bottom: 8px; font-size: 12px; color: var(--gray); }
        .product-price { font-size: 16px; font-weight: 700; color: var(--primary-dark); margin-bottom: 8px; }
        .price-change { display: flex; align-items: center; gap: 4px; font-weight: 600; font-size: 12px; }
        .price-up { color: var(--danger); }
        .price-down { color: var(--success); }
        .added-by { font-size: 10px; color: var(--gray); margin-top: 4px; font-style: italic; }
        footer { position: fixed; bottom: 0; left: 0; right: 0; background: white; color: var(--dark); padding: 5px 0; display: flex; justify-content: space-around; z-index: 100; box-shadow: 0 -2px 10px rgba(0, 0, 0, 0.1); border-top: 1px solid #e0e0e0; }
        .nav-item { display: flex; flex-direction: column; align-items: center; gap: 4px; cursor: pointer; padding: 5px 10px; border-radius: var(--border-radius); transition: color 0.3s; flex-basis: 0; flex-grow: 1; }
        .nav-item i { font-size: 18px; }
        .nav-item span { font-size: 12px; }
        .nav-item.active { color: var(--primary); }
        .tabs { display: flex; gap: 5px; margin: 10px 0 15px; background-color: #e0e0e0; border-radius: var(--border-radius); padding: 5px; position: sticky; top: 66px; z-index: 80; }
        .tab { flex: 1; text-align: center; padding: 10px; border-radius: var(--border-radius); cursor: pointer; font-weight: 500; font-size: 14px; transition: background-color 0.3s; }
        .tab.active { background-color: var(--primary); color: white; }
        .tab-content { display: none; }
        .tab-content.active { display: block; }
        .trend-chart { height: 150px; background-color: #f9f9f9; border-radius: var(--border-radius); margin-top: 15px; display: flex; align-items: flex-end; padding: 8px; gap: 8px; overflow-x: auto; }
        
        /* *** MODIFIED CSS for trend chart *** */
        .trend-bar { flex: 1; min-width: 40px; border-radius: 5px 5px 0 0; position: relative; min-height: 1px; transition: height 0.5s ease-out, background-color 0.3s; }
        .trend-label { position: absolute; top: -20px; left: 0; right: 0; text-align: center; font-size: 10px; font-weight: bold; color: #fff; text-shadow: 1px 1px 2px rgba(0,0,0,0.5); }
        
        .loading, .empty-state { text-align: center; padding: 30px 15px; grid-column: span 2; }
        .spinner { width: 24px; height: 24px; border: 3px solid rgba(0, 0, 0, 0.1); border-radius: 50%; border-top-color: var(--primary); animation: spin 1s ease-in-out infinite; margin: 0 auto; }
        @keyframes spin { to { transform: rotate(360deg); } }
        .empty-state i { font-size: 40px; color: var(--gray); margin-bottom: 10px; }
        .empty-state p { color: var(--gray); font-size: 14px; }
        .modal { display: none; position: fixed; top: 0; left: 0; right: 0; bottom: 0; background-color: rgba(0, 0, 0, 0.5); z-index: 200; align-items: center; justify-content: center; }
        .modal.active { display: flex; }
        .modal-content { background-color: white; border-radius: var(--border-radius); width: 90%; max-width: 400px; max-height: 90vh; overflow-y: auto; box-shadow: var(--shadow); animation: modalFadeIn 0.3s; }
        @keyframes modalFadeIn { from { opacity: 0; transform: translateY(-20px); } to { opacity: 1; transform: translateY(0); } }
        .modal-header { padding: 15px; border-bottom: 1px solid #e0e0e0; display: flex; justify-content: space-between; align-items: center; }
        .modal-title { font-weight: 600; font-size: 18px; }
        .modal-close { background: none; border: none; font-size: 20px; cursor: pointer; color: var(--gray); }
        .modal-body { padding: 15px; }
        .modal-footer { padding: 15px; border-top: 1px solid #e0e0e0; display: flex; justify-content: flex-end; gap: 10px; }
        .form-group { margin-bottom: 15px; }
        .form-group label { display: block; margin-bottom: 5px; font-weight: 500; font-size: 14px; }
        .form-group input, .form-group select { width: 100%; padding: 10px; border: 1px solid #e0e0e0; border-radius: var(--border-radius); font-size: 14px; }
        .btn { padding: 10px 15px; border: none; border-radius: var(--border-radius); font-weight: 600; cursor: pointer; transition: background-color 0.3s; }
        .btn:disabled { background-color: var(--gray); cursor: not-allowed; }
        .btn-primary { background-color: var(--primary); color: white; }
        .btn-secondary { background-color: #e0e0e0; color: var(--dark); }
        .add-product-btn { position: fixed; bottom: 80px; right: 20px; width: 50px; height: 50px; border-radius: 50%; background-color: var(--primary); color: white; display: none; align-items: center; justify-content: center; font-size: 24px; box-shadow: var(--shadow); z-index: 90; cursor: pointer; }
        .add-product-btn.visible { display: flex; }
        .profile-section, .products-section { background-color: white; border-radius: var(--border-radius); padding: 15px; margin-bottom: 15px; box-shadow: var(--shadow); }
        .profile-info-item { margin-bottom: 10px; }
        .profile-info-label { font-weight: 500; color: var(--gray); font-size: 14px; }
        .profile-info-value { font-size: 16px; }
        .toast-notification { position: fixed; top: 80px; left: 50%; transform: translateX(-50%); background-color: var(--dark); color: white; padding: 10px 20px; border-radius: var(--border-radius); z-index: 1000; display: none; box-shadow: var(--shadow); }
        .toast-notification.success { background-color: var(--success); }
        .toast-notification.error { background-color: var(--danger); }
        .list-item { display: flex; justify-content: space-between; align-items: center; padding: 8px; border-bottom: 1px solid #eee; }
        .list-item:last-child { border-bottom: none; }
        .status-pending { color: var(--warning); font-weight: bold; }
        @media (min-width: 480px) { .categories, .products-grid { grid-template-columns: repeat(3, 1fr); } }
        @media (min-width: 768px) { .container { padding: 0 15px; } .categories, .products-grid { grid-template-columns: repeat(4, 1fr); } }
    </style>
</head>
<body>
    <div id="toast" class="toast-notification"></div>

    <div class="container">
        <div id="user-app">
            <header>
                <div class="date-display" id="current-date"></div>
                <div class="user-profile" id="user-profile-menu">
                    <div class="user-avatar" id="user-avatar-initial"><i class="fas fa-user"></i></div>
                    <div class="user-menu" id="user-menu-dropdown">
                        <div class="user-menu-item" id="login-btn"><i class="fas fa-sign-in-alt"></i> লগইন</div>
                        <div class="user-menu-item" id="register-btn"><i class="fas fa-user-plus"></i> একাউন্ট খুলুন</div>
                        <div class="user-menu-item" id="logout-btn" style="display: none;"><i class="fas fa-sign-out-alt"></i> লগআউট</div>
                    </div>
                </div>
            </header>
            
            <div class="search-section">
                <div class="search-container">
                    <input type="text" id="search-input" placeholder="পণ্য খুঁজুন...">
                    <button id="search-btn"><i class="fas fa-search"></i></button>
                </div>
            </div>
            
            <div class="tabs">
                <div class="tab active" data-tab="today">আজকের দর</div>
                <div class="tab" data-tab="divisions">বিভাগ</div>
                <div class="tab" data-tab="trends">প্রবণতা</div>
            </div>
            
            <div class="tab-content active" id="today-tab">
                <div class="products-section">
                    <div class="section-title"><i class="fas fa-calendar-day"></i> আজকের বাজারদর</div>
                    <div class="products-grid" id="today-products"><div class="loading"><div class="spinner"></div></div></div>
                </div>
            </div>
            
            <div class="tab-content" id="divisions-tab">
                <div class="section-title"><i class="fas fa-map-marked-alt"></i> বিভাগ অনুযায়ী বাজারদর</div>
                <div class="categories"> <!-- Categories will be generated by JS --> </div>
                <div class="products-section" id="division-products-container" style="display: none;">
                    <div class="products-grid" id="division-products"></div>
                </div>
            </div>
            
            <div class="tab-content" id="trends-tab">
                <div class="products-section">
                    <div class="section-title"><i class="fas fa-chart-line"></i> মূল্য প্রবণতা</div>
                    <div class="products-grid" id="trend-products-list">
                        <div class="empty-state"><i class="fas fa-hand-pointer"></i><p>প্রবণতা দেখতে কোনো পণ্যে ক্লিক করুন</p></div>
                    </div>
                    <div class="trend-chart" id="price-trend-chart">
                        <div class="empty-state"><p>পণ্যের মূল্য চার্ট এখানে দেখানো হবে</p></div>
                    </div>
                </div>
            </div>
            
            <div class="tab-content" id="profile-tab">
                <div class="profile-section">
                    <div class="section-title"><i class="fas fa-user-circle"></i> আমার প্রোফাইল</div>
                    <div id="profile-info-container">
                        <div class="empty-state"><i class="fas fa-user-lock"></i><p>প্রোফাইল দেখতে লগইন করুন</p></div>
                    </div>
                </div>
                <div class="products-section" id="my-submissions-section" style="display: none;">
                    <div class="section-title"><i class="fas fa-hourglass-half"></i> আমার অপেক্ষমান পণ্য</div>
                    <div id="my-submissions-list"></div>
                </div>
            </div>
            
            <div class="add-product-btn" id="add-product-button"><i class="fas fa-plus"></i></div>
            
            <footer>
                <div class="nav-item active" data-tab-name="today"><i class="fas fa-home"></i><span>হোম</span></div>
                <div class="nav-item" data-tab-name="profile"><i class="fas fa-user"></i><span>প্রোফাইল</span></div>
            </footer>
        </div>
    </div>

    <!-- Modals -->
    <div class="modal" id="login-modal">
        <div class="modal-content">
            <div class="modal-header"><div class="modal-title">লগইন</div><button class="modal-close" data-modal="login-modal">×</button></div>
            <div class="modal-body"><form id="login-form"><div class="form-group"><label for="login-email">ইমেইল</label><input type="email" id="login-email" required></div><div class="form-group"><label for="login-password">পাসওয়ার্ড</label><input type="password" id="login-password" required></div></form></div>
            <div class="modal-footer"><button class="btn btn-secondary modal-close" data-modal="login-modal">বাতিল</button><button class="btn btn-primary" id="login-submit-btn">লগইন</button></div>
        </div>
    </div>
    <div class="modal" id="register-modal">
        <div class="modal-content">
            <div class="modal-header"><div class="modal-title">নতুন অ্যাকাউন্ট</div><button class="modal-close" data-modal="register-modal">×</button></div>
            <div class="modal-body"><form id="register-form"><div class="form-group"><label for="register-name">নাম</label><input type="text" id="register-name" required></div><div class="form-group"><label for="register-email">ইমেইল</label><input type="email" id="register-email" required></div><div class="form-group"><label for="register-password">পাসওয়ার্ড (কমপক্ষে ৬ অক্ষর)</label><input type="password" id="register-password" required minlength="6"></div></form></div>
            <div class="modal-footer"><button class="btn btn-secondary modal-close" data-modal="register-modal">বাতিল</button><button class="btn btn-primary" id="register-submit-btn">রেজিস্টার</button></div>
        </div>
    </div>
    <div class="modal" id="add-product-modal">
        <div class="modal-content">
            <div class="modal-header"><div class="modal-title">নতুন পণ্য যোগ</div><button class="modal-close" data-modal="add-product-modal">×</button></div>
            <div class="modal-body"><form id="add-product-form"><div class="form-group"><label for="add-product-name">পণ্যের নাম</label><input type="text" id="add-product-name" required></div><div class="form-group"><label for="add-product-division">বিভাগ</label><select id="add-product-division" required></select></div><div class="form-group"><label for="add-product-unit">একক</label><input type="text" id="add-product-unit" placeholder="যেমন: কেজি, লিটার" required></div><div class="form-group"><label for="add-product-price">বর্তমান মূল্য (৳)</label><input type="number" id="add-product-price" required step="0.01"></div><div class="form-group"><label for="add-product-image">ছবির লিংক (URL)</label><input type="url" id="add-product-image"></div></form></div>
            <div class="modal-footer"><button class="btn btn-secondary modal-close" data-modal="add-product-modal">বাতিল</button><button class="btn btn-primary" id="add-product-submit-btn">জমা দিন</button></div>
        </div>
    </div>
    
    <script>
        document.addEventListener('DOMContentLoaded', () => {
            const firebaseConfig = { apiKey: "YOUR_API_KEY", authDomain: "YOUR_AUTH_DOMAIN", projectId: "YOUR_PROJECT_ID", storageBucket: "YOUR_STORAGE_BUCKET", messagingSenderId: "YOUR_MESSAGING_SENDER_ID", appId: "YOUR_APP_ID" };
            firebase.initializpp(firebaseConfig);
            const auth = firebase.auth();
            const db = firebase.firestore();

            let allProducts = [];
            const divisions = { dhaka: 'ঢাকা', chattogram: 'চট্টগ্রাম', khulna: 'খুলনা', rajshahi: 'রাজশাহী', barishal: 'বরিশাল', sylhet: 'সিলেট', rangpur: 'রংপুর', mymensingh: 'ময়মনসিংহ' };
            const divisionIcons = { dhaka: 'fa-city', chattogram: 'fa-anchor', khulna: 'fa-fish', rajshahi: 'fa-apple-alt', barishal: 'fa-water', sylhet: 'fa-mountain', rangpur: 'fa-wheat-awn', mymensingh: 'fa-tractor' };

            const showToast = (message, type = 'success') => { const toast = document.getElementById('toast'); toast.textContent = message; toast.className = `toast-notification ${type}`; toast.style.display = 'block'; setTimeout(() => { toast.style.display = 'none'; }, 3000); };
            const openModal = (modalId) => document.getElementById(modalId).classList.add('active');
            const closeModal = (modalId) => document.getElementById(modalId).classList.remove('active');
            
            const switchTab = (tabName) => {
                 document.querySelectorAll('.tab, .nav-item').forEach(el => el.classList.remove('active'));
                 document.querySelectorAll(`.tab[data-tab="${tabName}"], .nav-item[data-tab-name="${tabName}"]`).forEach(el => el.classList.add('active'));
                 document.querySelectorAll('.tab-content').forEach(el => el.classList.remove('active'));
                 document.getElementById(`${tabName}-tab`).classList.add('active'
