<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0"/>
<title>Weather</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;500;600;700;800&family=DM+Mono:wght@300;400;500&display=swap" rel="stylesheet"/>
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --bg: #0b0f1a;
    --bg2: #111827;
    --card: #141c2e;
    --border: rgba(255,255,255,0.06);
    --accent: #4f9cf9;
    --accent2: #7dd3fc;
    --warm: #fb923c;
    --text: #f1f5f9;
    --muted: #64748b;
    --sub: #94a3b8;
  }

  body {
    font-family: 'Syne', sans-serif;
    background: var(--bg);
    color: var(--text);
    min-height: 100vh;
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 2rem 1rem 4rem;
    overflow-x: hidden;
  }

  /* Ambient glow background */
  body::before {
    content: '';
    position: fixed;
    top: -200px; left: 50%;
    transform: translateX(-50%);
    width: 700px; height: 500px;
    background: radial-gradient(ellipse, rgba(79,156,249,0.12) 0%, transparent 70%);
    pointer-events: none;
    z-index: 0;
  }

  .wrapper {
    width: 100%;
    max-width: 560px;
    position: relative;
    z-index: 1;
  }

  /* Header */
  .header {
    margin-bottom: 2.5rem;
    text-align: center;
  }
  .header h1 {
    font-size: clamp(2rem, 6vw, 2.8rem);
    font-weight: 800;
    letter-spacing: -0.03em;
    color: var(--text);
  }
  .header h1 span { color: var(--accent); }
  .header p {
    font-family: 'DM Mono', monospace;
    font-size: 0.72rem;
    color: var(--muted);
    letter-spacing: 0.12em;
    text-transform: uppercase;
    margin-top: 0.4rem;
  }

  /* Search */
  .search-wrap {
    display: flex;
    gap: 0.5rem;
    margin-bottom: 2rem;
  }
  .search-wrap input {
    flex: 1;
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 14px;
    padding: 0.85rem 1.2rem;
    font-family: 'Syne', sans-serif;
    font-size: 1rem;
    color: var(--text);
    outline: none;
    transition: border-color 0.2s, box-shadow 0.2s;
  }
  .search-wrap input::placeholder { color: var(--muted); }
  .search-wrap input:focus {
    border-color: var(--accent);
    box-shadow: 0 0 0 3px rgba(79,156,249,0.12);
  }
  .search-wrap button {
    background: var(--accent);
    border: none;
    border-radius: 14px;
    padding: 0.85rem 1.4rem;
    font-family: 'Syne', sans-serif;
    font-weight: 700;
    font-size: 0.9rem;
    color: #fff;
    cursor: pointer;
    transition: opacity 0.2s, transform 0.1s;
    white-space: nowrap;
  }
  .search-wrap button:hover { opacity: 0.9; }
  .search-wrap button:active { transform: scale(0.97); }

  /* Error */
  .error-msg {
    background: rgba(239,68,68,0.1);
    border: 1px solid rgba(239,68,68,0.3);
    border-radius: 12px;
    padding: 0.75rem 1rem;
    font-size: 0.85rem;
    color: #fca5a5;
    margin-bottom: 1.5rem;
    display: none;
  }
  .error-msg.show { display: block; }

  /* Main card */
  .card {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 24px;
    padding: 2rem;
    margin-bottom: 1rem;
    opacity: 0;
    transform: translateY(20px);
    transition: opacity 0.5s ease, transform 0.5s ease;
    display: none;
  }
  .card.visible {
    opacity: 1;
    transform: translateY(0);
    display: block;
  }

  .city-row {
    display: flex;
    align-items: flex-start;
    justify-content: space-between;
    gap: 1rem;
    margin-bottom: 1.5rem;
  }
  .city-name {
    font-size: clamp(1.6rem, 5vw, 2.2rem);
    font-weight: 800;
    letter-spacing: -0.03em;
    line-height: 1.1;
  }
  .country-code {
    font-family: 'DM Mono', monospace;
    font-size: 0.75rem;
    color: var(--muted);
    letter-spacing: 0.1em;
    text-transform: uppercase;
    margin-top: 4px;
  }
  .weather-icon-wrap {
    width: 72px; height: 72px;
    background: rgba(79,156,249,0.08);
    border-radius: 50%;
    display: flex; align-items: center; justify-content: center;
    flex-shrink: 0;
  }
  .weather-icon-wrap img { width: 52px; height: 52px; }

  .temp-row {
    display: flex;
    align-items: baseline;
    gap: 0.4rem;
    margin-bottom: 0.4rem;
  }
  .temp-main {
    font-size: clamp(3.5rem, 12vw, 5rem);
    font-weight: 800;
    letter-spacing: -0.04em;
    line-height: 1;
    color: var(--text);
  }
  .temp-unit {
    font-size: 1.6rem;
    font-weight: 400;
    color: var(--muted);
  }
  .feels-like {
    font-family: 'DM Mono', monospace;
    font-size: 0.75rem;
    color: var(--sub);
    margin-bottom: 0.5rem;
  }
  .description {
    font-size: 1.05rem;
    font-weight: 600;
    color: var(--accent2);
    text-transform: capitalize;
    margin-bottom: 1.8rem;
    letter-spacing: -0.01em;
  }

  /* Stats grid */
  .stats {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 0.75rem;
  }
  .stat {
    background: rgba(255,255,255,0.03);
    border: 1px solid var(--border);
    border-radius: 14px;
    padding: 1rem 1.1rem;
  }
  .stat-label {
    font-family: 'DM Mono', monospace;
    font-size: 0.65rem;
    color: var(--muted);
    letter-spacing: 0.1em;
    text-transform: uppercase;
    margin-bottom: 0.4rem;
    display: flex;
    align-items: center;
    gap: 6px;
  }
  .stat-label svg { opacity: 0.6; }
  .stat-value {
    font-size: 1.3rem;
    font-weight: 700;
    letter-spacing: -0.02em;
    color: var(--text);
  }
  .stat-value span {
    font-size: 0.85rem;
    font-weight: 400;
    color: var(--sub);
  }

  /* Humidity bar */
  .hum-bar {
    height: 3px;
    background: rgba(255,255,255,0.08);
    border-radius: 99px;
    margin-top: 6px;
    overflow: hidden;
  }
  .hum-fill {
    height: 100%;
    background: var(--accent);
    border-radius: 99px;
    transition: width 0.8s ease;
  }

  /* Divider / time */
  .time-row {
    font-family: 'DM Mono', monospace;
    font-size: 0.7rem;
    color: var(--muted);
    letter-spacing: 0.1em;
    text-transform: uppercase;
    text-align: right;
    margin-top: 1.5rem;
    padding-top: 1rem;
    border-top: 1px solid var(--border);
  }

  /* Skeleton loader */
  .skeleton { display: none; }
  .skeleton.show { display: block; }
  .skel-card {
    background: var(--card);
    border: 1px solid var(--border);
    border-radius: 24px;
    padding: 2rem;
  }
  .skel-line {
    background: linear-gradient(90deg, rgba(255,255,255,0.04) 0%, rgba(255,255,255,0.09) 50%, rgba(255,255,255,0.04) 100%);
    background-size: 200% 100%;
    animation: shimmer 1.4s infinite;
    border-radius: 8px;
  }
  @keyframes shimmer {
    0% { background-position: -200% 0; }
    100% { background-position: 200% 0; }
  }

  /* Responsive */
  @media (max-width: 400px) {
    .card { padding: 1.4rem; }
    .stats { grid-template-columns: 1fr 1fr; }
  }
