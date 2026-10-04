# QuantumWear

Design your own polo shirt. Start from a black or white preset, add text or artwork, drag it into place on the front or back, save the design, and order it.

**Stack:** MongoDB, Express, React 18 (Vite) and Node, with React Router DOM for routing.

## Repository guides

- [Client](client/README.md): frontend setup, scripts, and configuration.
- [Server](server/README.md): API setup, environment variables, and endpoints.

## Run it

You need Node 18 or newer.

```bash
npm run install:all          # installs root, server and client packages
cp server/.env.example server/.env
npm run dev                  # API on :5000, website on :5173
```

Open http://localhost:5173.

**MongoDB:** put your connection string in `server/.env` as `MONGO_URI` (local MongoDB or an Atlas URI).
If `MONGO_URI` is empty or unreachable, the API falls back to temporary in-memory storage so you can still try everything. Data resets when the server restarts.

### Production

```bash
npm run build                # builds client/dist
npm start                    # Express serves the API and the built site on :5000
```

## Pages (React Router)

| Route | What it does |
| --- | --- |
| `/` | Home with a live "type on the shirt" preview, the two presets and how it works |
| `/presets` | Black and white polos: customize one or add it to the cart |
| `/customize` and `/customize/:designId` | The designer: text, images, fonts, colors, rotation, front and back, save and add to cart |
| `/designs` | Designs saved in this browser: edit, add to cart, delete |
| `/cart`, `/checkout`, `/order/:orderId` | Cart, delivery details and order confirmation |

## API

| Method and path | Purpose |
| --- | --- |
| `GET /api/presets` | Black and white presets, prices and sizes |
| `GET /api/designs?ids=a,b` | Designs by id |
| `POST /api/designs`, `GET/PUT/DELETE /api/designs/:id` | Create, read, update, delete a design |
| `POST /api/orders`, `GET /api/orders/:id` | Place and read an order (the server recalculates the total) |
| `GET /api/health` | Status and storage mode (`mongo` or `memory`) |

## Where to change things

- **Prices and sizes:** `server/src/config.js` and `client/src/config.js` (keep both in sync).
- **Presets (colors, names, text):** `server/src/data/presets.js` and `client/src/config.js`.
- **Shirt drawing:** `client/src/components/Polo.jsx` (one SVG, any color, front and back).
- **Fonts for customers:** `FONTS` in `client/src/config.js` and the Google Fonts link in `client/index.html`.
- **Logo:** `client/public/logo-mark.svg` is the icon cut from your supplied logo and is used in the header and footer next to the "QuantumWear" name. Your original `logo-black-text.svg` and `logo-white-text.svg` are in the same folder.

## Not included yet

- Payments (orders are stored, nothing is charged) and order emails.
- User accounts: saved designs are remembered by this browser, using ids kept in localStorage.
- An admin screen to view orders. They are in the `orders` collection.
