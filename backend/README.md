# Backend – AAC App

Backend proxy and API server for the WizzWords AAC app. It sits between the React Native frontend and external services (Azure OpenAI, ARASAAC), handling phrase generation, image generation, pictograms, user data, and categories while avoiding CORS issues.

**For an overview of the project and the app**, see the [main README](../README.md).

## 🚀 Installation

**Requirements:** Node.js v18+ and npm (or yarn).

**From repository root:**
```bash
npm run install:all
# or backend only:
npm run backend:install
```

**From this directory (`backend/`):**
```bash
npm install
```

## ⚙️ Configuration

Create or edit `backend/.env` with:

```env
PORT=3000

# Azure OpenAI – phrase generation (required for AI phrases)
AZURE_OPENAI_PHRASE_URL=https://your-resource.cognitiveservices.azure.com/openai/responses?api-version=2025-04-01-preview
AZURE_OPENAI_PHRASE_KEY=your_azure_key
AZURE_OPENAI_PHRASE_DEPLOYMENT=gpt-5-mini

# Azure OpenAI – image generation (optional, for /api/generate-image and /api/generate-images)
AZURE_OPENAI_IMAGE_ENDPOINT=https://your-resource.cognitiveservices.azure.com/openai/deployments/dall-e-3/images/generations?api-version=2024-02-01
AZURE_OPENAI_IMAGE_API_KEY=your_azure_image_key
```

## ▶️ Execution

**From repository root** (recommended when working on the full app):
```bash
npm run server          # start backend
npm run server:dev      # start with auto-reload (nodemon)
```

**From this directory (`backend/`):**
```bash
npm start               # start server (ts-node index.ts)
npm run dev             # start with auto-reload (nodemon)
```

The server listens on `http://localhost:3000` (and `0.0.0.0` so emulators/devices can reach it).

## 📡 Endpoints

**Auth:** Endpoints marked *(auth)* require `userId` via `Authorization: Bearer <userId>` header, or `userId` in body/query when applicable.

### Frases (Azure OpenAI)
| Method | Path | Body | Description |
|--------|------|------|-------------|
| POST | `/api/generate-phrases` | `{ words: string[], childAge?: number }` | Generate natural phrases from words |
| POST | `/api/generate-more-phrases` | `{ words: string[], existingPhrases: string[], childAge?: number }` | Generate more phrases without repeating |

### Imágenes (DALL-E)
| Method | Path | Body | Description |
|--------|------|------|-------------|
| POST | `/api/generate-image` | `{ phrase: string }` | Generate one image for a phrase (returns `imageBase64`) |
| POST | `/api/generate-images` | `{ phrases: string[] }` | Generate images for multiple phrases (returns `{ images, total, successful }`) |

### ARASAAC (Pictogramas)
| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/arasaac/search/:language/:searchTerm` | Search pictograms by term |
| GET | `/api/arasaac/pictogram/:language/:idPictogram` | Get pictogram metadata by ID |
| GET | `/api/arasaac/image/:idPictogram` | Get pictogram image (query: `color`, `backgroundColor`, `plural`, `skin`, `hair`, `action`) |
| POST | `/api/arasaac/search-multiple` | Body: `{ words: string[], language?: string }` — search multiple words |

### Usuario y perfil
| Method | Path | Auth | Body/Query | Description |
|--------|------|------|------------|-------------|
| GET | `/api/user` | ✓ | — | Get user data |
| PUT | `/api/user` | ✓ | `{ email?, fullName?, preferences? }` | Update user |
| POST | `/api/user/reset` | ✓ | — | Reset user to defaults |
| GET | `/api/user/initials` | ✓ | — | Get user initials |
| GET | `/api/user/cache-stats` | — | — | Cache stats (debug) |
| POST | `/api/avatar` | — | `{ userId?, email?, fullName? }` | Generate avatar (returns `avatarUrl`, `seed`) |

### Categorías
| Method | Path | Auth | Query/Body | Description |
|--------|------|------|------------|-------------|
| GET | `/api/categories` | — | `?userId=` | Get all categories for user |
| GET | `/api/categories/:categoryName` | — | `?userId=` | Get pictogram IDs for a category |
| POST | `/api/categories` | ✓ | `{ categoryName, maxResults?, description? }` | Create custom category (AI-filled) |
| POST | `/api/categories/empty` | ✓ | `{ categoryName }` | Create empty custom category |
| DELETE | `/api/categories/:categoryName` | ✓ | — | Delete custom category (not predefined) |
| POST | `/api/categories/initialize` | — | — | Initialize predefined categories |

### Sistema
| Method | Path | Description |
|--------|------|-------------|
| GET | `/api/health` | Health check (`status`, `hasAzureOpenAI`) |
| GET | `/` | Server info and list of endpoints |

## 🔧 Configuration for Emulators

### Android Emulator
The frontend should use: `http://10.0.2.2:3000` (10.0.2.2 is the alias for localhost on Android)

### iOS Simulator
The frontend should use: `http://localhost:3000`

### Web Browser
The frontend should use: `http://localhost:3000`

### Physical Device
The frontend should use: `http://YOUR_LOCAL_IP:3000` (e.g., `http://192.168.1.100:3000`)



## 📖 Usage Examples

### Search pictograms

```bash
# Search pictograms for "casa" in Spanish
curl http://localhost:3000/api/arasaac/search/es/casa

# Search pictograms for "house" in English
curl http://localhost:3000/api/arasaac/search/en/house
```

### Get a specific pictogram

```bash
# Get pictogram with ID 2 in Spanish
curl http://localhost:3000/api/arasaac/pictogram/es/2
```

### Search multiple words

```bash
curl -X POST http://localhost:3000/api/arasaac/search-multiple \
  -H "Content-Type: application/json" \
  -d '{"words": ["casa", "perro", "comer"], "language": "es"}'
```

### Generate phrases with Azure OpenAI

```bash
curl -X POST http://localhost:3000/api/generate-phrases \
  -H "Content-Type: application/json" \
  -d '{"words": ["I", "want", "pizza"]}'
```
