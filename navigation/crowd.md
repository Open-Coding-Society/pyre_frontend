---
layout: tailwind
search_exclude: true
hide: true
show_reading_time: false
permalink: /crowd/
title: Report
---

<div class="min-h-screen bg-gray-100 text-gray-900 py-8 px-4">
  <div class="max-w-4xl mx-auto">
    <div class="mb-8 text-center">
      <h1 class="text-3xl font-bold text-red-600">Fire Hazard Reporting</h1>
<<<<<<< HEAD
      <p class="mt-2 text-gray-600">Help keep our community safe by reporting potential fire hazards</p>
=======
      <p class="mt-2 text-gray-600">Help keep our community safe and healthy by reporting potential fire hazards</p>
>>>>>>> ac833d53ba64975681e3b996ae5a2b62e99b3592
    </div>
    <div class="bg-white rounded-lg shadow-md p-6 mb-8">
      <h2 class="text-xl font-semibold mb-4">Report a Fire Hazard</h2>
      <form id="reportForm" class="space-y-4">
        <div>
          <label for="location" class="block text-sm font-medium text-gray-700 mb-1">Location</label>
          <input type="text" id="location" class="w-full px-3 py-2 border border-gray-300 rounded-md" placeholder="Enter location description or coordinates" required>
        </div>
        <div>
          <label for="hazardType" class="block text-sm font-medium text-gray-700 mb-1">Hazard Type</label>
          <select id="hazardType" class="w-full px-3 py-2 border border-gray-300 rounded-md" required>
            <option value="">Select hazard type</option>
            <option value="Unattended Campfire">Unattended Campfire</option>
            <option value="Burning Debris">Burning Debris</option>
            <option value="Discarded Cigarette">Discarded Cigarette</option>
            <option value="Lightning Strike">Lightning Strike</option>
            <option value="Other">Other (specify in description)</option>
          </select>
        </div>
        <div>
          <label for="description" class="block text-sm font-medium text-gray-700 mb-1">Description</label>
          <textarea id="description" rows="3" class="w-full px-3 py-2 border border-gray-300 rounded-md" placeholder="Describe the hazard and potential risk" required></textarea>
        </div>
        <div>
          <label for="photoUpload" class="block text-sm font-medium text-gray-700 mb-1">Photo Evidence <span class="text-green-500">*optional</span></label>
          <input type="file" id="photoUpload" class="w-full" accept="image/*">
          <div id="imagePreview" class="mt-2 hidden">
            <img id="previewImg" class="h-40 object-cover rounded" alt="Preview">
          </div>
        </div>
        <div>
          <label for="contactInfo" class="block text-sm font-medium text-gray-700 mb-1">Contact Info (optional)</label>
          <input type="text" id="contactInfo" class="w-full px-3 py-2 border border-gray-300 rounded-md" placeholder="Your name and phone/email (optional)">
        </div>
        <div>
          <button type="submit" class="w-full bg-red-600 hover:bg-red-700 text-white font-medium py-2 px-4 rounded-md transition">
            Submit Report
          </button>
        </div>
      </form>
    </div>
    <div class="flex justify-between items-center mb-4">
      <h2 class="text-xl font-semibold">Recent Reports</h2>
      <div class="flex space-x-2">
        <select id="filterStatus" class="px-3 py-1 border border-gray-300 rounded-md text-sm">
          <option value="all">All Reports</option>
          <option value="pending">Pending</option>
          <option value="verified">Verified</option>
          <option value="resolved">Resolved</option>
        </select>
        <button id="adminModeToggle" class="px-3 py-1 border border-gray-300 rounded-md text-sm bg-gray-200 hover:bg-gray-300">
          Admin Mode
        </button>
      </div>
    </div>
    <div id="reportsList" class="space-y-4">
      <!-- Reports will be inserted here by JavaScript -->
    </div>
    <div id="adminModal" class="fixed inset-0 bg-black bg-opacity-50 flex items-center justify-center hidden">
      <div class="bg-white rounded-lg p-6 max-w-md w-full">
        <h3 class="text-lg font-semibold mb-4">Admin Authentication</h3>
        <input type="password" id="adminPassword" class="w-full px-3 py-2 border border-gray-300 rounded-md mb-4" placeholder="Enter admin password">
        <div class="flex justify-end space-x-2">
          <button id="cancelAdminAuth" class="px-4 py-2 border border-gray-300 rounded-md hover:bg-gray-100">Cancel</button>
          <button id="confirmAdminAuth" class="px-4 py-2 bg-blue-600 text-white rounded-md hover:bg-blue-700">Login</button>
        </div>
      </div>
    </div>
  </div>
</div>

