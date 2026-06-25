name: Live Stream to Kick

on:
  workflow_dispatch:

jobs:
  stream:
    runs-on: ubuntu-latest
    steps:
    - name: Install FFmpeg and Streamlink
      run: |
        sudo apt-get update
        sudo apt-get install -y ffmpeg python3-pip
        pip3 install --upgrade streamlink

    - name: Start Streaming
      run: |
        STREAM_URL=$(streamlink --stream-url https://www.twitch.tv/aljazeeramubasher best)
        ffmpeg -re -i "$STREAM_URL" -c:v copy -c:a aac -f flv "${{ secrets.KICK_URL }}/${{ secrets.KICK_KEY }}"
