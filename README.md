# Ollama Stack with Cloudflare Tunnel

This project provides a Docker Compose setup to run a self-hosted [Ollama](https://ollama.ai/) instance and expose it securely to the internet using a [Cloudflare Tunnel](https://www.cloudflare.com/products/tunnel/).

## 🚀 Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes.

### Prerequisites

*   [Docker](https://docs.docker.com/get-docker/) and [Docker Compose](https://docs.docker.com/compose/install/)
*   A [Cloudflare](https://www.cloudflare.com/) account
*   A Cloudflare Tunnel token

### Installation

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/chriskyfung/ollama-stack.git
    cd ollama-stack
    ```

2.  **Set up Cloudflare Tunnel:**
    *   Follow the instructions [here](https://developers.cloudflare.com/cloudflare-one/connections/connect-apps/install-and-run-tunnel/run-tunnel/) to create a tunnel and get your token.
    *   Create a `.secrets` directory in the project root.
    *   Create a file named `tunnel_token.txt` inside the `.secrets` directory.
    *   Paste your Cloudflare Tunnel token into this file.

    The directory structure should look like this:
    ```
    .
    ├── .secrets/
    │   └── tunnel_token.txt
    ├── docker-compose.yaml
    └── ...
    ```

## 🛠️ Usage

*   **Start the services:**
    ```bash
    docker-compose up -d
    ```
    This will start the Ollama and Cloudflared services in detached mode.

*   **Access Ollama shell:**
    ```bash
    docker-compose -it exec ollama /bin/bash
    ```

*   **View logs:**
    ```bash
    docker-compose logs -f
    ```

*   **Stop the services:**
    ```bash
    docker-compose down
    ```

## ⚙️ Configuration

The main configuration is located in the `docker-compose.yaml` file. You can adjust the Ollama environment variables and resource allocation as needed.

### Services

*   **`ollama`**: The Ollama service. The default configuration is set for a machine with approximately 24GB of RAM available for the models.
*   **`cloudflared`**: The Cloudflare Tunnel service. It connects to the `ollama` service and exposes it to the internet.

### Local Overrides

You can use the `docker-compose.override.yaml` file to apply your own local configurations without modifying the main compose file. For example, you can change the `OLLAMA_ORIGINS` or network settings.

## 🧠 CPU and Memory Resources

The `docker-compose.yaml` includes resource allocation settings for the `ollama` service under the `deploy` key.

```yaml
deploy:
  resources:
    limits:
      cpus: '3.4'  # ~75% of 4 OCPU
      memory: 18G
    reservations:
      memory: 16G
```

*   **`limits`**: These are the maximum resources the container can use. The `ollama` service will be stopped if it tries to exceed these limits.
*   **`reservations`**: These are the resources that are guaranteed to be available to the container.

The default values are tuned for a system with at least 24GB of RAM and 4 CPU cores. You should adjust these values based on your own system's specifications and the models you plan to run.

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
