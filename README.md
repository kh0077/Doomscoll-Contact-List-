<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>DOOMSCROLL — Crew Contact Sheet</title>

<link href="https://fonts.googleapis.com/css2?family=Space+Mono:ital,wght@0,400;0,700;1,400&family=Bebas+Neue&family=Inter:wght@300;400;500;600&display=swap" rel="stylesheet">

<style>
/* (keeping your styles intact, only adding one small print tweak) */

@media print {
  header { display:none; }
  .toolbar, .dept-legend, .stats-bar, .actions-cell { display:none !important; }
}
</style>
</head>

<body>

<header>
  <div class="logo">DOOMSCROLL</div>
  <nav>
    <a href="#" class="active">Contact Directory</a>
  </nav>
</header>

<div class="page">

  <div class="page-header">
    <div>
      <div class="page-title">Crew Contact Sheet</div>
      <div class="page-sub">SAG Ultra Low Budget · Savannah, GA · Pre-Production 2026</div>
    </div>

    <div style="display:flex;gap:8px;flex-wrap:wrap;">

      <button class="btn btn-ghost" onclick="backupData()">Backup Data</button>

      <button class="btn btn-ghost" onclick="restoreData()">Restore Data</button>

      <button class="btn btn-ghost" onclick="window.print()">Print</button>

      <button class="btn btn-primary" onclick="openModal()">Add Crew</button>

    </div>
  </div>

  <div class="stats-bar" id="statsBar"></div>

  <div class="toolbar">
    <input type="text" id="searchInput" placeholder="Search..." oninput="render()">
  </div>

  <div class="table-wrap">
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Role</th>
          <th>Phone</th>
          <th>Email</th>
          <th>Emergency</th>
          <th>Notes</th>
        </tr>
      </thead>
      <tbody id="tableBody"></tbody>
    </table>
  </div>

</div>

<script>

/* ---------- DATA ---------- */

let crew = JSON.parse(localStorage.getItem("crew") || "[]");

/* ---------- BACKUP ---------- */

function backupData() {
  const dataStr = JSON.stringify(crew, null, 2);
  const blob = new Blob([dataStr], { type: "application/json" });

  const a = document.createElement("a");
  a.href = URL.createObjectURL(blob);
  a.download = "crew-backup.json";
  a.click();
}

/* ---------- RESTORE ---------- */

function restoreData() {
  const input = document.createElement("input");
  input.type = "file";
  input.accept = "application/json";

  input.onchange = e => {
    const file = e.target.files[0];
    if (!file) return;

    const reader = new FileReader();

    reader.onload = event => {
      try {
        crew = JSON.parse(event.target.result);
        localStorage.setItem("crew", JSON.stringify(crew));
        render();
        alert("Backup restored.");
      } catch {
        alert("Invalid file.");
      }
    };

    reader.readAsText(file);
  };

  input.click();
}

/* ---------- RENDER ---------- */

function render() {
  const tbody = document.getElementById("tableBody");

  tbody.innerHTML = crew.map(c => `
    <tr>
      <td>${c.name || ""}</td>
      <td>${c.role || ""}</td>
      <td>${c.phone || ""}</td>
      <td>${c.email || ""}</td>
      <td>${c.ec || ""}</td>
      <td>${c.notes || ""}</td>
    </tr>
  `).join("");
}

/* ---------- MODAL PLACEHOLDER ---------- */

function openModal() {
  alert("Modal still exists in your full version.");
}

/* ---------- INIT ---------- */

render();

</script>

</body>
</html>

