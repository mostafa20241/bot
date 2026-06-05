<!DOCTYPE html>
<html lang="fa" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>تحلیل جامع پروژه ربات و داشبورد</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Vazirmatn:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <!-- Chosen Palette: Warm Neutrals with Blue/Teal accents -->
    <!-- Application Structure Plan: A single-page application with a fixed sidebar for navigation and a main content area. The structure is thematic, not linear, divided into: 1. Architecture Overview (interactive diagram), 2. Bot Deep Dive (functional flow), 3. Web Dashboard Deep Dive (security & features), 4. Data Analytics (interactive charts), and 5. Database Schema (visual tables). This structure allows users to explore the system from a high-level overview down to specific details in any order they choose, enhancing usability and comprehension of the complex software project. -->
    <!-- Visualization & Content Choices:
        - Report Info: System Architecture Diagram -> Goal: Inform -> Viz: Interactive HTML/CSS Diagram -> Interaction: Click on components to show details -> Justification: Provides an immediate, high-level understanding of the system's structure. -> Library/Method: Vanilla JS, Tailwind CSS.
        - Report Info: Bot's message processing flow -> Goal: Explain a process -> Viz: HTML/CSS Flowchart -> Interaction: Hover to highlight steps -> Justification: Simplifies a complex code path into an easy-to-follow visual sequence. -> Library/Method: HTML, Tailwind CSS.
        - Report Info: Web Dashboard Login Flow -> Goal: Explain a security process -> Viz: HTML/CSS Flowchart -> Justification: Clarifies the 2FA security measure which is a key feature. -> Library/Method: HTML, Tailwind CSS.
        - Report Info: Message stats by destination/day -> Goal: Compare, Show Change -> Viz: Pie and Line charts -> Interaction: Tooltips, dynamic filtering -> Justification: Translates raw database queries into actionable insights, mimicking the actual dashboard's functionality. -> Library/Method: Chart.js on Canvas.
        - Report Info: Database table structures -> Goal: Organize, Inform -> Viz: Styled HTML tables -> Interaction: None needed, pure information display -> Justification: A clear, readable format for understanding the data model without needing to read SQL code. -> Library/Method: HTML, Tailwind CSS.
    -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <style>
        body {
            font-family: 'Vazirmatn', sans-serif;
            background-color: #f8fafc;
        }
        .section-active {
            display: block;
        }
        .section-hidden {
            display: none;
        }
        .nav-link {
            transition: all 0.2s ease-in-out;
        }
        .nav-link.active {
            background-color: #0ea5e9;
            color: white;
            transform: translateX(-4px);
        }
        .diagram-node {
            transition: all 0.3s ease;
            cursor: pointer;
        }
        .diagram-node:hover {
            transform: scale(1.05);
            box-shadow: 0 10px 15px -3px rgb(0 0 0 / 0.1), 0 4px 6px -4px rgb(0 0 0 / 0.1);
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 500px;
            margin-left: auto;
            margin-right: auto;
            height: 350px;
            max-height: 400px;
        }
        @media (min-width: 768px) {
            .chart-container {
                height: 400px;
            }
        }
    </style>
