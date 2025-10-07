# nigeria-goods-monitor
Website to monitor goods in Nigeria
// Nigeria Goods Monitoring Web App (Backend + Frontend)
// Author: You 😎

// --- 1. Basic server setup ---
const express = require("express");
const app = express();
const PORT = process.env.PORT || 3000;

app.use(express.json());
app.use(express.static("public"));

// --- 2. Basic homepage ---
app.get("/", (req, res) => {
  res.sendFile(__dirname + "/public/index.html");
});

// --- 3. Admin login (simple version with secret code) ---
const ADMIN_CODE = "NaijaStrong2025"; // you can change this anytime!

app.post("/admin-login", (req, res) => {
  const { code } = req.body;
  if (code === ADMIN_CODE) {
    res.json({ success: true, message: "Welcome, Admin!" });
  } else {
    res.json({ success: false, message: "Wrong admin code." });
  }
});

// --- 4. Reports data ---
let reports = [];

app.post("/report", (req, res) => {
  const { name, goods, location } = req.body;
  if (!name || !goods || !location) {
    return res.status(400).json({ message: "Missing fields" });
  }
  const report = { id: reports.length + 1, name, goods, location, date: new Date() };
  reports.push(report);
  res.json({ message: "Report received", report });
});

app.get("/reports", (req, res) => {
  res.json(reports);
});

// --- 5. Start the server ---
app.listen(PORT, () => console.log(`✅ Server running on port ${PORT}`));
