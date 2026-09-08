# AI Assistant (Android)

A private, personal AI chat assistant for Android. Chats stream live replies,
keep full history on-device (Room database — nothing is uploaded anywhere
except directly to whichever AI provider you choose), support image
attachments, voice input, and spoken replies.

## Features
- Live streaming chat (replies appear word-by-word)
- Multiple AI providers: Anthropic Claude, OpenAI GPT, Google Gemini — pick
  whichever you have a key for, per your choice in Settings
- Full conversation history, stored only on your phone
- Multiple simultaneous chats, rename/delete
- Image attachments (ask questions about a photo)
- Voice input (microphone → text) and optional spoken replies (text → voice),
  both using Android's own built-in speech engine — no extra service needed
- Dark / light theme
- Custom system prompt (change the assistant's personality/instructions)

## You will need an API key
This app does not include any AI "brain" of its own — it calls whichever
provider you configure, using **your own** account and key. This is true of
every AI app; there is no way around it. Pick one (or add more than one and
switch anytime in Settings):

| Provider | Where to get a key |
|---|---|
| Anthropic Claude | https://console.anthropic.com |
| OpenAI GPT | https://platform.openai.com |
| Google Gemini | https://aistudio.google.com |

Paste the key into **Settings → API key** inside the app. It's stored only
in the app's private on-device storage.

## Building the APK (no computer needed — GitHub does it for you)

1. Create a free GitHub account if you don't have one (https://github.com).
2. Create a new repository and upload this entire project folder to it
   (drag-and-drop on github.com works, or `git push` if you're comfortable
   with git).
3. Go to the repository's **Actions** tab. A workflow called **"Build APK"**
   will run automatically (it's already set up in
   `.github/workflows/build.yml`).
4. When it finishes (green checkmark, a few minutes), open that run and
   download the **app-debug-apk** artifact — this is your installable APK.
5. Transfer it to your phone and install it (you'll need to allow
   "install unknown apps" for your file manager/browser once).

If the Actions build ever shows a red ❌ (failed), open the failing step,
copy the error text, and paste it back into this chat — the project will be
fixed and you re-run the workflow (Actions tab → the failed run → **Re-run
jobs**).

## Building locally instead (optional, if you have Android Studio)
1. Open this folder in Android Studio (Koala or newer).
2. **If you hit a Kotlin/JVM version crash mentioning a Java version like
   "25.x"**, your system's default JDK is too new for this project's tools.
   The most reliable fix is pointing Gradle straight at a compatible JDK you
   already have (Android Studio installs one). Open PowerShell and run:
   ```
   New-Item -ItemType Directory -Force -Path "$env:USERPROFILE\.gradle" | Out-Null
   Add-Content -Path "$env:USERPROFILE\.gradle\gradle.properties" -Value "org.gradle.java.home=<path to a JDK 17 or 21 folder, e.g. C:/Users/<you>/.jdks/jbr-21.0.11>"
   ```
   Then fully restart Android Studio. (Alternative: File → Settings → Build,
   Execution, Deployment → Build Tools → Gradle → set "Gradle JVM" to a
   JDK 17/21 — but the file above is more reliable since it can't be
   undone by a stale background process.)
3. Let it sync Gradle (needs internet the first time to download
   dependencies).
4. Run ▶ on a device/emulator, or **Build → Build Bundle(s)/APK(s) → Build
   APK(s)**.

## Project structure
```
app/src/main/java/com/myai/assistant/
├── data/
│   ├── local/        Room database (chats, messages), DataStore settings
│   ├── remote/        API clients: Anthropic, OpenAI, Gemini
│   └── repository/    Combines local storage + remote calls
├── ui/
│   ├── screens/        Chat, History, Settings screens (Jetpack Compose)
│   ├── theme/          Colors, typography, dark/light theme
│   └── ChatViewModel.kt
└── util/               Image encoding, speech-to-text/text-to-speech helpers
```

---
میں کیا بنایا ہے (اردو خلاصہ):
یہ ایک مکمل، پروفیشنل اینڈرائیڈ AI چیٹ ایپ کا سورس کوڈ ہے۔ اوپر دی گئی
ہدایات کے مطابق GitHub پر اپ لوڈ کریں، خودکار طریقے سے APK بن جائے گی،
اسے ڈاؤن لوڈ کر کے فون میں انسٹال کریں۔ Settings میں اپنی API key ڈالیں
(Anthropic, OpenAI, یا Gemini میں سے کوئی ایک) — بس، ایپ استعمال کے لیے
تیار ہے۔

**اگر آپ Android Studio میں خود بلڈ کر رہے ہیں تو ایک ضروری سیٹنگ:**
File → Settings → Build, Execution, Deployment → Build Tools → Gradle میں
جائیں اور "Gradle JVM" کو JDK 17 یا 21 پر سیٹ کریں (JDK 25 یا اس سے نیا
ہرگز نہ رکھیں — پرانا Kotlin کمپائلر اتنے نئے ورژن کو نہیں پہچانتا اور
کریش ہو جاتا ہے)۔ اگر فہرست میں "jbr-17" یا "jbr-21" نظر آئے تو وہی
منتخب کریں۔ یہ ایک بار کی سیٹنگ ہے، دوبارہ کچھ نہیں کرنا پڑے گا۔
