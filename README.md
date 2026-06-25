<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Rotina Full-Stack – 4h/dia</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&family=JetBrains+Mono:wght@400;600&display=swap');

  :root {
    --bg:        #0d0f14;
    --surface:   #161a22;
    --border:    #242834;
    --accent:    #5b6af0;
    --accent2:   #9b5cf6;
    --green:     #2dd4aa;
    --yellow:    #f6c340;
    --red:       #f05b5b;
    --text:      #e4e6ef;
    --muted:     #7a8099;
    --radius:    12px;
  }

  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  body {
    background: var(--bg);
    color: var(--text);
    font-family: 'Inter', sans-serif;
    min-height: 100vh;
    padding: 32px 16px 64px;
  }

  /* ── HEADER ── */
  header {
    max-width: 860px;
    margin: 0 auto 48px;
    text-align: center;
  }
  .badge {
    display: inline-block;
    background: linear-gradient(135deg, var(--accent), var(--accent2));
    font-size: 11px;
    font-weight: 700;
    letter-spacing: .12em;
    text-transform: uppercase;
    padding: 4px 14px;
    border-radius: 999px;
    margin-bottom: 18px;
  }
  h1 {
    font-size: clamp(28px, 5vw, 46px);
    font-weight: 800;
    line-height: 1.15;
    letter-spacing: -.02em;
    background: linear-gradient(120deg, #fff 0%, var(--accent) 60%, var(--accent2) 100%);
    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent;
    background-clip: text;
  }
  .subtitle {
    margin-top: 12px;
    color: var(--muted);
    font-size: 15px;
    font-weight: 400;
  }

  /* ── STATS ROW ── */
  .stats {
    display: flex;
    justify-content: center;
    gap: 12px;
    flex-wrap: wrap;
    margin-top: 28px;
  }
  .stat {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    padding: 14px 22px;
    text-align: center;
    min-width: 110px;
  }
  .stat-num {
    font-size: 22px;
    font-weight: 800;
    font-family: 'JetBrains Mono', monospace;
    color: var(--accent);
  }
  .stat-label {
    font-size: 11px;
    color: var(--muted);
    margin-top: 2px;
    text-transform: uppercase;
    letter-spacing: .08em;
  }

  /* ── WEEK TABS ── */
  .week-nav {
    max-width: 860px;
    margin: 0 auto 24px;
    display: flex;
    gap: 8px;
    flex-wrap: wrap;
  }
  .week-btn {
    background: var(--surface);
    border: 1px solid var(--border);
    color: var(--muted);
    font-size: 12px;
    font-weight: 600;
    letter-spacing: .06em;
    text-transform: uppercase;
    padding: 8px 16px;
    border-radius: 8px;
    cursor: pointer;
    transition: all .18s;
  }
  .week-btn:hover { border-color: var(--accent); color: var(--text); }
  .week-btn.active {
    background: var(--accent);
    border-color: var(--accent);
    color: #fff;
  }

  /* ── WEEK PANELS ── */
  .week-panel { display: none; max-width: 860px; margin: 0 auto; }
  .week-panel.active { display: block; }

  .week-title {
    font-size: 13px;
    font-weight: 700;
    color: var(--muted);
    text-transform: uppercase;
    letter-spacing: .1em;
    margin-bottom: 20px;
  }
  .week-title span {
    color: var(--accent);
    font-family: 'JetBrains Mono', monospace;
  }

  /* ── DAY CARD ── */
  .day-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: var(--radius);
    margin-bottom: 16px;
    overflow: hidden;
    transition: border-color .18s;
  }
  .day-card:hover { border-color: #353a4f; }

  .day-header {
    display: flex;
    align-items: center;
    gap: 14px;
    padding: 16px 20px;
    cursor: pointer;
    user-select: none;
  }
  .day-dot {
    width: 36px;
    height: 36px;
    border-radius: 10px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 13px;
    font-weight: 800;
    font-family: 'JetBrains Mono', monospace;
    flex-shrink: 0;
  }
  .day-info { flex: 1; min-width: 0; }
  .day-name { font-size: 14px; font-weight: 700; }
  .day-theme { font-size: 12px; color: var(--muted); margin-top: 2px; }
  .day-toggle {
    font-size: 18px;
    color: var(--muted);
    transition: transform .2s;
  }
  .day-card.open .day-toggle { transform: rotate(90deg); }

  /* color variants */
  .c-blue  { background: rgba(91,106,240,.15); color: var(--accent); }
  .c-purple{ background: rgba(155,92,246,.15); color: var(--accent2); }
  .c-green { background: rgba(45,212,170,.15); color: var(--green); }
  .c-yellow{ background: rgba(246,195,64,.15);  color: var(--yellow); }
  .c-red   { background: rgba(240,91,91,.15);   color: var(--red); }

  /* ── BLOCKS ── */
  .day-body { display: none; padding: 0 20px 20px; }
  .day-card.open .day-body { display: block; }

  .block {
    display: flex;
    gap: 14px;
    padding: 14px 0;
    border-top: 1px solid var(--border);
    align-items: flex-start;
  }
  .block-time {
    font-family: 'JetBrains Mono', monospace;
    font-size: 11px;
    color: var(--muted);
    white-space: nowrap;
    padding-top: 2px;
    min-width: 94px;
  }
  .block-content { flex: 1; min-width: 0; }
  .block-title {
    font-size: 13px;
    font-weight: 700;
    margin-bottom: 4px;
  }
  .block-desc {
    font-size: 12px;
    color: var(--muted);
    line-height: 1.55;
    margin-bottom: 8px;
  }
  .links {
    display: flex;
    flex-wrap: wrap;
    gap: 6px;
  }
  .link-chip {
    display: inline-flex;
    align-items: center;
    gap: 5px;
    font-size: 11px;
    font-weight: 600;
    padding: 4px 10px;
    border-radius: 6px;
    text-decoration: none;
    border: 1px solid var(--border);
    background: var(--bg);
    color: var(--text);
    transition: all .15s;
  }
  .link-chip:hover { border-color: var(--accent); color: var(--accent); }
  .link-chip::before { content: '↗'; font-size: 10px; opacity: .6; }

  /* ── BREAK BLOCK ── */
  .block-break .block-title { color: var(--green); }

  /* ── PROGRESS ── */
  .progress-bar {
    width: 100%;
    height: 3px;
    background: var(--border);
    border-radius: 99px;
    overflow: hidden;
    margin-top: 6px;
  }
  .progress-fill {
    height: 100%;
    background: linear-gradient(90deg, var(--accent), var(--accent2));
    border-radius: 99px;
  }

  /* ── TIP BOX ── */
  .tip {
    background: rgba(91,106,240,.07);
    border: 1px solid rgba(91,106,240,.2);
    border-radius: var(--radius);
    padding: 14px 18px;
    font-size: 12px;
    color: var(--muted);
    line-height: 1.6;
    margin-top: 24px;
  }
  .tip strong { color: var(--text); }
</style>
</head>
<body>

<header>
  <div class="badge">🚀 Rotina de Estudos</div>
  <h1>Full-Stack Developer<br/>Nível Básico → Sólido</h1>
  <p class="subtitle">4 horas por dia · Segunda a Sexta · 12 semanas estruturadas</p>
  <div class="stats">
    <div class="stat"><div class="stat-num">4h</div><div class="stat-label">por dia</div></div>
    <div class="stat"><div class="stat-num">12</div><div class="stat-label">semanas</div></div>
    <div class="stat"><div class="stat-num">240h</div><div class="stat-label">total</div></div>
    <div class="stat"><div class="stat-num">60</div><div class="stat-label">dias úteis</div></div>
  </div>
</header>

<!-- WEEK TABS -->
<div class="week-nav">
  <button class="week-btn active" onclick="showWeek(1,this)">Sem. 1–3 · HTML/CSS/JS</button>
  <button class="week-btn" onclick="showWeek(2,this)">Sem. 4–6 · React</button>
  <button class="week-btn" onclick="showWeek(3,this)">Sem. 7–9 · Node + BD</button>
  <button class="week-btn" onclick="showWeek(4,this)">Sem. 10–12 · Full-Stack</button>
</div>

<!-- ═══════════════════════════════════════ SEMANA 1–3 -->
<div class="week-panel active" id="week-1">
  <p class="week-title">Fase 1 — <span>Semanas 1 a 3</span> · HTML, CSS e JavaScript</p>

  <!-- SEG -->
  <div class="day-card open">
    <div class="day-header" onclick="toggle(this)">
      <div class="day-dot c-blue">SEG</div>
      <div class="day-info">
        <div class="day-name">Segunda-feira</div>
        <div class="day-theme">HTML Fundamentos + Semântica</div>
      </div>
      <div class="day-toggle">›</div>
    </div>
    <div class="day-body">

      <div class="block">
        <div class="block-time">08:00 – 09:30</div>
        <div class="block-content">
          <div class="block-title">📖 Teoria: Estrutura HTML</div>
          <div class="block-desc">Tags semânticas, formulários, acessibilidade básica e boas práticas de marcação.</div>
          <div class="links">
            <a class="link-chip" href="https://www.youtube.com/watch?v=Ejkb_YpuHWs" target="_blank">Curso em Vídeo – HTML5 Completo</a>
            <a class="link-chip" href="https://developer.mozilla.org/pt-BR/docs/Learn/HTML" target="_blank">MDN · Aprender HTML</a>
          </div>
        </div>
      </div>

      <div class="block block-break">
        <div class="block-time">09:30 – 09:45</div>
        <div class="block-content">
          <div class="block-title">☕ Pausa</div>
        </div>
      </div>

      <div class="block">
        <div class="block-time">09:45 – 11:15</div>
        <div class="block-content">
          <div class="block-title">💻 Prática: Página pessoal</div>
          <div class="block-desc">Construa sua página de perfil usando somente HTML semântico: header, nav, main, section, footer.</div>
          <div class="links">
            <a class="link-chip" href="https://www.frontendmentor.io/challenges" target="_blank">Frontend Mentor – Desafios</a>
          </div>
        </div>
      </div>

      <div class="block block-break">
        <div class="block-time">11:15 – 11:30</div>
        <div class="block-content"><div class="block-title">🧘 Pausa longa</div></div>
      </div>

      <div class="block">
        <div class="block-time">11:30 – 12:00</div>
        <div class="block-content">
          <div class="block-title">📝 Revisão + Exercícios</div>
          <div class="block-desc">Exercícios de fixação no freeCodeCamp – módulo HTML.</div>
          <div class="links">
            <a class="link-chip" href="https://www.freecodecamp.org/learn/2022/responsive-web-design/" target="_blank">freeCodeCamp – Responsive Web Design</a>
          </div>
        </div>
      </div>

    </div>
  </div><!-- /SEG -->

  <!-- TER -->
  <div class="day-card">
    <div class="day-header" onclick="toggle(this)">
      <div class="day-dot c-purple">TER</div>
      <div class="day-info">
        <div class="day-name">Terça-feira</div>
        <div class="day-theme">CSS – Box Model, Flexbox e Grid</div>
      </div>
      <div class="day-toggle">›</div>
    </div>
    <div class="day-body">
      <div class="block">
        <div class="block-time">08:00 – 09:30</div>
        <div class="block-content">
          <div class="block-title">📖 Teoria: CSS Fundamentals</div>
          <div class="block-desc">Box model, especificidade, Flexbox e CSS Grid do zero.</div>
          <div class="links">
            <a class="link-chip" href="https://www.youtube.com/watch?v=FdZFSMYQlus" target="_blank">Curso em Vídeo – CSS3 Completo</a>
            <a class="link-chip" href="https://css-tricks.com/snippets/css/a-guide-to-flexbox/" target="_blank">CSS-Tricks · Guia Flexbox</a>
            <a class="link-chip" href="https://cssgridgarden.com/" target="_blank">Grid Garden (jogo)</a>
          </div>
        </div>
      </div>
      <div class="block block-break">
        <div class="block-time">09:30 – 09:45</div>
        <div class="block-content"><div class="block-title">☕ Pausa</div></div>
      </div>
      <div class="block">
        <div class="block-time">09:45 – 11:15</div>
        <div class="block-content">
          <div class="block-title">💻 Prática: Estilizar página pessoal</div>
          <div class="block-desc">Aplique Flexbox para o layout da sua página do dia anterior. Adicione variáveis CSS.</div>
          <div class="links">
            <a class="link-chip" href="https://flexboxfroggy.com/#pt-br" target="_blank">Flexbox Froggy (jogo)</a>
          </div>
        </div>
      </div>
      <div class="block block-break">
        <div class="block-time">11:15 – 11:30</div>
        <div class="block-content"><div class="block-title">🧘 Pausa longa</div></div>
      </div>
      <div class="block">
        <div class="block-time">11:30 – 12:00</div>
        <div class="block-content">
          <div class="block-title">📝 Desafio: Clone de card</div>
          <div class="block-desc">Reproduza um card de produto ou perfil usando apenas CSS.</div>
          <div class="links">
            <a class="link-chip" href="https://www.frontendmentor.io/challenges/product-preview-card-component-GO7UmttRfa" target="_blank">Frontend Mentor · Card Desafio</a>
          </div>
        </div>
      </div>
    </div>
  </div><!-- /TER -->

  <!-- QUA -->
  <div class="day-card">
    <div class="day-header" onclick="toggle(this)">
      <div class="day-dot c-green">QUA</div>
      <div class="day-info">
        <div class="day-name">Quarta-feira</div>
        <div class="day-theme">JavaScript – Variáveis, Funções e DOM</div>
      </div>
      <div class="day-toggle">›</div>
    </div>
    <div class="day-body">
      <div class="block">
        <div class="block-time">08:00 – 09:30</div>
        <div class="block-content">
          <div class="block-title">📖 Teoria: JS Básico</div>
          <div class="block-desc">Variáveis, tipos, funções, loops, arrays e objetos. Manipulação básica do DOM.</div>
          <div class="links">
            <a class="link-chip" href="https://www.youtube.com/watch?v=BXqUH86F-kA" target="_blank">Curso em Vídeo – JavaScript</a>
            <a class="link-chip" href="https://javascript.info/" target="_blank">javascript.info (PT via tradução)</a>
          </div>
        </div>
      </div>
      <div class="block block-break">
        <div class="block-time">09:30 – 09:45</div>
        <div class="block-content"><div class="block-title">☕ Pausa</div></div>
      </div>
      <div class="block">
        <div class="block-time">09:45 – 11:15</div>
        <div class="block-content">
          <div class="block-title">💻 Prática: Contador interativo</div>
          <div class="block-desc">Crie um contador com botões de incrementar, decrementar e resetar usando JS puro e DOM.</div>
          <div class="links">
            <a class="link-chip" href="https://www.freecodecamp.org/learn/javascript-algorithms-and-data-structures/" target="_blank">freeCodeCamp – JS Algorithms</a>
          </div>
        </div>
      </div>
      <div class="block block-break">
        <div class="block-time">11:15 – 11:30</div>
        <div class="block-content"><div class="block-title">🧘 Pausa longa</div></div>
      </div>
      <div class="block">
        <div class="block-time">11:30 – 12:00</div>
        <div class="block-content">
          <div class="block-title">📝 Revisão + Katas</div>
          <div class="block-desc">Resolva 2–3 katas de nível 8–7 kyu no Codewars para fixar lógica.</div>
          <div class="links">
            <a class="link-chip" href="https://www.codewars.com/" target="_blank">Codewars</a>
          </div>
        </div>
      </div>
    </div>
  </div><!-- /QUA -->

  <!-- QUI -->
  <div class="day-card">
    <div class="day-header" onclick="toggle(this)">
      <div class="day-dot c-yellow">QUI</div>
      <div class="day-info">
        <div class="day-name">Quinta-feira</div>
        <div class="day-theme">JavaScript – ES6+, Promises e Fetch API</div>
      </div>
      <div class="day-toggle">›</div>
    </div>
    <div class="day-body">
      <div class="block">
        <div class="block-time">08:00 – 09:30</div>
        <div class="block-content">
          <div class="block-title">📖 Teoria: ES6+ Moderno</div>
          <div class="block-desc">Arrow functions, destructuring, spread, async/await, Promises e Fetch API.</div>
          <div class="links">
            <a class="link-chip" href="https://www.youtube.com/watch?v=i6Oi-YtXnAU" target="_blank">Rocketseat – ES6 Completo</a>
            <a class="link-chip" href="https://javascript.info/async" target="_blank">javascript.info – Async</a>
          </div>
        </div>
      </div>
      <div class="block block-break">
        <div class="block-time">09:30 – 09:45</div>
        <div class="block-content"><div class="block-title">☕ Pausa</div></div>
      </div>
      <div class="block">
        <div class="block-time">09:45 – 11:15</div>
        <div class="block-content">
          <div class="block-title">💻 Prática: Consumir API pública</div>
          <div class="block-desc">Use fetch para buscar dados da PokeAPI e renderizar cards dinamicamente no HTML.</div>
          <div class="links">
            <a class="link-chip" href="https://pokeapi.co/" target="_blank">PokeAPI</a>
            <a class="link-chip" href="https://github.com/public-apis/public-apis" target="_blank">Lista de APIs públicas</a>
          </div>
        </div>
      </div>
      <div class="block block-break">
        <div class="block-time">11:15 – 11:30</div>
        <div class="block-content"><div class="block-title">🧘 Pausa longa</div></div>
      </div>
      <div class="block">
        <div class="block-time">11:30 – 12:00</div>
        <div class="block-content">
          <div class="block-title">📝 Git + GitHub</div>
          <div class="block-desc">Commite seu projeto do dia. Aprenda o fluxo básico: init, add, commit, push.</div>
          <div class="links">
            <a class="link-chip" href="https://www.youtube.com/watch?v=UBAX-13g8OM" target="_blank">Curso em Vídeo – Git e GitHub</a>
          </div>
        </div>
      </div>
    </div>
  </div><!-- /QUI -->

  <!-- SEX -->
  <div class="day-card">
    <div class="day-header" onclick="toggle(this)">
      <div class="day-dot c-red">SEX</div>
      <div class="day-info">
        <div class="day-name">Sexta-feira</div>
        <div class="day-theme">Projeto integrador da semana + Revisão</div>
      </div>
      <div class="day-toggle">›</div>
    </div>
    <div class="day-body">
      <div class="block">
        <div class="block-time">08:00 – 10:00</div>
        <div class="block-content">
          <div class="block-title">🏗️ Projeto: To-Do List</div>
          <div class="block-desc">Construa uma lista de tarefas completa com HTML, CSS e JS. Adicionar, concluir e remover itens. Use localStorage para persistência.</div>
          <div class="links">
            <a class="link-chip" href="https://www.youtube.com/watch?v=HSssE1mZkVc" target="_blank">Tutorial To-Do JS Puro</a>
          </div>
        </div>
      </div>
      <div class="block block-break">
        <div class="block-time">10:00 – 10:15</div>
        <div class="block-content"><div class="block-title">☕ Pausa</div></div>
      </div>
      <div class="block">
        <div class="block-time">10:15 – 11:30</div>
        <div class="block-content">
          <div class="block-title">🔁 Revisão da semana</div>
          <div class="block-desc">Releia anotações, conserte bugs do projeto e documente o README no GitHub.</div>
          <div class="links">
            <a class="link-chip" href="https://readme.so/pt" target="_blank">readme.so – Gerador de README</a>
          </div>
        </div>
      </div>
      <div class="block block-break">
        <div class="block-time">11:30 – 11:45</div>
        <div class="block-content"><div class="block-title">🧘 Pausa</div></div>
      </div>
      <div class="block">
        <div class="block-time">11:45 – 12:00</div>
        <div class="block-content">
          <div class="block-title">📌 Planejamento da próxima semana</div>
          <div class="block-desc">Anote 3 dúvidas que surgiram, 1 conquista e o próximo tema.</div>
        </div>
      </div>
    </div>
  </div><!-- /SEX -->

  <div class="tip">
    <strong>💡 Meta das semanas 1–3:</strong> ao final, você deve conseguir criar páginas web responsivas do zero, consumir APIs com JS puro e versionar projetos com Git. Progresso esperado: <strong>HTML/CSS 80%</strong> · <strong>JS básico 70%</strong>.
  </div>
</div>


<!-- ═══════════════════════════════════════ SEMANA 4–6 -->
<div class="week-panel" id="week-2">
  <p class="week-title">Fase 2 — <span>Semanas 4 a 6</span> · React.js</p>

  <div class="day-card open">
    <div class="day-header" onclick="toggle(this)">
      <div class="day-dot c-blue">SEG</div>
      <div class="day-info"><div class="day-name">Segunda-feira</div><div class="day-theme">React – Componentes, Props e State</div></div>
      <div class="day-toggle">›</div>
    </div>
    <div class="day-body">
      <div class="block">
        <div class="block-time">08:00 – 09:30</div>
        <div class="block-content">
          <div class="block-title">📖 Teoria: Fundamentos do React</div>
          <div class="block-desc">JSX, componentes funcionais, props, useState e re-renderização.</div>
          <div class="links">
            <a class="link-chip" href="https://react.dev/learn" target="_blank">Documentação oficial React</a>
            <a class="link-chip" href="https://www.youtube.com/watch?v=pDbcC-xSat4" target="_blank">Rocketseat – React do Zero</a>
          </div>
        </div>
      </div>
      <div class="block block-break"><div class="block-time">09:30 – 09:45</div><div class="block-content"><div class="block-title">☕ Pausa</div></div></div>
      <div class="block">
        <div class="block-time">09:45 – 11:15</div>
        <div class="block-content">
          <div class="block-title">💻 Prática: Vite + primeiro componente</div>
          <div class="block-desc">Crie um projeto com Vite, estruture componentes de Card e Button reutilizáveis.</div>
          <div class="links">
            <a class="link-chip" href="https://vitejs.dev/guide/" target="_blank">Vite – Guia Rápido</a>
          </div>
        </div>
      </div>
      <div class="block block-break"><div class="block-time">11:15 – 11:30</div><div class="block-content"><div class="block-title">🧘 Pausa</div></div></div>
      <div class="block">
        <div class="block-time">11:30 – 12:00</div>
        <div class="block-content">
          <div class="block-title">📝 Exercício: Lista de produtos</div>
          <div class="block-desc">Renderize uma lista de objetos usando .map() e componentes filhos.</div>
        </div>
      </div>
    </div>
  </div>

  <div class="day-card">
    <div class="day-header" onclick="toggle(this)">
      <div class="day-dot c-purple">TER</div>
      <div class="day-info"><div class="day-name">Terça-feira</div><div class="day-theme">React – Hooks: useEffect e useContext</div></div>
      <div class="day-toggle">›</div>
    </div>
    <div class="day-body">
      <div class="block">
        <div class="block-time">08:00 – 09:30</div>
        <div class="block-content">
          <div class="block-title">📖 Teoria: Hooks avançados</div>
          <div class="block-desc">useEffect para efeitos colaterais, fetch de dados, useContext para estado global.</div>
          <div class="links">
            <a class="link-chip" href="https://react.dev/reference/react/useEffect" target="_blank">Docs – useEffect</a>
            <a class="link-chip" href="https://www.youtube.com/watch?v=TNhaISOUy6Q" target="_blank">Traversy – React Hooks Crash</a>
          </div>
        </div>
      </div>
      <div class="block block-break"><div class="block-time">09:30 – 09:45</div><div class="block-content"><div class="block-title">☕ Pausa</div></div></div>
      <div class="block">
        <div class="block-time">09:45 – 11:15</div>
        <div class="block-content">
          <div class="block-title">💻 Prática: App de clima</div>
          <div class="block-desc">Consuma a OpenWeather API no React com useEffect e exiba dados dinâmicos.</div>
          <div class="links">
            <a class="link-chip" href="https://openweathermap.org/api" target="_blank">OpenWeather API</a>
          </div>
        </div>
      </div>
      <div class="block block-break"><div class="block-time">11:15 – 11:30</div><div class="block-content"><div class="block-title">🧘 Pausa</div></div></div>
      <div class="block">
        <div class="block-time">11:30 – 12:00</div>
        <div class="block-content">
          <div class="block-title">📝 Revisão + Codewars</div>
          <div class="block-desc">2 katas de JavaScript para manter o raciocínio lógico afiado.</div>
          <div class="links"><a class="link-chip" href="https://www.codewars.com/" target="_blank">Codewars</a></div>
        </div>
      </div>
    </div>
  </div>

  <div class="day-card">
    <div class="day-header" onclick="toggle(this)">
      <div class="day-dot c-green">QUA</div>
      <div class="day-info"><div class="day-name">Quarta-feira</div><div class="day-theme">React Router + Formulários</div></div>
      <div class="day-toggle">›</div>
    </div>
    <div class="day-body">
      <div class="block">
        <div class="block-time">08:00 – 09:30</div>
        <div class="block-content">
          <div class="block-title">📖 Teoria: Roteamento no React</div>
          <div class="block-desc">React Router v6, rotas aninhadas, useNavigate, useParams e formulários controlados.</div>
          <div class="links">
            <a class="link-chip" href="https://reactrouter.com/en/main/start/tutorial" target="_blank">React Router – Tutorial oficial</a>
          </div>
        </div>
      </div>
      <div class="block block-break"><div class="block-time">09:30 – 09:45</div><div class="block-content"><div class="block-title">☕ Pausa</div></div></div>
      <div class="block">
        <div class="block-time">09:45 – 11:15</div>
        <div class="block-content">
          <div class="block-title">💻 Prática: SPA com múltiplas páginas</div>
          <div class="block-desc">Crie Home, Sobre e Contato com React Router. Adicione formulário de contato com validação.</div>
        </div>
      </div>
      <div class="block block-break"><div class="block-time">11:15 – 11:30</div><div class="block-content"><div class="block-title">🧘 Pausa</div></div></div>
      <div class="block">
        <div class="block-time">11:30 – 12:00</div>
        <div class="block-content">
          <div class="block-title">📝 Tailwind CSS</div>
          <div class="block-desc">Introdução ao Tailwind para estilizar componentes React rapidamente.</div>
          <div class="links"><a class="link-chip" href="https://tailwindcss.com/docs" target="_blank">Tailwind Docs</a></div>
        </div>
      </div>
    </div>
  </div>

  <div class="day-card">
    <div class="day-header" onclick="toggle(this)">
      <div class="day-dot c-yellow">QUI</div>
      <div class="day-info"><div class="day-name">Quinta-feira</div><div class="day-theme">TypeScript básico + React + TS</div></div>
      <div class="day-toggle">›</div>
    </div>
    <div class="day-body">
      <div class="block">
        <div class="block-time">08:00 – 09:30</div>
        <div class="block-content">
          <div class="block-title">📖 Teoria: TypeScript para React</div>
          <div class="block-desc">Tipos básicos, interfaces, generics e como tipar props e hooks no React.</div>
          <div class="links">
            <a class="link-chip" href="https://www.typescriptlang.org/docs/handbook/intro.html" target="_blank">TypeScript Handbook</a>
            <a class="link-chip" href="https://www.youtube.com/watch?v=30LWjhZzg50" target="_blank">Traversy – TypeScript Crash</a>
          </div>
        </div>
      </div>
      <div class="block block-break"><div class="block-time">09:30 – 09:45</div><div class="block-content"><div class="block-title">☕ Pausa</div></div></div>
      <div class="block">
        <div class="block-time">09:45 – 11:30</div>
        <div class="block-content">
          <div class="block-title">💻 Migrar projeto para TS</div>
          <div class="block-desc">Converta um dos projetos anteriores para TypeScript, adicionando tipagem completa.</div>
        </div>
      </div>
      <div class="block block-break"><div class="block-time">11:30 – 11:45</div><div class="block-content"><div class="block-title">🧘 Pausa</div></div></div>
      <div class="block">
        <div class="block-time">11:45 – 12:00</div>
        <div class="block-content">
          <div class="block-title">📝 Revisão rápida</div>
          <div class="block-desc">Revise os erros de tipo encontrados e anote padrões aprendidos.</div>
        </div>
      </div>
    </div>
  </div>

  <div class="day-card">
    <div class="day-header" onclick="toggle(this)">
      <div class="day-dot c-red">SEX</div>
      <div class="day-info"><div class="day-name">Sexta-feira</div><div class="day-theme">Projeto: App completo em React</div></div>
      <div class="day-toggle">›</div>
    </div>
    <div class="day-body">
      <div class="block">
        <div class="block-time">08:00 – 11:30</div>
        <div class="block-content">
          <div class="block-title">🏗️ Projeto: GitHub Finder</div>
          <div class="block-desc">App React + TypeScript que busca perfis do GitHub pela API, exibe repos e estatísticas. Deploy no Vercel.</div>
          <div class="links">
            <a class="link-chip" href="https://docs.github.com/en/rest" target="_blank">GitHub API Docs</a>
            <a class="link-chip" href="https://vercel.com/" target="_blank">Vercel – Deploy grátis</a>
          </div>
        </div>
      </div>
      <div class="block block-break"><div class="block-time">11:30 – 11:45</div><div class="block-content"><div class="block-title">🧘 Pausa</div></div></div>
      <div class="block">
        <div class="block-time">11:45 – 12:00</div>
        <div class="block-content"><div class="block-title">📌 Retrospectiva da semana</div><div class="block-desc">Anote o que foi difícil, o que ficou bom, e atualize o perfil no LinkedIn com o projeto.</div></div>
      </div>
    </div>
  </div>

  <div class="tip">
    <strong>💡 Meta das semanas 4–6:</strong> dominar React com hooks, roteamento, TypeScript e consumo de APIs. Você deve ter ao menos 2 projetos no GitHub com deploy no Vercel.
  </div>
</div>


<!-- ═══════════════════════════════════════ SEMANA 7–9 -->
<div class="week-panel" id="week-3">
  <p class="week-title">Fase 3 — <span>Semanas 7 a 9</span> · Node.js + Banco de Dados</p>

  <div class="day-card open">
    <div class="day-header" onclick="toggle(this)">
      <div class="day-dot c-blue">SEG</div>
      <div class="day-info"><div class="day-name">Segunda-feira</div><div class="day-theme">Node.js + Express – Servidor e Rotas</div></div>
      <div class="day-toggle">›</div>
    </div>
    <div class="day-body">
      <div class="block">
        <div class="block-time">08:00 – 09:30</div>
        <div class="block-content">
          <div class="block-title">📖 Teoria: Node.js e Express</div>
          <div class="block-desc">Event loop, módulos, Express.js, middlewares e estrutura de rotas REST.</div>
          <div class="links">
            <a class="link-chip" href="https://nodejs.org/en/learn/getting-started/introduction-to-nodejs" target="_blank">Node.js – Guia oficial</a>
            <a class="link-chip" href="https://www.youtube.com/watch?v=Oe421EPjeBE" target="_blank">Traversy – Node & Express Crash</a>
          </div>
        </div>
      </div>
      <div class="block block-break"><div class="block-time">09:30 – 09:45</div><div class="block-content"><div class="block-title">☕ Pausa</div></div></div>
      <div class="block">
        <div class="block-time">09:45 – 11:15</div>
        <div class="block-content">
          <div class="block-title">💻 Prática: API REST básica</div>
          <div class="block-desc">Crie endpoints GET, POST, PUT, DELETE para um recurso "tarefas" com dados em memória.</div>
        </div>
      </div>
      <div class="block block-break"><div class="block-time">11:15 – 11:30</div><div class="block-content"><div class="block-title">🧘 Pausa</div></div></div>
      <div class="block">
        <div class="block-time">11:30 – 12:00</div>
        <div class="block-content">
          <div class="block-title">📝 Testar com Insomnia</div>
          <div class="block-desc">Instale o Insomnia e teste todos os endpoints da sua API.</div>
          <div class="links"><a class="link-chip" href="https://insomnia.rest/download" target="_blank">Insomnia – Download</a></div>
        </div>
      </div>
    </div>
  </div>

  <div class="day-card">
    <div class="day-header" onclick="toggle(this)">
      <div class="day-dot c-purple">TER</div>
      <div class="day-info"><div class="day-name">Terça-feira</div><div class="day-theme">Banco de Dados – PostgreSQL + Prisma</div></div>
      <div class="day-toggle">›</div>
    </div>
    <div class="day-body">
      <div class="block">
        <div class="block-time">08:00 – 09:30</div>
        <div class="block-content">
          <div class="block-title">📖 Teoria: SQL e ORMs</div>
          <div class="block-desc">SQL básico (SELECT, INSERT, JOIN), modelagem relacional e Prisma ORM com PostgreSQL.</div>
          <div class="links">
            <a class="link-chip" href="https://www.prisma.io/docs/getting-started" target="_blank">Prisma – Getting Started</a>
            <a class="link-chip" href="https://sqlbolt.com/" target="_blank">SQLBolt – SQL interativo</a>
          </div>
        </div>
      </div>
      <div class="block block-break"><div class="block-time">09:30 – 09:45</div><div class="block-content"><div class="block-title">☕ Pausa</div></div></div>
      <div class="block">
        <div class="block-time">09:45 – 11:15</div>
        <div class="block-content">
          <div class="block-title">💻 Prática: Conectar API ao BD</div>
          <div class="block-desc">Substitua os dados em memória da API por um banco PostgreSQL via Prisma. CRUD completo.</div>
          <div class="links"><a class="link-chip" href="https://railway.app/" target="_blank">Railway – PostgreSQL grátis</a></div>
        </div>
      </div>
      <div class="block block-break"><div class="block-time">11:15 – 11:30</div><div class="block-content"><div class="block-title">🧘 Pausa</div></div></div>
      <div class="block">
        <div class="block-time">11:30 – 12:00</div>
        <div class="block-content">
          <div class="block-title">📝 Migrations e Seed</div>
          <div class="block-desc">Crie migrations com Prisma e popule o banco com dados iniciais via seed.</div>
        </div>
      </div>
    </div>
  </div>

  <div class="day-card">
    <div class="day-header" onclick="toggle(this)">
      <div class="day-dot c-green">QUA</div>
      <div class="day-info"><div class="day-name">Quarta-feira</div><div class="day-theme">Autenticação – JWT + bcrypt</div></div>
      <div class="day-toggle">›</div>
    </div>
    <div class="day-body">
      <div class="block">
        <div class="block-time">08:00 – 09:30</div>
        <div class="block-content">
          <div class="block-title">📖 Teoria: Auth com JWT</div>
          <div class="block-desc">Hashing de senhas com bcrypt, geração e validação de tokens JWT, middleware de autenticação.</div>
          <div class="links">
            <a class="link-chip" href="https://jwt.io/introduction" target="_blank">JWT.io – Introdução</a>
            <a class="link-chip" href="https://www.youtube.com/watch?v=mbsmsi7l3r4" target="_blank">Tutorial JWT Node.js</a>
          </div>
        </div>
      </div>
      <div class="block block-break"><div class="block-time">09:30 – 09:45</div><div class="block-content"><div class="block-title">☕ Pausa</div></div></div>
      <div class="block">
        <div class="block-time">09:45 – 11:30</div>
        <div class="block-content">
          <div class="block-title">💻 Prática: Registro e Login</div>
          <div class="block-desc">Adicione rotas de /register e /login à API, proteja rotas privadas com middleware JWT.</div>
        </div>
      </div>
      <div class="block block-break"><div class="block-time">11:30 – 11:45</div><div class="block-content"><div class="block-title">🧘 Pausa</div></div></div>
      <div class="block">
        <div class="block-time">11:45 – 12:00</div>
        <div class="block-content"><div class="block-title">📝 Revisão de segurança</div><div class="block-desc">Revise variáveis de ambiente com .env e boas práticas de segurança.</div></div>
      </div>
    </div>
  </div>

  <div class="day-card">
    <div class="day-header" onclick="toggle(this)">
      <div class="day-dot c-yellow">QUI</div>
      <div class="day-info"><div class="day-name">Quinta-feira</div><div class="day-theme">Upload de arquivos + Deploy back-end</div></div>
      <div class="day-toggle">›</div>
    </div>
    <div class="day-body">
      <div class="block">
        <div class="block-time">08:00 – 09:30</div>
        <div class="block-content">
          <div class="block-title">📖 Teoria: Uploads e Deploy</div>
          <div class="block-desc">Multer para upload de arquivos, variáveis de ambiente, deploy no Railway ou Render.</div>
          <div class="links">
            <a class="link-chip" href="https://render.com/" target="_blank">Render – Deploy grátis</a>
            <a class="link-chip" href="https://railway.app/" target="_blank">Railway</a>
          </div>
        </div>
      </div>
      <div class="block block-break"><div class="block-time">09:30 – 09:45</div><div class="block-content"><div class="block-title">☕ Pausa</div></div></div>
      <div class="block">
        <div class="block-time">09:45 – 11:30</div>
        <div class="block-content">
          <div class="block-title">💻 Deploy da API</div>
          <div class="block-desc">Publique a API no Render, conecte ao banco em produção e teste com Insomnia.</div>
        </div>
      </div>
      <div class="block block-break"><div class="block-time">11:30 – 11:45</div><div class="block-content"><div class="block-title">🧘 Pausa</div></div></div>
      <div class="block">
        <div class="block-time">11:45 – 12:00</div>
        <div class="block-content"><div class="block-title">📝 Documentação com Swagger</div>
          <div class="block-desc">Instale swagger-ui-express e documente os principais endpoints da sua API.</div>
          <div class="links"><a class="link-chip" href="https://swagger.io/docs/" target="_blank">Swagger Docs</a></div>
        </div>
      </div>
    </div>
  </div>

  <div class="day-card">
    <div class="day-header" onclick="toggle(this)">
      <div class="day-dot c-red">SEX</div>
      <div class="day-info"><div class="day-name">Sexta-feira</div><div class="day-theme">Projeto: API de Blog completa</div></div>
      <div class="day-toggle">›</div>
    </div>
    <div class="day-body">
      <div class="block">
        <div class="block-time">08:00 – 11:30</div>
        <div class="block-content">
          <div class="block-title">🏗️ Projeto: REST API de Blog</div>
          <div class="block-desc">API com usuários, posts, comentários, autenticação JWT, deploy no Railway/Render. CRUD completo com Prisma + PostgreSQL.</div>
        </div>
      </div>
      <div class="block block-break"><div class="block-time">11:30 – 11:45</div><div class="block-content"><div class="block-title">🧘 Pausa</div></div></div>
      <div class="block">
        <div class="block-time">11:45 – 12:00</div>
        <div class="block-content"><div class="block-title">📌 Retrospectiva</div><div class="block-desc">Commite tudo, atualize o README e compartilhe no LinkedIn.</div></div>
      </div>
    </div>
  </div>

  <div class="tip">
    <strong>💡 Meta das semanas 7–9:</strong> criar APIs REST completas com Node.js, banco de dados relacional, autenticação e deploy. Você deve ter uma API funcional e pública no ar.
  </div>
</div>


<!-- ═══════════════════════════════════════ SEMANA 10–12 -->
<div class="week-panel" id="week-4">
  <p class="week-title">Fase 4 — <span>Semanas 10 a 12</span> · Integração Full-Stack + Portfólio</p>

  <div class="day-card open">
    <div class="day-header" onclick="toggle(this)">
      <div class="day-dot c-blue">SEG</div>
      <div class="day-info"><div class="day-name">Segunda-feira</div><div class="day-theme">Conectar React com a sua API</div></div>
      <div class="day-toggle">›</div>
    </div>
    <div class="day-body">
      <div class="block">
        <div class="block-time">08:00 – 09:30</div>
        <div class="block-content">
          <div class="block-title">📖 Teoria: Axios + React Query</div>
          <div class="block-desc">Axios para requisições, React Query (TanStack Query) para cache, loading states e sincronização de dados.</div>
          <div class="links">
            <a class="link-chip" href="https://tanstack.com/query/latest/docs/framework/react/overview" target="_blank">TanStack Query Docs</a>
            <a class="link-chip" href="https://axios-http.com/docs/intro" target="_blank">Axios – Intro</a>
          </div>
        </div>
      </div>
      <div class="block block-break"><div class="block-time">09:30 – 09:45</div><div class="block-content"><div class="block-title">☕ Pausa</div></div></div>
      <div class="block">
        <div class="block-time">09:45 – 11:30</div>
        <div class="block-content">
          <div class="block-title">💻 Prática: Front conectado ao back</div>
          <div class="block-desc">Conecte o front-end React à API de blog: listar, criar e deletar posts com autenticação.</div>
        </div>
      </div>
      <div class="block block-break"><div class="block-time">11:30 – 11:45</div><div class="block-content"><div class="block-title">🧘 Pausa</div></div></div>
      <div class="block">
        <div class="block-time">11:45 – 12:00</div>
        <div class="block-content"><div class="block-title">📝 CORS e variáveis de ambiente</div><div class="block-desc">Configure CORS no Express e .env no React para apontar para a API em produção.</div></div>
      </div>
    </div>
  </div>

  <div class="day-card">
    <div class="day-header" onclick="toggle(this)">
      <div class="day-dot c-purple">TER</div>
      <div class="day-info"><div class="day-name">Terça-feira</div><div class="day-theme">Gerenciamento de estado global – Zustand</div></div>
      <div class="day-toggle">›</div>
    </div>
    <div class="day-body">
      <div class="block">
        <div class="block-time">08:00 – 09:30</div>
        <div class="block-content">
          <div class="block-title">📖 Teoria: Zustand</div>
          <div class="block-desc">Estado global com Zustand: stores, actions e persistência. Comparação com Context API.</div>
          <div class="links"><a class="link-chip" href="https://zustand-demo.pmnd.rs/" target="_blank">Zustand – Docs</a></div>
        </div>
      </div>
      <div class="block block-break"><div class="block-time">09:30 – 09:45</div><div class="block-content"><div class="block-title">☕ Pausa</div></div></div>
      <div class="block">
        <div class="block-time">09:45 – 11:30</div>
        <div class="block-content">
          <div class="block-title">💻 Prática: Auth store</div>
          <div class="block-desc">Implemente um store de autenticação com Zustand: login, logout e persistência do token.</div>
        </div>
      </div>
      <div class="block block-break"><div class="block-time">11:30 – 11:45</div><div class="block-content"><div class="block-title">🧘 Pausa</div></div></div>
      <div class="block">
        <div class="block-time">11:45 – 12:00</div>
        <div class="block-content"><div class="block-title">📝 Testes com Vitest</div>
          <div class="block-desc">Introdução a testes unitários com Vitest e Testing Library.</div>
          <div class="links"><a class="link-chip" href="https://vitest.dev/" target="_blank">Vitest Docs</a></div>
        </div>
      </div>
    </div>
  </div>

  <div class="day-card">
    <div class="day-header" onclick="toggle(this)">
      <div class="day-dot c-green">QUA</div>
      <div class="day-info"><div class="day-name">Quarta-feira</div><div class="day-theme">Next.js – SSR e Full-Stack em um só</div></div>
      <div class="day-toggle">›</div>
    </div>
    <div class="day-body">
      <div class="block">
        <div class="block-time">08:00 – 09:30</div>
        <div class="block-content">
          <div class="block-title">📖 Teoria: Next.js App Router</div>
          <div class="block-desc">SSR, SSG, Server Components, Server Actions e Route Handlers no Next.js 14+.</div>
          <div class="links">
            <a class="link-chip" href="https://nextjs.org/docs" target="_blank">Next.js – Docs</a>
            <a class="link-chip" href="https://www.youtube.com/watch?v=ZVnjOPwW4ZA" target="_blank">Vercel – Next.js 14 Tutorial</a>
          </div>
        </div>
      </div>
      <div class="block block-break"><div class="block-time">09:30 – 09:45</div><div class="block-content"><div class="block-title">☕ Pausa</div></div></div>
      <div class="block">
        <div class="block-time">09:45 – 11:30</div>
        <div class="block-content">
          <div class="block-title">💻 Prática: Migrar projeto para Next.js</div>
          <div class="block-desc">Converta o front-end do blog para Next.js com SSR, melhorando SEO e performance.</div>
        </div>
      </div>
      <div class="block block-break"><div class="block-time">11:30 – 11:45</div><div class="block-content"><div class="block-title">🧘 Pausa</div></div></div>
      <div class="block">
        <div class="block-time">11:45 – 12:00</div>
        <div class="block-content"><div class="block-title">📝 Deploy no Vercel</div><div class="block-desc">Deploy automático via GitHub no Vercel com preview por branch.</div></div>
      </div>
    </div>
  </div>

  <div class="day-card">
    <div class="day-header" onclick="toggle(this)">
      <div class="day-dot c-yellow">QUI</div>
      <div class="day-info"><div class="day-name">Quinta-feira</div><div class="day-theme">Portfólio – Projeto Final</div></div>
      <div class="day-toggle">›</div>
    </div>
    <div class="day-body">
      <div class="block">
        <div class="block-time">08:00 – 11:30</div>
        <div class="block-content">
          <div class="block-title">🏗️ Projeto Final: escolha um</div>
          <div class="block-desc">Desenvolva um projeto full-stack do zero: <strong>E-commerce</strong> (produtos, carrinho, checkout), <strong>App de finanças</strong> (receitas, despesas, gráficos) ou <strong>Rede social simples</strong> (posts, seguir, feed). Next.js + Prisma + PostgreSQL.</div>
          <div class="links">
            <a class="link-chip" href="https://roadmap.sh/full-stack" target="_blank">Roadmap Full-Stack</a>
          </div>
        </div>
      </div>
      <div class="block block-break"><div class="block-time">11:30 – 11:45</div><div class="block-content"><div class="block-title">🧘 Pausa</div></div></div>
      <div class="block">
        <div class="block-time">11:45 – 12:00</div>
        <div class="block-content"><div class="block-title">📝 Planejamento do projeto</div><div class="block-desc">Wireframes, definição de rotas, modelagem do banco e criação do repositório.</div></div>
      </div>
    </div>
  </div>

  <div class="day-card">
    <div class="day-header" onclick="toggle(this)">
      <div class="day-dot c-red">SEX</div>
      <div class="day-info"><div class="day-name">Sexta-feira</div><div class="day-theme">Portfólio + LinkedIn + Próximos passos</div></div>
      <div class="day-toggle">›</div>
    </div>
    <div class="day-body">
      <div class="block">
        <div class="block-time">08:00 – 10:00</div>
        <div class="block-content">
          <div class="block-title">🌐 Portfólio pessoal</div>
          <div class="block-desc">Crie seu portfólio com Next.js mostrando todos os projetos, habilidades e contato. Deploy no Vercel.</div>
          <div class="links">
            <a class="link-chip" href="https://brittanychiang.com/" target="_blank">Referência de portfólio</a>
          </div>
        </div>
      </div>
      <div class="block block-break"><div class="block-time">10:00 – 10:15</div><div class="block-content"><div class="block-title">☕ Pausa</div></div></div>
      <div class="block">
        <div class="block-time">10:15 – 11:15</div>
        <div class="block-content">
          <div class="block-title">💼 LinkedIn + Currículo</div>
          <div class="block-desc">Atualize seu LinkedIn com os projetos, habilidades técnicas e destaque o portfólio.</div>
        </div>
      </div>
      <div class="block block-break"><div class="block-time">11:15 – 11:30</div><div class="block-content"><div class="block-title">🧘 Pausa</div></div></div>
      <div class="block">
        <div class="block-time">11:30 – 12:00</div>
        <div class="block-content">
          <div class="block-title">🎯 Próximos passos</div>
          <div class="block-desc">Defina o que estudar a seguir: Docker, CI/CD, AWS, testes avançados ou especialização em uma stack.</div>
          <div class="links">
            <a class="link-chip" href="https://roadmap.sh/" target="_blank">Roadmap.sh – Todos os caminhos</a>
          </div>
        </div>
      </div>
    </div>
  </div>

  <div class="tip">
    <strong>🎉 Meta das semanas 10–12:</strong> ter uma aplicação full-stack completa em produção, portfólio online e LinkedIn atualizado. Você estará pronto para aplicar às primeiras vagas júnior.
  </div>
</div>

<script>
  function toggle(header) {
    const card = header.closest('.day-card');
    card.classList.toggle('open');
  }

  function showWeek(num, btn) {
    document.querySelectorAll('.week-panel').forEach(p => p.classList.remove('active'));
    document.querySelectorAll('.week-btn').forEach(b => b.classList.remove('active'));
    document.getElementById('week-' + num).classList.add('active');
    btn.classList.add('active');
  }
</script>
</body>
</html>
