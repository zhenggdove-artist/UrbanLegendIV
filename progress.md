Original prompt: Refactor the existing single-file 3D web game index.html into a reusable web game creation template with a World Builder, UI Editor, Shift+C creator backend, save/load/import/export, and browser-direct lightweight deployment.

Progress:
- Inspected the legacy `index.html` structure. It is a large single-file Three.js game with renderer/scene setup, requestAnimationFrame loop, input handling, UI layout editor, localStorage/JSON export, and an appended Game Creator shell.
- Decision: rebuild `index.html` as a clean reusable template instead of layering more editor code onto the old game-specific globals/assets.
- Replaced `index.html` with a clean single-file Three.js template: runtime scene/player loop, World Builder, UI Editor, localStorage project persistence, JSON import/export modals, and `render_game_to_text`/`advanceTime` hooks.
- Ran a module syntax check by extracting the module script to a temp `.mjs`; `node --check` passed.
- Ran the develop-web-game Playwright client against `http://127.0.0.1:5173`; rendered scene screenshot and `render_game_to_text` were valid, with no captured console errors.
- Ran a direct Playwright smoke test for `Shift+C`, World Builder add, UI Editor add, `Shift+C` close, and runtime application after close; all passed with no console/page errors.
- Fixed camera placement and UI editor overlay stacking after screenshot review.
- Added `TEMPLATE_GUIDE.md` with architecture, editor entry points, JSON formats, saving/deployment, and recommended new-game workflow.
- Localized the in-game UI and creator/editor panels to Traditional Chinese, including legacy localStorage label migration for default English names.
- Fixed movement orientation so Up/W moves forward relative to the camera and Down/S moves backward; fixed vertical mouse drag so dragging downward increases the top-down view angle.
- Re-ran syntax check, direct Playwright smoke test for movement/drag/editor Chinese UI, and the develop-web-game Playwright client. No console/page errors were captured.

TODO:
- No open implementation TODOs from this pass.
