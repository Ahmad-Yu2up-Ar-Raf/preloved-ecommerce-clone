# 🏷️ Preloved Clone App

A frontend clone of the [Preloved](https://preloved.co.id) app — a trusted Indonesian marketplace for buying and selling secondhand fashion items.

> **Note:** This is a **learning project**. Only the frontend (UI/UX) has been cloned. There is no backend, authentication, or real transactions involved. Product data comes from the [Platzi Fake Store API](https://api.escuelajs.co/api/v1).

---

## 📱 Preview

<div align="center">

|                                                        |                           Light Mode                           |                                                      |
| :----------------------------------------------------: | :------------------------------------------------------------: | :--------------------------------------------------: |
| ![Home](./assets/images/mockup/ligth/beranda-1.png) | ![Products](./assets/images/mockup/ligth/products-explore-1.png) | ![Detail](./assets/images/mockup/ligth/detail-1.png) |

|                                                       |                      Dark Mode                       |                                                |
| :---------------------------------------------------: | :--------------------------------------------------: | :--------------------------------------------: |
| ![Explore](./assets/images/mockup/dark/keranjang.png) | ![Filter](./assets/images/mockup/dark/profile-1.png) | ![Dark](./assets/images/mockup/dark/jual.png) |

</div>

---

## ✨ Features

- **Home** — Product carousels (For You, Your Likes, Recently Viewed), seller recommendations, and a hot-items grid
- **Infinite Scroll** — Automatically loads more products as the user scrolls down
- **Filter Carousel** — Product filters with hide/show animation on scroll
- **Multi-Image Card** — Each product supports multiple images with a swipeable carousel
- **Wishlist Button** — Lottie animation (lazy-mounted for optimal performance)
- **Pull-to-Refresh** — Refreshes all data at once
- **Dark / Light Mode** — Automatic theme support via NativeWind
- **Image Fallback** — Automatically handles broken images from the Platzi API

---

## 🛠️ Tech Stack

| Category      | Library                       |
| ------------- | ------------------------------ |
| Framework     | React Native + Expo SDK 52     |
| Routing       | Expo Router v4                 |
| Server State  | TanStack Query v5               |
| Animation     | React Native Reanimated v3     |
| Styling       | NativeWind v4 (Tailwind CSS)    |
| Image         | expo-image                     |
| Lottie        | lottie-react-native             |
| Language      | TypeScript                      |
| Data Source   | Platzi Fake Store API          |

---

## 📁 Project Structure

```
preloved-clone/
├── app/
│   ├── (tabs)/
│   │   ├── index.tsx          # Home screen
│   │   ├── explore.tsx        # Explore screen
│   │   └── products.tsx       # Products screen + filters
│   ├── product/[id]/
│   │   └── index.tsx          # Product detail
│   └── _layout.tsx            # Root layout + providers
│
├── components/
│   ├── ui/core/block/
│   │   ├── beranda-block.tsx  # Main home block
│   │   └── explore-products.tsx
│   └── ui/fragments/
│       ├── custom/card/product-card.tsx
│       ├── custom/carousel/
│       └── shadcn-ui/image.tsx
│
├── lib/server/products/
│   ├── products-server.ts         # Fetch calls to the Platzi API
│   ├── products-queris-server.ts  # TanStack Query options
│   └── product-mappers.ts         # Data transformation
│
└── type/
    └── products-type.ts           # TypeScript types
```

---

## 🚀 Getting Started

### Prerequisites

- Node.js 18+
- Expo CLI: `npm install -g expo-cli`
- iOS Simulator / Android Emulator **or** Expo Go on a physical device

### Installation

```bash
# Clone the repo
git clone https://github.com/username/preloved-clone.git
cd preloved-clone

# Install dependencies
npm install

# Run
npx expo start
```

### Run on a device/simulator

```bash
# iOS
npx expo run:ios

# Android
npx expo run:android
```

---

## 🔌 API

This project uses the [Platzi Fake Store API](https://api.escuelajs.co/api/v1) as its dummy data source.

| Endpoint                       | Description                                       |
| ------------------------------- | -------------------------------------------------- |
| `GET /products`                | Fetch product list (with filtering & pagination)   |
| `GET /products/:id`            | Get a single product's detail                       |
| `GET /categories`              | List categories                                     |
| `GET /categories/:id/products` | Products within a category                          |

---

## ⚙️ TanStack Query Configuration

The provider is configured in `app/_layout.tsx` with:

```ts
const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: 60 * 1000, // Data stays fresh for 1 minute
      gcTime: 5 * 60 * 1000, // Cache retained for 5 minutes
      retry: 1, // Retry once on failure
    },
  },
});
```

**AppState integration** uses `focusManager` (instead of `refetchQueries`) to prevent infinite scroll from being cancelled when the app returns to the foreground:

```ts
AppState.addEventListener('change', (status) => {
  focusManager.setFocused(status === 'active');
});
```

---

## 🐛 Bug Fixes & Optimizations

Technical issues found and resolved during development:

| Issue                                       | Fix                                                        |
| -------------------------------------------- | ----------------------------------------------------------- |
| Header hidden behind carousel while scrolling | Header rendered as an absolute `View` (instead of `Stack.Screen`) |
| Infinite scroll delay / required an extra scroll to trigger | `onMomentumScrollEnd` + `onScrollEndDrag` handlers          |
| List footer spinner stuck / not updating     | `useRef` pattern + empty-deps `useCallback` + `extraData`   |
| Cache collision between Home and Explore     | `MAX_ITEMS` included in the query key                        |
| Heavy app lag with many products             | `React.memo` on `ProductCard` + lazy-mounted `LottieView`   |
| `removeClippedSubviews` Android bug          | Enabled on iOS only                                          |
| `queryClient.refetchQueries()` deadlock      | Replaced with `focusManager.setFocused()`                   |

---

## 📖 References

- [Preloved App — App Store](https://apps.apple.com/id/app/preloved-buy-sell-fashion/id6502086431)
- [preloved.co.id](https://preloved.co.id)
- [Platzi Fake Store API](https://api.escuelajs.co/api/v1)
- [TanStack Query — React Native](https://tanstack.com/query/latest/docs/react/react-native)
- [Expo Router Docs](https://docs.expo.dev/router/introduction/)
- [React Native Reanimated](https://docs.swmansion.com/react-native-reanimated/)

---

## 📄 License

This project was built for educational purposes. All visual assets and UI concepts reference the Preloved app, which belongs to the Preloved Indonesia team.

---

<div align="center">
  <p>Made with ❤️ as a learning project</p>
  <p>Data by <a href="https://api.escuelajs.co/api/v1">Platzi Fake Store API</a></p>
</div>

---

## 📝 Translator's notes

A few small things worth polishing beyond the translation:

- The "Struktur Proyek" tree mixes `beranda-block.tsx` (Indonesian for "home") with otherwise English file names — consider renaming to `home-block.tsx` for consistency now that the docs are in English.
- The bug-fix table is genuinely useful — you could turn it into a running "Lessons Learned" doc that grows with future PRs, since it's the most technically interesting section here.
- A short "Known Limitations" section (e.g. no persisted cart, no real checkout) would set expectations clearly for anyone browsing the repo.
