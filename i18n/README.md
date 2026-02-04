# Internationalization (i18n) for Asteroid Health

This directory contains translation files for the Asteroid Health application.

## Translation System

The application uses Qt's internationalization system with the following features:
- **Translation IDs**: All user-facing strings are marked with unique IDs (e.g., `id-overview`, `id-settings`)
- **Qt Linguist format**: Translation files use Qt's TS (Translation Source) XML format
- **AsteroidOS integration**: Uses the AsteroidTranslations CMake module for build integration

## File Structure

- `asteroid-health.desktop.h` - Translation context for the desktop launcher
- `asteroid-health.en.ts` - English translation template (source language)
- `asteroid-health.*.ts` - Translation files for other languages (e.g., `de.ts` for German)

## How Translations Work

### In QML Files

All user-facing strings use the `qsTrId()` function with translation IDs:

```qml
// Simple string
PageHeader {
    //% "Overview"
    text: qsTrId("id-overview")
}

// String with parameters
Label {
    //% "You've walked %1 steps today, keep it up!"
    //: %1 is the number of steps
    text: qsTrId("id-steps-walked-today").arg(stepsDataLoader.todayTotal)
}
```

The `//%` comments before each string define the source text that will appear in the translation files.

### Translation IDs

Translation IDs follow this naming convention:
- Use lowercase with hyphens
- Prefix with `id-` to make them easily identifiable
- Be descriptive but concise (e.g., `id-show-step-count-preview`)

## Adding New Translations

### For Translators

1. Copy `asteroid-health.en.ts` to a new file with your language code (e.g., `asteroid-health.fr.ts` for French)
2. Update the `<TS>` tag's language attribute: `<TS version="2.1" language="fr">`
3. Translate the `<translation>` tags while keeping the `<source>` tags unchanged
4. For strings with parameters (like `%1`), ensure the parameters remain in the translated text
5. Use Qt Linguist tool for easier translation management (optional)

### For Developers

When adding new translatable strings:

1. Add the string to your QML file with proper formatting:
   ```qml
   //% "Your translatable text"
   text: qsTrId("id-your-unique-id")
   ```

2. Run the build system to update translation files:
   ```bash
   mkdir build && cd build
   cmake ..
   make
   ```

3. The build system will automatically update all `.ts` files with new strings

## Building with Translations

The CMakeLists.txt is configured to automatically:
1. Extract translatable strings from QML files
2. Update `.ts` translation files
3. Compile `.ts` files to binary `.qm` files
4. Install `.qm` files to the appropriate location

The `generate_translations()` function in CMakeLists.txt handles this process.

## Supported Languages

Currently available translations:
- English (en) - Source language
- German (de) - Sample translation

To add support for your language, create a new `.ts` file following the format above.

## Testing Translations

To test translations on AsteroidOS:
1. Set your device's language in Settings
2. Restart the Asteroid Health application
3. The UI should display in your selected language (if available)

If a translation is not available for a string, the English source text will be displayed as fallback.

## Contributing Translations

Translations can be contributed via:
1. Pull requests with new or updated `.ts` files
2. Integration with translation platforms like Weblate (if available)

## Notes

- The desktop launcher name ("Health") is translated separately in `asteroid-health.desktop.h`
- Date/time formatting may vary by locale and is handled by Qt automatically
- **Known limitation**: Weekday abbreviations in bar graphs ("Sun", "Mon", etc.) are currently hardcoded in English. These should be localized in a future update using Qt's locale system (e.g., `Qt.locale().dayName()`).
