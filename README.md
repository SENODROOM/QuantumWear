# QuantumWear

Design your own T-shirt, polo shirt, or hoodie. Explore example designs for inspiration, then choose the garment and make your own design with text or artwork. Save it to your account and order it.

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
**Accounts:** set `JWT_SECRET` in `server/.env` to a long, random secret; signup, login, and saved designs require MongoDB and this secret.

### Production

```bash
npm run build                # builds client/dist
npm start                    # Express serves the API and the built site on :5000
```

## Pages (React Router)

| Route | What it does |
| --- | --- |
| `/` | Home with a live shirt preview, design examples and how it works |
| `/presets` | Black and white shirt examples to inspire your own design |
| `/customize` and `/customize/:designId` | Design a T-shirt, polo shirt, or hoodie; add text or images, save and add to cart |
| `/login` and `/signup` | Sign in or create an account |
| `/designs` | View and manage designs saved to your account |
| `/cart`, `/checkout`, `/order/:orderId` | Cart, delivery details and order confirmation |

## API

| Method and path | Purpose |
| --- | --- |
| `GET /api/presets` | Shirt design examples, prices and sizes |
| `GET /api/auth/me` | Current signed-in account |
| `POST /api/auth/signup`, `POST /api/auth/login`, `POST /api/auth/logout` | Create an account, sign in, and sign out |
| `GET /api/designs` | List the signed-in account's designs |
| `POST /api/designs`, `GET/PUT/DELETE /api/designs/:id` | Create, read, update, delete an account-owned design (sign-in required) |
| `POST /api/orders`, `GET /api/orders/:id` | Place and read an order (the server recalculates the total) |
| `GET /api/health` | Status and storage mode (`mongo` or `memory`) |

## Where to change things

- **Prices and sizes:** `server/src/config.js` and `client/src/config.js` (keep both in sync).
- **Presets (colors, names, text):** `server/src/data/presets.js` and `client/src/config.js`.
- **Shirt drawing:** `client/src/components/Polo.jsx` (one SVG, any color, front and back).
- **Fonts for customers:** `FONTS` in `client/src/config.js` and the Google Fonts link in `client/index.html`.
- **Site logo:** `client/public/logo-mark.svg` is used in the header and footer. Example shirt designs do not include the site logo.

## Not included yet

- Payments (orders are stored, nothing is charged) and order emails.
- An admin screen to view orders. They are in the `orders` collection.
