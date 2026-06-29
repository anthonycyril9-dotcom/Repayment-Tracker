# 📱 Repayment Tracker — Android App

A professional loan repayment tracking app for Android 12+ that manages client records and sends 7-day advance notifications before each repayment date.

---

## ✨ Features

| Feature | Details |
|---|---|
| **Client Management** | Add, edit, delete clients with full loan details |
| **Smart Calendar** | Monthly calendar (2020–2050) with repayment markers |
| **7-Day Notifications** | Automatic push alerts 7 days before every due date |
| **Dashboard** | Live stats: total clients, due soon, portfolio total |
| **Color Indicators** | Red = due ≤7 days, Orange = ≤14 days, Green = safe |
| **Boot Persistence** | Alarms rescheduled automatically after device reboot |
| **Nigerian Naira (₦)** | All amounts formatted in ₦ with commas |

---

## 📋 Input Fields

When adding a client, the following are captured:

1. **Name of Client** — full name
2. **Client ID** — unique identifier
3. **Client Account Number** — bank/account number
4. **Client Phone Number** — mobile number
5. **Loan Amount** — in Nigerian Naira (₦)
6. **Loan Duration** — number of months (e.g. 12 = 1 year)
7. **Date of Disbursement** — date the loan was given out

The app then automatically calculates **monthly repayment dates** (disbursement date + 1 month, + 2 months, etc.) and schedules **push notifications 7 days before each one**.

---

## 🏗️ Architecture

```
com.repaymenttracker/
├── data/
│   ├── model/Client.kt           — Room entity
│   ├── db/AppDatabase.kt         — Room database
│   ├── db/ClientDao.kt           — Data access object
│   └── repository/ClientRepository.kt
├── viewmodel/ClientViewModel.kt  — MVVM ViewModel
├── ui/
│   ├── MainActivity.kt           — Host + bottom nav
│   ├── home/HomeFragment.kt      — Dashboard + client list
│   ├── home/ClientAdapter.kt     — RecyclerView adapter
│   ├── addclient/AddClientFragment.kt — Add/edit form
│   └── calendar/
│       ├── CalendarFragment.kt   — Monthly calendar view
│       ├── CalendarDayAdapter.kt — Grid day cells
│       └── DuePaymentsAdapter.kt — Due list
├── receiver/
│   ├── NotificationReceiver.kt   — Fires notification on alarm
│   └── BootReceiver.kt           — Reschedules on reboot
└── utils/NotificationHelper.kt   — Alarm scheduling logic
```

---

## 🚀 Setup Instructions

### Requirements
- Android Studio Hedgehog (2023.1.1) or newer
- JDK 17
- Android device / emulator running Android 12+ (API 31+)

### Steps

1. **Open in Android Studio**
   ```
   File → Open → select the RepaymentTracker folder
   ```

2. **Sync Gradle**
   Android Studio will auto-prompt. Click **Sync Now**.

3. **Run the app**
   Click ▶ Run or press `Shift+F10`

4. **Grant permissions when prompted**
   - POST_NOTIFICATIONS (Android 13+)
   - SCHEDULE_EXACT_ALARM (for precise 7-day alerts)

---

## 🔔 Notification System

- **When**: 7 days before each monthly repayment date
- **What it shows**: Client name, ID, due date, loan amount, days remaining
- **Persistence**: Survives device reboots (BootReceiver reschedules all alarms)
- **Exact alarm fallback**: If exact alarms are not permitted, falls back to `setAndAllowWhileIdle` (slightly less precise but still functional)

### Notification Permission (Android 13+)
The app requests `POST_NOTIFICATIONS` on first launch with a clear rationale dialog.

---

## 📅 Calendar

- Swipe left/right (← →) to navigate months
- **Blue dot** = repayment due on that day
- **Orange dot** = 7-day warning alert falls on that day
- Tap any day to filter the list below to that date
- Range: January 2020 – December 2050

---

## 🗃️ Database

Uses **Room** (SQLite) for local persistence. All client data is stored on-device. No internet connection required.

---

## 🔑 Key Dependencies

```
Room 2.6.1          — Local database
Navigation 2.7.5    — Fragment navigation
WorkManager 2.9.0   — Background work (optional extension)
Material 1.11.0     — UI components
Coroutines 1.7.3    — Async operations
Kotlin 1.9.0        — Language
```

---

## 📂 Project Structure

```
RepaymentTracker/
├── app/
│   ├── src/main/
│   │   ├── java/com/repaymenttracker/  ← All Kotlin source
│   │   ├── res/                        ← Layouts, drawables, values
│   │   └── AndroidManifest.xml
│   └── build.gradle
├── build.gradle
└── settings.gradle
```

---

*Built for Android 12+ (minSdk 31, targetSdk 34)*
