# 🗣️ WizzWords - Augmentative and Alternative Communication Platform

<div align="center">

**Augmentative and Alternative Communication (AAC) platform that helps children with special needs communicate through ARASAAC pictograms and AI-powered phrase generation.**

[Features](#-features) • [Installation](#-installation) • [API Configuration](#-api-configuration) • [Usage](#-usage) • [Architecture](#-architecture) • [Technologies](#-technologies)

</div>

---

## 📖 About the Project

**WizzWords** is a mobile application designed to facilitate communication for children with special needs through the use of ARASAAC pictograms. The application uses artificial intelligence to generate natural phrases from selected symbols, which are shown as flashcards with AI-generated images and text-to-speech.

### 🎯 Objectives

- **Generate** natural-language phrases from selected PCS symbols and display them as flashcards with an image and audio.
- **Adapt** to each child's communication profile and preferences (themes, categories, parent controls).
- **Provide** an accessible, easy-to-use interface with large touch targets and high-contrast color palettes.

---

## ✨ Features

### 🎨 User Interface
- ✅ **Accessible design** with large buttons and high readability
- ✅ **6 customizable color palettes** 
- ✅ **Dual mode**: Interface for children and menu for parents/caregivers
- ✅ **Custom notification system** 
- ✅ **Clear visual indicators** for all actions

### 🖼️ ARASAAC Pictograms
- ✅ **Complete integration** with ARASAAC library
- ✅ **Smart pictogram search** by categories
- ✅ **User-customizable categories**
- ✅ **Dynamic pictogram loading**

### 🤖 Artificial Intelligence
- ✅ **Natural phrase generation** from selected symbols using Azure OpenAI

- ✅ **AI-generated images** on flashcards (DALL-E 3 via backend)
- ✅ **“Generate More”** for extra phrase variants

### 🔊 Text-to-Speech
- ✅ **Voice playback** integrated with Expo Speech
- ✅ **Interactive flashcards** with audio indicator
- ✅ **Tap to play** generated phrases

### 👤 User Management
- ✅ **Firebase authentication**
- ✅ **Personalized user profiles**
- ✅ **User-scoped categories** 
- ✅ **Onboarding system** for new users
- ✅ **Custom avatar generation**

### 🔒 Security and Accessibility
- ✅ **Protected parent mode** with password and 5-attempt lockout (30s cooldown after 5 failed tries)
- ✅ **Password validation** on registration and parent-menu access (min length, no common weak passwords)
- ✅ **Accessibility labels** on main buttons and navigation for screen readers


---

## 🚀 Installation

### Prerequisites

- **Node.js** v18 or higher
- **npm** or **yarn**
- **Expo CLI** 
- **Azure OpenAI API** (required for AI features)
- **Firebase** (required for app login and user data)

### Quick Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/pablojhdcoder/wizzwords-aac-platform.git
   cd wizzwords-aac-platform
   ```

2. **Install dependencies**
   ```bash
   npm run install:all
   ```

3. **Configure environment variables**
   
   Create `frontend/.env` file:
   ```env
   EXPO_PUBLIC_API_URL=http://localhost:3000
   
   # Firebase Configuration
   EXPO_PUBLIC_FIREBASE_API_KEY=your_firebase_api_key
   EXPO_PUBLIC_FIREBASE_AUTH_DOMAIN=your_project.firebaseapp.com
   EXPO_PUBLIC_FIREBASE_PROJECT_ID=your_project_id
   EXPO_PUBLIC_FIREBASE_STORAGE_BUCKET=your_project.firebasestorage.app
   EXPO_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=your_sender_id
   EXPO_PUBLIC_FIREBASE_APP_ID=your_app_id
   ```
   
   Create `backend/.env` file:
   ```env
   PORT=3000
   
   # Azure OpenAI 
   AZURE_OPENAI_PHRASE_URL=https://your-resource.cognitiveservices.azure.com/openai/responses?api-version=2025-04-01-preview
   AZURE_OPENAI_PHRASE_KEY=your_azure_key
   AZURE_OPENAI_PHRASE_DEPLOYMENT=gpt-5-mini
   
   # Azure OpenAI Image Generation (DALL-E)
   AZURE_OPENAI_IMAGE_ENDPOINT=https://your-resource.cognitiveservices.azure.com/openai/deployments/dall-e-3/images/generations?api-version=2024-02-01
   AZURE_OPENAI_IMAGE_API_KEY=your_azure_image_key
   ```

4. **Start the backend server** (Terminal 1)
   ```bash
   npm run server
   ```

5. **Start the application** (Terminal 2)
   ```bash
   npm start
   ```

### Platform Configuration

#### Android Emulator
- Set `EXPO_PUBLIC_API_URL=http://10.0.2.2:3000` in `frontend/.env`

#### iOS Simulator
-  Already configured for `http://localhost:3000`

#### Physical Device
- Set `EXPO_PUBLIC_API_URL=http://YOUR_LOCAL_IP:3000` in `frontend/.env`

---

## 🔧 API Configuration

### Azure OpenAI API (Required)

1. Go to [Azure Portal](https://portal.azure.com)
2. Navigate to your Azure OpenAI resource
3. Go to "Keys and Endpoint" section
4. Copy the complete endpoint URL and API key

**Configuration:**
```env
# Phrase Generation
AZURE_OPENAI_PHRASE_URL=https://your-resource.cognitiveservices.azure.com/openai/responses?api-version=2025-04-01-preview
AZURE_OPENAI_PHRASE_KEY=your_key
AZURE_OPENAI_PHRASE_DEPLOYMENT=gpt-5-mini

# Image Generation (DALL-E)
AZURE_OPENAI_IMAGE_ENDPOINT=https://your-resource.cognitiveservices.azure.com/openai/deployments/dall-e-3/images/generations?api-version=2024-02-01
AZURE_OPENAI_IMAGE_API_KEY=your_key
```

**Available Models:**
- `gpt-5-mini`: Cost-effective (default)
- `gpt-4o`: More powerful but expensive

### Verifying Configuration

After starting the backend, you'll see in the console:
```
API Keys configured:
   - Azure OpenAI: Yes   (or No if not configured)
```

---

## 🖼️ ARASAAC Integration

### API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/arasaac/search/:language/:searchTerm` | GET | Search pictograms |
| `/api/arasaac/pictogram/:language/:idPictogram` | GET | Get pictogram details |
| `/api/arasaac/image/:idPictogram` | GET | Get pictogram image |
| `/api/arasaac/search-multiple` | POST | Search multiple words |

### Image Customization Options

| Parameter | Type | Description |
|-----------|------|-------------|
| `color` | Boolean | Color version |
| `backgroundColor` | String | Background color  |
| `plural` | Boolean | Plural form |
| `skin` | String | Skin color variation |
| `hair` | String | Hair color variation |

**Example:**
```bash
curl "http://localhost:3000/api/arasaac/image/2?color=true&backgroundColor=white"
```



### Using in React Native

```typescript
import { getPictogramImageUrl } from './services/arasaacService';

// Basic image
const url = getPictogramImageUrl(2);

// With customization
const url = getPictogramImageUrl(2, {
  color: true,
  backgroundColor: 'white',
  plural: false
});
```

---

## 📱 Usage

### Main Flow

1. **Pictogram Selection**
   - Navigate through categories (Food, Games, School, Family, etc.)
   - Select pictograms by tapping the symbols
   - Selected symbols appear in the top bar
   - Tap a selected symbol to remove it

2. **Phrase Generation**
   - Press "Generate Phrases" to create natural phrases from the selected symbols
   - Phrases are shown as flashcards with AI-generated images
   - Tap a phrase to hear it with text-to-speech
   - Press "Generate More" to get one extra phrase variant

3. **Parent Menu**
   - Access the password-protected menu
   - Manage custom categories
   - Configure user preferences
   - Edit profile and settings

### Advanced Features

- **Custom Categories**: Create and manage your own pictogram categories
- **Customizable Themes**: Choose from 6 accessible color palettes
- **User Profile**: Customize name, email, and preferences
- **Onboarding**: Initial guide for new users

---

## 🏛️ Architecture


### Project Structure

```
AAC/
├── frontend/                 # React Native/Expo application
│   ├── screens/              # Application screens 
│   ├── components/           # Reusable components
│   ├── services/             # Services (Firebase, API, arasaac, etc.)
│   ├── context/              # Context providers (Theme, User, Toast)
│   ├── types/                # TypeScript definitions
│   └── assets/               # Images and resources
│
├── backend/                  # Node.js/Express server (TypeScript)
│   ├── index.ts              # Main server entry
│   ├── config.ts             # Centralized configuration (env, Azure, server)
│   ├── services/             # Services
│   └── data/                 # Data and categories (predefined, user_categories)
│
├── assets/                   # Project-level assets (e.g. Polimi logo)
└── README.md                 # This file
```

For a complete API reference and backend-only setup, see [backend/README.md](backend/README.md).

---

## 🛠️ Technologies

### Frontend
- **React Native** with Expo SDK 54
- **TypeScript** for type safety
- **React Navigation** for navigation
- **Firebase** for authentication and storage
- **Expo Speech** for text-to-speech
- **React Context API** for state management

### Backend
- **Node.js** (v18+)
- **Express.js** framework
- **TypeScript** for services
- **CORS** middleware

### AI Services
- **Azure OpenAI** (GPT-5-mini for phrase generation)
- **Azure OpenAI DALL-E 3** for image generation
- **ARASAAC API** for pictograms

### Storage
- **Firebase Firestore** for user profiles, preferences, and custom symbols
- **AsyncStorage** for local caching (theme, user cache, lockout state)

---

## 🧪 Development

### Available Scripts

All commands are run from the **repository root** (where the main `package.json` lives).

```bash
# Installation
npm run install:all          # Install root, frontend and backend dependencies
npm run frontend:install     # Install frontend only
npm run backend:install      # Install backend only

# Frontend
npm start                    # Start frontend (Expo)
npm run frontend:start       # Start Expo dev server
npm run frontend:android     # Run on Android
npm run frontend:ios         # Run on iOS

# Backend (from root)
npm run server               # Start backend (runs backend via backend:start)
npm run server:dev           # Start backend with auto-reload (nodemon)
```

### API Endpoints Reference

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/api/health` | GET | Health check |
| `/api/generate-phrases` | POST | Generate phrases with Azure OpenAI |
| `/api/generate-more-phrases` | POST | Generate additional phrases |
| `/api/generate-image` | POST | Generate single image with DALL-E |
| `/api/generate-images` | POST | Generate multiple images for phrases |
| `/api/arasaac/search/:language/:searchTerm` | GET | Search pictograms |
| `/api/arasaac/pictogram/:language/:idPictogram` | GET | Get pictogram details |
| `/api/arasaac/image/:idPictogram` | GET | Get pictogram image |
| `/api/arasaac/search-multiple` | POST | Search multiple words |
| `/api/categories` | GET | Get all categories (requires `?userId=`) |
| `/api/categories` | POST | Create custom category (auth) |
| `/api/categories/:categoryName` | GET | Get category pictograms (requires `?userId=`) |
| `/api/categories/empty` | POST | Create empty category (auth) |
| `/api/categories/:categoryName` | DELETE | Delete custom category (auth) |
| `/api/categories/initialize` | POST | Initialize predefined categories |
| `/api/user` | GET / PUT | Get or update user (auth) |
| `/api/user/reset` | POST | Reset user to defaults (auth) |
| `/api/user/initials` | GET | Get user initials (auth) |
| `/api/avatar` | POST | Generate user avatar |

**Auth**: endpoints marked *(auth)* require `Authorization: Bearer <userId>` or `userId` in body/query. For the full list with request/response details, see [backend/README.md](backend/README.md).

### Troubleshooting

**"API Key not configured"**
- Verify the key is in the correct `.env` file
- Restart the server after adding the key

**"Cannot connect to backend"**
- Verify backend server is running
- Check `EXPO_PUBLIC_API_URL` matches your platform

**"CORS Error"**
- Always use backend proxy endpoints for ARASAAC
- Never call external APIs directly from frontend

**Android connection issues**
- Use `http://10.0.2.2:3000` instead of `localhost`

---

## 📄 License

This project is an academic prototype developed for the **Advanced User Interfaces** course at:

<div align="center">

![Politecnico di Milano](./assets/PolimiLogo.png)

**Politecnico di Milano (Polimi)**

</div>

---

## 👥 Authors



| Author | Contact |
|--------|---------|
| Massimo Bakker | [mwgbakker@student.tudelft.nl](mailto:mwgbakker@student.tudelft.nl) |
| Pablo Jesús Herrero Díaz | [pablojesus.herrero@mail.polimi.it](mailto:pablojesus.herrero@mail.polimi.it) |
| José Martínez Estévez | [jose3.martinez@mail.polimi.it](mailto:jose3.martinez@mail.polimi.it) |