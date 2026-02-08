# Deploying LumenRoute to DigitalOcean

There are two recommended ways to deploy this application:
1.  **Dropet (Virtual Machine) with Docker Compose** (Recommended for Hackathons)
2.  **App Platform** (Serverless/Managed)

## Option 1: Droplet + Docker Compose (Easiest)

This method gives you a full virtual machine where you can run the exact same Docker setup as local.

1.  **Create a Droplet**:
    - Choose **Marketplace** -> **Docker** (Ubuntu with Docker pre-installed).
    - Choose a size (Basic $6/mo is fine).
    - Add your SSH key.

2.  **SSH into the Droplet**:
    ```bash
    ssh root@<your-droplet-ip>
    ```

3.  **Clone the Repository**:
    ```bash
    git clone https://github.com/your-username/UGAHacksXI.git
    cd UGAHacksXI
    ```

4.  **Configure Environment**:
    ```bash
    cp .env.example .env
    nano .env
    # Paste your API keys (Gemini, Google Maps, OpenWeather)
    # Save with Ctrl+X, Y, Enter.
    ```

5.  **Run the App**:
    ```bash
    docker compose up -d --build
    ```

6.  **Access**:
    Open `http://<your-droplet-ip>` in your browser.

---

## Option 2: DigitalOcean App Platform

This method is fully managed but requires slightly more configuration for the monorepo structure.

1.  **Create App**:
    - Connect your GitHub repository.
    - Providing access to the repository.

2.  **Configure Services**:
    - **Service 1 (Backend)**:
        - Source Directory: `/backend`
        - Build Command: `bun install`
        - Run Command: `bun run src/index.ts`
        - HTTP Port: `3000`
        - Environment Variables: Add your API keys here.

    - **Service 2 (Frontend)**:
        - Source Directory: `/webapp`
        - Build Command: `npm install && npm run build`
        - Output Directory: `dist`
        - Type: **Static Site**
        - **IMPORTANT**: You must configure the frontend to point to the backend URL.
        - Add an Environment Variable `VITE_API_URL` pointing to your Backend Service URL.

---

## Troubleshooting

- **Ports**: 
  - Ensure Firewall rules on DigitalOcean allow inbound traffic on port 80 (HTTP) and 443 (HTTPS).
- **Backend Connection**:
  - If the frontend cannot reach the backend, check the browser console.
  - In Docker Compose, the Nginx proxy handles this automatically (`/api` -> `backend:3000`).
