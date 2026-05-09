🎙️ Real-time Scrolling Subtitles & Translation (Enhanced Standalone Edition)

This is a completely free, single-file, ready-to-use real-time speech recognition and multi-language translation subtitle tool. Designed specifically for VTubers, live streamers, online teachers, and video creators, it comes with a built-in green screen background and perfectly integrates with OBS.

This project is based on the browser's native Web Speech API for speech recognition and cleverly utilizes Google Apps Script (GAS) to achieve zero-cost, unlimited high-accuracy Google Translation.

✨ Core Features

🚀 Completely Free: No need to configure expensive API keys from major cloud providers. Say goodbye to pay-per-character billing forever.

📦 Ultra-Lightweight: All logic, styles, and UI are packed into a single index.html file. No Node.js environment required, no npm install.

🌍 Multi-language Co-display: Supports recognizing multiple languages and displaying the original text + 2 different translated languages simultaneously.

⚙️ Highly Customizable: Easily adjust font family, font weight, stroke size & color, line height, letter spacing, and green screen opacity.

🧠 Smart Breath-Pause Segmentation: A pioneering "millisecond-level breath pause detection" mechanism. It guarantees the accuracy of NLP semantic error correction for long sentences while achieving lightning-fast real-time display.

💾 Local/Cloud Configuration: Automatically saves your latest settings to the browser. Supports exporting configurations as .json files for seamless migration across different devices.

🎬 Immersive OBS Mode: Click anywhere on the screen to hide the entire control panel, leaving only the green screen and subtitles—tailor-made for live streaming.

🚀 Quick Start

Method 1: Online Usage (Recommended)

Deploy the index.html file directly through static web hosting services (such as GitHub Pages, Netlify, Vercel).

⚠️ Note: Modern browser security policies require that web pages must be served over HTTPS (i.e., URLs starting with https://) to access the microphone. Therefore, deploying it to GitHub Pages is highly recommended.

Method 2: Local Execution

If you want to test it locally, you can use the Live Server extension in VS Code, or start a local server via Python (localhost also allows microphone access):

python -m http.server 8000


Then visit http://localhost:8000 in your browser.

🛠️ Core Step: Configure Free Translation API (GAS)

To enable the translation feature completely for free, you need to spend 2 minutes deploying your own Google Apps Script interface:

Log in to your Google account and visit the Google Apps Script Dashboard.

Click on "New Project" in the top left corner.

Clear the default code in the editor, and copy and paste the following code entirely:

function doGet(e) {
  var text = e.parameter.text;
  var source = e.parameter.source || 'auto';
  var target = e.parameter.target || 'en';
  
  if(!text) return ContentService.createTextOutput("");
  
  try {
    var translatedText = LanguageApp.translate(text, source, target);
    return ContentService.createTextOutput(translatedText);
  } catch(error) {
    return ContentService.createTextOutput("[Translation Error]");
  }
}


Click the blue "Deploy" button in the top right corner -> "New deployment".

Click the gear icon ⚙️ next to "Select type" on the left, and check "Web app".

Configure the form on the right:

Description: Fill in anything (e.g., Subtitle Translation API)

Execute as: Select "Me"

Who has access: MUST select "Anyone" 7. Click "Deploy". (On your first deployment, Google might prompt an "Unverified app" warning. Click "Advanced" and then click the grey link "Go to..." to authorize it).

Once deployed successfully, you will get a Web app URL ending with https://script.google.com/macros/s/.../exec.

Copy this URL and paste it into the Google Apps Script (GAS) URL input box in the settings panel of this subtitle project!

🎥 How to Use in OBS

Open your subtitle page in the browser, and configure your fonts, colors, and the GAS URL.

Click the screen to enter Pure Mode (hides all UI elements).

In OBS, add a "Window Capture" source and select your browser window.

Add a "Chroma Key" filter to this Window Capture source.

Set the Key Color Type to "Green", which will perfectly key out the background, leaving only the subtitles floating on your live stream overlay.

(Note: Since this tool uses the browser's native Web Speech API, it is NOT recommended to add it directly via OBS's "Browser" source, because configuring microphone permissions for the OBS built-in browser is cumbersome. Using a regular browser + Window Capture is the best approach.)

❓ FAQ

Q: Why is there no response when I click "Start Recognition"?
A: Please check: 1. Whether your device has a microphone connected; 2. Whether the webpage is running in an HTTPS or localhost environment; 3. Whether you clicked "Allow" in the microphone permission prompt popped up by the browser.

Q: Why does the original text recognize fine, but the translation keeps showing empty or error?
A: Please check if your GAS URL is filled in correctly, and ensure that the access permission was set to "Anyone" when deploying the GAS script.

Q: The subtitles feel a bit slow to appear when I speak?
A: You can decrease the "Pause Duration" in the settings panel (e.g., to 600ms). A smaller value means faster translation and display, but if set too small, it might break a single sentence into too many pieces. It is recommended to fine-tune this based on your usual speaking speed.

📄 Tech Stack

Core Framework: React 18 (Imported via CDN without build tools)

UI Styling: Tailwind CSS

Icons: Phosphor Icons

Speech Recognition: Web Speech API

Translation Service: Google Apps Script (LanguageApp)

📜 License

This project is open-sourced under the MIT License. You are free to use, modify, and distribute it, but please retain the original author's copyright notice.
