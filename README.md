# Declan Sweeney
Personal Landing Page
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Declan Sweeney — Senior Sales Leader</title>
  <meta name="description" content="Senior Sales Leader with a track record in enterprise SaaS: $86M portfolio, 180 accounts, operational rigor, and team development." />
  <meta property="og:title" content="Declan Sweeney — Senior Sales Leader" />
  <meta property="og:description" content="Driving revenue, developing leaders, and scaling teams to excellence." />
  <meta property="og:type" content="website" />
  <style>
    :root {
      --bg: #0b1623;
      --bg-soft: #0f1e30;
      --card: #0f243a;
      --text: #e9eef5;
      --muted: #a6b4c8;
      --brand: #3ba0ff;
      --brand-2: #5ce1e6;
      --accent: #8bd18b;
      --danger: #ff6b6b;
      --shadow: 0 20px 40px rgba(0,0,0,.35);
      --radius: 18px;
      --radius-lg: 28px;
      --maxw: 1120px;
    }
    * { box-sizing: border-box; }
    html, body { margin: 0; padding: 0; background: radial-gradient(1200px 800px at 20% -10%, #173557 0%, var(--bg) 55%); color: var(--text); font: 16px/1.6 system-ui, -apple-system, Segoe UI, Roboto, Inter, "Helvetica Neue", Arial, sans-serif; }
    a { color: var(--brand); text-decoration: none; }
    a:hover { text-decoration: underline; }
    .wrap { max-width: var(--maxw); margin: 0 auto; padding: 24px; }

    /* Header */
    header { position: sticky; top: 0; z-index: 5; backdrop-filter: blur(8px); background: linear-gradient(180deg, rgba(11,22,35,.85), rgba(11,22,35,.6)); border-bottom: 1px solid rgba(255,255,255,.06); }
    .nav { display: flex; align-items: center; justify-content: space-between; gap: 16px; }
    .brand { display: flex; align-items: center; gap: 12px; }
    .brand-logo { width: 40px; height: 40px; border-radius: 10px; background: linear-gradient(135deg, var(--brand), var(--brand-2)); box-shadow: var(--shadow); display: grid; place-items: center; font-weight: 800; color: #06111d; }
    .brand h1 { font-size: 18px; margin: 0; letter-spacing: .2px; }
    .nav a.btn { border: 1px solid rgba(255,255,255,.12); padding: 10px 14px; border-radius: 12px; display: inline-flex; align-items: center; gap: 8px; font-weight: 600; }
    .nav a.btn.primary { background: linear-gradient(135deg, var(--brand), var(--brand-2)); color: #05121e; border: none; box-shadow: var(--shadow); }

    /* Hero */
    .hero { display: grid; gap: 28px; padding: 48px 0 24px; grid-template-columns: 1.2fr .8fr; align-items: center; }
    .hero h2 { font-size: clamp(28px, 5vw, 48px); line-height: 1.1; margin: 0 0 12px; }
    .hero p.kicker { color: var(--brand-2); text-transform: uppercase; letter-spacing: 1.6px; font-weight: 700; margin: 0 0 8px; }
    .hero p.sub { color: var(--muted); margin: 0 0 20px; max-width: 62ch; }
    .cta { display: flex; gap: 12px; flex-wrap: wrap; }
    .cta .btn { padding: 14px 18px; border-radius: 14px; font-weight: 700; border: 1px solid rgba(255,255,255,.12); background: rgba(255,255,255,.03); color: var(--text); }
    .cta .btn.primary { background: linear-gradient(135deg, var(--brand), var(--brand-2)); color: #05121e; border: none; box-shadow: var(--shadow); }
    .hero-pic { justify-self: end; width: min(360px, 90vw); aspect-ratio: 1; border-radius: var(--radius-lg); background: linear-gradient(135deg, #16283f, #09131f); border: 1px solid rgba(255,255,255,.08); position: relative; overflow: hidden; box-shadow: var(--shadow); }
    .hero-pic::after { content: "Upload headshot"; position: absolute; inset: 0; display: grid; place-items: center; color: var(--muted); font-weight: 600; letter-spacing: .3px; }

    /* Stats */
    .stats { display: grid; grid-template-columns: repeat(4, 1fr); gap: 16px; margin: 28px 0 12px; }
    .stat { background: linear-gradient(180deg, rgba(255,255,255,.03), rgba(255,255,255,.015)); border: 1px solid rgba(255,255,255,.08); border-radius: var(--radius); padding: 18px; box-shadow: var(--shadow); }
    .stat h3 { margin: 0; font-size: 28px; line-height: 1; }
    .stat p { margin: 6px 0 0; color: var(--muted); font-size: 14px; }

    /* Sections */
    section { margin: 56px 0; }
    .section-title { display: flex; align-items: baseline; justify-content: space-between; margin-bottom: 18px; }
    .section-title h3 { margin: 0; font-size: clamp(22px, 3vw, 32px); }
    .lead { color: var(--muted); max-width: 75ch; }

    /* Cards & layout */
    .grid-3 { display: grid; grid-template-columns: repeat(3, 1fr); gap: 16px; }
    .grid-2 { display: grid; grid-template-columns: repeat(2, 1fr); gap: 16px; }
    .card { background: linear-gradient(180deg, rgba(255,255,255,.03), rgba(255,255,255,.015)); border: 1px solid rgba(255,255,255,.08); border-radius: var(--radius); padding: 20px; box-shadow: var(--shadow); }
    .pill { display: inline-flex; align-items: center; gap: 8px; padding: 8px 12px; border-radius: 999px; background: rgba(255,255,255,.06); margin: 6px 6px 0 0; font-weight: 600; color: var(--text); border: 1px solid rgba(255,255,255,.1); }

    /* Timeline */
    .timeline { position: relative; padding-left: 20px; }
    .timeline::before { content: ""; position: absolute; left: 8px; top: 0; bottom: 0; width: 2px; background: linear-gradient(180deg, rgba(255,255,255,.25), rgba(255,255,255,0)); }
    .event { position: relative; margin: 0 0 18px 0; padding-left: 12px; }
    .event::before { content: ""; position: absolute; left: -3px; top: 6px; width: 12px; height: 12px; border-radius: 50%; background: linear-gradient(135deg, var(--brand), var(--brand-2)); box-shadow: 0 0 0 4px rgba(59,160,255,.15); }

    /* CTA Footer */
    .cta-footer { background: linear-gradient(135deg, rgba(59,160,255,.2), rgba(92,225,230,.16)); border: 1px solid rgba(255,255,255,.14); border-radius: var(--radius-lg); padding: 28px; text-align: center; box-shadow: var(--shadow); }

    /* Footer */
    footer { margin: 48px 0 24px; color: var(--muted); font-size: 14px; display: grid; place-items: center; }

    /* Responsive */
    @media (max-width: 960px) {
      .hero { grid-template-columns: 1fr; }
      .hero-pic { justify-self: start; order: -1; }
      .stats { grid-template-columns: repeat(2, 1fr); }
      .grid-3 { grid-template-columns: 1fr; }
      .grid-2 { grid-template-columns: 1fr; }
    }
  </style>
</head>
<body>
  <header>
    <div class="wrap nav">
      <div class="brand">
        <div class="brand-logo" aria-hidden="true">DS</div>
        <h1>Declan Sweeney</h1>
      </div>
      <nav>
        <a class="btn" href="#approach">Approach</a>
        <a class="btn" href="#achievements">Achievements</a>
        <a class="btn" href="#contact">Contact</a>
        <a class="btn primary" href="#resume" id="resume-btn">Download Resume</a>
      </nav>
    </div>
  </header>

  <main class="wrap">
    <!-- Hero -->
    <section class="hero" aria-label="Hero">
      <div>
        <p class="kicker">Senior Sales Leader · Enterprise SaaS</p>
        <h2>Driving Revenue, Developing Leaders, and Scaling Teams to Excellence</h2>
        <p class="sub">Sales leader managing high-performing enterprise teams and complex portfolios. Known for operational rigor, executive engagement, and coaching that elevates performance. Currently leading a <strong>$86M</strong> book across <strong>180</strong> large domestic accounts with <strong>$48M ARR</strong>.</p>
        <div class="cta">
          <a class="btn primary" href="#contact">Book Intro Call</a>
          <a class="btn" href="#contact">Email Me</a>
          <a class="btn" href="https://www.linkedin.com/in/USERNAME" target="_blank" rel="noopener">LinkedIn</a>
        </div>
      </div>
      <div class="hero-pic" aria-label="Headshot placeholder"></div>
    </section>

    <!-- Stats -->
    <section aria-label="Key metrics">
      <div class="stats">
        <div class="stat">
          <h3>$86M</h3>
          <p>Enterprise portfolio led</p>
        </div>
        <div class="stat">
          <h3>180</h3>
          <p>Strategic accounts</p>
        </div>
        <div class="stat">
          <h3>+18% YoY</h3>
          <p>Growth in a down market</p>
        </div>
        <div class="stat">
          <h3>$5M+</h3>
          <p>New ARR via cross-sell</p>
        </div>
      </div>
    </section>

    <!-- Leadership Approach -->
    <section id="approach">
      <div class="section-title">
        <h3>Leadership Approach</h3>
      </div>
      <p class="lead">I build durable, high-output sales cultures through clear strategy, operational discipline, and coaching that turns good sellers into great leaders.</p>
      <div class="grid-3">
        <div class="card">
          <h4>Operational Excellence</h4>
          <p>Forecast accuracy, pipeline hygiene, and stage-by-stage exit criteria (MEDDPICC). Weekly QBRs that drive action, not theatrics.</p>
          <div>
            <span class="pill">Forecasting</span>
            <span class="pill">Pipeline Design</span>
            <span class="pill">Deal Reviews</span>
          </div>
        </div>
        <div class="card">
          <h4>Customer-Centric Selling</h4>
          <p>C-suite alignment, co-authored value hypotheses, and commercial structures that protect margin while de-risking adoption.</p>
          <div>
            <span class="pill">Value Engineering</span>
            <span class="pill">Exec Workshops</span>
            <span class="pill">Mutual Close Plans</span>
          </div>
        </div>
        <div class="card">
          <h4>People & Coaching</h4>
          <p>Clear expectations, skill maps, and targeted practice. I invest in reps’ careers while holding a high bar for performance.</p>
          <div>
            <span class="pill">Call Coaching</span>
            <span class="pill">Career Paths</span>
            <span class="pill">Hiring Bar</span>
          </div>
        </div>
      </div>
    </section>

    <!-- Notable Achievements -->
    <section id="achievements">
      <div class="section-title">
        <h3>Notable Achievements</h3>
      </div>
      <div class="grid-2">
        <div class="card">
          <h4>Contract Renegotiation & Recovery</h4>
          <p><strong>Situation:</strong> Customer receiving higher-tier product than paid for ($33K/year gap, 2.5 years).<br />
          <strong>Action:</strong> Led phased correction strategy (0% now, 50% Y2, 100% Y3) with value narrative and executive alignment.<br />
          <strong>Result:</strong> Protected relationship and secured multi-year revenue recovery while minimizing churn risk.</p>
        </div>
        <div class="card">
          <h4>Temporary Coverage → Expansion</h4>
          <p><strong>Situation:</strong> Took on additional enterprise books during a maternity leave.<br />
          <strong>Action:</strong> Treated as own territory: prioritized risk, uncovered whitespace, built cross-sell plays with CSMs.<br />
          <strong>Result:</strong> Closed multimillion-dollar cross-sell and received leadership recognition for impact beyond remit.</p>
        </div>
        <div class="card">
          <h4>Team Performance Lift</h4>
          <p><strong>Situation:</strong> Mixed attainment across team.<br />
          <strong>Action:</strong> Introduced weekly rhythm (QBRs, deal clinics, skill blocks) and better territory design.<br />
          <strong>Result:</strong> Drove 100%+ quota for multiple reps and improved win rates on complex deals.</p>
        </div>
        <div class="card">
          <h4>Executive Trust & Access</h4>
          <p>Built repeatable executive engagement motion (problem framing, business impact, proof, and plan). Led to shorter cycles and bigger scope.</p>
        </div>
      </div>
    </section>

    <!-- Career Snapshot / Timeline -->
    <section id="experience">
      <div class="section-title">
        <h3>Career Snapshot</h3>
      </div>
      <div class="timeline">
        <div class="event">
          <h4>Senior Sales Manager — HR SaaS (Enterprise)</h4>
          <p>Lead 7 Account Directors across 180 enterprise accounts ($86M TCV, $48M ARR). Focus: renewals, expansion, and multi-product adoption.</p>
        </div>
        <div class="event">
          <h4>Sales Leader — Large Enterprise</h4>
          <p>Scaled cross-sell programs, established governance for forecast accuracy, and mentored future managers.</p>
        </div>
        <div class="event">
          <h4>Enterprise AE & Team Lead</h4>
          <p>Closed seven-figure ARR deals with complex stakeholders; piloted executive workshops that became the org standard.</p>
        </div>
      </div>
    </section>

    <!-- Testimonials -->
    <section id="testimonials">
      <div class="section-title">
        <h3>What Leaders & Partners Say</h3>
      </div>
      <div class="grid-3">
        <blockquote class="card"><p>“Balances strategy and execution flawlessly — a reliable builder of pipeline and people.”</p><p>— VP, Enterprise Sales</p></blockquote>
        <blockquote class="card"><p>“Elevates the team with calm confidence. Trusted with our most complex accounts.”</p><p>— Director, Customer Success</p></blockquote>
        <blockquote class="card"><p>“Executive presence with real business acumen. Deals move faster with him in the room.”</p><p>— CRO, SaaS</p></blockquote>
      </div>
    </section>

    <!-- Download Resume / CTA -->
    <section id="resume" class="cta-footer">
      <h3>Download Resume & Book an Intro</h3>
      <p class="lead">Prefer a one-pager? Grab the PDF, or reach out to compare calendars for a 20-minute conversation.</p>
      <div class="cta" style="justify-content:center;">
        <a class="btn primary" id="resume-download" href="#">Download Resume (PDF)</a>
        <a class="btn" href="mailto:declan@example.com?subject=Intro%20Conversation%20—%20Senior%20Sales%20Leader">Email Me</a>
        <a class="btn" href="#contact">Book Intro Call</a>
      </div>
    </section>

    <!-- Contact -->
    <section id="contact">
      <div class="section-title">
        <h3>Contact</h3>
      </div>
      <div class="grid-2">
        <div class="card">
          <h4>Get in Touch</h4>
          <p>Email: <a href="mailto:declan@example.com">declan@example.com</a><br />
          LinkedIn: <a href="https://www.linkedin.com/in/USERNAME" target="_blank" rel="noopener">linkedin.com/in/USERNAME</a></p>
          <p>Location: US (remote-friendly) · Open to travel</p>
        </div>
        <div class="card">
          <h4>Focus Areas</h4>
          <div>
            <span class="pill">Enterprise SaaS</span>
            <span class="pill">Sales Leadership</span>
            <span class="pill">GTM Strategy</span>
            <span class="pill">Renewals & Expansion</span>
            <span class="pill">MEDDPICC</span>
            <span class="pill">Coaching & Enablement</span>
          </div>
        </div>
      </div>
    </section>
  </main>

  <footer>
    <small>© <span id="year"></span> Declan Sweeney · Built for senior sales leadership opportunities</small>
  </footer>

  <script>
    // Smooth scrolling for same-page anchors
    document.querySelectorAll('a[href^="#"]').forEach(a => {
      a.addEventListener('click', e => {
        const id = a.getAttribute('href');
        if (id.length > 1) {
          const el = document.querySelector(id);
          if (el) {
            e.preventDefault();
            el.scrollIntoView({ behavior: 'smooth', block: 'start' });
          }
        }
      });
    });

    // Dynamic year
    document.getElementById('year').textContent = new Date().getFullYear();

    // Resume placeholder
    const resumeBtn = document.getElementById('resume-download');
    resumeBtn?.addEventListener('click', (e) => {
      e.preventDefault();
      alert('Hook up your resume PDF: replace this button\'s href with your hosted file (e.g., /Declan-Sweeney-Resume.pdf).');
    });
  </script>
</body>
</html>