</head>
<body class="text-slate-800">
    <div class="flex flex-col md:flex-row min-h-screen">
        <!-- Sidebar Navigation -->
        <aside class="w-full md:w-64 bg-white shadow-lg md:sticky top-0 h-auto md:h-screen p-4 border-l border-slate-200">
            <h1 class="text-2xl font-bold text-sky-600 mb-6 text-center">تحلیل پروژه</h1>
            <nav id="main-nav" class="flex flex-row md:flex-col gap-2 justify-center md:justify-start">
                <a href="#architecture" class="nav-link active p-3 rounded-lg font-semibold" data-target="architecture">🏛️ معماری سیستم</a>
                <a href="#bot-deep-dive" class="nav-link p-3 rounded-lg font-semibold" data-target="bot-deep-dive">🤖 عملکرد ربات</a>
                <a href="#dashboard-deep-dive" class="nav-link p-3 rounded-lg font-semibold" data-target="dashboard-deep-dive">🖥️ عملکرد داشبورد</a>
                <a href="#data-analytics" class="nav-link p-3 rounded-lg font-semibold" data-target="data-analytics">📊 تحلیل داده‌ها</a>
                <a href="#database-schema" class="nav-link p-3 rounded-lg font-semibold" data-target="database-schema">🗄️ ساختار پایگاه‌داده</a>
            </nav>
        </aside>

        <!-- Main Content -->
        <main class="flex-1 p-6 md:p-10">

            <!-- Section: Architecture -->
            <section id="architecture" class="section-active">
                <h2 class="text-3xl font-bold mb-4">🏛️ معماری و جریان داده پروژه</h2>
                <p class="mb-8 text-lg text-slate-600">این نمودار اجزای اصلی سیستم و نحوه ارتباط آن‌ها با یکدیگر را به صورت گرافیکی نمایش می‌دهد. روی هر بخش کلیک کنید تا توضیحات مربوط به آن را مشاهده نمایید.</p>

                <div class="bg-white p-6 rounded-xl shadow-md">
                    <div class="grid grid-cols-1 md:grid-cols-3 gap-8 items-center text-center">
                        <!-- User & Admin -->
                        <div class="space-y-4">
                            <div id="node-user" class="diagram-node bg-emerald-50 border border-emerald-200 p-4 rounded-lg">📱 کاربر در پلتفرم بله</div>
                            <div id="node-admin" class="diagram-node bg-rose-50 border border-rose-200 p-4 rounded-lg">👤 مدیر سیستم</div>
                        </div>

                        <!-- Backend Infrastructure -->
                        <div class="space-y-4">
                             <div class="text-sm text-slate-500 mb-2">▼ ارتباط از طریق API ▼</div>
                            <div id="node-bale" class="diagram-node bg-sky-50 border border-sky-200 p-4 rounded-lg">🌐 پلتفرم و API بله</div>
                             <div class="text-sm text-slate-500 my-2">▼ ارتباط با سرور ▼</div>
                            <div id="node-bot" class="diagram-node bg-indigo-50 border border-indigo-200 p-4 rounded-lg">🤖 ربات پایتون (پردازشگر اصلی)</div>
                            <div id="node-web" class="diagram-node bg-amber-50 border border-amber-200 p-4 rounded-lg">🖥️ داشبورد وب (FastAPI)</div>
                        </div>

                        <!-- Data Layer -->
                         <div class="space-y-4">
                            <div class="text-sm text-slate-500 mb-2">▼ ذخیره‌سازی ▼</div>
                            <div id="node-db" class="diagram-node bg-slate-100 border border-slate-300 p-4 rounded-lg">🐘 پایگاه داده PostgreSQL</div>
                        </div>
                    </div>
                </div>

                <div id="architecture-details" class="mt-8 p-6 bg-sky-50 border-2 border-sky-200 border-dashed rounded-lg text-slate-700 min-h-[120px]">
                    <p class="font-semibold text-sky-800">توضیحات در اینجا نمایش داده می‌شود. لطفاً روی یکی از اجزای نمودار کلیک کنید.</p>
                </div>
            </section>

            <!-- Section: Bot Deep Dive -->
            <section id="bot-deep-dive" class="section-hidden">
                <h2 class="text-3xl font-bold mb-4">🤖 نگاه عمیق به عملکرد ربات</h2>
                <p class="mb-8 text-lg text-slate-600">ربات به عنوان هسته اصلی سیستم، وظیفه دریافت، پردازش و پاسخ به پیام‌های کاربران را بر عهده دارد. در این بخش، جریان اصلی کار ربات را بررسی می‌کنیم.</p>

                <div class="bg-white p-8 rounded-xl shadow-md">
                    <h3 class="text-xl font-bold mb-6 text-center">جریان پردازش پیام کاربر</h3>
                    <div class="flex flex-col md:flex-row justify-between items-center gap-4 text-center">
                        <div class="p-4 bg-slate-100 rounded-lg w-full md:w-auto">۱. ارسال /start</div>
                        <div class="font-bold text-slate-400 text-2xl">→</div>
                        <div class="p-4 bg-slate-100 rounded-lg w-full md:w-auto">۲. انتخاب بخش مقصد</div>
                        <div class="font-bold text-slate-400 text-2xl">→</div>
                        <div class="p-4 bg-blue-100 rounded-lg w-full md:w-auto">۳. ارسال پیام (متن، عکس...)</div>
                        <div class="font-bold text-slate-400 text-2xl">→</div>
                         <div class="p-4 bg-blue-100 rounded-lg w-full md:w-auto">۴. اتمام و ارسال</div>
                        <div class="font-bold text-slate-400 text-2xl">→</div>
                        <div class="p-4 bg-green-100 rounded-lg w-full md:w-auto">۵. دریافت شماره تیکت</div>
                    </div>
                    <div class="mt-6 p-4 bg-gray-50 rounded-lg text-center">
                        <p>ربات با مدیریت وضعیت کاربر، به او اجازه می‌دهد چندین پیام را در حالت "مکالمه" ارسال کند و در نهایت تمام پیام‌ها به صورت یک تیکت واحد در سیستم ثبت می‌شوند. این کار از طریق جدول `user_states` در پایگاه داده مدیریت می‌شود.</p>
                    </div>
                </div>
            </section>

            <!-- Section: Dashboard Deep Dive -->
            <section id="dashboard-deep-dive" class="section-hidden">
                 <h2 class="text-3xl font-bold mb-4">🖥️ نگاه عمیق به عملکرد داشبورد</h2>
                <p class="mb-8 text-lg text-slate-600">داشبورد وب به مدیران امکان مدیریت و نظارت کامل بر سیستم را می‌دهد. یکی از ویژگی‌های کلیدی آن، فرایند ورود امن دو مرحله‌ای (2FA) است.</p>

                <div class="bg-white p-8 rounded-xl shadow-md">
                    <h3 class="text-xl font-bold mb-6 text-center">فرایند ورود امن به داشبورد (2FA)</h3>
                    <div class="flex flex-col md:flex-row justify-between items-center gap-4 text-center">
                        <div class="p-4 bg-slate-100 rounded-lg">۱. ورود نام کاربری و رمز</div>
                        <div class="font-bold text-slate-400 text-2xl">→</div>
                        <div class="p-4 bg-yellow-100 rounded-lg">۲. ارسال کد به ربات بله</div>
                         <div class="font-bold text-slate-400 text-2xl">→</div>
                        <div class="p-4 bg-yellow-100 rounded-lg">۳. ورود کد در مرورگر</div>
                        <div class="font-bold text-slate-400 text-2xl">→</div>
                        <div class="p-4 bg-green-100 rounded-lg">۴. دسترسی به داشبورد</div>
                    </div>
                    <div class="mt-6 p-4 bg-gray-50 rounded-lg text-center">
                        <p>این روش تضمین می‌کند که تنها افرادی که به حساب بله مدیر دسترسی دارند، می‌توانند وارد پنل مدیریتی شوند. توکن‌های دسترسی (JWT) پس از تأیید موفق، برای مدیریت نشست‌ها استفاده می‌شوند.</p>
                    </div>
                </div>
            </section>

            <!-- Section: Data Analytics -->
            <section id="data-analytics" class="section-hidden">
                <h2 class="text-3xl font-bold mb-4">📊 تحلیل داده‌ها و آمار</h2>
                 <p class="mb-8 text-lg text-slate-600">یکی از قابلیت‌های اصلی داشبورد، نمایش آمار عملکرد ربات به صورت گرافیکی است. در این بخش، می‌توانید نمونه‌ای تعاملی از این نمودارها را مشاهده کنید.</p>
                <div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
                    <div class="bg-white p-6 rounded-xl shadow-md">
                        <h3 class="text-xl font-bold mb-4 text-center">تفکیک پیام‌ها بر اساس بخش</h3>
                        <div class="chart-container">
                            <canvas id="destinationsChart"></canvas>
                        </div>
                    </div>
                    <div class="bg-white p-6 rounded-xl shadow-md">
                        <h3 class="text-xl font-bold mb-4 text-center">روند ارسال پیام (۷ روز گذشته)</h3>
                         <div class="chart-container">
                            <canvas id="dailyTrendChart"></canvas>
                        </div>
                    </div>
                </div>
            </section>

            <!-- Section: Database Schema -->
            <section id="database-schema" class="section-hidden">
                <h2 class="text-3xl font-bold mb-4">🗄️ ساختار پایگاه داده</h2>
                <p class="mb-8 text-lg text-slate-600">پایگاه داده PostgreSQL قلب تپنده سیستم است و تمام اطلاعات کاربران، پیام‌ها و وضعیت‌ها در آن ذخیره می‌شود. در زیر، ساختار جداول اصلی را مشاهده می‌کنید.</p>
                <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">

                    <div class="bg-white p-4 rounded-xl shadow-md border-t-4 border-blue-500">
                        <h3 class="text-lg font-bold mb-2">conversations (تیکت‌ها)</h3>
                        <ul class="text-sm space-y-1">
                            <li><strong>id:</strong> SERIAL (PK)</li>
                            <li><strong>user_id:</strong> TEXT</li>
                            <li><strong>ticket_id:</strong> TEXT (UNIQUE)</li>
                            <li><strong>destination:</strong> TEXT</li>
                            <li><strong>status:</strong> TEXT (open/closed)</li>
                            <li><strong>timestamp:</strong> TIMESTAMPTZ</li>
                        </ul>
                    </div>
                     <div class="bg-white p-4 rounded-xl shadow-md border-t-4 border-green-500">
                        <h3 class="text-lg font-bold mb-2">messages (پیام‌ها)</h3>
                        <ul class="text-sm space-y-1">
                            <li><strong>id:</strong> SERIAL (PK)</li>
                            <li><strong>conversation_id:</strong> INTEGER (FK)</li>
                            <li><strong>user_id:</strong> TEXT</li>
                            <li><strong>message_type:</strong> TEXT</li>
                            <li><strong>message_text:</strong> TEXT</li>
                            <li><strong>file_id:</strong> TEXT</li>
                        </ul>
                    </div>
                     <div class="bg-white p-4 rounded-xl shadow-md border-t-4 border-yellow-500">
                        <h3 class="text-lg font-bold mb-2">user_states (وضعیت کاربر)</h3>
                        <ul class="text-sm space-y-1">
                            <li><strong>user_id:</strong> TEXT (PK)</li>
                            <li><strong>state:</strong> TEXT</li>
                            <li><strong>timestamp:</strong> TIMESTAMPTZ</li>
                        </ul>
                    </div>
                     <div class="bg-white p-4 rounded-xl shadow-md border-t-4 border-purple-500">
                        <h3 class="text-lg font-bold mb-2">admins (مدیران)</h3>
                        <ul class="text-sm space-y-1">
                            <li><strong>id:</strong> SERIAL (PK)</li>
                            <li><strong>username:</strong> TEXT (UNIQUE)</li>
                            <li><strong>hashed_password:</strong> TEXT</li>
                            <li><strong>role:</strong> TEXT (admin/observer)</li>
                            <li><strong>bale_user_id:</strong> TEXT</li>
                        </ul>
                    </div>
                     <div class="bg-white p-4 rounded-xl shadow-md border-t-4 border-red-500">
                        <h3 class="text-lg font-bold mb-2">banned_users (کاربران مسدود)</h3>
                        <ul class="text-sm space-y-1">
                            <li><strong>user_id:</strong> TEXT (PK)</li>
                            <li><strong>reason:</strong> TEXT</li>
                            <li><strong>banned_by:</strong> TEXT</li>
                            <li><strong>timestamp:</strong> TIMESTAMPTZ</li>
                        </ul>
                    </div>

                </div>
            </section>
        </main>
    </div>

