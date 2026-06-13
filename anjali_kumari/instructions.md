# Plixora - Online Gaming Website 

| Field | Value |
| --- | --- |
| Product | Plixora |
| Tagline | Play Instantly. Anywhere. Free. |
| Type | Online gaming website |
| Stack | PERN: PostgreSQL, Express, React, Node.js |
| Architecture | Full stack: React frontend, Express and Node API, PostgreSQL database, admin panel |
| Platforms | Responsive web (desktop, tablet, mobile) |

---

## 1. Project Overview

Plixora is a complete online gaming website for discovering and
playing free, instantly playable browser games with no download and
no mandatory sign in. This brief defines the full build: the public
website, the backend APIs, the database, and the admin panel. Build
every section below so the parts work together as one product.

---

## 2. Brand Guidelines

### 2.1 Colour Palette

| Use | Colour Name | Hex |
| --- | --- | --- |
| Primary brand | Plixora Purple | #7C5CFF |
| Accent | Teal | #00D1B2 |
| Highlight | Pink | #FF5C9E |
| Rating and badges | Amber | #FFB84C |
| Background | Deep Navy | #0F1226 |
| Surface (cards, header) | Panel Navy | #181C36 |
| Primary text | Off White | #ECEEF8 |
| Muted text | Cool Grey | #969CC4 |

### 2.2 Mandatory Brand Rules (apply to every screen)

* "Plixora" branding in header + footer on every page.
* Tagline is exactly "Play Instantly. Anywhere. Free." with no rewording.
* Use #7C5CFF for primary actions and active states. No colour substitutions.
* Keep it clean, no heavy ads, family friendly throughout.
* Keyboard accessible interactive elements. Alt text on all images.

---

## 3. Information Architecture (Pages)

| Page | Path | Purpose |
| --- | --- | --- |
| Homepage | / | Featured, popular, new, and category sections |
| Browse | /category/:slug | Filterable, paginated grid for one category |
| Play | /game/:slug | Embedded game, full screen, details, related games |
| Search | /search?q= | Live suggestions and results grid |
| Sign in | /login | Optional account sign in |
| Register | /register | Optional account creation |
| Favourites | /favourites | Saved games (account or local) |
| History | /history | Recently played games |
| Static | /about, /contact, /privacy, /terms | Information pages |

---

## 4. Frontend Requirements

The frontend is a React single page app that uses React Router for
the pages in Section 3. Build the homepage in this order:

1. Sticky header with the Plixora logo on the left, a centred search bar, and Sign in on the right.
2. A horizontal, scrollable row of category chips below the header (Action, Puzzle, Racing, Sports, Multiplayer, Arcade, Casual, .io, 2-Player).
3. A featured hero that rotates through 3 to 5 highlighted games.
4. Game grids under the headings Popular, New, and one row per major category.
5. A reusable game card with thumbnail, title, category badge, star rating, and a hover state that lifts the card and shows a play overlay.
6. A footer with links to About, Contact, Privacy, and Terms.

### Responsive rules

| Breakpoint | Cards per row | Navigation |
| --- | --- | --- |
| Desktop (>= 1200px) | 5 to 6 | Full category bar |
| Tablet (768 to 1199px) | 3 to 4 | Scrollable category bar |
| Mobile (< 768px) | 2 | Hamburger menu, scrollable chips |

---

## 5. Backend Requirements (REST API)

Build a REST API that the frontend and admin panel both consume.

| Method | Endpoint | Purpose |
| --- | --- | --- |
| GET | /api/games | List games (supports page, limit, category, sort) |
| GET | /api/games/:slug | Get one game |
| GET | /api/categories | List categories |
| GET | /api/search?q= | Search games by title and tags |
| POST | /api/auth/register | Create an account |
| POST | /api/auth/login | Sign in and return a token or session |
| POST | /api/auth/logout | End the session |
| GET | /api/favourites | List the signed in user favourites |
| POST | /api/favourites/:gameId | Add a favourite |
| DELETE | /api/favourites/:gameId | Remove a favourite |
| GET | /api/history | List recently played for the user |
| POST | /api/history/:gameId | Record a play event |
| POST | /api/ratings/:gameId | Submit a star rating |

API rules:

* All list endpoints support pagination, filtering, and sorting.
* All inputs are validated and errors return clear JSON messages with correct HTTP status codes.
* Authentication is required only for account specific endpoints (favourites, history write, ratings).
* Rate limiting protects auth and write endpoints.

---

## 6. Database Schema

| Table | Key Fields |
| --- | --- |
| games | id, slug, title, category_id, thumbnail_url, embed_url, rating_avg, plays, is_featured, is_published, created_at |
| categories | id, slug, name, cover_url |
| users | id, email, password_hash, display_name, created_at |
| favourites | id, user_id, game_id, created_at |
| play_history | id, user_id, game_id, played_at |
| ratings | id, user_id, game_id, stars, created_at |

