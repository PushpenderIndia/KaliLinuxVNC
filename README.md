# Kali Linux Docker Container with Desktop Access

[![Build Status](https://github.com/PushpenderIndia/KaliLinuxVNC/actions/workflows/build.yml/badge.svg)](https://github.com/PushpenderIndia/KaliLinuxVNC/actions/workflows/build.yml)

A lightweight Docker container running Kali Linux with a complete desktop environment accessible via VNC and noVNC for browser-based remote access.

## Features

- **Full Kali Linux Desktop**: Complete XFCE desktop environment
- **Browser Access**: noVNC web interface for easy access
- **VNC Support**: Traditional VNC client compatibility
- **SSL/TLS Security**: Self-signed certificates for encrypted connections
- **Configurable**: Customizable metapackages, desktop environments, and display settings
- **Lightweight**: Optimized for minimal resource usage

## Quick Start

### Using Docker Compose (Recommended)

```bash
# Build and start the container
docker-compose up --build

# Run in background
docker-compose up -d

# Stop the container
docker-compose down
```

### Using Docker (Alternative)

```bash
# Build the container
docker build -t kali .

# Run the container
docker run --rm -it --name kali -p 9020:8080 -p 9021:5900 kali
```

### Access the Desktop

Open your web browser and navigate to:
```
https://localhost:9020/vnc.html
```

Default VNC password: `kali`

## Configuration Options

### Environment Variables

| Variable | Default | Description |
|----------|---------|-------------|
| `VNCEXPOSE` | `0` | Set to `1` to expose VNC port for external clients |
| `VNCPORT` | `5900` | VNC server port within container |
| `VNCPWD` | `kali` | VNC authentication password |
| `VNCDISPLAY` | `2560x1440` | Desktop resolution |
| `VNCDEPTH` | `16` | Color depth (8, 16, 24, or 32) |
| `NOVNCPORT` | `8080` | noVNC web server port within container |

### Example with Custom Configuration

#### Using Docker Compose

Create or modify `docker-compose.yml`:

```yaml
version: '3.8'

services:
  kali:
    build: .
    container_name: kali
    ports:
      - "9020:8080"
      - "9021:5900"
    environment:
      - VNCEXPOSE=1
      - VNCPWD=mypassword
      - VNCDISPLAY=1920x1080
      - VNCDEPTH=24
```

#### Using Docker Run

```bash
docker run -it --rm \
  -p 9020:8080 \
  -p 9021:5900 \
  -e VNCEXPOSE=1 \
  -e VNCPWD=mypassword \
  -e VNCDISPLAY=1920x1080 \
  -e VNCDEPTH=24 \
  kali
```

## Access Methods

### Web Browser (Recommended)

Access via noVNC web interface at `https://localhost:9020/vnc.html`

- No additional software required
- SSL/TLS encrypted connection
- Works on any modern browser

### VNC Client

For traditional VNC client access:

1. Set `VNCEXPOSE=1` in your configuration
2. Install a VNC viewer ([RealVNC](https://www.realvnc.com/en/connect/download/viewer/), TigerVNC, etc.)
3. Connect to `localhost:9021`
4. Use the configured password (default: `kali`)

## Customization

### Desktop Environments

Modify the `KALI_DESKTOP` argument in the Dockerfile:

- `xfce` (default)
- `mate`
- `gnome` 
- `kde`

### Kali Linux Metapackages

Update the `KALI_METAPACKAGE` argument in the Dockerfile:

- `core` - Essential tools only
- `default` - Standard Kali tools
- `light` - Lightweight selection
- `large` - Extended tool collection
- `everything` - Complete tool suite
- `top10` - Most popular tools (default)

For more information about metapackages, visit the [official Kali documentation](https://www.kali.org/news/major-metapackage-makeover/).

## SSL Certificates

### Default Behavior

The container automatically generates self-signed certificates for SSL/TLS encryption.

### Custom Certificates

Mount your own certificates using volumes:

```bash
-v /path/to/cert.pem:/etc/ssl/certs/novnc_cert.pem \
-v /path/to/key.pem:/etc/ssl/private/novnc_key.pem
```

Generate new certificates:
```bash
openssl req -new -x509 -days 365 -nodes \
  -out cert.pem -keyout key.pem \
  -subj "/C=US/ST=State/L=City/O=Organization/CN=localhost"
```

## Security Considerations

- Change the default VNC password in production environments
- Use custom SSL certificates for public deployments
- Consider network isolation for sensitive workloads
- Keep the container updated with the latest Kali Linux packages

## Troubleshooting

### Connection Issues

1. Verify port mappings match your configuration
2. Check firewall settings on the host system
3. Ensure the container is running: `docker ps`

### Certificate Warnings

Browser certificate warnings are normal with self-signed certificates. You can:
- Accept the security exception
- Use custom certificates from a trusted CA
- Add the certificate to your system's trust store

### Performance Optimization

- Reduce `VNCDEPTH` for slower connections
- Lower `VNCDISPLAY` resolution for better performance
- Use VNC client instead of browser for improved responsiveness

## Docker Hub

Pre-built images are available at: [pushpenderindia](https://hub.docker.com/u/pushpenderindia)

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## Support

For issues and questions:
- Check the [troubleshooting section](#troubleshooting)
- Review existing [GitHub issues](https://github.com/PushpenderIndia/KaliLinuxVNC/issues)
- Create a new issue with detailed information