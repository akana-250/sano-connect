# SANO CONNECT 1.0 (MVP)

iPhone camera + microphone -> Windows browser receiver using WebRTC.

## What this version does
- iPhone Safari captures camera and microphone after the user taps the permission button.
- Windows browser receives the live audio/video stream.
- Supabase Realtime Broadcast is used only for WebRTC signalling. Media travels peer-to-peer through WebRTC when the network permits it.
- This is a browser MVP, not yet a Windows virtual camera/microphone driver.

## 1. Create Supabase project
1. Create a project at https://supabase.com/dashboard
2. Open Project Settings / API (or Connect) and copy the Project URL and Publishable key.
3. Do NOT use a `service_role` or secret key in browser code.
4. Realtime is used with public room channels in this prototype. Anyone who knows the room code may attempt to join. Use a fresh random code each session; do not use this for private/sensitive calls.

## 2. Configure
Edit `config.js`:
- replace `PASTE_YOUR_SUPABASE_PROJECT_URL_HERE`
- replace `PASTE_YOUR_SUPABASE_PUBLISHABLE_KEY_HERE`

## 3. Publish on GitHub Pages
Upload `index.html`, `phone.html`, `pc.html`, and `config.js` to the repository root on the `main` branch.
Then GitHub repository -> Settings -> Pages -> Deploy from a branch -> `main` -> `/ (root)` -> Save.
Wait for deployment.

## 4. Connect
1. Turn on Personal Hotspot on iPhone.
2. Connect Windows 11 PC to that hotspot.
3. On the PC, open `https://YOUR-USERNAME.github.io/sano-connect/pc.html`.
4. Enter a fresh 6-digit room code and press Join room.
5. On iPhone Safari, open `https://YOUR-USERNAME.github.io/sano-connect/phone.html`.
6. Enter the same code and press Enable camera & microphone. Allow both permissions.
7. On PC, press Start connection.
8. Keep both pages open and the iPhone awake.

## Troubleshooting
- Camera/mic requires HTTPS and user permission. GitHub Pages provides HTTPS.
- If it stays at Connecting, the hotspot or carrier network may block direct peer-to-peer WebRTC. This prototype has STUN but no TURN relay. A TURN service may be needed; TURN can add cost and routes media through that relay.
- If Safari says camera unavailable, close other camera apps, reload the page, and check iOS Settings -> Safari -> Camera/Microphone permissions.
- The video element may need to be unmuted by the user on the PC.

## Use in Zoom / Teams / OBS
The browser receiver alone is not a Windows camera/microphone device.
- Install OBS Studio from https://obsproject.com/
- In OBS add a Window Capture source for the SANO CONNECT PC receiver page, crop to the video, then click Start Virtual Camera. OBS documents this virtual camera feature.
- For microphone routing, a virtual audio cable such as VB-CABLE may be needed. In OBS, monitor the SANO CONNECT browser audio to the virtual cable's playback input; select the cable's recording output as the microphone in the meeting app. Configure carefully to avoid feedback/echo.
- OBS and VB-CABLE are separate third-party tools, not part of SANO CONNECT.

## Important
This is an MVP. It does not silently activate the camera or microphone. The user must grant permission. Do not expose secret Supabase keys. Use a fresh room code and stop the stream when finished.
