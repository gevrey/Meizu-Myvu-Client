# MYVU Client 0.4 local test build

This integration branch combines the source changes proposed in upstream PRs
[6](https://github.com/Panny777/Meizu-Myvu-Client/pull/6),
[7](https://github.com/Panny777/Meizu-Myvu-Client/pull/7),
[8](https://github.com/Panny777/Meizu-Myvu-Client/pull/8), and
[9](https://github.com/Panny777/Meizu-Myvu-Client/pull/9).

- Discover IMIKI glasses from their manufacturer advertisement and start with
  an empty MAC field on a new installation.
- Keep RFCOMM recovery within its attempt budget, with stale callbacks ignored.
- Give socket connection and session initialization separate timeout windows.
- Preserve wear detection and screen timeout across reconnects.
- Add explicit Autolock duration requests of 15, 30 or 120 seconds and a read
  request. The UI reports requested state, not unverified application success.

Version name is 0.4 and version code is 4. Compile/target SDK remains 34 and
minimum SDK remains 26. No SDK migration is included in these protocol fixes.

## Separate installation

The debug APK uses `com.myvu.client.test`, labelled **MYVU Client 0.4 Test**,
and the local Android debug signing key. It can coexist with an upstream
`com.myvu.client` release signed with another key. It does not migrate or erase
that release's private data. Release configuration retains `com.myvu.client`.

Only one companion client should connect to the glasses during a test. Do not
run the previous client concurrently: it still applies its old automatic
settings. This build needs its own Bluetooth permissions and app configuration.

## Build and validation

With JDK 17 and an installed Android SDK:

```sh
cd android
./gradlew --no-daemon testDebugUnitTest assembleDebug
```

The signed test APK is `app/build/outputs/apk/debug/app-debug.apk`. APKs and
signing keys are excluded from Git. Publishing this branch is not an official
upstream release or an APK deployment.

The integrated build passes all 156 JVM tests across 18 suites, with no failures
or skipped tests. The signed APK reports version 0.4, version code 4, the
separate test package and the expected launcher activity. The new Autolock UI
also compiles and its independent branch passes 136 JVM tests.
Android lint reports the same pre-existing 20 errors and 144 warnings as the
upstream base, with no new findings from that UI change. Full integration test
results and APK verification output are recorded alongside the built artifact.

Hardware validation is still required. The Autolock reply schema and firmware
acceptance of requested values have not been established on the target glasses.
This build does not change firmware or claim to fix every later socket loss.
