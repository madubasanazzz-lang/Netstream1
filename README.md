# NET STREAM  (com.rex.netstream)

Flutter app + Admin panel + Firebase Realtime Database rules.

## 1. Project හදාගන්න
```bash
flutter create --org com.rex --project-name netstream --platforms android netstream_app
cd netstream_app
# මේ zip එකේ තියෙන lib/ folder එකයි pubspec.yaml එකයි project එකට copy කරන්න (replace)
flutter pub get
```
Package name = `com.rex.netstream` (applicationId) වෙනවා.

## 2. Firebase (project: netstream-auth)
Easy way:
```bash
dart pub global activate flutterfire_cli
flutterfire configure --project=netstream-auth --platforms=android --android-package-name=com.rex.netstream
```
(මේකෙන් google-services.json දානවා + Gradle plugin එක add කරනවා.)

Console එකේ:
- Authentication > Sign-in method > **Email/Password** සහ **Google** enable කරන්න.
- Project settings > Your apps > Android app (com.rex.netstream) > **SHA-1 සහ SHA-256** add කරන්න (Google sign-in වැඩ කරන්න ඕනේ).
  Debug SHA: `cd android && ./gradlew signingReport`
- SHA add කළාට පස්සේ google-services.json අලුතින් download කරලා `android/app/` එකට දාන්න.
- Realtime Database > Rules > `database.rules.json` එකේ content paste කරලා Publish.

## 3. Android settings
`android/app/build.gradle(.kts)` එකේ **minSdk = 23** (firebase_auth ඕනේ).

`android/app/src/main/AndroidManifest.xml`:
```xml
<uses-permission android:name="android.permission.INTERNET"/>
<application
    android:usesCleartextTraffic="true"
    ... >
```
(http:// video links වලට cleartext ඕනේ.)

## 4. Admin panel
1. Console > Authentication > Users > admin email/password user කෙනෙක් හදන්න.
2. `admin/admin.html` open කරලා sign in වෙන්න. "not an admin" කියලා UID එකක් පෙන්නයි.
3. Realtime Database එකේ `admins/<ඒ UID එක>` = `true` add කරන්න. Refresh කරන්න.
4. Settings වලට imgbb API key එක දාන්න.
5. file:// වලට වඩා host කරන එක හොඳයි: `npx serve admin` හෝ Firebase Hosting.

Admin panel එකෙන්: movie / series add, edit, delete (type, category, title, description, tags, poster, .m3u8/.mp4 link, episodes), slider banners, Trending flag.
Poster image auto **1024x1536 (2:3)** , banner auto **1280x720 (16:9)** crop කරලා imgbb එකට upload වෙනවා.

## 5. Run / Build
```bash
flutter run
flutter build apk --release
```

## Database structure
```
titles/{id}  : type, title, category, tags[], year, duration, description,
               posterUrl, videoUrl | episodes[{title,url}], isTrending, createdAt
banners/{id} : imageUrl, title, titleId, order
admins/{uid} : true
users/{uid}/history/{titleId} : title, posterUrl, type, watchedAt, positionMs, durationMs, episodeIndex
```
