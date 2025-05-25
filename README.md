<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>Catatan Pertemuan Kolaboratif</title>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600;700&display=swap" rel="stylesheet">
  <style>
    :root {
      --bg: #ffffff;
      --text: #333;
      --border: #e0e0e0;
      --primary: #bdbdbd;
      --online-bg: #90caf9;
      --offline-bg: #a5d6a7;
      --asinkronus-bg: #ffcc80;
      --putih-abu-bg: #f5f5f5;
      --uts-bg: #fff176;
      --libur-bg: #ef5350;
      --uas-bg: #b71c1c;
      --connected: #4CAF50;
      --disconnected: #f44336;
    }
    * {
      box-sizing: border-box;
    }
    body {
      font-family: 'Poppins', sans-serif;
      background: var(--bg);
      color: var(--text);
      margin: 0;
      padding: 1rem;
    }
    
    .header {
      display: flex;
      justify-content: space-between;
      align-items: center;
      margin-bottom: 1rem;
      flex-wrap: wrap;
      gap: 1rem;
    }
    
    .connection-status {
      display: flex;
      align-items: center;
      gap: 0.5rem;
      padding: 0.5rem 1rem;
      border-radius: 0.5rem;
      font-weight: 600;
      font-size: 0.875rem;
    }
    
    .status-online {
      background: var(--connected);
      color: white;
    }
    
    .status-offline {
      background: var(--disconnected);
      color: white;
    }
    
    .status-dot {
      width: 8px;
      height: 8px;
      border-radius: 50%;
      background: currentColor;
      animation: pulse 2s infinite;
    }
    
    @keyframes pulse {
      0% { opacity: 1; }
      50% { opacity: 0.5; }
      100% { opacity: 1; }
    }
    
    .users-online {
      display: flex;
      align-items: center;
      gap: 0.5rem;
      font-size: 0.875rem;
      color: #666;
    }
    
    .user-avatar {
      width: 24px;
      height: 24px;
      border-radius: 50%;
      background: linear-gradient(45deg, #667eea, #764ba2);
      display: flex;
      align-items: center;
      justify-content: center;
      color: white;
      font-size: 0.75rem;
      font-weight: bold;
    }
    
    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 1rem;
      overflow-x: auto;
      box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
      border-radius: 8px;
      font-size: 0.8rem;
    }
    
    .table-container {
      overflow-x: auto;
      position: relative;
    }
    
    th, td {
      border: 1px solid var(--border);
      padding: 0.6rem;
      text-align: center;
      vertical-align: top;
      font-size: 0.8rem;
      min-width: 80px;
      position: relative;
    }
    
    th:first-child, td:first-child {
      text-align: left;
      min-width: 200px;
      font-size: 0.85rem;
    }
    
    th {
      background: var(--primary);
      font-weight: 600;
    }
    
    .Online, .Praktisi { background-color: var(--online-bg); }
    .Offline { background-color: var(--offline-bg); }
    .Asinkronus, .Penugasan, .Elearning { background-color: var(--asinkronus-bg); }
    .none { font-style: italic; font-weight: normal; color: #757575; background-color: var(--putih-abu-bg);}
    .UTS { background-color: var(--uts-bg); }
    .Libur { background-color: var(--libur-bg); color: white; }
    .UAS { background-color: var(--uas-bg); color: white; }
    .Kosong { background-color: white; }
    .Tetap { color: #0d47a1; font-weight: bold; }
    .Dipindahkan { color: #ef6c00; font-weight: bold; }
    .Dibatalkan { color: #b71c1c; font-weight: bold; }
    .TildeStatus { color: #9e9e9e; font-weight: normal; }
    .NoInfo { color: #757575; font-style: italic; font-weight: normal; }
    
    .editor {
      display: none;
      flex-direction: column;
      gap: 0.5rem;
      background: #ffffff;
      border: 1px solid var(--border);
      padding: 1rem;
      position: absolute;
      z-index: 10;
      border-radius: 0.5rem;
      box-shadow: 0 4px 12px rgba(0,0,0,0.1);
      min-width: 250px;
    }
    
    .editor input,
    .editor select,
    .editor button {
      padding: 0.5rem;
      font-size: 0.875rem;
      width: 100%;
      border: 1px solid #ccc;
      border-radius: 0.25rem;
      font-family: 'Poppins', sans-serif;
    }
    
    .editor button {
      background: #3a86ff;
      color: white;
      font-weight: bold;
      border: none;
      cursor: pointer;
    }
    
    .editor button:hover {
      background: #2f70d9;
    }
    
    .editable {
      cursor: pointer;
      transition: all 0.2s;
      position: relative;
    }
    
    .editable:hover {
      opacity: 0.8;
      transform: scale(1.02);
    }
    
    .editable.editing {
      outline: 2px solid #3a86ff;
      outline-offset: -2px;
    }
    
    .cell-editor-indicator {
      position: absolute;
      top: 2px;
      right: 2px;
      width: 8px;
      height: 8px;
      border-radius: 50%;
      background: #ff9800;
      display: none;
    }
    
    .editable.being-edited .cell-editor-indicator {
      display: block;
    }
    
    .recent-changes {
      position: fixed;
      bottom: 20px;
      right: 20px;
      background: white;
      border: 1px solid var(--border);
      border-radius: 0.5rem;
      padding: 1rem;
      box-shadow: 0 4px 12px rgba(0,0,0,0.1);
      max-width: 300px;
      max-height: 200px;
      overflow-y: auto;
      font-size: 0.75rem;
      display: none;
    }
    
    .recent-changes h4 {
      margin: 0 0 0.5rem 0;
      font-size: 0.875rem;
    }
    
    .change-item {
      padding: 0.25rem 0;
      border-bottom: 1px solid #eee;
    }
    
    .change-item:last-child {
      border-bottom: none;
    }
    
    .sync-indicator {
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      background: rgba(0,0,0,0.8);
      color: white;
      padding: 1rem 2rem;
      border-radius: 0.5rem;
      display: none;
      z-index: 1000;
    }
    
    .sync-spinner {
      display: inline-block;
      width: 16px;
      height: 16px;
      border: 2px solid rgba(255,255,255,0.3);
      border-top: 2px solid white;
      border-radius: 50%;
      animation: spin 1s linear infinite;
      margin-right: 0.5rem;
    }
    
    @keyframes spin {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
    }
    
    @media (max-width: 768px) {
      .header {
        flex-direction: column;
        align-items: stretch;
      }
      
      .connection-status {
        justify-content: center;
      }
      
      .editor {
        width: 90%;
        left: 5% !important;
        top: 20% !important;
        position: fixed !important;
      }
      
      .recent-changes {
        bottom: 10px;
        right: 10px;
        left: 10px;
        max-width: none;
      }
    }
  </style>
</head>
<body>
  <div class="header">
    <h2>Catatan Pertemuan Kolaboratif</h2>
    <div style="display: flex; align-items: center; gap: 1rem; flex-wrap: wrap;">
      <div id="connectionStatus" class="connection-status status-online">
        <div class="status-dot"></div>
        <span>Terhubung</span>
      </div>
      <div class="users-online">
        <span>Pengguna aktif:</span>
        <div id="activeUsers">
          <div class="user-avatar">A</div>
          <div class="user-avatar">B</div>
          <div class="user-avatar">C</div>
        </div>
      </div>
    </div>
  </div>

  <div class="table-container">
    <table id="mainTable">
    <thead>
      <tr>
        <th>Mata Kuliah</th>
        <th colspan="16">Pertemuan</th>
      </tr>
      <tr>
        <th></th>
        <th>1</th><th>2</th><th>3</th><th>4</th><th>5</th>
        <th>6</th><th>7</th><th>8</th><th>9</th><th>10</th>
        <th>11</th><th>12</th><th>13</th><th>14</th><th>15</th><th>16</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Agama Hindu</td>
        <td class="editable" data-row="0" data-col="0"></td><td class="editable" data-row="0" data-col="1"></td>
        <td class="editable" data-row="0" data-col="2"></td><td class="editable" data-row="0" data-col="3"></td>
        <td class="editable" data-row="0" data-col="4"></td><td class="editable" data-row="0" data-col="5"></td>
        <td class="editable" data-row="0" data-col="6"></td><td class="editable" data-row="0" data-col="7"></td>
        <td class="editable" data-row="0" data-col="8"></td><td class="editable" data-row="0" data-col="9"></td>
        <td class="editable" data-row="0" data-col="10"></td><td class="editable" data-row="0" data-col="11"></td>
        <td class="editable" data-row="0" data-col="12"></td><td class="editable" data-row="0" data-col="13"></td>
        <td class="editable" data-row="0" data-col="14"></td><td class="editable" data-row="0" data-col="15"></td>
      </tr>
      <tr>
        <td>Bahasa Indonesia</td>
        <td class="editable" data-row="1" data-col="0"></td><td class="editable" data-row="1" data-col="1"></td>
        <td class="editable" data-row="1" data-col="2"></td><td class="editable" data-row="1" data-col="3"></td>
        <td class="editable" data-row="1" data-col="4"></td><td class="editable" data-row="1" data-col="5"></td>
        <td class="editable" data-row="1" data-col="6"></td><td class="editable" data-row="1" data-col="7"></td>
        <td class="editable" data-row="1" data-col="8"></td><td class="editable" data-row="1" data-col="9"></td>
        <td class="editable" data-row="1" data-col="10"></td><td class="editable" data-row="1" data-col="11"></td>
        <td class="editable" data-row="1" data-col="12"></td><td class="editable" data-row="1" data-col="13"></td>
        <td class="editable" data-row="1" data-col="14"></td><td class="editable" data-row="1" data-col="15"></td>
      </tr>
      <tr>
        <td>Didaktik Metodik Pembelajaran Informatika</td>
        <td class="editable" data-row="2" data-col="0"></td><td class="editable" data-row="2" data-col="1"></td>
        <td class="editable" data-row="2" data-col="2"></td><td class="editable" data-row="2" data-col="3"></td>
        <td class="editable" data-row="2" data-col="4"></td><td class="editable" data-row="2" data-col="5"></td>
        <td class="editable" data-row="2" data-col="6"></td><td class="editable" data-row="2" data-col="7"></td>
        <td class="editable" data-row="2" data-col="8"></td><td class="editable" data-row="2" data-col="9"></td>
        <td class="editable" data-row="2" data-col="10"></td><td class="editable" data-row="2" data-col="11"></td>
        <td class="editable" data-row="2" data-col="12"></td><td class="editable" data-row="2" data-col="13"></td>
        <td class="editable" data-row="2" data-col="14"></td><td class="editable" data-row="2" data-col="15"></td>
      </tr>
      <tr>
        <td>Asesmen dan Evaluasi Pembelajaran Teknik Informatika</td>
        <td class="editable" data-row="3" data-col="0"></td><td class="editable" data-row="3" data-col="1"></td>
        <td class="editable" data-row="3" data-col="2"></td><td class="editable" data-row="3" data-col="3"></td>
        <td class="editable" data-row="3" data-col="4"></td><td class="editable" data-row="3" data-col="5"></td>
        <td class="editable" data-row="3" data-col="6"></td><td class="editable" data-row="3" data-col="7"></td>
        <td class="editable" data-row="3" data-col="8"></td><td class="editable" data-row="3" data-col="9"></td>
        <td class="editable" data-row="3" data-col="10"></td><td class="editable" data-row="3" data-col="11"></td>
        <td class="editable" data-row="3" data-col="12"></td><td class="editable" data-row="3" data-col="13"></td>
        <td class="editable" data-row="3" data-col="14"></td><td class="editable" data-row="3" data-col="15"></td>
      </tr>
      <tr>
        <td>Sistem Informasi</td>
        <td class="editable" data-row="4" data-col="0"></td><td class="editable" data-row="4" data-col="1"></td>
        <td class="editable" data-row="4" data-col="2"></td><td class="editable" data-row="4" data-col="3"></td>
        <td class="editable" data-row="4" data-col="4"></td><td class="editable" data-row="4" data-col="5"></td>
        <td class="editable" data-row="4" data-col="6"></td><td class="editable" data-row="4" data-col="7"></td>
        <td class="editable" data-row="4" data-col="8"></td><td class="editable" data-row="4" data-col="9"></td>
        <td class="editable" data-row="4" data-col="10"></td><td class="editable" data-row="4" data-col="11"></td>
        <td class="editable" data-row="4" data-col="12"></td><td class="editable" data-row="4" data-col="13"></td>
        <td class="editable" data-row="4" data-col="14"></td><td class="editable" data-row="4" data-col="15"></td>
      </tr>
      <tr>
        <td>Algoritma dan Pemrograman</td>
        <td class="editable" data-row="5" data-col="0"></td><td class="editable" data-row="5" data-col="1"></td>
        <td class="editable" data-row="5" data-col="2"></td><td class="editable" data-row="5" data-col="3"></td>
        <td class="editable" data-row="5" data-col="4"></td><td class="editable" data-row="5" data-col="5"></td>
        <td class="editable" data-row="5" data-col="6"></td><td class="editable" data-row="5" data-col="7"></td>
        <td class="editable" data-row="5" data-col="8"></td><td class="editable" data-row="5" data-col="9"></td>
        <td class="editable" data-row="5" data-col="10"></td><td class="editable" data-row="5" data-col="11"></td>
        <td class="editable" data-row="5" data-col="12"></td><td class="editable" data-row="5" data-col="13"></td>
        <td class="editable" data-row="5" data-col="14"></td><td class="editable" data-row="5" data-col="15"></td>
      </tr>
      <tr>
        <td>Belajar dan Pembelajaran</td>
        <td class="editable" data-row="6" data-col="0"></td><td class="editable" data-row="6" data-col="1"></td>
        <td class="editable" data-row="6" data-col="2"></td><td class="editable" data-row="6" data-col="3"></td>
        <td class="editable" data-row="6" data-col="4"></td><td class="editable" data-row="6" data-col="5"></td>
        <td class="editable" data-row="6" data-col="6"></td><td class="editable" data-row="6" data-col="7"></td>
        <td class="editable" data-row="6" data-col="8"></td><td class="editable" data-row="6" data-col="9"></td>
        <td class="editable" data-row="6" data-col="10"></td><td class="editable" data-row="6" data-col="11"></td>
        <td class="editable" data-row="6" data-col="12"></td><td class="editable" data-row="6" data-col="13"></td>
        <td class="editable" data-row="6" data-col="14"></td><td class="editable" data-row="6" data-col="15"></td>
      </tr>
      <tr>
        <td>Statistika Dasar</td>
        <td class="editable" data-row="7" data-col="0"></td><td class="editable" data-row="7" data-col="1"></td>
        <td class="editable" data-row="7" data-col="2"></td><td class="editable" data-row="7" data-col="3"></td>
        <td class="editable" data-row="7" data-col="4"></td><td class="editable" data-row="7" data-col="5"></td>
        <td class="editable" data-row="7" data-col="6"></td><td class="editable" data-row="7" data-col="7"></td>
        <td class="editable" data-row="7" data-col="8"></td><td class="editable" data-row="7" data-col="9"></td>
        <td class="editable" data-row="7" data-col="10"></td><td class="editable" data-row="7" data-col="11"></td>
        <td class="editable" data-row="7" data-col="12"></td><td class="editable" data-row="7" data-col="13"></td>
        <td class="editable" data-row="7" data-col="14"></td><td class="editable" data-row="7" data-col="15"></td>
      </tr>
      <tr>
        <td>UI/UX</td>
        <td class="editable" data-row="8" data-col="0"></td><td class="editable" data-row="8" data-col="1"></td>
        <td class="editable" data-row="8" data-col="2"></td><td class="editable" data-row="8" data-col="3"></td>
        <td class="editable" data-row="8" data-col="4"></td><td class="editable" data-row="8" data-col="5"></td>
        <td class="editable" data-row="8" data-col="6"></td><td class="editable" data-row="8" data-col="7"></td>
        <td class="editable" data-row="8" data-col="8"></td><td class="editable" data-row="8" data-col="9"></td>
        <td class="editable" data-row="8" data-col="10"></td><td class="editable" data-row="8" data-col="11"></td>
        <td class="editable" data-row="8" data-col="12"></td><td class="editable" data-row="8" data-col="13"></td>
        <td class="editable" data-row="8" data-col="14"></td><td class="editable" data-row="8" data-col="15"></td>
      </tr>
      <tr>
        <td>Basis Data</td>
        <td class="editable" data-row="9" data-col="0"></td><td class="editable" data-row="9" data-col="1"></td>
        <td class="editable" data-row="9" data-col="2"></td><td class="editable" data-row="9" data-col="3"></td>
        <td class="editable" data-row="9" data-col="4"></td><td class="editable" data-row="9" data-col="5"></td>
        <td class="editable" data-row="9" data-col="6"></td><td class="editable" data-row="9" data-col="7"></td>
        <td class="editable" data-row="9" data-col="8"></td><td class="editable" data-row="9" data-col="9"></td>
        <td class="editable" data-row="9" data-col="10"></td><td class="editable" data-row="9" data-col="11"></td>
        <td class="editable" data-row="9" data-col="12"></td><td class="editable" data-row="9" data-col="13"></td>
        <td class="editable" data-row="9" data-col="14"></td><td class="editable" data-row="9" data-col="15"></td>
      </tr>
    </tbody>
    </table>
  </div>

  <div id="editor" class="editor">
    <label>Jenis:
      <select id="jenis">
        <option value="Kosong">Kosong</option>
        <option value="none">none</option>
        <option value="Online">Online</option>
        <option value="Praktisi">Praktisi</option>
        <option value="Offline">Offline</option>
        <option value="Asinkronus">Asinkronus</option>
        <option value="Penugasan">Penugasan</option>
        <option value="Elearning">E-learning</option>
        <option value="UTS">UTS</option>
        <option value="Libur">Libur</option>
        <option value="UAS">UAS</option>
      </select>
    </label>
    <label>Status:
      <select id="status">
        <option value="Tetap">Tetap</option>
        <option value="Dipindahkan">Dipindahkan</option>
        <option value="Dibatalkan">Dibatalkan</option>
        <option value="~">~</option>
        <option value="NoInfo">No Info</option>
      </select>
    </label>
    <label>Tanggal:
      <input type="date" id="tanggal">
    </label>
    <label>Jam:
      <input type="time" id="jam">
    </label>
    <button onclick="applyEdit()">Simpan</button>
    <button onclick="closeEditor()" style="background: #666; margin-top: 0.25rem;">Batal</button>
  </div>

  <div id="recentChanges" class="recent-changes">
    <h4>Perubahan Terbaru</h4>
    <div id="changesList"></div>
  </div>

  <div id="syncIndicator" class="sync-indicator">
    <div class="sync-spinner"></div>
    Menyinkronkan...
  </div>

  <script>
    const editor = document.getElementById('editor');
    const connectionStatus = document.getElementById('connectionStatus');
    const recentChanges = document.getElementById('recentChanges');
    const changesList = document.getElementById('changesList');
    const syncIndicator = document.getElementById('syncIndicator');
    
    let currentCell = null;
    let tableData = {};
    let isOnline = true;
    let editingCells = new Set();
    let recentChangesList = [];
    let simulatedUsers = ['Anna', 'Budi', 'Citra', 'Dani', 'Eka'];
    let currentUser = simulatedUsers[Math.floor(Math.random() * simulatedUsers.length)];

    // Initialize collaborative features
    window.addEventListener('load', () => {
      initializeCollaborativeFeatures();
      initializeEventListeners();
      simulateNetworkStatus();
      simulateCollaborativeEditing();
    });

    function initializeCollaborativeFeatures() {
      // Simulate real-time collaboration
      setInterval(syncData, 3000); // Sync every 3 seconds
      setInterval(updateActiveUsers, 5000); // Update users every 5 seconds
      setInterval(simulateRemoteChanges, 8000); // Simulate remote changes every 8 seconds
      
      // Show recent changes panel
      setTimeout(() => {
        recentChanges.style.display = 'block';
      }, 2000);
    }

    function initializeEventListeners() {
      document.querySelectorAll('.editable').forEach(cell => {
        cell.addEventListener('click', openEditor);
        
        // Add cell editor indicator
        const indicator = document.createElement('div');
        indicator.className = 'cell-editor-indicator';
        cell.appendChild(indicator);
      });

      // Close editor when clicking outside
      document.addEventListener('click', (e) => {
        if (!editor.contains(e.target) && !e.target.classList.contains('editable')) {
          closeEditor();
        }
      });

      // Close editor on Escape key
      document.addEventListener('keydown', (e) => {
        if (e.key === 'Escape') {
          closeEditor();
        }
      });
    }

    function openEditor(event) {
      const cell = event.target.closest('.editable');
      if (!cell) return;
      
      currentCell = cell;
      const rect = cell.getBoundingClientRect();
      
      // Better positioning logic
      let left = rect.left + window.scrollX;
      let top = rect.bottom + window.scrollY + 5;
      
      // Adjust if editor would go offscreen
      const editorWidth = 250;
      const editorHeight = 300;
      
      if (left + editorWidth > window.innerWidth) {
        left = window.innerWidth - editorWidth - 20;
      }
      
      if (top + editorHeight > window.innerHeight + window.scrollY) {
        top = rect.top + window.scrollY - editorHeight - 5;
      }
      
      editor.style.left = `${Math.max(10, left)}px`;
      editor.style.top = `${Math.max(10, top)}px`;
      editor.style.display = 'flex';
      
      // Mark cell as being edited
      cell.classList.add('editing', 'being-edited');
      editingCells.add(cell);
      
      fillEditorFormFromCell(cell);
    }

    function fillEditorFormFromCell(cell) {
      const jenisSelect = document.getElementById('jenis');
      const statusSelect = document.getElementById('status');
      const tanggalInput = document.getElementById('tanggal');
      const jamInput = document.getElementById('jam');

      // Reset form
