# Nutrilore Storefront

A lightweight single-page storefront with cart persistence and Express backend checkout.

## Local development

1. Install dependencies:
   ```powershell
   npm.cmd install
   ```
2. Start the server:
   ```powershell
   npm.cmd start
   ```
3. Open the site in your browser:
   ```text
   http://localhost:3000
   ```

## UI test

Run the automated UI persistence and checkout validation:
```powershell
npm.cmd run test:ui
```

## Production deployment

### Access from other devices on your local network

The app listens on all network interfaces by default, so other devices on the same Wi-Fi or LAN can access it using your machine's local IP address.

1. Find your IPv4 address in PowerShell:
   ```powershell
   ipconfig
   ```
2. Look for `IPv4 Address` on your active adapter, for example:
   ```text
   192.168.1.10
   ```
3. Open the app from another device using:
   ```text
   http://192.168.1.10:3001
   ```

If you run the app on a different port, replace `3001` with that port.

> Note: `http://localhost:3001` only works on the same computer.

### Docker

Build the image:
```powershell
docker build -t nutrilore-site .
```

Run the container:
```powershell
docker run --rm -p 3000:3000 nutrilore-site
```

The container persists backend product and order state in the `data/` directory inside the app.

### Docker Compose

Start the app with Docker Compose:
```powershell
docker compose up --build
```

Stop it with:
```powershell
docker compose down
```

This mounts `./data` into the container so product stock and orders survive restarts.

## GitHub-based production deployment

### Setup

1. Push your code to GitHub:
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin https://github.com/Pareedirfan366/nutrilore-site.git
   git push -u origin main
   ```

2. A GitHub Actions workflow (`.github/workflows/deploy.yml`) will automatically:
   - Run on every push to `main`
   - Install dependencies
   - Run the UI test suite
   - Build the Docker image

### Deploy to Railway

1. Go to [railway.app](https://railway.app) and sign in with GitHub.
2. Create a new project → select **Deploy from GitHub repo**.
3. Choose your `nutrilore-site` repository.
4. Railway auto-detects `Dockerfile` and deploys.
5. Your site is live at the Railway URL.

### Deploy to Render

1. Go to [render.com](https://render.com) and sign in with GitHub.
2. Create a new **Web Service** → select your GitHub repo.
3. Set build command: `npm install --omit=dev`
4. Set start command: `node server.js`
5. Render auto-deploys on every push to `main`.

### Deploy to Heroku (legacy)

1. Install [Heroku CLI](https://devcenter.heroku.com/articles/heroku-cli).
2. Connect to GitHub:
   ```bash
   heroku login
   heroku create nutrilore-site
   git push heroku main
   ```
3. Your site is live at `https://nutrilore-site.herokuapp.com`.

### Environment variables

Create a `.env` file (or use your platform's secrets manager) with:
```
PORT=3000
NODE_ENV=production
```

See `.env.example` for a template.

### Notes

- The backend now saves `data/products.json` and `data/orders.json`.
- Product stock and orders survive server restarts.
