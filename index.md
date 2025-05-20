<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Samueliano的导航页</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Arial, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 40px 20px;
            position: relative;
        }

        .avatar-container {
            position: fixed;
            top: 20px;
            left: 20px;
            width: 60px;
            height: 60px;
            cursor: pointer;
            z-index: 100;
            overflow: visible;
        }

        .avatar {
            width: 100%;
            height: 100%;
            object-fit: cover;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            border-radius: 8px;
        }

        .avatar-tooltip {
            position: absolute;
            left: 60px;
            top: 0;
            width: 150px;
            background: white;
            border-radius: 8px;
            display: flex;
            flex-direction: column;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
            opacity: 0;
            visibility: hidden;
            transition: all 0.3s ease;
            z-index: 101;
            padding: 10px;
        }

        .avatar-container:hover .avatar-tooltip {
            opacity: 1;
            visibility: visible;
        }

        .tooltip-button {
            margin: 5px 0;
            padding: 8px 16px;
            background: #4CAF50;
            color: white;
            border: none;
            border-radius: 20px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-size: 12px;
            text-align: center;
        }

        .tooltip-button:hover {
            background: #45a049;
        }

        .title {
            margin-bottom: 20px;
            text-align: center;
            color: #333;
            font-size: 24px;
            font-weight: bold;
        }

        .search-container {
            width: 100%;
            max-width: 680px;
            margin-bottom: 40px;
        }

        .search-box {
            width: 100%;
            display: flex;
            gap: 12px;
            justify-content: center;
        }

        #searchInput {
            width: 100%;
            padding: 14px 28px;
            font-size: 16px;
            border: 2px solid #4CAF50;
            border-radius: 30px;
            outline: none;
            box-shadow: 0 3px 6px rgba(0, 0, 0, 0.1);
            transition: all 0.3s ease;
        }

        #searchButton {
            padding: 14px 32px;
            background: #4CAF50;
            color: white;
            border: none;
            border-radius: 30px;
            cursor: pointer;
            transition: all 0.3s ease;
            flex-shrink: 0;
        }

        .category-tabs {
            display: flex;
            gap: 10px;
            margin-bottom: 30px;
            flex-wrap: wrap;
            justify-content: center;
        }

        .category-tab {
            padding: 10px 25px;
            background: rgba(255, 255, 255, 0.8);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            cursor: pointer;
            transition: all 0.3s ease;
            border: 2px solid #e0e0e0;
            font-weight: 500;
        }

        .category-tab.active {
            background: #4CAF50;
            color: white;
            border-color: #4CAF50;
        }

        .category-content {
            width: 100%;
            max-width: 1000px;
            display: none;
        }

        .category-content.active {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
            gap: 20px;
            padding: 0 20px;
        }

        .nav-button {
            background: white;
            border-radius: 12px;
            padding: 20px;
            text-align: center;
            text-decoration: none;
            color: #333;
            transition: all 0.3s ease;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            display: flex;
            flex-direction: column;
            align-items: center;
            backdrop-filter: blur(10px);
            position: relative;
        }

        .nav-button:hover {
            transform: translateY(-5px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
        }

        .nav-icon {
            width: 32px;
            height: 32px;
            margin-bottom: 10px;
        }

        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            z-index: 1001;
            justify-content: center;
            align-items: center;
        }

        .modal-content {
            background: white;
            padding: 30px;
            border-radius: 10px;
            width: 90%;
            max-width: 500px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
        }

        .modal-title {
            margin-bottom: 20px;
            text-align: center;
            color: #333;
            font-size: 20px;
        }

        .form-group {
            margin-bottom: 15px;
        }

        .form-group label {
            display: block;
            margin-bottom: 5px;
            font-weight: 500;
        }

        .form-control {
            width: 100%;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 16px;
        }

        .modal-actions {
            display: flex;
            justify-content: flex-end;
            gap: 10px;
            margin-top: 20px;
        }

        .btn {
            padding: 8px 16px;
            border: none;
            border-radius: 5px;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .btn-primary {
            background: #4CAF50;
            color: white;
        }

        .btn-secondary {
            background: #f0f0f0;
            color: #333;
        }

        .delete-btn {
            position: absolute;
            top: 5px;
            right: 5px;
            width: 20px;
            height: 20px;
            background: #ff4d4d;
            color: white;
            border: none;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 12px;
            cursor: pointer;
            opacity: 0;
            transition: opacity 0.3s ease;
        }

        .nav-button:hover .delete-btn {
            opacity: 1;
        }

        .share-code-modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            z-index: 1002;
            justify-content: center;
            align-items: center;
        }

        .share-code-content {
            background: white;
            padding: 30px;
            border-radius: 10px;
            width: 90%;
            max-width: 500px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
        }

        .share-code-title {
            margin-bottom: 20px;
            text-align: center;
            color: #333;
            font-size: 20px;
        }

        .share-code-input {
            width: 100%;
            height: 120px;
            padding: 10px;
            border: 1px solid #ddd;
            border-radius: 5px;
            font-size: 14px;
            margin-bottom: 15px;
            resize: vertical;
        }

        .share-tabs {
            display: flex;
            margin-bottom: 20px;
            border-bottom: 1px solid #ddd;
        }

        .share-tab {
            padding: 10px 20px;
            cursor: pointer;
            background: #f5f5f5;
            border: 1px solid #ddd;
            border-bottom: none;
            margin-right: 5px;
            border-radius: 5px 5px 0 0;
        }

        .share-tab.active {
            background: white;
            border-bottom: 1px solid white;
            margin-bottom: -1px;
        }

        .share-tab-content {
            display: none;
        }

        .share-tab-content.active {
            display: block;
        }

        .copy-btn {
            background: #9E9E9E;
            color: white;
            border: none;
            border-radius: 5px;
            padding: 8px 12px;
            cursor: pointer;
            transition: all 0.3s ease;
            margin-left: 10px;
        }

        .copy-btn:hover {
            background: #757575;
        }

        .cat-image-container {
            position: fixed;
            bottom: 20px;
            right: 20px;
            width: 100px;
            height: 100px;
            border-radius: 50%;
            cursor: pointer;
            transition: all 0.3s ease;
            z-index: 100;
        }

        .cat-image {
            width: 100%;
            height: 100%;
            border-radius: 50%;
            object-fit: cover;
            border: 5px solid white;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            transition: all 0.3s ease;
        }

        .cat-image-container:hover .cat-image {
            transform: scale(1.1);
        }

        .cat-image-hint {
            position: absolute;
            top: -30px;
            left: 50%;
            transform: translateX(-50%);
            background: rgba(0, 0, 0, 0.7);
            color: white;
            padding: 5px 10px;
            border-radius: 15px;
            font-size: 12px;
            white-space: nowrap;
            opacity: 0;
            transition: opacity 0.5s ease;
            pointer-events: none;
            animation: fadeInOut 3s infinite;
        }

        .cat-image-container:hover .cat-image-hint {
            opacity: 1;
            animation: none;
        }

        @keyframes fadeInOut {
            0%, 100% { opacity: 0.5; }
            50% { opacity: 1; }
        }

        @media (max-width: 768px) {
            .category-content.active {
                grid-template-columns: repeat(2, 1fr);
            }

            .search-box {
                flex-direction: column;
                align-items: center;
            }

            #searchButton {
                width: 100%;
                max-width: 200px;
            }
        }

        @media (max-width: 480px) {
            .category-tabs {
                gap: 8px;
            }

            .category-tab {
                padding: 8px 16px;
                font-size: 14px;
            }

            .nav-button {
                padding: 15px;
            }
        }
    </style>