</style>
</head>
<body>
<div class="wrapper">

  <div class="header">
    <h1>Sky<span>Cast</span></h1>
    <p>Live weather · Anywhere on Earth</p>
  </div>

  <div class="search-wrap">
    <input type="text" id="cityInput" placeholder="Search city…" autocomplete="off" />
    <button id="searchBtn">Search</button>
  </div>

  <div class="error-msg" id="errorMsg"></div>

  <!-- Skeleton loader -->
  <div class="skeleton" id="skeleton">
    <div class="skel-card">
      <div class="skel-line" style="height:28px;width:55%;margin-bottom:12px;"></div>
      <div class="skel-line" style="height:80px;width:40%;margin-bottom:16px;"></div>
      <div class="skel-line" style="height:18px;width:30%;margin-bottom:24px;"></div>
      <div style="display:grid;grid-template-columns:1fr 1fr;gap:12px;">
        <div class="skel-line" style="height:72px;border-radius:14px;"></div>
        <div class="skel-line" style="height:72px;border-radius:14px;"></div>
        <div class="skel-line" style="height:72px;border-radius:14px;"></div>
        <div class="skel-line" style="height:72px;border-radius:14px;"></div>
      </div>
    </div>
  </div>

  <!-- Main card -->
  <div class="card" id="weatherCard">
    <div class="city-row">
      <div>
        <div class="city-name" id="cityName">—</div>
        <div class="country-code" id="countryCode">—</div>
      </div>
      <div class="weather-icon-wrap">
        <img id="weatherIcon" src="" alt="icon" />
      </div>
    </div>

    <div class="temp-row">
      <div class="temp-main" id="tempMain">—</div>
      <div class="temp-unit">°C</div>
    </div>
    <div class="feels-like" id="feelsLike">Feels like —°C</div>
    <div class="description" id="description">—</div>

    <div class="stats">
      <div class="stat">
        <div class="stat-label">
          <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M12 2a7 7 0 0 1 7 7c0 5-7 13-7 13S5 14 5 9a7 7 0 0 1 7-7z"/><circle cx="12" cy="9" r="2.5"/></svg>
          Humidity
        </div>
        <div class="stat-value" id="humidity">—<span>%</span></div>
        <div class="hum-bar"><div class="hum-fill" id="humFill" style="width:0%"></div></div>
      </div>
      <div class="stat">
        <div class="stat-label">
          <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M9.59 4.59A2 2 0 1 1 11 8H2m10.59 11.41A2 2 0 1 0 14 16H2m15.73-8.27A2.5 2.5 0 1 1 19.5 12H2"/></svg>
          Wind
        </div>
        <div class="stat-value" id="wind">—<span> km/h</span></div>
      </div>
      <div class="stat">
        <div class="stat-label">
          <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M2 12h20M12 2v20"/></svg>
          Pressure
        </div>
        <div class="stat-value" id="pressure">—<span> hPa</span></div>
      </div>
      <div class="stat">
        <div class="stat-label">
          <svg width="12" height="12" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><circle cx="12" cy="12" r="5"/><line x1="12" y1="1" x2="12" y2="3"/><line x1="12" y1="21" x2="12" y2="23"/><line x1="4.22" y1="4.22" x2="5.64" y2="5.64"/><line x1="18.36" y1="18.36" x2="19.78" y2="19.78"/><line x1="1" y1="12" x2="3" y2="12"/><line x1="21" y1="12" x2="23" y2="12"/></svg>
          Visibility
        </div>
        <div class="stat-value" id="visibility">—<span> km</span></div>
      </div>
    </div>

    <div class="time-row" id="timeRow">—</div>
  </div>

