# Workflow to Setup YT-DLP

<br>

## Github page for YT-DLP (You may find other parameters and command line to try out with yt-dlp)
https://github.com/yt-dlp/yt-dlp

<br>

## Install core dependencies via Winget
* Make sure you got AV1 codec installed to be able to play the video locally
* If AV1 codec is not installed, you may need to install AV1 Video Extension using Microsoft Store to run Videos Downloaded using AV1 codec
* Make sure to install FFmpeg which will be required while compiling videos
```bash
## Reset winget before installing
winget source reset --force

winget install yt-dlp.yt-dlp --source winget
winget install Gyan.FFmpeg --source winget
winget install DenoLand.Deno --source winget
winget install OpenJS.NodeJS --source winget
```

<br>

## Verify Deno installation
```bash
deno --version
```

<br>

---

<br>

## Run your yt-dlp commands (These remain identical as the flags are universal)
* Replace this URL with the YouTube Video's URL which you would like to download
* Replace it from "https://www.youtube.com/watch?v=UvV74ex-02M" to "Your_YouTube_Videos_URL"
* Run your yt-dlp command lines using CMD or Powershell with or without admin permission

### Video and Audio download
```bash
yt-dlp --remote-components ejs:github --no-playlist --cookies-from-browser firefox --extractor-args "youtube:player_client=android_vr,web_embedded" -f "bestvideo+bestaudio/best" --merge-output-format mkv --postprocessor-args "ffmpeg:-c:v copy -c:a flac" "https://www.youtube.com/watch?v=UvV74ex-02M"
yt-dlp --remote-components ejs:github --no-playlist --cookies-from-browser firefox --extractor-args "youtube:player_client=android_vr,web_embedded" -f "bestvideo+bestaudio/best" --merge-output-format mkv --postprocessor-args "ffmpeg:-c:v copy -c:a flac" "Your_Video_URL"
```

### Audio-only download
```bash
yt-dlp --remote-components ejs:github --no-playlist --cookies-from-browser firefox --extractor-args "youtube:player_client=android_vr,web_embedded" -f "bestaudio" -x --audio-format flac "https://www.youtube.com/watch?v=xZ3-_Tx0Kfg"
yt-dlp --remote-components ejs:github --no-playlist --cookies-from-browser firefox --extractor-args "youtube:player_client=android_vr,web_embedded" -f "bestaudio" -x --audio-format flac "Your_Video-URL"
```