* The database is PostgreSQL.
* Add indexes on games.category_id, games.title, and games.plays.
* Provide seed data with at least 8 categories and 30 games.

---

## 7. Accounts and Persistence

* Sign in is optional and never required to play a game.
* Signed out users: favourites and recently played are stored in browser localStorage.
* Signed in users: favourites and recently played sync to the account through the API.
* Passwords are hashed (for example bcrypt) and never stored in plain text.

---

## 8. Core Features

| Feature | Behaviour |
| --- | --- |
| Browse by category | Filter the catalogue by the selected category |
| Search | Live suggestions while typing, full results on submit |
| Play game | Open the play page and load the game in an iframe with full screen |
| Favourites | Toggle and persist (local or account) |
| Recently played | Record on play, show newest first |
| Ratings | Star rating updates the game average |

---

## 9. Admin Panel

| Capability | Detail |
| --- | --- |
| Secure login | Admin only, separate from public accounts |
| Manage games | Create, edit, delete, set thumbnail and embed URL |
| Manage categories | Create, edit, delete categories |
| Publish control | Publish or unpublish, and feature selected games |
| Stats | View total plays and most popular games |

---

## 10. Non Functional Requirements

* Homepage loads under 2.5s on broadband. Catalogue/search API responses under 300ms.
* Layout works on desktop, tablet, mobile.
* Keyboard nav, focus states, alt text on images.
* Page titles + meta descriptions for SEO. HTTPS in production.
* Passwords hashed, inputs validated.

---

## 11. Technology Stack (PERN)

This project is built on the PERN stack.

| Layer | Technology |
| --- | --- |
| Frontend | React (Vite) with React Router |
| Backend | Node.js with Express |
| Database | PostgreSQL |
| Auth | JWT (or express-session) with bcrypt password hashing |
| Game embed | HTML5 iframe with the Fullscreen API |
| Hosting | React static build on a static host, Express API on a Node host, managed PostgreSQL database |

---

## 12. API and Code Naming Conventions

* REST endpoints: lowercase, plural (`/api/games`).
* JSON fields: snake_case (`thumbnail_url`).
* React components: PascalCase (`GameCard`).
* CSS classes: kebab-case (`game-card`).
* Asset filenames: lowercase with hyphens (`pixel-dash.png`).

---

## 13. Folder Structure

| Path | Contents |
| --- | --- |
| plixora/ | Root folder |
| plixora/frontend/ | React app (Vite) |
| plixora/frontend/src/components/ | Reusable React components (GameCard, Header, SearchBar) |
| plixora/frontend/src/pages/ | Route pages (Home, Browse, Play, Search, Login) |
| plixora/frontend/src/assets/ | images, icons, fonts |
| plixora/backend/ | Node and Express API server |
| plixora/backend/routes/ | Express route handlers |
| plixora/backend/models/ | PostgreSQL queries and models |
| plixora/backend/middleware/ | Auth, validation, rate limiting |
| plixora/admin/ | Admin panel (React) |
| plixora/database/ | PostgreSQL schema and seed data |
| plixora/docs/ | Project documentation |

---

## 14. Getting Started

```bash
# 1. Clone
git clone <your-repo-url>
cd plixora

# 2. Backend (Node + Express + PostgreSQL)
cd backend
npm install
# create a PostgreSQL database, then set DATABASE_URL in .env
npm run migrate      # create the tables
npm run seed         # load the starter catalogue
npm run dev          # start the Express API

# 3. Frontend (React)
cd ../frontend
npm install
npm run dev          # start the React app (Vite)
```

---

## 15. Testing Checklist

* [ ] Homepage shows featured, popular, new, and category sections.
* [ ] Category navigation filters the grid correctly.
* [ ] Search returns matching games with live suggestions.
* [ ] Clicking a card opens the play page and loads the game.
* [ ] Full screen works on the play page.
* [ ] Favourites and recently played persist when signed out and sync when signed in.
* [ ] All API endpoints return correct data and status codes.
* [ ] Admin can create, edit, delete, publish, and feature games.
* [ ] Layout is correct on desktop, tablet, and mobile.

---

## 16. Deliverables Checklist

* [ ] Frontend website (all pages in Section 3).
* [ ] Backend REST API (all endpoints in Section 5).
* [ ] Database schema and seed data (Section 6).
* [ ] Admin panel (Section 9).
* [ ] Documentation: overview.txt, scope_of_work.txt, assets.pdf, reference_image.png, instructions.md, rubrics.txt.
* [ ] Public Git repository with the complete project.
