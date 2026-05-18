# 🌍 Conheça o Mundo

Aplicativo mobile em React Native (Expo) para explorar países do mundo, gerenciar favoritos e personalizar seu perfil. Desenvolvido como projeto integrador do curso de Desenvolvimento Mobile do IFPE — Campus Jaboatão.

[![React Native](https://img.shields.io/badge/React_Native-0.81-61DAFB?logo=react&logoColor=white)](https://reactnative.dev/)
[![Expo](https://img.shields.io/badge/Expo-SDK_54-000020?logo=expo&logoColor=white)](https://expo.dev/)
[![Firebase](https://img.shields.io/badge/Firebase-Auth_+_Firestore-FFCA28?logo=firebase&logoColor=black)](https://firebase.google.com/)
[![Cloudinary](https://img.shields.io/badge/Cloudinary-Upload-3448C5?logo=cloudinary&logoColor=white)](https://cloudinary.com/)
[![License](https://img.shields.io/badge/license-Academic-blue)]()

---

## 📋 Sobre o Projeto

App mobile que permite ao usuário criar conta, explorar informações detalhadas sobre os países do mundo, favoritar destinos e personalizar seu perfil com foto. Construído consumindo múltiplas APIs externas e aplicando boas práticas de UX mobile.

### 🎯 Funcionalidades

- 🔐 **Autenticação** completa (cadastro, login, logout) via Firebase Auth
- 🌎 **Lista de 250+ países** com bandeiras e busca em tempo real
- 📊 **Detalhes do país** (capital, população, idioma, moeda, região, etc) em pt-BR
- ❤️ **Sistema de favoritos** com sincronização entre Firestore e cache local
- 👤 **Perfil personalizado** com avatar, estatísticas e configurações
- 📷 **Upload de foto de perfil** via galeria ou câmera com upload pro Cloudinary
- 💾 **Cache offline** com AsyncStorage (UX rápida + economia de rede)
- 🌐 **Tradução para português** dos nomes de países

---

## 🛠️ Stack Tecnológica

### Core
- **React Native** 0.81 + **Expo SDK 54** — framework e plataforma
- **React Navigation 7** — navegação (Native Stack + Bottom Tabs)
- **Axios** — cliente HTTP
- **AsyncStorage** — persistência local

### Backend / Serviços
- **Firebase Authentication** — auth via REST API (compatível com Expo Go)
- **Cloud Firestore** — banco de dados (users + favorites) via REST API
- **Cloudinary** — armazenamento e CDN de imagens (unsigned upload)
- **RestCountries v3.1** — dados dos países

### Ferramentas
- **expo-image-picker** — acesso à galeria e câmera
- **@expo/vector-icons** (Ionicons) — biblioteca de ícones
- **react-native-safe-area-context** — gerenciamento de safe areas

---


## 🚀 Como Rodar Localmente

### Pré-requisitos

- **Node.js** 18+ ([download](https://nodejs.org))
- **npm** ou **yarn**
- **Expo Go** instalado no celular ([iOS](https://apps.apple.com/app/expo-go/id982107779) / [Android](https://play.google.com/store/apps/details?id=host.exp.exponent))
- Conta no [Firebase](https://console.firebase.google.com)
- Conta no [Cloudinary](https://cloudinary.com)

### 1. Clonar o repositório

```bash
git clone https://github.com/DanilloFelipe06/conheca-o-mundo.git
cd conheca-o-mundo
```

### 2. Instalar dependências

```bash
npm install --legacy-peer-deps
```

### 3. Configurar Firebase

1. Crie um projeto no [Firebase Console](https://console.firebase.google.com)
2. Habilite **Authentication** → método **Email/Password**
3. Crie um **Firestore Database** (modo teste ou com as regras abaixo)
4. Registre um **Web App** e copie as credenciais
5. Configure as **regras de segurança** do Firestore:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
    match /favorites/{favoriteId} {
      allow read, create: if request.auth != null;
      allow update, delete: if request.auth != null && resource.data.uid == request.auth.uid;
    }
  }
}
```

### 4. Configurar Cloudinary

1. Crie conta no [Cloudinary](https://cloudinary.com)
2. Vá em **Settings → Upload → Upload presets**
3. Crie um preset com **Signing Mode: Unsigned**
4. Anote o **Cloud Name** e o nome do preset

### 5. Configurar variáveis de ambiente

Copie o template:

```bash
cp .env.example .env
```

Edite `.env` com suas credenciais:

```bash
# Firebase
EXPO_PUBLIC_FIREBASE_API_KEY=sua-api-key
EXPO_PUBLIC_FIREBASE_AUTH_DOMAIN=seu-projeto.firebaseapp.com
EXPO_PUBLIC_FIREBASE_PROJECT_ID=seu-projeto-id
EXPO_PUBLIC_FIREBASE_STORAGE_BUCKET=seu-projeto.firebasestorage.app
EXPO_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=seu-sender-id
EXPO_PUBLIC_FIREBASE_APP_ID=seu-app-id

# Cloudinary
EXPO_PUBLIC_CLOUDINARY_CLOUD_NAME=seu-cloud-name
EXPO_PUBLIC_CLOUDINARY_UPLOAD_PRESET=seu-preset-unsigned

# APIs (não precisa mudar)
EXPO_PUBLIC_RESTCOUNTRIES_BASE_URL=https://restcountries.com/v3.1
EXPO_PUBLIC_FIREBASE_AUTH_BASE_URL=https://identitytoolkit.googleapis.com/v1
EXPO_PUBLIC_FIRESTORE_BASE_URL=https://firestore.googleapis.com/v1
```

### 6. Rodar o projeto

```bash
npx expo start --clear
```

Escaneie o QR Code com o **Expo Go** no seu celular. Certifique-se de que o celular está na **mesma rede WiFi** que o computador.

---

## 📁 Estrutura do Projeto

```
conheca-o-mundo/
├── App.js                        # Entry point + providers
├── .env                          # Variáveis de ambiente (não commitado)
├── .env.example                  # Template de variáveis
├── package.json
├── src/
│   ├── components/               # Componentes reutilizáveis
│   │   ├── Button.js
│   │   ├── Input.js
│   │   ├── CountryCard.js
│   │   └── InfoRow.js
│   │
│   ├── contexts/                 # Estado global
│   │   └── AuthContext.js
│   │
│   ├── hooks/                    # Custom hooks
│   │   └── useFavorite.js
│   │
│   ├── navigation/               # Navegação
│   │   ├── RootNavigator.js
│   │   ├── AuthStack.js
│   │   ├── AppNavigator.js
│   │   ├── HomeStack.js
│   │   └── ProfileStack.js
│   │
│   ├── screens/                  # Telas do app
│   │   ├── LoginScreen.js
│   │   ├── RegisterScreen.js
│   │   ├── HomeScreen.js
│   │   ├── CountryDetailsScreen.js
│   │   ├── FavoritesScreen.js
│   │   ├── ProfileScreen.js
│   │   └── ChangePhotoScreen.js
│   │
│   ├── services/                 # Camada de integração externa
│   │   ├── api.js                # Instâncias Axios
│   │   ├── firebaseConfig.js     # Config Firebase
│   │   ├── authService.js        # Firebase Auth REST
│   │   ├── firestoreService.js   # Firestore REST (CRUD)
│   │   ├── countriesService.js   # RestCountries + cache
│   │   └── cloudinaryService.js  # Upload de imagens
│   │
│   └── utils/                    # Helpers e constantes
│       ├── colors.js             # Paleta de cores
│       └── constants.js          # URLs, chaves AsyncStorage
```

---

## 🔌 APIs Utilizadas

### 1. RestCountries v3.1

API pública e gratuita com dados de todos os países.

```
GET https://restcountries.com/v3.1/all?fields=name,cca3,capital,flags,...
GET https://restcountries.com/v3.1/alpha/{code}
```

Os nomes são traduzidos para português via campo `translations.por`. A resposta completa é cacheada localmente por 24h.

### 2. Firebase Authentication (REST)

Autenticação via REST API (não usa o SDK do Firebase, que tem incompatibilidade com Expo Go).

```
POST /accounts:signUp
POST /accounts:signInWithPassword
POST /token (refresh)
```

### 3. Firestore (REST)

Banco de dados NoSQL acessado via REST API.

**Coleções:**
- `users/{uid}` — perfil do usuário (nome, email, photoURL, stats)
- `favorites/{auto-id}` — países favoritados (uid, countryCode, countryName, capital, flag, createdAt)

### 4. Cloudinary Upload (Unsigned)

Upload direto do cliente (sem backend) via preset Unsigned.

```
POST https://api.cloudinary.com/v1_1/{cloud_name}/image/upload
```

---

## 🏗️ Conceitos Aplicados

### React
- ✅ **Hooks** (`useState`, `useEffect`, `useCallback`, `useMemo`)
- ✅ **Context API** para estado global de autenticação
- ✅ **Custom Hooks** (`useAuth`, `useFavorite`)
- ✅ **Composição de componentes** (CountryCard com `rightSlot`)

### React Native / Expo
- ✅ **Navegação** com React Navigation v7 (Stack + Tabs aninhados)
- ✅ **SafeAreaView** com edges customizados
- ✅ **KeyboardAvoidingView** para forms
- ✅ **FlatList** com performance tuning (windowSize, removeClippedSubviews)
- ✅ **Pull-to-refresh** com `RefreshControl`
- ✅ **Permissões** de câmera e galeria

### Boas práticas
- ✅ **Estado otimista** com rollback em caso de erro
- ✅ **Debounce** em busca textual (300ms)
- ✅ **Cache local + sync remoto** (AsyncStorage + Firestore)
- ✅ **Validação client-side** antes de requests
- ✅ **Tratamento de erros** com mensagens amigáveis em pt-BR
- ✅ **Environment variables** via Expo (prefixo `EXPO_PUBLIC_`)
- ✅ **Loading states** em todas as operações assíncronas
- ✅ **Empty states** com CTAs

---

## 📚 O que aprendi com esse projeto

- Integração de múltiplas APIs externas com Axios e tratamento de erros
- Autenticação completa com Firebase Auth via REST (workaround para Expo Go)
- Modelagem de dados no Firestore com regras de segurança baseadas em UID
- Upload de arquivos sem backend usando Cloudinary unsigned presets
- Gerenciamento de estado global com Context API
- Navegação complexa com stacks aninhados em bottom tabs
- Otimização de performance em listas grandes
- UX de aplicativos mobile (loading, empty, error states)

---

## 👤 Autor

**Danillo Felipe Barboza**

- 🎓 Estudante de Desenvolvimento Mobile — IFPE Campus Jaboatão
- 💻 GitHub: [@DanilloFelipe06](https://github.com/DanilloFelipe06)

---

## 📄 Licença

Projeto acadêmico desenvolvido para o curso de Desenvolvimento Mobile do IFPE.
