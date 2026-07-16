# Checks automatizables — evidencia objetiva en Fase 1

Parte del checklist se puede correr por script contra la app viva (Playwright + axe-core).
Corré esto ANTES del fan-out de subagentes: los hallazgos objetivos (contraste, focus,
scroll-x, targets) salen gratis y los agentes se concentran en lo que requiere juicio.

Setup mínimo (una vez, en el repo auditado o en scratchpad):
```bash
npm i -D playwright @axe-core/playwright && npx playwright install chromium
```

## Script base — recorre rutas y junta hallazgos
```js
// audit.mjs — node audit.mjs http://localhost:3000 /ruta1 /ruta2 ...
import { chromium } from 'playwright';
import AxeBuilder from '@axe-core/playwright';

const [base, ...routes] = process.argv.slice(2);
const browser = await chromium.launch();
const findings = [];

for (const route of routes.length ? routes : ['/']) {
  const page = await browser.newPage();
  const consoleErrors = [];
  page.on('console', m => m.type() === 'error' && consoleErrors.push(m.text()));
  page.on('pageerror', e => consoleErrors.push(String(e)));
  await page.goto(base + route, { waitUntil: 'networkidle' });

  // 1. axe-core: contraste AA, labels/roles/nombres accesibles, aria
  const axe = await new AxeBuilder({ page })
    .withTags(['wcag2a', 'wcag2aa', 'wcag22aa']).analyze();
  for (const v of axe.violations)
    findings.push({ route, check: `axe:${v.id}`, impact: v.impact,
      n: v.nodes.length, help: v.help });

  // 2. Scroll horizontal del body (checklist #15)
  const scrollX = await page.evaluate(() =>
    document.documentElement.scrollWidth > document.documentElement.clientWidth);
  if (scrollX) findings.push({ route, check: 'body-scroll-x', impact: 'serious' });

  // 3. Targets táctiles chicos (< 24px WCAG 2.5.8; warn < 44px) (checklist #28)
  const small = await page.evaluate(() =>
    [...document.querySelectorAll('a,button,[role=button],input,select,textarea')]
      .map(el => ({ el: el.outerHTML.slice(0, 80), r: el.getBoundingClientRect() }))
      .filter(({ r }) => r.width > 0 && (r.width < 24 || r.height < 24))
      .map(({ el, r }) => ({ el, w: Math.round(r.width), h: Math.round(r.height) })));
  small.forEach(t => findings.push({ route, check: 'target-too-small', impact: 'serious', ...t }));

  // 4. Focus visible: tab-walk, ¿algún focused sin outline/box-shadow? (checklist #12)
  const noFocus = await page.evaluate(() => {
    const out = [];
    for (const el of document.querySelectorAll('a,button,input,select,textarea')) {
      el.focus();
      const s = getComputedStyle(el);
      if (s.outlineStyle === 'none' && !s.boxShadow.includes('rgb'))
        out.push(el.outerHTML.slice(0, 80));
    }
    return out.slice(0, 10);
  });
  noFocus.forEach(el => findings.push({ route, check: 'focus-invisible', impact: 'serious', el }));

  // 5. Placeholder-como-label e inputs sin autocomplete (patterns §C, writing §5)
  const formIssues = await page.evaluate(() =>
    [...document.querySelectorAll('input:not([type=hidden])')].flatMap(i => {
      const issues = [];
      const hasLabel = i.labels?.length || i.getAttribute('aria-label') || i.getAttribute('aria-labelledby');
      if (!hasLabel && i.placeholder) issues.push('placeholder-as-label');
      if (['email', 'tel', 'password'].includes(i.type) && !i.getAttribute('autocomplete'))
        issues.push('missing-autocomplete');
      return issues.map(check => ({ check, el: i.outerHTML.slice(0, 80) }));
    }));
  formIssues.forEach(f => findings.push({ route, impact: 'moderate', ...f }));

  // 6. Errores de consola/red durante la carga
  consoleErrors.slice(0, 5).forEach(e =>
    findings.push({ route, check: 'console-error', impact: 'moderate', e: e.slice(0, 120) }));

  await page.close();
}
await browser.close();
console.log(JSON.stringify(findings, null, 2));
console.error(`${findings.length} hallazgos automáticos`);
```

Variantes útiles:
- **Dark mode**: relanzar con `browser.newPage({ colorScheme: 'dark' })` — el contraste AA se
  chequea en AMBOS temas (checklist #14).
- **Reduced motion**: `page.emulateMedia({ reducedMotion: 'reduce' })` y verificar que las
  animaciones grandes no corren (checklist #19).
- **Mobile**: `browser.newPage({ ...devices['iPhone 15'] })` (import de playwright) y re-correr
  targets/scroll-x.
- **Límites advisory** (checklist #23): con Playwright, `page.fill(sel, '9'.repeat(50))` en
  campos con máximo documentado y verificar que el submit lo rechaza.

## Qué es automatizable y qué no
| Checklist ítem | Auto | Técnica |
|---|---|---|
| #12 focus visible, #13 labels/roles, #14 contraste AA | ✅ | axe + tab-walk |
| #15 scroll-x, #19 reduced-motion, #28 targets | ✅ | script base |
| #22 inputs basura, #23 límites advisory | ✅ parcial | fill() con valores inválidos |
| #2 loading/error/vacío, #27 Back, #21 modales | 🟡 semi | Playwright guiado por agente (route mocking para forzar error: `page.route('**/api/**', r => r.abort())`) |
| #1 undo real, #9 modelo mental, #25 auto-evidente, #26 goodwill, #29 trigger words, #30 dark patterns | ❌ | juicio humano/agente — el corazón de la auditoría |

Los ✅ dan evidencia con selector exacto → convertilos directo en hallazgos `[H/M/L] — file:line`
mapeando el selector al componente. Los ❌ son donde el agente aporta valor: no gastes juicio en
lo que el script ya midió.
