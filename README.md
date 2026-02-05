<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>School System</title>
<style>
  body { margin:0; font-family: Arial, sans-serif; background:black; }
  .login { height:100vh; display:flex; justify-content:center; align-items:center; }
  .box { background:skyblue; padding:25px; width:300px; border-radius:8px; box-shadow:0 10px 20px rgba(0,0,0,0.2); text-align:center; }
  input, button, textarea, select { width:100%; padding:10px; margin-top:10px; }
  button { background:#007bff; color:gold yellow; border:none; cursor:pointer; }
  .dashboard { display:none; height:100vh; }
  .header { background:#003366; color gold yellow; padding:15px; text-align:center; font-size:18px; }
  .container { display:flex; height: calc(100vh - 60px); }
  .menu { width:220px; background:white; padding:10px; box-shadow:2px 0 10px rgba(0,0,0,0.1); display:flex; flex-direction:column; transition:width 0.3s; }
  .menu.collapsed { width:60px; }
  .menu button { background:#eee; color:black; margin-top:6px; font-size:14px; white-space:nowrap; overflow:hidden; }
  .menu.collapsed button { font-size:0; }
  .menu.collapsed button::before { content:"•"; font-size:18px; }
  .toggle-btn { background:#003366; color:white; font-size:18px; }
  .logout { margin-top:auto; background:#d9534f !important; color:white !important; font-size:14px !important; }
  .content { flex:1; padding:20px; text-align:center; display:flex; flex-direction:column; align-items:center; overflow-y:auto; }
  .section { background:white; padding:15px; border-radius:5px; width:90%; margin-top:15px; }
  img.school { max-width:80%; border-radius:10px; margin-bottom:20px; }
  table { border-collapse:collapse; width:100%; margin-bottom:15px; }
  table, th, td { border:1px solid #ccc; text-align:center; padding:5px; }
  .edit { border:1px solid #ccc; }
  .view-only { background:#f9f9f9; pointer-events:none; }
  .subsection { background:#f9f9f9; padding:10px; margin-top:10px; border-radius:5px; text-align:left; }
</style>
</head>
<body>

<!-- LOGIN -->
<div class="login" id="login">
  <div class="box">
    <h2>Login</h2>
    <input type="password" id="password" placeholder="Enter password">
    <button onclick="login()">Login</button>
    <p id="msg"></p>
  </div>
</div>

<!-- DASHBOARD -->
<div class="dashboard" id="dashboard">
  <div class="header" id="roleTitle"></div>
  <div class="container">
    <div class="menu" id="menu"></div>
    <div class="content" id="content">
      <img src="https://source.unsplash.com/800x400/?school" alt="School" class="school">
      <div class="section">
        <h3>Welcome</h3>
        <p>Select an option from the menu.</p>
      </div>
    </div>
  </div>
</div>

<script>
// Role and student tracking
let role = "";
let studentName = ""; // for logged-in student
let childName = "";   // for parent

const students = ["Jibdel","Viennes","Jurl","Sam","Justine","Ashley","Gerlie","Meriem"];
const gradesData = {
  "1st": { "Entrepreneurship": {}, "Three eyes": {}, "🧢 🗿": {}, "Math": {}, "Pi6": {} },
  "2nd": { "Entrepreneurship": {}, "Three eyes": {}, "🧢 🗿": {}, "Math": {}, "Pi6": {} }
};

// Fill grades with sample values
students.forEach(s=>{
  gradesData["1st"]["Entrepreneurship"][s]="A";
  gradesData["1st"]["Three eyes"][s]="B+";
  gradesData["1st"]["🧢 🗿"][s]="C";
  gradesData["1st"]["Math"][s]="A-";
  gradesData["1st"]["Pi6"][s]="B";
  
  gradesData["2nd"]["Entrepreneurship"][s]="A-";
  gradesData["2nd"]["Three eyes"][s]="A";
  gradesData["2nd"]["🧢 🗿"][s]="B+";
  gradesData["2nd"]["Math"][s]="A";
  gradesData["2nd"]["Pi6"][s]="B+";
});

// Mapping passwords to roles/students
const roleMapping = {
  "admingwaposijibdel": { role: "teacher" },
  "studentgwaposijibdel": { role: "student", name: "Jibdel" },
  "parentgwaposijibdel": { role: "parent", child: "Jibdel" }
};

/* ---------- LOGIN ---------- */
function login() {
  const pass = document.getElementById("password").value;
  if(!roleMapping[pass]) { document.getElementById("msg").innerText="Invalid password"; return; }

  role = roleMapping[pass].role;
  studentName = roleMapping[pass].name || "";
  childName = roleMapping[pass].child || "";

  document.getElementById("login").style.display="none";
  document.getElementById("dashboard").style.display="block";
  loadDashboard();
}

/* ---------- DASHBOARD ---------- */
function loadDashboard() {
  document.getElementById("roleTitle").innerText = role.toUpperCase() + " DASHBOARD";

  const menu = document.getElementById("menu");
  menu.innerHTML = "";

  // Toggle
  const toggle = document.createElement("button");
  toggle.innerText="☰"; toggle.className="toggle-btn"; toggle.onclick=toggleMenu;
  menu.appendChild(toggle);

  // Menu items
  let items = [];
  if(role==="teacher") items=["Attendance","Grades","Student Files","Parents","Subjects","Schedule","Bulletin Board"];
  if(role==="student") items=["Attendance","Grades","Schedule","AI Chat","Bulletin Board","Planner"];
  if(role==="parent") items=["Child Attendance","Child Grades","Child Schedule","Teacher Chat","Bulletin Board"];

  createMenu(items);

  // Logout/back button
  const logoutBtn=document.createElement("button");
  logoutBtn.innerText="⬅ Back"; logoutBtn.className="logout";
  logoutBtn.onclick=logout;
  menu.appendChild(logoutBtn);

  showSchoolPhoto();
}

/* ---------- MENU ---------- */
function createMenu(items){
  const menu=document.getElementById("menu");
  items.forEach(item=>{
    const btn=document.createElement("button");
    btn.innerText=item;
    btn.onclick=()=>loadSection(item);
    menu.appendChild(btn);
  });
}

/* ---------- CONTENT ---------- */
function loadSection(name){
  const content=document.getElementById("content");
  content.innerHTML="";
  const img=document.createElement("img");
  img.src="https://source.unsplash.com/800x400/?school"; img.className="school";
  content.appendChild(img);

  let editable = (role==="teacher");
  let editClass = editable?"edit":"view-only";

  /* ---------- Attendance ---------- */
  if(name==="Attendance"||name==="Child Attendance"){
    const section=document.createElement("div"); section.className="section";
    const dates=["2026-02-01","2026-02-02","2026-02-03","2026-02-04","2026-02-05","2026-02-06","2026-02-07"];

    let tableHTML = "<h3>Attendance Table</h3><table><tr><th>Name</th>";
    dates.forEach(d=>tableHTML+=`<th>${d}</th>`); tableHTML+="</tr>";

    students.forEach(s=>{
      if(role==="student" && s!==studentName) return;
      if(role==="parent" && s!==childName) return;
      tableHTML+=`<tr><td>${s}</td>`;
      dates.forEach(()=>{ tableHTML+=`<td><input type="checkbox" ${editable?"":"disabled"}></td>`; });
      tableHTML+="</tr>";
    });
    tableHTML+="</table>";
    tableHTML+=`<h3>Check Attendance by Date</h3><label for="attDate">Select Date: </label>
                <input type="date" id="attDate" min="2026-02-01" max="2026-02-07">
                <div id="dateAttendance" style="margin-top:10px;"></div>`;
    section.innerHTML = tableHTML;
    content.appendChild(section);

    const dateInput=section.querySelector("#attDate");
    const dateDiv=section.querySelector("#dateAttendance");
    dateInput.addEventListener("change",()=>{
      const selectedDate=dateInput.value; if(!selectedDate) return;
      let listHTML=`<h4>Attendance for ${selectedDate}</h4><ul>`;
      students.forEach(s=>{
        if(role==="student" && s!==studentName) return;
        if(role==="parent" && s!==childName) return;
        const status=Math.random()>0.2?"Present":"Absent";
        listHTML+=`<li>${s}: ${status}</li>`;
      });
      listHTML+="</ul>"; dateDiv.innerHTML=listHTML;
    });
    return;
  }

  /* ---------- Grades ---------- */
  if(name==="Grades"||name==="Child Grades"){
    const section=document.createElement("div"); section.className="section";
    section.innerHTML=`<h3>${name}</h3>
      <label for="semesterSelect">Select Semester: </label>
      <select id="semesterSelect">
        <option value="1st">1st Semester</option>
        <option value="2nd">2nd Semester</option>
      </select>
      <div id="gradesContent" style="margin-top:15px;"></div>`;
    content.appendChild(section);

    const gradesContent=section.querySelector("#gradesContent");
    const semesterSelect=section.querySelector("#semesterSelect");

    function renderGrades(sem){
      let html=`<table><tr><th>Subject</th><th>Grade</th></tr>`;
      for(let subj in gradesData[sem]){
        students.forEach(s=>{
          if(role==="student" && s!==studentName) return;
          if(role==="parent" && s!==childName) return;
          html+=`<tr><td>${subj}</td>
                 <td><input type="text" value="${gradesData[sem][subj][s]}" class="${editable?"edit":"view-only"}" ${editable?"":"disabled"}></td></tr>`;
        });
      }
      html+="</table>";
      gradesContent.innerHTML=html;
    }

    renderGrades("1st");
    semesterSelect.addEventListener("change",()=>renderGrades(semesterSelect.value));
    return;
  }

  /* ---------- Student Files ---------- */
  if(name==="Student Files"){
    const section=document.createElement("div"); section.className="section";
    section.innerHTML=`<h3>${name}</h3><h4>Performance Review</h4>`;
    students.forEach(s=>{
      const div=document.createElement("div"); div.className="subsection";
      div.innerHTML=`<strong>${s}</strong><br>
        <textarea class="${editable?"edit":"view-only"}" ${editable?"":"disabled"}>Performance review for ${s}.</textarea>`;
      section.appendChild(div);
    });
    content.appendChild(section); return;
  }

  /* ---------- Parents ---------- */
  if(name==="Parents"){
    const section=document.createElement("div"); section.className="section";
    section.innerHTML=`<h3>${name}</h3>
      <div class="subsection">
        <h4>Chat with Parent</h4>
        <textarea placeholder="Type message..." class="${editable?"edit":"view-only"}" ${editable?"":"disabled"}></textarea>
        <button disabled>Send</button>
      </div>`;
    content.appendChild(section); return;
  }

  /* ---------- Planner ---------- */
  if(name==="Planner"){
    const section=document.createElement("div"); section.className="section";
    section.innerHTML="<h3>Planner</h3>";
    const days=["Monday","Tuesday","Wednesday","Thursday","Friday","Saturday","Sunday"];
    days.forEach(day=>{
      const div=document.createElement("div"); div.className="subsection";
      div.innerHTML=`<strong>${day}</strong><br>
        <textarea class="${editable?"edit":"view-only"}" ${editable?"":"disabled"} placeholder="Write plans here..."></textarea>`;
      section.appendChild(div);
    });
    content.appendChild(section); return;
  }

  /* ---------- Subjects ---------- */
  if(name==="Subjects"){
    const section=document.createElement("div"); section.className="section";
    const subjects=["Entrepreneurship","Three eyes","🧢 🗿","Math","Pi6"];
    section.innerHTML=`<h3>${name}</h3>`;
    subjects.forEach(sub=>{
      const div=document.createElement("div"); div.className="subsection";
      div.innerHTML=`<strong>${sub}</strong><br>Schedule: <input type="text" value="Mon 8AM" class="${editable?"edit":"view-only"}" ${editable?"":"disabled"}>`;
      section.appendChild(div);
    });
    content.appendChild(section); return;
  }

  /* ---------- Bulletin Board ---------- */
  if(name==="Bulletin Board"){
    const section=document.createElement("div"); section.className="section";
    section.innerHTML=`<h3>${name}</h3>`;
    const subsections=["Suspended Classes","Announcement"];
    subsections.forEach(sub=>{
      const div=document.createElement("div"); div.className="subsection";
      div.innerHTML=`<strong>${sub}</strong><br>
        <textarea class="${editable?"edit":"view-only"}" ${editable?"":"disabled"}>Content for ${sub}.</textarea>`;
      section.appendChild(div);
    });
    content.appendChild(section); return;
  }

  /* ---------- Chat Sections ---------- */
  if(name.includes("Chat")){
    const section=document.createElement("div"); section.className="section";
    section.innerHTML=`<h3>${name}</h3>
      <p>Chat placeholder</p>
      <textarea placeholder="Type message..."></textarea>
      <button disabled>Send</button>`;
    content.appendChild(section); return;
  }

  /* ---------- Default ---------- */
  const section=document.createElement("div"); section.className="section";
  section.innerHTML=`<h3>${name}</h3>
    <textarea class="${editable?"edit":"view-only"}">${editable?"You can edit this as a teacher.":"View only for students/parents."}</textarea>`;
  content.appendChild(section);
}

/* ---------- SHOW SCHOOL PHOTO ---------- */
function showSchoolPhoto(){
  const content=document.getElementById("content");
  content.innerHTML=`<img src="https://source.unsplash.com/800x400/?school" alt="School" class="school">
    <div class="section">
      <h3>Welcome</h3>
      <p>Select an option from the menu.</p>
    </div>`;
}

/* ---------- TOGGLE MENU ---------- */
function toggleMenu(){ document.getElementById("menu").classList.toggle("collapsed"); }

/* ---------- LOGOUT ---------- */
function logout(){
  role=""; studentName=""; childName="";
  document.getElementById("dashboard").style.display="none";
  document.getElementById("login").style.display="flex";
}
</script>

</body>
</html>