<script type="module">
  // Import configuration
  import { pythonURI, fetchOptions } from '/QcommVNE_Frontend/assets/js/api/config.js';

  // Initialize variables
  let db_reports = [];
  let dataLoaded = false;

  // Fetch reports from the database
  async function fetchReports() {
    try {
      const response = await fetch(`${pythonURI}/api/emails`, fetchOptions);
      if (!response.ok) {
        throw new Error('Failed to fetch reports: ' + response.statusText);
      }

      const reports = await response.json();
      console.log("API response:", reports);

      // Clear existing reports before adding new ones
      db_reports = [];

      // Process reports from the API
      if (reports && reports.length > 0) {
        reports.forEach(report => {
          const newReport = {
            id: report.id,
            location: report.location || "Unknown Location",
            hazardType: report.hazard_type || "Unspecified Hazard",
            description: report.description || "No description provided",
            photoUrl: 'https://media.istockphoto.com/id/587213412/photo/smoking-cigarette-on-black-background.jpg?s=612x612&w=0&k=20&c=OYsuZU-1InoPZaissXB1MJ4HdASwhqq-5NO3SC2q8TM=',
            timestamp: report.date || new Date().toISOString(),
            status: report.status || "pending",
            contactInfo: report.email_id || ""
          };

          db_reports.unshift(newReport);
        });
      }

      dataLoaded = true;
      console.log("DB reports after loading:", db_reports);
      console.log("DB reports info after loading:", typeof db_reports[0]);

      return db_reports;
    } catch (error) {
      console.error("Error fetching reports:", error);
      return [];
    }
  }

  // UI elements
  let isAdminMode = false;
  const ADMIN_PASSWORD = "fireadmin123"; // admin feature is temporary - will integrate with login base later

  const reportForm = document.getElementById('reportForm');
  const photoUpload = document.getElementById('photoUpload');
  const imagePreview = document.getElementById('imagePreview');
  const previewImg = document.getElementById('previewImg');
  const reportsList = document.getElementById('reportsList');
  const filterStatus = document.getElementById('filterStatus');
  const adminModeToggle = document.getElementById('adminModeToggle');
  const adminModal = document.getElementById('adminModal');
  const adminPassword = document.getElementById('adminPassword');
  const confirmAdminAuth = document.getElementById('confirmAdminAuth');
  const cancelAdminAuth = document.getElementById('cancelAdminAuth');

  // Initialize application when DOM is loaded
  document.addEventListener('DOMContentLoaded', async function() {
    await loadInitialData();
    setupEventListeners();
  });

  // Load initial data
  async function loadInitialData() {
    await fetchReports();
    renderReports();
  }

  // Setup all event listeners
  function setupEventListeners() {
    // Photo upload preview
    photoUpload.addEventListener('change', function(e) {
      const file = e.target.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = function(event) {
          previewImg.src = event.target.result;
          imagePreview.classList.remove('hidden');
        };
        reader.readAsDataURL(file);
      } else {
        imagePreview.classList.add('hidden');
      }
    });

    // Form submission
    reportForm.addEventListener('submit', function(e) {
      e.preventDefault();
      submitReport();
    });

    // Filter change
    filterStatus.addEventListener('change', function() {
      renderReports();
    });

    // Admin mode toggle
    adminModeToggle.addEventListener('click', function() {
      if (isAdminMode) {
        isAdminMode = false;
        adminModeToggle.classList.remove('bg-blue-600', 'text-white');
        adminModeToggle.classList.add('bg-gray-200');
        adminModeToggle.textContent = 'Admin Mode';
        renderReports();
      } else {
        adminModal.classList.remove('hidden');
      }
    });

    // Admin authentication
    confirmAdminAuth.addEventListener('click', function() {
      if (adminPassword.value === ADMIN_PASSWORD) {
        isAdminMode = true;
        adminModal.classList.add('hidden');
        adminModeToggle.classList.remove('bg-gray-200');
        adminModeToggle.classList.add('bg-blue-600', 'text-white');
        adminModeToggle.textContent = 'Exit Admin Mode';
        renderReports();
      } else {
        alert('Invalid password');
      }
    });

    cancelAdminAuth.addEventListener('click', function() {
      adminModal.classList.add('hidden');
    });
  }

  // Submit a new report
  async function submitReport() {
    // Get form values
    const timestamp = new Date().toISOString();
    const contactInfo = document.getElementById('contactInfo').value;
    const description = document.getElementById('description').value;
    const hazardType = document.getElementById('hazardType').value;
    const location = document.getElementById('location').value;
    
    try {
      // Send report to backend
      const response = await fetch(`${pythonURI}/api/email`, {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json'
        },
        credentials: 'include',
        body: JSON.stringify({
          "date": timestamp, 
          "email_id": contactInfo, 
          "subject": "FIRE", 
          "sender": contactInfo, 
          "description": description, 
          "hazard_type": hazardType, 
          "location": location
        })
      });

      if (!response.ok) {
        throw new Error('Failed to submit report');
      }

      // Add report to local array so we don't need to refetch
      const newReport = {
        id: db_reports.length > 0 ? Math.max(...db_reports.map(r => r.id)) + 1 : 1,
        location: location,
        hazardType: hazardType,
        description: description,
        photoUrl: previewImg.src || "/api/placeholder/400/300",
        timestamp: timestamp,
        status: "pending",
        contactInfo: contactInfo
      };

      // Add to local data
      db_reports.unshift(newReport);
      
      // Reset form
      reportForm.reset();
      imagePreview.classList.add('hidden');

      // Render updated reports
      renderReports();
      
      // Show confirmation
      alert('Report submitted successfully!');
    } catch (error) {
      console.error("Error submitting report:", error);
      alert('Failed to submit report. Please try again.');
    }
  }

  // Render reports based on filter
  function renderReports() {
    // Clear the reports list
    reportsList.innerHTML = '';

    // If no data is loaded yet, show loading message
    if (db_reports.length === 0) {
      reportsList.innerHTML = '<div class="text-center p-8 text-gray-500">No reports available</div>';
      return;
    }

    // Apply filtering
    const statusFilter = filterStatus.value;
    const filteredReports = statusFilter === 'all' 
      ? db_reports
      : db_reports.filter(report => report.status === statusFilter);

    // Check if we have any reports after filtering
    if (filteredReports.length === 0) {
      reportsList.innerHTML = '<div class="text-center p-8 text-gray-500">No reports match the selected filter</div>';
      return;
    }

    // Render each report
    filteredReports.forEach(report => {
      const reportEl = document.createElement('div');
      reportEl.className = 'bg-white rounded-lg shadow-md overflow-hidden';

      // Determine status styling
      let statusColor = 'bg-yellow-500';
      if (report.status === 'verified') statusColor = 'bg-blue-500';
      if (report.status === 'resolved') statusColor = 'bg-green-500';

      // Create report HTML
      reportEl.innerHTML = `
        <div class="md:flex">
          <div class="md:flex-shrink-0">
            <img class="h-48 w-full object-cover md:w-48" src="${report.photoUrl}" alt="Report image">
          </div>
          <div class="p-4 flex-grow">
            <div class="flex justify-between items-start">
              <div>
                <p class="text-sm font-medium text-gray-600">${new Date(report.timestamp).toLocaleString()}</p>
                <h3 class="text-xl font-semibold mt-1">${report.hazardType}</h3>
                <p class="mt-1 text-gray-900 font-medium">${report.location}</p>
              </div>
              <span class="px-2 py-1 text-xs font-bold uppercase rounded text-white ${statusColor}">
                ${report.status}
              </span>
            </div>
            <p class="mt-3 text-gray-700">${report.description}</p>
            ${report.contactInfo ? `<p class="mt-2 text-sm text-gray-600">Reported by: ${report.contactInfo}</p>` : ''}

            ${isAdminMode ? `
              <div class="mt-4 flex space-x-2">
                <button class="verify-btn px-3 py-1 bg-blue-100 text-blue-700 rounded-md text-sm hover:bg-blue-200" data-id="${report.id}">
                  Mark Verified
                </button>
                <button class="resolve-btn px-3 py-1 bg-green-100 text-green-700 rounded-md text-sm hover:bg-green-200" data-id="${report.id}">
                  Mark Resolved
                </button>
                <button class="delete-btn px-3 py-1 bg-red-100 text-red-700 rounded-md text-sm hover:bg-red-200" data-id="${report.id}">
                  Delete
                </button>
              </div>
            ` : ''}
          </div>
        </div>
      `;

      reportsList.appendChild(reportEl);
    });

    // Add event listeners to admin buttons if in admin mode
    if (isAdminMode) {
      document.querySelectorAll('.verify-btn').forEach(btn => {
        btn.addEventListener('click', function() {
          const id = parseInt(this.getAttribute('data-id'));
          updateReportStatus(id, 'verified');
        });
      });

      document.querySelectorAll('.resolve-btn').forEach(btn => {
        btn.addEventListener('click', function() {
          const id = parseInt(this.getAttribute('data-id'));
          updateReportStatus(id, 'resolved');
        });
      });

      document.querySelectorAll('.delete-btn').forEach(btn => {
        btn.addEventListener('click', function() {
          const id = parseInt(this.getAttribute('data-id'));
          deleteReport(id);
        });
      });
    }
  }

  // Update report status
  async function updateReportStatus(id, status) {
    const report = db_reports.find(r => r.id === id);
    if (report) {
      try {
        // In a real implementation, you would update the status in the backend here
        // await fetch(`${pythonURI}/api/emails/${id}/status`, {
        //   method: 'PUT',
        //   headers: {
        //     'Content-Type': 'application/json'
        //   },
        //   credentials: 'include',
        //   body: JSON.stringify({ status: status })
        // });

        // Update local data
        report.status = status;
        renderReports();
      } catch (error) {
        console.error("Error updating report status:", error);
        alert('Failed to update report status. Please try again.');
      }
    }
  }

  // Delete a report
  async function deleteReport(id) {
    if (confirm('Are you sure you want to delete this report?')) {
      try {
        // In a real implementation, you would delete the report in the backend here
        // await fetch(`${pythonURI}/api/emails/${id}`, {
        //   method: 'DELETE',
        //   credentials: 'include'
        // });

        // Update local data
        db_reports = db_reports.filter(r => r.id !== id);
        renderReports();
      } catch (error) {
        console.error("Error deleting report:", error);
        alert('Failed to delete report. Please try again.');
      }
    }
  }
</script>