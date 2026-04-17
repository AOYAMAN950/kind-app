[index.html](https://github.com/user-attachments/files/26823038/index.html)
# kind-app<!DOCTYPE html>
<html lang="ja">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, viewport-fit=cover">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="default">
  <meta name="apple-mobile-web-app-title" content="勤務表">
  <title>勤務表</title>
  <style>
    *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

    :root {
      --accent:       #007AFF;
      --accent-bg:    #E8F0FE;
      --bg:           #F2F2F7;
      --card:         #FFFFFF;
      --text:         #1C1C1E;
      --text-sub:     #6E6E73;
      --border:       #C6C6C8;
      --sat:          #007AFF;
      --sun:          #FF3B30;
      --break-color:  #FF9500;
    }

    html, body {
      height: 100%;
      font-family: -apple-system, BlinkMacSystemFont,
                   'Hiragino Sans', 'Hiragino Kaku Gothic ProN',
                   'Yu Gothic', sans-serif;
      background: var(--bg);
      color: var(--text);
      -webkit-text-size-adjust: 100%;
    }

    /* ── Header ───────────────────────────────── */
    .app-header {
      position: sticky;
      top: 0;
      z-index: 50;
      background: var(--card);
      padding: 12px 16px 10px;
      padding-top: max(12px, env(safe-area-inset-top));
      box-shadow: 0 1px 0 rgba(0,0,0,0.12);
    }

    .app-title {
      font-size: 17px;
      font-weight: 700;
      text-align: center;
      margin-bottom: 8px;
      letter-spacing: 0.03em;
    }

    .period-nav {
      display: flex;
      align-items: center;
      gap: 8px;
    }

    .nav-btn {
      width: 44px;
      height: 44px;
      border-radius: 10px;
      border: none;
      background: var(--accent);
      color: #fff;
      font-size: 24px;
      font-weight: 300;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      flex-shrink: 0;
      -webkit-tap-highlight-color: transparent;
      user-select: none;
    }
    .nav-btn:active { opacity: 0.65; }

    .period-text {
      flex: 1;
      text-align: center;
      font-size: 13px;
      font-weight: 600;
      line-height: 1.4;
    }

    /* ── Summary Banner ───────────────────────── */
    .summary-banner {
      background: var(--accent);
      color: #fff;
      padding: 14px 16px;
      text-align: center;
    }

    .summary-label {
      font-size: 11px;
      font-weight: 500;
      opacity: 0.85;
      letter-spacing: 0.06em;
    }

    .summary-value {
      font-size: 28px;
      font-weight: 700;
      margin-top: 3px;
      letter-spacing: 0.01em;
      min-height: 34px;
    }

    /* ── Day List ─────────────────────────────── */
    .day-list {
      padding: 10px 12px;
      padding-bottom: max(28px, calc(env(safe-area-inset-bottom) + 20px));
    }

    /* ── Day Card ─────────────────────────────── */
    .day-card {
      background: var(--card);
      border-radius: 14px;
      padding: 10px 12px;
      margin-bottom: 8px;
      box-shadow: 0 1px 3px rgba(0,0,0,0.08);
      border-left: 4px solid transparent;
    }

    .day-card.has-entry  { border-left-color: var(--accent); }
    .day-card.is-today   { background: #EEF5FF; }

    /* ── Card Header ──────────────────────────── */
    .card-header {
      display: flex;
      align-items: flex-start;
      justify-content: space-between;
      min-height: 22px;
      margin-bottom: 8px;
    }

    .date-label {
      font-size: 15px;
      font-weight: 600;
    }

    .weekday-tag {
      font-size: 13px;
      font-weight: 400;
      margin-left: 2px;
    }
    .weekday-tag.sat { color: var(--sat); }
    .weekday-tag.sun { color: var(--sun); }

    .work-result { text-align: right; }

    .work-hours {
      font-size: 14px;
      font-weight: 700;
      color: var(--accent);
    }

    .break-badge {
      display: block;
      font-size: 10px;
      font-weight: 500;
      color: var(--break-color);
      margin-top: 1px;
    }

    /* ── Input Rows ───────────────────────────── */
    .input-row {
      display: flex;
      align-items: center;
      gap: 6px;
      margin-bottom: 6px;
    }
    .input-row:last-child { margin-bottom: 0; }

    .input-label {
      font-size: 11px;
      font-weight: 600;
      color: var(--text-sub);
      width: 28px;
      flex-shrink: 0;
    }

    /* Start time buttons */
    .start-buttons {
      display: flex;
      gap: 5px;
      flex: 1;
    }

    .start-btn {
      flex: 1;
      min-height: 44px;
      border-radius: 8px;
      border: 1.5px solid var(--border);
      background: var(--card);
      color: var(--text);
      font-size: 12px;
      font-weight: 600;
      font-family: inherit;
      cursor: pointer;
      padding: 0 2px;
      -webkit-tap-highlight-color: transparent;
      user-select: none;
    }
    .start-btn.active {
      background: var(--accent);
      border-color: var(--accent);
      color: #fff;
    }
    .start-btn:active:not(.active) { background: var(--accent-bg); }

    /* End time input */
    .end-time-input {
      flex: 1;
      height: 44px;
      border-radius: 8px;
      border: 1.5px solid var(--border);
      background: var(--card);
      color: var(--text);
      font-size: 15px;
      font-family: inherit;
      padding: 0 10px;
      -webkit-appearance: none;
      appearance: none;
    }
    .end-time-input:focus {
      outline: none;
      border-color: var(--accent);
    }

    /* Clear button */
    .clear-btn {
      width: 44px;
      height: 44px;
      border-radius: 8px;
      border: 1.5px solid var(--border);
      background: var(--card);
      color: var(--text-sub);
      font-size: 18px;
      font-family: inherit;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      flex-shrink: 0;
      -webkit-tap-highlight-color: transparent;
    }
    .clear-btn:active {
      background: #FEE2E2;
      border-color: #FCA5A5;
      color: #EF4444;
    }

    button { -webkit-user-select: none; user-select: none; }
  </style>
</head>
<body>

<div class="app-header">
  <div class="app-title">勤務表</div>
  <div class="period-nav">
    <button class="nav-btn" id="btn-prev" aria-label="前の期間">&#8249;</button>
    <div class="period-text" id="period-label"></div>
    <button class="nav-btn" id="btn-next" aria-label="次の期間">&#8250;</button>
  </div>
</div>

<div class="summary-banner">
  <div class="summary-label">期間合計実働時間</div>
  <div class="summary-value" id="total-hours">—</div>
</div>

<div class="day-list" id="day-list"></div>

<script>
'use strict';

// ── Constants ────────────────────────────────────────────────────────────────

const START_TIMES        = ['7:45', '8:30', '8:45', '10:30'];
const WEEKDAYS           = ['日','月','火','水','木','金','土'];
const BREAK_THRESHOLD    = 390; // 6 h 30 min in minutes
const BREAK_DURATION     = 60;  // 1 h in minutes

// ── Helpers ──────────────────────────────────────────────────────────────────

function pad(n) { return String(n).padStart(2, '0'); }

function isoDate(d) {
  return d.getFullYear() + '-' + pad(d.getMonth() + 1) + '-' + pad(d.getDate());
}

// ── Period Logic ─────────────────────────────────────────────────────────────

/**
 * Determine the work period that contains refDate.
 * Rules:
 *   day 1–20  →  prev-month/21 .. this-month/20
 *   day 21–31 →  this-month/21 .. next-month/20
 */
function getPeriodForDate(refDate) {
  const y  = refDate.getFullYear();
  const mo = refDate.getMonth();   // 0-indexed
  const day = refDate.getDate();

  if (day >= 21) {
    return {
      start: new Date(y, mo,     21),
      end:   new Date(y, mo + 1, 20)   // JS handles month-12 overflow
    };
  } else {
    return {
      start: new Date(y, mo - 1, 21),  // JS handles month -1 wrap
      end:   new Date(y, mo,     20)
    };
  }
}

/** Move a period forward or backward by one month (delta = +1 or -1). */
function shiftPeriod(period, delta) {
  return {
    start: new Date(period.start.getFullYear(), period.start.getMonth() + delta, 21),
    end:   new Date(period.end.getFullYear(),   period.end.getMonth()   + delta, 20)
  };
}

/** Build an array of Date objects covering period.start .. period.end. */
function getDatesInPeriod(period) {
  const list = [];
  const d = new Date(period.start);
  while (d <= period.end) {
    list.push(new Date(d));
    d.setDate(d.getDate() + 1);
  }
  return list;
}

/** LocalStorage key for a period. */
function periodStorageKey(period) {
  return 'wt_' + isoDate(period.start) + '_' + isoDate(period.end);
}

/** Human-readable period label (same-year end date omits year). */
function fmtPeriodLabel(period) {
  const s = period.start;
  const e = period.end;
  const startStr = s.getFullYear() + '年' + (s.getMonth() + 1) + '月' + s.getDate() + '日';
  const endStr   = e.getFullYear() === s.getFullYear()
    ? (e.getMonth() + 1) + '月' + e.getDate() + '日'
    : e.getFullYear() + '年' + (e.getMonth() + 1) + '月' + e.getDate() + '日';
  return startStr + '〜' + endStr;
}

// ── Time Calculation ─────────────────────────────────────────────────────────

/**
 * Parse "H:MM" or "HH:MM" → total minutes from midnight, or null if invalid.
 */
function parseTimeStr(str) {
  if (!str) return null;
  const m = str.match(/^(\d{1,2}):(\d{2})$/);
  if (!m) return null;
  const h   = parseInt(m[1], 10);
  const min = parseInt(m[2], 10);
  if (h > 23 || min > 59) return null;
  return h * 60 + min;
}

/**
 * Calculate work time for one day.
 * Returns { truncated: number, remainder: number, hasBreak: boolean } or null.
 *   truncated  – work minutes rounded DOWN to nearest 15
 *   remainder  – leftover minutes (0–14)
 *   hasBreak   – whether 1 h break was deducted
 */
function calcWork(startStr, endStr) {
  const s = parseTimeStr(startStr);
  const e = parseTimeStr(endStr);
  if (s === null || e === null) return null;

  let elapsed = e - s;
  if (elapsed <= 0) elapsed += 1440;       // overnight shift

  const hasBreak = elapsed >= BREAK_THRESHOLD;
  const raw      = elapsed - (hasBreak ? BREAK_DURATION : 0);
  if (raw <= 0) return null;

  const truncated  = Math.floor(raw / 15) * 15;
  const remainder  = raw - truncated;
  return { truncated, remainder, hasBreak };
}

/**
 * Format truncated minutes (always a multiple of 15) as a decimal hour string.
 * e.g. 450 → "7.5",  465 → "7.75"
 */
function decimalHours(minutes) {
  const h = Math.floor(minutes / 60);
  const m = minutes % 60;
  if (m === 0)  return String(h);
  if (m === 15) return h + '.25';
  if (m === 30) return h + '.5';
  return h + '.75';
}

/**
 * Format a work result as "X.Xh Ym" (or "X.Xh" if no remainder).
 */
function fmtWork(result) {
  if (!result) return '';
  const d = decimalHours(result.truncated);
  return result.remainder > 0
    ? d + '時間 ' + result.remainder + '分'
    : d + '時間';
}

// ── Storage ──────────────────────────────────────────────────────────────────

function loadData(key) {
  try { return JSON.parse(localStorage.getItem(key) || '{}'); }
  catch (e) { return {}; }
}

function saveData(key, data) {
  try { localStorage.setItem(key, JSON.stringify(data)); }
  catch (e) { /* quota exceeded – ignore */ }
}

// ── App State ────────────────────────────────────────────────────────────────

let currentPeriod = getPeriodForDate(new Date());
let storageKey    = periodStorageKey(currentPeriod);
let periodData    = loadData(storageKey);

// ── Render: Summary ──────────────────────────────────────────────────────────

function renderTotal() {
  let totalTruncated = 0;
  let totalRemainder = 0;

  for (const dateKey of Object.keys(periodData)) {
    const e = periodData[dateKey];
    if (!e || !e.start || !e.end) continue;
    const r = calcWork(e.start, e.end);
    if (r) {
      totalTruncated += r.truncated;
      totalRemainder += r.remainder;
    }
  }

  const el = document.getElementById('total-hours');
  if (totalTruncated === 0 && totalRemainder === 0) {
    el.textContent = '—';
    return;
  }
  const d = decimalHours(totalTruncated);
  el.textContent = totalRemainder > 0
    ? d + '時間 ' + totalRemainder + '分'
    : d + '時間';
}

// ── Render: Period Label ──────────────────────────────────────────────────────

function renderPeriodLabel() {
  document.getElementById('period-label').textContent = fmtPeriodLabel(currentPeriod);
}

// ── Render: Day List ─────────────────────────────────────────────────────────

function renderDayList() {
  const container = document.getElementById('day-list');
  container.innerHTML = '';

  const dates    = getDatesInPeriod(currentPeriod);
  const todayStr = isoDate(new Date());

  dates.forEach(function(d) {
    const dateKey    = isoDate(d);
    const entry      = periodData[dateKey] || {};
    const wd         = d.getDay();     // 0 = Sun, 6 = Sat
    const workResult = (entry.start && entry.end) ? calcWork(entry.start, entry.end) : null;

    // ── Card ───────────────────────────────────────────────
    const card = document.createElement('div');
    card.className = 'day-card'
      + (workResult        ? ' has-entry' : '')
      + (dateKey === todayStr ? ' is-today'  : '');

    // ── Card Header ────────────────────────────────────────
    const header = document.createElement('div');
    header.className = 'card-header';

    // Date label
    const dateLabel = document.createElement('div');
    dateLabel.className = 'date-label';
    const wdClass = wd === 0 ? 'sun' : wd === 6 ? 'sat' : '';
    dateLabel.innerHTML =
      (d.getMonth() + 1) + '/' + d.getDate() +
      '<span class="weekday-tag ' + wdClass + '">(' + WEEKDAYS[wd] + ')</span>';

    // Work result area (updated in-place)
    const resultDiv = document.createElement('div');
    resultDiv.className = 'work-result';

    function updateResultDiv() {
      const e2 = periodData[dateKey] || {};
      const r   = (e2.start && e2.end) ? calcWork(e2.start, e2.end) : null;
      if (r) {
        resultDiv.innerHTML =
          '<span class="work-hours">' + fmtWork(r) + '</span>' +
          (r.hasBreak ? '<span class="break-badge">☕ 休憩あり</span>' : '');
        card.classList.add('has-entry');
      } else {
        resultDiv.innerHTML = '';
        card.classList.remove('has-entry');
      }
    }

    updateResultDiv();
    header.appendChild(dateLabel);
    header.appendChild(resultDiv);
    card.appendChild(header);

    // ── Start Time Row ─────────────────────────────────────
    const startRow = document.createElement('div');
    startRow.className = 'input-row';

    const startLabel = document.createElement('div');
    startLabel.className = 'input-label';
    startLabel.textContent = '始業';
    startRow.appendChild(startLabel);

    const startBtns = document.createElement('div');
    startBtns.className = 'start-buttons';

    START_TIMES.forEach(function(t) {
      const btn = document.createElement('button');
      btn.className = 'start-btn' + (entry.start === t ? ' active' : '');
      btn.textContent = t;

      btn.addEventListener('click', function() {
        if (!periodData[dateKey]) periodData[dateKey] = {};
        if (periodData[dateKey].start === t) {
          delete periodData[dateKey].start;    // tap same → deselect
        } else {
          periodData[dateKey].start = t;
        }
        // Sync button states
        startBtns.querySelectorAll('.start-btn').forEach(function(b) {
          b.classList.toggle('active', b.textContent === (periodData[dateKey] && periodData[dateKey].start));
        });
        saveData(storageKey, periodData);
        updateResultDiv();
        renderTotal();
      });

      startBtns.appendChild(btn);
    });

    startRow.appendChild(startBtns);
    card.appendChild(startRow);

    // ── End Time Row ───────────────────────────────────────
    const endRow = document.createElement('div');
    endRow.className = 'input-row';

    const endLabel = document.createElement('div');
    endLabel.className = 'input-label';
    endLabel.textContent = '終業';
    endRow.appendChild(endLabel);

    const endInput = document.createElement('input');
    endInput.type = 'time';
    endInput.className = 'end-time-input';
    endInput.setAttribute('aria-label', '終業時刻');
    if (entry.end) endInput.value = entry.end;

    endInput.addEventListener('change', function() {
      if (!periodData[dateKey]) periodData[dateKey] = {};
      if (endInput.value) {
        periodData[dateKey].end = endInput.value;
      } else {
        delete periodData[dateKey].end;
      }
      saveData(storageKey, periodData);
      updateResultDiv();
      renderTotal();
    });

    endRow.appendChild(endInput);

    // Clear button
    const clearBtn = document.createElement('button');
    clearBtn.className = 'clear-btn';
    clearBtn.setAttribute('aria-label', 'この日のデータをクリア');
    clearBtn.textContent = '×';

    clearBtn.addEventListener('click', function() {
      delete periodData[dateKey];
      saveData(storageKey, periodData);
      startBtns.querySelectorAll('.start-btn').forEach(function(b) {
        b.classList.remove('active');
      });
      endInput.value = '';
      updateResultDiv();
      renderTotal();
    });

    endRow.appendChild(clearBtn);
    card.appendChild(endRow);

    container.appendChild(card);
  });
}

// ── Full Render ──────────────────────────────────────────────────────────────

function renderAll() {
  renderPeriodLabel();
  renderDayList();
  renderTotal();
}

// ── Period Navigation ────────────────────────────────────────────────────────

document.getElementById('btn-prev').addEventListener('click', function() {
  currentPeriod = shiftPeriod(currentPeriod, -1);
  storageKey    = periodStorageKey(currentPeriod);
  periodData    = loadData(storageKey);
  renderAll();
  window.scrollTo({ top: 0 });
});

document.getElementById('btn-next').addEventListener('click', function() {
  currentPeriod = shiftPeriod(currentPeriod, +1);
  storageKey    = periodStorageKey(currentPeriod);
  periodData    = loadData(storageKey);
  renderAll();
  window.scrollTo({ top: 0 });
});

// ── Boot ─────────────────────────────────────────────────────────────────────

renderAll();

// Auto-scroll so today's card is visible near the top of the viewport
requestAnimationFrame(function() {
  var todayStr = isoDate(new Date());
  var dates    = getDatesInPeriod(currentPeriod);
  var idx      = dates.findIndex(function(d) { return isoDate(d) === todayStr; });
  if (idx >= 0) {
    var cards = document.querySelectorAll('.day-card');
    if (cards[idx]) {
      cards[idx].scrollIntoView({ behavior: 'auto', block: 'start' });
    }
  }
});
</script>
</body>
</html>
