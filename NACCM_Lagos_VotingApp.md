# 🗳️ NACCM Lagos Voting App

### National Association of Catholic Corps Members — Lagos Chapter

#### Built with React Native · Expo SDK 52 · Expo Router · Supabase

> **Based on:** [notJust.dev — Build a Voting App with React Native and Supabase](https://www.notjust.dev/projects/poll-voting-app)  
> **Color Scheme:** Green `#006600` / White `#FFFFFF`  
> **Design:** Glassmorphism · Animations · Transitions  
> **Platforms:** Android & iOS (overlap-safe)

---

## 📋 Table of Contents

1. [Project Overview](#1-project-overview)
2. [Tech Stack](#2-tech-stack)
3. [Positions / Offices](#3-positions--offices)
4. [Dependencies](#4-dependencies)
5. [Project Structure](#5-project-structure)
6. [Supabase Setup](#6-supabase-setup)
7. [Environment Variables](#7-environment-variables)
8. [Source Code](#8-source-code)
   - [app.json (Expo Config)](#81-appjson)
   - [constants/theme.ts](#82-constantsthemets)
   - [constants/offices.ts](#83-constantsofficestsing)
   - [lib/supabase.ts](#84-libsupabasetsing)
   - [app/\_layout.tsx](#85-app_layouttsx)
   - [app/index.tsx (Splash / Home)](#86-appindextsx)
   - [app/(tabs)/\_layout.tsx](#87-apptabs_layouttsx)
   - [app/(tabs)/elections.tsx](#88-apptabselectionstsx)
   - [app/(tabs)/vote.tsx](#89-apptabsvotetsx)
   - [app/(tabs)/results.tsx](#810-apptabsresultstsx)
   - [components/GlassCard.tsx](#811-componentsglasscardtsx)
   - [components/CandidateCard.tsx](#812-componentscandidatecardtsx)
   - [components/AnimatedButton.tsx](#813-componentsanimatedbuttontsx)
   - [components/ResultBar.tsx](#814-componentsresultbartsx)
   - [components/Header.tsx](#815-componentsheadertsx)
9. [Running the App](#9-running-the-app)
10. [Building for Production](#10-building-for-production)
11. [Troubleshooting](#11-troubleshooting)

---

## 1. Project Overview

The **NACCM Lagos Voting App** is a secure, real-time election platform for the National Association of Catholic Corps Members, Lagos Chapter. It allows registered members to vote for candidates across **8 executive positions**, view live results, and ensures each member can only vote once per position.

### Features

- 🔐 Anonymous secure authentication (Supabase Auth or any better recommemdetion )
- 🗳️ One-vote-per-member-per-position enforcement
- 📊 Real-time live results with animated progress bars
- 💚 Green & White NACCM-themed glassmorphism UI
- ✨ Smooth page transitions and micro-animations
- 📱 Responsive layout — works on Android & iOS without overlaps
- 🧭 Tab-based navigation via Expo Router

---

## 2. Tech Stack

| Technology                     | Purpose                       | Version         |
| ------------------------------ | ----------------------------- | --------------- |
| React Native                   | Core mobile framework         | 0.76.x          |
| Expo SDK                       | Managed workflow, native APIs | **52** (latest) |
| Expo Router                    | File-based navigation         | 4.x             |
| Supabase                       | Backend, Auth, Real-time DB   | latest          |
| TypeScript                     | Type safety                   | 5.x             |
| React Native Reanimated        | Animations                    | 3.x             |
| Expo Linear Gradient           | Gradient backgrounds          | ~14.x           |
| Expo Blur                      | Glassmorphism blur effects    | ~14.x           |
| React Native Safe Area Context | Safe area / no overlaps       | 4.x             |
| React Native Gesture Handler   | Touch gestures                | ~2.x            |

---

## 3. Positions / Offices

The app handles elections for the following **8 executive positions**:

| #   | Office                         | Description                              |
| --- | ------------------------------ | ---------------------------------------- |
| 1   | **President**                  | Head of the association                  |
| 2   | **Vice President**             | Deputy head                              |
| 3   | **Secretary**                  | Records and correspondence               |
| 4   | **Assistant Secretary**        | Supports the Secretary                   |
| 5   | **Liturgist**                  | Oversees spiritual/liturgical activities |
| 6   | **Disciplinary Officer (DPO)** | Enforces association rules               |
| 7   | **Director of Information**    | Handles communication & media            |
| 8   | **Financial Secretary**        | Manages financial records                |

---

## 4. Dependencies

### Install all at once:

```bash
# 1. Create Expo project with latest SDK
npx create-expo-app@latest NACCMLagos --template blank-typescript
cd NACCMLagos

# 2. Install Expo Router
npx expo install expo-router

# 3. Install Supabase client
npx expo install @supabase/supabase-js

# 4. Install AsyncStorage (required by Supabase for session persistence)
npx expo install @react-native-async-storage/async-storage

# 5. Install animation libraries
npx expo install react-native-reanimated

# 6. Install UI & styling
npx expo install expo-linear-gradient
npx expo install expo-blur

# 7. Install safe area and gesture handler (overlap prevention)
npx expo install react-native-safe-area-context
npx expo install react-native-screens
npx expo install react-native-gesture-handler

# 8. Install status bar control
npx expo install expo-status-bar

# 9. Install fonts
npx expo install expo-font @expo-google-fonts/poppins

# 10. Install haptics (tactile feedback on vote)
npx expo install expo-haptics

# 11. Install splash screen
npx expo install expo-splash-screen

# 12. Install vector icons
npx expo install @expo/vector-icons
```

### Complete `package.json` dependencies block:

```json
{
  "dependencies": {
    "@expo/vector-icons": "^14.0.0",
    "@expo-google-fonts/poppins": "^0.2.3",
    "@react-native-async-storage/async-storage": "^2.0.0",
    "@supabase/supabase-js": "^2.43.0",
    "expo": "~52.0.0",
    "expo-blur": "~14.0.1",
    "expo-constants": "~17.0.3",
    "expo-font": "~13.0.1",
    "expo-haptics": "~14.0.0",
    "expo-linear-gradient": "~14.0.1",
    "expo-router": "~4.0.0",
    "expo-splash-screen": "~0.29.13",
    "expo-status-bar": "~2.0.0",
    "react": "18.3.2",
    "react-native": "0.76.5",
    "react-native-gesture-handler": "~2.20.2",
    "react-native-reanimated": "~3.16.1",
    "react-native-safe-area-context": "4.12.0",
    "react-native-screens": "~4.4.0"
  }
}
```

---

## 5. Project Structure

```
NACCMLagos/
├── app/
│   ├── _layout.tsx              # Root layout with fonts & Supabase session
│   ├── index.tsx                # Splash / Welcome screen
│   └── (tabs)/
│       ├── _layout.tsx          # Tab navigator layout
│       ├── elections.tsx        # List of all 8 office elections
│       ├── vote.tsx             # Voting screen for each position
│       └── results.tsx          # Real-time results screen
├── components/
│   ├── GlassCard.tsx            # Reusable glassmorphism card
│   ├── CandidateCard.tsx        # Candidate selection card with animation
│   ├── AnimatedButton.tsx       # Animated pressable button
│   ├── ResultBar.tsx            # Animated vote count progress bar
│   └── Header.tsx               # App header with NACCM branding
├── constants/
│   ├── theme.ts                 # Colors, fonts, spacing constants
│   └── offices.ts               # All 8 offices and candidate data
├── lib/
│   └── supabase.ts              # Supabase client initialization
├── assets/
│   ├── icon.png
│   ├── splash.png
│   └── naccm-logo.png           # NACCM Lagos logo
├── app.json                     # Expo config
├── babel.config.js              # Babel with reanimated plugin
└── tsconfig.json
```

---

## 6. Supabase Setup

### Step 1 — Create a Supabase Project

1. Go to [https://supabase.com](https://supabase.com) and create a free account
2. Create a new project named `naccm-lagos-voting`
3. Note your **Project URL** and **anon public key**

### Step 2 — Create Database Tables

Run the following SQL in Supabase SQL Editor:

```sql
-- ============================================================
-- OFFICES TABLE: Stores the 8 election positions
-- ============================================================
create table offices (
  id uuid default gen_random_uuid() primary key,
  title text not null,
  description text,
  is_active boolean default true,
  created_at timestamp with time zone default now()
);

-- ============================================================
-- CANDIDATES TABLE: Stores candidates per office
-- ============================================================
create table candidates (
  id uuid default gen_random_uuid() primary key,
  office_id uuid references offices(id) on delete cascade,
  full_name text not null,
  photo_url text,
  bio text,
  vote_count integer default 0,
  created_at timestamp with time zone default now()
);

-- ============================================================
-- VOTES TABLE: Records each vote (one per user per office)
-- ============================================================
create table votes (
  id uuid default gen_random_uuid() primary key,
  user_id uuid references auth.users(id),
  office_id uuid references offices(id),
  candidate_id uuid references candidates(id),
  created_at timestamp with time zone default now(),
  unique(user_id, office_id)  -- Prevents double-voting per office
);

-- ============================================================
-- Enable Row Level Security
-- ============================================================
alter table offices enable row level security;
alter table candidates enable row level security;
alter table votes enable row level security;

-- ============================================================
-- RLS Policies: Public read, authenticated write
-- ============================================================
create policy "Allow public read on offices" on offices for select using (true);
create policy "Allow public read on candidates" on candidates for select using (true);
create policy "Allow authenticated users to vote" on votes for insert
  with check (auth.uid() = user_id);
create policy "Allow users to read own votes" on votes for select
  using (auth.uid() = user_id);

-- ============================================================
-- Function to increment vote count atomically (prevents race conditions)
-- ============================================================
create or replace function increment_vote(candidate_id uuid)
returns void as $$
  update candidates set vote_count = vote_count + 1 where id = candidate_id;
$$ language sql security definer;

-- ============================================================
-- Enable Realtime on candidates table (live results)
-- ============================================================
alter publication supabase_realtime add table candidates;

-- ============================================================
-- Seed data: Insert the 8 offices
-- ============================================================
insert into offices (title, description) values
  ('President', 'Head of the National Association of Catholic Corps Members, Lagos Chapter'),
  ('Vice President', 'Deputy head and support to the President'),
  ('Secretary', 'Responsible for records and official correspondence'),
  ('Assistant Secretary', 'Assists the Secretary in administrative duties'),
  ('Liturgist', 'Oversees all spiritual and liturgical activities'),
  ('Disciplinary Officer (DPO)', 'Enforces the constitution and maintains discipline'),
  ('Director of Information', 'Manages communication, media, and publicity'),
  ('Financial Secretary', 'Records and manages all financial transactions');
```

---

## 7. Environment Variables

Create a `.env` file at the project root:

```env
EXPO_PUBLIC_SUPABASE_URL=https://your-project-ref.supabase.co
EXPO_PUBLIC_SUPABASE_ANON_KEY=your-anon-key-here
```

> ⚠️ Never commit this file to version control. Add `.env` to `.gitignore`.

---

## 8. Source Code

---

### 8.1 `app.json`

```json
{
  "expo": {
    "name": "NACCM Lagos Voting",
    "slug": "naccm-lagos-voting",
    "version": "1.0.0",
    "orientation": "portrait",
    "icon": "./assets/icon.png",
    "scheme": "naccm",
    "userInterfaceStyle": "light",
    "splash": {
      "image": "./assets/splash.png",
      "resizeMode": "contain",
      "backgroundColor": "#006600"
    },
    "ios": {
      "supportsTablet": true,
      "bundleIdentifier": "com.naccm.lagosvoting"
    },
    "android": {
      "adaptiveIcon": {
        "foregroundImage": "./assets/adaptive-icon.png",
        "backgroundColor": "#006600"
      },
      "package": "com.naccm.lagosvoting",
      "softwareKeyboardLayoutMode": "pan"
    },
    "web": {
      "bundler": "metro",
      "output": "static",
      "favicon": "./assets/favicon.png"
    },
    "plugins": ["expo-router", "expo-font"],
    "experiments": {
      "typedRoutes": true
    }
  }
}
```

---

### 8.2 `babel.config.js`

```js
// babel.config.js
// IMPORTANT: react-native-reanimated plugin MUST be listed last
module.exports = function (api) {
  api.cache(true);
  return {
    presets: ["babel-preset-expo"],
    plugins: [
      "react-native-reanimated/plugin", // ← Must be last
    ],
  };
};
```

---

### 8.3 `constants/theme.ts`

```typescript
// constants/theme.ts
// Central design token file — all colors, spacing, and font sizes live here.
// Change values here to restyle the entire app.

export const COLORS = {
  // ─── Primary Brand Colors ───────────────────────────────
  primary: "#013001ff", // Deep NACCM green
  primaryLight: "#00AA00", // Lighter green for accents
  primaryDark: "#004400", // Darker green for shadows
  primaryFaded: "rgba(0, 102, 0, 0.15)", // Transparent green for glass tints

  // ─── Secondary ──────────────────────────────────────────
  white: "#FFFFFF",
  offWhite: "#F5FFF5", // Slightly green-tinted white
  whiteTransparent: "rgba(255,255,255,0.15)", // For glass cards

  // ─── Utility ────────────────────────────────────────────
  black: "#111111",
  gray: "#888888",
  grayLight: "#DDDDDD",
  danger: "#CC0000",
  success: "#009900",

  // ─── Glassmorphism surfaces ──────────────────────────────
  glassBg: "rgba(255, 255, 255, 0.12)",
  glassBorder: "rgba(255, 255, 255, 0.3)",
  glassShadow: "rgba(0, 102, 0, 0.25)",

  // ─── Gradient array used in LinearGradient ───────────────
  backgroundGradient: ["#003300", "#006600", "#009933"],
};

export const FONTS = {
  regular: "Poppins_400Regular",
  medium: "Poppins_500Medium",
  semiBold: "Poppins_600SemiBold",
  bold: "Poppins_700Bold",
};

export const SPACING = {
  xs: 4,
  sm: 8,
  md: 16,
  lg: 24,
  xl: 32,
  xxl: 48,
};

export const RADIUS = {
  sm: 8,
  md: 16,
  lg: 24,
  xl: 32,
  full: 9999,
};

export const SHADOW = {
  card: {
    shadowColor: COLORS.primaryDark,
    shadowOffset: { width: 0, height: 4 },
    shadowOpacity: 0.3,
    shadowRadius: 8,
    elevation: 6, // Android shadow
  },
};
```

---

### 8.4 `constants/offices.ts`

```typescript
// constants/offices.ts
// Defines the 8 executive offices for NACCM Lagos elections.
// In a real deployment, candidates come from Supabase —
// this file provides the static office metadata and icons.

import { Ionicons } from "@expo/vector-icons";

export interface Office {
  id: string; // Matches Supabase office UUID (seed in order)
  title: string;
  shortTitle: string;
  description: string;
  icon: keyof typeof Ionicons.glyphMap;
  color: string; // Accent color per office card
}

export const OFFICES: Office[] = [
  {
    id: "president",
    title: "President",
    shortTitle: "PRES",
    description: "Head of the NACCM Lagos Chapter",
    icon: "shield-checkmark",
    color: "#006600",
  },
  {
    id: "vice-president",
    title: "Vice President",
    shortTitle: "V/PRES",
    description: "Deputy head of the association",
    icon: "people",
    color: "#007700",
  },
  {
    id: "secretary",
    title: "Secretary",
    shortTitle: "SEC",
    description: "Handles records & correspondence",
    icon: "document-text",
    color: "#008800",
  },
  {
    id: "asst-secretary",
    title: "Assistant Secretary",
    shortTitle: "A/SEC",
    description: "Supports the Secretary",
    icon: "documents",
    color: "#009900",
  },
  {
    id: "liturgist",
    title: "Liturgist",
    shortTitle: "LITURG",
    description: "Oversees spiritual & liturgical activities",
    icon: "flower",
    color: "#00AA00",
  },
  {
    id: "dpo",
    title: "Disciplinary Officer",
    shortTitle: "DPO",
    description: "Enforces association constitution",
    icon: "hammer",
    color: "#005500",
  },
  {
    id: "director-info",
    title: "Director of Information",
    shortTitle: "D/INFO",
    description: "Manages media & communication",
    icon: "megaphone",
    color: "#006633",
  },
  {
    id: "fin-secretary",
    title: "Financial Secretary",
    shortTitle: "FIN/SEC",
    description: "Records all financial transactions",
    icon: "cash",
    color: "#004400",
  },
];
```

---

### 8.5 `lib/supabase.ts`

```typescript
// lib/supabase.ts
// Initializes and exports the Supabase client.
// AsyncStorage is used to persist the user session across app restarts.

import "react-native-url-polyfill/auto";
import AsyncStorage from "@react-native-async-storage/async-storage";
import { createClient } from "@supabase/supabase-js";

// ─── Supabase credentials from .env ─────────────────────────────────────────
const supabaseUrl = process.env.EXPO_PUBLIC_SUPABASE_URL!;
const supabaseAnonKey = process.env.EXPO_PUBLIC_SUPABASE_ANON_KEY!;

// ─── Create client with persistent session storage ───────────────────────────
export const supabase = createClient(supabaseUrl, supabaseAnonKey, {
  auth: {
    // Use AsyncStorage so session survives app restarts on both Android & iOS
    storage: AsyncStorage,
    autoRefreshToken: true,
    persistSession: true,
    detectSessionInUrl: false,
  },
});

// ─── Type definitions for DB tables ─────────────────────────────────────────
export interface DBOffice {
  id: string;
  title: string;
  description: string;
  is_active: boolean;
  created_at: string;
}

export interface DBCandidate {
  id: string;
  office_id: string;
  full_name: string;
  photo_url: string | null;
  bio: string | null;
  vote_count: number;
  created_at: string;
}

export interface DBVote {
  id: string;
  user_id: string;
  office_id: string;
  candidate_id: string;
  created_at: string;
}
```

---

### 8.6 `app/_layout.tsx`

```tsx
// app/_layout.tsx
// Root layout — loads fonts, initializes Supabase anonymous auth session,
// controls the splash screen, and wraps everything in safe area context.

import { useEffect, useState } from "react";
import { Stack } from "expo-router";
import { StatusBar } from "expo-status-bar";
import * as SplashScreen from "expo-splash-screen";
import * as Font from "expo-font";
import {
  Poppins_400Regular,
  Poppins_500Medium,
  Poppins_600SemiBold,
  Poppins_700Bold,
} from "@expo-google-fonts/poppins";
import { SafeAreaProvider } from "react-native-safe-area-context";
import { GestureHandlerRootView } from "react-native-gesture-handler";
import { supabase } from "../lib/supabase";

// Prevent the splash screen from hiding until fonts are loaded
SplashScreen.preventAutoHideAsync();

export default function RootLayout() {
  const [fontsLoaded, setFontsLoaded] = useState(false);

  useEffect(() => {
    async function prepare() {
      try {
        // ── Load custom Poppins fonts ─────────────────────────────────────
        await Font.loadAsync({
          Poppins_400Regular,
          Poppins_500Medium,
          Poppins_600SemiBold,
          Poppins_700Bold,
        });

        // ── Create anonymous Supabase session if none exists ──────────────
        // This gives each device a unique identity without requiring a login form
        const {
          data: { session },
        } = await supabase.auth.getSession();
        if (!session) {
          await supabase.auth.signInAnonymously();
        }
      } catch (error) {
        console.warn("Layout prepare error:", error);
      } finally {
        setFontsLoaded(true);
        // Hide splash screen once everything is ready
        await SplashScreen.hideAsync();
      }
    }

    prepare();
  }, []);

  // Don't render anything until fonts are loaded to prevent flash of unstyled text
  if (!fontsLoaded) return null;

  return (
    // GestureHandlerRootView MUST wrap everything for gestures to work on Android
    <GestureHandlerRootView style={{ flex: 1 }}>
      {/* SafeAreaProvider prevents overlap with notches and system bars */}
      <SafeAreaProvider>
        {/* Green status bar icons for white backgrounds */}
        <StatusBar style="light" backgroundColor="#003300" />
        <Stack screenOptions={{ headerShown: false }}>
          <Stack.Screen name="index" />
          <Stack.Screen name="(tabs)" />
        </Stack>
      </SafeAreaProvider>
    </GestureHandlerRootView>
  );
}
```

---

### 8.7 `app/index.tsx`

```tsx
// app/index.tsx
// Welcome / Splash screen shown on first open.
// Displays NACCM branding with animated fade-in and a "Begin Voting" CTA.
// Uses Reanimated for smooth entrance animations.

import { useEffect } from "react";
import {
  View,
  Text,
  StyleSheet,
  Dimensions,
  Image,
  Platform,
} from "react-native";
import { router } from "expo-router";
import { LinearGradient } from "expo-linear-gradient";
import Animated, {
  useSharedValue,
  useAnimatedStyle,
  withTiming,
  withDelay,
  withSpring,
  Easing,
} from "react-native-reanimated";
import { useSafeAreaInsets } from "react-native-safe-area-context";
import { COLORS, FONTS, SPACING, RADIUS } from "../constants/theme";
import AnimatedButton from "../components/AnimatedButton";

const { width, height } = Dimensions.get("window");

export default function WelcomeScreen() {
  // ── Get safe area insets to avoid notch/navbar overlap ──────────────────
  const insets = useSafeAreaInsets();

  // ── Reanimated shared values for entrance animations ────────────────────
  const logoOpacity = useSharedValue(0);
  const logoScale = useSharedValue(0.5);
  const titleOpacity = useSharedValue(0);
  const titleTranslateY = useSharedValue(30);
  const buttonOpacity = useSharedValue(0);

  useEffect(() => {
    // Stagger the entrance animations for a polished feel
    logoOpacity.value = withDelay(300, withTiming(1, { duration: 700 }));
    logoScale.value = withDelay(300, withSpring(1, { damping: 12 }));
    titleOpacity.value = withDelay(700, withTiming(1, { duration: 600 }));
    titleTranslateY.value = withDelay(
      700,
      withTiming(0, {
        duration: 600,
        easing: Easing.out(Easing.cubic),
      }),
    );
    buttonOpacity.value = withDelay(1200, withTiming(1, { duration: 500 }));
  }, []);

  // ── Animated styles ──────────────────────────────────────────────────────
  const logoStyle = useAnimatedStyle(() => ({
    opacity: logoOpacity.value,
    transform: [{ scale: logoScale.value }],
  }));

  const titleStyle = useAnimatedStyle(() => ({
    opacity: titleOpacity.value,
    transform: [{ translateY: titleTranslateY.value }],
  }));

  const buttonStyle = useAnimatedStyle(() => ({
    opacity: buttonOpacity.value,
  }));

  return (
    // Full-screen gradient background — green gradient top to bottom
    <LinearGradient
      colors={COLORS.backgroundGradient as [string, string, string]}
      style={styles.container}
    >
      {/* Decorative circle shapes for visual depth */}
      <View style={[styles.circle, styles.circleTopLeft]} />
      <View style={[styles.circle, styles.circleBottomRight]} />

      {/* Main content — padded for safe area on all devices */}
      <View style={[styles.content, { paddingTop: insets.top + SPACING.xl }]}>
        {/* NACCM Logo / Icon area */}
        <Animated.View style={[styles.logoContainer, logoStyle]}>
          {/* Cross + Hands SVG placeholder — replace with actual NACCM logo */}
          <View style={styles.logoCircle}>
            <Text style={styles.logoEmoji}>✝️</Text>
          </View>
        </Animated.View>

        {/* Association name and title */}
        <Animated.View style={[styles.titleContainer, titleStyle]}>
          <Text style={styles.orgName}>NACCM Lagos</Text>
          <Text style={styles.title}>2024/2025 Elections</Text>
          <Text style={styles.subtitle}>
            National Association of Catholic Corps Members{"\n"}Lagos Chapter
          </Text>
        </Animated.View>

        {/* Divider line */}
        <View style={styles.divider} />

        {/* Info cards */}
        <Animated.View style={[styles.infoContainer, titleStyle]}>
          <View style={styles.infoItem}>
            <Text style={styles.infoNumber}>8</Text>
            <Text style={styles.infoLabel}>Positions</Text>
          </View>
          <View style={styles.infoDivider} />
          <View style={styles.infoItem}>
            <Text style={styles.infoNumber}>1</Text>
            <Text style={styles.infoLabel}>Vote Each</Text>
          </View>
          <View style={styles.infoDivider} />
          <View style={styles.infoItem}>
            <Text style={styles.infoNumber}>🔒</Text>
            <Text style={styles.infoLabel}>Secure</Text>
          </View>
        </Animated.View>

        {/* CTA Button */}
        <Animated.View
          style={[
            styles.buttonContainer,
            buttonStyle,
            { paddingBottom: insets.bottom + SPACING.xl },
          ]}
        >
          <AnimatedButton
            title="Begin Voting"
            onPress={() => router.replace("/(tabs)/elections")}
            variant="white"
            size="large"
          />
          <Text style={styles.disclaimer}>
            Your vote is anonymous and final
          </Text>
        </Animated.View>
      </View>
    </LinearGradient>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
  },
  // ── Decorative background circles ──────────────────────────────────────
  circle: {
    position: "absolute",
    width: width * 0.7,
    height: width * 0.7,
    borderRadius: width * 0.35,
    backgroundColor: "rgba(255,255,255,0.05)",
  },
  circleTopLeft: { top: -width * 0.2, left: -width * 0.2 },
  circleBottomRight: { bottom: -width * 0.2, right: -width * 0.2 },

  content: {
    flex: 1,
    alignItems: "center",
    paddingHorizontal: SPACING.xl,
  },
  logoContainer: {
    marginBottom: SPACING.xl,
  },
  logoCircle: {
    width: 110,
    height: 110,
    borderRadius: 55,
    backgroundColor: "rgba(255,255,255,0.2)",
    borderWidth: 2,
    borderColor: "rgba(255,255,255,0.5)",
    alignItems: "center",
    justifyContent: "center",
  },
  logoEmoji: {
    fontSize: 52,
  },
  titleContainer: {
    alignItems: "center",
    marginBottom: SPACING.lg,
  },
  orgName: {
    fontFamily: FONTS.bold,
    fontSize: 32,
    color: COLORS.white,
    letterSpacing: 1.5,
  },
  title: {
    fontFamily: FONTS.semiBold,
    fontSize: 18,
    color: "rgba(255,255,255,0.9)",
    marginTop: SPACING.xs,
  },
  subtitle: {
    fontFamily: FONTS.regular,
    fontSize: 13,
    color: "rgba(255,255,255,0.65)",
    textAlign: "center",
    marginTop: SPACING.sm,
    lineHeight: 20,
  },
  divider: {
    width: 60,
    height: 3,
    backgroundColor: "rgba(255,255,255,0.4)",
    borderRadius: 2,
    marginVertical: SPACING.lg,
  },
  infoContainer: {
    flexDirection: "row",
    backgroundColor: "rgba(255,255,255,0.12)",
    borderRadius: RADIUS.lg,
    borderWidth: 1,
    borderColor: "rgba(255,255,255,0.2)",
    paddingVertical: SPACING.md,
    paddingHorizontal: SPACING.xl,
    marginBottom: SPACING.xl,
    width: "100%",
    justifyContent: "space-around",
    alignItems: "center",
  },
  infoItem: { alignItems: "center" },
  infoNumber: {
    fontFamily: FONTS.bold,
    fontSize: 22,
    color: COLORS.white,
  },
  infoLabel: {
    fontFamily: FONTS.regular,
    fontSize: 12,
    color: "rgba(255,255,255,0.7)",
    marginTop: 2,
  },
  infoDivider: {
    width: 1,
    height: 36,
    backgroundColor: "rgba(255,255,255,0.2)",
  },
  buttonContainer: {
    width: "100%",
    alignItems: "center",
    marginTop: "auto",
  },
  disclaimer: {
    fontFamily: FONTS.regular,
    fontSize: 12,
    color: "rgba(255,255,255,0.5)",
    marginTop: SPACING.md,
  },
});
```

---

### 8.8 `app/(tabs)/_layout.tsx`

```tsx
// app/(tabs)/_layout.tsx
// Tab navigator layout with three tabs: Elections, Vote, Results.
// The bottom tab bar is styled with the NACCM green/white theme.
// Safe area padding ensures the tab bar doesn't overlap system navigation on Android.

import { Tabs } from "expo-router";
import { Ionicons } from "@expo/vector-icons";
import { Platform, View, StyleSheet } from "react-native";
import { useSafeAreaInsets } from "react-native-safe-area-context";
import { BlurView } from "expo-blur";
import { COLORS, FONTS } from "../../constants/theme";

export default function TabsLayout() {
  // ── Use insets to prevent tab bar from overlapping Android nav buttons ──
  const insets = useSafeAreaInsets();

  return (
    <Tabs
      screenOptions={{
        headerShown: false,
        // Tab bar background: frosted glass effect
        tabBarBackground: () => (
          <BlurView
            intensity={80}
            tint="dark"
            style={StyleSheet.absoluteFill}
          />
        ),
        tabBarStyle: {
          backgroundColor:
            Platform.OS === "ios" ? "transparent" : "rgba(0,40,0,0.92)",
          borderTopColor: "rgba(255,255,255,0.15)",
          borderTopWidth: 1,
          // Pad bottom to account for Android gesture navigation bar
          paddingBottom: insets.bottom > 0 ? insets.bottom : 8,
          paddingTop: 8,
          height: 60 + (insets.bottom > 0 ? insets.bottom : 8),
          position: "absolute", // Floating tab bar for modern look
          elevation: 0, // Remove Android shadow on tab bar
        },
        tabBarActiveTintColor: COLORS.primaryLight,
        tabBarInactiveTintColor: "rgba(255,255,255,0.45)",
        tabBarLabelStyle: {
          fontFamily: FONTS.medium,
          fontSize: 11,
          marginTop: 2,
        },
      }}
    >
      {/* ── Tab 1: Elections ── */}
      <Tabs.Screen
        name="elections"
        options={{
          title: "Elections",
          tabBarIcon: ({ color, size }) => (
            <Ionicons name="list" color={color} size={size} />
          ),
        }}
      />

      {/* ── Tab 2: Vote (prominent center tab) ── */}
      <Tabs.Screen
        name="vote"
        options={{
          title: "Vote",
          tabBarIcon: ({ color, focused }) => (
            <View
              style={[
                styles.voteIconWrapper,
                focused && styles.voteIconWrapperActive,
              ]}
            >
              <Ionicons
                name="checkmark-circle"
                color={focused ? COLORS.white : color}
                size={28}
              />
            </View>
          ),
        }}
      />

      {/* ── Tab 3: Results ── */}
      <Tabs.Screen
        name="results"
        options={{
          title: "Results",
          tabBarIcon: ({ color, size }) => (
            <Ionicons name="bar-chart" color={color} size={size} />
          ),
        }}
      />
    </Tabs>
  );
}

const styles = StyleSheet.create({
  // Elevated pill wrapper for the center Vote tab icon
  voteIconWrapper: {
    width: 52,
    height: 36,
    borderRadius: 18,
    backgroundColor: "rgba(0,102,0,0.4)",
    alignItems: "center",
    justifyContent: "center",
    marginBottom: 2,
  },
  voteIconWrapperActive: {
    backgroundColor: COLORS.primary,
  },
});
```

---

### 8.9 `app/(tabs)/elections.tsx`

```tsx
// app/(tabs)/elections.tsx
// Displays all 8 election positions as interactive glass cards.
// Each card shows the office title, icon, and whether the user has already voted.
// Tapping a card navigates to the Vote tab for that specific office.

import { useEffect, useState, useCallback } from "react";
import {
  View,
  Text,
  ScrollView,
  StyleSheet,
  TouchableOpacity,
  Dimensions,
  Platform,
} from "react-native";
import { router } from "expo-router";
import { LinearGradient } from "expo-linear-gradient";
import { Ionicons } from "@expo/vector-icons";
import { useSafeAreaInsets } from "react-native-safe-area-context";
import Animated, {
  useSharedValue,
  useAnimatedStyle,
  withDelay,
  withTiming,
  FadeInDown,
} from "react-native-reanimated";
import { supabase, DBOffice } from "../../lib/supabase";
import { COLORS, FONTS, SPACING, RADIUS, SHADOW } from "../../constants/theme";
import Header from "../../components/Header";

const { width } = Dimensions.get("window");
const CARD_WIDTH = (width - SPACING.md * 3) / 2; // Two-column grid

export default function ElectionsScreen() {
  const insets = useSafeAreaInsets();
  const [offices, setOffices] = useState<DBOffice[]>([]);
  const [votedOffices, setVotedOffices] = useState<string[]>([]);
  const [loading, setLoading] = useState(true);

  // ── Fetch offices from Supabase ──────────────────────────────────────────
  const fetchOffices = useCallback(async () => {
    const { data, error } = await supabase
      .from("offices")
      .select("*")
      .eq("is_active", true)
      .order("created_at");

    if (!error && data) setOffices(data);
    setLoading(false);
  }, []);

  // ── Fetch which offices the current user has already voted in ────────────
  const fetchVotedOffices = useCallback(async () => {
    const {
      data: { user },
    } = await supabase.auth.getUser();
    if (!user) return;

    const { data, error } = await supabase
      .from("votes")
      .select("office_id")
      .eq("user_id", user.id);

    if (!error && data) {
      setVotedOffices(data.map((v) => v.office_id));
    }
  }, []);

  useEffect(() => {
    fetchOffices();
    fetchVotedOffices();
  }, []);

  // ── Navigate to vote screen with selected office ─────────────────────────
  const handleOfficePress = (office: DBOffice) => {
    // Pass office id as query param
    router.push({ pathname: "/(tabs)/vote", params: { officeId: office.id } });
  };

  const officeIcons: Record<string, keyof typeof Ionicons.glyphMap> = {
    President: "shield-checkmark",
    "Vice President": "people",
    Secretary: "document-text",
    "Assistant Secretary": "documents",
    Liturgist: "flower",
    "Disciplinary Officer (DPO)": "hammer",
    "Director of Information": "megaphone",
    "Financial Secretary": "cash",
  };

  return (
    <LinearGradient
      colors={COLORS.backgroundGradient as [string, string, string]}
      style={styles.container}
    >
      {/* Header with NACCM branding */}
      <Header title="Elections" subtitle="Select an office to vote" />

      {/* Scrollable grid of election office cards */}
      <ScrollView
        style={styles.scroll}
        contentContainerStyle={[
          styles.scrollContent,
          // Add bottom padding so last row isn't hidden behind the tab bar
          { paddingBottom: insets.bottom + 80 },
        ]}
        showsVerticalScrollIndicator={false}
      >
        {/* Progress summary */}
        <View style={styles.progressContainer}>
          <View style={styles.progressBar}>
            <View
              style={[
                styles.progressFill,
                { width: `${(votedOffices.length / 8) * 100}%` },
              ]}
            />
          </View>
          <Text style={styles.progressText}>
            {votedOffices.length} of 8 votes cast
          </Text>
        </View>

        {/* Two-column grid of office cards */}
        <View style={styles.grid}>
          {offices.map((office, index) => {
            const hasVoted = votedOffices.includes(office.id);
            const icon = officeIcons[office.title] ?? "person";

            return (
              // Stagger entrance animation per card using FadeInDown
              <Animated.View
                key={office.id}
                entering={FadeInDown.delay(index * 80).springify()}
              >
                <TouchableOpacity
                  style={[styles.card, hasVoted && styles.cardVoted]}
                  onPress={() => handleOfficePress(office)}
                  activeOpacity={0.85}
                >
                  {/* Glassmorphism overlay */}
                  <View style={styles.glassOverlay} />

                  {/* Voted badge */}
                  {hasVoted && (
                    <View style={styles.votedBadge}>
                      <Ionicons
                        name="checkmark"
                        size={12}
                        color={COLORS.white}
                      />
                    </View>
                  )}

                  {/* Office icon */}
                  <View
                    style={[
                      styles.iconCircle,
                      hasVoted && styles.iconCircleVoted,
                    ]}
                  >
                    <Ionicons
                      name={icon}
                      size={28}
                      color={hasVoted ? COLORS.white : COLORS.primaryLight}
                    />
                  </View>

                  {/* Office title */}
                  <Text style={styles.cardTitle} numberOfLines={2}>
                    {office.title}
                  </Text>

                  {/* Status label */}
                  <Text
                    style={[
                      styles.statusText,
                      hasVoted && styles.statusTextVoted,
                    ]}
                  >
                    {hasVoted ? "✓ Voted" : "Tap to vote"}
                  </Text>
                </TouchableOpacity>
              </Animated.View>
            );
          })}
        </View>
      </ScrollView>
    </LinearGradient>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1 },
  scroll: { flex: 1 },
  scrollContent: {
    paddingHorizontal: SPACING.md,
    paddingTop: SPACING.md,
  },
  progressContainer: {
    marginBottom: SPACING.lg,
  },
  progressBar: {
    height: 6,
    backgroundColor: "rgba(255,255,255,0.2)",
    borderRadius: 3,
    overflow: "hidden",
    marginBottom: SPACING.xs,
  },
  progressFill: {
    height: "100%",
    backgroundColor: COLORS.primaryLight,
    borderRadius: 3,
  },
  progressText: {
    fontFamily: FONTS.medium,
    fontSize: 13,
    color: "rgba(255,255,255,0.7)",
  },
  grid: {
    flexDirection: "row",
    flexWrap: "wrap",
    gap: SPACING.md,
    justifyContent: "space-between",
  },
  card: {
    width: CARD_WIDTH,
    backgroundColor: COLORS.glassBg,
    borderRadius: RADIUS.lg,
    borderWidth: 1,
    borderColor: COLORS.glassBorder,
    padding: SPACING.md,
    alignItems: "center",
    minHeight: 150,
    justifyContent: "center",
    overflow: "hidden",
    ...SHADOW.card,
  },
  cardVoted: {
    backgroundColor: "rgba(0,120,0,0.25)",
    borderColor: "rgba(0,200,0,0.4)",
  },
  glassOverlay: {
    ...StyleSheet.absoluteFillObject,
    backgroundColor: "rgba(255,255,255,0.04)",
    borderRadius: RADIUS.lg,
  },
  votedBadge: {
    position: "absolute",
    top: SPACING.sm,
    right: SPACING.sm,
    width: 22,
    height: 22,
    borderRadius: 11,
    backgroundColor: COLORS.success,
    alignItems: "center",
    justifyContent: "center",
  },
  iconCircle: {
    width: 60,
    height: 60,
    borderRadius: 30,
    backgroundColor: "rgba(255,255,255,0.12)",
    borderWidth: 1,
    borderColor: "rgba(255,255,255,0.2)",
    alignItems: "center",
    justifyContent: "center",
    marginBottom: SPACING.sm,
  },
  iconCircleVoted: {
    backgroundColor: "rgba(0,150,0,0.4)",
    borderColor: "rgba(0,255,0,0.3)",
  },
  cardTitle: {
    fontFamily: FONTS.semiBold,
    fontSize: 13,
    color: COLORS.white,
    textAlign: "center",
    lineHeight: 18,
    marginBottom: SPACING.xs,
  },
  statusText: {
    fontFamily: FONTS.regular,
    fontSize: 11,
    color: "rgba(255,255,255,0.55)",
  },
  statusTextVoted: {
    color: "#66FF66",
  },
});
```

---

### 8.10 `app/(tabs)/vote.tsx`

```tsx
// app/(tabs)/vote.tsx
// The actual voting screen. Shows candidates for the selected office.
// Handles: selecting a candidate, confirming vote, submitting to Supabase,
// and preventing duplicate votes via the unique constraint in the DB.

import { useEffect, useState } from "react";
import {
  View,
  Text,
  ScrollView,
  StyleSheet,
  TouchableOpacity,
  Alert,
  Dimensions,
} from "react-native";
import { useLocalSearchParams, router } from "expo-router";
import { LinearGradient } from "expo-linear-gradient";
import { Ionicons } from "@expo/vector-icons";
import * as Haptics from "expo-haptics";
import { useSafeAreaInsets } from "react-native-safe-area-context";
import Animated, {
  useSharedValue,
  useAnimatedStyle,
  withSpring,
  withTiming,
  FadeIn,
  ZoomIn,
} from "react-native-reanimated";
import { supabase, DBCandidate, DBOffice } from "../../lib/supabase";
import { COLORS, FONTS, SPACING, RADIUS, SHADOW } from "../../constants/theme";
import Header from "../../components/Header";
import AnimatedButton from "../../components/AnimatedButton";

const { width } = Dimensions.get("window");

export default function VoteScreen() {
  const insets = useSafeAreaInsets();
  // ── Get officeId passed from elections screen ────────────────────────────
  const { officeId } = useLocalSearchParams<{ officeId: string }>();

  const [office, setOffice] = useState<DBOffice | null>(null);
  const [candidates, setCandidates] = useState<DBCandidate[]>([]);
  const [selectedId, setSelectedId] = useState<string | null>(null);
  const [hasVoted, setHasVoted] = useState(false);
  const [submitting, setSubmitting] = useState(false);
  const [loading, setLoading] = useState(true);

  // ── Success animation scale ──────────────────────────────────────────────
  const successScale = useSharedValue(0);
  const successStyle = useAnimatedStyle(() => ({
    transform: [{ scale: successScale.value }],
    opacity: successScale.value,
  }));

  useEffect(() => {
    if (officeId) {
      fetchOfficeData();
      checkIfVoted();
    }
  }, [officeId]);

  // ── Fetch office details and its candidates ──────────────────────────────
  const fetchOfficeData = async () => {
    // Fetch office info
    const { data: officeData } = await supabase
      .from("offices")
      .select("*")
      .eq("id", officeId)
      .single();

    if (officeData) setOffice(officeData);

    // Fetch candidates for this office
    const { data: candidateData } = await supabase
      .from("candidates")
      .select("*")
      .eq("office_id", officeId)
      .order("full_name");

    if (candidateData) setCandidates(candidateData);
    setLoading(false);
  };

  // ── Check if user already voted for this office ──────────────────────────
  const checkIfVoted = async () => {
    const {
      data: { user },
    } = await supabase.auth.getUser();
    if (!user) return;

    const { data } = await supabase
      .from("votes")
      .select("candidate_id")
      .eq("user_id", user.id)
      .eq("office_id", officeId)
      .maybeSingle();

    if (data) {
      setHasVoted(true);
      setSelectedId(data.candidate_id);
    }
  };

  // ── Select candidate with haptic feedback ───────────────────────────────
  const handleSelect = (candidateId: string) => {
    if (hasVoted) return; // Prevent re-selection after voting
    Haptics.impactAsync(Haptics.ImpactFeedbackStyle.Light);
    setSelectedId(candidateId);
  };

  // ── Submit vote to Supabase ──────────────────────────────────────────────
  const handleVote = async () => {
    if (!selectedId || !officeId) return;

    // Confirm with the user before submitting
    Alert.alert(
      "⚠️ Confirm Vote",
      `You are about to vote for:\n\n${candidates.find((c) => c.id === selectedId)?.full_name}\n\nThis action cannot be undone.`,
      [
        { text: "Cancel", style: "cancel" },
        {
          text: "Confirm Vote",
          style: "default",
          onPress: submitVote,
        },
      ],
    );
  };

  const submitVote = async () => {
    setSubmitting(true);
    const {
      data: { user },
    } = await supabase.auth.getUser();

    if (!user) {
      Alert.alert("Error", "Authentication error. Please restart the app.");
      setSubmitting(false);
      return;
    }

    // Insert vote record — the unique constraint (user_id, office_id)
    // prevents double voting at the database level
    const { error: voteError } = await supabase
      .from("votes")
      .insert({
        user_id: user.id,
        office_id: officeId,
        candidate_id: selectedId,
      });

    if (voteError) {
      if (voteError.code === "23505") {
        // Unique constraint violation = already voted
        Alert.alert(
          "Already Voted",
          "You have already voted for this position.",
        );
      } else {
        Alert.alert("Error", "Failed to submit your vote. Please try again.");
      }
      setSubmitting(false);
      return;
    }

    // Increment the candidate's vote count using our DB function
    await supabase.rpc("increment_vote", { candidate_id: selectedId });

    // Trigger success animation
    Haptics.notificationAsync(Haptics.NotificationFeedbackType.Success);
    successScale.value = withSpring(1, { damping: 10 });
    setHasVoted(true);
    setSubmitting(false);
  };

  return (
    <LinearGradient
      colors={COLORS.backgroundGradient as [string, string, string]}
      style={styles.container}
    >
      <Header
        title={office?.title ?? "Vote"}
        subtitle="Select your preferred candidate"
        showBack
        onBack={() => router.back()}
      />

      <ScrollView
        style={styles.scroll}
        contentContainerStyle={[
          styles.scrollContent,
          { paddingBottom: insets.bottom + 90 },
        ]}
        showsVerticalScrollIndicator={false}
      >
        {/* Office info card */}
        {office && (
          <Animated.View
            entering={FadeIn.duration(400)}
            style={styles.officeCard}
          >
            <Text style={styles.officeDescription}>{office.description}</Text>
          </Animated.View>
        )}

        {/* Already-voted banner */}
        {hasVoted && (
          <Animated.View style={[styles.votedBanner, successStyle]}>
            <Ionicons
              name="checkmark-circle"
              size={24}
              color={COLORS.success}
            />
            <Text style={styles.votedBannerText}>
              Vote submitted successfully!
            </Text>
          </Animated.View>
        )}

        {/* Candidate list */}
        <Text style={styles.sectionTitle}>
          {candidates.length} Candidate{candidates.length !== 1 ? "s" : ""}
        </Text>

        {candidates.map((candidate, index) => {
          const isSelected = selectedId === candidate.id;
          return (
            <Animated.View
              key={candidate.id}
              entering={FadeIn.delay(index * 100)}
            >
              <TouchableOpacity
                style={[
                  styles.candidateCard,
                  isSelected && styles.candidateCardSelected,
                ]}
                onPress={() => handleSelect(candidate.id)}
                activeOpacity={0.85}
                disabled={hasVoted}
              >
                {/* Avatar circle with initials */}
                <View
                  style={[styles.avatar, isSelected && styles.avatarSelected]}
                >
                  <Text style={styles.avatarText}>
                    {candidate.full_name
                      .split(" ")
                      .map((n) => n[0])
                      .join("")
                      .slice(0, 2)
                      .toUpperCase()}
                  </Text>
                </View>

                {/* Candidate info */}
                <View style={styles.candidateInfo}>
                  <Text style={styles.candidateName}>
                    {candidate.full_name}
                  </Text>
                  {candidate.bio && (
                    <Text style={styles.candidateBio} numberOfLines={2}>
                      {candidate.bio}
                    </Text>
                  )}
                </View>

                {/* Selection radio indicator */}
                <View
                  style={[styles.radio, isSelected && styles.radioSelected]}
                >
                  {isSelected && <View style={styles.radioDot} />}
                </View>
              </TouchableOpacity>
            </Animated.View>
          );
        })}
      </ScrollView>

      {/* Fixed bottom vote button — safe area padded */}
      {!hasVoted && (
        <View
          style={[
            styles.bottomBar,
            { paddingBottom: insets.bottom + SPACING.md },
          ]}
        >
          <AnimatedButton
            title={submitting ? "Submitting..." : "Cast My Vote"}
            onPress={handleVote}
            variant="white"
            size="large"
            disabled={!selectedId || submitting}
            icon="checkmark-circle"
          />
        </View>
      )}
    </LinearGradient>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1 },
  scroll: { flex: 1 },
  scrollContent: {
    paddingHorizontal: SPACING.md,
    paddingTop: SPACING.md,
  },
  officeCard: {
    backgroundColor: "rgba(255,255,255,0.1)",
    borderRadius: RADIUS.md,
    borderWidth: 1,
    borderColor: "rgba(255,255,255,0.2)",
    padding: SPACING.md,
    marginBottom: SPACING.lg,
  },
  officeDescription: {
    fontFamily: FONTS.regular,
    fontSize: 14,
    color: "rgba(255,255,255,0.8)",
    lineHeight: 21,
  },
  votedBanner: {
    flexDirection: "row",
    alignItems: "center",
    backgroundColor: "rgba(0,180,0,0.2)",
    borderRadius: RADIUS.md,
    borderWidth: 1,
    borderColor: "rgba(0,255,0,0.3)",
    padding: SPACING.md,
    marginBottom: SPACING.lg,
    gap: SPACING.sm,
  },
  votedBannerText: {
    fontFamily: FONTS.semiBold,
    fontSize: 14,
    color: "#66FF66",
  },
  sectionTitle: {
    fontFamily: FONTS.semiBold,
    fontSize: 14,
    color: "rgba(255,255,255,0.7)",
    marginBottom: SPACING.md,
    textTransform: "uppercase",
    letterSpacing: 1,
  },
  candidateCard: {
    flexDirection: "row",
    alignItems: "center",
    backgroundColor: "rgba(255,255,255,0.1)",
    borderRadius: RADIUS.lg,
    borderWidth: 1,
    borderColor: "rgba(255,255,255,0.15)",
    padding: SPACING.md,
    marginBottom: SPACING.md,
    gap: SPACING.md,
    ...SHADOW.card,
  },
  candidateCardSelected: {
    backgroundColor: "rgba(0,160,0,0.25)",
    borderColor: "rgba(0,255,0,0.5)",
  },
  avatar: {
    width: 52,
    height: 52,
    borderRadius: 26,
    backgroundColor: "rgba(255,255,255,0.15)",
    borderWidth: 1,
    borderColor: "rgba(255,255,255,0.3)",
    alignItems: "center",
    justifyContent: "center",
  },
  avatarSelected: {
    backgroundColor: COLORS.primary,
    borderColor: COLORS.primaryLight,
  },
  avatarText: {
    fontFamily: FONTS.bold,
    fontSize: 18,
    color: COLORS.white,
  },
  candidateInfo: { flex: 1 },
  candidateName: {
    fontFamily: FONTS.semiBold,
    fontSize: 16,
    color: COLORS.white,
    marginBottom: 3,
  },
  candidateBio: {
    fontFamily: FONTS.regular,
    fontSize: 12,
    color: "rgba(255,255,255,0.6)",
    lineHeight: 17,
  },
  radio: {
    width: 24,
    height: 24,
    borderRadius: 12,
    borderWidth: 2,
    borderColor: "rgba(255,255,255,0.4)",
    alignItems: "center",
    justifyContent: "center",
  },
  radioSelected: {
    borderColor: COLORS.primaryLight,
    backgroundColor: "rgba(0,170,0,0.2)",
  },
  radioDot: {
    width: 12,
    height: 12,
    borderRadius: 6,
    backgroundColor: COLORS.primaryLight,
  },
  bottomBar: {
    paddingHorizontal: SPACING.md,
    paddingTop: SPACING.md,
    backgroundColor: "rgba(0,30,0,0.85)",
    borderTopWidth: 1,
    borderTopColor: "rgba(255,255,255,0.1)",
  },
});
```

---

### 8.11 `app/(tabs)/results.tsx`

```tsx
// app/(tabs)/results.tsx
// Live election results screen.
// Shows animated progress bars for each candidate grouped by office.
// Uses Supabase real-time subscription to update results automatically.

import { useEffect, useState, useCallback } from "react";
import {
  View,
  Text,
  ScrollView,
  StyleSheet,
  TouchableOpacity,
} from "react-native";
import { LinearGradient } from "expo-linear-gradient";
import { useSafeAreaInsets } from "react-native-safe-area-context";
import Animated, { FadeIn } from "react-native-reanimated";
import { supabase, DBOffice, DBCandidate } from "../../lib/supabase";
import { COLORS, FONTS, SPACING, RADIUS } from "../../constants/theme";
import Header from "../../components/Header";
import ResultBar from "../../components/ResultBar";

interface OfficeWithCandidates extends DBOffice {
  candidates: DBCandidate[];
}

export default function ResultsScreen() {
  const insets = useSafeAreaInsets();
  const [data, setData] = useState<OfficeWithCandidates[]>([]);
  const [selectedOfficeId, setSelectedOfficeId] = useState<string | null>(null);

  // ── Fetch all offices with their candidates and vote counts ─────────────
  const fetchResults = useCallback(async () => {
    const { data: offices } = await supabase
      .from("offices")
      .select("*, candidates(*)")
      .eq("is_active", true)
      .order("created_at");

    if (offices) {
      // Sort candidates by vote count descending for each office
      const sorted = offices.map((office: any) => ({
        ...office,
        candidates: (office.candidates ?? []).sort(
          (a: DBCandidate, b: DBCandidate) => b.vote_count - a.vote_count,
        ),
      }));
      setData(sorted);
      if (!selectedOfficeId && sorted.length > 0) {
        setSelectedOfficeId(sorted[0].id);
      }
    }
  }, [selectedOfficeId]);

  useEffect(() => {
    fetchResults();

    // ── Subscribe to real-time updates on the candidates table ───────────
    // Fires whenever a vote count changes
    const channel = supabase
      .channel("results-realtime")
      .on(
        "postgres_changes",
        { event: "UPDATE", schema: "public", table: "candidates" },
        () => fetchResults(), // Re-fetch on any candidate update
      )
      .subscribe();

    // Cleanup subscription when component unmounts
    return () => {
      supabase.removeChannel(channel);
    };
  }, []);

  const selectedOffice = data.find((o) => o.id === selectedOfficeId);

  return (
    <LinearGradient
      colors={COLORS.backgroundGradient as [string, string, string]}
      style={styles.container}
    >
      <Header title="Live Results" subtitle="Updated in real time" />

      {/* Horizontal office selector tabs */}
      <ScrollView
        horizontal
        showsHorizontalScrollIndicator={false}
        style={styles.tabScroll}
        contentContainerStyle={styles.tabContent}
      >
        {data.map((office) => (
          <TouchableOpacity
            key={office.id}
            style={[
              styles.tab,
              selectedOfficeId === office.id && styles.tabActive,
            ]}
            onPress={() => setSelectedOfficeId(office.id)}
          >
            <Text
              style={[
                styles.tabText,
                selectedOfficeId === office.id && styles.tabTextActive,
              ]}
            >
              {office.title
                .replace("Disciplinary Officer ", "DPO")
                .replace("Director of Information", "D/Info")
                .replace("Financial Secretary", "Fin. Sec.")
                .replace("Assistant Secretary", "Asst. Sec.")}
            </Text>
          </TouchableOpacity>
        ))}
      </ScrollView>

      {/* Results for selected office */}
      <ScrollView
        style={styles.scroll}
        contentContainerStyle={[
          styles.scrollContent,
          { paddingBottom: insets.bottom + 80 },
        ]}
        showsVerticalScrollIndicator={false}
      >
        {selectedOffice && (
          <Animated.View entering={FadeIn.duration(300)}>
            {/* Total votes summary */}
            <View style={styles.summaryCard}>
              <Text style={styles.summaryTitle}>{selectedOffice.title}</Text>
              <Text style={styles.summaryVotes}>
                Total votes:{" "}
                {selectedOffice.candidates.reduce(
                  (sum, c) => sum + c.vote_count,
                  0,
                )}
              </Text>
            </View>

            {/* Candidate result bars */}
            {selectedOffice.candidates.map((candidate, index) => {
              const totalVotes = selectedOffice.candidates.reduce(
                (sum, c) => sum + c.vote_count,
                0,
              );
              const percentage =
                totalVotes > 0
                  ? Math.round((candidate.vote_count / totalVotes) * 100)
                  : 0;
              const isLeading = index === 0 && candidate.vote_count > 0;

              return (
                <ResultBar
                  key={candidate.id}
                  name={candidate.full_name}
                  votes={candidate.vote_count}
                  percentage={percentage}
                  isLeading={isLeading}
                  rank={index + 1}
                  delay={index * 150}
                />
              );
            })}
          </Animated.View>
        )}
      </ScrollView>
    </LinearGradient>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1 },
  tabScroll: { maxHeight: 52, flexGrow: 0 },
  tabContent: {
    paddingHorizontal: SPACING.md,
    paddingVertical: SPACING.sm,
    gap: SPACING.sm,
    alignItems: "center",
  },
  tab: {
    paddingHorizontal: SPACING.md,
    paddingVertical: SPACING.xs + 2,
    borderRadius: RADIUS.full,
    backgroundColor: "rgba(255,255,255,0.1)",
    borderWidth: 1,
    borderColor: "rgba(255,255,255,0.15)",
  },
  tabActive: {
    backgroundColor: COLORS.primaryLight,
    borderColor: "transparent",
  },
  tabText: {
    fontFamily: FONTS.medium,
    fontSize: 12,
    color: "rgba(255,255,255,0.7)",
  },
  tabTextActive: {
    color: COLORS.white,
    fontFamily: FONTS.semiBold,
  },
  scroll: { flex: 1 },
  scrollContent: {
    paddingHorizontal: SPACING.md,
    paddingTop: SPACING.md,
  },
  summaryCard: {
    backgroundColor: "rgba(255,255,255,0.1)",
    borderRadius: RADIUS.md,
    borderWidth: 1,
    borderColor: "rgba(255,255,255,0.15)",
    padding: SPACING.md,
    marginBottom: SPACING.lg,
  },
  summaryTitle: {
    fontFamily: FONTS.bold,
    fontSize: 18,
    color: COLORS.white,
    marginBottom: 4,
  },
  summaryVotes: {
    fontFamily: FONTS.regular,
    fontSize: 13,
    color: "rgba(255,255,255,0.6)",
  },
});
```

---

### 8.12 `components/GlassCard.tsx`

```tsx
// components/GlassCard.tsx
// Reusable glassmorphism card component.
// Uses BlurView for the frosted glass effect on iOS,
// falls back to a semi-transparent View on Android (BlurView is iOS-optimised).

import { View, StyleSheet, Platform, ViewStyle } from "react-native";
import { BlurView } from "expo-blur";
import { COLORS, RADIUS, SHADOW } from "../constants/theme";

interface GlassCardProps {
  children: React.ReactNode;
  style?: ViewStyle;
  intensity?: number; // Blur intensity 0–100
}

export default function GlassCard({
  children,
  style,
  intensity = 40,
}: GlassCardProps) {
  // iOS: Use BlurView for true frosted glass
  // Android: Use semi-transparent background (BlurView has limited Android support)
  if (Platform.OS === "ios") {
    return (
      <BlurView intensity={intensity} tint="dark" style={[styles.card, style]}>
        {/* Inner tint layer for green hue */}
        <View style={styles.tintOverlay} />
        {children}
      </BlurView>
    );
  }

  // Android fallback — solid semi-transparent card
  return (
    <View style={[styles.card, styles.androidCard, style]}>{children}</View>
  );
}

const styles = StyleSheet.create({
  card: {
    borderRadius: RADIUS.lg,
    borderWidth: 1,
    borderColor: COLORS.glassBorder,
    overflow: "hidden",
    ...SHADOW.card,
  },
  tintOverlay: {
    ...StyleSheet.absoluteFillObject,
    backgroundColor: COLORS.glassBg,
  },
  androidCard: {
    backgroundColor: "rgba(0, 40, 0, 0.75)",
  },
});
```

---

### 8.13 `components/CandidateCard.tsx`

```tsx
// components/CandidateCard.tsx
// Animated candidate selection card with press scale animation.
// Shows candidate name, initials avatar, and selection state.

import { TouchableOpacity, View, Text, StyleSheet } from "react-native";
import Animated, {
  useSharedValue,
  useAnimatedStyle,
  withSpring,
} from "react-native-reanimated";
import { COLORS, FONTS, SPACING, RADIUS } from "../constants/theme";

interface CandidateCardProps {
  name: string;
  bio?: string;
  selected: boolean;
  disabled?: boolean;
  onPress: () => void;
}

const AnimatedTouchable = Animated.createAnimatedComponent(TouchableOpacity);

export default function CandidateCard({
  name,
  bio,
  selected,
  disabled,
  onPress,
}: CandidateCardProps) {
  // Scale down on press, spring back on release for tactile feedback
  const scale = useSharedValue(1);
  const animatedStyle = useAnimatedStyle(() => ({
    transform: [{ scale: scale.value }],
  }));

  // Extract initials from full name (max 2 characters)
  const initials = name
    .split(" ")
    .map((word) => word[0])
    .join("")
    .slice(0, 2)
    .toUpperCase();

  return (
    <AnimatedTouchable
      style={[styles.card, selected && styles.selected, animatedStyle]}
      onPressIn={() => {
        scale.value = withSpring(0.97);
      }}
      onPressOut={() => {
        scale.value = withSpring(1);
      }}
      onPress={onPress}
      disabled={disabled}
      activeOpacity={1}
    >
      {/* Initials avatar */}
      <View style={[styles.avatar, selected && styles.avatarSelected]}>
        <Text style={styles.initials}>{initials}</Text>
      </View>

      {/* Name and bio */}
      <View style={styles.info}>
        <Text style={styles.name}>{name}</Text>
        {bio && (
          <Text style={styles.bio} numberOfLines={1}>
            {bio}
          </Text>
        )}
      </View>

      {/* Selection indicator */}
      <View style={[styles.check, selected && styles.checkSelected]}>
        {selected && <Text style={styles.checkMark}>✓</Text>}
      </View>
    </AnimatedTouchable>
  );
}

const styles = StyleSheet.create({
  card: {
    flexDirection: "row",
    alignItems: "center",
    backgroundColor: "rgba(255,255,255,0.1)",
    borderRadius: RADIUS.lg,
    borderWidth: 1,
    borderColor: "rgba(255,255,255,0.15)",
    padding: SPACING.md,
    marginBottom: SPACING.md,
    gap: SPACING.md,
  },
  selected: {
    backgroundColor: "rgba(0,150,0,0.25)",
    borderColor: "rgba(0,255,0,0.4)",
  },
  avatar: {
    width: 50,
    height: 50,
    borderRadius: 25,
    backgroundColor: "rgba(255,255,255,0.15)",
    alignItems: "center",
    justifyContent: "center",
  },
  avatarSelected: { backgroundColor: COLORS.primary },
  initials: {
    fontFamily: FONTS.bold,
    fontSize: 18,
    color: COLORS.white,
  },
  info: { flex: 1 },
  name: {
    fontFamily: FONTS.semiBold,
    fontSize: 15,
    color: COLORS.white,
  },
  bio: {
    fontFamily: FONTS.regular,
    fontSize: 12,
    color: "rgba(255,255,255,0.6)",
    marginTop: 2,
  },
  check: {
    width: 26,
    height: 26,
    borderRadius: 13,
    borderWidth: 2,
    borderColor: "rgba(255,255,255,0.3)",
    alignItems: "center",
    justifyContent: "center",
  },
  checkSelected: {
    backgroundColor: COLORS.primaryLight,
    borderColor: "transparent",
  },
  checkMark: {
    color: COLORS.white,
    fontSize: 14,
    fontFamily: FONTS.bold,
  },
});
```

---

### 8.14 `components/ResultBar.tsx`

```tsx
// components/ResultBar.tsx
// Animated horizontal progress bar for displaying vote results.
// The bar animates in from 0% to the actual percentage on mount.

import { useEffect } from "react";
import { View, Text, StyleSheet } from "react-native";
import Animated, {
  useSharedValue,
  useAnimatedStyle,
  withDelay,
  withTiming,
  Easing,
} from "react-native-reanimated";
import { COLORS, FONTS, SPACING, RADIUS } from "../constants/theme";

interface ResultBarProps {
  name: string;
  votes: number;
  percentage: number; // 0–100
  isLeading: boolean;
  rank: number;
  delay?: number; // Animation delay in ms
}

export default function ResultBar({
  name,
  votes,
  percentage,
  isLeading,
  rank,
  delay = 0,
}: ResultBarProps) {
  // Shared value drives the bar width animation (0 → percentage)
  const barWidth = useSharedValue(0);

  useEffect(() => {
    // Animate bar width after optional delay
    barWidth.value = withDelay(
      delay,
      withTiming(percentage, {
        duration: 800,
        easing: Easing.out(Easing.cubic),
      }),
    );
  }, [percentage]);

  // Bar width animates as a percentage of the container
  const barStyle = useAnimatedStyle(() => ({
    width: `${barWidth.value}%`,
  }));

  return (
    <View style={[styles.container, isLeading && styles.containerLeading]}>
      {/* Rank + Name row */}
      <View style={styles.header}>
        <View style={[styles.rankBadge, isLeading && styles.rankBadgeLeading]}>
          <Text style={styles.rankText}>#{rank}</Text>
        </View>
        <Text style={styles.name} numberOfLines={1}>
          {name}
        </Text>
        {isLeading && (
          <View style={styles.leadingBadge}>
            <Text style={styles.leadingText}>LEADING</Text>
          </View>
        )}
      </View>

      {/* Animated progress bar */}
      <View style={styles.track}>
        <Animated.View
          style={[styles.fill, isLeading && styles.fillLeading, barStyle]}
        />
      </View>

      {/* Stats row: percentage and vote count */}
      <View style={styles.stats}>
        <Text
          style={[styles.percentage, isLeading && styles.percentageLeading]}
        >
          {percentage}%
        </Text>
        <Text style={styles.voteCount}>
          {votes} vote{votes !== 1 ? "s" : ""}
        </Text>
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    backgroundColor: "rgba(255,255,255,0.08)",
    borderRadius: RADIUS.md,
    borderWidth: 1,
    borderColor: "rgba(255,255,255,0.12)",
    padding: SPACING.md,
    marginBottom: SPACING.md,
  },
  containerLeading: {
    backgroundColor: "rgba(0,120,0,0.2)",
    borderColor: "rgba(0,200,0,0.35)",
  },
  header: {
    flexDirection: "row",
    alignItems: "center",
    marginBottom: SPACING.sm,
    gap: SPACING.sm,
  },
  rankBadge: {
    width: 28,
    height: 28,
    borderRadius: 14,
    backgroundColor: "rgba(255,255,255,0.15)",
    alignItems: "center",
    justifyContent: "center",
  },
  rankBadgeLeading: { backgroundColor: COLORS.primaryLight },
  rankText: {
    fontFamily: FONTS.bold,
    fontSize: 12,
    color: COLORS.white,
  },
  name: {
    flex: 1,
    fontFamily: FONTS.semiBold,
    fontSize: 15,
    color: COLORS.white,
  },
  leadingBadge: {
    backgroundColor: "rgba(0,200,0,0.25)",
    borderWidth: 1,
    borderColor: "rgba(0,255,0,0.4)",
    borderRadius: RADIUS.full,
    paddingHorizontal: 8,
    paddingVertical: 2,
  },
  leadingText: {
    fontFamily: FONTS.bold,
    fontSize: 10,
    color: "#66FF66",
    letterSpacing: 0.5,
  },
  track: {
    height: 10,
    backgroundColor: "rgba(255,255,255,0.1)",
    borderRadius: 5,
    overflow: "hidden",
    marginBottom: SPACING.xs,
  },
  fill: {
    height: "100%",
    backgroundColor: COLORS.primaryLight,
    borderRadius: 5,
    minWidth: 4,
  },
  fillLeading: {
    backgroundColor: "#00EE00",
  },
  stats: {
    flexDirection: "row",
    justifyContent: "space-between",
  },
  percentage: {
    fontFamily: FONTS.bold,
    fontSize: 14,
    color: COLORS.primaryLight,
  },
  percentageLeading: { color: "#66FF66" },
  voteCount: {
    fontFamily: FONTS.regular,
    fontSize: 13,
    color: "rgba(255,255,255,0.55)",
  },
});
```

---

### 8.15 `components/AnimatedButton.tsx`

```tsx
// components/AnimatedButton.tsx
// Reusable animated button with press scale feedback.
// Supports two variants: 'white' (filled) and 'outline'.
// Sizes: 'small', 'medium', 'large'.

import { TouchableOpacity, Text, StyleSheet, ViewStyle } from "react-native";
import { Ionicons } from "@expo/vector-icons";
import Animated, {
  useSharedValue,
  useAnimatedStyle,
  withSpring,
} from "react-native-reanimated";
import { COLORS, FONTS, SPACING, RADIUS } from "../constants/theme";

interface AnimatedButtonProps {
  title: string;
  onPress: () => void;
  variant?: "white" | "outline" | "green";
  size?: "small" | "medium" | "large";
  disabled?: boolean;
  icon?: keyof typeof Ionicons.glyphMap;
  style?: ViewStyle;
}

const AnimatedTouchable = Animated.createAnimatedComponent(TouchableOpacity);

export default function AnimatedButton({
  title,
  onPress,
  variant = "green",
  size = "medium",
  disabled,
  icon,
  style,
}: AnimatedButtonProps) {
  const scale = useSharedValue(1);

  const animatedStyle = useAnimatedStyle(() => ({
    transform: [{ scale: scale.value }],
    opacity: disabled ? 0.5 : 1,
  }));

  const sizeStyles = {
    small: { paddingVertical: 8, paddingHorizontal: 16 },
    medium: { paddingVertical: 12, paddingHorizontal: 24 },
    large: { paddingVertical: 16, paddingHorizontal: 32, width: "100%" as any },
  };

  const variantStyles = {
    white: {
      backgroundColor: COLORS.white,
      borderWidth: 0,
    },
    outline: {
      backgroundColor: "transparent",
      borderWidth: 2,
      borderColor: COLORS.white,
    },
    green: {
      backgroundColor: COLORS.primaryLight,
      borderWidth: 0,
    },
  };

  const textColors = {
    white: COLORS.primaryDark,
    outline: COLORS.white,
    green: COLORS.white,
  };

  return (
    <AnimatedTouchable
      style={[
        styles.button,
        sizeStyles[size],
        variantStyles[variant],
        style,
        animatedStyle,
      ]}
      onPressIn={() => {
        scale.value = withSpring(0.96);
      }}
      onPressOut={() => {
        scale.value = withSpring(1);
      }}
      onPress={onPress}
      disabled={disabled}
      activeOpacity={1}
    >
      {icon && (
        <Ionicons
          name={icon}
          size={20}
          color={textColors[variant]}
          style={{ marginRight: SPACING.xs }}
        />
      )}
      <Text style={[styles.text, { color: textColors[variant] }]}>{title}</Text>
    </AnimatedTouchable>
  );
}

const styles = StyleSheet.create({
  button: {
    flexDirection: "row",
    borderRadius: RADIUS.full,
    alignItems: "center",
    justifyContent: "center",
  },
  text: {
    fontFamily: FONTS.semiBold,
    fontSize: 16,
    textAlign: "center",
  },
});
```

---

### 8.16 `components/Header.tsx`

```tsx
// components/Header.tsx
// Reusable top header for all screens.
// Includes NACCM brand mark, title, subtitle, and optional back button.
// Respects safe area top inset to avoid status bar overlap.

import { View, Text, StyleSheet, TouchableOpacity } from "react-native";
import { Ionicons } from "@expo/vector-icons";
import { useSafeAreaInsets } from "react-native-safe-area-context";
import { COLORS, FONTS, SPACING } from "../constants/theme";

interface HeaderProps {
  title: string;
  subtitle?: string;
  showBack?: boolean;
  onBack?: () => void;
}

export default function Header({
  title,
  subtitle,
  showBack,
  onBack,
}: HeaderProps) {
  // Use top inset to pad header content away from status bar / notch
  const insets = useSafeAreaInsets();

  return (
    <View style={[styles.container, { paddingTop: insets.top + SPACING.sm }]}>
      <View style={styles.row}>
        {/* Back button — only shown when showBack is true */}
        {showBack ? (
          <TouchableOpacity style={styles.backButton} onPress={onBack}>
            <Ionicons name="arrow-back" size={22} color={COLORS.white} />
          </TouchableOpacity>
        ) : (
          // NACCM cross logo mark
          <View style={styles.logoMark}>
            <Text style={styles.logoText}>✝️</Text>
          </View>
        )}

        {/* Title and subtitle */}
        <View style={styles.textContainer}>
          <Text style={styles.title}>{title}</Text>
          {subtitle && <Text style={styles.subtitle}>{subtitle}</Text>}
        </View>

        {/* Right spacer for symmetric layout */}
        <View style={styles.spacer} />
      </View>

      {/* Bottom separator line */}
      <View style={styles.separator} />
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    backgroundColor: "rgba(0,30,0,0.6)",
    paddingBottom: SPACING.md,
    paddingHorizontal: SPACING.md,
    borderBottomWidth: 1,
    borderBottomColor: "rgba(255,255,255,0.1)",
  },
  row: {
    flexDirection: "row",
    alignItems: "center",
    gap: SPACING.md,
  },
  backButton: {
    width: 40,
    height: 40,
    borderRadius: 20,
    backgroundColor: "rgba(255,255,255,0.12)",
    alignItems: "center",
    justifyContent: "center",
  },
  logoMark: {
    width: 40,
    height: 40,
    borderRadius: 20,
    backgroundColor: "rgba(255,255,255,0.15)",
    alignItems: "center",
    justifyContent: "center",
  },
  logoText: { fontSize: 20 },
  textContainer: { flex: 1 },
  title: {
    fontFamily: FONTS.bold,
    fontSize: 20,
    color: COLORS.white,
  },
  subtitle: {
    fontFamily: FONTS.regular,
    fontSize: 12,
    color: "rgba(255,255,255,0.6)",
    marginTop: 2,
  },
  spacer: { width: 40 },
  separator: {
    height: 0, // Separator is handled by borderBottomWidth on container
  },
});
```

---

## 9. Running the App

```bash
# Start the Expo development server
npx expo start

# Run on Android emulator or device
npx expo start --android

# Run on iOS simulator (Mac only)
npx expo start --ios

# Run in Expo Go on a physical device
# → Scan the QR code shown in terminal with Expo Go app
```

---

## 10. Building for Production

```bash
# Install EAS CLI
npm install -g eas-cli

# Log in to Expo account
eas login

# Configure EAS build
eas build:configure

# Build for Android (.apk or .aab)
eas build --platform android

# Build for iOS (.ipa)
eas build --platform ios

# Submit to stores
eas submit --platform android
eas submit --platform ios
```

---

## 11. Troubleshooting

| Issue                            | Fix                                                                           |
| -------------------------------- | ----------------------------------------------------------------------------- |
| `reanimated` crash on Android    | Ensure `react-native-reanimated/plugin` is **last** in `babel.config.js`      |
| Tab bar overlaps content         | Wrap screen content in `useSafeAreaInsets()` and add `paddingBottom`          |
| Supabase session not persisting  | Confirm `AsyncStorage` is installed and passed to `createClient`              |
| BlurView no effect on Android    | Expected — falls back to semi-transparent background. Use `Platform.OS` check |
| "Already voted" error            | This is correct behavior — the DB unique constraint prevents double voting    |
| Fonts not loading                | Check `@expo-google-fonts/poppins` is installed and all 4 variants are loaded |
| White status bar icons invisible | Set `<StatusBar style="light" />` from `expo-status-bar`                      |
| Notch overlap on iPhone          | Ensure `SafeAreaProvider` wraps the root and `useSafeAreaInsets()` is used    |
| Android nav bar overlap          | Use `insets.bottom` from `useSafeAreaInsets()` for bottom padding             |

---

> **Made with ❤️ for NACCM Lagos Chapter**  
> _"Ad Majorem Dei Gloriam"_
