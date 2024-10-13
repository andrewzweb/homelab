# OpenWebUi

This project sets up a homelab service using Docker Compose to run two main services: open-webui and pipelines. These services provide a web interface and backend for interacting with various AI pipelines.

### Prerequisites
Before setting up this project, make sure you have the following installed:

Docker
Docker Compose
An OpenAI API key

## Setup

### 1. Clone the repository (or set up the directory for your Docker Compose file):

```bash
git clone https://github.com/your-repository/homelab-service.git
cd homelab-service
```

### 2. Create an .env file in the project root and add your OpenAI API key:

```bash
OPENAI_API_KEY=your-openai-api-key
```

### 3. Prepare the Docker Compose file: Ensure the docker-compose.yml file looks like this:

```yaml
version: '3'

services:
  open-webui:
    image: ghcr.io/open-webui/open-webui:main
    container_name: open-webui
    restart: always
    ports:
      - "3001:8080"
    volumes:
      - ./open-webui:/app/backend/data
    environment:
      - WEBUI_AUTH=True
      - OPENAI_API_KEY=${OPENAI_API_KEY}
      - NETWORKING_ALLOWED=True
    networks:
      - app-network
    security_opt:
      - seccomp:unconfined

  pipelines:
    image: ghcr.io/open-webui/pipelines:main
    container_name: pipelines
    restart: always
    ports:
      - "9099:8080"
    environment:
      - OPENAI_API_KEY=${OPENAI_API_KEY}
      - PIPELINE_BACKEND_URL=http://open-webui:3001
      - NETWORKING_ALLOWED=True
    depends_on:
      - open-webui
    volumes:
      - ./open-webui:/app/backend/data
    networks:
      - app-network
    security_opt:
      - seccomp:unconfined

volumes:
  open-webui:

networks:
  app-network:
    external: false
```

### 4. Start the services:

Run the following command to bring up the services:

```bash
docker-compose up -d
```

This will pull the necessary Docker images and start the services in detached mode.

### 5. Access the services:

Open WebUI: Visit http://localhost:3001 to access the open-webui service.
Pipelines: The pipelines service runs on http://localhost:9099 and depends on the open-webui service.

## Services

### Open WebUI
This service provides the web interface for managing and interacting with backend pipelines. It's authenticated and uses the OpenAI API key provided in the environment file.

### Pipelines
The pipelines service handles various AI-related tasks and interacts with open-webui via the backend URL defined in the environment variables.

## Configuration
Networking Allowed
The NETWORKING_ALLOWED=True environment variable ensures that the services can perform network-related operations, like communicating with external services.

## Security Considerations

`seccomp:unconfined:` This setting disables Docker's default Seccomp security profile. This is required for the services to function but reduces the security isolation of the containers. Use it with caution.
Volumes

The open-webui volume is mounted to persist application data. This ensures that the service’s data is not lost when the container restarts.

Stopping the Services
To stop the services, use:

```bash
docker-compose down
```

This will stop and remove the containers, but the data will remain intact.

## Troubleshooting
If the services don’t start properly, check the logs:

```bash
docker-compose logs
```

Ensure that your .env file is properly configured with a valid OpenAI API key.

## License
This project is licensed under the MIT License. See the LICENSE file for details.

## Contributing
Feel free to submit issues or pull requests to contribute to this project.
