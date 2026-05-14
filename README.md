# Rakta-Seva Connect

Free-stack Android emergency blood donor app using Kotlin, Jetpack Compose, MVVM, LiveData, Supabase REST/Postgres, Supabase Edge Functions, OneSignal, and OpenStreetMap/osmdroid.

## Open In Android Studio

Open this folder:

```text
C:\Users\HP\Desktop\RaktaSevaConnect
```

## Free Backend Setup

### 1. Supabase

1. Create a free project at `https://supabase.com`.
2. Open **SQL Editor**.
3. Run `supabase/schema.sql`.
4. Copy your project URL and anon key from **Project Settings > API**.
5. Put them in `app/build.gradle.kts`:

```kotlin
buildConfigField("String", "SUPABASE_URL", "\"https://YOUR_PROJECT_REF.supabase.co\"")
buildConfigField("String", "SUPABASE_ANON_KEY", "\"YOUR_SUPABASE_ANON_KEY\"")
```

### 2. OneSignal

1. Create a free OneSignal app at `https://onesignal.com`.
2. Copy the OneSignal app id.
3. Put it in `app/build.gradle.kts`.
4. Deploy `supabase/functions/alert-eligible-donors` as a Supabase Edge Function.
5. Set Edge Function secrets:

```bash
supabase secrets set ONESIGNAL_APP_ID=...
supabase secrets set ONESIGNAL_REST_API_KEY=...
```

The current Android app keeps a `push_token` field ready for OneSignal player/subscription ids. For a classroom demo, request creation and donor matching work through Supabase even before push token registration is wired to a real OneSignal account.

## Demo Login

Phone OTP is mocked to avoid paid SMS. Enter any phone number and use:

```text
123456
```

## Free Map Provider

Google Maps was removed. The app now uses OpenStreetMap through `osmdroid`, so no Google Maps API key is required.

## Important Files

- `supabase/schema.sql`: tables, indexes, and demo RLS policies
- `supabase/functions/alert-eligible-donors/index.ts`: donor matching and OneSignal notification sender
- `app/src/main/java/com/raktaseva/connect/data/repository`: Supabase REST repositories
- `app/src/main/java/com/raktaseva/connect/ui/post/MapPicker.kt`: OpenStreetMap picker

## Privacy Logic

Accepted responses store the donor phone number. Declined responses store an empty phone number, so the requester sees `Hidden` until acceptance.
