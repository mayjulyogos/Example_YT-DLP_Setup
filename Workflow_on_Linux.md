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

## Install FFmpeg

### 1. Enable RPM Fusion repositories (Required for full FFmpeg codecs)
```bash
sudo rpm-ostree install \
  https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm \
  https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm
```

<br>

### 2. Swap ffmpeg-free libraries with full ffmpeg AND layer all required packages in a single transaction
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

### 3. Reboot to apply system image changes
```bash
systemctl reboot
```

<br>

---

<br>

## Install Deno via official script into user space (~/.deno)
```bash
curl -fsSL https://deno.land/install.sh | sh

# Verify Deno installation
deno --version
```

<br>

---

<br>

## Run yt-dlp commands
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

<br>

---

<br>

### Download Videos with lower quality which could be played on tv
- Format Selection (-f):

- [height<=1080] (or 720): Old TVs usually have hardware decoding limits. Restricting it to 1080p (or 720p if your TV is older/low-end) prevents your TV from crashing or lagging due to attempting to decode 4K streams. (You can change 1080 to 720 if the TV still stutters).

- [fps<=30]: Older TVs often lack hardware acceleration for 60fps video, causing dropped frames or choppy playback. Limiting to 30fps ensures smooth playback.

- [vcodec^=avc1]: This forces H.264 (AVC). Almost all older TVs, media boxes, and hardware decoders have dedicated, highly optimized hardware acceleration for H.264. Newer codecs like VP9 or AV1 will force your CPU to do software decoding, which will lag heavily on weak chips.

- ba[acodec^=mp4a]: Selects standard AAC audio, which has universal hardware/software support.
```bash
yt-dlp --remote-components ejs:github --no-playlist --cookies-from-browser firefox \
  --extractor-args "youtube:player_client=android_vr,web_embedded" \
  -f "bv*[height<=1080][fps<=30][vcodec^=avc1]+ba[acodec^=mp4a] / b[height<=1080][fps<=30]" \
  --merge-output-format mkv \
  --postprocessor-args "ffmpeg:-c:v copy -c:a aac -b:a 192k" \
  "https://www.youtube.com/watch?v=kp0nbIAnhn0"
```

<br>

---

<br>

## Troubleshoot
* If you reset your rpm-ostree layers you may found out these specific errors while trying to run the yt-dlp command lines
* `yt-dlp command not found`
* `ffmpeg is already layered`
```bash
# Solution
# Simply reinstall yt-dlp without any other packages
rpm-ostree install yt-dlp

# Explanation for this specific error
# Since the INSTALLATION COMMAND Line we used for installing ffmpeg and required packages started with installing ffmpeg first, the ffmpeg which is installed will force stop the executed services of the command line and return output errors
# By removing ffmpeg packages from the installation command line prevent us from causing the ffmpeg installed error
```  
