# Number Heroes for Android

The student side of Number Heroes as an Android app. Teacher mode is removed. It works fully offline, needs no permissions and collects no data.

- **First launch:** the Android launch screen (star on blue), then a 4-page welcome for new users ("Welcome to Number Heroes!", "Practice any skill", "Earn medals and level up", "Make it yours"). Users can swipe or tap Next, or Skip.
- **Back button:** closes a sheet, then steps back through screens, and leaves the app from the home screen.
- **Read aloud:** uses the phone's own text-to-speech voice.
- **Dark mode:** follows the phone's setting.
- **Progress:** saved on the phone. App progress is separate from the website. To move a hero across, use Grown-ups → Backup code on one and Restore on the other.

## 1. Get a test app on your phone (no installs needed)

GitHub can build the app for you.

1. On GitHub, create a **new repository** (for example `number-heroes-android`). Keep it separate from your website repository.
2. Upload **everything in this folder** to it. On the repository page, choose **Add file → Upload files** and drag in all the files and folders.
   - Check that the `.github` folder was uploaded. Some computers hide folders that start with a dot. If it's missing, choose **Add file → Create new file**, type `.github/workflows/android.yml` as the name, and paste in the contents of that file.
3. Open the **Actions** tab. A run called "Build Android app" starts by itself; it takes about 5 minutes. If it hasn't started, choose it on the left and click **Run workflow**.
4. When it shows a green tick, open the run. Under **Artifacts**, download **number-heroes-test-apk**. Unzip it to get `app-debug.apk`.
5. Send the APK to an Android phone and open it. Allow "install unknown apps" when asked. This test version is for you and testers only. Google Play needs the signed bundle in step 3.

## 2. Or build it in Android Studio

Open this folder in Android Studio (File → Open), wait for it to finish syncing, and press **Run** with a phone or emulator connected.

## 3. Publishing on Google Play

1. **Choose your app ID.** In `app/build.gradle`, change `applicationId "com.numberheroes.app"` to something unique to you, such as `ca.yourname.numberheroes`. It can't change after the first upload.
2. **Create an upload key** (once; keep the file and passwords safe forever):
   - In Android Studio: **Build → Generate Signed App Bundle or APK → Android App Bundle → Create new…**. The wizard then builds the signed `.aab` for you.
   - Or use the GitHub workflow. Create the key with this command (it comes with Java):
     `keytool -genkeypair -v -keystore upload.jks -alias upload -keyalg RSA -keysize 2048 -validity 10000`
     Then in your repository go to **Settings → Secrets and variables → Actions** and add four secrets:
     - `KEYSTORE_BASE64`: the output of `base64 -w0 upload.jks` (on a Mac: `base64 -i upload.jks`)
     - `KEYSTORE_PASSWORD`: the keystore password
     - `KEY_ALIAS`: `upload`
     - `KEY_PASSWORD`: the key password

     Every run then also produces **number-heroes-play-bundle** (`app-release.aab`).
3. **Set up the listing.** You need a Google Play Console developer account (one-time fee). Create the app, then fill in:
   - **Main store listing:** text from `store-listing/play-store-text.md`, the icon `play-icon-512.png`, the `feature-graphic-1024x500.png`, and at least 2 phone screenshots.
   - **Privacy policy:** host `store-listing/privacy.html` (for example, next to your website on GitHub Pages) and paste its address. Replace `[your contact email]` first.
   - **Target audience and content:** choose the children's age groups. The app then has to follow Google Play's Families policy. It has no ads, no data collection and no outside links, which keeps that simple.
   - **Data safety:** answer that no data is collected or shared.
   - **Content rating:** complete the questionnaire.
4. **Upload and test.** Upload the `.aab` to a testing track first. New personal developer accounts must run a closed test with testers for a set period before they can publish to everyone. Play Console shows the current requirement.
5. **Updates:** for every new upload, raise `versionCode` by 1 (and `versionName`, such as 1.1) in `app/build.gradle`.

## Updating the math content

The whole app is one page: `app/src/main/assets/www/index.html`, generated from the Number Heroes source files. When the website gets new skills, regenerate this file, raise `versionCode`, and build again.

## Fonts

The app bundles its two fonts (Atkinson Hyperlegible and Baloo 2, both free and open source). The build downloads them automatically into `app/src/main/assets/www/fonts/`. If a build can't reach the internet, the app still works and uses the phone's own fonts.
