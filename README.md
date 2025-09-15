# Docker Webservices Stack

A Docker Compose stack for running [Nginx Proxy Manager](https://nginxproxymanager.com/) - a simple, powerful proxy server with SSL support, perfect for managing multiple web services behind a single entry point.

This stack provides a containerized reverse proxy solution with automatic SSL certificate management through Let's Encrypt integration.

> **⚠️ Warning:** This is my actual live configuration for my homelab, and it has ports exposed that you may not intend to expose. Do not deploy this in your environment without reviewing the exposed ports and removing any that you do not need. See the [Ports](#ports) section for a list of default ports.

## Prerequisites

Before deploying this stack, ensure you have:

- Docker Engine installed (version 20.10 or higher recommended)
- Docker Compose installed (version 2.0 or higher recommended)
- **External proxy network created** (see [Network Setup](#network-setup) below)

### Network Setup

⚠️ **Important**: This stack requires an external Docker network named `proxy` to be created before deployment.

Create the external proxy network:

```bash
docker network create proxy
```

This network allows the Nginx Proxy Manager to communicate with other containerized services that you want to proxy.

## Deployment

### 1. Clone and Setup

```bash
git clone <your-repo-url>
cd docker-webservices-stack
```

### 2. Environment Configuration

Create a `.env` file in the project directory:

```bash
cp .env.example .env  # If you have an example file, or create manually:
echo "TZ=America/New_York" > .env  # Replace with your timezone
```

Available timezones can be found [here](https://en.wikipedia.org/wiki/List_of_tz_database_time_zones).

### 3. Deploy the Stack

```bash
docker-compose up -d
```

### 4. Initial Setup

After deployment, access the Nginx Proxy Manager web interface:

- **Admin Interface**: http://your-server-ip:81
- **Default Login**:
  - Email: `admin@example.com`
  - Password: `changeme`

⚠️ **Security**: Change the default credentials immediately after first login.

## Configuration

### Ports

The stack exposes the following ports:

- `80` - HTTP traffic (redirect to HTTPS when SSL is configured)
- `443` - HTTPS traffic
- `81` - Admin web interface

### Volumes

- `npm_data` - Application data and configuration
- `npm_letsencrypt` - SSL certificates and Let's Encrypt data

### Environment Variables

- `TZ` - Timezone for the container (defaults to UTC if not set)

## Usage

1. Access the admin interface at `http://your-server-ip:81`
2. Add proxy hosts for your services
3. Configure SSL certificates (automatic with Let's Encrypt)
4. Manage access lists and security settings

## Documentation

For detailed configuration and advanced usage, refer to the official [Nginx Proxy Manager documentation](https://github.com/NginxProxyManager/nginx-proxy-manager).

## Troubleshooting

### Common Issues

- **Cannot connect to admin interface**: Ensure port 81 is not blocked by firewall
- **Proxy network not found**: Run `docker network create proxy` before deployment
- **SSL certificate issues**: Check domain DNS points to your server IP

### Logs

View container logs:

```bash
docker-compose logs -f app
```

## License

This project is open source. Please refer to the [Nginx Proxy Manager license](https://github.com/NginxProxyManager/nginx-proxy-manager/blob/develop/LICENSE) for details.
