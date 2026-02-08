# AGENTS.md

This repository contains a lunar birthday calendar generator - a single-page web application that converts lunar birthdays to ICS calendar files.

## Build/Test/Lint Commands

**No build process required** - This is a pure static HTML/CSS/JavaScript application with no dependencies to install or build commands to run.

### Manual Testing
```bash
# Open the application in browser
open index.html
# Or serve with a local server (recommended for testing)
python3 -m http.server 8000
# Then visit http://localhost:8000
```

### Browser Console Testing
- Open browser DevTools (F12)
- Check Console for errors during ICS generation
- Test functionality: add birthdays, generate ICS, verify localStorage
- Test both languages (zh-CN, zh-TW)

### Deployment Test
```bash
# Deploy to GitHub Pages (automatic on push to main)
git push origin main
# Or test locally with:
npx serve . -p 8000
```

## Code Style Guidelines

### File Structure
- **Single-file architecture**: All HTML, CSS, and JavaScript in `index.html`
- **No external files**: Do not create separate .css or .js files
- **No build tools**: No Webpack, Vite, npm, or bundlers
- **CDN-only dependencies**: All libraries loaded from jsDelivr CDN

### External Libraries (CDN versions)
Use these specific stable versions:
- `lunar-javascript@1.7.7` - Lunar/solar calendar conversion
- `chinese-conv@4.0.0` - Simplified/traditional Chinese conversion
- `ics.js@2.3.2` - ICS file generation (DO NOT use @latest)

**Critical**: Always pin specific versions, never use `@latest` due to breaking changes.

### JavaScript Conventions
- **Naming**: camelCase for variables and functions (e.g., `addBirthday`, `checkLeapMonth`)
- **Function names**: Descriptive, verb-first (e.g., `renderBirthdays`, `saveBirthdays`)
- **Constants**: UPPER_SNAKE_CASE for translations and month names
- **Event handlers**: Direct function references in HTML (e.g., `onclick="showHelp()"`)
- **Error handling**: Wrap external library calls in try-catch blocks
- **User feedback**: Use `showToast()` for success/error messages (never alert)

### Data Management
- **Persistence**: Use `localStorage` with key `'lunarBirthdays'`
- **Language preference**: Store in `localStorage` with key `'language'`
- **Data structure**: Array of objects with `id`, `name`, `referenceYear`, `lunarMonth`, `lunarDay`, `isLeapMonth`, `note`, `createdAt`
- **ID generation**: Use `Date.now().toString()` for unique IDs

### CSS Conventions
- **Inline styles**: All CSS in `<style>` tags within `<head>`
- **Class naming**: kebab-case (e.g., `.btn-primary`, `.birthday-item`)
- **Responsive**: Mobile-first with `@media (max-width: 768px)` breakpoint
- **Colors**: Primary `#667eea`, success `#2ed573`, danger `#ff4757`
- **Animations**: CSS transitions for hover states, keyframes for toast slide-in

### HTML Conventions
- **Semantic**: Use `<header>`, `<main>`, `<section>` where appropriate
- **Accessibility**: Include labels for all form inputs, use semantic buttons
- **Chinese text**: Form labels stay in simplified Chinese (not affected by language switch)
- **UI labels**: Use `<span class="optional-indicator">` for (必填)/(可选) indicators

### ICS File Generation
- **Recurrence**: Use `YEARLY` frequency with no `UNTIL` or `COUNT` (permanent)
- **Date format**: `YYYYMMDDTHHMMSSZ` (all-day: `T000000Z`)
- **Base year**: Always use next year (currentYear + 1) for first occurrence
- **Timezone**: UTC only (Z suffix)
- **Error check**: Verify `typeof ics !== 'undefined'` before generating

### Date Calculations
**Critical pattern - Manual date formatting** (DO NOT use `solar.toYmd()`):
```javascript
const solar = lunar.getSolar();
const yearNum = solar.getYear();
const monthNum = solar.getMonth();  // Returns 0-11
const dayNum = solar.getDay();
const month = String(monthNum + 1).padStart(2, '0');  // Convert to 1-12
const day = String(dayNum).padStart(2, '0');
const date = `${yearNum}-${month}-${day}`;
```

### Variable Scope
- **Avoid naming collisions**: Use different variable names in different functions (e.g., `i` in one loop, `j` in another)
- **Loop variables**: Initialize at loop start, not from outer scope
- **Global variables**: Minimize - only `birthdays`, `currentLanguage`, `translations`, `monthNames`

### Language Support
- **Supported**: zh-CN (simplified), zh-TW (traditional) only
- **Switch scope**: Affects UI elements, NOT form field labels (those stay simplified Chinese)
- **Storage**: Save preference to localStorage
- **Conversion**: Use `ChineseConv()` from chinese-conv library
- **DO NOT add**: English or other languages unless explicitly requested

### Error Handling Patterns
```javascript
try {
  const result = someLibraryCall();
} catch (e) {
  console.error('Description:', e);
  showToast('User-friendly message', 'error');
}
```

### Testing Checklist Before Commit
- [ ] Open `index.html` in browser and verify no console errors
- [ ] Test adding a birthday (simplified Chinese)
- [ ] Test adding a birthday (traditional Chinese)
- [ ] Test ICS file generation and download
- [ ] Verify language switch doesn't affect form labels
- [ ] Test leap month warning display
- [ ] Test delete and clear all functions
- [ ] Verify localStorage persistence across page refresh
- [ ] Check responsive design on mobile viewport

## Deployment
- **Platform**: GitHub Pages (automatic on push to main)
- **Workflow**: `.github/workflows/deploy.yml`
- **URL**: https://oroliy.github.io/lunar-birthday-calendar/
- **No manual deployment**: Just push to main branch

## Important Bugs to Watch For
1. **Variable naming collisions**: Different loops must use different variable names
2. **ICS library loading**: Always check `typeof ics !== 'undefined'` before use
3. **Date formatting**: Manually construct dates, don't rely on library methods
4. **Loop initialization**: Start loops at 0 unless intentionally starting at 1
5. **CDN version stability**: Never use `@latest`, always pin specific versions
6. **Month indexing**: `solar.getMonth()` returns 0-11, add 1 for display

## Feature Requests - Decision Framework
- **New languages**: Only implement if explicitly requested
- **Batch import**: Consider CSV format if requested
- **Build tools**: Reject - maintain pure static approach
- **Separate files**: Reject - keep single-file architecture
- **English support**: Reject unless explicitly requested

## Git Workflow
- **Branch**: Work directly on main (no feature branches needed for this simple project)
- **Commits**: Descriptive, single purpose
- **No auto-merge**: Manual testing required before considering work complete
