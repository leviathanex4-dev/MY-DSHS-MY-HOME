<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>DSHS School System</title>
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<style>
  /*------ COLOR SCHEME ------*/
  :root {
    --main-red: #8B0000;
    --main-blue: #003366;
    --main-gray: #6c757d;
    --sub-yellow: #ffc107;
    --sub-orange: #fd7e14;
    --light-gray: #f8f9fa;
    --dark-gray: #495057;
    --success-green: #28a745;
    --danger-red: #dc3545;
    --warning-yellow: #ffc107;
  }
  
  body { 
    margin: 0; 
    min-height: 100vh; 
    background: linear-gradient(135deg, #8B0000 0%, #003366 50%, #6c757d 100%);
    background-attachment: fixed;
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
  }
  
  /* Dark mode support */
  body.dark-mode {
    --main-red: #a00000;
    --main-blue: #004080;
    --light-gray: #2d2d2d;
    --dark-gray: #e0e0e0;
    background: linear-gradient(135deg, #1a0000 0%, #001a33 50%, #1a1a1a 100%);
    color: #e0e0e0;
  }
  
  body.dark-mode .box,
  body.dark-mode .section,
  body.dark-mode .id-card,
  body.dark-mode .parent-account-card,
  body.dark-mode .print-controls,
  body.dark-mode .school-map-container,
  body.dark-mode .attendance-legend,
  body.dark-mode .performance-box,
  body.dark-mode .chatbox,
  body.dark-mode .planner-container,
  body.dark-mode .subject-item,
  body.dark-mode .planner-item {
    background: #2d2d2d;
    color: #e0e0e0;
    border-color: #444;
  }
  
  body.dark-mode input,
  body.dark-mode select,
  body.dark-mode textarea {
    background: #1a1a1a;
    color: #e0e0e0;
    border-color: #444;
  }
  
  body.dark-mode table th {
    background: linear-gradient(135deg, #660000 0%, #002244 100%);
  }
  
  body.dark-mode .schedule-box div {
    background: linear-gradient(135deg, #2d2d2d 0%, #1a1a1a 100%);
    border-color: #444;
  }
  
  .signup-link { color: var(--main-blue); font-weight: bold; cursor: pointer; }
  .signup-link:hover { text-decoration: underline; }
  
  /* Password strength indicator */
  .password-strength {
    height: 5px;
    margin-top: 5px;
    border-radius: 3px;
    transition: all 0.3s;
  }
  
  .password-strength.weak { background: #dc3545; width: 33%; }
  .password-strength.medium { background: #ffc107; width: 66%; }
  .password-strength.strong { background: #28a745; width: 100%; }
  
  .strength-text {
    font-size: 12px;
    margin-top: 3px;
    font-weight: bold;
  }
  
  input, button, textarea, select { 
    width: 100%; 
    padding: 12px; 
    margin-top: 8px; 
    box-sizing: border-box; 
    font-size: 14px;
    border: 2px solid #ddd;
    border-radius: 6px;
    transition: border-color 0.3s;
  }
  
  input:focus, textarea:focus, select:focus {
    border-color: var(--main-blue);
    outline: none;
  }
  
  button { 
    cursor: pointer; 
    border-radius: 6px; 
    font-weight: bold;
    transition: all 0.3s;
  }
  
  button:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 8px rgba(0,0,0,0.2);
  }
  
  button:disabled {
    opacity: 0.6;
    cursor: not-allowed;
    transform: none;
  }
  
  .toggle-btn { 
    background: linear-gradient(135deg, var(--main-red) 0%, var(--main-blue) 100%); 
    color: white; 
    font-size: 18px; 
    border: none; 
    position: fixed; 
    top: 12px; 
    left: 12px; 
    z-index: 1000; 
    padding: 12px 18px; 
    border-radius: 8px;
    cursor: pointer;
    display: flex;
    align-items: center;
    gap: 8px;
    box-shadow: 0 4px 15px rgba(0,0,0,0.3);
  }
  .toggle-btn:hover { 
    background: linear-gradient(135deg, #660000 0%, #002244 100%);
    transform: scale(1.05);
  }
  .toggle-btn .icon { font-size: 22px; }
  
  .toggle-btn.small {
    padding: 8px 12px;
    font-size: 14px;
  }
  
  .toggle-btn.small .text {
    display: none;
  }
  
  /* Logout button in header */
  .logout-btn {
    position: fixed;
    top: 12px;
    right: 80px;
    background: linear-gradient(135deg, #dc3545 0%, #c82333 100%);
    color: white;
    font-size: 14px;
    border: none;
    padding: 12px 20px;
    border-radius: 8px;
    cursor: pointer;
    z-index: 999;
    display: flex;
    align-items: center;
    gap: 8px;
    box-shadow: 0 4px 15px rgba(0,0,0,0.3);
    transition: all 0.3s;
  }
  
  .logout-btn:hover {
    background: linear-gradient(135deg, #c82333 0%, #a71e2a 100%);
    transform: scale(1.05);
  }
  
  /* Session timeout warning */
  .session-warning {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    background: #dc3545;
    color: white;
    text-align: center;
    padding: 10px;
    z-index: 10000;
    display: none;
    animation: pulse 2s infinite;
  }
  
  @keyframes pulse {
    0%, 100% { opacity: 1; }
    50% { opacity: 0.8; }
  }
  
  .login { 
    height: 100vh; 
    display: flex; 
    justify-content: center; 
    align-items: center; 
    padding: 20px;
  }
  
  .box { 
    background: rgba(255,255,255,0.98); 
    padding: 40px; 
    width: 100%; 
    max-width: 420px; 
    border-radius: 15px; 
    box-shadow: 0 15px 40px rgba(0,0,0,0.3); 
    text-align: center; 
    border-top: 5px solid var(--main-red);
  }
  
  .box h2 {
    color: var(--main-blue);
    margin-bottom: 25px;
  }
  
  .pass-wrapper { position: relative; }
  .pass-wrapper span { 
    position: absolute; 
    right: 15px; 
    top: 18px; 
    cursor: pointer; 
    font-size: 18px;
  }
  
  .dashboard { display: none; min-height: 100vh; }
  
  .header { 
    background: linear-gradient(135deg, var(--main-red) 0%, var(--main-blue) 100%); 
    color: white; 
    padding: 18px; 
    text-align: center; 
    font-size: 18px; 
    position: relative; 
    z-index: 1;
    padding-left: 70px;
    box-shadow: 0 4px 15px rgba(0,0,0,0.2);
  }
  
  .container { 
    display: flex; 
    min-height: calc(100vh - 60px); 
    position: relative; 
  }
  
  .menu-overlay {
    display: none;
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,0.6);
    z-index: 998;
    backdrop-filter: blur(3px);
  }
  .menu-overlay.show { display: block; }
  
  .menu {
    width: 300px;
    background: linear-gradient(180deg, var(--main-red) 0%, #5a0000 50%, var(--main-blue) 100%);
    padding: 15px;
    display: flex;
    flex-direction: column;
    position: fixed;
    top: 0;
    left: 0;
    height: 100%;
    z-index: 999;
    transition: transform 0.4s cubic-bezier(0.68, -0.55, 0.265, 1.55);
    transform: translateX(-100%);
    overflow-y: auto;
    padding-top: 80px;
    box-shadow: 5px 0 25px rgba(0,0,0,0.4);
  }
  
  .menu.show { transform: translateX(0); }
  
  .menu-header {
    color: white;
    text-align: center;
    padding: 15px;
    border-bottom: 2px solid rgba(255,255,255,0.3);
    margin-bottom: 20px;
    background: rgba(255,255,255,0.1);
    border-radius: 10px;
  }
  
  .menu-header h3 {
    margin: 0;
    font-size: 22px;
    color: var(--sub-yellow);
  }
  
  .menu-header p {
    margin: 5px 0 0 0;
    font-size: 13px;
    opacity: 0.9;
  }
  
  .menu button {
    background: rgba(255,255,255,0.15);
    color: white;
    margin-top: 10px;
    font-size: 15px;
    border: 1px solid rgba(255,255,255,0.25);
    border-radius: 8px;
    padding: 14px 15px;
    transition: all 0.3s;
    text-align: left;
    display: flex;
    align-items: center;
    gap: 12px;
  }
  
  .menu button:hover { 
    background: rgba(255,255,255,0.3); 
    transform: translateX(8px);
    padding-left: 20px;
  }
  
  .menu button:active {
    background: rgba(255,255,255,0.4);
    transform: scale(0.98);
  }
  
  .menu button .tab-icon {
    font-size: 20px;
    width: 30px;
    text-align: center;
  }
  
  .logout { 
    margin-top: auto; 
    background: var(--sub-orange) !important; 
    color: white !important; 
    text-align: center;
    justify-content: center;
  }
  .logout:hover { background: #e6730f !important; }
  
  .content { 
    flex: 1; 
    padding: 25px; 
    display: flex; 
    flex-direction: column; 
    align-items: center; 
    overflow-y: auto; 
    background: rgba(248,249,250,0.9);
  }
  
  .section { 
    background: white; 
    padding: 25px; 
    border-radius: 12px; 
    width: 100%; 
    max-width: 850px; 
    margin-top: 20px; 
    box-shadow: 0 4px 20px rgba(0,0,0,0.1);
    border-left: 5px solid var(--main-blue);
  }
  
  .section h3 {
    color: var(--main-blue);
    border-bottom: 2px solid var(--sub-yellow);
    padding-bottom: 10px;
    margin-top: 0;
  }
  
  /* Report Card Styles - DepEd Format */
  .report-card {
    background: white;
    border: 3px solid var(--main-blue);
    padding: 30px;
    max-width: 800px;
    margin: 0 auto;
    font-family: 'Times New Roman', Times, serif;
  }
  
  .report-card-header {
    text-align: center;
    border-bottom: 3px double var(--main-blue);
    padding-bottom: 20px;
    margin-bottom: 20px;
  }
  
  .report-card-header h2 {
    margin: 0;
    font-size: 24px;
    text-transform: uppercase;
    letter-spacing: 2px;
  }
  
  .report-card-header h3 {
    margin: 10px 0;
    font-size: 18px;
    color: var(--main-blue);
    border: none;
    padding: 0;
  }
  
  .report-card-info {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 15px;
    margin-bottom: 20px;
    font-size: 14px;
  }
  
  .report-card-info p {
    margin: 5px 0;
  }
  
  .report-card table {
    width: 100%;
    border-collapse: collapse;
    margin: 20px 0;
    font-size: 13px;
  }
  
  .report-card th, .report-card td {
    border: 1px solid #333;
    padding: 8px;
    text-align: center;
  }
  
  .report-card th {
    background: #f0f0f0;
    font-weight: bold;
    color: #000;
  }
  
  .report-card .descriptor-row {
    font-size: 11px;
    background: #f9f9f9;
  }
  
  .report-card-footer {
    margin-top: 30px;
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 30px;
  }
  
  .signature-line {
    border-top: 1px solid #333;
    margin-top: 40px;
    padding-top: 5px;
    text-align: center;
    font-size: 12px;
  }
  
  /* Transmutation Table */
  .transmutation-table {
    background: #f8f9fa;
    padding: 20px;
    border-radius: 10px;
    margin: 15px 0;
  }
  
  .transmutation-table h4 {
    margin-top: 0;
    color: var(--main-blue);
  }
  
  /* Honor Roll */
  .honor-roll {
    background: linear-gradient(135deg, #ffd700 0%, #ffed4e 100%);
    border: 3px solid #b8860b;
    padding: 20px;
    border-radius: 15px;
    margin: 15px 0;
  }
  
  .honor-roll h4 {
    color: #8b6914;
    margin-top: 0;
    text-align: center;
    font-size: 20px;
  }
  
  .honor-roll-list {
    list-style: none;
    padding: 0;
  }
  
  .honor-roll-list li {
    padding: 10px;
    border-bottom: 1px solid rgba(0,0,0,0.1);
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  
  .honor-badge {
    background: #b8860b;
    color: white;
    padding: 5px 15px;
    border-radius: 20px;
    font-size: 12px;
    font-weight: bold;
  }
  
  /* Messaging System */
  .messaging-container {
    display: grid;
    grid-template-columns: 250px 1fr;
    gap: 20px;
    height: 500px;
  }
  
  .contacts-list {
    background: #f8f9fa;
    border-radius: 10px;
    overflow-y: auto;
    padding: 10px;
  }
  
  .contact-item {
    padding: 12px;
    border-radius: 8px;
    cursor: pointer;
    transition: all 0.3s;
    margin-bottom: 8px;
    display: flex;
    align-items: center;
    gap: 10px;
  }
  
  .contact-item:hover, .contact-item.active {
    background: var(--main-blue);
    color: white;
  }
  
  .contact-avatar {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    background: #ddd;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 20px;
  }
  
  .chat-area {
    display: flex;
    flex-direction: column;
    background: white;
    border-radius: 10px;
    border: 2px solid #dee2e6;
  }
  
  .chat-header {
    padding: 15px;
    border-bottom: 2px solid #dee2e6;
    background: linear-gradient(135deg, var(--main-red) 0%, var(--main-blue) 100%);
    color: white;
    border-radius: 8px 8px 0 0;
  }
  
  .chat-messages {
    flex: 1;
    overflow-y: auto;
    padding: 15px;
    background: #f8f9fa;
  }
  
  .message {
    max-width: 70%;
    padding: 12px;
    border-radius: 15px;
    margin-bottom: 10px;
    position: relative;
    animation: fadeIn 0.3s ease;
  }
  
  .message.sent {
    background: linear-gradient(135deg, var(--main-blue) 0%, #0056b3 100%);
    color: white;
    margin-left: auto;
    border-bottom-right-radius: 5px;
  }
  
  .message.received {
    background: white;
    border: 1px solid #dee2e6;
    margin-right: auto;
    border-bottom-left-radius: 5px;
  }
  
  .message-time {
    font-size: 11px;
    opacity: 0.7;
    margin-top: 5px;
  }
  
  .chat-input-area {
    padding: 15px;
    border-top: 2px solid #dee2e6;
    display: flex;
    gap: 10px;
  }
  
  .chat-input-area input {
    flex: 1;
    margin: 0;
  }
  
  /* Conference Scheduling */
  .conference-slot {
    padding: 15px;
    border: 2px solid #dee2e6;
    border-radius: 10px;
    margin: 10px 0;
    display: flex;
    justify-content: space-between;
    align-items: center;
    transition: all 0.3s;
  }
  
  .conference-slot:hover {
    border-color: var(--main-blue);
    transform: translateX(5px);
  }
  
  .conference-slot.booked {
    background: #f8d7da;
    border-color: #dc3545;
  }
  
  .conference-slot.available {
    background: #d4edda;
    border-color: #28a745;
  }
  
  /* Payment Styles - REMOVED */
  
  /* Scholarship Card - REMOVED */
  
  /* Notification Center */
  .notification-bell {
    position: fixed;
    top: 20px;
    right: 20px;
    background: var(--main-red);
    color: white;
    width: 50px;
    height: 50px;
    border-radius: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 24px;
    cursor: pointer;
    z-index: 999;
    box-shadow: 0 4px 15px rgba(0,0,0,0.3);
    transition: all 0.3s;
  }
  
  .notification-bell:hover {
    transform: scale(1.1);
  }
  
  .notification-badge {
    position: absolute;
    top: -5px;
    right: -5px;
    background: var(--sub-orange);
    color: white;
    width: 25px;
    height: 25px;
    border-radius: 50%;
    font-size: 12px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-weight: bold;
  }
  
  .notification-panel {
    position: fixed;
    top: 80px;
    right: 20px;
    width: 350px;
    max-height: 500px;
    background: white;
    border-radius: 12px;
    box-shadow: 0 10px 40px rgba(0,0,0,0.3);
    z-index: 998;
    display: none;
    overflow: hidden;
  }
  
  .notification-panel.show {
    display: block;
  }
  
  .notification-header {
    background: linear-gradient(135deg, var(--main-red) 0%, var(--main-blue) 100%);
    color: white;
    padding: 15px;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }
  
  .notification-list {
    max-height: 400px;
    overflow-y: auto;
  }
  
  .notification-item {
    padding: 15px;
    border-bottom: 1px solid #eee;
    cursor: pointer;
    transition: all 0.3s;
  }
  
  .notification-item:hover {
    background: #f8f9fa;
  }
  
  .notification-item.unread {
    border-left: 4px solid var(--main-blue);
    background: #f0f8ff;
  }
  
  /* Calendar Styles */
  .calendar-grid {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    gap: 5px;
    margin-top: 20px;
  }
  
  .calendar-day {
    aspect-ratio: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    border: 1px solid #dee2e6;
    border-radius: 8px;
    cursor: pointer;
    transition: all 0.3s;
    position: relative;
  }
  
  .calendar-day:hover {
    background: #f0f8ff;
    transform: scale(1.05);
  }
  
  .calendar-day.has-event::after {
    content: '';
    position: absolute;
    bottom: 5px;
    width: 6px;
    height: 6px;
    background: var(--main-red);
    border-radius: 50%;
  }
  
  .calendar-day.today {
    background: var(--main-blue);
    color: white;
    font-weight: bold;
  }
  
  /* Analytics Dashboard */
  .analytics-card {
    background: white;
    padding: 20px;
    border-radius: 12px;
    box-shadow: 0 4px 15px rgba(0,0,0,0.1);
    text-align: center;
  }
  
  .analytics-number {
    font-size: 36px;
    font-weight: bold;
    color: var(--main-blue);
    margin: 10px 0;
  }
  
  .analytics-label {
    color: var(--main-gray);
    font-size: 14px;
  }
  
  .chart-container {
    background: white;
    padding: 20px;
    border-radius: 12px;
    margin: 20px 0;
    box-shadow: 0 4px 15px rgba(0,0,0,0.1);
  }
  
  /* Library Styles - REMOVED */
  
  /* Room Booking - REMOVED */
  
  /* Enrollment Form */
  .enrollment-step {
    display: none;
  }
  
  .enrollment-step.active {
    display: block;
  }
  
  .step-indicator {
    display: flex;
    justify-content: space-between;
    margin-bottom: 30px;
  }
  
  .step {
    flex: 1;
    text-align: center;
    padding: 10px;
    border-bottom: 3px solid #dee2e6;
    color: var(--main-gray);
  }
  
  .step.active {
    border-color: var(--main-blue);
    color: var(--main-blue);
    font-weight: bold;
  }
  
  .step.completed {
    border-color: var(--success-green);
    color: var(--success-green);
  }
  
  /* Behavioral Report */
  .behavior-item {
    display: flex;
    align-items: center;
    padding: 15px;
    border-left: 4px solid;
    margin: 10px 0;
    background: #f8f9fa;
    border-radius: 0 8px 8px 0;
  }
  
  .behavior-item.merit {
    border-color: var(--success-green);
  }
  
  .behavior-item.demerit {
    border-color: var(--danger-red);
  }
  
  .behavior-points {
    font-size: 24px;
    font-weight: bold;
    margin-right: 20px;
  }
  
  .behavior-item.merit .behavior-points {
    color: var(--success-green);
  }
  
  .behavior-item.demerit .behavior-points {
    color: var(--danger-red);
  }
  
  /* Assignment Submission */
  .assignment-card {
    padding: 20px;
    border: 2px solid #dee2e6;
    border-radius: 12px;
    margin: 15px 0;
    transition: all 0.3s;
  }
  
  .assignment-card:hover {
    border-color: var(--main-blue);
  }
  
  .assignment-status {
    display: inline-block;
    padding: 5px 15px;
    border-radius: 20px;
    font-size: 12px;
    font-weight: bold;
    margin-top: 10px;
  }
  
  .assignment-status.pending {
    background: #fff3cd;
    color: #856404;
  }
  
  .assignment-status.submitted {
    background: #d4edda;
    color: #155724;
  }
  
  .assignment-status.graded {
    background: #cce5ff;
    color: #004085;
  }
  
  .assignment-status.late {
    background: #f8d7da;
    color: #721c24;
  }
  
  /* File Upload Zone */
  .upload-zone {
    border: 3px dashed #dee2e6;
    border-radius: 12px;
    padding: 40px;
    text-align: center;
    transition: all 0.3s;
    cursor: pointer;
  }
  
  .upload-zone:hover, .upload-zone.dragover {
    border-color: var(--main-blue);
    background: #f0f8ff;
  }
  
  .upload-zone i {
    font-size: 48px;
    color: var(--main-gray);
    margin-bottom: 15px;
  }
  
  /* Quiz/Exam Styles */
  .quiz-timer {
    position: fixed;
    top: 80px;
    right: 20px;
    background: var(--danger-red);
    color: white;
    padding: 15px 25px;
    border-radius: 10px;
    font-size: 24px;
    font-weight: bold;
    z-index: 100;
    box-shadow: 0 4px 15px rgba(0,0,0,0.3);
  }
  
  .quiz-question {
    background: white;
    padding: 25px;
    border-radius: 12px;
    margin: 20px 0;
    box-shadow: 0 4px 15px rgba(0,0,0,0.1);
  }
  
  .quiz-options {
    display: grid;
    gap: 10px;
    margin-top: 20px;
  }
  
  .quiz-option {
    padding: 15px;
    border: 2px solid #dee2e6;
    border-radius: 8px;
    cursor: pointer;
    transition: all 0.3s;
  }
  
  .quiz-option:hover {
    border-color: var(--main-blue);
    background: #f0f8ff;
  }
  
  .quiz-option.selected {
    border-color: var(--main-blue);
    background: var(--main-blue);
    color: white;
  }
  
  /* Transcript Styles - REMOVED FROM STUDENT */
  
  /* Alumni Styles - REMOVED */
  
  /* Settings Panel */
  .settings-section {
    padding: 20px;
    border-bottom: 1px solid #dee2e6;
  }
  
  .settings-section:last-child {
    border-bottom: none;
  }
  
  .toggle-switch {
    position: relative;
    display: inline-block;
    width: 60px;
    height: 34px;
  }
  
  .toggle-switch input {
    opacity: 0;
    width: 0;
    height: 0;
  }
  
  .slider {
    position: absolute;
    cursor: pointer;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: #ccc;
    transition: .4s;
    border-radius: 34px;
  }
  
  .slider:before {
    position: absolute;
    content: "";
    height: 26px;
    width: 26px;
    left: 4px;
    bottom: 4px;
    background-color: white;
    transition: .4s;
    border-radius: 50%;
  }
  
  input:checked + .slider {
    background-color: var(--main-blue);
  }
  
  input:checked + .slider:before {
    transform: translateX(26px);
  }
  
  /* PWA Install Button */
  .install-pwa-btn {
    position: fixed;
    bottom: 20px;
    right: 20px;
    background: var(--success-green);
    color: white;
    padding: 15px 25px;
    border-radius: 50px;
    box-shadow: 0 4px 15px rgba(0,0,0,0.3);
    z-index: 999;
    display: none;
  }
  
  .install-pwa-btn.show {
    display: block;
  }
  
  /* Loading Spinner */
  .spinner {
    border: 4px solid #f3f3f3;
    border-top: 4px solid var(--main-blue);
    border-radius: 50%;
    width: 40px;
    height: 40px;
    animation: spin 1s linear infinite;
    margin: 20px auto;
  }
  
  @keyframes spin {
    0% { transform: rotate(0deg); }
    100% { transform: rotate(360deg); }
  }
  
  /* Accessibility: Focus visible */
  *:focus-visible {
    outline: 3px solid var(--sub-yellow);
    outline-offset: 2px;
  }
  
  /* Screen reader only */
  .sr-only {
    position: absolute;
    width: 1px;
    height: 1px;
    padding: 0;
    margin: -1px;
    overflow: hidden;
    clip: rect(0,0,0,0);
    white-space: nowrap;
    border: 0;
  }
  
  table { 
    border-collapse: collapse; 
    width: 100%; 
    margin-top: 20px; 
    overflow-x: auto;
  }
  table, th, td { 
    border: 1px solid #dee2e6; 
    text-align: center; 
    padding: 12px; 
  }
  th { 
    background: linear-gradient(135deg, var(--main-red) 0%, var(--main-blue) 100%); 
    color: white; 
    font-weight: bold;
  }
  
  tr:nth-child(even) {
    background-color: #f8f9fa;
  }
  
  tr:hover {
    background-color: #e9ecef;
  }
  
  .schedule-box { 
    display: flex; 
    flex-wrap: wrap; 
    gap: 15px; 
    margin-top: 20px; 
  }
  
  .schedule-box div { 
    flex: 1 1 180px; 
    background: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%); 
    border: 2px solid #dee2e6; 
    padding: 18px; 
    border-radius: 10px; 
    text-align: center; 
    transition: all 0.3s;
  }
  
  .schedule-box div:hover {
    border-color: var(--main-blue);
    transform: translateY(-3px);
    box-shadow: 0 6px 15px rgba(0,0,0,0.15);
  }
  
  .schedule-box div strong {
    color: var(--main-blue);
    display: block;
    margin-bottom: 8px;
  }
  
  .chatbox { 
    border: 2px solid #dee2e6; 
    padding: 15px; 
    height: 320px; 
    width: 100%; 
    overflow-y: auto; 
    margin-top: 12px; 
    border-radius: 8px; 
    background: #fafafa;
  }
  
  .chat-message { 
    margin-bottom: 12px; 
    padding: 12px; 
    border-radius: 8px;
    animation: fadeIn 0.3s ease;
  }
  
  @keyframes fadeIn {
    from { opacity: 0; transform: translateY(10px); }
    to { opacity: 1; transform: translateY(0); }
  }
  
  .chat-user { 
    background: linear-gradient(135deg, #e3f2fd 0%, #bbdefb 100%); 
    text-align: left; 
    margin-left: 20%;
  }
  .chat-ai { 
    background: linear-gradient(135deg, #f3e5f5 0%, #e1bee7 100%); 
    text-align: left; 
    margin-right: 20%;
  }
  
  .id-card { 
    background: linear-gradient(135deg, var(--main-red) 0%, var(--main-blue) 100%); 
    border: 4px solid var(--sub-yellow); 
    border-radius: 18px; 
    padding: 22px; 
    width: 100%; 
    max-width: 360px; 
    margin: 18px; 
    display: inline-block; 
    vertical-align: top; 
    color: white;
    position: relative;
    box-shadow: 0 10px 30px rgba(0,0,0,0.3);
  }
  
  .id-card-header { 
    background: rgba(255,255,255,0.12); 
    padding: 18px; 
    text-align: center; 
    border-radius: 12px 12px 0 0; 
    margin: -22px -22px 18px -22px; 
    border-bottom: 3px solid var(--sub-yellow);
  }
  
  .id-card-header h3 { 
    margin: 0; 
    font-size: 17px; 
    color: var(--sub-yellow); 
    letter-spacing: 1px;
  }
  
  .id-card-header p { 
    margin: 6px 0 0 0; 
    font-size: 11px; 
    color: #ddd; 
  }
  
  .id-card-photo { 
    width: 110px; 
    height: 110px; 
    background: #eee; 
    border-radius: 50%; 
    margin: 12px auto; 
    display: flex; 
    align-items: center; 
    justify-content: center; 
    overflow: hidden; 
    border: 5px solid var(--sub-yellow);
    box-shadow: 0 4px 15px rgba(0,0,0,0.3);
  }
  
  .id-card-photo img { 
    width: 100%; 
    height: 100%; 
    object-fit: cover; 
  }
  
  .id-card-info { 
    text-align: left; 
    font-size: 14px; 
    background: rgba(0,0,0,0.25); 
    padding: 18px; 
    border-radius: 12px;
  }
  
  .id-card-info p { 
    margin: 10px 0; 
    display: flex;
    justify-content: space-between;
  }
  
  .id-card-info strong { 
    color: var(--sub-yellow); 
  }
  
  .id-card-footer {
    text-align: center;
    margin-top: 18px;
    padding-top: 12px;
    border-top: 1px solid rgba(255,255,255,0.3);
    font-size: 10px;
    color: #ccc;
  }
  
  .id-card-checkbox {
    position: absolute;
    top: 12px;
    right: 12px;
    width: 28px;
    height: 28px;
    cursor: pointer;
    accent-color: var(--sub-yellow);
  }
  
  .attendance-calendar { 
    display: grid; 
    grid-template-columns: repeat(7, 1fr); 
    gap: 6px; 
    margin-top: 18px; 
    overflow-x: auto;
  }
  
  .attendance-day { 
    padding: 14px 10px; 
    text-align: center; 
    border: 2px solid #dee2e6; 
    border-radius: 8px; 
    min-width: 45px;
    cursor: pointer;
    transition: all 0.2s;
    font-weight: bold;
  }
  
  .attendance-day:hover {
    transform: scale(1.1);
    border-color: var(--main-blue);
  }
  
  .attendance-day.present { 
    background: linear-gradient(135deg, #d4edda 0%, #c3e6cb 100%); 
    border-color: #28a745;
  }
  
  .attendance-day.absent { 
    background: linear-gradient(135deg, #f8d7da 0%, #f5c6cb 100%); 
    border-color: #dc3545;
  }
  
  .attendance-day.header { 
    background: linear-gradient(135deg, var(--main-red) 0%, var(--main-blue) 100%); 
    color: white; 
    font-weight: bold; 
    padding: 12px;
    cursor: default;
  }
  
  .attendance-day.header:hover {
    transform: none;
  }
  
  .subject-item { 
    display: flex;
    align-items: center; 
    padding: 12px; 
    border: 2px solid #dee2e6; 
    margin: 10px 0; 
    border-radius: 8px; 
    flex-wrap: wrap;
    gap: 12px;
    background: #fafafa;
  }
  
  .subject-item:hover {
    border-color: var(--main-blue);
    background: white;
  }
  
  .subject-item input[type="text"] { 
    flex: 1; 
    min-width: 140px; 
    margin: 0; 
  }
  
  .planner-container { 
    max-height: 450px; 
    overflow-y: auto; 
    padding: 18px; 
    border: 2px solid #dee2e6; 
    border-radius: 10px; 
    background: #f8f9fa; 
  }
  
  .planner-section { margin-bottom: 25px; }
  
  .planner-section h4 { 
    background: linear-gradient(135deg, var(--main-red) 0%, var(--main-blue) 100%); 
    color: white; 
    padding: 12px; 
    margin: 0 0 12px 0; 
    border-radius: 8px 8px 0 0; 
    display: flex;
    align-items: center;
    gap: 10px;
  }
  
  .planner-item { 
    display: flex; 
    align-items: center; 
    padding: 12px; 
    border: 2px solid #dee2e6; 
    margin: 8px 0; 
    border-radius: 8px; 
    background: white; 
    flex-wrap: wrap;
    gap: 12px;
    transition: all 0.2s;
  }
  
  .planner-item:hover {
    border-color: var(--main-blue);
  }
  
  .planner-item input[type="checkbox"] { 
    width: auto; 
    margin: 0; 
    width: 22px;
    height: 22px;
    cursor: pointer;
    accent-color: var(--main-blue);
  }
  
  .planner-item.completed { 
    text-decoration: line-through; 
    opacity: 0.6; 
    background: #f0f0f0;
  }
  
  .planner-item .goal-text { 
    flex: 1; 
    min-width: 180px; 
  }
  
  .planner-item .goal-time { 
    font-size: 13px; 
    color: var(--main-gray);
    background: #e9ecef;
    padding: 4px 10px;
    border-radius: 15px;
  }
  
  .planner-item .alarm-btn { 
    width: auto; 
    padding: 8px 14px; 
    font-size: 13px; 
    background: linear-gradient(135deg, var(--main-red) 0%, var(--main-blue) 100%); 
    color: white; 
    border-radius: 20px;
  }
  
  .attendance-legend { 
    position: fixed; 
    bottom: 25px; 
    right: 25px; 
    background: white; 
    padding: 18px; 
    border-radius: 10px; 
    box-shadow: 0 4px 20px rgba(0,0,0,0.25); 
    z-index: 100;
    border: 3px solid var(--main-blue);
    min-width: 180px;
  }
  
  .attendance-legend h4 {
    margin: 0 0 12px 0;
    color: var(--main-blue);
  }
  
  .performance-box {
    background: linear-gradient(135deg, var(--main-red) 0%, var(--main-blue) 100%); 
    color: white;
    padding: 25px;
    border-radius: 12px;
    margin: 20px 0;
    text-align: center;
    box-shadow: 0 6px 20px rgba(0,0,0,0.2);
  }
  
  .performance-box h3 {
    color: var(--sub-yellow);
    margin-top: 0;
  }
  
  .performance-stats { 
    display: flex; 
    justify-content: space-around; 
    margin-top: 20px; 
    flex-wrap: wrap;
    gap: 15px;
  }
  
  .stat-item { 
    text-align: center; 
    background: rgba(255,255,255,0.15);
    padding: 15px 25px;
    border-radius: 10px;
    min-width: 100px;
  }
  
  .stat-number { 
    font-size: 32px; 
    font-weight: bold; 
    color: var(--sub-yellow);
  }
  
  .stat-label { 
    font-size: 13px; 
    opacity: 0.9; 
    margin-top: 5px;
  }
  
  .mood-selector {
    display: flex;
    justify-content: center;
    gap: 18px;
    margin: 25px 0;
    flex-wrap: wrap;
  }
  
  .mood-btn {
    width: 70px;
    height: 70px;
    border-radius: 50%;
    border: 4px solid transparent;
    font-size: 35px;
    cursor: pointer;
    transition: all 0.3s;
    background: white;
    box-shadow: 0 4px 15px rgba(0,0,0,0.1);
  }
  
  .mood-btn:hover { 
    transform: scale(1.15); 
    border-color: var(--main-blue);
    box-shadow: 0 8px 25px rgba(0,0,0,0.2);
  }
  
  .mood-btn.selected {
    transform: scale(1.2);
    border-color: var(--sub-orange);
    box-shadow: 0 0 25px rgba(253, 126, 20, 0.5);
  }
  
  .parent-account-card {
    background: linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%);
    border: 3px solid var(--main-blue);
    border-radius: 12px;
    padding: 20px;
    margin: 15px 0;
  }
  
  .parent-account-card h4 {
    color: var(--main-blue);
    margin-top: 0;
    display: flex;
    align-items: center;
    gap: 10px;
  }
  
  .parent-account-card .credentials {
    background: white;
    padding: 15px;
    border-radius: 8px;
    margin-top: 12px;
    border: 2px dashed var(--main-blue);
  }
  
  .parent-account-card .credentials p {
    margin: 8px 0;
    font-family: 'Courier New', monospace;
    font-weight: bold;
    color: var(--main-blue);
  }
  
  .print-controls {
    background: linear-gradient(135deg, #fff3cd 0%, #ffeeba 100%);
    padding: 18px;
    border-radius: 10px;
    margin-bottom: 20px;
    border: 3px solid var(--sub-yellow);
  }
  
  .print-controls h4 {
    color: var(--main-gray);
    margin-top: 0;
    display: flex;
    align-items: center;
    gap: 10px;
  }
  
  .school-map-container {
    background: #f8f9fa;
    border-radius: 12px;
    padding: 20px;
    margin-top: 15px;
  }
  
  .school-map-container h4 {
    color: var(--main-blue);
    text-align: center;
    margin-top: 0;
  }
  
  .school-map-container img {
    max-width: 100%;
    border-radius: 10px;
    box-shadow: 0 4px 15px rgba(0,0,0,0.2);
  }
  
  .map-legend {
    margin-top: 15px;
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
    gap: 12px;
  }
  
  .map-legend-item {
    background: white;
    padding: 12px;
    border-radius: 8px;
    border-left: 4px solid var(--main-red);
    box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  }
  
  .map-legend-item strong {
    color: var(--main-blue);
    display: block;
    margin-bottom: 5px;
  }
  
  .qr-scanner-container {
    background: #000;
    border-radius: 10px;
    overflow: hidden;
    margin: 15px 0;
    max-width: 400px;
  }
  
  #qr-scanner video {
    width: 100%;
    display: block;
  }
  
  .scanner-result {
    padding: 12px;
    border-radius: 8px;
    margin-top: 12px;
    text-align: center;
    font-weight: bold;
  }
  
  .scanner-result.success {
    background: #d4edda;
    color: #155724;
    border: 2px solid #c3e6cb;
  }
  
  .scanner-result.error {
    background: #f8d7da;
    color: #721c24;
    border: 2px solid #f5c6cb;
  }
  
  .sem-selector {
    display: flex;
    gap: 12px;
    margin: 15px 0;
    flex-wrap: wrap;
    justify-content: center;
  }
  
  .sem-btn {
    padding: 12px 24px;
    border: 3px solid var(--main-blue);
    background: white;
    color: var(--main-blue);
    border-radius: 25px;
    font-weight: bold;
    cursor: pointer;
    transition: all 0.3s;
    min-width: 140px;
  }
  
  .sem-btn:hover {
    background: var(--main-blue);
    color: white;
    transform: translateY(-2px);
  }
  
  .sem-btn.active {
    background: linear-gradient(135deg, var(--main-red) 0%, var(--main-blue) 100%);
    color: white;
    border-color: var(--sub-yellow);
  }
  
  .quarter-selector {
    display: flex;
    gap: 10px;
    margin: 15px 0;
    flex-wrap: wrap;
    justify-content: center;
  }
  
  .quarter-btn {
    padding: 10px 20px;
    border: 2px solid var(--main-gray);
    background: white;
    color: var(--main-gray);
    border-radius: 20px;
    font-weight: bold;
    cursor: pointer;
    transition: all 0.3s;
  }
  
  .quarter-btn:hover {
    border-color: var(--main-blue);
    color: var(--main-blue);
  }
  
  .quarter-btn.active {
    background: var(--main-blue);
    color: white;
    border-color: var(--main-blue);
  }
  
  .school-welcome {
    background: linear-gradient(135deg, var(--main-red) 0%, var(--main-blue) 100%);
    color: white;
    padding: 30px;
    border-radius: 15px;
    text-align: center;
    margin-bottom: 25px;
    box-shadow: 0 8px 25px rgba(0,0,0,0.2);
  }
  
  .school-welcome h2 {
    margin: 0 0 10px 0;
    color: var(--sub-yellow);
  }
  
  .school-welcome p {
    margin: 0;
    opacity: 0.95;
  }
  
  /* Logo styling - FIXED NOT TO SHRINK */
  .login-wrapper {
    text-align: center;
    padding-top: 30px;
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    z-index: 1001;
    background: transparent;
  }
  
  .login-wrapper img {
    width: 120px;
    height: 120px;
    min-width: 120px;
    min-height: 120px;
    max-width: 120px;
    max-height: 120px;
    border-radius: 50%;
    box-shadow: 0 8px 25px rgba(0,0,0,0.3);
    border: 4px solid white;
    background: white;
    object-fit: cover;
  }
  
  /* Bulletin Board Image Upload */
  .bulletin-image {
    max-width: 100%;
    border-radius: 8px;
    margin-top: 10px;
    box-shadow: 0 2px 8px rgba(0,0,0,0.1);
  }
  
  .bulletin-upload-preview {
    max-width: 200px;
    max-height: 200px;
    border-radius: 8px;
    margin-top: 10px;
    border: 2px solid #ddd;
  }
  
  @media (max-width: 768px) {
    .menu { 
      width: 85%; 
      max-width: 340px; 
    }
    
    .section { 
      padding: 18px; 
      margin-top: 12px; 
    }
    
    .attendance-legend { 
      position: relative; 
      bottom: auto; 
      right: auto; 
      margin-top: 18px; 
      width: 100%; 
    }
    
    .schedule-box div { flex: 1 1 100%; }
    
    table { 
      display: block; 
      overflow-x: auto; 
      white-space: nowrap; 
    }
    
    .id-card { 
      max-width: 100%; 
      margin: 12px 0; 
    }
    
    .mood-selector {
      gap: 12px;
    }
    
    .mood-btn {
      width: 60px;
      height: 60px;
      font-size: 28px;
    }
    
    .header {
      padding-left: 55px;
      font-size: 15px;
    }
    
    .toggle-btn {
      padding: 10px 14px;
      font-size: 14px;
    }
    
    .logout-btn {
      right: 70px;
      padding: 10px 15px;
      font-size: 12px;
    }
    
    .box {
      padding: 25px;
    }
    
    .stat-number {
      font-size: 26px;
    }
    
    .login-wrapper img {
      width: 100px;
      height: 100px;
      min-width: 100px;
      min-height: 100px;
      max-width: 100px;
      max-height: 100px;
    }
    
    .messaging-container {
      grid-template-columns: 1fr;
    }
    
    .contacts-list {
      max-height: 200px;
    }
    
    .notification-panel {
      width: 90%;
      right: 5%;
    }
  }
  
  @media print {
    .menu, .toggle-btn, .header, .print-controls, .attendance-legend, .menu-overlay,
    .notification-bell, .session-warning, .quiz-timer, .install-pwa-btn, .logout-btn {
      display: none !important;
    }
    
    .id-card { 
      break-inside: avoid; 
      page-break-inside: avoid; 
      border: 3px solid #000; 
      display: inline-block;
      margin: 10px;
    }
    
    .content {
      padding: 0;
    }
    
    .section {
      box-shadow: none;
      padding: 10px;
      border: none;
    }
    
    .report-card {
      border: 2px solid #000;
    }
  }
</style>
<script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
<script src="https://unpkg.com/html5-qrcode/html5-qrcode.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
</head>
<body>
  
  <!-- Session Timeout Warning -->
  <div class="session-warning" id="sessionWarning">
    ⚠️ Your session will expire in <span id="countdown">60</span> seconds. Click anywhere to stay logged in.
  </div>
  
  <!-- Notification Bell -->
  <div class="notification-bell" id="notificationBell" onclick="toggleNotifications()">
    🔔
    <span class="notification-badge" id="notificationBadge" style="display: none;">0</span>
  </div>
  
  <!-- Notification Panel -->
  <div class="notification-panel" id="notificationPanel">
    <div class="notification-header">
      <strong>🔔 Notifications</strong>
      <button onclick="toggleNotifications()" style="width: auto; padding: 5px 10px; background: white; color: var(--main-blue);">✕</button>
    </div>
    <div class="notification-list" id="notificationList">
      <!-- Notifications populated by JS -->
    </div>
  </div>
  
  <!-- PWA Install Button -->
  <button class="install-pwa-btn" id="installPwaBtn" onclick="installPWA()">
    📱 Install App
  </button>

  <!-- FIXED LOGO - Not Shrinking -->
  <div class="login-wrapper">
    <img src="https://i.ibb.co/4R9Fz7z/dshs-logo.png" alt="DSHS Logo" onerror="this.src='https://via.placeholder.com/120/8B0000/FFFFFF?text=DSHS'">
  </div>

<div class="login" id="login">
  <div class="box">
    <h2>🎓 Login</h2>
    <input type="text" id="loginUsername" placeholder="👤 Username">
    <div class="pass-wrapper">
      <input type="password" id="loginPassword" placeholder="🔒 Password">
      <span onclick="togglePassword()">👁️</span>
    </div>
    <p id="loginAttempts" style="color: #dc3545; font-size: 12px; display: none;">⚠️ Failed attempts: <span id="attemptCount">0</span>/5</p>
    <button onclick="login()" style="background: linear-gradient(135deg, var(--main-red) 0%, var(--main-blue) 100%); color: white; padding: 14px; font-size: 16px;">🚀 Login</button>
    <p id="msg"></p>
    <p style="font-size: 13px; margin-top: 12px;">
      <span class="signup-link" onclick="showForgotPassword()">Forgot Password?</span> | 
      <span class="signup-link" onclick="showSignup()">Sign up!</span>
    </p>
  </div>
</div>

<!-- Forgot Password Modal -->
<div class="login" id="forgotPassword" style="display:none;">
  <div class="box">
    <h2>🔑 Password Recovery</h2>
    <p style="font-size: 13px; color: #666;">Enter your username or email to receive a reset code.</p>
    <input type="text" id="recoveryUsername" placeholder="👤 Username or Email">
    <div id="recoveryCodeSection" style="display: none;">
      <input type="text" id="recoveryCode" placeholder="🔢 Enter 6-digit code" maxlength="6">
      <input type="password" id="newPassword" placeholder="🔒 New Password (min 8 chars)">
      <div class="password-strength" id="passwordStrength"></div>
      <p class="strength-text" id="strengthText"></p>
    </div>
    <button onclick="sendRecoveryCode()" id="sendCodeBtn" style="background: linear-gradient(135deg, var(--main-red) 0%, var(--main-blue) 100%); color: white; padding: 14px; font-size: 16px;">📧 Send Code</button>
    <button onclick="verifyRecoveryCode()" id="verifyCodeBtn" style="display: none; background: #28a745; color: white; padding: 14px; font-size: 16px;">✅ Reset Password</button>
    <p style="font-size: 13px; margin-top: 12px;"><span class="signup-link" onclick="showLogin()">Back to Login</span></p>
  </div>
</div>

<div class="login" id="signup" style="display:none;">
  <div class="box">
    <h2>📝 Student Sign Up</h2>
    <input type="text" id="signupName" placeholder="👤 Full Name">
    <input type="text" id="signupID" placeholder="🆔 Student I.D">
    <input type="text" id="signupSection" placeholder="🏫 Section">
    <input type="text" id="signupTrack" placeholder="📚 Track">
    <input type="text" id="signupStrand" placeholder="🎯 Strand">
    <input type="text" id="signupGradeLevel" placeholder="📖 Grade Level">
    <input type="text" id="signupUsername" placeholder="👤 Username">
    <input type="email" id="signupEmail" placeholder="📧 Email (for verification)">
    <div class="pass-wrapper">
      <input type="password" id="signupPass" placeholder="🔒 Password (min 8 chars)" onkeyup="checkPasswordStrength()">
    </div>
    <div class="password-strength" id="signupPasswordStrength"></div>
    <p class="strength-text" id="signupStrengthText"></p>
    <p style="font-size: 11px; color: #666; text-align: left; margin-top: 5px;">
      Password must contain: 8+ characters, uppercase, lowercase, number, special character
    </p>
    <label style="display: flex; align-items: center; gap: 8px; font-size: 12px; margin: 10px 0;">
      <input type="checkbox" id="agreeTerms" style="width: auto;"> I agree to the Terms and Conditions
    </label>
    <button onclick="submitSignup()" id="signupBtn" style="background: linear-gradient(135deg, var(--main-red) 0%, var(--main-blue) 100%); color: white; padding: 14px; font-size: 16px;" disabled>✅ Create Account</button>
    <button onclick="clearSignup()" style="background: var(--main-gray); color: white;">🔄 Clear</button>
    <p style="font-size: 13px; margin-top: 12px;">Already have an account? <span class="signup-link" onclick="showLogin()">Login</span></p>
  </div>
</div>

<div class="dashboard" id="dashboard">
  <button class="toggle-btn" id="toggleBtn" onclick="toggleMenu()">
    <span class="icon">☰</span>
    <span class="text">Menu</span>
  </button>
  
  <!-- Logout Button in Header -->
  <button class="logout-btn" id="headerLogoutBtn" onclick="logout()">
    <span>🚪</span>
    <span>Logout</span>
  </button>
  
  <div class="menu-overlay" id="menuOverlay" onclick="toggleMenu()"></div>
  <div class="header" id="roleTitle"></div>
  <div class="container">
    <div class="menu" id="menu"></div>
    <div class="content" id="content">
      <div class="school-welcome">
        <h2>🏫 Dumingag Senior High School</h2>
        <p>Welcome to the DSHS School System</p>
      </div>
      <div class="section"><h3>👋 Welcome</h3><p>Select an option from the menu to get started.</p></div>
    </div>
  </div>
</div>

<script>
  //------ SECURITY & AUTHENTICATION ------
  let loginAttempts = 0;
  const MAX_LOGIN_ATTEMPTS = 5;
  let lockoutTimer = null;
  let sessionTimer = null;
  let sessionWarningTimer = null;
  const SESSION_TIMEOUT = 30 * 60 * 1000; // 30 minutes
  const WARNING_BEFORE = 60 * 1000; // 1 minute warning
  
  //------ HASH PASSWORD ------
  function simpleHash(str){
    let hash = 0;
    for(let i=0;i<str.length;i++){
      const char = str.charCodeAt(i);
      hash = ((hash<<5)-hash)+char;
      hash = hash & hash;
    }
    return hash.toString();
  }
  
  //------ PASSWORD STRENGTH CHECKER ------
  function checkPasswordStrength() {
    const password = document.getElementById('signupPass').value;
    const strengthBar = document.getElementById('signupPasswordStrength');
    const strengthText = document.getElementById('signupStrengthText');
    const signupBtn = document.getElementById('signupBtn');
    
    let strength = 0;
    if (password.length >= 8) strength++;
    if (password.match(/[a-z]+/)) strength++;
    if (password.match(/[A-Z]+/)) strength++;
    if (password.match(/[0-9]+/)) strength++;
    if (password.match(/[$@#&!]+/)) strength++;
    
    strengthBar.className = 'password-strength';
    
    if (strength <= 2) {
      strengthBar.classList.add('weak');
      strengthText.textContent = 'Weak - Add more character types';
      strengthText.style.color = '#dc3545';
    } else if (strength === 3 || strength === 4) {
      strengthBar.classList.add('medium');
      strengthText.textContent = 'Medium - Good but could be stronger';
      strengthText.style.color = '#ffc107';
    } else {
      strengthBar.classList.add('strong');
      strengthText.textContent = 'Strong - Excellent password!';
      strengthText.style.color = '#28a745';
    }
    
    // Enable signup button only if password is medium or strong and terms agreed
    const termsAgreed = document.getElementById('agreeTerms').checked;
    signupBtn.disabled = strength < 3 || !termsAgreed;
  }
  
  // Enable/disable signup based on terms checkbox
  document.getElementById('agreeTerms')?.addEventListener('change', checkPasswordStrength);
  
  //------ FORGOT PASSWORD ------
  let recoveryCode = '';
  let recoveryUsername = '';
  
  function showForgotPassword() {
    document.getElementById('login').style.display = 'none';
    document.getElementById('forgotPassword').style.display = 'flex';
    document.getElementById('recoveryCodeSection').style.display = 'none';
    document.getElementById('sendCodeBtn').style.display = 'block';
    document.getElementById('verifyCodeBtn').style.display = 'none';
  }
  
  function sendRecoveryCode() {
    const username = document.getElementById('recoveryUsername').value.trim();
    if (!username) {
      alert('⚠️ Please enter your username or email');
      return;
    }
    
    // Check if user exists
    const user = findUserByUsername(username);
    if (!user) {
      alert('❌ Username not found');
      return;
    }
    
    recoveryUsername = username;
    // Generate random 6-digit code
    recoveryCode = Math.floor(100000 + Math.random() * 900000).toString();
    
    // In real implementation, send email/SMS here
    console.log('Recovery code for', username, ':', recoveryCode); // For demo only
    
    alert('📧 A 6-digit recovery code has been sent to your registered email.\n\nFor demo purposes, check the browser console (F12) to see the code.');
    
    document.getElementById('recoveryCodeSection').style.display = 'block';
    document.getElementById('sendCodeBtn').style.display = 'none';
    document.getElementById('verifyCodeBtn').style.display = 'block';
  }
  
  function verifyRecoveryCode() {
    const enteredCode = document.getElementById('recoveryCode').value;
    const newPassword = document.getElementById('newPassword').value;
    
    if (enteredCode !== recoveryCode) {
      alert('❌ Invalid recovery code');
      return;
    }
    
    if (newPassword.length < 8) {
      alert('⚠️ Password must be at least 8 characters');
      return;
    }
    
    // Update password
    const user = findUserByUsername(recoveryUsername);
    if (user) {
      user.password = simpleHash(newPassword);
      saveData();
      alert('✅ Password reset successful! Please login with your new password.');
      showLogin();
    }
  }
  
  function findUserByUsername(username) {
    if (adminAccount.username === username) return adminAccount;
    const student = students.find(s => s.username === username || s.email === username);
    if (student) return student;
    const teacher = teachers.find(t => t.username === username || t.email === username);
    if (teacher) return teacher;
    const parent = parents.find(p => p.username === username || p.email === username);
    return parent || null;
  }
  
  //------ SESSION MANAGEMENT ------
  function startSessionTimer() {
    // Clear existing timers
    if (sessionTimer) clearTimeout(sessionTimer);
    if (sessionWarningTimer) clearTimeout(sessionWarningTimer);
    
    // Set warning timer
    sessionWarningTimer = setTimeout(() => {
      showSessionWarning();
    }, SESSION_TIMEOUT - WARNING_BEFORE);
    
    // Set logout timer
    sessionTimer = setTimeout(() => {
      logout();
      alert('🔒 Your session has expired. Please login again.');
    }, SESSION_TIMEOUT);
    
    // Reset timer on user activity
    document.addEventListener('click', resetSessionTimer);
    document.addEventListener('keypress', resetSessionTimer);
  }
  
  function resetSessionTimer() {
    // Clear and restart timers
    if (sessionTimer) clearTimeout(sessionTimer);
    if (sessionWarningTimer) clearTimeout(sessionWarningTimer);
    
    document.getElementById('sessionWarning').style.display = 'none';
    
    sessionWarningTimer = setTimeout(() => {
      showSessionWarning();
    }, SESSION_TIMEOUT - WARNING_BEFORE);
    
    sessionTimer = setTimeout(() => {
      logout();
      alert('🔒 Your session has expired. Please login again.');
    }, SESSION_TIMEOUT);
  }
  
  function showSessionWarning() {
    const warning = document.getElementById('sessionWarning');
    const countdown = document.getElementById('countdown');
    warning.style.display = 'block';
    
    let seconds = 60;
    const interval = setInterval(() => {
      seconds--;
      countdown.textContent = seconds;
      if (seconds <= 0) {
        clearInterval(interval);
      }
    }, 1000);
  }
  
  //------ ACCOUNT LOCKOUT ------
  function recordFailedLogin() {
    loginAttempts++;
    document.getElementById('loginAttempts').style.display = 'block';
    document.getElementById('attemptCount').textContent = loginAttempts;
    
    if (loginAttempts >= MAX_LOGIN_ATTEMPTS) {
      lockAccount();
    }
  }
  
  function lockAccount() {
    const loginBtn = document.querySelector('#login button');
    loginBtn.disabled = true;
    loginBtn.textContent = '🔒 Account Locked (5 min)';
    
    let minutes = 5;
    lockoutTimer = setInterval(() => {
      minutes--;
      if (minutes > 0) {
        loginBtn.textContent = `🔒 Account Locked (${minutes} min)`;
      } else {
        clearInterval(lockoutTimer);
        loginAttempts = 0;
        loginBtn.disabled = false;
        loginBtn.textContent = '🚀 Login';
        document.getElementById('loginAttempts').style.display = 'none';
      }
    }, 60000);
  }
  
  function resetLoginAttempts() {
    loginAttempts = 0;
    document.getElementById('loginAttempts').style.display = 'none';
  }

  //------ DATA ------
  let role="", currentUser="";
  let students = JSON.parse(localStorage.getItem('students')) || [
    {name:"Jibdel",id:"ST001",section:"12-A",track:"Academic",strand:"STEM",gradeLevel:"12",username:"jibdel",password:simpleHash("Test123!"),approved:true,pic:"",email:"jibdel@dshs.edu",behaviorLog:[]},
    {name:"Viennes",id:"ST002",section:"12-B",track:"Academic",strand:"ABM",gradeLevel:"12",username:"viennes",password:simpleHash("Test123!"),approved:true,pic:"",email:"viennes@dshs.edu",behaviorLog:[]},
    {name:"Jurl",id:"ST003",section:"12-C",track:"Academic",strand:"STEM",gradeLevel:"12",username:"jurl",password:simpleHash("Test123!"),approved:true,pic:"",email:"jurl@dshs.edu",behaviorLog:[]}
  ];
  let teachers = JSON.parse(localStorage.getItem('teachers')) || [
    {name:"Mr. Smith",id:"TE001",position:"Teacher III",sectionHandled:"12-A",username:"teacher1",password:simpleHash("Teach123!"),email:"smith@dshs.edu"}
  ];
  let parents = JSON.parse(localStorage.getItem('parents')) || [
    {name:"Parent of Jibdel",child:"Jibdel",username:"parentSt001",password:simpleHash("Parent123!"),email:"parent1@email.com"}
  ];
  let subjects = JSON.parse(localStorage.getItem('subjects')) || ["3I's","Genchem 2","P6 2","Perdev","CPAR","Entrepreneurship"];
  let scheduleTimes = JSON.parse(localStorage.getItem('scheduleTimes')) || {"3I's":"8:00-9:00","Genchem 2":"9:00-10:00","P6 2":"10:00-11:00","Perdev":"11:45-12:30","CPAR":"12:30-1:00","Entrepreneurship":"1:00-2:00"};
  
  // Enhanced grade data with history
  let gradesData = JSON.parse(localStorage.getItem('gradesData')) || {};
  let gradeHistory = JSON.parse(localStorage.getItem('gradeHistory')) || {};
  
  if(Object.keys(gradesData).length===0){
    students.forEach(function(s){
      gradesData[s.name]={};
      gradeHistory[s.name]={};
      subjects.forEach(function(sub){ 
        const grade = Math.floor(Math.random()*21)+80;
        gradesData[s.name][sub]=grade;
        gradeHistory[s.name][sub]=[{quarter:1,grade:grade,date:new Date().toISOString()}];
      });
    });
  }
  
  let attendanceData = JSON.parse(localStorage.getItem('attendanceData')) || {};
  if(Object.keys(attendanceData).length===0){
    students.forEach(function(s){ attendanceData[s.name]={}; });
  }
  let bulletinBoard = JSON.parse(localStorage.getItem('bulletinBoard')) || [
    {type: 'text', content: 'Welcome to the DSHS School System!', date: new Date().toISOString()}
  ];
  let plannerData = JSON.parse(localStorage.getItem('plannerData')) || {};
  let adminAccount = {name:"Administrator", username:"admin", password:simpleHash("Admin123!"),email:"admin@dshs.edu"};
  
  // New data structures
  let messages = JSON.parse(localStorage.getItem('messages')) || [];
  let notifications = JSON.parse(localStorage.getItem('notifications')) || [];
  let assignments = JSON.parse(localStorage.getItem('assignments')) || [];
  let submissions = JSON.parse(localStorage.getItem('submissions')) || [];
  let quizzes = JSON.parse(localStorage.getItem('quizzes')) || [];
  let quizResults = JSON.parse(localStorage.getItem('quizResults')) || [];
  let courseMaterials = JSON.parse(localStorage.getItem('courseMaterials')) || [];
  let conferences = JSON.parse(localStorage.getItem('conferences')) || [];
  
  //------ SAVE ------
  function saveData(){
    try{
      localStorage.setItem('students', JSON.stringify(students));
      localStorage.setItem('teachers', JSON.stringify(teachers));
      localStorage.setItem('parents', JSON.stringify(parents));
      localStorage.setItem('subjects', JSON.stringify(subjects));
      localStorage.setItem('scheduleTimes', JSON.stringify(scheduleTimes));
      localStorage.setItem('gradesData', JSON.stringify(gradesData));
      localStorage.setItem('gradeHistory', JSON.stringify(gradeHistory));
      localStorage.setItem('attendanceData', JSON.stringify(attendanceData));
      localStorage.setItem('bulletinBoard', JSON.stringify(bulletinBoard));
      localStorage.setItem('plannerData', JSON.stringify(plannerData));
      localStorage.setItem('messages', JSON.stringify(messages));
      localStorage.setItem('notifications', JSON.stringify(notifications));
      localStorage.setItem('assignments', JSON.stringify(assignments));
      localStorage.setItem('submissions', JSON.stringify(submissions));
      localStorage.setItem('quizzes', JSON.stringify(quizzes));
      localStorage.setItem('quizResults', JSON.stringify(quizResults));
      localStorage.setItem('courseMaterials', JSON.stringify(courseMaterials));
      localStorage.setItem('conferences', JSON.stringify(conferences));
    } catch(e){ alert("Storage error."); }
  }

  //------ TOGGLE MENU ------
  function toggleMenu(){
    const menu = document.getElementById("menu");
    const overlay = document.getElementById("menuOverlay");
    const toggleBtn = document.getElementById("toggleBtn");
    
    menu.classList.toggle("show");
    overlay.classList.toggle("show");
    
    if (menu.classList.contains("show")) {
      toggleBtn.classList.add("small");
    } else {
      toggleBtn.classList.remove("small");
    }
  }

  //------ PASSWORD TOGGLE ------
  function togglePassword(){
    const p=document.getElementById("loginPassword");
    p.type=(p.type==="password")?"text":"password";
  }

  //------ LOGIN ------
  function login(){
    const username=document.getElementById("loginUsername").value.trim();
    const pass=simpleHash(document.getElementById("loginPassword").value);
    let found=false;

    // Check for lockout
    if (loginAttempts >= MAX_LOGIN_ATTEMPTS) {
      alert('🔒 Account is temporarily locked. Please try again later.');
      return;
    }

    if(username===adminAccount.username && pass===adminAccount.password){ 
      role="admin"; 
      currentUser=adminAccount.name; 
      found=true; 
    }
    
    teachers.forEach(function(t){ 
      if(username===t.username && pass===t.password){ 
        role="teacher"; 
        currentUser=t.name; 
        found=true; 
      }
    });
    
    students.forEach(function(s){ 
      if(username===s.username && pass===s.password){ 
        if(s.approved){ 
          role="student"; 
          currentUser=s.name; 
          found=true; 
        } else { 
          document.getElementById("msg").innerText="Account not approved yet"; 
          recordFailedLogin();
          return; 
        } 
      }
    });
    
    parents.forEach(function(p){ 
      if(username===p.username && pass===p.password){ 
        role="parent"; 
        currentUser=p.child; 
        found=true; 
      }
    });

    if(!found){ 
      document.getElementById("msg").innerText="Invalid login or not approved yet"; 
      recordFailedLogin();
      return; 
    }
    
    resetLoginAttempts();
    
    document.getElementById("login").style.display="none";
    document.getElementById("signup").style.display="none";
    document.getElementById("dashboard").style.display="block";
    loadDashboard();
    startSessionTimer();
    addNotification('Welcome back, ' + currentUser + '!', 'system');
  }

  //------ DASHBOARD ------
  function loadDashboard(){
    document.getElementById("roleTitle").innerText = role.toUpperCase()+" DASHBOARD";
    const menu=document.getElementById("menu"); menu.innerHTML="";
    
    let items = [];
    
    if(role==="teacher") {
      items = [
        {name: "Attendance", icon: "📅"},
        {name: "Subject Schedule", icon: "📚"},
        {name: "My Info", icon: "👤"},
        {name: "Bulletin Board", icon: "📢"},
        {name: "Grades", icon: "📊"},
        {name: "Report Cards", icon: "📋"},
        {name: "Assignments", icon: "📝"},
        {name: "Quizzes", icon: "❓"},
        {name: "Course Materials", icon: "📖"},
        {name: "Messages", icon: "💬"},
        {name: "Conference Schedule", icon: "📅"},
        {name: "Behavior Reports", icon: "📋"},
        {name: "My Planner", icon: "🗓️"},
        {name: "Emergency Numbers", icon: "🚨"},
        {name: "School Map", icon: "🗺️"},
        {name: "Settings", icon: "⚙️"}
      ];
    }
    else if(role==="student") {
      items = [
        {name: "Attendance", icon: "📅"},
        {name: "Subject Schedule", icon: "📚"},
        {name: "My Info", icon: "👤"},
        {name: "Bulletin Board", icon: "📢"},
        {name: "Grades", icon: "📊"},
        {name: "Assignments", icon: "📝"},
        {name: "Quizzes", icon: "❓"},
        {name: "Course Materials", icon: "📖"},
        {name: "QR Code", icon: "🔳"},
        {name: "Messages", icon: "💬"},
        {name: "My Mood", icon: "😊"},
        {name: "Honor Roll", icon: "🏆"},
        {name: "Emergency Numbers", icon: "🚨"},
        {name: "School Map", icon: "🗺️"},
        {name: "Settings", icon: "⚙️"}
      ];
    }
    else if(role==="parent") {
      items = [
        {name: "Child Attendance", icon: "📅"},
        {name: "Child Grades", icon: "📊"},
        {name: "Child Schedule", icon: "📚"},
        {name: "My Info", icon: "👤"},
        {name: "Bulletin Board", icon: "📢"},
        {name: "Messages", icon: "💬"},
        {name: "Conference Booking", icon: "📅"},
        {name: "Progress Reports", icon: "📈"},
        {name: "Behavior Reports", icon: "📋"},
        {name: "Emergency Numbers", icon: "🚨"},
        {name: "School Map", icon: "🗺️"},
        {name: "Settings", icon: "⚙️"}
      ];
    }
    else if(role==="admin") {
      items = [
        {name: "Student Approval", icon: "✅"},
        {name: "Create Teacher", icon: "👨‍🏫"},
        {name: "All Students", icon: "👨‍🎓"},
        {name: "Parent Accounts", icon: "👪"},
        {name: "Manage Users", icon: "⚙️"},
        {name: "Enrollment", icon: "📝"},
        {name: "Class Scheduling", icon: "📅"},
        {name: "Honor Roll", icon: "🏆"},
        {name: "Analytics", icon: "📊"},
        {name: "Bulletin Board", icon: "📢"},
        {name: "Emergency Numbers", icon: "🚨"},
        {name: "School Map", icon: "🗺️"},
        {name: "Settings", icon: "⚙️"}
      ];
    }

    menu.innerHTML = '<div class="menu-header"><h3>🎓 DSHS Menu</h3><p>👤 ' + currentUser + '</p></div>';
    
    items.forEach(function(item){
      const btn=document.createElement("button");
      btn.innerHTML = '<span class="tab-icon">' + item.icon + '</span> ' + item.name;
      btn.onclick=function() {
        toggleMenu();
        loadSection(item.name);
      };
      menu.appendChild(btn);
    });

    // Logout button in menu
    const logoutBtn=document.createElement("button"); 
    logoutBtn.innerHTML = '<span class="tab-icon">🚪</span> Logout'; 
    logoutBtn.className="logout"; 
    logoutBtn.onclick=function() {
      toggleMenu();
      logout();
    }; 
    menu.appendChild(logoutBtn);
    
    // Update notification badge
    updateNotificationBadge();
  }

  //------ LOGOUT ------
  function logout(){
    role=""; currentUser="";
    document.getElementById("dashboard").style.display="none";
    document.getElementById("login").style.display="flex";
    document.getElementById("menu").classList.remove("show");
    document.getElementById("menuOverlay").classList.remove("show");
    
    // Clear session timers
    if (sessionTimer) clearTimeout(sessionTimer);
    if (sessionWarningTimer) clearTimeout(sessionWarningTimer);
    document.getElementById('sessionWarning').style.display = 'none';
  }

  //------ SHOW SIGNUP/LOGIN ------
  function showSignup(){ document.getElementById("login").style.display="none"; document.getElementById("signup").style.display="flex"; }
  function showLogin(){ 
    document.getElementById("signup").style.display="none"; 
    document.getElementById("forgotPassword").style.display="none";
    document.getElementById("login").style.display="flex"; 
  }
  function clearSignup(){ 
    ["signupName","signupID","signupSection","signupTrack","signupStrand","signupGradeLevel","signupUsername","signupEmail","signupPass"].forEach(function(id){ 
      document.getElementById(id).value=""; 
    });
    document.getElementById('signupPasswordStrength').className = 'password-strength';
    document.getElementById('signupStrengthText').textContent = '';
    document.getElementById('agreeTerms').checked = false;
    document.getElementById('signupBtn').disabled = true;
  }

  //------ SUBMIT SIGNUP ------
  function submitSignup(){
    let name=document.getElementById("signupName").value.trim();
    let id=document.getElementById("signupID").value.trim();
    let section=document.getElementById("signupSection").value.trim();
    let track=document.getElementById("signupTrack").value.trim();
    let strand=document.getElementById("signupStrand").value.trim();
    let gradeLevel=document.getElementById("signupGradeLevel").value.trim();
    let username=document.getElementById("signupUsername").value.trim();
    let email=document.getElementById("signupEmail").value.trim();
    let password=document.getElementById("signupPass").value.trim();
    
    if(!name||!id||!section||!track||!strand||!gradeLevel||!username||!email||!password){ 
      alert("All fields required!"); 
      return; 
    }
    
    // Validate email
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!emailRegex.test(email)) {
      alert("Please enter a valid email address");
      return;
    }
    
    // Check password strength
    let strength = 0;
    if (password.length >= 8) strength++;
    if (password.match(/[a-z]+/)) strength++;
    if (password.match(/[A-Z]+/)) strength++;
    if (password.match(/[0-9]+/)) strength++;
    if (password.match(/[$@#&!]+/)) strength++;
    
    if (strength < 3) {
      alert("Password is too weak. Please follow the requirements.");
      return;
    }
    
    if(students.find(function(s){ return s.username===username; }) || students.find(function(s){ return s.id===id; })){ 
      alert("Username or ID already exists!"); 
      return; 
    }

    // Generate verification code
    const verificationCode = Math.floor(100000 + Math.random() * 900000).toString();
    
    // Store pending verification
    const pendingUser = {
      name,id,section,track,strand,gradeLevel,username,email,
      password:simpleHash(password),
      approved:false,
      pic:"",
      verificationCode: verificationCode,
      behaviorLog: []
    };
    
    localStorage.setItem('pendingVerification', JSON.stringify(pendingUser));
    
    // In real implementation, send email
    console.log('Verification code for', email, ':', verificationCode);
    
    alert('📧 A verification code has been sent to ' + email + '\n\nFor demo, check browser console (F12).\n\nPlease verify your email to complete registration.');
    
    // Show verification input (simplified - in real app, show new screen)
    const verifyCode = prompt('Enter the 6-digit verification code:');
    if (verifyCode === verificationCode) {
      // Move from pending to approved
      students.push(pendingUser);
      delete pendingUser.verificationCode;
      pendingUser.approved = true;
      
      // Generate parent account
      let parentUsername = "parent" + id;
      let parentPassword = Math.random().toString(36).slice(-6);
      
      gradesData[name]={}; 
      subjects.forEach(function(sub){ 
        gradesData[name][sub]=0; 
        gradeHistory[name] = gradeHistory[name] || {};
        gradeHistory[name][sub] = [];
      });
      attendanceData[name]={};
      
      parents.push({
        name: "Parent of " + name,
        child: name,
        username: parentUsername,
        password: simpleHash(parentPassword),
        email: "parent." + email
      });
      
      let parentAccounts = JSON.parse(localStorage.getItem('parentAccounts')) || [];
      parentAccounts.push({
        studentName: name,
        studentId: id,
        section: section,
        parentUsername: parentUsername,
        parentPassword: parentPassword
      });
      localStorage.setItem('parentAccounts', JSON.stringify(parentAccounts));
      localStorage.removeItem('pendingVerification');
      
      saveData();
      alert("✅ Email verified! Signup successful!\n\nParent Account:\nUsername: " + parentUsername + "\nPassword: " + parentPassword);
      clearSignup(); 
      showLogin();
    } else {
      alert('❌ Invalid verification code. Please try again.');
    }
  }
  
  //------ NOTIFICATION SYSTEM ------
  function addNotification(message, type = 'info', recipient = null) {
    const notification = {
      id: Date.now(),
      message: message,
      type: type,
      recipient: recipient || currentUser,
      date: new Date().toISOString(),
      read: false
    };
    
    notifications.push(notification);
    saveData();
    updateNotificationBadge();
    
    // Show browser notification if permitted
    if ('Notification' in window && Notification.permission === 'granted') {
      new Notification('DSHS School System', {
        body: message,
        icon: 'https://via.placeholder.com/100'
      });
    }
  }
  
  function updateNotificationBadge() {
    const unreadCount = notifications.filter(n => n.recipient === currentUser && !n.read).length;
    const badge = document.getElementById('notificationBadge');
    
    if (unreadCount > 0) {
      badge.textContent = unreadCount > 99 ? '99+' : unreadCount;
      badge.style.display = 'flex';
    } else {
      badge.style.display = 'none';
    }
  }
  
  function toggleNotifications() {
    const panel = document.getElementById('notificationPanel');
    panel.classList.toggle('show');
    
    if (panel.classList.contains('show')) {
      renderNotifications();
    }
  }
  
  function renderNotifications() {
    const list = document.getElementById('notificationList');
    const userNotifications = notifications.filter(n => n.recipient === currentUser).sort((a,b) => new Date(b.date) - new Date(a.date));
    
    if (userNotifications.length === 0) {
      list.innerHTML = '<div style="padding: 20px; text-align: center; color: #666;">No notifications</div>';
      return;
    }
    
    list.innerHTML = '';
    userNotifications.forEach(n => {
      const item = document.createElement('div');
      item.className = 'notification-item' + (n.read ? '' : ' unread');
      item.innerHTML = `
        <div style="font-weight: ${n.read ? 'normal' : 'bold'}">${n.message}</div>
        <div style="font-size: 11px; color: #666; margin-top: 5px;">${new Date(n.date).toLocaleString()}</div>
      `;
      item.onclick = () => {
        n.read = true;
        saveData();
        updateNotificationBadge();
        renderNotifications();
      };
      list.appendChild(item);
    });
  }
  
  // Request notification permission on load
  if ('Notification' in window && Notification.permission === 'default') {
    Notification.requestPermission();
  }

  //------ LOAD SECTION ------
  function loadSection(tab){
    const content=document.getElementById("content");
    content.innerHTML="";
    const section=document.createElement("div"); 
    section.className="section";

    //------ ATTENDANCE ------
    if(tab==="Attendance" || tab==="Child Attendance"){
      section.innerHTML="<h3>📅 Attendance</h3>";
      
      let studentName = currentUser;
      if(role === "teacher") {
        const selectStudent = document.createElement("select");
        selectStudent.id = "attendanceStudentSelect";
        const teacher = teachers.find(function(t){ return t.name === currentUser; });
        students.filter(function(s){ return s.section === teacher.sectionHandled; }).forEach(function(s){
          const opt = document.createElement("option");
          opt.value = s.name;
          opt.innerText = s.name;
          selectStudent.appendChild(opt);
        });
        section.appendChild(selectStudent);
        studentName = selectStudent.value;
        
        selectStudent.onchange = function() {
          studentName = this.value;
          renderAttendanceCalendar(section, studentName);
        };
      } else if(role === "parent") {
        const selectStudent = document.createElement("select");
        selectStudent.id = "attendanceStudentSelect";
        const parent = parents.find(function(p){ return p.child === currentUser; });
        if(parent) {
          const child = students.find(function(s){ return s.name === parent.child; });
          if(child) {
            const opt = document.createElement("option");
            opt.value = child.name;
            opt.innerText = child.name;
            selectStudent.appendChild(opt);
            studentName = child.name;
          }
        }
        section.appendChild(selectStudent);
      }
      
      const dateInput = document.createElement("input");
      dateInput.type = "month";
      dateInput.value = new Date().toISOString().split("T")[0].substring(0, 7);
      section.appendChild(dateInput);
      
      content.appendChild(section);
      renderAttendanceCalendar(section, studentName);
      
      // QR Scanner for teachers
      if(role === "teacher") {
        const scannerDiv = document.createElement("div");
        scannerDiv.id = "qr-scanner";
        scannerDiv.className = "qr-scanner-container";
        scannerDiv.style.marginTop = "20px";
        section.appendChild(scannerDiv);
        
        const scannerResult = document.createElement("div");
        scannerResult.id = "scanner-result";
        scannerResult.className = "scanner-result";
        scannerResult.style.display = "none";
        section.appendChild(scannerResult);
        
        setTimeout(function(){
          try {
            const html5QrCode = new Html5Qrcode("qr-scanner");
            html5QrCode.start(
              { facingMode: "environment" },
              { fps: 10, qrbox: 250 },
              function(decodedText){
                const parts = decodedText.split("-");
                const scannedStudent = parts[0];
                const today = new Date().toISOString().split("T")[0];
                
                if(attendanceData[scannedStudent]) {
                  attendanceData[scannedStudent][today] = "P";
                  saveData();
                  const resultDiv = document.getElementById("scanner-result");
                  resultDiv.style.display = "block";
                  resultDiv.className = "scanner-result success";
                  resultDiv.innerHTML = "✅ Marked " + scannedStudent + " present on " + today;
                  renderAttendanceCalendar(section, studentName);
                  
                  // Notify student
                  addNotification('You were marked present on ' + today, 'attendance', scannedStudent);
                }
                html5QrCode.stop();
              },
              function(errorMessage){}
            ).catch(function(err){
              console.log("Scanner error:", err);
            });
          } catch(e) {
            console.log("Scanner initialization error:", e);
          }
        }, 500);
      }
    }
    
    //------ GRADES ------
    if(tab === "Grades" || tab === "Child Grades"){
      section.innerHTML = "<h3>📊 Grades</h3>";
      
      // Semester Selector
      const semSelector = document.createElement("div");
      semSelector.className = "sem-selector";
      semSelector.innerHTML = `
        <button class="sem-btn active" onclick="selectSemester(1, this)">1st Semester</button>
        <button class="sem-btn" onclick="selectSemester(2, this)">2nd Semester</button>
      `;
      section.appendChild(semSelector);
      
      // Quarter Selector
      const quarterSelector = document.createElement("div");
      quarterSelector.className = "quarter-selector";
      quarterSelector.id = "quarterSelector";
      quarterSelector.innerHTML = `
        <button class="quarter-btn active" onclick="selectQuarter(1, this)">1st Quarter</button>
        <button class="quarter-btn" onclick="selectQuarter(2, this)">2nd Quarter</button>
        <button class="quarter-btn" onclick="selectQuarter(3, this)">3rd Quarter</button>
        <button class="quarter-btn" onclick="selectQuarter(4, this)">4th Quarter</button>
      `;
      section.appendChild(quarterSelector);
      
      // Grade History Toggle
      const historyToggle = document.createElement('label');
      historyToggle.style.cssText = 'display: flex; align-items: center; gap: 10px; margin: 15px 0; cursor: pointer;';
      historyToggle.innerHTML = '<input type="checkbox" id="showHistory" style="width: auto;" onchange="toggleGradeHistory()"> Show Grade History';
      section.appendChild(historyToggle);
      
      let studentName = currentUser;
      if(role === "teacher") {
        const selectStudent = document.createElement("select");
        selectStudent.id = "gradesStudentSelect";
        const teacher = teachers.find(function(t){ return t.name === currentUser; });
        students.filter(function(s){ return s.section === teacher.sectionHandled; }).forEach(function(s){
          const opt = document.createElement("option");
          opt.value = s.name;
          opt.innerText = s.name;
          selectStudent.appendChild(opt);
        });
        section.appendChild(selectStudent);
        studentName = selectStudent.value;
        
        selectStudent.onchange = function() {
          studentName = this.value;
          renderGradesTable(section, studentName);
        };
      } else if(role === "parent") {
        const parent = parents.find(function(p){ return p.child === currentUser; });
        if(parent) {
          const child = students.find(function(s){ return s.name === parent.child; });
          if(child) studentName = child.name;
        }
      }
      
      content.appendChild(section);
      renderGradesTable(section, studentName);
    }
    
    //------ REPORT CARDS (Teacher) ------
    if(tab === "Report Cards" && role === "teacher"){
      section.innerHTML = "<h3>📋 Report Cards</h3>";
      section.innerHTML += "<p>Generate official DepEd-style report cards for students.</p>";
      
      const teacher = teachers.find(t => t.name === currentUser);
      const sectionSelect = document.createElement('select');
      sectionSelect.innerHTML = '<option value="">Select Section</option>';
      const sections = [...new Set(students.filter(s => s.section === teacher.sectionHandled).map(s => s.section))];
      sections.forEach(sec => {
        sectionSelect.innerHTML += `<option value="${sec}">${sec}</option>`;
      });
      section.appendChild(sectionSelect);
      
      const generateBtn = document.createElement('button');
      generateBtn.innerHTML = '📄 Generate Report Cards';
      generateBtn.style.cssText = 'background: linear-gradient(135deg, var(--main-red) 0%, var(--main-blue) 100%); color: white; margin-top: 15px;';
      generateBtn.onclick = () => generateReportCards(sectionSelect.value);
      section.appendChild(generateBtn);
      
      const reportContainer = document.createElement('div');
      reportContainer.id = 'reportCardsContainer';
      section.appendChild(reportContainer);
      
      content.appendChild(section);
    }
    
    //------ ASSIGNMENTS ------
    if(tab === "Assignments"){
      section.innerHTML = "<h3>📝 Assignments</h3>";
      
      if(role === "teacher"){
        // Teacher view - Create and grade assignments
        section.innerHTML += `
          <div style="margin-bottom: 20px;">
            <button onclick="showCreateAssignment()" style="background: #28a745; color: white; width: auto;">➕ Create Assignment</button>
          </div>
          <div id="createAssignmentForm" style="display: none; background: #f8f9fa; padding: 20px; border-radius: 10px; margin-bottom: 20px;">
            <input type="text" id="assignmentTitle" placeholder="Assignment Title">
            <textarea id="assignmentDesc" placeholder="Description" rows="3"></textarea>
            <select id="assignmentSubject">
              ${subjects.map(s => `<option value="${s}">${s}</option>`).join('')}
            </select>
            <input type="date" id="assignmentDue">
            <input type="number" id="assignmentPoints" placeholder="Total Points" value="100">
            <input type="file" id="assignmentFile" style="width: auto;">
            <button onclick="createAssignment()" style="background: var(--main-blue); color: white; margin-top: 10px;">✅ Create</button>
          </div>
        `;
        
        // List assignments
        const teacherAssignments = assignments.filter(a => a.teacher === currentUser);
        if(teacherAssignments.length === 0){
          section.innerHTML += "<p>No assignments created yet.</p>";
        } else {
          teacherAssignments.forEach(a => {
            const submissions_count = submissions.filter(s => s.assignmentId === a.id).length;
            section.innerHTML += `
              <div class="assignment-card">
                <h4>${a.title}</h4>
                <p>${a.description}</p>
                <p><strong>Subject:</strong> ${a.subject} | <strong>Due:</strong> ${new Date(a.dueDate).toLocaleDateString()}</p>
                <p><strong>Submissions:</strong> ${submissions_count}/${students.filter(s => s.section === teachers.find(t => t.name === currentUser).sectionHandled).length}</p>
                <button onclick="viewSubmissions('${a.id}')" style="width: auto; background: var(--main-blue); color: white;">📋 View Submissions</button>
              </div>
            `;
          });
        }
      } else if(role === "student"){
        // Student view - View and submit assignments
        const student = students.find(s => s.name === currentUser);
        const studentAssignments = assignments.filter(a => 
          a.section === student.section && new Date(a.dueDate) >= new Date()
        );
        
        if(studentAssignments.length === 0){
          section.innerHTML += "<p>No pending assignments. Great job! 🎉</p>";
        } else {
          studentAssignments.forEach(a => {
            const submission = submissions.find(s => s.assignmentId === a.id && s.student === currentUser);
            let status = 'pending';
            let statusText = '⏳ Pending';
            let grade = '';
            
            if(submission){
              status = submission.graded ? 'graded' : 'submitted';
              statusText = submission.graded ? '✅ Graded' : '📤 Submitted';
              grade = submission.graded ? `<br><strong>Grade: ${submission.grade}/${a.points}</strong>` : '';
            } else if(new Date(a.dueDate) < new Date()){
              status = 'late';
              statusText = '⚠️ Late';
            }
            
            section.innerHTML += `
              <div class="assignment-card">
                <div style="display: flex; justify-content: space-between; align-items: start;">
                  <div>
                    <h4>${a.title}</h4>
                    <p>${a.description}</p>
                    <p><strong>Subject:</strong> ${a.subject} | <strong>Due:</strong> ${new Date(a.dueDate).toLocaleDateString()}</p>
                    <span class="assignment-status ${status}">${statusText}</span>
                    ${grade}
                  </div>
                  ${!submission ? `
                    <button onclick="submitAssignment('${a.id}')" style="width: auto; background: #28a745; color: white;">📤 Submit</button>
                  ` : ''}
                </div>
              </div>
            `;
          });
        }
      }
      content.appendChild(section);
    }
    
    //------ QUIZZES ------
    if(tab === "Quizzes"){
      section.innerHTML = "<h3>❓ Quizzes & Exams</h3>";
      
      if(role === "teacher"){
        section.innerHTML += `
          <button onclick="showCreateQuiz()" style="background: #28a745; color: white; width: auto; margin-bottom: 20px;">➕ Create Quiz</button>
          <div id="createQuizForm" style="display: none; background: #f8f9fa; padding: 20px; border-radius: 10px;">
            <input type="text" id="quizTitle" placeholder="Quiz Title">
            <select id="quizSubject">${subjects.map(s => `<option value="${s}">${s}</option>`).join('')}</select>
            <input type="number" id="quizDuration" placeholder="Duration (minutes)" value="30">
            <input type="datetime-local" id="quizStart">
            <input type="datetime-local" id="quizEnd">
            <div id="quizQuestions"></div>
            <button onclick="addQuestion()" style="width: auto; background: var(--sub-yellow); color: #333; margin: 10px 5px 10px 0;">➕ Add Question</button>
            <button onclick="saveQuiz()" style="background: #28a745; color: white;">💾 Save Quiz</button>
          </div>
        `;
        
        // List quizzes
        const teacherQuizzes = quizzes.filter(q => q.teacher === currentUser);
        teacherQuizzes.forEach(q => {
          const results = quizResults.filter(r => r.quizId === q.id);
          section.innerHTML += `
            <div class="assignment-card">
              <h4>${q.title}</h4>
              <p><strong>Subject:</strong> ${q.subject} | <strong>Questions:</strong> ${q.questions.length}</p>
              <p><strong>Participants:</strong> ${results.length} | <strong>Average:</strong> ${results.length ? (results.reduce((a,b) => a + b.score, 0) / results.length).toFixed(2) : 'N/A'}%</p>
              <button onclick="viewQuizResults('${q.id}')" style="width: auto; background: var(--main-blue); color: white;">📊 Results</button>
            </div>
          `;
        });
      } else if(role === "student"){
        const student = students.find(s => s.name === currentUser);
        const availableQuizzes = quizzes.filter(q => 
          q.section === student.section && 
          new Date(q.startTime) <= new Date() && 
          new Date(q.endTime) >= new Date()
        );
        
        availableQuizzes.forEach(q => {
          const taken = quizResults.find(r => r.quizId === q.id && r.student === currentUser);
          if(!taken){
            section.innerHTML += `
              <div class="assignment-card">
                <h4>${q.title}</h4>
                <p><strong>Subject:</strong> ${q.subject} | <strong>Duration:</strong> ${q.duration} minutes</p>
                <p><strong>Questions:</strong> ${q.questions.length} | <strong>Ends:</strong> ${new Date(q.endTime).toLocaleString()}</p>
                <button onclick="startQuiz('${q.id}')" style="background: #28a745; color: white;">▶️ Start Quiz</button>
              </div>
            `;
          } else {
            section.innerHTML += `
              <div class="assignment-card">
                <h4>${q.title} ✅</h4>
                <p><strong>Score:</strong> ${taken.score}% | <strong>Submitted:</strong> ${new Date(taken.submittedAt).toLocaleString()}</p>
              </div>
            `;
          }
        });
      }
      content.appendChild(section);
    }
    
    //------ COURSE MATERIALS ------
    if(tab === "Course Materials"){
      section.innerHTML = "<h3>📖 Course Materials</h3>";
      
      if(role === "teacher"){
        // Teacher can upload materials
        section.innerHTML += `
          <div style="background: #f8f9fa; padding: 20px; border-radius: 10px; margin-bottom: 20px;">
            <h4>Upload New Material</h4>
            <input type="text" id="materialTitle" placeholder="Material Title">
            <textarea id="materialDesc" placeholder="Description" rows="2"></textarea>
            <select id="materialSubject">
              ${subjects.map(s => `<option value="${s}">${s}</option>`).join('')}
            </select>
            <select id="materialSection">
              <option value="all">All Sections</option>
              ${[...new Set(students.map(s => s.section))].map(sec => `<option value="${sec}">${sec}</option>`).join('')}
            </select>
            <input type="file" id="materialFile" style="width: auto;">
            <button onclick="uploadMaterial()" style="background: #28a745; color: white; margin-top: 10px;">⬆️ Upload Material</button>
          </div>
        `;
        
        // List teacher's uploaded materials
        const myMaterials = courseMaterials.filter(m => m.uploadedBy === currentUser);
        if(myMaterials.length > 0){
          section.innerHTML += "<h4>Your Uploaded Materials</h4>";
          myMaterials.forEach(m => {
            section.innerHTML += `
              <div style="padding: 15px; border: 2px solid #dee2e6; border-radius: 10px; margin: 10px 0;">
                <h5>${m.title}</h5>
                <p>${m.description}</p>
                <p style="font-size: 12px; color: #666;">Subject: ${m.subject} | Section: ${m.section} | Uploaded: ${new Date(m.uploadedAt).toLocaleDateString()}</p>
                <button onclick="deleteMaterial('${m.id}')" style="width: auto; background: #dc3545; color: white;">🗑️ Delete</button>
              </div>
            `;
          });
        }
      } else if(role === "student"){
        // Student view materials
        const student = students.find(s => s.name === currentUser);
        
        subjects.forEach(sub => {
          const materials = courseMaterials.filter(m => m.subject === sub && (m.section === 'all' || m.section === student.section));
          if(materials.length > 0){
            section.innerHTML += `<h4>${sub}</h4>`;
            materials.forEach(m => {
              section.innerHTML += `
                <div style="padding: 15px; border: 2px solid #dee2e6; border-radius: 10px; margin: 10px 0;">
                  <h5>${m.title}</h5>
                  <p>${m.description}</p>
                  <p style="font-size: 12px; color: #666;">Uploaded: ${new Date(m.uploadedAt).toLocaleDateString()}</p>
                  <a href="${m.fileUrl}" download style="display: inline-block; padding: 8px 15px; background: var(--main-blue); color: white; border-radius: 5px; text-decoration: none; margin-top: 10px;">⬇️ Download</a>
                </div>
              `;
            });
          }
        });
        
        if(section.innerHTML === "<h3>📖 Course Materials</h3>"){
          section.innerHTML += "<p>No materials uploaded yet.</p>";
        }
      }
      content.appendChild(section);
    }
    
    //------ MESSAGES (Real-time Chat) ------
    if(tab === "Messages"){
      section.innerHTML = "<h3>💬 Messages</h3>";
      
      const messagingContainer = document.createElement('div');
      messagingContainer.className = 'messaging-container';
      
      // Contacts list
      const contactsList = document.createElement('div');
      contactsList.className = 'contacts-list';
      
      let contacts = [];
      if(role === 'teacher'){
        // Show parents of students in teacher's section
        const teacher = teachers.find(t => t.name === currentUser);
        const sectionStudents = students.filter(s => s.section === teacher.sectionHandled);
        sectionStudents.forEach(s => {
          const parent = parents.find(p => p.child === s.name);
          if(parent){
            contacts.push({name: parent.name, username: parent.username, type: 'parent', avatar: '👨‍👩‍👧'});
          }
        });
      } else if(role === 'parent'){
        // Show teachers of parent's child
        const parent = parents.find(p => p.child === currentUser);
        const child = students.find(s => s.name === parent.child);
        const childTeachers = teachers.filter(t => t.sectionHandled === child.section);
        childTeachers.forEach(t => {
          contacts.push({name: t.name, username: t.username, type: 'teacher', avatar: '👨‍🏫'});
        });
      } else if(role === 'student'){
        // Show teachers and classmates
        const student = students.find(s => s.name === currentUser);
        const classTeachers = teachers.filter(t => t.sectionHandled === student.section);
        classTeachers.forEach(t => {
          contacts.push({name: t.name, username: t.username, type: 'teacher', avatar: '👨‍🏫'});
        });
      }
      
      contacts.forEach((contact, index) => {
        const contactDiv = document.createElement('div');
        contactDiv.className = 'contact-item' + (index === 0 ? ' active' : '');
        contactDiv.innerHTML = `
          <div class="contact-avatar">${contact.avatar}</div>
          <div>
            <div style="font-weight: bold;">${contact.name}</div>
            <div style="font-size: 12px; color: #666;">${contact.type}</div>
          </div>
        `;
        contactDiv.onclick = () => selectContact(contact, contactDiv);
        contactsList.appendChild(contactDiv);
      });
      
      messagingContainer.appendChild(contactsList);
      
      // Chat area
      const chatArea = document.createElement('div');
      chatArea.className = 'chat-area';
      chatArea.id = 'chatArea';
      
      chatArea.innerHTML = `
        <div class="chat-header" id="chatHeader">Select a contact to start messaging</div>
        <div class="chat-messages" id="chatMessages"></div>
        <div class="chat-input-area">
          <input type="text" id="messageInput" placeholder="Type your message..." disabled>
          <button onclick="sendMessage()" id="sendBtn" disabled style="width: auto; background: var(--main-blue); color: white;">📤</button>
        </div>
      `;
      
      messagingContainer.appendChild(chatArea);
      section.appendChild(messagingContainer);
      
      // Load first contact if exists
      if(contacts.length > 0){
        setTimeout(() => selectContact(contacts[0], contactsList.firstChild), 100);
      }
      
      content.appendChild(section);
    }
    
    //------ CONFERENCE SCHEDULING ------
    if(tab === "Conference Schedule" || tab === "Conference Booking"){
      section.innerHTML = "<h3>📅 Parent-Teacher Conference</h3>";
      
      if(role === "teacher"){
        // Teacher sets available slots
        section.innerHTML += `
          <div style="background: #f8f9fa; padding: 20px; border-radius: 10px; margin-bottom: 20px;">
            <h4>Set Available Slots</h4>
            <input type="date" id="conferenceDate">
            <input type="time" id="conferenceStart">
            <input type="time" id="conferenceEnd">
            <input type="number" id="slotDuration" placeholder="Slot duration (minutes)" value="30">
            <button onclick="generateSlots()" style="background: #28a745; color: white; margin-top: 10px;">➕ Generate Slots</button>
          </div>
        `;
        
        // Show booked conferences
        const teacherConferences = conferences.filter(c => c.teacher === currentUser);
        if(teacherConferences.length > 0){
          section.innerHTML += "<h4>Upcoming Conferences</h4>";
          teacherConferences.filter(c => new Date(c.dateTime) >= new Date()).forEach(c => {
            const parent = parents.find(p => p.username === c.parent);
            section.innerHTML += `
              <div class="conference-slot booked">
                <div>
                  <strong>${parent ? parent.name : 'Unknown'}</strong><br>
                  <small>${new Date(c.dateTime).toLocaleString()}</small>
                </div>
                <span style="background: #dc3545; color: white; padding: 5px 15px; border-radius: 20px; font-size: 12px;">Booked</span>
              </div>
            `;
          });
        }
      } else if(role === "parent"){
        // Parent books available slots
        const parent = parents.find(p => p.child === currentUser);
        const child = students.find(s => s.name === parent.child);
        
        // Get available slots from child's teachers
        const availableSlots = conferences.filter(c => 
          c.status === 'available' && 
          teachers.find(t => t.name === c.teacher)?.sectionHandled === child.section
        );
        
        if(availableSlots.length === 0){
          section.innerHTML += "<p>No available slots at the moment. Please check back later.</p>";
        } else {
          section.innerHTML += "<h4>Available Slots</h4>";
          availableSlots.forEach(c => {
            const teacher = teachers.find(t => t.name === c.teacher);
            section.innerHTML += `
              <div class="conference-slot available">
                <div>
                  <strong>${teacher ? teacher.name : 'Unknown'}</strong><br>
                  <small>${new Date(c.dateTime).toLocaleString()}</small>
                </div>
                <button onclick="bookConference('${c.id}')" style="width: auto; background: #28a745; color: white;">Book</button>
              </div>
            `;
          });
        }
        
        // Show parent's booked conferences
        const myConferences = conferences.filter(c => c.parent === parent.username && c.status === 'booked');
        if(myConferences.length > 0){
          section.innerHTML += "<h4>Your Booked Conferences</h4>";
          myConferences.forEach(c => {
            section.innerHTML += `
              <div class="conference-slot booked">
                <div>
                  <strong>${teachers.find(t => t.name === c.teacher)?.name}</strong><br>
                  <small>${new Date(c.dateTime).toLocaleString()}</small>
                </div>
                <button onclick="cancelConference('${c.id}')" style="width: auto; background: #dc3545; color: white;">Cancel</button>
              </div>
            `;
          });
        }
      }
      content.appendChild(section);
    }
    
    //------ BEHAVIOR REPORTS ------
    if(tab === "Behavior Reports"){
      section.innerHTML = "<h3>📋 Behavioral Records</h3>";
      
      if(role === "teacher"){
        // Teacher adds behavior records with student selection
        const teacher = teachers.find(t => t.name === currentUser);
        
        // Student selection dropdown
        const studentSelect = document.createElement('select');
        studentSelect.id = 'behaviorStudentSelect';
        studentSelect.innerHTML = '<option value="">Select Student</option>';
        students.filter(s => s.section === teacher.sectionHandled).forEach(s => {
          studentSelect.innerHTML += `<option value="${s.name}">${s.name} (${s.id})</option>`;
        });
        section.appendChild(studentSelect);
        
        section.innerHTML += `
          <select id="behaviorType">
            <option value="merit">Merit (+)</option>
            <option value="demerit">Demerit (-)</option>
          </select>
          <input type="number" id="behaviorPoints" placeholder="Points" value="1" min="1" max="10">
          <input type="text" id="behaviorReason" placeholder="Reason/Description">
          <button onclick="addBehaviorRecord()" style="background: #28a745; color: white; margin-top: 10px;">➕ Add Record</button>
        `;
        
        // Show recent records
        section.innerHTML += "<h4>Recent Records</h4>";
        const recentRecords = [];
        students.filter(s => s.section === teacher.sectionHandled).forEach(s => {
          if(s.behaviorLog){
            s.behaviorLog.forEach(log => {
              recentRecords.push({...log, student: s.name});
            });
          }
        });
        
        recentRecords.sort((a,b) => new Date(b.date) - new Date(a.date)).slice(0, 10).forEach(r => {
          section.innerHTML += `
            <div class="behavior-item ${r.type}">
              <div class="behavior-points">${r.type === 'merit' ? '+' : '-'}${r.points}</div>
              <div style="flex: 1;">
                <strong>${r.student}</strong> - ${r.reason}<br>
                <small>${new Date(r.date).toLocaleDateString()}</small>
              </div>
            </div>
          `;
        });
      } else {
        // Student/Parent view
        let targetStudent = currentUser;
        if(role === "parent"){
          const parent = parents.find(p => p.child === currentUser);
          targetStudent = parent.child;
        }
        
        const student = students.find(s => s.name === targetStudent);
        const totalPoints = student.behaviorLog ? student.behaviorLog.reduce((acc, log) => {
          return acc + (log.type === 'merit' ? log.points : -log.points);
        }, 0) : 0;
        
        section.innerHTML += `
          <div class="performance-box" style="background: ${totalPoints >= 0 ? 'linear-gradient(135deg, #28a745 0%, #20c997 100%)' : 'linear-gradient(135deg, #dc3545 0%, #fd7e14 100%)'}">
            <h3>Behavior Points</h3>
            <div class="stat-number">${totalPoints > 0 ? '+' : ''}${totalPoints}</div>
            <p>${totalPoints >= 20 ? '🏆 Excellent Conduct' : totalPoints >= 0 ? '👍 Good Standing' : '⚠️ Needs Improvement'}</p>
          </div>
        `;
        
        if(student.behaviorLog && student.behaviorLog.length > 0){
          section.innerHTML += "<h4>Record History</h4>";
          student.behaviorLog.sort((a,b) => new Date(b.date) - new Date(a.date)).forEach(log => {
            section.innerHTML += `
              <div class="behavior-item ${log.type}">
                <div class="behavior-points">${log.type === 'merit' ? '+' : '-'}${log.points}</div>
                <div style="flex: 1;">
                  <strong>${log.reason}</strong><br>
                  <small>${new Date(log.date).toLocaleDateString()} by ${log.teacher}</small>
                </div>
              </div>
            `;
          });
        }
      }
      content.appendChild(section);
    }
    
    //------ HONOR ROLL ------
    if(tab === "Honor Roll"){
      section.innerHTML = "<h3>🏆 Honor Roll & Academic Excellence</h3>";
      
      // Section selector
      const sectionSelect = document.createElement('select');
      sectionSelect.id = 'honorRollSection';
      sectionSelect.innerHTML = '<option value="all">All Sections</option>';
      const allSections = [...new Set(students.map(s => s.section))];
      allSections.forEach(sec => {
        sectionSelect.innerHTML += `<option value="${sec}">${sec}</option>`;
      });
      sectionSelect.onchange = () => renderHonorRoll(section);
      section.appendChild(sectionSelect);
      
      renderHonorRoll(section);
      
      content.appendChild(section);
    }
    
    //------ QR CODE (ALL STUDENTS) ------
    if(tab === "QR Code" && role === "student"){
      section.innerHTML = "<h3>🔳 Your QR Code</h3>";
      const qrDiv = document.createElement("div");
      qrDiv.id = "qr-code";
      qrDiv.style.textAlign = "center";
      qrDiv.style.padding = "20px";
      section.appendChild(qrDiv);
      
      const qrCodeData = currentUser + "-" + new Date().toISOString().split("T")[0];
      new QRCode(qrDiv, {
        text: qrCodeData,
        width: 200,
        height: 200,
        colorDark: "#003366",
        colorLight: "#ffffff"
      });
      
      section.innerHTML += "<p style='text-align:center;margin-top:15px;'>📱 Scan this QR code for attendance</p>";
      
      // Performance Box
      const perfBox = document.createElement("div");
      perfBox.className = "performance-box";
      const totalDays = Object.keys(attendanceData[currentUser] || {}).length;
      const presentDays = Object.values(attendanceData[currentUser] || {}).filter(function(s){ return s === "P"; }).length;
      const absentDays = totalDays - presentDays;
      const percentage = totalDays > 0 ? Math.round((presentDays / totalDays) * 100) : 0;
      
      perfBox.innerHTML = "<h3>📈 Your Attendance Performance</h3>" +
        "<div class='performance-stats'>" +
        "<div class='stat-item'><div class='stat-number'>" + presentDays + "</div><div class='stat-label'>✅ Present</div></div>" +
        "<div class='stat-item'><div class='stat-number'>" + absentDays + "</div><div class='stat-label'>❌ Absent</div></div>" +
        "<div class='stat-item'><div class='stat-number'>" + percentage + "%</div><div class='stat-label'>📊 Rate</div></div>" +
        "</div>";
      section.appendChild(perfBox);
      
      content.appendChild(section);
    }
    
    //------ MY INFO (ALL PANELS) ------
    if(tab === "My Info"){
      section.innerHTML = "<h3>👤 My Information</h3>";
      
      let user = null;
      
      if(role === "student") {
        user = students.find(function(s){ return s.name === currentUser; });
      } else if(role === "teacher") {
        user = teachers.find(function(t){ return t.name === currentUser; });
      } else if(role === "parent") {
        user = parents.find(function(p){ return p.child === currentUser; });
      } else if(role === "admin") {
        user = adminAccount;
      }
      
      if(user) {
        section.innerHTML += '<div style="text-align:center;margin-bottom:20px;">' +
          '<div class="id-card-photo" style="width:120px;height:120px;margin:0 auto 15px;">' +
          '<img id="profilePic" src="' + (user.pic || '') + '" alt="Profile Photo" onerror="this.src=\'https://via.placeholder.com/120\'">' +
          '</div>' +
          '<input type="file" id="uploadPic" accept="image/*" style="width:auto;">' +
          '<button onclick="uploadProfilePhoto()" style="width:auto;background:linear-gradient(135deg, #8B0000 0%, #003366 100%);color:white;padding:10px 20px;">📷 Upload Photo</button>' +
          '</div>' +
          '<div class="id-card-info">' +
          '<p><strong>Name:</strong> ' + user.name + '</p>' +
          '<p><strong>ID:</strong> ' + (user.id || '-') + '</p>' +
          '<p><strong>Username:</strong> ' + (user.username || '-') + '</p>' +
          '<p><strong>Email:</strong> ' + (user.email || '-') + '</p>' +
          '<p><strong>Section:</strong> ' + (user.section || user.sectionHandled || '-') + '</p>' +
          '<p><strong>Track:</strong> ' + (user.track || '-') + '</p>' +
          '<p><strong>Strand:</strong> ' + (user.strand || '-') + '</p>' +
          '<p><strong>Grade Level:</strong> ' + (user.gradeLevel || '-') + '</p>' +
          '<p><strong>Position:</strong> ' + (user.position || '-') + '</p>' +
          '</div>';
      } else {
        section.innerHTML += "<p>❌ User information not found.</p>";
      }
      
      content.appendChild(section);
    }

    //------ BULLETIN BOARD ------
    if(tab === "Bulletin Board"){
      section.innerHTML = "<h3>📢 Bulletin Board</h3>";
      
      const board = document.createElement("div");
      board.id = "bulletinBoard";
      
      // Display existing announcements
      renderBulletinBoard(board);
      section.appendChild(board);
      
      if(role === "teacher" || role === "admin") {
        // Add new announcement with image upload
        const newMsg = document.createElement("input");
        newMsg.id = "newAnnouncement";
        newMsg.placeholder = "Type new announcement...";
        section.appendChild(newMsg);
        
        // Image upload
        const imgUpload = document.createElement("input");
        imgUpload.type = "file";
        imgUpload.id = "bulletinImage";
        imgUpload.accept = "image/*";
        imgUpload.style.width = "auto";
        imgUpload.style.marginTop = "10px";
        section.appendChild(imgUpload);
        
        // Image preview
        const imgPreview = document.createElement("img");
        imgPreview.id = "bulletinPreview";
        imgPreview.className = "bulletin-upload-preview";
        imgPreview.style.display = "none";
        section.appendChild(imgPreview);
        
        imgUpload.onchange = function() {
          if(this.files && this.files[0]) {
            const reader = new FileReader();
            reader.onload = function(e) {
              imgPreview.src = e.target.result;
              imgPreview.style.display = "block";
            };
            reader.readAsDataURL(this.files[0]);
          }
        };
        
        const addBtn = document.createElement("button");
        addBtn.innerText = "➕ Add Announcement";
        addBtn.style.background = "linear-gradient(135deg, #8B0000 0%, #003366 100%)";
        addBtn.style.color = "white";
        addBtn.style.marginTop = "10px";
        addBtn.onclick = function() {
          const text = newMsg.value.trim();
          const imageFile = imgUpload.files[0];
          
          if(!text && !imageFile) {
            alert("Please enter text or select an image");
            return;
          }
          
          const announcement = {
            type: imageFile ? 'image' : 'text',
            content: text,
            image: null,
            date: new Date().toISOString()
          };
          
          if(imageFile) {
            const reader = new FileReader();
            reader.onload = function(e) {
              announcement.image = e.target.result;
              bulletinBoard.push(announcement);
              saveData();
              
              // Notify all users
              students.forEach(s => addNotification('New announcement posted', 'announcement', s.name));
              teachers.forEach(t => addNotification('New announcement posted', 'announcement', t.name));
              parents.forEach(p => addNotification('New announcement posted', 'announcement', p.username));
              
              renderBulletinBoard(board);
              newMsg.value = "";
              imgUpload.value = "";
              imgPreview.style.display = "none";
            };
            reader.readAsDataURL(imageFile);
          } else {
            bulletinBoard.push(announcement);
            saveData();
            
            // Notify all users
            students.forEach(s => addNotification('New announcement: ' + text.substring(0, 50) + '...', 'announcement', s.name));
            teachers.forEach(t => addNotification('New announcement: ' + text.substring(0, 50) + '...', 'announcement', t.name));
            parents.forEach(p => addNotification('New announcement: ' + text.substring(0, 50) + '...', 'announcement', p.username));
            
            renderBulletinBoard(board);
            newMsg.value = "";
          }
        };
        section.appendChild(addBtn);
      }
      
      content.appendChild(section);
    }
    
    //------ ADMIN FUNCTIONS ------
    if(role === "admin"){
      //------ STUDENT APPROVAL ------
      if(tab === "Student Approval"){
        section.innerHTML = "<h3>✅ Approve Students</h3>";
        const pendingStudents = students.filter(function(s){ return !s.approved; });
        
        if(pendingStudents.length === 0) {
          section.innerHTML += "<p>✅ No pending approvals.</p>";
        } else {
          pendingStudents.forEach(function(s){
            const div = document.createElement("div");
            div.style.padding = "18px";
            div.style.border = "2px solid #ddd";
            div.style.marginBottom = "12px";
            div.style.borderRadius = "10px";
            div.style.background = "#fff3cd";
            div.innerHTML = '<p><strong>👤 Name:</strong> ' + s.name + '</p>' +
              '<p><strong>🆔 ID:</strong> ' + s.id + '</p>' +
              '<p><strong>🏫 Section:</strong> ' + s.section + '</p>' +
              '<button onclick="approveStudent(\'' + s.id + '\')" style="background:#28a745;color:white;width:auto;padding:12px 24px;margin-right:10px;">✅ Approve</button>' +
              '<button onclick="deleteStudent(\'' + s.id + '\')" style="background:#dc3545;color:white;width:auto;padding:12px 24px;">🗑️ Delete</button>';
            section.appendChild(div);
          });
        }
        
        content.appendChild(section);
      }
      
      //------ CREATE TEACHER ------
      if(tab === "Create Teacher"){
        section.innerHTML = "<h3>👨‍🏫 Create Teacher Account</h3>";
        section.innerHTML += '<input type="text" id="newTeacherName" placeholder="👤 Full Name">' +
          '<input type="text" id="newTeacherID" placeholder="🆔 Teacher ID">' +
          '<input type="text" id="newTeacherPosition" placeholder="💼 Position">' +
          '<input type="text" id="newTeacherSection" placeholder="🏫 Section Handled">' +
          '<input type="text" id="newTeacherUsername" placeholder="👤 Username">' +
          '<input type="email" id="newTeacherEmail" placeholder="📧 Email">' +
          '<input type="password" id="newTeacherPassword" placeholder="🔒 Password (min 8 chars)">' +
          '<button onclick="createTeacher()" style="background:linear-gradient(135deg, #8B0000 0%, #003366 100%);color:white;padding:14px;">👨‍🏫 Create Teacher</button>';
        
        content.appendChild(section);
      }
      
      //------ ALL STUDENTS (PRINT ID) ------
      if(tab === "All Students"){
        section.innerHTML = "<h3>👨‍🎓 All Students</h3>";
        
        // Print controls
        const printControls = document.createElement("div");
        printControls.className = "print-controls";
        printControls.innerHTML = '<h4>🖨️ Print ID Cards</h4>' +
          '<p>Check the students you want to print, then click Print Selected.</p>' +
          '<label><input type="checkbox" id="selectAllStudents" onchange="toggleSelectAllStudents()" style="width:auto;"> ✅ Select All</label><br><br>' +
          '<button onclick="printSelectedIDs()" style="background:#28a745;color:white;width:auto;padding:12px 24px;">🖨️ Print Selected</button>';
        section.appendChild(printControls);
        
        // Filter by section
        const filterSection = document.createElement("select");
        filterSection.id = "filterSection";
        filterSection.innerHTML = "<option value='all'>📋 All Sections</option>";
        const sections = [...new Set(students.map(function(s){ return s.section; }))];
        sections.forEach(function(sec){
          const opt = document.createElement("option");
          opt.value = sec;
          opt.innerText = sec;
          filterSection.appendChild(opt);
        });
        section.appendChild(filterSection);
        
        const studentsContainer = document.createElement("div");
        studentsContainer.id = "studentsContainer";
        section.appendChild(studentsContainer);
        
        filterSection.onchange = function(){
          renderStudentsBySection(studentsContainer, this.value);
        };
        
        renderStudentsBySection(studentsContainer, "all");
        
        content.appendChild(section);
      }
      
      //------ PARENT ACCOUNTS ------
      if(tab === "Parent Accounts"){
        section.innerHTML = "<h3>👪 Parent Accounts</h3>";
        section.innerHTML += "<p>📝 These credentials are auto-generated when students sign up. Share with advisers to give to parents.</p>";
        
        // Filter by section
        const filterSection = document.createElement("select");
        filterSection.id = "filterParentSection";
        filterSection.innerHTML = "<option value='all'>📋 All Sections</option>";
        const sections = [...new Set(students.map(function(s){ return s.section; }))];
        sections.forEach(function(sec){
          const opt = document.createElement("option");
          opt.value = sec;
          opt.innerText = sec;
          filterSection.appendChild(opt);
        });
        section.appendChild(filterSection);
        
        const parentContainer = document.createElement("div");
        parentContainer.id = "parentContainer";
        section.appendChild(parentContainer);
        
        filterSection.onchange = function(){
          renderParentAccounts(parentContainer, this.value);
        };
        
        renderParentAccounts(parentContainer, "all");
        
        content.appendChild(section);
      }
      
      //------ MANAGE USERS ------
      if(tab === "Manage Users"){
        section.innerHTML = "<h3>⚙️ Manage Users</h3>";
        
        // Students
        section.innerHTML += "<h4>👨‍🎓 Students</h4>";
        const studentsTable = document.createElement("table");
        studentsTable.innerHTML = "<tr><th>Name</th><th>ID</th><th>Section</th><th>Action</th></tr>";
        students.forEach(function(s){
          const tr = document.createElement("tr");
          tr.innerHTML = '<td>' + s.name + '</td><td>' + s.id + '</td><td>' + s.section + '</td>' +
            '<td><button onclick="deleteStudent(\'' + s.id + '\')" style="background:#dc3545;color:white;width:auto;padding:6px 12px;">🗑️ Delete</button></td>';
          studentsTable.appendChild(tr);
        });
        section.appendChild(studentsTable);
        
        // Teachers
        section.innerHTML += "<h4 style='margin-top:25px;'>👨‍🏫 Teachers</h4>";
        const teachersTable = document.createElement("table");
        teachersTable.innerHTML = "<tr><th>Name</th><th>ID</th><th>Section</th><th>Action</th></tr>";
        teachers.forEach(function(t){
          const tr = document.createElement("tr");
          tr.innerHTML = '<td>' + t.name + '</td><td>' + t.id + '</td><td>' + t.sectionHandled + '</td>' +
            '<td><button onclick="deleteTeacher(\'' + t.id + '\')" style="background:#dc3545;color:white;width:auto;padding:6px 12px;">🗑️ Delete</button></td>';
          teachersTable.appendChild(tr);
        });
        section.appendChild(teachersTable);
        
        content.appendChild(section);
      }
      
      //------ ANALYTICS ------
      if(tab === "Analytics"){
        section.innerHTML = "<h3>📊 School Analytics Dashboard</h3>";
        
        // Stats cards
        section.innerHTML += `
          <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 20px; margin-bottom: 30px;">
            <div class="analytics-card">
              <div class="analytics-number">${students.length}</div>
              <div class="analytics-label">Total Students</div>
            </div>
            <div class="analytics-card">
              <div class="analytics-number">${teachers.length}</div>
              <div class="analytics-label">Teachers</div>
            </div>
            <div class="analytics-card">
              <div class="analytics-number">${parents.length}</div>
              <div class="analytics-label">Parents</div>
            </div>
            <div class="analytics-card">
              <div class="analytics-number">${students.filter(s => s.approved === false).length}</div>
              <div class="analytics-label">Pending Approvals</div>
            </div>
          </div>
        `;
        
        // Enrollment by track
        const trackData = {};
        students.forEach(s => {
          trackData[s.track] = (trackData[s.track] || 0) + 1;
        });
        
        section.innerHTML += `
          <div class="chart-container">
            <h4>Enrollment by Track</h4>
            <div style="margin-top: 20px;">
              ${Object.entries(trackData).map(([track, count]) => `
                <div style="margin: 10px 0;">
                  <div style="display: flex; justify-content: space-between; margin-bottom: 5px;">
                    <span>${track}</span>
                    <span><strong>${count}</strong> students</span>
                  </div>
                  <div style="background: #e9ecef; height: 30px; border-radius: 15px; overflow: hidden;">
                    <div style="background: linear-gradient(90deg, var(--main-red), var(--main-blue)); height: 100%; width: ${(count/students.length*100)}%; display: flex; align-items: center; justify-content: center; color: white; font-size: 12px; font-weight: bold;">
                      ${Math.round(count/students.length*100)}%
                    </div>
                  </div>
                </div>
              `).join('')}
            </div>
          </div>
        `;
        
        content.appendChild(section);
      }
    } // End of admin block
    
    //------ MY PLANNER (TEACHER ONLY) ------
    if(tab === "My Planner" && role === "teacher"){
      section.innerHTML = "<h3>📝 My Planner</h3>";
      
      if(!plannerData[currentUser]) {
        plannerData[currentUser] = { daily: [], weekly: [], monthly: [] };
      }
      
      const plannerContainer = document.createElement("div");
      plannerContainer.className = "planner-container";
      
      // Daily Goals
      const dailySection = document.createElement("div");
      dailySection.className = "planner-section";
      dailySection.innerHTML = "<h4>📅 Daily Goals</h4>";
      plannerData[currentUser].daily.forEach(function(goal, index){
        const item = document.createElement("div");
        item.className = "planner-item" + (goal.completed ? " completed" : "");
        item.innerHTML = '<input type="checkbox"' + (goal.completed ? " checked" : "") + ' onchange="togglePlannerGoal(\'daily\', ' + index + ')">' +
          '<span class="goal-text">' + goal.text + '</span>' +
          '<span class="goal-time">' + (goal.time || "") + '</span>' +
          '<button class="alarm-btn" onclick="setAlarm(\'' + goal.time + '\')">⏰</button>' +
          '<button onclick="deletePlannerGoal(\'daily\', ' + index + ')" style="width:auto;background:#dc3545;color:white;padding:6px 10px;">×</button>';
        dailySection.appendChild(item);
      });
      dailySection.innerHTML += '<input type="text" id="dailyGoalInput" placeholder="Add daily goal..." style="width:55%;">' +
        '<input type="time" id="dailyGoalTime" style="width:auto;">' +
        '<button onclick="addPlannerGoal(\'daily\')" style="width:auto;background:#28a745;color:white;">➕ Add</button>';
      plannerContainer.appendChild(dailySection);
      
      // Weekly Goals
      const weeklySection = document.createElement("div");
      weeklySection.className = "planner-section";
      weeklySection.innerHTML = "<h4>📆 Weekly Goals</h4>";
      plannerData[currentUser].weekly.forEach(function(goal, index){
        const item = document.createElement("div");
        item.className = "planner-item" + (goal.completed ? " completed" : "");
        item.innerHTML = '<input type="checkbox"' + (goal.completed ? " checked" : "") + ' onchange="togglePlannerGoal(\'weekly\', ' + index + ')">' +
          '<span class="goal-text">' + goal.text + '</span>' +
          '<span class="goal-time">' + (goal.time || "") + '</span>' +
          '<button onclick="deletePlannerGoal(\'weekly\', ' + index + ')" style="width:auto;background:#dc3545;color:white;padding:6px 10px;">×</button>';
        weeklySection.appendChild(item);
      });
      weeklySection.innerHTML += '<input type="text" id="weeklyGoalInput" placeholder="Add weekly goal..." style="width:55%;">' +
        '<input type="date" id="weeklyGoalDate" style="width:auto;">' +
        '<button onclick="addPlannerGoal(\'weekly\')" style="width:auto;background:#28a745;color:white;">➕ Add</button>';
      plannerContainer.appendChild(weeklySection);
      
      // Monthly Goals
      const monthlySection = document.createElement("div");
      monthlySection.className = "planner-section";
      monthlySection.innerHTML = "<h4>📆 Monthly Goals</h4>";
      plannerData[currentUser].monthly.forEach(function(goal, index){
        const item = document.createElement("div");
        item.className = "planner-item" + (goal.completed ? " completed" : "");
        item.innerHTML = '<input type="checkbox"' + (goal.completed ? " checked" : "") + ' onchange="togglePlannerGoal(\'monthly\', ' + index + ')">' +
          '<span class="goal-text">' + goal.text + '</span>' +
          '<span class="goal-time">' + (goal.time || "") + '</span>' +
          '<button onclick="deletePlannerGoal(\'monthly\', ' + index + ')" style="width:auto;background:#dc3545;color:white;padding:6px 10px;">×</button>';
        monthlySection.appendChild(item);
      });
      monthlySection.innerHTML += '<input type="text" id="monthlyGoalInput" placeholder="Add monthly goal..." style="width:55%;">' +
        '<input type="date" id="monthlyGoalDate" style="width:auto;">' +
        '<button onclick="addPlannerGoal(\'monthly\')" style="width:auto;background:#28a745;color:white;">➕ Add</button>';
      plannerContainer.appendChild(monthlySection);
      
      section.appendChild(plannerContainer);
      content.appendChild(section);
    }
    
    //------ MY MOOD (AI CHAT FOR STUDENTS) ------
    if(tab === "My Mood" && role === "student"){
      section.innerHTML = "<h3>🤗 My Mood - AI Companion</h3>";
      section.innerHTML += "<p>Share how you're feeling and I'll help you feel better!</p>";
      
      // Mood selector
      const moodDiv = document.createElement("div");
      moodDiv.className = "mood-selector";
      moodDiv.innerHTML = '<button class="mood-btn" onclick="selectMood(\'😊\')">😊</button>' +
        '<button class="mood-btn" onclick="selectMood(\'😢\')">😢</button>' +
        '<button class="mood-btn" onclick="selectMood(\'😠\')">😠</button>' +
        '<button class="mood-btn" onclick="selectMood(\'😰\')">😰</button>' +
        '<button class="mood-btn" onclick="selectMood(\'😴\')">😴</button>' +
        '<button class="mood-btn" onclick="selectMood(\'🤗\')">🤗</button>';
      section.appendChild(moodDiv);
      
      const chatbox = document.createElement("div");
      chatbox.id = "moodChatbox";
      chatbox.className = "chatbox";
      chatbox.style.background = "#f0f8ff";
      chatbox.innerHTML = '<div class="chat-message chat-ai"><strong>🤗 AI Companion:</strong> Hi! How are you feeling today? You can select a mood above or just tell me what\'s on your mind.</div>';
      section.appendChild(chatbox);
      
      const input = document.createElement("input");
      input.type = "text";
      input.id = "moodInput";
      input.placeholder = "Tell me what's on your mind...";
      section.appendChild(input);
      
      const sendBtn = document.createElement("button");
      sendBtn.innerText = "💬 Share";
      sendBtn.style.background = "#6c5ce7";
      sendBtn.style.color = "white";
      sendBtn.onclick = function() { sendMoodMessage(); };
      section.appendChild(sendBtn);
      
      section.innerHTML += '<div style="margin-top:18px;padding:15px;background:#fff3cd;border-radius:8px;font-size:13px;">' +
        '<strong>💡 Tips:</strong> Talk about your feelings! I\'m here to listen and help.' +
        '</div>';
      
      content.appendChild(section);
    }
    
    //------ EMERGENCY NUMBERS ------
    if(tab === "Emergency Numbers"){
      section.innerHTML = "<h3>🚨 Emergency Numbers</h3>";
      const numbers = [
        {name:"🚔 PNP DUMINGAG", num:"099859558677"},
        {name:"🔥 BFP DUMINGAG", num:"09300459871"},
        {name:"🏛️ LGU DUMINGAG", num:"09482121024"},
        {name:"🆘 MDRRMO DUMINGAG", num:"09098046609"}
      ];
      numbers.forEach(function(n){
        const div = document.createElement("div");
        div.style.margin = "12px 0";
        div.innerHTML = '<a href="tel:' + n.num + '" style="display:block;padding:18px;background:linear-gradient(135deg, #f8f9fa 0%, #e9ecef 100%);border-radius:10px;border-left:5px solid #dc3545;text-decoration:none;color:#333;transition:all 0.3s;">' +
          '<strong>' + n.name + '</strong><br>' + n.num + '</a>';
        section.appendChild(div);
      });
      
      content.appendChild(section);
    }
    
    //------ SCHOOL MAP ------
    if(tab === "School Map"){
      section.innerHTML = "<h3>🗺️ School Map</h3>";
      
      const mapContainer = document.createElement("div");
      mapContainer.className = "school-map-container";
      // Hardcoded DSHS map from the provided link
      mapContainer.innerHTML = `
        <h4>🏫 DSHS Campus Layout</h4>
        <img src="https://i.ibb.co/bgY7H7SJ" alt="DSHS School Map" style="max-width:100%;border-radius:10px;box-shadow:0 4px 15px rgba(0,0,0,0.2);" onerror="this.src='https://via.placeholder.com/800x600/003366/FFFFFF?text=DSHS+Campus+Map'">
        <p style="text-align: center; margin-top: 15px; color: #666; font-size: 14px;">
          <em>Official Campus Map of Dumingag Senior High School</em>
        </p>
        <div class="map-legend">
          <div class="map-legend-item">
            <strong>🏫 Main Building</strong>
            <p>Administrative offices and classrooms</p>
          </div>
          <div class="map-legend-item">
            <strong>🔬 Science Lab</strong>
            <p>Laboratory facilities</p>
          </div>
          <div class="map-legend-item">
            <strong>📚 Library</strong>
            <p>Learning resource center</p>
          </div>
          <div class="map-legend-item">
            <strong>🏃 Gymnasium</strong>
            <p>Sports and activities</p>
          </div>
        </div>
      `;
      section.appendChild(mapContainer);
      
      content.appendChild(section);
    }
    
    //------ SETTINGS ------
    if(tab === "Settings"){
      section.innerHTML = "<h3>⚙️ Settings</h3>";
      
      // Appearance
      section.innerHTML += `
        <div class="settings-section">
          <h4>🎨 Appearance</h4>
          <div style="display: flex; justify-content: space-between; align-items: center; margin: 15px 0;">
            <div>
              <strong>Dark Mode</strong>
              <p style="margin: 5px 0; font-size: 13px; color: #666;">Easier on the eyes</p>
            </div>
            <label class="toggle-switch">
              <input type="checkbox" id="toggleDarkMode" onchange="toggleDarkMode()" ${localStorage.getItem('darkMode') === 'true' ? 'checked' : ''}>
              <span class="slider"></span>
            </label>
          </div>
        </div>
      `;
      
      // Notifications
      section.innerHTML += `
        <div class="settings-section">
          <h4>🔔 Notifications</h4>
          <div style="display: flex; justify-content: space-between; align-items: center; margin: 15px 0;">
            <div>
              <strong>Push Notifications</strong>
              <p style="margin: 5px 0; font-size: 13px; color: #666;">Get notified about updates</p>
            </div>
            <label class="toggle-switch">
              <input type="checkbox" id="togglePushNotif" checked>
              <span class="slider"></span>
            </label>
          </div>
          <div style="display: flex; justify-content: space-between; align-items: center; margin: 15px 0;">
            <div>
              <strong>Email Notifications</strong>
              <p style="margin: 5px 0; font-size: 13px; color: #666;">Weekly digest and alerts</p>
            </div>
            <label class="toggle-switch">
              <input type="checkbox" id="toggleEmailNotif" checked>
              <span class="slider"></span>
            </label>
          </div>
        </div>
      `;
      
      // Language
      section.innerHTML += `
        <div class="settings-section">
          <h4>🌐 Language</h4>
          <select id="languageSelect" onchange="changeLanguage()">
            <option value="en">English</option>
            <option value="fil">Filipino</option>
            <option value="ceb">Cebuano</option>
          </select>
        </div>
      `;
      
      content.appendChild(section);
    }
    
    //------ SUBJECT SCHEDULE ------
    if(tab === "Subject Schedule" || tab === "Child Schedule"){
      section.innerHTML = "<h3>📚 Subject Schedule</h3>";
      
      let studentName = currentUser;
      if(role === "parent") {
        const parent = parents.find(function(p){ return p.child === currentUser; });
        if(parent) {
          const child = students.find(function(s){ return s.name === parent.child; });
          if(child) studentName = child.name;
        }
      }
      
      const scheduleBox = document.createElement("div");
      scheduleBox.className = "schedule-box";
      
      subjects.forEach(function(sub){
        const div = document.createElement("div");
        const time = scheduleTimes[sub] || "TBA";
        div.innerHTML = "<strong>" + sub + "</strong><br>" + time;
        scheduleBox.appendChild(div);
      });
      section.appendChild(scheduleBox);
      
      if(role === "teacher" || role === "admin") {
        section.innerHTML += "<h4>Manage Subjects</h4>";
        subjects.forEach(function(sub, index){
          const item = document.createElement("div");
          item.className = "subject-item";
          item.innerHTML = '<input type="text" value="' + sub + '" onchange="updateSubject(' + index + ', this.value)">' +
            '<input type="text" value="' + (scheduleTimes[sub] || '') + '" placeholder="Time" onchange="updateScheduleTime(\'' + sub + '\', this.value)">' +
            '<button onclick="deleteSubject(' + index + ')" style="width:auto;background:#dc3545;color:white;">🗑️ Delete</button>';
          section.appendChild(item);
        });
        
        const addDiv = document.createElement("div");
        addDiv.className = "subject-item";
        addDiv.innerHTML = '<input type="text" id="newSubjectName" placeholder="New Subject">' +
          '<input type="text" id="newSubjectTime" placeholder="Time">' +
          '<button onclick="addSubject()" style="width:auto;background:#28a745;color:white;">➕ Add</button>';
        section.appendChild(addDiv);
      }
      
      content.appendChild(section);
    }

    // Ensure section is appended if not already done
    if(section.parentNode !== content) {
      content.appendChild(section);
    }
  }
  
  //------ RENDER BULLETIN BOARD ------
  function renderBulletinBoard(container) {
    container.innerHTML = "";
    bulletinBoard.forEach(function(item, index){
      const msgDiv = document.createElement("div");
      msgDiv.style.padding = "18px";
      msgDiv.style.borderBottom = "2px solid #ddd";
      msgDiv.style.marginBottom = "12px";
      msgDiv.style.borderRadius = "8px";
      msgDiv.style.background = "#f8f9fa";
      
      if(item.type === 'image' && item.image) {
        msgDiv.innerHTML = '<p>' + (item.content || '') + '</p>' +
          '<img src="' + item.image + '" class="bulletin-image" alt="Announcement Image">';
      } else {
        msgDiv.innerHTML = '<p>' + item.content + '</p>';
      }
      
      msgDiv.innerHTML += '<small style="color: #666; display: block; margin-top: 8px;">' + new Date(item.date).toLocaleString() + '</small>';
      
      if(role === "teacher" || role === "admin") {
        const deleteBtn = document.createElement("button");
        deleteBtn.innerText = "🗑️ Delete";
        deleteBtn.style.width = "auto";
        deleteBtn.style.background = "#dc3545";
        deleteBtn.style.color = "white";
        deleteBtn.style.padding = "8px 15px";
        deleteBtn.style.marginTop = "10px";
        deleteBtn.onclick = function() {
          bulletinBoard.splice(index, 1);
          saveData();
          renderBulletinBoard(container);
        };
        msgDiv.appendChild(deleteBtn);
      }
      
      container.appendChild(msgDiv);
    });
  }
  
  //------ RENDER HONOR ROLL ------
  function renderHonorRoll(container) {
    const sectionFilter = document.getElementById('honorRollSection')?.value || 'all';
    
    // Calculate rankings
    let filteredStudents = students;
    if(sectionFilter !== 'all') {
      filteredStudents = students.filter(s => s.section === sectionFilter);
    }
    
    const rankings = filteredStudents.map(s => {
      const grades = gradesData[s.name] || {};
      const avg = Object.values(grades).reduce((a,b) => a+b, 0) / Object.values(grades).length || 0;
      return {...s, average: avg};
    }).sort((a,b) => b.average - a.average);
    
    // Clear previous honor roll content (keep selector)
    const selector = document.getElementById('honorRollSection');
    container.innerHTML = '<h3>🏆 Honor Roll & Academic Excellence</h3>';
    container.appendChild(selector);
    
    // With Honors (90-94)
    const withHonors = rankings.filter(s => s.average >= 90 && s.average < 95);
    // With High Honors (95-97)
    const withHighHonors = rankings.filter(s => s.average >= 95 && s.average < 98);
    // With Highest Honors (98-100)
    const withHighestHonors = rankings.filter(s => s.average >= 98);
    
    if(withHighestHonors.length > 0){
      container.innerHTML += `
        <div class="honor-roll">
          <h4>🥇 With Highest Honors (98-100)</h4>
          <ul class="honor-roll-list">
            ${withHighestHonors.map(s => `
              <li>
                <span>${s.name} - ${s.average.toFixed(2)}%</span>
                <span class="honor-badge">Highest Honors</span>
              </li>
            `).join('')}
          </ul>
        </div>
      `;
    }
    
    if(withHighHonors.length > 0){
      container.innerHTML += `
        <div class="honor-roll" style="background: linear-gradient(135deg, #c0c0c0 0%, #e8e8e8 100%); border-color: #808080;">
          <h4 style="color: #555;">🥈 With High Honors (95-97)</h4>
          <ul class="honor-roll-list">
            ${withHighHonors.map(s => `
              <li>
                <span>${s.name} - ${s.average.toFixed(2)}%</span>
                <span class="honor-badge" style="background: #808080;">High Honors</span>
              </li>
            `).join('')}
          </ul>
        </div>
      `;
    }
    
    if(withHonors.length > 0){
      container.innerHTML += `
        <div class="honor-roll" style="background: linear-gradient(135deg, #cd7f32 0%, #daa520 100%); border-color: #8b4513;">
          <h4 style="color: #8b4513;">🥉 With Honors (90-94)</h4>
          <ul class="honor-roll-list">
            ${withHonors.map(s => `
              <li>
                <span>${s.name} - ${s.average.toFixed(2)}%</span>
                <span class="honor-badge" style="background: #8b4513;">Honors</span>
              </li>
            `).join('')}
          </ul>
        </div>
      `;
    }
    
    // Student's rank if student view
    if(role === "student"){
      const myRank = rankings.findIndex(s => s.name === currentUser) + 1;
      const myAvg = rankings.find(s => s.name === currentUser)?.average.toFixed(2) || 0;
      container.innerHTML += `
        <div class="section" style="margin-top: 30px; text-align: center;">
          <h4>Your Ranking ${sectionFilter !== 'all' ? '(' + sectionFilter + ')' : '(School-wide)'}</h4>
          <div style="font-size: 48px; font-weight: bold; color: var(--main-blue);">#${myRank}</div>
          <p>out of ${rankings.length} students</p>
          <p><strong>General Average:</strong> ${myAvg}%</p>
        </div>
      `;
    }
  }
  
  //------ RENDER ATTENDANCE CALENDAR ------
  function renderAttendanceCalendar(container, studentName) {
    const existingCalendar = document.getElementById("attendanceCalendar");
    if(existingCalendar) existingCalendar.remove();
    
    const calendar = document.createElement("div");
    calendar.id = "attendanceCalendar";
    calendar.className = "attendance-calendar";
    
    const days = ["Sun", "Mon", "Tue", "Wed", "Thu", "Fri", "Sat"];
    days.forEach(function(day){
      const div = document.createElement("div");
      div.className = "attendance-day header";
      div.innerText = day;
      calendar.appendChild(div);
    });
    
    const dateInput = document.querySelector('input[type="month"]');
    const selectedDate = dateInput ? dateInput.value : new Date().toISOString().split("T")[0].substring(0, 7);
    const yearMonth = selectedDate.split("-");
    const year = yearMonth[0];
    const month = yearMonth[1];
    
    const daysInMonth = new Date(year, month, 0).getDate();
    const firstDay = new Date(year, month - 1, 1).getDay();
    
    for(let i = 0; i < firstDay; i++) {
      const div = document.createElement("div");
      div.className = "attendance-day";
      div.style.background = "#f9f9f9";
      calendar.appendChild(div);
    }
    
    for(let day = 1; day <= daysInMonth; day++) {
      const dateStr = year + "-" + month.padStart(2, '0') + "-" + day.toString().padStart(2, '0');
      const div = document.createElement("div");
      div.className = "attendance-day";
      div.innerText = day;
      
      const status = attendanceData[studentName] ? attendanceData[studentName][dateStr] : null;
      if(status === "P") {
        div.classList.add("present");
        div.innerHTML += " ✅";
      } else if(status === "A") {
        div.classList.add("absent");
        div.innerHTML += " ❌";
      }
      
      div.onclick = function(){
        if(role === "teacher" || role === "admin") {
          const currentStatus = attendanceData[studentName] ? attendanceData[studentName][dateStr] : null;
          if(currentStatus === "P") {
            attendanceData[studentName][dateStr] = "A";
          } else if(currentStatus === "A") {
            delete attendanceData[studentName][dateStr];
          } else {
            attendanceData[studentName][dateStr] = "P";
          }
          saveData();
          renderAttendanceCalendar(container, studentName);
        }
      };
      
      calendar.appendChild(div);
    }
    
    container.appendChild(calendar);
    
    // Legend
    const legend = document.createElement("div");
    legend.className = "attendance-legend";
    legend.innerHTML = "<h4>📋 Legend</h4>" +
      "<p style='color:green;'>✅ Present</p>" +
      "<p style='color:red;'>❌ Absent</p>" +
      "<p style='font-size:12px;color:#666;margin-top:10px;'>" + ((role === "teacher" || role === "admin") ? "👆 Click to toggle" : "👁️ View only") + "</p>";
    container.appendChild(legend);
  }

  //------ RENDER GRADES TABLE ------
  function renderGradesTable(container, studentName) {
    const existingTable = document.getElementById("gradesTable");
    if(existingTable) existingTable.remove();
    
    const showHistory = document.getElementById('showHistory')?.checked;
    
    const table = document.createElement("table");
    table.id = "gradesTable";
    
    if(showHistory){
      // Show grade history
      table.innerHTML = "<tr><th>📚 Subject</th><th>Q1</th><th>Q2</th><th>Q3</th><th>Q4</th><th>📊 Current</th></tr>";
      
      subjects.forEach(function(sub){
        const tr = document.createElement("tr");
        const history = gradeHistory[studentName]?.[sub] || [];
        const currentGrade = gradesData[studentName]?.[sub] || 0;
        
        const q1 = history.find(h => h.quarter === 1)?.grade || '-';
        const q2 = history.find(h => h.quarter === 2)?.grade || '-';
        const q3 = history.find(h => h.quarter === 3)?.grade || '-';
        const q4 = history.find(h => h.quarter === 4)?.grade || '-';
        
        tr.innerHTML = "<td>" + sub + "</td><td>" + q1 + "</td><td>" + q2 + "</td><td>" + q3 + "</td><td>" + q4 + "</td><td><strong>" + currentGrade + "</strong></td>";
        table.appendChild(tr);
      });
    } else {
      // Show current grades with editing
      table.innerHTML = "<tr><th>📚 Subject</th><th>📊 Grade</th><th>📈 Status</th></tr>";
      
      let total = 0;
      let count = 0;
      
      subjects.forEach(function(sub){
        const tr = document.createElement("tr");
        const grade = gradesData[studentName] ? gradesData[studentName][sub] : 0;
        total += grade;
        count++;
        
        let status = "✅ Passing";
        if(grade < 75) status = "⚠️ Needs Improvement";
        
        const editable = (role === "teacher" || role === "admin") ? "contenteditable" : "";
        const onblur = (role === "teacher" || role === "admin") ? " onblur=\"updateGrade('" + studentName + "', '" + sub + "', this.innerText)\"" : "";
        
        tr.innerHTML = "<td>" + sub + "</td><td " + editable + onblur + ">" + grade + "</td><td style='color:" + (grade >= 75 ? 'green' : 'red') + "'>" + status + "</td>";
        table.appendChild(tr);
      });
      
      const avgRow = document.createElement("tr");
      avgRow.innerHTML = "<td><strong>📉 Average</strong></td><td><strong>" + (count > 0 ? (total / count).toFixed(2) : 0) + "</strong></td><td></td>";
      table.appendChild(avgRow);
    }
    
    container.appendChild(table);
  }
  
  function toggleGradeHistory(){
    const studentSelect = document.getElementById('gradesStudentSelect');
    const studentName = studentSelect ? studentSelect.value : currentUser;
    renderGradesTable(document.querySelector('.section'), studentName);
  }

  //------ RENDER STUDENTS BY SECTION (WITH CHECKBOXES) ------
  function renderStudentsBySection(container, sectionFilter) {
    container.innerHTML = "";
    
    let filteredStudents = students;
    if(sectionFilter !== "all") {
      filteredStudents = students.filter(function(s){ return s.section === sectionFilter; });
    }
    
    filteredStudents.forEach(function(s){
      const idCard = document.createElement("div");
      idCard.className = "id-card";
      idCard.innerHTML = '<input type="checkbox" class="id-card-checkbox" data-student-id="' + s.id + '">' +
        '<div class="id-card-header"><h3 style="margin:0;">🎓 DUMINGAG SENIOR HIGH SCHOOL</h3><p style="margin:0;font-size:11px;">Student ID Card</p></div>' +
        '<div class="id-card-photo"><img src="' + (s.pic || 'https://via.placeholder.com/80') + '" alt="Photo" onerror="this.src=\'https://via.placeholder.com/80\'"></div>' +
        '<div class="id-card-info"><p><strong>Name:</strong> ' + s.name + '</p><p><strong>ID No:</strong> ' + s.id + '</p>' +
        '<p><strong>Section:</strong> ' + s.section + '</p><p><strong>Track:</strong> ' + s.track + '</p>' +
        '<p><strong>Strand:</strong> ' + s.strand + '</p><p><strong>Grade:</strong> ' + s.gradeLevel + '</p></div>' +
        '<div class="id-card-footer">🎓 Dumingag Senior High School - Dumingag, Zamboanga del Sur</div>' +
        '<div id="qr-' + s.id + '" style="text-align:center;margin-top:12px;"></div>';
      
      container.appendChild(idCard);
      
      setTimeout(function(){
        new QRCode(document.getElementById("qr-" + s.id), {
          text: s.name + "-" + s.id,
          width: 65,
          height: 65
        });
      }, 100);
    });
  }

  //------ RENDER PARENT ACCOUNTS ------
  function renderParentAccounts(container, sectionFilter) {
    container.innerHTML = "";
    
    let parentAccounts = JSON.parse(localStorage.getItem('parentAccounts')) || [];
    
    let filteredAccounts = parentAccounts;
    if(sectionFilter !== "all") {
      filteredAccounts = parentAccounts.filter(function(p){ return p.section === sectionFilter; });
    }
    
    if(filteredAccounts.length === 0) {
      container.innerHTML = "<p>❌ No parent accounts found.</p>";
      return;
    }
    
    filteredAccounts.forEach(function(p){
      const card = document.createElement("div");
      card.className = "parent-account-card";
      card.innerHTML = '<h4>👤 ' + p.studentName + '</h4>' +
        '<p><strong>🆔 Student ID:</strong> ' + p.studentId + '</p>' +
        '<p><strong>🏫 Section:</strong> ' + p.section + '</p>' +
        '<div class="credentials">' +
        '<p><strong>👨‍💼 Parent Username:</strong> ' + p.parentUsername + '</p>' +
        '<p><strong>🔑 Parent Password:</strong> ' + p.parentPassword + '</p>' +
        '</div>';
      container.appendChild(card);
    });
  }

  //------ TOGGLE SELECT ALL STUDENTS ------
  function toggleSelectAllStudents() {
    const selectAll = document.getElementById("selectAllStudents");
    const checkboxes = document.querySelectorAll(".id-card-checkbox");
    checkboxes.forEach(function(cb){
      cb.checked = selectAll.checked;
    });
  }

  //------ PRINT SELECTED IDS ------
  function printSelectedIDs() {
    const checkboxes = document.querySelectorAll(".id-card-checkbox:checked");
    if(checkboxes.length === 0) {
      alert("⚠️ Please select at least one student to print!");
      return;
    }
    window.print();
  }

  //------ APPROVE STUDENT ------
  function approveStudent(id) {
    const student = students.find(function(s){ return s.id === id; });
    if(student) {
      student.approved = true;
      saveData();
      
      // Notify student and parent
      addNotification('Your account has been approved! You can now login.', 'approval', student.name);
      const parent = parents.find(p => p.child === student.name);
      if(parent){
        addNotification('Your child ' + student.name + '\'s account has been approved.', 'approval', parent.username);
      }
      
      alert("✅ " + student.name + " has been approved!");
      loadSection("Student Approval");
    }
  }

  //------ DELETE STUDENT ------
  function deleteStudent(id) {
    const student = students.find(function(s){ return s.id === id; });
    if(student) {
      if(confirm("⚠️ Are you sure you want to delete " + student.name + "?")) {
        students = students.filter(function(s){ return s.id !== id; });
        delete gradesData[student.name];
        delete attendanceData[student.name];
        delete gradeHistory[student.name];
        saveData();
        alert("🗑️ " + student.name + " has been deleted.");
        loadSection("Student Approval");
      }
    }
  }

  //------ DELETE TEACHER ------
  function deleteTeacher(id) {
    const teacher = teachers.find(function(t){ return t.id === id; });
    if(teacher) {
      if(confirm("⚠️ Are you sure you want to delete " + teacher.name + "?")) {
        teachers = teachers.filter(function(t){ return t.id !== id; });
        saveData();
        alert("🗑️ " + teacher.name + " has been deleted.");
        loadSection("Manage Users");
      }
    }
  }

  //------ CREATE TEACHER ------
  function createTeacher() {
    const name = document.getElementById("newTeacherName").value.trim();
    const id = document.getElementById("newTeacherID").value.trim();
    const position = document.getElementById("newTeacherPosition").value.trim();
    const section = document.getElementById("newTeacherSection").value.trim();
    const username = document.getElementById("newTeacherUsername").value.trim();
    const email = document.getElementById("newTeacherEmail").value.trim();
    const password = document.getElementById("newTeacherPassword").value.trim();
    
    if(!name || !username || !password || !section || !email) {
      alert("⚠️ Please fill in all required fields!");
      return;
    }
    
    // Validate email
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!emailRegex.test(email)) {
      alert("Please enter a valid email address");
      return;
    }
    
    // Check password strength
    if (password.length < 8) {
      alert("Password must be at least 8 characters");
      return;
    }
    
    if(teachers.find(function(t){ return t.username === username; })) {
      alert("❌ Username already exists!");
      return;
    }
    
    teachers.push({
      name: name,
      id: id,
      position: position,
      sectionHandled: section,
      username: username,
      email: email,
      password: simpleHash(password)
    });
    
    saveData();
    alert("✅ Teacher account created successfully!");
    
    document.getElementById("newTeacherName").value = "";
    document.getElementById("newTeacherID").value = "";
    document.getElementById("newTeacherPosition").value = "";
    document.getElementById("newTeacherSection").value = "";
    document.getElementById("newTeacherUsername").value = "";
    document.getElementById("newTeacherEmail").value = "";
    document.getElementById("newTeacherPassword").value = "";
  }
  
  //------ UPDATE SUBJECT ------
  function updateSubject(index, newValue) {
    const oldValue = subjects[index];
    subjects[index] = newValue;
    
    if(scheduleTimes[oldValue]) {
      scheduleTimes[newValue] = scheduleTimes[oldValue];
      delete scheduleTimes[oldValue];
    }
    
    students.forEach(function(s){
      if(gradesData[s.name] && gradesData[s.name][oldValue] !== undefined) {
        gradesData[s.name][newValue] = gradesData[s.name][oldValue];
        delete gradesData[s.name][oldValue];
      }
    });
    
    saveData();
  }

  //------ UPDATE SCHEDULE TIME ------
  function updateScheduleTime(subject, time) {
    scheduleTimes[subject] = time;
    saveData();
  }

  //------ DELETE SUBJECT ------
  function deleteSubject(index) {
    const subject = subjects[index];
    subjects.splice(index, 1);
    delete scheduleTimes[subject];
    
    students.forEach(function(s){
      if(gradesData[s.name]) {
        delete gradesData[s.name][subject];
      }
    });
    
    saveData();
    loadSection("Subject Schedule");
  }

  //------ ADD SUBJECT ------
  function addSubject() {
    const name = document.getElementById("newSubjectName").value.trim();
    const time = document.getElementById("newSubjectTime").value.trim();
    
    if(!name) {
      alert("⚠️ Please enter a subject name!");
      return;
    }
    
    if(subjects.includes(name)) {
      alert("❌ Subject already exists!");
      return;
    }
    
    subjects.push(name);
    scheduleTimes[name] = time || "TBA";
    
    students.forEach(function(s){
      if(!gradesData[s.name]) gradesData[s.name] = {};
      gradesData[s.name][name] = 0;
    });
    
    saveData();
    loadSection("Subject Schedule");
  }

  //------ UPDATE GRADE ------
  function updateGrade(student, subject, newGrade) {
    const grade = parseInt(newGrade);
    if(isNaN(grade) || grade < 0 || grade > 100) {
      alert("⚠️ Grade must be between 0 and 100.");
      loadSection("Grades");
      return;
    }
    
    // Save to history before updating
    if(!gradeHistory[student]) gradeHistory[student] = {};
    if(!gradeHistory[student][subject]) gradeHistory[student][subject] = [];
    
    const currentQuarter = 1; // Should be determined by date
    gradeHistory[student][subject].push({
      quarter: currentQuarter,
      grade: grade,
      date: new Date().toISOString(),
      updatedBy: currentUser
    });
    
    if(!gradesData[student]) gradesData[student] = {};
    gradesData[student][subject] = grade;
    saveData();
    
    // Notify student and parent
    addNotification(`Your grade in ${subject} has been updated to ${grade}`, 'grade', student);
    const parent = parents.find(p => p.child === student);
    if(parent){
      addNotification(`${student}'s grade in ${subject} is now ${grade}`, 'grade', parent.username);
    }
  }

  //------ UPLOAD PROFILE PHOTO ------
  function uploadProfilePhoto() {
    const fileInput = document.getElementById("uploadPic");
    if(!fileInput.files || !fileInput.files[0]) {
      alert("⚠️ Please select a photo first!");
      return;
    }
    
    const reader = new FileReader();
    reader.onload = function(e){
      const photoData = e.target.result;
      
      if(role === "student") {
        const user = students.find(function(s){ return s.name === currentUser; });
        if(user) {
          user.pic = photoData;
          saveData();
          document.getElementById("profilePic").src = photoData;
          alert("✅ Photo uploaded successfully!");
        }
      } else if(role === "teacher") {
        const user = teachers.find(function(t){ return t.name === currentUser; });
        if(user) {
          user.pic = photoData;
          saveData();
          document.getElementById("profilePic").src = photoData;
          alert("✅ Photo uploaded successfully!");
        }
      } else if(role === "parent") {
        const user = parents.find(function(p){ return p.child === currentUser; });
        if(user) {
          user.pic = photoData;
          saveData();
          document.getElementById("profilePic").src = photoData;
          alert("✅ Photo uploaded successfully!");
        }
      } else if(role === "admin") {
        adminAccount.pic = photoData;
        saveData();
        document.getElementById("profilePic").src = photoData;
        alert("✅ Photo uploaded successfully!");
      }
    };
    reader.readAsDataURL(fileInput.files[0]);
  }

  //------ MESSAGING FUNCTIONS ------
  let currentContact = null;
  
  function selectContact(contact, element) {
    // Update active state
    document.querySelectorAll('.contact-item').forEach(el => el.classList.remove('active'));
    element.classList.add('active');
    
    currentContact = contact;
    
    // Update chat header
    document.getElementById('chatHeader').innerHTML = `
      <div style="display: flex; align-items: center; gap: 10px;">
        <div class="contact-avatar" style="width: 35px; height: 35px; font-size: 18px;">${contact.avatar}</div>
        <div>${contact.name}</div>
      </div>
    `;
    
    // Enable inputs
    document.getElementById('messageInput').disabled = false;
    document.getElementById('sendBtn').disabled = false;
    
    // Load messages
    loadMessages(contact.username);
  }
  
  function loadMessages(contactUsername) {
    const chatMessages = document.getElementById('chatMessages');
    chatMessages.innerHTML = '';
    
    const conversation = messages.filter(m => 
      (m.sender === currentUser && m.recipient === contactUsername) ||
      (m.sender === contactUsername && m.recipient === currentUser)
    ).sort((a,b) => new Date(a.date) - new Date(b.date));
    
    if(conversation.length === 0){
      chatMessages.innerHTML = '<p style="text-align: center; color: #999; margin-top: 50px;">No messages yet. Start the conversation!</p>';
    } else {
      conversation.forEach(m => {
        const isSent = m.sender === currentUser;
        const msgDiv = document.createElement('div');
        msgDiv.className = 'message ' + (isSent ? 'sent' : 'received');
        msgDiv.innerHTML = `
          <div>${m.text}</div>
          <div class="message-time">${new Date(m.date).toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'})}</div>
        `;
        chatMessages.appendChild(msgDiv);
      });
    }
    
    chatMessages.scrollTop = chatMessages.scrollHeight;
  }
  
  function sendMessage() {
    const input = document.getElementById('messageInput');
    const text = input.value.trim();
    
    if(!text || !currentContact) return;
    
    const message = {
      id: Date.now(),
      sender: currentUser,
      recipient: currentContact.username,
      text: text,
      date: new Date().toISOString(),
      read: false
    };
    
    messages.push(message);
    saveData();
    
    input.value = '';
    loadMessages(currentContact.username);
    
    // Add notification for recipient
    addNotification(`New message from ${currentUser}`, 'message', currentContact.username);
  }
  
  // Allow Enter key to send
  document.addEventListener('keypress', function(e) {
    if(e.key === 'Enter' && document.getElementById('messageInput') === document.activeElement) {
      sendMessage();
    }
  });

  //------ PLANNER FUNCTIONS ------
  function addPlannerGoal(type) {
    const input = document.getElementById(type + "GoalInput");
    const timeInput = document.getElementById(type + "GoalTime") || document.getElementById(type + "GoalDate");
    
    if(!input.value.trim()) {
      alert("⚠️ Please enter a goal!");
      return;
    }
    
    if(!plannerData[currentUser]) {
      plannerData[currentUser] = { daily: [], weekly: [], monthly: [] };
    }
    
    plannerData[currentUser][type].push({
      text: input.value.trim(),
      time: timeInput ? timeInput.value : "",
      completed: false
    });
    
    saveData();
    loadSection("My Planner");
  }

  function togglePlannerGoal(type, index) {
    plannerData[currentUser][type][index].completed = !plannerData[currentUser][type][index].completed;
    saveData();
    loadSection("My Planner");
  }

  function deletePlannerGoal(type, index) {
    plannerData[currentUser][type].splice(index, 1);
    saveData();
    loadSection("My Planner");
  }

  function setAlarm(time) {
    if(!time) {
      alert("⏰ No time set for this goal!");
      return;
    }
    alert("🔔 Alarm set for " + time + "\n\nNote: Browser notifications must be enabled for actual alerts.");
  }

  //------ MY MOOD AI CHAT FUNCTIONS ------
  let selectedMood = "";

  function selectMood(mood) {
    selectedMood = mood;
    
    // Update UI
    const buttons = document.querySelectorAll(".mood-btn");
    buttons.forEach(function(btn) {
      if(btn.innerText === mood) {
        btn.classList.add("selected");
      } else {
        btn.classList.remove("selected");
      }
    });
    
    // Send mood to chat
    const chatbox = document.getElementById("moodChatbox");
    chatbox.innerHTML += '<div class="chat-message chat-user"><strong>👤 You:</strong> ' + mood + '</div>';
    chatbox.scrollTop = chatbox.scrollHeight;
    
    // Get AI response based on mood
    getAIResponse("I feel " + mood);
  }

  function sendMoodMessage() {
    const input = document.getElementById("moodInput");
    const message = input.value.trim();
    
    if(!message && !selectedMood) {
      alert("⚠️ Please select a mood or type a message!");
      return;
    }
    
    const chatbox = document.getElementById("moodChatbox");
    
    if(message) {
      chatbox.innerHTML += '<div class="chat-message chat-user"><strong>👤 You:</strong> ' + message + '</div>';
      chatbox.scrollTop = chatbox.scrollHeight;
      input.value = "";
      
      // Send to AI
      getAIResponse(message);
    }
  }

  function getAIResponse(message) {
    const chatbox = document.getElementById("moodChatbox");
    
    // Show typing indicator
    chatbox.innerHTML += '<div class="chat-message chat-ai" id="typingIndicator"><em>💭 Thinking...</em></div>';
    chatbox.scrollTop = chatbox.scrollHeight;
    
    // Simulated AI responses (rule-based - no API needed)
    setTimeout(function() {
      const typingIndicator = document.getElementById("typingIndicator");
      if(typingIndicator) typingIndicator.remove();
      
      const response = getSupportiveResponse(message);
      chatbox.innerHTML += '<div class="chat-message chat-ai"><strong>🤗 AI Companion:</strong> ' + response + '</div>';
      chatbox.scrollTop = chatMessages.scrollHeight;
    }, 1000);
  }

  function getSupportiveResponse(message) {
    const lowerMessage = message.toLowerCase();
    
    // Happy/Good moods
    if(lowerMessage.includes("😊") || lowerMessage.includes("happy") || lowerMessage.includes("good") || lowerMessage.includes("great") || lowerMessage.includes("excited") || lowerMessage.includes("love")) {
      return "That's wonderful! 🎉 I'm so glad you're feeling good! Keep that positive energy going! Remember to share your happiness with others too!";
    }
    
    // Sad/Down moods
    if(lowerMessage.includes("😢") || lowerMessage.includes("sad") || lowerMessage.includes("depressed") || lowerMessage.includes("unhappy") || lowerMessage.includes("down") || lowerMessage.includes("crying") || lowerMessage.includes("hurt")) {
      return "I'm here for you. 💙 It's okay to feel sad sometimes. Would you like to talk about what's making you feel this way? Remember, you're not alone. Consider talking to a trusted teacher or counselor.";
    }
    
    // Angry/Frustrated moods
    if(lowerMessage.includes("😠") || lowerMessage.includes("angry") || lowerMessage.includes("mad") || lowerMessage.includes("frustrated") || lowerMessage.includes("annoyed") || lowerMessage.includes("hate")) {
      return "I understand you're feeling angry. 😤 Take a deep breath... Inhale for 4 seconds, hold for 7, exhale for 8. Would you like to talk about what's frustrating you?";
    }
    
    // Anxious/Worried/Nervous
    if(lowerMessage.includes("😰") || lowerMessage.includes("anxious") || lowerMessage.includes("nervous") || lowerMessage.includes("worried") || lowerMessage.includes("stressed") || lowerMessage.includes("scared") || lowerMessage.includes("fear")) {
      return "It's okay to feel anxious sometimes. 🌸 Try these tips:\n\n1. Take deep breaths\n2. Write down your worries\n3. Talk to someone you trust\n\nRemember, one step at a time. You can get through this!";
    }
    
    // Tired/Sleepy
    if(lowerMessage.includes("😴") || lowerMessage.includes("tired") || lowerMessage.includes("sleepy") || lowerMessage.includes("exhausted") || lowerMessage.includes("fatigued") || lowerMessage.includes("sleep")) {
      return "You sound tired! 😴 Make sure you're getting enough sleep - teens need 8-10 hours. Take breaks when studying, stay hydrated, and don't forget to rest. Your health comes first!";
    }
    
    // Love/Grateful
    if(lowerMessage.includes("🤗") || lowerMessage.includes("love") || lowerMessage.includes("grateful") || lowerMessage.includes("thankful") || lowerMessage.includes("blessed")) {
      return "That's a beautiful feeling! 💕 Gratitude is so important for mental health. Consider writing down 3 things you're grateful for each day. You're amazing!";
    }
    
    // Default supportive responses
    const responses = [
      "Thank you for sharing that with me. 💙 I'm here to listen. Do you want to talk more about it?",
      "I hear you. 🌸 Remember, it's okay to not be okay. Would you like some suggestions on how to cope?",
      "Thanks for opening up to me. 💜 You're very brave. Is there anything specific you'd like to talk about?",
      "I appreciate you sharing your feelings. 💙 Would you like me to share some helpful tips?",
      "Your feelings are valid. 🌟 You're doing great by talking about them. Keep going!"
    ];
    
    return responses[Math.floor(Math.random() * responses.length)];
  }

  //------ SEMESTER AND QUARTER SELECTORS ------
  function selectSemester(sem, btn) {
    // Update button styles
    const buttons = document.querySelectorAll(".sem-btn");
    buttons.forEach(function(b) {
      b.classList.remove("active");
    });
    btn.classList.add("active");
    
    // Update quarter selector based on semester
    const quarterSelector = document.getElementById("quarterSelector");
    if(quarterSelector) {
      if(sem === 1) {
        quarterSelector.innerHTML = `
          <button class="quarter-btn active" onclick="selectQuarter(1, this)">1st Quarter</button>
          <button class="quarter-btn" onclick="selectQuarter(2, this)">2nd Quarter</button>
        `;
      } else {
        quarterSelector.innerHTML = `
          <button class="quarter-btn active" onclick="selectQuarter(3, this)">3rd Quarter</button>
          <button class="quarter-btn" onclick="selectQuarter(4, this)">4th Quarter</button>
        `;
      }
    }
  }

  function selectQuarter(quarter, btn) {
    // Update button styles
    const buttons = document.querySelectorAll(".quarter-btn");
    buttons.forEach(function(b) {
      b.classList.remove("active");
    });
    btn.classList.add("active");
    
    // Here you would typically load grades for the selected quarter
    console.log("Selected Quarter: " + quarter);
  }

  //------ ADDITIONAL FUNCTIONS FOR NEW FEATURES ------
  
  // Report Card Generation (DepEd Style)
  function generateReportCards(section) {
    if(!section) {
      alert('Please select a section first');
      return;
    }
    
    const container = document.getElementById('reportCardsContainer');
    container.innerHTML = '';
    
    const sectionStudents = students.filter(s => s.section === section);
    
    sectionStudents.forEach(s => {
      const reportCard = document.createElement('div');
      reportCard.className = 'report-card';
      reportCard.style.marginBottom = '30px';
      
      // Calculate general average
      const grades = Object.values(gradesData[s.name] || {});
      const genAvg = grades.length ? (grades.reduce((a,b) => a+b, 0) / grades.length).toFixed(2) : 0;
      
      reportCard.innerHTML = `
        <div class="report-card-header">
          <h2>Republic of the Philippines</h2>
          <h3>Department of Education</h3>
          <p>Region IX, Zamboanga Peninsula</p>
          <p style="font-size: 14px; margin-top: 10px;">DUMINGAG SENIOR HIGH SCHOOL</p>
          <p style="font-size: 12px;">Dumingag, Zamboanga del Sur</p>
        </div>
        
        <div class="report-card-info">
          <p><strong>Name:</strong> ${s.name}</p>
          <p><strong>LRN:</strong> ${s.id.replace('ST', '')}</p>
          <p><strong>Grade & Section:</strong> ${s.gradeLevel} - ${s.section}</p>
          <p><strong>Track/Strand:</strong> ${s.track} - ${s.strand}</p>
          <p><strong>School Year:</strong> 2023-2024</p>
          <p><strong>Semester:</strong> 1st Semester</p>
        </div>
        
        <table>
          <tr>
            <th rowspan="2">Learning Areas</th>
            <th colspan="2">Quarter</th>
            <th rowspan="2">Final<br>Grade</th>
            <th rowspan="2">Remarks</th>
          </tr>
          <tr>
            <th>1</th>
            <th>2</th>
          </tr>
          ${subjects.map(sub => {
            const grade = gradesData[s.name]?.[sub] || 0;
            const q1 = Math.floor(Math.random() * 10) + 80; // Simulated Q1
            const final = Math.round((q1 + grade) / 2);
            return `
              <tr>
                <td style="text-align: left;">${sub}</td>
                <td>${q1}</td>
                <td>${grade}</td>
                <td><strong>${final}</strong></td>
                <td>${final >= 75 ? 'Passed' : 'Failed'}</td>
              </tr>
            `;
          }).join('')}
          <tr style="background: #f0f0f0; font-weight: bold;">
            <td colspan="3" style="text-align: right;">General Average:</td>
            <td>${genAvg}</td>
            <td>${genAvg >= 75 ? 'Passed' : 'Failed'}</td>
          </tr>
        </table>
        
        <div class="descriptor-row">
          <p style="margin: 5px 0;"><strong>Descriptors:</strong> AO = Always Outstanding (90-100) | SO = Sometimes Outstanding (85-89) | RO = Rarely Outstanding (80-84) | NO = Not Outstanding (Below 80)</p>
        </div>
        
        <div class="report-card-footer">
          <div class="signature-line">
            <strong>${teachers.find(t => t.sectionHandled === s.section)?.name || 'Class Adviser'}</strong><br>
            <small>Class Adviser</small>
          </div>
          <div class="signature-line">
            <strong>School Principal</strong><br>
            <small>School Head</small>
          </div>
        </div>
        
        <div style="text-align: center; margin-top: 20px; font-size: 11px; color: #666;">
          Not valid without school seal
        </div>
      `;
      
      container.appendChild(reportCard);
    });
    
    // Add print button
    const printBtn = document.createElement('button');
    printBtn.innerHTML = '🖨️ Print All Report Cards';
    printBtn.style.cssText = 'background: #28a745; color: white; margin-top: 20px;';
    printBtn.onclick = () => window.print();
    container.appendChild(printBtn);
  }
  
  // Assignment Functions
  function showCreateAssignment() {
    const form = document.getElementById('createAssignmentForm');
    form.style.display = form.style.display === 'none' ? 'block' : 'none';
  }
  
  function createAssignment() {
    const title = document.getElementById('assignmentTitle').value;
    const desc = document.getElementById('assignmentDesc').value;
    const subject = document.getElementById('assignmentSubject').value;
    const due = document.getElementById('assignmentDue').value;
    const points = document.getElementById('assignmentPoints').value;
    
    if(!title || !due) {
      alert('Please fill in all required fields');
      return;
    }
    
    const teacher = teachers.find(t => t.name === currentUser);
    
    assignments.push({
      id: Date.now().toString(),
      title,
      description: desc,
      subject,
      dueDate: due,
      points: parseInt(points),
      teacher: currentUser,
      section: teacher.sectionHandled,
      createdAt: new Date().toISOString()
    });
    
    saveData();
    
    // Notify students
    students.filter(s => s.section === teacher.sectionHandled).forEach(s => {
      addNotification(`New assignment: ${title} in ${subject}`, 'assignment', s.name);
    });
    
    alert('✅ Assignment created successfully!');
    loadSection('Assignments');
  }
  
  function viewSubmissions(assignmentId) {
    const assignment = assignments.find(a => a.id === assignmentId);
    const subs = submissions.filter(s => s.assignmentId === assignmentId);
    
    let html = `<h4>Submissions for: ${assignment.title}</h4>`;
    
    if(subs.length === 0) {
      html += '<p>No submissions yet.</p>';
    } else {
      subs.forEach(sub => {
        const student = students.find(s => s.name === sub.student);
        html += `
          <div style="padding: 15px; border: 1px solid #ddd; margin: 10px 0; border-radius: 8px;">
            <p><strong>${sub.student}</strong> - Submitted: ${new Date(sub.submittedAt).toLocaleString()}</p>
            ${sub.fileName ? `<p>📎 ${sub.fileName}</p>` : ''}
            ${sub.graded ? 
              `<p><strong>Grade: ${sub.grade}/${assignment.points}</strong></p>` :
              `<div style="margin-top: 10px;">
                <input type="number" id="grade_${sub.id}" placeholder="Grade" max="${assignment.points}" style="width: 100px;">
                <button onclick="gradeSubmission('${sub.id}', ${assignment.points})" style="width: auto; background: #28a745; color: white;">✓ Grade</button>
              </div>`
            }
          </div>
        `;
      });
    }
    
    // Show in modal-like alert (in real app, use proper modal)
    const div = document.createElement('div');
    div.innerHTML = html;
    div.style.cssText = 'max-height: 400px; overflow-y: auto;';
    alert(div.innerText); // Simplified - in real app show proper UI
    loadSection('Assignments');
  }
  
  function gradeSubmission(submissionId, maxPoints) {
    const grade = document.getElementById(`grade_${submissionId}`).value;
    if(!grade || grade < 0 || grade > maxPoints) {
      alert('Invalid grade');
      return;
    }
    
    const sub = submissions.find(s => s.id === submissionId);
    sub.grade = parseInt(grade);
    sub.graded = true;
    sub.gradedBy = currentUser;
    sub.gradedAt = new Date().toISOString();
    
    saveData();
    
    addNotification(`Your submission has been graded: ${grade}/${maxPoints}`, 'grade', sub.student);
    alert('✅ Graded successfully!');
    loadSection('Assignments');
  }
  
  function submitAssignment(assignmentId) {
    const input = document.createElement('input');
    input.type = 'file';
    input.onchange = function() {
      const file = this.files[0];
      if(file) {
        submissions.push({
          id: Date.now().toString(),
          assignmentId,
          student: currentUser,
          fileName: file.name,
          submittedAt: new Date().toISOString(),
          graded: false
        });
        saveData();
        alert('✅ Assignment submitted successfully!');
        loadSection('Assignments');
      }
    };
    input.click();
  }
  
  // Quiz Functions
  let currentQuiz = null;
  let quizTimer = null;
  
  function showCreateQuiz() {
    const form = document.getElementById('createQuizForm');
    form.style.display = form.style.display === 'none' ? 'block' : 'none';
  }
  
  let questionCount = 0;
  function addQuestion() {
    questionCount++;
    const container = document.getElementById('quizQuestions');
    const div = document.createElement('div');
    div.style.cssText = 'background: white; padding: 15px; margin: 10px 0; border-radius: 8px;';
    div.innerHTML = `
      <h5>Question ${questionCount}</h5>
      <input type="text" placeholder="Question text" class="q-text" style="margin-bottom: 10px;">
      <input type="text" placeholder="Option A" class="q-opt">
      <input type="text" placeholder="Option B" class="q-opt">
      <input type="text" placeholder="Option C" class="q-opt">
      <input type="text" placeholder="Option D" class="q-opt">
      <select class="q-answer" style="margin-top: 10px;">
        <option value="">Correct Answer</option>
        <option value="0">A</option>
        <option value="1">B</option>
        <option value="2">C</option>
        <option value="3">D</option>
      </select>
    `;
    container.appendChild(div);
  }
  
  function saveQuiz() {
    const title = document.getElementById('quizTitle').value;
    const subject = document.getElementById('quizSubject').value;
    const duration = document.getElementById('quizDuration').value;
    const start = document.getElementById('quizStart').value;
    const end = document.getElementById('quizEnd').value;
    
    const questions = [];
    document.querySelectorAll('#quizQuestions > div').forEach(div => {
      const text = div.querySelector('.q-text').value;
      const options = Array.from(div.querySelectorAll('.q-opt')).map(i => i.value);
      const answer = div.querySelector('.q-answer').value;
      if(text && options.every(o => o) && answer !== '') {
        questions.push({text, options, answer: parseInt(answer)});
      }
    });
    
    if(!title || questions.length === 0) {
      alert('Please fill in all fields and add at least one question');
      return;
    }
    
    const teacher = teachers.find(t => t.name === currentUser);
    
    quizzes.push({
      id: Date.now().toString(),
      title,
      subject,
      duration: parseInt(duration),
      startTime: start,
      endTime: end,
      questions,
      teacher: currentUser,
      section: teacher.sectionHandled
    });
    
    saveData();
    alert('✅ Quiz created successfully!');
    loadSection('Quizzes');
  }
  
  function startQuiz(quizId) {
    const quiz = quizzes.find(q => q.id === quizId);
    currentQuiz = quiz;
    
    // Show quiz interface
    const section = document.querySelector('.section');
    section.innerHTML = `
      <div class="quiz-timer" id="quizTimer">⏱️ ${quiz.duration}:00</div>
      <h3>${quiz.title}</h3>
      <p><strong>Subject:</strong> ${quiz.subject} | <strong>Questions:</strong> ${quiz.questions.length}</p>
      <div id="quizContainer"></div>
      <button onclick="submitQuiz()" style="background: #28a745; color: white; margin-top: 20px;">✅ Submit Quiz</button>
    `;
    
    // Render questions
    const container = document.getElementById('quizContainer');
    quiz.questions.forEach((q, idx) => {
      const qDiv = document.createElement('div');
      qDiv.className = 'quiz-question';
      qDiv.innerHTML = `
        <h4>Question ${idx + 1}</h4>
        <p>${q.text}</p>
        <div class="quiz-options">
          ${q.options.map((opt, optIdx) => `
            <div class="quiz-option" onclick="selectOption(${idx}, ${optIdx})" data-q="${idx}" data-opt="${optIdx}">
              ${String.fromCharCode(65 + optIdx)}. ${opt}
            </div>
          `).join('')}
        </div>
      `;
      container.appendChild(qDiv);
    });
    
    // Start timer
    let timeLeft = quiz.duration * 60;
    quizTimer = setInterval(() => {
      timeLeft--;
      const mins = Math.floor(timeLeft / 60);
      const secs = timeLeft % 60;
      document.getElementById('quizTimer').textContent = `⏱️ ${mins}:${secs.toString().padStart(2, '0')}`;
      
      if(timeLeft <= 0) {
        clearInterval(quizTimer);
        submitQuiz();
      }
    }, 1000);
  }
  
  let selectedAnswers = {};
  
  function selectOption(questionIdx, optionIdx) {
    selectedAnswers[questionIdx] = optionIdx;
    document.querySelectorAll(`[data-q="${questionIdx}"]`).forEach(el => el.classList.remove('selected'));
    document.querySelector(`[data-q="${questionIdx}"][data-opt="${optionIdx}"]`).classList.add('selected');
  }
  
  function submitQuiz() {
    clearInterval(quizTimer);
    document.getElementById('quizTimer')?.remove();
    
    let correct = 0;
    currentQuiz.questions.forEach((q, idx) => {
      if(selectedAnswers[idx] === q.answer) correct++;
    });
    
    const score = Math.round((correct / currentQuiz.questions.length) * 100);
    
    quizResults.push({
      quizId: currentQuiz.id,
      student: currentUser,
      score,
      answers: selectedAnswers,
      submittedAt: new Date().toISOString()
    });
    
    saveData();
    
    alert(`✅ Quiz submitted!\n\nScore: ${score}%\nCorrect: ${correct}/${currentQuiz.questions.length}`);
    loadSection('Quizzes');
  }
  
  // Conference Functions
  function generateSlots() {
    const date = document.getElementById('conferenceDate').value;
    const start = document.getElementById('conferenceStart').value;
    const end = document.getElementById('conferenceEnd').value;
    const duration = parseInt(document.getElementById('slotDuration').value);
    
    if(!date || !start || !end) {
      alert('Please fill in all fields');
      return;
    }
    
    const startTime = new Date(`${date}T${start}`);
    const endTime = new Date(`${date}T${end}`);
    
    let current = startTime;
    while(current < endTime) {
      conferences.push({
        id: Date.now().toString() + Math.random(),
        teacher: currentUser,
        dateTime: current.toISOString(),
        status: 'available',
        parent: null
      });
      current = new Date(current.getTime() + duration * 60000);
    }
    
    saveData();
    alert('✅ Slots generated successfully!');
    loadSection('Conference Schedule');
  }
  
  function bookConference(conferenceId) {
    const parent = parents.find(p => p.child === currentUser);
    const conf = conferences.find(c => c.id === conferenceId);
    
    conf.status = 'booked';
    conf.parent = parent.username;
    
    saveData();
    
    addNotification(`Conference booked with ${teachers.find(t => t.name === conf.teacher)?.name}`, 'conference', parent.username);
    addNotification(`Parent ${parent.name} booked a conference`, 'conference', conf.teacher);
    
    alert('✅ Conference booked successfully!');
    loadSection('Conference Booking');
  }
  
  function cancelConference(conferenceId) {
    if(!confirm('Are you sure you want to cancel this conference?')) return;
    
    const conf = conferences.find(c => c.id === conferenceId);
    conf.status = 'available';
    conf.parent = null;
    
    saveData();
    alert('✅ Conference cancelled');
    loadSection('Conference Booking');
  }
  
  // Behavior Functions
  function addBehaviorRecord() {
    const studentName = document.getElementById('behaviorStudentSelect').value;
    const type = document.getElementById('behaviorType').value;
    const points = parseInt(document.getElementById('behaviorPoints').value);
    const reason = document.getElementById('behaviorReason').value;
    
    if(!studentName) {
      alert('Please select a student');
      return;
    }
    
    if(!reason) {
      alert('Please enter a reason');
      return;
    }
    
    const student = students.find(s => s.name === studentName);
    if(!student.behaviorLog) student.behaviorLog = [];
    
    student.behaviorLog.push({
      type,
      points,
      reason,
      teacher: currentUser,
      date: new Date().toISOString()
    });
    
    saveData();
    
    addNotification(`New behavior record: ${type} ${points} points for ${reason}`, 'behavior', studentName);
    
    const parent = parents.find(p => p.child === studentName);
    if(parent) {
      addNotification(`Your child received a ${type} (${points} points): ${reason}`, 'behavior', parent.username);
    }
    
    alert('✅ Record added successfully!');
    loadSection('Behavior Reports');
  }
  
  // Course Material Functions
  function uploadMaterial() {
    const title = document.getElementById('materialTitle').value;
    const desc = document.getElementById('materialDesc').value;
    const subject = document.getElementById('materialSubject').value;
    const section = document.getElementById('materialSection').value;
    const fileInput = document.getElementById('materialFile');
    
    if(!title || !fileInput.files[0]) {
      alert('Please fill in title and select a file');
      return;
    }
    
    const reader = new FileReader();
    reader.onload = function(e) {
      courseMaterials.push({
        id: Date.now().toString(),
        title,
        description: desc,
        subject,
        section,
        fileUrl: e.target.result,
        uploadedBy: currentUser,
        uploadedAt: new Date().toISOString()
      });
      
      saveData();
      
      // Notify students
      if(section === 'all') {
        students.forEach(s => addNotification(`New material uploaded: ${title}`, 'material', s.name));
      } else {
        students.filter(s => s.section === section).forEach(s => {
          addNotification(`New material uploaded: ${title}`, 'material', s.name);
        });
      }
      
      alert('✅ Material uploaded successfully!');
      loadSection('Course Materials');
    };
    reader.readAsDataURL(fileInput.files[0]);
  }
  
  function deleteMaterial(materialId) {
    if(!confirm('Are you sure you want to delete this material?')) return;
    
    courseMaterials = courseMaterials.filter(m => m.id !== materialId);
    saveData();
    loadSection('Course Materials');
  }
  
  // Settings Functions
  function toggleDarkMode() {
    document.body.classList.toggle('dark-mode');
    localStorage.setItem('darkMode', document.body.classList.contains('dark-mode'));
  }
  
  function changeLanguage() {
    const lang = document.getElementById('languageSelect').value;
    localStorage.setItem('language', lang);
    alert('🌐 Language changed to ' + (lang === 'en' ? 'English' : lang === 'fil' ? 'Filipino' : 'Cebuano') + '\n\nNote: Full localization would require translation files.');
  }
  
  // PWA Support
  let deferredPrompt;
  
  window.addEventListener('beforeinstallprompt', (e) => {
    deferredPrompt = e;
    document.getElementById('installPwaBtn').classList.add('show');
  });
  
  function installPWA() {
    if(deferredPrompt) {
      deferredPrompt.prompt();
      deferredPrompt.userChoice.then((choiceResult) => {
        if(choiceResult.outcome === 'accepted') {
          console.log('User accepted PWA install');
        }
        deferredPrompt = null;
        document.getElementById('installPwaBtn').classList.remove('show');
      });
    }
  }
  
  // Check for saved dark mode preference
  if(localStorage.getItem('darkMode') === 'true') {
    document.body.classList.add('dark-mode');
  }

  //------ INITIALIZE ON PAGE LOAD ------
  window.onload = function() {
    saveData();
    
    // Check for saved session
    const savedRole = localStorage.getItem('currentRole');
    const savedUser = localStorage.getItem('currentUser');
    if(savedRole && savedUser) {
      // Could implement "Remember me" functionality here
    }
  };
</script>
</body>
</html>
