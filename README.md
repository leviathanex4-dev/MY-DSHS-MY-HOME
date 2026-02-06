Your Companion To A Wonderful DSHS Schoo-life
<html lang="en">
<head>
<meta charset="UTF-8">
<title>School System</title>
<style>
  /* ---------- NOTES PAPER BACKGROUND ---------- */
body {
  margin: 0;
  font-family: "Allassy Caps", serif;
  background-color: #ffffff;

  /* dotted horizontal note lines */
  background-image:
    repeating-linear-gradient(
      to bottom,
      transparent 0px,
      transparent 26px,
      rgba(0,0,0,0.15) 27px
    );

  background-size: 100% 28px;
  color: #222;
}

.signup-link {
  color: #007bff;
  font-weight: bold;
  cursor: pointer;
}

.signup-link:hover {
  text-decoration: underline;
}

/* ---------- LOGIN ---------- */
  .login {
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
  }

<p style="font-size:13px; margin-top:15px;">
  Don’t have an account yet?
  <span class="signup-link" onclick="showSignup()">Sign up!</span>
</p>

  .box {
    background: rgba(255,255,255,0.95);
    padding: 25px;
    width: 300px;
    border-radius: 8px;
    box-shadow: 0 10px 20px rgba(0,0,0,0.3);
    text-align: center;
  }

  input, button, textarea, select {
    width: 100%;
    padding: 10px;
    margin-top: 10px;
  }

  button {
    background: #007bff;
    color: white;
    border: none;
    cursor: pointer;
  }

<!-- SIGN UP -->
<div class="login" id="signup" style="display:none;">
  <div class="box">
    <h2>Student Sign Up</h2>

    <input type="text" placeholder="Full Name">
    <input type="text" placeholder="Student I.D">
    <input type="text" placeholder="Section">
    <input type="text" placeholder="Track">
    <input type="text" placeholder="Strand">
    <input type="text" placeholder="Grade Level">

    <button onclick="submitSignup()">Create Account</button>

    <p style="font-size:13px; margin-top:10px;">
      Already have an account?
      <span class="signup-link" onclick="showLogin()">Login</span>
    </p>
  </div>
</div>

  /* ---------- DASHBOARD ---------- */
  .dashboard {
    display: none;
    height: 100vh;
  }

  .header {
    background: rgba(0,51,102,0.95);
    color: white;
    padding: 15px;
    text-align: center;
    font-size: 18px;
  }

  .container {
    display: flex;
    height: calc(100vh - 60px);
  }

  /* ---------- MENU ---------- */
  .menu {
    width: 220px;
    background: rgba(255,255,255,0.95);
    padding: 10px;
    box-shadow: 2px 0 10px rgba(0,0,0,0.2);
    display: flex;
    flex-direction: column;
    transition: width 0.3s;
  }

  .menu.collapsed {
    width: 60px;
  }

  .menu button {
    background: #eee;
    color: black;
    margin-top: 6px;
    font-size: 14px;
    white-space: nowrap;
    overflow: hidden;
  }

  .menu.collapsed button {
    font-size: 0;
  }

  .menu.collapsed button::before {
    content: "•";
    font-size: 18px;
  }

  .toggle-btn {
    background: #003366;
    color: white;
    font-size: 18px;
  }

  .logout {
    margin-top: auto;
    background: #d9534f !important;
    color: white !important;
    font-size: 14px !important;
  }

  /* ---------- CONTENT ---------- */
  .content {
    flex: 1;
    padding: 20px;
    display: flex;
    flex-direction: column;
    align-items: center;
    overflow-y: auto;
  }

  .section {
    background: rgba(255,255,255,0.9);
    padding: 15px;
    border-radius: 5px;
    width: 90%;
    margin-top: 15px;
  }

  img.school {
    max-width: 80%;
    border-radius: 10px;
    margin-bottom: 20px;
  }

  table {
    border-collapse: collapse;
    width: 100%;
    margin-bottom: 15px;
  }

  table, th, td {
    border: 1px solid #ccc;
    text-align: center;
    padding: 5px;
  }

  .edit {
    border: 1px solid #ccc;
  }

  .view-only {
    background: #f9f9f9;
    pointer-events: none;
  }

  .subsection {
    background: rgba(245,245,245,0.8);
    padding: 10px;
    margin-top: 10px;
    border-radius: 5px;
    text-align: left;
  }
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
let role = "";
let currentStudent = "Jibdel"; // For student login
let currentParentChild = "Jibdel"; // For parent login

/* ---------- LOGIN ---------- */
function login() {
  const pass = document.getElementById("password").value;
  if (pass === "teacher") role = "teacher";
  else if (pass === "student") role = "student";
  else if (pass === "parent") role = "parent";
  else { document.getElementById("msg").innerText="Invalid password"; return; }

  document.getElementById("login").style.display="none";
  document.getElementById("dashboard").style.display="block";
  loadDashboard();
}

