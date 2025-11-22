<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Alok Kumar Kesarwani — Resume</title>
  <style>
    :root{--bg:#f6f8fa;--card:#ffffff;--accent:#0b73ff;--muted:#6b7280}
    body{font-family:Inter,system-ui,Arial,sans-serif;margin:0;background:var(--bg);color:#111}
    .container{max-width:900px;margin:36px auto;padding:24px}
    .card{background:var(--card);border-radius:12px;box-shadow:0 6px 20px rgba(15,23,42,0.06);padding:28px}
    header{display:flex;gap:20px;align-items:center}
    .avatar{width:92px;height:92px;border-radius:12px;background:linear-gradient(135deg,var(--accent),#7c3aed);display:flex;align-items:center;justify-content:center;color:white;font-weight:700;font-size:28px}
    h1{margin:0;font-size:22px}
    .meta{color:var(--muted);font-size:14px;margin-top:6px}
    .grid{display:grid;grid-template-columns:1fr 320px;gap:18px;margin-top:20px}
    .section{margin-bottom:18px}
    h2{font-size:16px;margin:0 0 8px 0}
    ul{margin:0;padding-left:18px;color:#222}
    .right-card{background:#fbfdff;padding:18px;border-radius:10px}
    .btn{display:inline-block;padding:8px 12px;border-radius:8px;text-decoration:none;background:var(--accent);color:#fff;font-weight:600}
    .muted{color:var(--muted);font-size:13px}
    .edit-hint{font-size:13px;margin-top:8px;color:var(--muted)}
    .experience-item{margin-bottom:12px}
    .small{font-size:13px;color:var(--muted)}
    footer{margin-top:18px;font-size:13px;color:var(--muted)}
    pre{white-space:pre-wrap;word-wrap:break-word}
    @media (max-width:880px){.grid{grid-template-columns:1fr}}
  </style>
</head>
<body>
  <div class="container">
    <div class="card" id="mainCard">
      <header>
        <div class="avatar" id="avatar">AK</div>
        <div>
          <h1 id="name">Loading…</h1>
          <div class="meta" id="headline">Loading…</div>
          <div class="meta" id="contact"></div>
        </div>
      </header>

      <div class="grid" style="margin-top:18px;">
        <main>
          <section class="section">
            <h2>Professional Summary</h2>
            <div id="summary" class="muted"></div>
          </section>

          <section class="section">
            <h2>Experience</h2>
            <div id="experience"></div>
          </section>

          <section class="section">
            <h2>Education</h2>
            <div id="education"></div>
          </section>

          <section class="section">
            <h2>Certifications</h2>
            <div id="certifications"></div>
          </section>
        </main>

        <aside>
          <div class="right-card">
            <h2>Skills</h2>
            <div id="skills"></div>

            <h2 style="margin-top:14px">Resume</h2>
            <p class="muted">Download full resume (PDF)</p>
            <!-- Resume PDF path: uploaded file reference (will be converted to URL on server). -->
            <p><a id="resumeLink" class="btn" href="/mnt/data/Alok_Kesarwani_Resume.pdf" target="_blank">Download PDF</a></p>
            <p class="small">If this link doesn't work after you upload to GitHub, replace the href with the PDF file path inside your repo (e.g. <code>/resume.pdf</code>).</p>

            <h2 style="margin-top:14px">Contact</h2>
            <div id="contactCard"></div>

            <div class="edit-hint">
              <strong>Quick update:</strong> Edit <code>profile.json</code> in this repo (or click "Edit" on GitHub) — the site reads that file automatically.
            </div>

            <div style="margin-top:12px">
              <a id="editOnGitHub" class="btn" href="#" target="_blank">Edit profile.json</a>
            </div>
          </div>
        </aside>
      </div>

      <footer>
        Built for <strong id="footerName"></strong>. Hosted by GitHub Pages (place files in the repo root). Last updated: <span id="lastUpdated">—</span>
      </footer>
    </div>
  </div>

<script>
  // Small helper: load profile.json (same folder) and populate the page.
  const PROFILE_URL = 'profile.json';

  async function loadProfile(){
    try{
      const res = await fetch(PROFILE_URL + '?v=' + Date.now());
      if(!res.ok) throw new Error('profile.json not found');
      const data = await res.json();

      document.getElementById('name').textContent = data.name || 'Name';
      document.getElementById('avatar').textContent = (data.name || 'N').split(' ').map(n=>n[0]).slice(0,2).join('');
      document.getElementById('headline').textContent = data.headline || '';
      document.getElementById('contact').innerHTML = (data.email?('✉ '+data.email+'<br>'):'') + (data.phone?('☎ '+data.phone):'');
      document.getElementById('summary').textContent = data.summary || '';

      // experience
      const expDiv = document.getElementById('experience'); expDiv.innerHTML = '';
      (data.experience||[]).forEach(e=>{
        const wrap = document.createElement('div'); wrap.className='experience-item';
        wrap.innerHTML = `<strong>${e.title} | ${e.company}</strong><div class="small">${e.period}</div><div><pre>${e.details}</pre></div>`;
        expDiv.appendChild(wrap);
      });

      // education
      const eduDiv = document.getElementById('education'); eduDiv.innerHTML = '';
      (data.education||[]).forEach(e=>{
        const d = document.createElement('div'); d.innerHTML = `<strong>${e.degree}</strong><div class="small">${e.institution} — ${e.year}</div>`;
        eduDiv.appendChild(d);
      });

      // certifications
      document.getElementById('certifications').textContent = (data.certifications||[]).join(' • ');

      // skills
      document.getElementById('skills').innerHTML = (data.skills||[]).map(s=>`<div style="margin:6px 0">${s}</div>`).join('');

      // contact card
      const contactHtml = [];
      if(data.location) contactHtml.push(`<div class="small">📍 ${data.location}</div>`);
      if(data.email) contactHtml.push(`<div class="small">✉ ${data.email}</div>`);
      if(data.phone) contactHtml.push(`<div class="small">☎ ${data.phone}</div>`);
      if(data.linkedin) contactHtml.push(`<div class="small">in: <a href="${data.linkedin}" target="_blank">${data.linkedin}</a></div>`);
      document.getElementById('contactCard').innerHTML = contactHtml.join('');

      // resume link: (already set to the PDF uploaded, but if profile.json includes 'resumeUrl' override)
      const resumeLink = document.getElementById('resumeLink');
      if(data.resumeUrl) resumeLink.href = data.resumeUrl;

      // Edit-on-github link (user should replace <USERNAME> and <REPO> below with actual repo)
      const repoEditUrl = data.repoEditUrl || 'https://github.com/alokkumarkesarwani/alok/edit/main/profile.json';
      document.getElementById('editOnGitHub').href = repoEditUrl;

      document.getElementById('footerName').textContent = data.name || '';
      document.getElementById('lastUpdated').textContent = new Date().toLocaleString();
    }catch(err){
      console.warn(err);
      document.getElementById('mainCard').insertAdjacentHTML('afterbegin','<div style="padding:12px;background:#fff6f6;border:1px solid #ffd2d2;border-radius:8px;margin-bottom:12px;color:#8b0000">Could not load profile.json — create <code>profile.json</code> in the site root. See README.</div>');
    }
  }

  loadProfile();
</script>
</body>
</html>
