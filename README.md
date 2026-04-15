# Reclip for Synology Container Manager

Docker Compose configuration to run [Reclip](https://github.com/averygan/reclip) on Synology NAS using Container Manager.

## What is Reclip?
Reclip is a web-based video downloader that allows you to download videos from various platforms using yt-dlp. It provides a simple web interface to download videos and manage your downloads.

## Prerequisites
- Synology NAS with Container Manager installed
- Port 8899 available

## Installation

1. Create the required folders on your NAS:
   ```bash
   mkdir -p /volume1/docker/reclip/downloads
   ```

2. Download the `docker-compose.yml` file to `/volume1/docker/reclip/`

3. **Edit the volume paths** in the file if needed:
   - Change `/volume1/docker/reclip` to your preferred path
   - Update the timezone in `TZ=` if you're not in Europe/Rome

4. Open Container Manager on your Synology NAS

5. Go to **Project** → **Create** → select the `docker-compose.yml` file

6. Start the project

## Access
Once started, access Reclip at:
```
http://[YOUR-NAS-IP]:8899
```

## Features
- Automatic Reclip installation on first run
- Downloads saved to persistent storage
- Auto-restart on crash or NAS reboot
- ffmpeg support for video processing

## Configuration

### Volume Paths
- `/volume1/docker/reclip:/app` - Application files
- `/volume1/docker/reclip/downloads:/app/downloads` - Downloaded videos

### Environment Variables
- `TZ=Europe/Rome` - Set your timezone (e.g., `America/New_York`, `Asia/Tokyo`)
- `DISPLAY=:0` - Display variable for GUI applications

### Port
- `8899:8899` - Web interface port (change if port 8899 is already in use)

## How It Works
On first container start:
1. Installs ffmpeg and git
2. Clones Reclip from GitHub
3. Installs Python dependencies (yt-dlp, flask, requests, flask-cors)
4. Patches app.py to listen on all interfaces (0.0.0.0)
5. Starts the Flask web server

On subsequent starts, it skips the installation and runs directly.

## Troubleshooting

### Container won't start
- Check logs in Container Manager
- Verify that port 8899 is not already in use
- Ensure the volume paths exist and have proper permissions

### Downloads not saving
- Check that the downloads folder has write permissions
- Verify the volume mount paths are correct

### Can't access the web interface
- Make sure you're using the correct NAS IP address
- Check if firewall rules are blocking port 8899
- Try accessing from `http://localhost:8899` if browsing from the NAS itself

## Updating Reclip
To update to the latest version:
1. Stop the container in Container Manager
2. Delete the contents of `/volume1/docker/reclip/` (except the downloads folder)
3. Restart the container - it will re-clone the latest version

## License
This docker-compose configuration is provided as-is. Reclip itself is licensed under its own terms - see the [original repository](https://github.com/averygan/reclip).

## Credits
- [Reclip](https://github.com/averygan/reclip) by averygan
- [yt-dlp](https://github.com/yt-dlp/yt-dlp) - Video downloader
