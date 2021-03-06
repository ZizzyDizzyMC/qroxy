# Vroxy
Self-hosted light-weight server proxy for YoutubeDL, originally designed for use in VRChat.

Currently in alpha.

Prerequisites:
- Ensure you have a VPS or better with a linux OS on it (debian is recommended). You will need to be able to run commands with escalated privilege.
- Ensure you have a domain name setup correctly with the desired DNS A entry pointing to your server's public IP address.

Debian/Ubuntu Setup:
- Pull the repo's install script and run it:  
    - `wget -q https://raw.githubusercontent.com/techanon/vroxy/master/vroxy_install_deb.sh -O vroxy_install.sh && sudo bash ./vroxy_install.sh`
    - This will pull in all dependencies and setup the nginx reverse proxy and SSL certs for you.
    - The script will use `/var/vroxy` as the install folder by default, but you can specify another location if you wish.
    - If running on an OS without the sudo command, you will either need to login as root `su -` or install sudo and add yourself as a sudo user.
- Run the reload script to update and (re)start the service: `bash /var/vroxy/vroxy_reload.sh`
- You can examine the service log with: `tmux a -t vroxy`
- Exit tmux with `CTRL+B` followed by the `D` key.

Generic Setup:
- Setup a public facing server on your VPS or whatever.
    - This requires something like Apache or Nginx and some SSL cert assigned (using LetsEncrypt's certbot tool is the recommended option)
    - Quest _requires_ HTTPS, so the SSL cert is a must.
- Install python3 (make sure the `pip` tool is also installed)
- Clone this repo to the server at your desired file path. `/var/vroxy` is recommended.
- Navigate your terminal to the given folder that Vroxy was cloned into.
- Install the dependant packages for python via `python3 -m pip install -U yt-dlp aiohttp`
- Copy example.ini to settings.ini and change the settings as you need
- Run the server via `python3 /path/to/repo/vroxy.py`
    - You may want to consider a terminal multiplexer (like tmux) to run the service without needing to be connected to the terminal.

Usage:
Access the url via `https://mydomain/?url=https://youtube.com/watch?v=VIDEO_ID` to receive a 307 redirect to the direct link video url.

Optional parameters:
- `f`: Specific format id for the given url that is desired. This is something that is looked up ahead of time via manually checking --list-formats in ytdl.
- `s`: Custom sort order for ytdl to determine what it thinks is 'best'. Sort order options can be found [Here](https://github.com/yt-dlp/yt-dlp/blob/release/README.md#sorting-formats)
- `m`: Specific presets for sort order options
    - `0` - aka "hasvid,hasaud,res:1440" for decent sized media with audio+video, generally compatible with all platforms
    - `1` - aka "hasvid,hasaud,res" for sort preferring audio+video with the highest quality
    - `2` - aka "hasvid,hasaud,+res" for sort preferring audio+video with the lowest quality
    - `3` - aka "hasvid,res,codec:vp9" for sort preferring highest quality with priority on VP9 codec for platform compatibility
    - `4` - aka "hasvid,res" for sort preferring highest quality without concern for codec or audio


TODO: Add discord embed support ala https://github.com/robinuniverse/TwitFix/blob/main/templates/index.html because why not?