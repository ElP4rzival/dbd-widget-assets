## Dead by Daylight Bit Goal Widget (StreamElements)

### HTML
```html
<div id="bbits-root" class="bbits hidden" data-visible="false">
  <div class="bbits-bar-wrap">
    <div class="bbits-bar">
      <div class="bbits-bar-fill">
        <div class="bbits-bar-glow"></div>
      </div>
      <div class="bbits-bar-overlay"></div>
      <div class="bbits-bar-effects"></div>
      <div class="bbits-bar-numbers">
        <span class="bbits-current">0</span>
        <span class="bbits-divider">/</span>
        <span class="bbits-goal">5,000</span>
        <span class="bbits-percent">0%</span>
      </div>
    </div>
  </div>
  <canvas id="bbits-particles" class="bbits-particles" aria-hidden="true"></canvas>
</div>
```

### CSS
```css
:root {
  --bbits-accent-red: #dc143c;
  --bbits-accent-white: #f5f5f5;
  --bbits-bg-dark: rgba(8, 8, 10, 0.65);
  --bbits-scale: 1;
  --bbits-entrance-ms: 650ms;
  --bbits-exit-ms: 550ms;
  --bbits-font: 'Rajdhani', 'Inter', system-ui, sans-serif;
}

#bbits-root {
  position: absolute;
  top: 3%;
  right: 3%;
  transform: scale(var(--bbits-scale));
  min-width: 360px;
  max-width: 520px;
  color: var(--bbits-accent-white);
  font-family: var(--bbits-font);
  letter-spacing: 0.03em;
  pointer-events: none;
  filter: drop-shadow(0 8px 24px rgba(0, 0, 0, 0.55));
  background: transparent;
}

#bbits-root.hidden {
  opacity: 0;
  transform: translateY(12px) scale(var(--bbits-scale));
  filter: drop-shadow(0 16px 30px rgba(0, 0, 0, 0));
}

.bbits {
  transition: opacity var(--bbits-exit-ms) ease, transform var(--bbits-exit-ms) ease,
    filter var(--bbits-exit-ms) ease;
}

.bbits.visible {
  opacity: 1;
  transform: translateY(0) scale(var(--bbits-scale));
  filter: drop-shadow(0 10px 32px rgba(220, 20, 60, 0.35));
}

.bbits-bar-wrap {
  position: relative;
  overflow: visible;
}

.bbits-bar {
  position: relative;
  height: 44px;
  border-radius: 12px;
  background: linear-gradient(135deg, rgba(18, 18, 20, 0.9), rgba(10, 10, 12, 0.92));
  overflow: visible;
  box-shadow: 0 0 18px rgba(0, 0, 0, 0.65), 0 0 18px rgba(220, 20, 60, 0.22),
    inset 0 0 0 1px rgba(220, 20, 60, 0.45), inset 0 0 18px rgba(255, 255, 255, 0.12);
}

.bbits-bar::before {
  content: '';
  position: absolute;
  inset: -6px;
  border-radius: 16px;
  border: 1px solid rgba(220, 20, 60, 0.35);
  box-shadow: 0 0 28px rgba(220, 20, 60, 0.28), 0 0 12px rgba(255, 255, 255, 0.18);
  opacity: 0.8;
  pointer-events: none;
}

.bbits-bar-fill {
  position: absolute;
  inset: 0;
  width: 0%;
  background: linear-gradient(90deg, rgba(220, 20, 60, 0.9), rgba(168, 22, 44, 0.95));
  box-shadow: inset 0 0 22px rgba(255, 255, 255, 0.24);
  transition: width 240ms ease;
  border-radius: 12px;
}

.bbits-bar-glow {
  position: absolute;
  inset: 0;
  background: radial-gradient(circle at 25% 50%, rgba(255, 255, 255, 0.28), transparent 42%),
    radial-gradient(circle at 80% 50%, rgba(255, 255, 255, 0.16), transparent 45%);
  opacity: 0.9;
  mix-blend-mode: screen;
}

.bbits-bar-overlay {
  position: absolute;
  inset: 0;
  background: linear-gradient(180deg, rgba(255, 255, 255, 0.08), rgba(0, 0, 0, 0.25));
  opacity: 0.38;
  pointer-events: none;
  mix-blend-mode: screen;
  border-radius: 12px;
}

.bbits-bar-effects {
  position: absolute;
  inset: -14px;
  pointer-events: none;
  border-radius: 16px;
  overflow: visible;
}

.bbits-bar-numbers {
  position: absolute;
  inset: 0;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 6px;
  font-weight: 700;
  text-shadow: 0 0 4px rgba(255, 255, 255, 0.9), 0 0 10px rgba(255, 255, 255, 0.7),
    0 0 18px rgba(255, 255, 255, 0.4), 0 0 28px rgba(220, 20, 60, 0.35);
}

.bbits-bar-numbers .bbits-divider {
  opacity: 0.78;
}

.bbits-bar-numbers .bbits-percent {
  margin-left: 6px;
  font-weight: 600;
  font-size: 14px;
  color: rgba(255, 255, 255, 0.9);
}

.bbits-particles {
  position: absolute;
  inset: -24px;
  width: calc(100% + 48px);
  height: calc(100% + 48px);
  pointer-events: none;
  opacity: 0;
  transition: opacity 200ms ease;
  mix-blend-mode: screen;
}

.bbits.visible.effect-glow-soft .bbits-bar-fill {
  box-shadow: inset 0 0 28px rgba(255, 255, 255, 0.22), 0 0 18px rgba(220, 20, 60, 0.32);
}

.bbits.visible.effect-pulse {
  animation: bbits-pulse 1.2s ease-out;
}

.bbits.visible.effect-shake {
  animation: bbits-shake 420ms ease-in-out;
}

.bbits.visible.effect-flash .bbits-bar-effects::after {
  content: '';
  position: absolute;
  inset: -10px;
  border-radius: 16px;
  background: radial-gradient(circle at center, rgba(255, 255, 255, 0.7), transparent 60%);
  opacity: 0;
  animation: bbits-flash 0.9s ease;
  mix-blend-mode: screen;
}

.bbits.visible.effect-vignette .bbits-bar-effects::before {
  content: '';
  position: absolute;
  inset: -20px;
  border-radius: 18px;
  background: radial-gradient(circle at center, rgba(0, 0, 0, 0.55), transparent 65%);
  opacity: 0;
  animation: bbits-vignette 1s ease;
}

.bbits.visible.effect-scanline .bbits-bar::after {
  content: '';
  position: absolute;
  inset: -4px;
  border-radius: 12px;
  background: linear-gradient(180deg, rgba(220, 20, 60, 0.55), transparent 70%);
  opacity: 0.75;
  mix-blend-mode: screen;
  animation: bbits-scanline 0.9s ease;
}

.bbits.visible.effect-scratch .bbits-bar-effects::before {
  content: '';
  position: absolute;
  inset: -10px;
  border-radius: 14px;
  background: repeating-linear-gradient(
      135deg,
      rgba(255, 255, 255, 0.08),
      rgba(255, 255, 255, 0.08) 12px,
      transparent 12px,
      transparent 24px
    ),
    repeating-linear-gradient(145deg, rgba(220, 20, 60, 0.18), rgba(220, 20, 60, 0.18) 10px, transparent 10px, transparent 24px);
  opacity: 0;
  animation: bbits-scratch 0.9s ease;
}

.bbits.visible.effect-bloodmist .bbits-bar-effects::after {
  content: '';
  position: absolute;
  inset: -20px;
  border-radius: 18px;
  background: radial-gradient(circle at 50% 50%, rgba(220, 20, 60, 0.45), transparent 70%);
  opacity: 0;
  animation: bbits-bloodmist 1.1s ease;
  mix-blend-mode: screen;
}

.bbits.visible.effect-claws .bbits-bar-effects::after {
  content: '';
  position: absolute;
  inset: -16px;
  border-radius: 16px;
  background: repeating-linear-gradient(
      130deg,
      rgba(255, 255, 255, 0.08),
      rgba(255, 255, 255, 0.08) 8px,
      transparent 8px,
      transparent 26px
    ),
    repeating-linear-gradient(140deg, rgba(220, 20, 60, 0.18), rgba(220, 20, 60, 0.18) 6px, transparent 6px, transparent 24px);
  opacity: 0;
  animation: bbits-claws 1s ease;
  mix-blend-mode: screen;
}

.bbits.visible.effect-cinematic .bbits-bar-effects::after {
  content: '';
  position: absolute;
  inset: -24px;
  border-radius: 18px;
  background: radial-gradient(circle at 45% 55%, rgba(255, 255, 255, 0.65), transparent 65%),
    radial-gradient(circle at 70% 45%, rgba(220, 20, 60, 0.55), transparent 70%);
  opacity: 0;
  animation: bbits-cinematic 1.2s ease;
  mix-blend-mode: screen;
}

.bbits.visible.effect-mori .bbits-bar-effects::before {
  content: '';
  position: absolute;
  inset: -26px;
  border-radius: 20px;
  background: radial-gradient(circle at 40% 60%, rgba(255, 255, 255, 0.6), transparent 55%),
    radial-gradient(circle at 70% 40%, rgba(220, 20, 60, 0.65), transparent 65%);
  opacity: 0;
  animation: bbits-mori 1.4s ease;
  mix-blend-mode: screen;
}

.bbits.visible.effect-mori .bbits-bar::before {
  box-shadow: 0 0 34px rgba(220, 20, 60, 0.45), 0 0 18px rgba(255, 255, 255, 0.3);
}

.bbits.visible.effect-glow-boost .bbits-bar-fill {
  box-shadow: inset 0 0 26px rgba(255, 255, 255, 0.26), 0 0 26px rgba(220, 20, 60, 0.4);
}

@keyframes bbits-pulse {
  0% {
    transform: translateY(0) scale(var(--bbits-scale));
  }
  40% {
    transform: translateY(-1px) scale(calc(var(--bbits-scale) * 1.01));
  }
  100% {
    transform: translateY(0) scale(var(--bbits-scale));
  }
}

@keyframes bbits-shake {
  0% {
    transform: translate(0, 0) scale(var(--bbits-scale));
  }
  25% {
    transform: translate(-2px, 2px) scale(var(--bbits-scale));
  }
  50% {
    transform: translate(2px, -2px) scale(var(--bbits-scale));
  }
  75% {
    transform: translate(-1px, 1px) scale(var(--bbits-scale));
  }
  100% {
    transform: translate(0, 0) scale(var(--bbits-scale));
  }
}

@keyframes bbits-flash {
  0% {
    opacity: 0.14;
  }
  35% {
    opacity: 0.6;
  }
  100% {
    opacity: 0;
  }
}

@keyframes bbits-vignette {
  0% {
    opacity: 0.38;
  }
  100% {
    opacity: 0;
  }
}

@keyframes bbits-scanline {
  0% {
    opacity: 0.9;
    transform: translateY(-100%);
  }
  100% {
    opacity: 0;
    transform: translateY(100%);
  }
}

@keyframes bbits-scratch {
  0% {
    opacity: 0.32;
    transform: translateY(0);
  }
  100% {
    opacity: 0;
    transform: translateY(-6px);
  }
}

@keyframes bbits-bloodmist {
  0% {
    opacity: 0.48;
    transform: scale(0.98);
  }
  100% {
    opacity: 0;
    transform: scale(1.08);
  }
}

@keyframes bbits-claws {
  0% {
    opacity: 0.4;
    transform: translate3d(0, 0, 0);
  }
  100% {
    opacity: 0;
    transform: translate3d(-8px, -6px, 0);
  }
}

@keyframes bbits-cinematic {
  0% {
    opacity: 0.48;
    transform: scale(0.96);
  }
  100% {
    opacity: 0;
    transform: scale(1.06);
  }
}

@keyframes bbits-mori {
  0% {
    opacity: 0.65;
    transform: scale(0.95);
  }
  100% {
    opacity: 0;
    transform: scale(1.08);
  }
}

#bbits-root.reduced-motion *,
#bbits-root.reduced-motion .bbits-bar::before,
#bbits-root.reduced-motion .bbits-bar-effects::before,
#bbits-root.reduced-motion .bbits-bar-effects::after {
  animation-duration: 0.001ms !important;
  animation-iteration-count: 1 !important;
  transition-duration: 0.001ms !important;
}

@media (max-width: 520px) {
  #bbits-root {
    left: 4%;
    right: 4%;
    top: 2%;
    min-width: unset;
    max-width: unset;
  }

  .bbits-bar-numbers {
    font-size: 14px;
  }
}
```