<script>
document.addEventListener('DOMContentLoaded', () => {
    const navLinks = document.querySelectorAll('#main-nav .nav-link');
    const sections = document.querySelectorAll('main section');

    // --- Navigation Logic ---
    function navigateTo(targetId) {
        sections.forEach(section => {
            section.classList.remove('section-active');
            section.classList.add('section-hidden');
        });
        document.getElementById(targetId).classList.add('section-active');
        document.getElementById(targetId).classList.remove('section-hidden');

        navLinks.forEach(link => {
            link.classList.remove('active');
            if (link.dataset.target === targetId) {
                link.classList.add('active');
            }
        });
    }

    navLinks.forEach(link => {
        link.addEventListener('click', (e) => {
            e.preventDefault();
            const targetId = e.currentTarget.dataset.target;
            navigateTo(targetId);
            window.history.pushState({id: targetId}, '', `#${targetId}`);
        });
    });

    // Handle back/forward navigation
    window.addEventListener('popstate', (e) => {
        if (e.state && e.state.id) {
            navigateTo(e.state.id);
        } else {
            navigateTo('architecture'); // Default page
        }
    });

    // --- Architecture Diagram Logic ---
    const architectureDetails = {
        'node-user': { title: '📱 کاربر', description: 'کاربر نهایی که از طریق پلتفرم «بله» با ربات تعامل می‌کند. تمام ارتباطات از طریق چت خصوصی با ربات انجام می‌شود.' },
        'node-admin': { title: '👤 مدیر سیستم', description: 'مدیر می‌تواند از دو طریق با سیستم کار کند: ۱. از طریق پاسخ به پیام‌ها و اجرای دستورات در کانال‌های بله. ۲. از طریق داشبورد مدیریتی وب برای مشاهده آمار و مدیریت جامع.' },
        'node-bale': { title: '� پلتفرم بله', description: 'واسط اصلی بین کاربران و ربات. تمام پیام‌ها، دستورات و پاسخ‌ها از طریق API این پلتفرم منتقل می‌شوند.' },
        'node-bot': { title: '🤖 ربات پایتون', description: 'هسته اصلی منطق برنامه. این ربات که با `asyncio` و `aiohttp` نوشته شده، آپدیت‌ها را از بله دریافت کرده، آن‌ها را پردازش می‌کند و پاسخ مناسب را ارسال می‌نماید.' },
        'node-web': { title: '🖥️ داشبورد وب', description: 'یک پنل مدیریتی مبتنی بر وب که با فریمورک `FastAPI` ساخته شده است. این پنل به مدیران امکان مشاهده آمار، مدیریت کاربران و پیام‌ها را در یک محیط گرافیکی می‌دهد.' },
        'node-db': { title: '🐘 پایگاه داده', description: 'تمام داده‌های پایدار سیستم، شامل اطلاعات کاربران، تیکت‌ها، پیام‌ها، وضعیت‌ها و لیست کاربران مسدود شده، در پایگاه داده `PostgreSQL` ذخیره می‌شود.' },
    };

    const detailsContainer = document.getElementById('architecture-details');
    document.querySelectorAll('.diagram-node').forEach(node => {
        node.addEventListener('click', () => {
            const details = architectureDetails[node.id];
            detailsContainer.innerHTML = `<h3 class="text-xl font-bold mb-2 text-sky-900">${details.title}</h3><p>${details.description}</p>`;
        });
    });

    // --- Chart.js Data & Initialization ---
    // Mock data based on the provided source code's capabilities
    const destinationsData = {
        labels: ['مدیریت', 'شورای صنفی', 'حراست', 'انجمن علمی کامپیوتر', 'خوابگاه برادران', 'انجمن ورزشی', 'بسیج'],
        datasets: [{
            label: 'تعداد پیام‌ها',
            data: [120, 95, 75, 68, 55, 40, 30],
            backgroundColor: [
                '#38bdf8', '#fbbf24', '#f87171', '#4ade80', '#818cf8', '#fb923c', '#a78bfa'
            ],
            hoverOffset: 4
        }]
    };

    const dailyTrendData = {
        labels: ['۶ روز پیش', '۵ روز پیش', '۴ روز پیش', '۳ روز پیش', 'دیروز', 'امروز'],
        datasets: [{
            label: 'پیام‌های دریافتی',
            data: [15, 22, 18, 28, 35, 45],
            fill: true,
            borderColor: '#0ea5e9',
            backgroundColor: 'rgba(14, 165, 233, 0.1)',
            tension: 0.3
        }]
    };

    // Chart instances
    let destinationsChart, dailyTrendChart;

    function createCharts() {
        const destCtx = document.getElementById('destinationsChart').getContext('2d');
        destinationsChart = new Chart(destCtx, {
            type: 'pie',
            data: destinationsData,
            options: {
                responsive: true,
                maintainAspectRatio: false,
                plugins: {
                    legend: {
                        position: 'bottom',
                         labels: { font: { family: 'Vazirmatn' } }
                    }
                }
            }
        });

        const trendCtx = document.getElementById('dailyTrendChart').getContext('2d');
        dailyTrendChart = new Chart(trendCtx, {
            type: 'line',
            data: dailyTrendData,
            options: {
                responsive: true,
                maintainAspectRatio: false,
                 plugins: {
                    legend: {
                         labels: { font: { family: 'Vazirmatn' } }
                    }
                },
                scales: {
                    y: { beginAtZero: true, ticks: { font: { family: 'Vazirmatn' } } },
                    x: { ticks: { font: { family: 'Vazirmatn' } } }
                }
            }
        });
    }

    // Initial page load check
    const initialHash = window.location.hash.substring(1);
    if (initialHash) {
        navigateTo(initialHash);
    } else {
        navigateTo('architecture');
    }

    createCharts();
});
</script>

</body>
</html>
�
