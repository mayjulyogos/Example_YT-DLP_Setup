# Workflow to Setup YT-DLP

<br>

## Install AV1 codec
* These following command lines are specifically written for Fedora Atomic
* You may change the packages name and package manager depends on the distro which you use

### AMD GPU
```bash
rpm-ostree install mesa-va-drivers-freeworld mesa-vulkan-drivers-freeworld
```
### Intel GPU
```bash
rpm-ostree install intel-media-driver
```
### NVIDIA GPU
```bash
rpm-ostree install nvidia-vaapi-driver
rpm-ostree install libva-utils
```

### Verification on Linux
```bash
# After rebooting:
vainfo | grep -i AV1
```

<br>

---

<br>

## 1. Enable RPM Fusion repositories (Required for full FFmpeg codecs)
```bash
sudo rpm-ostree install \
  https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm \
  https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

<br>

## 2. Swap ffmpeg-free libraries with full ffmpeg AND layer all required packages in a single transaction
```bash
sudo rpm-ostree override remove \
  ffmpeg-free \
  libavcodec-free \
  libavdevice-free \
  libavfilter-free \
  libavformat-free \
  libavutil-free \
  libswresample-free \
  libswscale-free \
  --install ffmpeg \
  --install yt-dlp \
  --install nodejs \
  --install gstreamer1-plugins-ugly
```

<br>

## 3. Reboot to apply system image changes
```bash
systemctl reboot
```

<br>

## 4. Install Deno via official script into user space (~/.deno)
```bash
curl -fsSL https://deno.land/install.sh | sh

# Verify Deno installation
deno --version
```

<br>

## 5. Run yt-dlp commands
* Run your yt-dlp commands (These remain identical as the flags are universal)
* Replace this URL with the YouTube Video's URL which you would like to download
* Replace it from "https://www.youtube.com/watch?v=UvV74ex-02M" to "Your_YouTube_Videos_URL"

### Video and Audio download
```bash
yt-dlp --remote-components ejs:github --no-playlist --cookies-from-browser firefox --extractor-args "youtube:player_client=android_vr,web_embedded" -f "bestvideo+bestaudio/best" --merge-output-format mkv --postprocessor-args "ffmpeg:-c:v copy -c:a flac" "[https://www.youtube.com/watch?v=UvV74ex-02M](https://www.youtube.com/watch?v=kp0nbIAnhn0)"
yt-dlp --remote-components ejs:github --no-playlist --cookies-from-browser firefox --extractor-args "youtube:player_client=android_vr,web_embedded" -f "bestvideo+bestaudio/best" --merge-output-format mkv --postprocessor-args "ffmpeg:-c:v copy -c:a flac" "Your_Video_URL"
```

### Audio-only download
```bash
yt-dlp --remote-components ejs:github --no-playlist --cookies-from-browser firefox --extractor-args "youtube:player_client=android_vr,web_embedded" -f "bestaudio" -x --audio-format flac "https://www.youtube.com/watch?v=UvV74ex-02M"
yt-dlp --remote-components ejs:github --no-playlist --cookies-from-browser firefox --extractor-args "youtube:player_client=android_vr,web_embedded" -f "bestaudio" -x --audio-format flac "Your_Video-URL"
```