</head>
<body>
    <!-- 左上角头像及悬停提示框 -->
    <div class="avatar-container">
        <img src="https://img.picui.cn/free/2025/05/20/682bc85dbd295.jpg   " alt="用户头像" class="avatar">
        <div class="avatar-tooltip">
            <a href="#" class="tooltip-button" id="tooltipShareCodeBtn">分享码</a>
            <a href="#" class="tooltip-button" id="tooltipAddLinkBtn">添加链接</a>
        </div>
    </div>

    <h1 class="title">Samueliano的导航页</h1>
    <div class="search-container">
        <div class="search-box">
            <input type="text" id="searchInput" placeholder="输入关键词搜索...">
            <button id="searchButton">搜索</button>
        </div>
    </div>

    <!-- 分类标签 -->
    <div class="category-tabs">
        <div class="category-tab active" data-category="social">电脑测试</div>
        <div class="category-tab" data-category="shopping">软件</div>
        <div class="category-tab" data-category="learning">资源网站</div>
        <div class="category-tab" data-category="custom">我的收藏</div>
    </div>
<!-- 分类内容 -->
    <div class="category-content active" data-category="social">
        <a href="http://cpu.jy6d.com/    " class="nav-button" target="_blank">
            <img src="http://cpu.jy6d.com/favicon.ico    " alt="英大师图标" class="nav-icon">
            <span>英大师CPU在线测试</span>
        </a>
        <a href="https://topstip.com/screen/#welcome    " class="nav-button" target="_blank">
            <img src="https://topstip.com/wp-content/uploads/2024/06/cropped-topstip-logo-black-1-32x32.png    " alt="在线屏幕测试和检测" class="nav-icon">
            <span>在线屏幕测试和检测</span>
        </a>
        <a href="https://wangsuceshi.bmcx.com/    " class="nav-button" target="_blank">
            <img src="https://f.bmcx.com/file/wangsuceshi/i_c_o_57x57.png    " alt="网速测试" class="nav-icon">
            <span>网速测试</span>
        </a>
        <a href="https://skill-test.net/cn/mouse-buttons-test    " class="nav-button" target="_blank">
            <img src="https://skill-test.net/img/favicon.ico    " alt="鼠标测试" class="nav-icon">
            <span>鼠标测试</span>
        </a>
        <a href="https://www.zfrontier.com/lab/keyboardTester    " class="nav-button" target="_blank">
            <img src="https://img.zfrontier.com/favicon/apple-touch-icon.png    " alt="在线键盘测试" class="nav-icon">
            <span>在线键盘测试</span>
        </a>
    </div>

    <div class="category-content" data-category="shopping">
        <a href="https://kkgithub.com/zbezj/HEU_KMS_Activator/releases    " class="nav-button" target="_blank">
            <img src="https://kkgithub.com/fluidicon.png    " alt="kms激活" class="nav-icon">
            <span>kms激活</span>
        </a>
        <a href="https://www.huorong.cn/    " class="nav-button" target="_blank">
            <img src="https://www.huorong.cn/favicon.png    " alt="火绒" class="nav-icon">
            <span>火绒</span>
        </a>
        <a href="https://www.52pojie.cn/thread-1982699-1-1.html    " class="nav-button" target="_blank">
            <img src="https://www.sysceo.com/Public/images/favicon.ico    " alt="驱动总裁" class="nav-icon">
            <span>驱动总裁v2.18.0.10免扫码登录绿色单文件版</span>
        </a>
        <a href="https://www.360.com/    " class="nav-button" target="_blank">
            <img src="https://www.360.cn/favicon.ico    " alt="360" class="nav-icon">
            <span>360</span>
        </a>
        <a href="https://store.steampowered.com/about/    " class="nav-button" target="_blank">
            <img src="https://store.steampowered.com/favicon.ico    " alt="Steam" class="nav-icon">
            <span>Steam</span>
        </a>
        <a href="https://steampp.net/    " class="nav-button" target="_blank">
            <img src="https://steampp.net/favicon.ico    " alt="Watt Toolkit" class="nav-icon">
            <span>Watt Toolkit</span>
        </a>
        <a href="https://clash.download/clash-verge" class="nav-button" target="_blank">
            <img src="https://clash.download/wp-content/uploads/clash-48x48.webp" alt="ClashVerge" class="nav-icon">
            <span>ClashVerge</span>
        </a>
        <a href="https://www.tbtool.cn/    " class="nav-button" target="_blank">
            <img src="https://www.tbtool.cn/resource/icon.ico    " alt="图拉丁吧工具箱" class="nav-icon">
            <span>图拉丁吧工具箱</span>
        </a>
        <a href="https://im.qq.com/index/    " class="nav-button" target="_blank">
            <img src="https://qzonestyle.gtimg.cn/qzone/qzact/act/external/tiqq/logo.png    " alt="QQ" class="nav-icon">
            <span>QQ</span>
        </a>
        <a href="https://www.jetbrains.com/zh-cn/idea/download/?section=windows    " class="nav-button" target="_blank">
            <img src="https://www.jetbrains.com/favicon.ico?r=1234    " alt="IntelliJ IDEA" class="nav-icon">
            <span>IntelliJ IDEA</span>
        </a>
        <a href="https://weixin.qq.com/    " class="nav-button" target="_blank">
            <img src="https://res.wx.qq.com/a/wx_fed/assets/res/NTI4MWU5.ico    " alt="微信" class="nav-icon">
            <span>微信</span>
        </a>
    </div>

    <div class="category-content" data-category="learning">
        <a href="https://github.com    " class="nav-button" target="_blank">
            <img src="https://github.com/favicon.ico    " alt="GitHub" class="nav-icon">
            <span>GitHub</span>
        </a>
        <a href="https://kkgithub.com    " class="nav-button" target="_blank">
            <img src="https://kkgithub.com/fluidicon.png    " alt="GitHub(加速镜像)" class="nav-icon">
            <span>GitHub(加速镜像)</span>
        </a>
        <a href="https://gitee.com/    " class="nav-button" target="_blank">
            <img src="https://gitee.com/favicon.ico    " alt="Gitee" class="nav-icon">
            <span>Gitee</span>
        </a>
    </div>
    <!-- 自定义链接分类内容 -->
    <div class="category-content" data-category="custom" id="customLinksContainer">
        <!-- 动态添加用户自定义的链接 -->
    </div>

    <!-- 添加/编辑链接模态框 -->
    <div class="modal" id="linkModal">
        <div class="modal-content">
            <h3 class="modal-title" id="modalTitle">添加自定义链接</h3>
            <form id="linkForm">
                <input type="hidden" id="editIndex">
                <div class="form-group">
                    <label for="linkName">网站名称</label>
                    <input type="text" id="linkName" class="form-control" required>
                </div>
                <div class="form-group">
                    <label for="linkUrl">网站地址</label>
                    <input type="url" id="linkUrl" class="form-control" required>
                </div>
                <div class="form-group">
                    <label for="linkIcon">图标地址 (可选)</label>
                    <input type="url" id="linkIcon" class="form-control" placeholder="https://example.com/favicon.ico   ">
                </div>
                <div class="modal-actions">
                    <button type="button" class="btn btn-secondary" id="cancelBtn">取消</button>
                    <button type="submit" class="btn btn-primary">保存</button>
                </div>
            </form>
        </div>
    </div>

    <!-- 右下角添加链接按钮 -->
    <div class="cat-image-container">
        <img src="https://p.qlogo.cn/gh/1047830047/1047830047/640/   " alt="添加链接" class="cat-image">
        <div class="cat-image-hint">点击添加自定义链接</div>
    </div>

    <!-- 分享码弹窗 -->
    <div class="share-code-modal" id="shareCodeModal">
        <div class="share-code-content">
            <h3 class="share-code-title">分享自定义链接</h3>
            <div class="share-tabs">
                <button class="share-tab active" data-tab="export">导出</button>
                <button class="share-tab" data-tab="import">导入</button>
            </div>
            
            <div class="share-tab-content active" data-tab="export">
                <textarea class="share-code-input" id="exportCodeArea" readonly></textarea>
                <div style="display: flex; justify-content: space-between; align-items: center;">
                    <button class="btn btn-primary" id="generateShareCodeBtn">生成分享码</button>
                    <button class="copy-btn" id="copyShareCodeBtn">复制</button>
                </div>
            </div>
            
            <div class="share-tab-content" data-tab="import">
                <textarea class="share-code-input" id="importCodeArea" placeholder="在此粘贴分享码..."></textarea>
                <div style="display: flex; justify-content: space-between; align-items: center;">
                    <button class="btn btn-primary" id="importShareCodeBtn">导入链接</button>
                </div>
            </div>
            
            <div class="modal-actions">
                <button type="button" class="btn btn-secondary" id="closeShareCodeModal">关闭</button>
            </div>
        </div>
    </div>

    <script>
        // 初始化功能
        document.addEventListener('DOMContentLoaded', () => {
            // 分类切换功能
            document.querySelectorAll('.category-tab').forEach(tab => {
                tab.addEventListener('click', () => {
                    document.querySelectorAll('.category-tab, .category-content').forEach(el => {
                        el.classList.remove('active');
                    });
                    tab.classList.add('active');
                    const category = tab.dataset.category;
                    document.querySelector(`.category-content[data-category="${category}"]`).classList.add('active');
                });
            });

            // 搜索功能
            document.getElementById('searchButton').addEventListener('click', () => {
                const query = document.getElementById('searchInput').value.trim();
                if (query) {
                    window.location.href = `https://cn.bing.com/search?q=   ${encodeURIComponent(query)}`;
                }
            });

            document.getElementById('searchInput').addEventListener('keypress', (e) => {
                if (e.key === 'Enter') document.getElementById('searchButton').click();
            });

            // 自定义链接功能
            let customLinks = JSON.parse(localStorage.getItem('customLinks')) || [];
            
            function renderCustomLinks() {
                const container = document.getElementById('customLinksContainer');
                container.innerHTML = '';
                
                customLinks.forEach((link, index) => {
                    const linkElement = document.createElement('a');
                    linkElement.href = link.url;
                    linkElement.className = 'nav-button';
                    linkElement.target = '_blank';
                    
                    const icon = document.createElement('img');
                    icon.className = 'nav-icon';
                    icon.src = link.icon || 'data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCAyNCAyNCI+PHBhdGggZD0iTTEyLDJMMSwyMUgyM0wxMiwyTTEyLDkuN0wxOC41MywyMUg1LjQ3TDEyLDkuN1oiIGZpbGw9IiM0Y2FmNTAiLz48L3N2Zz4=';
                    icon.alt = link.name + '图标';
                    
                    const name = document.createElement('span');
                    name.textContent = link.name;
                    
                    const deleteBtn = document.createElement('button');
                    deleteBtn.className = 'delete-btn';
                    deleteBtn.innerHTML = '×';
                    deleteBtn.addEventListener('click', (e) => {
                        e.preventDefault();
                        e.stopPropagation();
                        if (confirm(`确定要删除 "${link.name}" 吗？`)) {
                            customLinks.splice(index, 1);
                            saveCustomLinks();
                            renderCustomLinks();
                        }
                    });
                    
                    linkElement.appendChild(icon);
                    linkElement.appendChild(name);
                    linkElement.appendChild(deleteBtn);
                    container.appendChild(linkElement);
                });
                
                if (customLinks.length === 0) {
                    container.innerHTML = '<p style="grid-column: 1/-1; text-align: center; color: #666;">点击右下角图片添加自定义链接</p>';
                }
            }

            function saveCustomLinks() {
                localStorage.setItem('customLinks', JSON.stringify(customLinks));
            }

            // 模态框功能
            function showModal(title, linkData = null) {
                document.getElementById('modalTitle').textContent = title;
                const form = document.getElementById('linkForm');
                
                if (linkData) {
                    document.getElementById('editIndex').value = linkData.index;
                    document.getElementById('linkName').value = linkData.name;
                    document.getElementById('linkUrl').value = linkData.url;
                    document.getElementById('linkIcon').value = linkData.icon || '';
                } else {
                    form.reset();
                }
                
                document.getElementById('linkModal').style.display = 'flex';
                document.getElementById('linkName').focus();
            }

            function hideModal() {
                document.getElementById('linkModal').style.display = 'none';
            }

            document.getElementById('cancelBtn').addEventListener('click', hideModal);

            document.getElementById('linkForm').addEventListener('submit', (e) => {
                e.preventDefault();
                
                const urlInput = document.getElementById('linkUrl').value.trim();
                if (!isValidUrl(urlInput)) {
                    alert("请输入有效的URL地址（需包含http://或https://）");
                    return;
                }

                const linkName = document.getElementById('linkName').value.trim();
                if (!linkName) {
                    alert("请输入链接名称");
                    return;
                }

                const linkData = {
                    name: linkName,
                    url: urlInput,
                    icon: document.getElementById('linkIcon').value.trim() || null
                };

                const editIndex = document.getElementById('editIndex').value;
                
                if (editIndex) {
                    customLinks[editIndex] = linkData;
                } else {
                    customLinks.push(linkData);
                }
                
                saveCustomLinks();
                renderCustomLinks();
                hideModal();
            });

            function isValidUrl(url) {
                try {
                    new URL(url);
                    return url.startsWith('http://') || url.startsWith('https://');
                } catch {
                    return false;
                }
            }

            // 分享码功能
            const shareCodeModal = document.getElementById('shareCodeModal');
            const closeShareCodeModal = document.getElementById('closeShareCodeModal');
            const exportCodeArea = document.getElementById('exportCodeArea');
            const importCodeArea = document.getElementById('importCodeArea');
            const generateShareCodeBtn = document.getElementById('generateShareCodeBtn');
            const copyShareCodeBtn = document.getElementById('copyShareCodeBtn');
            const importShareCodeBtn = document.getElementById('importShareCodeBtn');
            const shareTabs = document.querySelectorAll('.share-tab');

            shareTabs.forEach(tab => {
                tab.addEventListener('click', () => {
                    const tabName = tab.dataset.tab;
                    shareTabs.forEach(t => t.classList.remove('active'));
                    tab.classList.add('active');
                    document.querySelectorAll('.share-tab-content').forEach(content => {
                        content.classList.remove('active');
                        if (content.dataset.tab === tabName) {
                            content.classList.add('active');
                        }
                    });
                });
            });

            function generateShareCode() {
                if (customLinks.length === 0) {
                    alert("请先添加自定义链接再生成分享码");
                    return;
                }
                
                try {
                    const jsonStr = encodeURIComponent(JSON.stringify(customLinks));
                    const shareCode = btoa(jsonStr);
                    exportCodeArea.value = shareCode;
                    alert("分享码已生成，可复制分享");
                } catch (error) {
                    alert("生成分享码失败：" + error.message);
                    console.error("生成分享码错误:", error);
                }
            }

            function copyShareCode() {
                if (!exportCodeArea.value) {
                    alert("请先生成分享码");
                    return;
                }
                
                exportCodeArea.select();
                document.execCommand('copy');
                alert("已复制到剪贴板！");
            }

            function importShareCode() {
                const shareCode = importCodeArea.value.trim();
                
                if (!shareCode) {
                    alert("请输入有效的分享码");
                    return;
                }
                
                try {
                    const decodedStr = decodeURIComponent(atob(shareCode));
                    const importedLinks = JSON.parse(decodedStr);
                    
                    if (!Array.isArray(importedLinks)) {
                        throw new Error("无效的分享码格式");
                    }

                    const newLinks = importedLinks.filter(newLink => 
                        !customLinks.some(existingLink => 
                            existingLink.url === newLink.url
                        )
                    );
                    
                    if (newLinks.length === 0) {
                        alert("没有可导入的新链接");
                        return;
                    }

                    customLinks = [...customLinks, ...newLinks];
                    saveCustomLinks();
                    renderCustomLinks();
                    
                    alert(`成功导入 ${newLinks.length} 个新链接`);
                    importCodeArea.value = '';
                } catch (error) {
                    alert("分享码无效或已损坏：" + error.message);
                    console.error("导入分享码错误:", error);
                }
            }

            // 事件绑定
            document.getElementById('tooltipShareCodeBtn').addEventListener('click', (e) => {
                e.preventDefault();
                shareCodeModal.style.display = 'flex';
            });

            document.querySelectorAll('.cat-image-container, #tooltipAddLinkBtn').forEach(el => {
                el.addEventListener('click', () => {
                    showModal('添加自定义链接');
                });
            });

            closeShareCodeModal.addEventListener('click', () => {
                shareCodeModal.style.display = 'none';
            });

            generateShareCodeBtn.addEventListener('click', generateShareCode);
            copyShareCodeBtn.addEventListener('click', copyShareCode);
            importShareCodeBtn.addEventListener('click', importShareCode);

            shareCodeModal.addEventListener('click', (e) => {
                if (e.target === shareCodeModal) {
                    shareCodeModal.style.display = 'none';
                }
            });

            // 初始渲染
            renderCustomLinks();
        });
    </script>
</body>
</html>
