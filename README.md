# n8n
Automated Python script for installing n8n with Nginx reverse proxy and SSL (Certbot/Cloudflare) on Debian-based systems.One-command setup for n8n, Nginx, and Let's Encrypt SSL on Debian/Ubuntu using Python.Python script to simplify the deployment of a self-hosted n8n instance with Nginx and SSL.

# n8n Nginx Debian Setup Script

This Python script automates the installation and configuration of a self-hosted n8n instance with Nginx as a reverse proxy and Let's Encrypt SSL certificates (via Certbot using the Cloudflare DNS challenge) on Debian-based systems (e.g., Debian, Ubuntu).

The goal is to simplify the setup process, getting you up and running with a secure n8n instance with minimal manual intervention.

## Features

* **Automated n8n Installation:** Installs the latest version of n8n globally via npm.
* **Node.js Installation:** Sets up Node.js v20.x from NodeSource.
* **Nginx Reverse Proxy:** Configures Nginx to serve n8n, handling incoming traffic.
* **SSL Certificate:** Obtains and configures a free SSL certificate from Let's Encrypt using Certbot with the Cloudflare DNS-01 challenge method. This means you don't need to open port 80 to the internet for HTTP-01 challenges if your server is behind a strict firewall.
* **Systemd Service:** Creates a systemd service for n8n to ensure it runs on boot and restarts if it crashes.
* **Security Headers:** Adds basic security headers to the Nginx configuration.

## Prerequisites

* A Debian-based system (e.g., Debian 11/12, Ubuntu 20.04/22.04).
* **Root access:** The script must be run as root (`sudo python3 install_n8n_nginx.py`).
* A registered domain name.
* Your domain's DNS managed by **Cloudflare**.
* **Cloudflare API Credentials:**
    * Either an **API Token** (Recommended: Create a token with `Zone:DNS:Edit` permissions for the specific zone/domain you are using).
    * Or your **Global API Key** and Cloudflare account email (Less recommended for security).
* Python 3 installed on the system (usually present by default on modern Debian/Ubuntu). The script will install necessary Python packages for Certbot plugins.

## How to Use

1.  **Clone the repository or download the script:**
    ```bash
    git clone [https://github.com/YOUR_USERNAME/YOUR_REPOSITORY_NAME.git](https://github.com/YOUR_USERNAME/YOUR_REPOSITORY_NAME.git)
    cd YOUR_REPOSITORY_NAME
    ```
    Or download `install_n8n_nginx.py` directly.

2.  **Make the script executable (optional but good practice):**
    ```bash
    chmod +x install_n8n_nginx.py
    ```

3.  **Run the script as root:**
    ```bash
    sudo python3 install_n8n_nginx.py
    ```

4.  **Follow Prompts:** The script will guide you through providing:
    * Your n8n domain (e.g., `n8n.yourdomain.com`).
    * Your email address (for Let's Encrypt registration and renewal notices).
    * Your Cloudflare API credentials.

## Important Security Notes

* **Cloudflare Credentials File:** The script will create a Cloudflare credentials file (e.g., `/root/.secrets/cloudflare.ini`) with `600` permissions. While the script provides a warning, it's **your responsibility** to secure or remove this file after the script successfully completes if you don't want it to persist.
* **n8n User:** By default, for simplicity matching the troubleshooting process, the n8n service is configured to run as the `root` user. **This is NOT recommended for production environments.**
    * **Recommendation:** Modify the script (see `DEFAULT_N8N_USER`, `DEFAULT_N8N_GROUP` variables and the `setup_n8n_user_and_env` function) or the generated `/etc/systemd/system/n8n.service` file to run n8n as a dedicated, unprivileged user (e.g., a user named `n8n`). Ensure this user owns the n8n data directory (e.g., `/home/n8n/.n8n`).
* **Review Script:** Always review any script that requires root privileges before running it on your system to understand the changes it will make.
* **Firewall:** The script includes basic UFW (Uncomplicated Firewall) setup if UFW is detected. If you use a different firewall or have specific cloud provider firewall rules, ensure ports 80 (for initial HTTP to HTTPS redirect) and 443 (for HTTPS) are open to your server.

## Post-Installation Checks

After the script completes:

* Access n8n at `https://your-n8n-domain.com`.
* Check n8n service status: `sudo systemctl status n8n`
* View n8n logs: `sudo journalctl -u n8n -f`
* Check Nginx status: `sudo systemctl status nginx`
* View Nginx error logs: `cat /var/log/nginx/error.log`
* Test SSL certificate renewal (dry run): `sudo certbot renew --dry-run`

## Contributing (Optional)

If you'd like to contribute, please fork the repository and use a feature branch. Pull requests are warmly welcome.

## License (Optional)

Consider adding a license file (e.g., MIT, Apache 2.0). For example, create a `LICENSE` file and put the text of the MIT license in it if you choose that.
Image for your "n8n script" GitH
