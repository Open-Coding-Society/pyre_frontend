---
layout: tailwind
search_exclude: true
hide: true
show_reading_time: false
permalink: /help/
title: Help
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Help Center</title>
    <style>
        body {
            background-image: url("../../images/background9674.png");
            background-size: cover;
            background-position: center;
            background-repeat: no-repeat;
            background-attachment: fixed;
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }

        .help-header {
            background: rgba(0, 0, 0, 0.95);
            backdrop-filter: blur(10px);
            border-radius: 12px;
            padding: 30px;
            margin-bottom: 30px;
            text-align: center;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
        }

        .help-tabs {
            display: flex;
            background: rgba(0, 0, 0, 0.9);
            border-radius: 12px;
            padding: 8px;
            margin-bottom: 20px;
            overflow-x: auto;
        }

        .tab-button {
            flex: 1;
            padding: 12px 20px;
            border: none;
            background: transparent;
            border-radius: 8px;
            cursor: pointer;
            font-weight: 500;
            transition: all 0.3s ease;
            white-space: nowrap;
        }

        .tab-button.active {
            background: #b91c1c;
            color: white;
        }

        .tab-button:hover:not(.active) {
            background: rgba(255, 255, 255, 0.1);
        }

        .tab-content {
            display: none;
            background: rgba(0, 0, 0, 0.95);
            backdrop-filter: blur(10px);
            border-radius: 12px;
            padding: 30px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
        }

        .tab-content.active {
            display: block;
        }

        .form-group {
            margin-bottom: 20px;
        }

        .form-label {
            display: block;
            margin-bottom: 8px;
            font-weight: 600;
            color: #333;
        }

        .form-input, .form-select, .form-textarea {
            width: 100%;
            padding: 12px;
            border: 2px solid rgb(0, 0, 0);
            border-radius: 8px;
            font-size: 16px;
            transition: border-color 0.3s ease;
            box-sizing: border-box;
            min-width: 0;
        }

        .form-input, .form-select {
            height: auto;
        }

        .form-textarea {
            min-height: 120px;
            resize: vertical;
        }

        .form-input:focus, .form-select:focus, .form-textarea:focus {
            outline: none;
            border-color: #b91c1c;
        }    .form-textarea {
            min-height: 120px;
            resize: vertical;
        }

        .btn {
            padding: 12px 24px;
            border: none;
            border-radius: 8px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            text-decoration: none;
            display: inline-block;
            text-align: center;
        }

        .btn-primary {
            background: #b91c1c;
            color: white;
        }

        .btn-primary:hover {
            background: #991b1b;
        }

        .btn-secondary {
            background: #6b7280;
            color: white;
        }

        .btn-secondary:hover {
            background: #4b5563;
        }

        .request-card, .faq-item {
            background: black;
            border-radius: 8px;
            padding: 20px;
            margin-bottom: 16px;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
            border-left: 4px solid #b91c1c;
        }

        .request-header {
            display: flex;
            justify-content: space-between;
            align-items: flex-start;
            margin-bottom: 12px;
        }

        .request-title {
            font-size: 18px;
            font-weight: 600;
            color:rgb(62, 66, 73);
        }

        .status-badge {
            padding: 4px 12px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: 600;
            text-transform: uppercase;
        }

        .status-open { background:rgb(0, 0, 0); color: #dc2626; }
        .status-in_progress { background: #fef3c7; color: #d97706; }
        .status-resolved { background: #dcfce7; color: #16a34a; }
        .status-closed { background: #f3f4f6; color: #6b7280; }

        .priority-badge {
            padding: 2px 8px;
            border-radius: 12px;
            font-size: 11px;
            font-weight: 600;
            margin-left: 8px;
        }

        .priority-low { background: #f0f9ff; color: #0369a1; }
        .priority-medium { background: #fef3c7; color: #d97706; }
        .priority-high { background: #fee2e2; color: #dc2626; }
        .priority-urgent { background: #fecaca; color: #991b1b; }

        .search-box {
            position: relative;
            margin-bottom: 20px;
        }

        .search-input {
            width: 100%;
            padding: 12px 16px 12px 44px;
            border: 2px solid #e5e7eb;
            border-radius: 8px;
            font-size: 16px;
        }

        .search-icon {
            position: absolute;
            left: 14px;
            top: 50%;
            transform: translateY(-50%);
            color: #6b7280;
        }

        .filters {
            display: flex;
            gap: 16px;
            margin-bottom: 24px;
            flex-wrap: wrap;
            }

            .filter-select {
                min-width: 220px;
                font-size: 1.2em;
                padding: 18px 22px;
                height: 56px;
            }
            .loading {    text-align: center;
            padding: 40px;
            color:rgb(0, 0, 0);
        }

        .error {
            background:rgb(0, 0, 0);
            color: #dc2626;
            padding: 12px;
            border-radius: 8px;
            margin-bottom: 20px;
        }

        .success {
            background: #dcfce7;
            color: #16a34a;
            padding: 12px;
            border-radius: 8px;
            margin-bottom: 20px;
        }

        .faq-question {
            font-weight: 600;
            cursor: pointer;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .faq-answer {
            display: none;
            margin-top: 12px;
            padding-top: 12px;
            border-top: 1px solidrgb(0, 0, 0);
            color: #4b5563;
            line-height: 1.6;
        }

        .faq-answer.show {
            display: block;
        }

        .chevron {
            transition: transform 0.3s ease;
        }

        .chevron.rotate {
            transform: rotate(180deg);
        }

        .request-detail {
            background:rgb(44, 45, 46);
            border-radius: 8px;
            padding: 16px;
            margin-top: 12px;
        }

        .pagination {
            display: flex;
            justify-content: center;
            gap: 8px;
            margin-top: 20px;
        }

        .page-btn {
            padding: 8px 12px;
            border: 1px solid #d1d5db;
            background: white;
            border-radius: 4px;
            cursor: pointer;
        }

        .page-btn.active {
            background: #b91c1c;
            color: white;
            border-color: #b91c1c;
        }

        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .stat-card {
            background: #23272f; /* dark, not black */    border-radius: 8px;
            padding: 20px;
            text-align: center;
            box-shadow: 0 2px 8px rgba(121, 22, 22, 0.59);
        }

        .stat-number {
            font-size: 2em;
            font-weight: bold;
            color: #b91c1c;
        }

        .stat-label {
            color: #6b7280;
            margin-top: 4px;
        }

        .message-area {
            position: fixed;
            top: 20px;
            right: 20px;
            z-index: 1000;
        }

        .message {
            padding: 12px 20px;
            border-radius: 8px;
            margin-bottom: 10px;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
            opacity: 0;
            transform: translateX(100%);
            animation: slideIn 0.3s ease forwards;
        }

        .message.success {
            background: #dcfce7;
            color: #16a34a;
            border: 1px solid #bbf7d0;
        }

        .message.error {
            background: #fee2e2;
            color: #dc2626;
            border: 1px solid #fecaca;
        }

        @keyframes slideIn {
            to {
                opacity: 1;
                transform: translateX(0);
            }
        }

        @keyframes slideOut {
            to {
                opacity: 0;
                transform: translateX(100%);
            }
        }

        @media (max-width: 768px) {
            .container {
                padding: 10px;
            }
            
            .help-tabs {
                flex-direction: column;
            }
            
            .filters {
                flex-direction: column;
            }
            
            .request-header {
                flex-direction: column;
                gap: 8px;
            }

            .message-area {
                left: 20px;
                right: 20px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="help-header">
            <h1 style="color: #b91c1c; margin: 0 0 10px 0; font-size: 2.5em;">Help Center</h1>
            <p style="color: #6b7280; margin: 0; font-size: 1.1em;">Get help, find answers, and manage your support requests</p>
        </div>

        <div class="help-tabs">
            <button class="tab-button active" onclick="showTab('faqs')">📚 FAQs</button>
            <button class="tab-button" onclick="showTab('my-requests')">📋 My Requests</button>
            <button class="tab-button" onclick="showTab('new-request')">➕ New Request</button>
            <button class="tab-button" id="admin-tab" onclick="showTab('admin')" style="display: none;">⚙️ Admin</button>
        </div>

        <!-- FAQ Tab -->
        <div id="faqs" class="tab-content active">
            <div class="search-box">
                <input type="text" class="search-input" id="faq-search" placeholder="Search FAQs...">
                <span class="search-icon">🔍</span>
            </div>
            
            <div class="filters">
                <select class="form-select filter-select" id="faq-category-filter">
                    <option value="all">All Categories</option>
                    <option value="technical">Technical</option>
                    <option value="account">Account</option>
                    <option value="general">General</option>
                    <option value="billing">Billing</option>
                </select>
            </div>

            <div id="faq-list"></div>
        </div>

        <!-- My Requests Tab -->
        <div id="my-requests" class="tab-content">
            <div class="filters">
                <select class="form-select filter-select" id="status-filter">
                    <option value="all">All Status</option>
                    <option value="open">Open</option>
                    <option value="in_progress">In Progress</option>
                    <option value="resolved">Resolved</option>
                    <option value="closed">Closed</option>
                </select>
                
                <select class="form-select filter-select" id="category-filter">
                    <option value="all">All Categories</option>
                    <option value="technical">Technical</option>
                    <option value="account">Account</option>
                    <option value="general">General</option>
                    <option value="bug_report">Bug Report</option>
                    <option value="feature_request">Feature Request</option>
                    <option value="billing">Billing</option>
                    <option value="other">Other</option>
                </select>
                
                <select class="form-select filter-select" id="priority-filter">
                    <option value="all">All Priorities</option>
                    <option value="low">Low</option>
                    <option value="medium">Medium</option>
                    <option value="high">High</option>
                    <option value="urgent">Urgent</option>
                </select>
                
                <button class="btn btn-secondary" onclick="loadMyRequests()">🔄 Refresh</button>
            </div>

            <div id="my-requests-list"></div>
            <div id="requests-pagination"></div>
        </div>

        <!-- New Request Tab -->
        <div id="new-request" class="tab-content">
            <form id="help-request-form">
                <div class="form-group">
                    <label class="form-label" for="request-title">Title *</label>
                    <input type="text" class="form-input" id="request-title" required placeholder="Brief description of your issue">
                </div>

                <div class="form-group">
                    <label class="form-label" for="request-category">Category *</label>
                    <select class="form-select" id="request-category" required>
                        <option value="">Select a category</option>
                        <option value="technical">Technical Issue</option>
                        <option value="account">Account Problem</option>
                        <option value="general">General Question</option>
                        <option value="bug_report">Bug Report</option>
                        <option value="feature_request">Feature Request</option>
                        <option value="billing">Billing</option>
                        <option value="other">Other</option>
                    </select>
                </div>

                <div class="form-group">
                    <label class="form-label" for="request-priority">Priority</label>
                    <select class="form-select" id="request-priority">
                        <option value="low">Low</option>
                        <option value="medium" selected>Medium</option>
                        <option value="high">High</option>
                        <option value="urgent">Urgent</option>
                    </select>
                </div>

                <div class="form-group">
                    <label class="form-label" for="request-description">Description *</label>
                    <textarea class="form-textarea" id="request-description" required placeholder="Please provide detailed information about your issue or question..."></textarea>
                </div>

                <div class="form-group">
                    <label class="form-label" for="contact-email">Contact Email</label>
                    <input type="email" class="form-input" id="contact-email" placeholder="Optional: Different email for responses">
                </div>

                <button type="submit" class="btn btn-primary">Submit Request</button>
            </form>
        </div>

        <!-- Admin Tab -->
        <div id="admin" class="tab-content">
            <div id="admin-stats" class="stats-grid"></div>
            
            <div class="filters">
                <select class="form-select filter-select" id="admin-status-filter">
                    <option value="all">All Status</option>
                    <option value="open">Open</option>
                    <option value="in_progress">In Progress</option>
                    <option value="resolved">Resolved</option>
                    <option value="closed">Closed</option>
                </select>
                
                <button class="btn btn-secondary" onclick="loadAllRequests()">🔄 Refresh</button>
                <button class="btn btn-primary" onclick="showCreateFaqForm()">➕ Add FAQ</button>
            </div>

            <div id="admin-requests-list"></div>
        </div>

        <!-- Message Area -->
        <div id="message-area" class="message-area"></div>
    </div>

    <script type="module">
        import { pythonURI, fetchOptions } from '../assets/js/api/config.js';
        
        let currentUser = null;
        let currentPage = 1;
        let isAdmin = false;

        // Initialize the page
        document.addEventListener('DOMContentLoaded', async function() {
            await checkUserAuth();
            await loadFAQs();
            setupEventListeners();
        });

        async function checkUserAuth() {
            try {
                // Check if user is authenticated and get user info
                const response = await fetch(`${pythonURI}/api/user`, fetchOptions);
                if (response.ok) {
                    currentUser = await response.json();
                    isAdmin = currentUser.role === 'Admin';
                    
                    if (isAdmin) {
                        document.getElementById('admin-tab').style.display = 'block';
                    }
                }
            } catch (error) {
                console.error('Error checking authentication:', error);
            }
        }

        function setupEventListeners() {
            // FAQ search
            document.getElementById('faq-search').addEventListener('input', debounce(searchFAQs, 300));
            document.getElementById('faq-category-filter').addEventListener('change', loadFAQs);
            
            // Request filters
            document.getElementById('status-filter').addEventListener('change', loadMyRequests);
            document.getElementById('category-filter').addEventListener('change', loadMyRequests);
            document.getElementById('priority-filter').addEventListener('change', loadMyRequests);
            
            // Form submission
            document.getElementById('help-request-form').addEventListener('submit', submitHelpRequest);
        }

        window.showTab = function(tabName) {
            // Hide all tabs
            document.querySelectorAll('.tab-content').forEach(tab => {
                tab.classList.remove('active');
            });
            document.querySelectorAll('.tab-button').forEach(btn => {
                btn.classList.remove('active');
            });
            
            // Show selected tab
            document.getElementById(tabName).classList.add('active');
            event.target.classList.add('active');
            
            // Load data for the tab
            switch(tabName) {
                case 'my-requests':
                    loadMyRequests();
                    break;
                case 'admin':
                    if (isAdmin) {
                        loadAdminStats();
                        loadAllRequests();
                    }
                    break;
            }
        }

        async function loadFAQs() {
            const category = document.getElementById('faq-category-filter').value;
            const search = document.getElementById('faq-search').value;
            
            try {
                let url = `${pythonURI}/api/help/faqs`;
                const params = new URLSearchParams();
                if (category !== 'all') params.append('category', category);
                if (search) params.append('search', search);
                if (params.toString()) url += '?' + params.toString();
                
                const response = await fetch(url);
                const data = await response.json();
                
                displayFAQs(data.faqs);
            } catch (error) {
                console.error('Error loading FAQs:', error);
                showMessage('Error loading FAQs', 'error');
            }
        }

        function displayFAQs(faqGroups) {
            const container = document.getElementById('faq-list');
            container.innerHTML = '';
            
            if (Object.keys(faqGroups).length === 0) {
                container.innerHTML = '<div class="loading">No FAQs found</div>';
                return;
            }
            
            Object.entries(faqGroups).forEach(([category, faqs]) => {
                const categoryHeader = document.createElement('h3');
                categoryHeader.textContent = category.charAt(0).toUpperCase() + category.slice(1).replace('_', ' ');
                categoryHeader.style.color = '#b91c1c';
                categoryHeader.style.marginTop = '30px';
                categoryHeader.style.marginBottom = '15px';
                container.appendChild(categoryHeader);
                
                faqs.forEach(faq => {
                    const faqElement = document.createElement('div');
                    faqElement.className = 'faq-item';
                    faqElement.innerHTML = `
                        <div class="faq-question" onclick="toggleFaq(this)">
                            <span>${faq.question}</span>
                            <span class="chevron">▼</span>
                        </div>
                        <div class="faq-answer">
                            ${faq.answer}
                        </div>
                    `;
                    container.appendChild(faqElement);
                });
            });
        }

        window.toggleFaq = function(element) {
            const answer = element.nextElementSibling;
            const chevron = element.querySelector('.chevron');
            
            answer.classList.toggle('show');
            chevron.classList.toggle('rotate');
        }

        async function searchFAQs() {
            await loadFAQs();
        }

        window.loadMyRequests = async function() {
            if (!currentUser) return;
            
            const status = document.getElementById('status-filter').value;
            const category = document.getElementById('category-filter').value;
            const priority = document.getElementById('priority-filter').value;
            
            try {
                let url = `${pythonURI}/api/help/requests`;
                const params = new URLSearchParams();
                if (status !== 'all') params.append('status', status);
                if (category !== 'all') params.append('category', category);
                if (priority !== 'all') params.append('priority', priority);
                params.append('page', currentPage);
                if (params.toString()) url += '?' + params.toString();
                
                const response = await fetch(url, fetchOptions);
                const data = await response.json();
                
                displayRequests(data.requests, 'my-requests-list');
                displayPagination(data.pagination, 'requests-pagination');
            } catch (error) {
                console.error('Error loading requests:', error);
                showMessage('Error loading requests', 'error');
            }
        }

        window.loadAllRequests = async function() {
            if (!isAdmin) return;
            
            const status = document.getElementById('admin-status-filter').value;
            
            try {
                let url = `${pythonURI}/api/help/requests`;
                const params = new URLSearchParams();
                if (status !== 'all') params.append('status', status);
                params.append('admin', 'true');
                if (params.toString()) url += '?' + params.toString();
                
                const response = await fetch(url, fetchOptions);
                const data = await response.json();
                
                displayRequests(data.requests, 'admin-requests-list', true);
            } catch (error) {
                console.error('Error loading admin requests:', error);
                showMessage('Error loading requests', 'error');
            }
        }

        function displayRequests(requests, containerId, isAdmin = false) {
            const container = document.getElementById(containerId);
            container.innerHTML = '';
            
            if (requests.length === 0) {
                container.innerHTML = '<div class="loading">No requests found</div>';
                return;
            }
            
            requests.forEach(request => {
                const requestElement = document.createElement('div');
                requestElement.className = 'request-card';
                requestElement.innerHTML = `
                    <div class="request-header">
                        <div>
                            <div class="request-title">${request.title}</div>
                            <div style="margin-top: 4px; color:rgb(139, 141, 144); font-size: 14px;">
                                ${request.category.replace('_', ' ')} • ${formatDate(request.created_date)}
                                ${isAdmin ? ` • User: ${request.user_name || 'Unknown'}` : ''}
                            </div>
                        </div>
                        <div>
                            <span class="status-badge status-${request.status}">${request.status.replace('_', ' ')}</span>
                            <span class="priority-badge priority-${request.priority}">${request.priority}</span>
                        </div>
                    </div>
                    <div class="request-detail">
                        <p style="margin: 0; color: #4b5563;">${request.description}</p>
                        ${isAdmin ? `
                            <div style="margin-top: 12px; padding-top: 12px; border-top: 1px solidrgb(39, 39, 40);">
                                <button class="btn btn-primary" onclick="respondToRequest(${request.id})" style="margin-right: 8px;">Respond</button>
                                <button class="btn btn-secondary" onclick="updateRequestStatus(${request.id})">Update Status</button>
                            </div>
                        ` : ''}
                    </div>
                `;
                container.appendChild(requestElement);
            });
        }

        function displayPagination(pagination, containerId) {
            const container = document.getElementById(containerId);
            if (!pagination || pagination.pages <= 1) {
                container.innerHTML = '';
                return;
            }
            
            let paginationHtml = '<div class="pagination">';
            
            if (pagination.has_prev) {
                paginationHtml += `<button class="page-btn" onclick="changePage(${pagination.page - 1})">Previous</button>`;
            }
            
            for (let i = 1; i <= pagination.pages; i++) {
                const activeClass = i === pagination.page ? ' active' : '';
                paginationHtml += `<button class="page-btn${activeClass}" onclick="changePage(${i})">${i}</button>`;
            }
            
            if (pagination.has_next) {
                paginationHtml += `<button class="page-btn" onclick="changePage(${pagination.page + 1})">Next</button>`;
            }
            
            paginationHtml += '</div>';
            container.innerHTML = paginationHtml;
        }

        window.changePage = function(page) {
            currentPage = page;
            loadMyRequests();
        }

        async function submitHelpRequest(event) {
            event.preventDefault();
            
            const formData = {
                title: document.getElementById('request-title').value,
                category: document.getElementById('request-category').value,
                priority: document.getElementById('request-priority').value,
                description: document.getElementById('request-description').value,
                contact_email: document.getElementById('contact-email').value
            };
            
            try {
                const response = await fetch(`${pythonURI}/api/help/requests`, {
                    ...fetchOptions,
                    method: 'POST',
                    headers: {
                        ...fetchOptions.headers,
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify(formData)
                });
                
                const data = await response.json();
                
                if (response.ok) {
                    showMessage('Help request submitted successfully!', 'success');
                    document.getElementById('help-request-form').reset();
                    setTimeout(() => showTab('my-requests'), 1000);
                } else {
                    showMessage(data.message || 'Error submitting request', 'error');
                }
            } catch (error) {
                console.error('Error submitting request:', error);
                showMessage('Error submitting request', 'error');
            }
        }

        async function loadAdminStats() {
            if (!isAdmin) return;
            
            try {
                const response = await fetch(`${pythonURI}/api/help/statistics`, fetchOptions);
                const data = await response.json();
                
                displayAdminStats(data);
            } catch (error) {
                console.error('Error loading admin stats:', error);
            }
        }

        function displayAdminStats(stats) {
            const container = document.getElementById('admin-stats');
            container.innerHTML = `
                <div class="stat-card">
                    <div class="stat-number">${stats.requests.total}</div>
                    <div class="stat-label">Total Requests</div>
                </div>
                <div class="stat-card">
                    <div class="stat-number">${stats.requests.open}</div>
                    <div class="stat-label">Open Requests</div>
                </div>
                <div class="stat-card">
                    <div class="stat-number">${stats.requests.in_progress}</div>
                    <div class="stat-label">In Progress</div>
                </div>
                <div class="stat-card">
                    <div class="stat-number">${stats.requests.resolved}</div>
                    <div class="stat-label">Resolved</div>
                </div>
            `;
        }

        window.respondToRequest = async function(requestId) {
            const response = prompt('Enter your response:');
            if (!response) return;
            
            try {
                const resp = await fetch(`${pythonURI}/api/help/requests/${requestId}/response`, {
                    ...fetchOptions,
                    method: 'POST',
                    headers: {
                        ...fetchOptions.headers,
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({ 
                        response: response,
                        new_status: 'in_progress'
                    })
                });
                
                if (resp.ok) {
                    showMessage('Response sent successfully!', 'success');
                    loadAllRequests();
                } else {
                    showMessage('Error sending response', 'error');
                }
            } catch (error) {
                console.error('Error responding to request:', error);
                showMessage('Error sending response', 'error');
            }
        }

        window.updateRequestStatus = async function(requestId) {
            const newStatus = prompt('Enter new status (open, in_progress, resolved, closed):');
            if (!newStatus || !['open', 'in_progress', 'resolved', 'closed'].includes(newStatus)) {
                showMessage('Invalid status', 'error');
                return;
            }
            
            try {
                const response = await fetch(`${pythonURI}/api/help/requests/${requestId}/status`, {
                    ...fetchOptions,
                    method: 'PUT',
                    headers: {
                        ...fetchOptions.headers,
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({ status: newStatus })
                });
                
                if (response.ok) {
                    showMessage('Status updated successfully!', 'success');
                    loadAllRequests();
                } else {
                    showMessage('Error updating status', 'error');
                }
            } catch (error) {
                console.error('Error updating status:', error);
                showMessage('Error updating status', 'error');
            }
        }

        window.showCreateFaqForm = function() {
            const question = prompt('Enter FAQ question:');
            if (!question) return;
            
            const answer = prompt('Enter FAQ answer:');
            if (!answer) return;
            
            const category = prompt('Enter FAQ category (technical, account, general, billing):');
            if (!category || !['technical', 'account', 'general', 'billing'].includes(category)) {
                showMessage('Invalid category', 'error');
                return;
            }
            
            createFaq(question, answer, category);
        }

        async function createFaq(question, answer, category) {
            try {
                const response = await fetch(`${pythonURI}/api/help/faqs`, {
                    ...fetchOptions,
                    method: 'POST',
                    headers: {
                        ...fetchOptions.headers,
                        'Content-Type': 'application/json'
                    },
                    body: JSON.stringify({
                        question: question,
                        answer: answer,
                        category: category
                    })
                });
                
                if (response.ok) {
                    showMessage('FAQ created successfully!', 'success');
                    loadFAQs();
                } else {
                    showMessage('Error creating FAQ', 'error');
                }
            } catch (error) {
                console.error('Error creating FAQ:', error);
                showMessage('Error creating FAQ', 'error');
            }
        }

        function formatDate(dateString) {
            return new Date(dateString).toLocaleDateString('en-US', {
                year: 'numeric',
                month: 'short',
                day: 'numeric',
                hour: '2-digit',
                minute: '2-digit'
            });
        }

        function showMessage(message, type) {
            const messageArea = document.getElementById('message-area');
            const messageElement = document.createElement('div');
            messageElement.className = `message ${type}`;
            messageElement.textContent = message;
            
            messageArea.appendChild(messageElement);
            
            // Auto-remove after 5 seconds
            setTimeout(() => {
                messageElement.style.animation = 'slideOut 0.3s ease forwards';
                setTimeout(() => {
                    if (messageElement.parentNode) {
                        messageElement.parentNode.removeChild(messageElement);
                    }
                }, 300);
            }, 5000);
        }

        function debounce(func, wait) {
            let timeout;
            return function executedFunction(...args) {
                const later = () => {
                    clearTimeout(timeout);
                    func(...args);
                };
                clearTimeout(timeout);
                timeout = setTimeout(later, wait);
            };
        }
    </script>
</body>
</html>