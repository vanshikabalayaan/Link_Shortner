# Link Shortener

A full-stack URL shortener. Paste a long link and get a short one that redirects to it. Anyone can shorten a link. Logged-in users can also choose a custom slug and see all their links in a dashboard.

## Features

- **Shorten URLs**: generates a random 7-character ID with `nanoid`, no account needed.
- **Custom slugs**: logged-in users can pick their own short path, and duplicate slugs are rejected.
- **Redirects**: opening `/<short_id>` redirects to the original URL.
- **Authentication**: register, log in and log out with a JWT stored in an httpOnly cookie.
- **Dashboard**: logged-in users see all their short links and can copy them.

## Tech Stack

| Layer    | Technologies |
| -------- | ------------ |
| Frontend | React 19, Vite, TanStack Router, TanStack Query, Redux Toolkit, Axios, Tailwind CSS 4 |
| Backend  | Node.js, Express 5, MongoDB (Mongoose), JWT, bcryptjs, nanoid |

## Project Structure

```
Link_Shortner/
├── BACKEND/
│   ├── app.js                    # Entry point (port 3000), middleware, routes
│   └── src/
│       ├── config/               # MongoDB connection, cookie options
│       ├── controller/           # auth, short_url, user controllers
│       ├── dao/                  # database access (short urls, users)
│       ├── middleware/           # auth middleware (JWT)
│       ├── models/               # shortUrl, user
│       ├── routes/               # auth, user, short_url routes
│       ├── services/             # auth + short url business logic
│       └── utils/                # error handler, JWT helpers, attachUser
└── FRONTEND/
    └── src/
        ├── api/                  # API calls
        ├── components/           # forms, navbar, user url list
        ├── pages/                # Home, Auth, Dashboard
        ├── routing/              # TanStack Router route tree
        └── store/                # Redux store and auth slice
```

## Getting Started

### Prerequisites

- Node.js 18 or newer
- A MongoDB database (local or MongoDB Atlas)

### 1. Clone the repository

```bash
git clone https://github.com/vanshikabalayaan/Link_Shortner.git
cd Link_Shortner
```

### 2. Set up the backend

```bash
cd BACKEND
npm install
```

Create a `BACKEND/.env` file:

```env
MONGO_URI=your_mongodb_connection_string
JWT_SECRET=a_long_random_secret
APP_URL=http://localhost:3000/
```

`APP_URL` must end with a trailing slash. It is prepended to the short ID to build the link returned to the client.

Start the server:

```bash
npm run dev
```

The API runs at `http://localhost:3000`.

### 3. Set up the frontend

In a new terminal:

```bash
cd FRONTEND
npm install
npm run dev
```

The app runs at `http://localhost:5173`. The backend only allows CORS requests from this origin.

## API Reference

| Method | Endpoint         | Access  | Description |
| ------ | ---------------- | ------- | ----------- |
| POST   | `/api/create`    | Public  | Create a short URL. Body: `{ "url": "...", "slug": "..." }`. `slug` is only used when logged in |
| GET    | `/:id`           | Public  | Redirect to the original URL |
| POST   | `/api/auth/register` | Public  | Register a new user |
| POST   | `/api/auth/login`    | Public  | Log in and set the auth cookie |
| POST   | `/api/auth/logout`   | Public  | Clear the auth cookie |
| GET    | `/api/auth/me`       | Private | Get the current user |
| POST   | `/api/user/urls`     | Private | List all short URLs of the logged-in user |

## Scripts

**Backend**

| Command       | Description |
| ------------- | ----------- |
| `npm run dev` | Start the server with nodemon |
| `npm start`   | Start the server with node |

**Frontend**

| Command           | Description |
| ----------------- | ----------- |
| `npm run dev`     | Start the Vite dev server |
| `npm run build`   | Build for production |
| `npm run preview` | Preview the production build |
| `npm run lint`    | Run ESLint |

## Notes

- Never commit your `.env` file.
- The API URL (`http://localhost:3000`) is hardcoded in `FRONTEND/src/utils/axiosInstance.js` and `FRONTEND/src/components/UserUrl.jsx`. Change it there when you deploy.
- Clicks are stored per link (`clicks` field), but the redirect does not increment the counter yet.
