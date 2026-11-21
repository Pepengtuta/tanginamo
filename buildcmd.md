### If Gradle fails or gets stuck, STOP it first:
```powershell
cd android
.\gradlew --stop
```

### How to clean and build the Debug APK (installable):
```powershell
cd android
.\gradlew clean
.\gradlew assembleDebug
```
The installable APK will be at:
`android\app\build\outputs\apk\debug\app-debug.apk`

---

### How to build the Release APK:
```powershell
cd android
.\gradlew assembleRelease
```
*Note: Release APKs built via CLI require signing or building via Android Studio (Build > Generate Signed Bundle / APK).*
