<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>College Complaint Management System</title>
  <style>
    body { 
      font-family: Arial, sans-serif; 
      margin: 0; 
      padding: 0;
      background-color: #e6f0ff; /* ✅ NEW BACKGROUND COLOR */
      color: #333;
      text-align: center;
    }

    /* ✅ CENTER BOX */
    .container {
      width: 80%;
      max-width: 900px;
      margin: 30px auto;
      background-color: #ffffff;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 0 10px rgba(0,0,0,0.2);
    }

    .screen { display: none; }
    .active { display: block; }

    button { margin: 5px; padding: 8px 12px; cursor: pointer; }

    .complaint-card { 
      border: 1px solid #ccc; 
      padding: 10px; 
      margin: 10px auto; 
      background-color: #0073e6; 
      color: white;
      width: 60%;
      text-align: left;
    }

    .status { font-weight: bold; }

    h1 { text-align: center; }

    label { font-weight: bold; }

    input, select, textarea { 
      margin-top: 5px; 
      margin-bottom: 15px; 
    }

    img.heading-img {
      display: block;
      margin: 0 auto 15px auto;
      width: 80px;
    }
  </style>
</head>

<body>

  <!-- ✅ CENTER BOX START -->
  <div class="container">

    <!-- Heading Image -->
    <img src="https://cdn-icons-png.flaticon.com/512/942/942748.png"
         alt="Complaint Management System"
         class="heading-img">

    <h1>College Complaint Management System</h1>

    <!-- Screen 1: User Dashboard -->
    <div id="dashboard" class="screen active">
      <h2>User Dashboard</h2>
      <button onclick="showScreen('form')">Submit Complaint</button>
      <button onclick="showScreen('admin')">Admin View</button>
      <h3>Recent Activity</h3>
      <div id="recentActivity">
        <p>No complaints yet.</p>
      </div>
    </div>

    <!-- Screen 2: Complaint Form -->
    <div id="form" class="screen">
      <h2>Submit Complaint</h2>
      <form id="complaintForm">
        <label>Student Name:</label><br>
        <input type="text" id="studentName"><br>

        <label>Roll Number:</label><br>
        <input type="text" id="rollNumber"><br>

        <label>Department:</label><br>
        <select id="department">
          <option value="IT">IT</option>
          <option value="CSE">CSE</option>
          <option value="EEE">EEE</option>
          <option value="ECE">ECE</option>
        </select><br>

        <label>Section:</label><br>
        <select id="section">
          <option value="A">A</option>
          <option value="B">B</option>
        </select><br>

        <label>Category:</label><br>
        <select id="category">
          <option value="Hostel">Hostel</option>
          <option value="Library">Library</option>
          <option value="Canteen">Canteen</option>
          <option value="Academics">Academics</option>
        </select><br>

        <label>Description:</label><br>
        <textarea id="description" rows="4" cols="40"></textarea><br>

        <button type="button" onclick="submitComplaint()">Submit</button>
        <button type="button" onclick="showScreen('dashboard')">Back</button>
      </form>
    </div>

    <!-- Screen 3: Admin View -->
    <div id="admin" class="screen">
      <h2>Admin View</h2>
      <div id="complaintsList"></div>
      <button onclick="showScreen('dashboard')">Back to Dashboard</button>
    </div>

  </div>
  <!-- ✅ CENTER BOX END -->

  <script>
    let complaints = [];

    function showScreen(screenId) {
      document.querySelectorAll('.screen').forEach(s => s.classList.remove('active'));
      document.getElementById(screenId).classList.add('active');
    }

    function submitComplaint() {
      const studentName = document.getElementById('studentName').value;
      const rollNumber = document.getElementById('rollNumber').value;
      const department = document.getElementById('department').value;
      const section = document.getElementById('section').value;
      const category = document.getElementById('category').value;
      const description = document.getElementById('description').value;

      if (!studentName || !rollNumber || !description) {
        alert("Please fill all required fields.");
        return;
      }

      const complaint = {
        studentName,
        rollNumber,
        department,
        section,
        category,
        description,
        status: "Pending"
      };
      complaints.push(complaint);

      updateRecentActivity();
      updateAdminView();

      document.getElementById('complaintForm').reset();
      alert("Complaint submitted successfully! You can enter another complaint.");
      showScreen('form');
    }

    function updateRecentActivity() {
      const activityDiv = document.getElementById('recentActivity');
      activityDiv.innerHTML = complaints.map((c, i) =>
        `<p>${i+1}. ${c.studentName} (${c.rollNumber}, ${c.department}-${c.section}) 
        [${c.category}] - ${c.description} (Status: ${c.status})</p>`
      ).join('');
    }

    function updateAdminView() {
      const listDiv = document.getElementById('complaintsList');
      listDiv.innerHTML = complaints.map((c, i) =>
        `<div class="complaint-card">
          <p><strong>Name:</strong> ${c.studentName}</p>
          <p><strong>Roll No:</strong> ${c.rollNumber}</p>
          <p><strong>Dept & Section:</strong> ${c.department}-${c.section}</p>
          <p><strong>Category:</strong> ${c.category}</p>
          <p><strong>Description:</strong> ${c.description}</p>
          <p class="status">Status: ${c.status}</p>
          <button onclick="markResolved(${i})">Mark as Resolved</button>
          <button onclick="escalate(${i})">Escalate</button>
        </div>`
      ).join('');
    }

    function markResolved(index) {
      complaints[index].status = "Resolved";
      updateRecentActivity();
      updateAdminView();
    }

    function escalate(index) {
      complaints[index].status = "Escalated";
      updateRecentActivity();
      updateAdminView();
    }
  </script>

</body>
</html>
