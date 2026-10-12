# Working on Interior Design Visualizer

The whole app is `interactive_interior_visualizer.html`, a single file using three.js r128. 1 unit = 1 ft; x runs right and z runs down the drawing.

## When the user drops a product name, an Amazon link or a Pinterest pin
- **Amazon:** curl the product page with a browser User-Agent (WebFetch usually gets a 503), take the title, dimensions, price and the main `m.media-amazon.com` image, then add it as a `SEED_LIBRARY` entry with the real size in feet.
- **Product name only:** find the product (Amazon.in first), confirm its size and price, then handle it the same way as an Amazon link.
- **Pinterest:** curl the pin page, take the `i.pinimg.com/736x/...` image, and either build the look (geometry and materials) or cut out and embed the art as a WebP data URI. Choose colours that fit Vastu for that zone.
- **Fixed slots:** a new product replaces the item of the same category in place (see `FAMILIES` / `useProduct`). It does not add a duplicate.
- **Changing the starter design:** add a one-time migration (`xxxOnce(st)`, guarded by a flag on `st`), chain it at boot and in `resetDesign`, and run it on both `state` and `state.vastu.before`. Migrations only apply to the built-in sample home (`IS_BUILTIN`).
- **Checks after every change:**
  - `node --check` on the extracted `<script>`.
  - Load the page headless and confirm there are no clashes (`collisions`), the Vastu score hasn't dropped without a reason, and the console shows no errors.
  - Look at a walk-view screenshot.
- **Vastu rules to respect:** deity art only on walls you face (never behind the sofa, never in toilets); fire in the SE of the kitchen and water in the NE; no WC in the NE; the mandir never shares a wall with a toilet.
- **Privacy:** the built-in plan is a stock plan from the internet. Never add real addresses or personal data.