</div>

<script>
  const API_KEY = 'dfc573300980b5488e5729d92dc48226';

  const input = document.getElementById('cityInput');
  const btn = document.getElementById('searchBtn');
  const errorMsg = document.getElementById('errorMsg');
  const skeleton = document.getElementById('skeleton');
  const card = document.getElementById('weatherCard');

  function showError(msg) {
    errorMsg.textContent = msg;
    errorMsg.classList.add('show');
  }
  function hideError() {
    errorMsg.classList.remove('show');
  }
  function showSkeleton() {
    skeleton.classList.add('show');
    card.classList.remove('visible');
    card.style.display = 'none';
  }
  function hideSkeleton() {
    skeleton.classList.remove('show');
  }

  async function fetchWeather(city) {
    if (!city.trim()) return;
    hideError();
    showSkeleton();

    const url = `https://api.openweathermap.org/data/2.5/weather?q=${encodeURIComponent(city)}&appid=${API_KEY}&units=metric`;

    try {
      const res = await fetch(url);
      if (!res.ok) {
        if (res.status === 404) throw new Error(`City "${city}" not found. Try another name.`);
        if (res.status === 401) throw new Error('Invalid API key. Please check your OpenWeatherMap key.');
        throw new Error(`Error ${res.status}: Could not fetch weather.`);
      }

      const data = await res.json();
      renderWeather(data);
    } catch (err) {
      hideSkeleton();
      showError(err.message);
    }
  }

  function renderWeather(d) {
    hideSkeleton();

    document.getElementById('cityName').textContent = d.name;
    document.getElementById('countryCode').textContent = `${d.sys.country} · ${d.coord.lat.toFixed(2)}°N, ${d.coord.lon.toFixed(2)}°E`;
    document.getElementById('tempMain').textContent = Math.round(d.main.temp);
    document.getElementById('feelsLike').textContent = `Feels like ${Math.round(d.main.feels_like)}°C · ${d.main.temp_min.toFixed(1)}° / ${d.main.temp_max.toFixed(1)}°`;
    document.getElementById('description').textContent = d.weather[0].description;

    const iconCode = d.weather[0].icon;
    document.getElementById('weatherIcon').src = `https://openweathermap.org/img/wn/${iconCode}@2x.png`;
    document.getElementById('weatherIcon').alt = d.weather[0].description;

    const hum = d.main.humidity;
    document.getElementById('humidity').innerHTML = `${hum}<span>%</span>`;
    document.getElementById('humFill').style.width = hum + '%';

    document.getElementById('wind').innerHTML = `${Math.round(d.wind.speed * 3.6)}<span> km/h</span>`;
    document.getElementById('pressure').innerHTML = `${d.main.pressure}<span> hPa</span>`;
    document.getElementById('visibility').innerHTML = `${(d.visibility / 1000).toFixed(1)}<span> km</span>`;

    const now = new Date();
    document.getElementById('timeRow').textContent =
      `Updated ${now.toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })} · ${now.toLocaleDateString([], { weekday: 'long', month: 'short', day: 'numeric' })}`;

    card.style.display = 'block';
    requestAnimationFrame(() => card.classList.add('visible'));
  }

  btn.addEventListener('click', () => fetchWeather(input.value));
  input.addEventListener('keydown', e => { if (e.key === 'Enter') fetchWeather(input.value); });

  // Demo on load
  window.addEventListener('load', () => fetchWeather('Karachi'));
</script>
</body>
</html>