### JavaScript
```js
/**
 * Runbook (StreamElements preview):
 * 1) Usa el botón "simulate event" y elige "Cheer" para probar rangos de bits.
 * 2) Comandos en chat (solo broadcaster/mods): !bbits, !bbits show, hide, status, reset, setgoal 5000, setcurrent 1200, add 100.
 * 3) Reset rápido: !bbits reset limpia el progreso local y reinicia la barra.
 * 4) Cambiar goal manual: !bbits setgoal 9000 (o usa current/goal en panel de settings del widget).
 *
 * Notas de modo auto: StreamElements no expone historial de bits; el modo auto suma solo los cheers recibidos mientras el overlay está activo. Persistencia localStorage guarda progreso entre recargas si persistProgress=true.
 *
 * Tabla de tiers -> efecto aplicado:
 * t0 (1-9): pulse suave + microglow
 * t1 (10-24): pulse + scanline rojo rápido
 * t2 (25-49): scratch-flash + pulse
 * t3 (50-99): shake breve + scratch + scanline
 * t4 (100-249): flash blanco controlado + vignette breve
 * t5 (250-499): blood mist burst + shake
 * t6 (500-999): claws streaks + partículas low
 * t7 (1000-1499): cinematic flash + partículas + glow boost
 * max (1500+): mori effect (flash + afterimage) + partículas high + shake suave
 */

(function () {
  const root = document.getElementById('bbits-root');
  const fill = root.querySelector('.bbits-bar-fill');
  const currentEl = root.querySelector('.bbits-current');
  const goalEl = root.querySelector('.bbits-goal');
  const percentEl = root.querySelector('.bbits-percent');
  const particleCanvas = /** @type {HTMLCanvasElement} */ (root.querySelector('#bbits-particles'));

  const defaultSettings = {
    goalBits: 5000,
    currentBitsInitial: 0,
    persistProgress: true,
    autoCycleEnabled: true,
    autoShowEveryMinutes: 5,
    autoVisibleSeconds: 30,
    donationVisibleSeconds: 60,
    entranceDurationMs: 650,
    exitDurationMs: 550,
    particleMode: 'low', // off | low | high
    reducedMotion: false,
    useExternalFonts: false,
    colors: {
      accentRed: '#dc143c',
      accentWhite: '#f5f5f5',
      bgDark: 'rgba(8,8,10,0.65)',
    },
    position: 'top-right',
    offsetX: 3,
    offsetY: 3,
    scale: 1,
    titleText: '',
    showNumbers: true,
    showPercent: true,
    numberFormatLocale: 'en-US',
    debugMode: false,
    allowMods: true,
    allowBroadcaster: true,
    modWhitelist: '',
    allowViewerCommands: false,
  };

  const state = {
    settings: { ...defaultSettings },
    current: 0,
    goal: defaultSettings.goalBits,
    visible: false,
    animFrame: null,
    hideTimer: null,
    cycleTimer: null,
    cycleHideTimer: null,
    lastPercent: 0,
    ready: false,
  };

  const storageKey = 'bbits-progress-v1';

  const clamp = (v, min, max) => Math.min(Math.max(v, min), max);
  const fmt = (n) => Number(n || 0).toLocaleString(state.settings.numberFormatLocale || 'en-US');

  function log(...args) {
    if (state.settings.debugMode) console.log('[bbits]', ...args);
  }

  function applySettings() {
    const s = state.settings;
    document.documentElement.style.setProperty('--bbits-accent-red', s.colors.accentRed || defaultSettings.colors.accentRed);
    document.documentElement.style.setProperty('--bbits-accent-white', s.colors.accentWhite || defaultSettings.colors.accentWhite);
    document.documentElement.style.setProperty('--bbits-bg-dark', s.colors.bgDark || defaultSettings.colors.bgDark);
    document.documentElement.style.setProperty('--bbits-scale', s.scale || 1);
    document.documentElement.style.setProperty('--bbits-entrance-ms', `${s.entranceDurationMs || 650}ms`);
    document.documentElement.style.setProperty('--bbits-exit-ms', `${s.exitDurationMs || 550}ms`);
    root.classList.toggle('reduced-motion', Boolean(s.reducedMotion));
    root.style.left = root.style.right = root.style.top = root.style.bottom = 'auto';
    const offset = `${s.position?.includes('bottom') ? s.offsetY || 3 : s.offsetY || 3}%`;
    const offsetX = `${s.offsetX || 3}%`;
    switch (s.position) {
      case 'top-left':
        root.style.top = offset;
        root.style.left = offsetX;
        break;
      case 'top-right':
        root.style.top = offset;
        root.style.right = offsetX;
        break;
      case 'bottom-left':
        root.style.bottom = offset;
        root.style.left = offsetX;
        break;
      case 'bottom-right':
      default:
        root.style.bottom = offset;
        root.style.right = offsetX;
        break;
    }

    if (s.useExternalFonts) {
      const linkId = 'bbits-font-link';
      if (!document.getElementById(linkId)) {
        const link = document.createElement('link');
        link.id = linkId;
        link.rel = 'stylesheet';
        link.href = 'https://fonts.googleapis.com/css2?family=Rajdhani:wght@600;700&family=Inter:wght@600;700&display=swap';
        document.head.appendChild(link);
      }
    }
  }

  function persist() {
    if (!state.settings.persistProgress) return;
    try {
      const payload = { current: state.current, goal: state.goal };
      localStorage.setItem(storageKey, JSON.stringify(payload));
    } catch (err) {
      log('persist failed', err);
    }
  }

  function restore() {
    if (!state.settings.persistProgress) return;
    try {
      const raw = localStorage.getItem(storageKey);
      if (raw) {
        const data = JSON.parse(raw);
        if (Number.isFinite(data.current)) state.current = data.current;
        if (Number.isFinite(data.goal)) state.goal = data.goal;
      }
    } catch (err) {
      log('restore failed', err);
    }
  }

  function updateNumbers() {
    currentEl.textContent = fmt(state.current);
    goalEl.textContent = fmt(state.goal);
    const pct = state.goal > 0 ? clamp((state.current / state.goal) * 100, 0, 9999) : 0;
    percentEl.textContent = state.settings.showPercent === false ? '' : `${Math.floor(pct)}%`;
    fill.style.width = `${clamp(pct, 0, 100)}%`;
    state.lastPercent = pct;
  }

  function tweenProgress(target) {
    cancelAnimationFrame(state.animFrame);
    const start = state.current;
    const delta = target - start;
    const duration = 480;
    const startTime = performance.now();
    const ease = (t) => 1 - Math.pow(1 - t, 3);

    const step = (now) => {
      const elapsed = now - startTime;
      const t = clamp(elapsed / duration, 0, 1);
      state.current = start + delta * ease(t);
      updateNumbers();
      if (t < 1) state.animFrame = requestAnimationFrame(step);
      else persist();
    };

    state.animFrame = requestAnimationFrame(step);
  }

  function setGoal(goal) {
    state.goal = Math.max(1, Number(goal) || defaultSettings.goalBits);
    updateNumbers();
    persist();
  }

  function setCurrent(value) {
    state.current = Math.max(0, Number(value) || 0);
    updateNumbers();
    persist();
  }

  function clearTimers() {
    if (state.hideTimer) clearTimeout(state.hideTimer);
    if (state.cycleTimer) clearTimeout(state.cycleTimer);
    if (state.cycleHideTimer) clearTimeout(state.cycleHideTimer);
    state.hideTimer = state.cycleTimer = state.cycleHideTimer = null;
  }

  function showBar(trigger = 'auto', effectClass = '') {
    clearTimeout(state.hideTimer);
    root.classList.remove('hidden');
    root.classList.add('visible');
    if (effectClass) {
      const effectClasses = [
        'effect-pulse',
        'effect-shake',
        'effect-flash',
        'effect-vignette',
        'effect-scanline',
        'effect-scratch',
        'effect-bloodmist',
        'effect-claws',
        'effect-cinematic',
        'effect-mori',
        'effect-glow-soft',
        'effect-glow-boost',
      ];
      root.classList.remove(...effectClasses);
      root.classList.add(...effectClass.split(' ').filter(Boolean));
    }
    state.visible = true;
    const visibleSeconds = trigger === 'donation' ? state.settings.donationVisibleSeconds : state.settings.autoVisibleSeconds;
    state.hideTimer = setTimeout(() => hideBar(), (visibleSeconds || 30) * 1000);
  }

  function hideBar() {
    root.classList.add('hidden');
    root.classList.remove('visible');
    state.visible = false;
  }

  function startAutoCycle() {
    if (!state.settings.autoCycleEnabled) return;
    const run = () => {
      showBar('auto', 'effect-pulse');
      state.cycleHideTimer = setTimeout(() => hideBar(), (state.settings.autoVisibleSeconds || 30) * 1000);
      state.cycleTimer = setTimeout(run, (state.settings.autoShowEveryMinutes || 5) * 60 * 1000);
    };
    run();
  }

  function tierForBits(bits) {
    const n = Number(bits) || 0;
    if (n >= 1500) return 'max';
    if (n >= 1000) return 't7';
    if (n >= 500) return 't6';
    if (n >= 250) return 't5';
    if (n >= 100) return 't4';
    if (n >= 50) return 't3';
    if (n >= 25) return 't2';
    if (n >= 10) return 't1';
    if (n >= 1) return 't0';
    return 'none';
  }

  function applyTierEffect(tier) {
    const map = {
      t0: 'effect-pulse effect-glow-soft',
      t1: 'effect-pulse effect-scanline',
      t2: 'effect-pulse effect-scratch',
      t3: 'effect-pulse effect-shake effect-scratch effect-scanline',
      t4: 'effect-pulse effect-flash effect-vignette',
      t5: 'effect-pulse effect-shake effect-bloodmist',
      t6: 'effect-pulse effect-claws effect-glow-boost',
      t7: 'effect-pulse effect-shake effect-cinematic effect-glow-boost',
      max: 'effect-pulse effect-shake effect-mori effect-glow-boost',
    };
    const cls = map[tier] || 'effect-pulse';
    showBar('donation', cls);
    triggerParticles(tier);
  }

  function triggerParticles(tier) {
    if (state.settings.particleMode === 'off') return;
    if (tier !== 't6' && tier !== 't7' && tier !== 'max') return;
    const ctx = particleCanvas.getContext('2d');
    const density =
      tier === 'max'
        ? state.settings.particleMode === 'high'
          ? 48
          : 28
        : state.settings.particleMode === 'high'
        ? 28
        : 18;
    const particles = [];
    const rect = particleCanvas.getBoundingClientRect();
    particleCanvas.width = rect.width * devicePixelRatio;
    particleCanvas.height = rect.height * devicePixelRatio;
    const w = particleCanvas.width;
    const h = particleCanvas.height;
    for (let i = 0; i < density; i++) {
      particles.push({
        x: Math.random() * w,
        y: Math.random() * h,
        vx: (Math.random() - 0.5) * 0.8,
        vy: -Math.random() * 1.8 - 0.2,
        life: 400 + Math.random() * 500,
        size: 1 + Math.random() * 2,
      });
    }
    const start = performance.now();
    particleCanvas.style.opacity = tier === 'max' ? '0.95' : '0.8';
    function loop(now) {
      const elapsed = now - start;
      ctx.clearRect(0, 0, w, h);
      particles.forEach((p) => {
        p.x += p.vx * 16;
        p.y += p.vy * 16;
        p.life -= 16;
        ctx.globalAlpha = Math.max(0, p.life / 500);
        ctx.fillStyle = p.life % 2 ? 'rgba(220,20,60,0.85)' : 'rgba(245,245,245,0.65)';
        ctx.beginPath();
        ctx.arc(p.x, p.y, p.size, 0, Math.PI * 2);
        ctx.fill();
      });
      if (elapsed < 950) requestAnimationFrame(loop);
      else {
        ctx.clearRect(0, 0, w, h);
        particleCanvas.style.opacity = '0';
      }
    }
    requestAnimationFrame(loop);
  }

  function handleBits(evt) {
    const bits = Number(evt.amount || evt.bits || evt.data?.bits || 0);
    if (!bits) return;
    tweenProgress(Math.max(0, state.current + bits));
    applyTierEffect(tierForBits(bits));
  }

  function isAuthorized(event) {
    if (state.settings.allowViewerCommands) return true;
    const badges = event.badges || event.data?.badges || event.tags?.badges || [];
    const isBroadcaster = badges.some?.((b) => b.type === 'broadcaster') || event.tags?.badges?.broadcaster === '1';
    const isMod = badges.some?.((b) => b.type === 'moderator') || event.tags?.mod === '1' || event.data?.tags?.mod === '1';
    const username = (event.nick || event.displayName || event.name || '').toLowerCase();
    const whitelist = (state.settings.modWhitelist || '')
      .split(',')
      .map((n) => n.trim().toLowerCase())
      .filter(Boolean);
    if (whitelist.includes(username)) return true;
    if (isBroadcaster && state.settings.allowBroadcaster) return true;
    if (isMod && state.settings.allowMods) return true;
    return false;
  }

  function handleCommand(text, event) {
    const parts = text.trim().split(/\s+/);
    const cmd = parts[0].toLowerCase();
    if (cmd !== '!bbits') return;
    if (!isAuthorized(event)) return log('command ignored (not authorized)');
    const sub = (parts[1] || '').toLowerCase();
    const val = parts[2];
    switch (sub) {
      case 'show':
        showBar('donation', 'effect-pulse');
        break;
      case 'hide':
        hideBar();
        break;
      case 'status':
        log({ visible: state.visible, current: state.current, goal: state.goal, timers: { hide: !!state.hideTimer, cycle: !!state.cycleTimer } });
        break;
      case 'reset':
        setCurrent(0);
        break;
      case 'setgoal':
        if (val) setGoal(Number(val));
        break;
      case 'setcurrent':
        if (val) setCurrent(Number(val));
        break;
      case 'add':
        if (val) tweenProgress(Math.max(0, state.current + Number(val)));
        break;
      default:
        if (state.visible) hideBar();
        else showBar('donation', 'effect-pulse');
        break;
    }
  }

  function handleMessage(event) {
    const text = event.data?.text || event.message || event.data?.message || '';
    if (!text) return;
    handleCommand(text, event.data || event);
  }

  function initState(detail) {
    const field = detail?.fieldData || {};
    state.settings = {
      ...defaultSettings,
      ...field,
      colors: {
        ...defaultSettings.colors,
        accentRed: field.accentRed || field.colors?.accentRed || defaultSettings.colors.accentRed,
        accentWhite: field.accentWhite || field.colors?.accentWhite || defaultSettings.colors.accentWhite,
        bgDark: field.bgDark || field.colors?.bgDark || defaultSettings.colors.bgDark,
      },
    };
    state.goal = Number(field.goalBits ?? defaultSettings.goalBits);
    state.current = Number(field.currentBitsInitial ?? defaultSettings.currentBitsInitial);
    applySettings();
    restore();
    updateNumbers();
    if (state.settings.autoCycleEnabled) startAutoCycle();
    state.ready = true;
    showBar('auto', 'effect-pulse');
  }

  window.addEventListener('onWidgetLoad', function (obj) {
    initState(obj.detail || {});
  });

  window.addEventListener('onEventReceived', function (obj) {
    const listener = obj.detail?.listener;
    const event = obj.detail?.event;
    if (!event) return;
    if (listener === 'cheer-latest' || event.type === 'cheer' || event.listener === 'cheer-latest') {
      handleBits(event);
      return;
    }
    if (listener === 'message') {
      handleMessage(event);
    }
  });
})();
```
