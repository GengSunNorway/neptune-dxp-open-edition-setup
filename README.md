# Neptune DXP Open Edition Setup

## Introduction

This project documents the setup and configuration process for Neptune DXP - Open Edition on a local machine. It includes steps for installation, optional configurations, and connecting to a database.

## Installation

### Downloading Neptune DXP

Neptune DXP can be installed using either a binary file or a Docker image. Here are the steps for both methods:

#### Binary Installation

1. Download the binary file from [Neptune Software](https://portal.neptune-software.com/launchpad/portal#product-download).
2. Extract the files and run the installation script according to the platform-specific instructions provided in the download.

#### Docker Installation

1. Pull the latest image from Docker Hub:

```shell
docker pull neptunesoftware/planet9:v21.10.10
```

> [!TIP]

> "**Warning**: The requested image's platform (linux/amd64) does not match the detected host platform (linux/arm64/v8) and no specific platform was requested." This warning indicates that the requested image's platform (`linux/amd64`) does not match the detected host platform (`linux/arm64/v8`). This situation is common when running AMD64 images on ARM64 architecture (e.g., Apple Silicon Macs). Docker can still run the image through emulation, but it may lead to decreased performance. If available, consider using an image built specifically for `arm64` to avoid potential compatibility issues.

2. Run the Docker image:

```shell
docker run -d -p 8080:8080 neptunesoftware/planet9:v21.10.10
```

3. Access the application at `http://localhost:8080` with the default credentials `admin/admin`.

## Optional Configurations

### Updating the Hosts File

To access Open Edition using a friendly domain name:

1. Edit your system's `hosts` file.
2. Add the following line: `127.0.0.1 open-edition.demo`.
3. Now, you can access the application at `http://open-edition.demo:8080`.

### Setting Up a Reverse Proxy with Nginx

1. Install Nginx.
2. Configure Nginx to forward requests to the Open Edition server.

   The Nginx configuration file is located in the `config/nginx.conf` directory. This file includes the server setup to forward requests to the Neptune DXP Open Edition web server. Refer to the [Nginx documentation](https://nginx.org/en/docs/) for how to apply this configuration.

```shell
server {
    listen 80;
    server_name open-edition.demo;

    location / {
        proxy_pass http://localhost:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

3. Restart Nginx to apply the changes.

### Connecting to PostgreSQL

1. Install PostgreSQL.
2. Create a database for Open Edition.
3. Configure Open Edition to use the PostgreSQL database by editing the application's configuration files.

## Conclusion

This guide outlines the initial setup for Neptune DXP Open Edition. For detailed documentation and advanced configurations, please refer to the official [Neptune DXP documentation](https://docs.neptune-software.com/neptune-dxp-open-edition/23/index.html).
