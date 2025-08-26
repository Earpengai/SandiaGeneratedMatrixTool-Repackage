**Terminal fix (removes quarantine + shows any real error)**
```bash
APP="/Applications/Sandia Generated Matrix Tool.app"
BIN="$APP/Contents/MacOS/Sandia Generated Matrix Tool"
APPDIR="$APP/Contents/app"
MAIN_JAR="$APPDIR/gov-sandia-cognition-generator-matrix.jar"

echo "== Remove quarantine (Gatekeeper) =="
xattr -dr com.apple.quarantine "$APP" || true
echo "Gatekeeper status (0 = allowed):"
spctl --assess --type execute "$APP"; echo $?

echo "== Verify packaged files =="
ls -1 "$APPDIR" || { echo "Missing Contents/app"; exit 1; }

echo "== Manifest =="
unzip -p "$MAIN_JAR" META-INF/MANIFEST.MF | sed -n '1,120p' || true

echo "== Launch =="
"$BIN"

```

Keep it working smoothly

Next time you can just double-click Sandia Generated Matrix Tool.app in Applications.

If macOS ever blocks a fresh build again, run just this:


```bash
xattr -dr com.apple.quarantine "/Applications/Sandia Generated Matrix Tool.app"
```
