# Workflow to Setup YT-DLP

```bash
# Github page for YT-DLP (You may find other parameters and command line to try out with yt-dlp)
https://github.com/yt-dlp/yt-dlp

# Make sure you got AV1 codec installed to be able to play the video locally
# If using Windows you may need to install AV1 Video Extension using Microsoft Store

# If using Linux ( Fedora Atomic specifically )
# AMD GPU
rpm-ostree install mesa-va-drivers-freeworld mesa-vulkan-drivers-freeworld
# Intel GPU
rpm-ostree install intel-media-driver
# NVIDIA GPU
rpm-ostree install nvidia-vaapi-driver
rpm-ostree install libva-utils
# Verification on Linux
# After rebooting:
vainfo | grep -i AV1

# 1. Install core dependencies via Winget
# Reset it before installing
winget source reset --force

winget install yt-dlp.yt-dlp --source winget
winget install Gyan.FFmpeg --source winget
winget install DenoLand.Deno --source winget
winget install OpenJS.NodeJS --source winget

# Verify Deno installation
deno --version

# 2. Run your yt-dlp commands (These remain identical as the flags are universal)
# Replace this URL with the YouTube Video's URL which you would like to download
# Replace it from "https://www.youtube.com/watch?v=UvV74ex-02M" to "Your_YouTube_Videos_URL"
# Video and Audio download
yt-dlp --remote-components ejs:github --no-playlist --cookies-from-browser firefox --extractor-args "youtube:player_client=android_vr,web_embedded" -f "bestvideo+bestaudio/best" --merge-output-format mkv --postprocessor-args "ffmpeg:-c:v copy -c:a flac" "https://www.youtube.com/watch?v=UvV74ex-02M"
yt-dlp --remote-components ejs:github --no-playlist --cookies-from-browser firefox --extractor-args "youtube:player_client=android_vr,web_embedded" -f "bestvideo+bestaudio/best" --merge-output-format mkv --postprocessor-args "ffmpeg:-c:v copy -c:a flac" "Your_Video_URL"

# Audio-only download
yt-dlp --remote-components ejs:github --no-playlist --cookies-from-browser firefox --extractor-args "youtube:player_client=android_vr,web_embedded" -f "bestaudio" -x --audio-format flac "https://www.youtube.com/watch?v=xZ3-_Tx0Kfg"
yt-dlp --remote-components ejs:github --no-playlist --cookies-from-browser firefox --extractor-args "youtube:player_client=android_vr,web_embedded" -f "bestaudio" -x --audio-format flac "Your_Video-URL"
```
