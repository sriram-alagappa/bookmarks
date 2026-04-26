




App launches
      ↓
Hermes JS engine starts (bundled inside your APK)
      ↓
Hermes executes your 2.6MB JS bundle
      ↓
Your JS calls React Native's bridge
      ↓
Bridge calls real native Android APIs
      ↓
Real native UI renders (actual Android Views)
