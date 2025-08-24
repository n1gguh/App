# Fitness Streak App (Flutter)

Track your daily goals and build a streak 🔥:
- **Steps** (default goal: 10,000)
- **Water** (goal: 3.0 liters)
- **Protein** (goal: `1.5 g * body weight (kg)`)
- **Exercises** (log anything you did: e.g., Push-ups 3x10, Run 20 min)
- **Weight**: update anytime; protein goal auto-updates
- **Streak**: counts consecutive days where you meet **all three goals** (steps, water, protein).

> Steps are **manual entry** by default (so it works on every device and no special permissions).  
> You can still enable pedometer later by adding a package and permission—see the optional section below.

---

## Quick Start (Local Build to APK)

1. **Install Flutter**
   - https://docs.flutter.dev/get-started/install
   - Make sure `flutter doctor` passes.

2. **Create the project folders** (if you downloaded a zip, unzip it first):
   ```bash
   cd fitness_streak_app
   ```

3. **Generate platform folders** (Android, iOS, etc.):
   ```bash
   flutter create .
   ```

4. **Install dependencies**:
   ```bash
   flutter pub get
   ```

5. **Build release APK**:
   ```bash
   flutter build apk --release
   ```

6. **Find your APK**:
   - `build/app/outputs/flutter-apk/app-release.apk`  
   Copy it to your phone and install (you may need to allow installation from unknown sources).

---

## Put it on GitHub & Auto-Build APK (GitHub Actions)

1. Create a **new GitHub repo** (empty).
2. Push this project:
   ```bash
   git init
   git add .
   git commit -m "Initial commit - Fitness Streak App"
   git branch -M main
   git remote add origin https://github.com/<your-username>/<your-repo>.git
   git push -u origin main
   ```
3. Go to your repo’s **Actions** tab → ensure Actions are enabled.
4. Wait for the **“Build APK (Flutter)”** workflow to finish or run it manually (Actions → Workflows → Build APK → **Run workflow**).
5. Download the APK artifact from the workflow run page.

> The CI workflow runs `flutter create .` automatically (so the Android folder is generated in CI) and then builds the release APK.

---

## Optional: Add Automatic Step Counting (Pedometer)

This app uses manual step entry by default to avoid extra setup. If you want **automatic step counting**:

1. Add packages (example):
   ```yaml
   # pubspec.yaml (dependencies)
   pedometer: ^4.0.1
   permission_handler: ^11.3.1
   ```

2. **Android permission** (after `flutter create .`):
   - Edit `android/app/src/main/AndroidManifest.xml`
   - Add inside the `<manifest ...>` tag (top-level):
     ```xml
     <uses-permission android:name="android.permission.ACTIVITY_RECOGNITION"/>
     ```
   - On Android 10+ you must also **request the permission at runtime** (via `permission_handler`).

3. In code, listen to the pedometer stream and update today's steps. (You can add this later; the current UI already supports manual entry.)

---

## How streaks work

- A **day counts** when you hit **all** three targets:
  - Steps ≥ 10,000
  - Water ≥ 3.0 L
  - Protein ≥ `1.5 × your weight (kg)`
- Your **streak** is how many consecutive calendar days (including today) you met all targets.
- Days with no data count as not meeting targets.

---

## Customize Goals

- Steps goal is fixed at 10,000 in this starter. You can change `kStepsGoal` in `lib/main.dart`.
- Water goal is fixed at 3.0 L in this starter. You can change `kWaterGoalLiters` in `lib/main.dart`.
- Protein goal auto-calculates from your weight: `1.5 × weight_kg`. Update your weight in the Settings tab.

---

## Project Structure

```
fitness_streak_app/
├─ lib/
│  └─ main.dart
├─ .github/
│  └─ workflows/
│     └─ build-apk.yml
├─ pubspec.yaml
└─ README.md
```

---

## Troubleshooting

- **“App not installed”**: On Android, enable "install from unknown sources" or use the GitHub Actions artifact.
- **Build errors**: Run `flutter clean`, then `flutter pub get`. Ensure Flutter stable channel is up-to-date.
- **Time zone / date**: The app saves per-day data using your device’s local date.

Enjoy and tweak as you like! 💪
