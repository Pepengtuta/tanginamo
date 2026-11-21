### How to build the SIGNED Release APK (Optimized & Installable):
```powershell
cd android
.\gradlew assembleRelease
```
The installable Release APK will be created at:
`android\app\build\outputs\apk\release\app-release.apk`

---

### If Gradle fails or gets stuck, STOP it first:
```powershell
cd android
.\gradlew --stop
```

### How to build the Debug APK:
```powershell
cd android
.\gradlew clean
.\gradlew assembleDebug
```
The Debug APK will be at:
`android\app\build\outputs\apk\debug\app-debug.apk`
