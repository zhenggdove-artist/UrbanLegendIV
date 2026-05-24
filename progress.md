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
- Redesigned World Builder around player-driven building: the creator backend can keep player movement active in world mode, `B/Tab` toggles a bottom toolkit, `E` places the current tool in front of the player, clicking objects switches into object adjustment, dragging can resize or move objects, and clicking the player/empty space returns to player control.
- Added an Actor/NPC editor mode in the Shift+C backend. It edits player/NPC role data, primitive/PNG sequence/GLTF model sources, model scale, PNG action frame ranges, interaction behavior, and NPC instances placed near the player.
- Extended project JSON/localStorage persistence to include `actors`, and added actor import/export through the creator backend.
- Re-ran syntax parsing, the develop-web-game Playwright client, and a direct Playwright flow covering Shift+C, world-mode player movement, toolkit opening, keyboard placement, object resize dragging, returning to player control, actor-mode NPC creation, and PNG sequence import. No console/page errors were captured.
- Adjusted World Builder empty-space/floor pointer handling so left-drag still controls the camera in creation mode; object installation is now through `E` or panel buttons. Re-tested that downward mouse drag increases top-down pitch and does not create objects, then re-ran and visually inspected the standard Playwright screenshot.

TODO:
- 3D model import currently supports GLB or self-contained GLTF data URLs; GLTF files with separate external `.bin`/texture files will need an asset resolver if that workflow becomes important.