/* ---------- DASHBOARD ---------- */
function loadDashboard() {
  document.getElementById("roleTitle").innerText = role.toUpperCase()+" DASHBOARD";

  const menu = document.getElementById("menu");
  menu.innerHTML="";

  const toggle = document.createElement("button");
  toggle.innerText="☰"; toggle.className="toggle-btn"; toggle.onclick=toggleMenu;
  menu.appendChild(toggle);

  let items=[];
  if(role==="teacher"){
    items=["Attendance","Grades","Student Files","Parents","Subjects","Schedule","Bulletin Board"];
  } else if(role==="student"){
    items=["Attendance","Grades","Schedule","AI Chat","Bulletin Board","Planner"];
  } else if(role==="parent"){
    items=["Child Attendance","Child Grades","Child Schedule","Teacher Chat","Bulletin Board"];
  }

  createMenu(items);

  const logoutBtn=document.createElement("button");
  logoutBtn.innerText="⬅ Back"; logoutBtn.className="logout"; logoutBtn.onclick=logout;
  menu.appendChild(logoutBtn);

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
  img.src="https://source.unsplash.com/800x400/?school";
  img.className="school";
  content.appendChild(img);

  let editable=(role==="teacher");
  let editClass=editable?"edit":"view-only";

  const students=["Jibdel","Viennes","Jurl","Sam","Justine","Ashley","Gerlie","Meriem"];
  const subjects=["Entrepreneurship","Three eyes","🧢 🗿","Math","Pi6","P ey"];

  /* ---------- Attendance ---------- */
  if(name==="Attendance"||name==="Child Attendance"){
    const section=document.createElement("div"); section.className="section";
    const dates=["2026-02-01","2026-02-02","2026-02-03","2026-02-04","2026-02-05","2026-02-06","2026-02-07"];

    let tableHTML="<h3>Attendance Table</h3><table><tr><th>Name</th>";    
    dates.forEach(d=>tableHTML+=`<th>${d}</th>`); tableHTML+="</tr>";    

    students.forEach(s=>{
      if(role==="student" && s!==currentStudent) return;
      if(role==="parent" && s!==currentParentChild) return;

      tableHTML+=`<tr><td>${s}</td>`;
      dates.forEach(()=>tableHTML+=`<td><input type="checkbox" ${editable?"":"disabled"}></td>`);
      tableHTML+="</tr>";
    });
    tableHTML+="</table>";    
    section.innerHTML=tableHTML;    
    content.appendChild(section);    
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
    const gradesData={"1st":{"Entrepreneurship":"A","Three eyes":"B+","🧢 🗿":"C","Math":"A-","Pi6":"B","P ey":"B+"},
                      "2nd":{"Entrepreneurship":"A-","Three eyes":"A","🧢 🗿":"B+","Math":"A","Pi6":"B+","P ey":"A-"}};
    function renderGrades(sem){
      let html=`<table><tr><th>Subject</th><th>Grade</th></tr>`;
      for(let subj in gradesData[sem]){
        html+=`<tr><td>${subj}</td><td><input type="text" value="${gradesData[sem][subj]}" class="${editable?"edit":"view-only"}" ${editable?"":"disabled"}></td></tr>`;
      }
      gradesContent.innerHTML=html;
    }
    renderGrades("1st"); semesterSelect.addEventListener("change",()=>{renderGrades(semesterSelect.value);});
    return;
  }

  /* ---------- Student Files ---------- */
  if(name==="Student Files"){
    const section=document.createElement("div"); section.className="section";
    section.innerHTML=`<h3>${name}</h3><h4>Performance Review</h4>`;
    students.forEach(s=>{
      const div=document.createElement("div"); div.className="subsection";
      div.innerHTML=`<strong>${s}</strong><br><textarea class="${editable?"edit":"view-only"}" ${editable?"":"disabled"}>Performance review for ${s}.</textarea>`;
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
      const div=document.createElement("div");
      div.className="subsection";
      div.innerHTML=`<strong>${day}</strong><br>
        <textarea class="${role==="teacher"?"edit":"view-only"}" ${role==="teacher"?"":"disabled"} placeholder="Write plans here..."></textarea>`;
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
      div.innerHTML=`<strong>${sub}</strong><br><textarea class="${editable?"edit":"view-only"}" ${editable?"":"disabled"}>Content for ${sub}.</textarea>`;
      section.appendChild(div);
    });
    content.appendChild(section); return;
  }

  /* ---------- Chat Sections ---------- */
  if(name.includes("Chat")){
    const section=document.createElement("div"); section.className="section";
    section.innerHTML=`<h3>${name}</h3>
      <p>Chat placeholder (to be implemented later)</p>
      <textarea placeholder="Type message..."></textarea>
      <button disabled>Send</button>`;
    content.appendChild(section); return;
  }

  /* ---------- Default ---------- */
  const section=document.createElement("div"); section.className="section";
  section.innerHTML=`<h3>${name}</h3><textarea class="${editable?"edit":"view-only"}">${editable?"You can edit this as a teacher.":"View only for students/parents."}</textarea>`;
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
  role=""; 
  document.getElementById("dashboard").style.display="none"; 
  document.getElementById("login").style.display="flex"; 
}
  function showSignup() {
  document.getElementById("login").style.display = "none";
  document.getElementById("signup").style.display = "flex";
}

function showLogin() {
  document.getElementById("signup").style.display = "none";
  document.getElementById("login").style.display = "flex";
}

function submitSignup() {
  alert("Signup successful! You can now log in.");
  showLogin();
}
</script>

</body>
</html>
