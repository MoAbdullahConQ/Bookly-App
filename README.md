# Bookly App

![Platform](https://img.shields.io/badge/platform-Flutter-02569B?logo=flutter&logoColor=white)
![Language](https://img.shields.io/badge/language-Dart-0175C2?logo=dart&logoColor=white)
![Architecture](https://img.shields.io/badge/architecture-Clean%20Architecture-orange)
![State Management](https://img.shields.io/badge/state%20management-BLoC%2FCubit-blueviolet)
![License](https://img.shields.io/badge/license-MIT-green)

A book discovery app built with Flutter following **Clean Architecture**. Bookly lets users browse featured and newest book releases, search the Google Books catalog, and view detailed book info — with offline caching so previously loaded books are available without a network call.

## Demo

<a href="https://youtube.com/shorts/C5MHfU3Lmqw">
  <img src="https://img.youtube.com/vi/C5MHfU3Lmqw/maxresdefault.jpg" width="600">
</a>

*Click the thumbnail above to watch the app in action or click here* ▶️ [Watch the demo](https://youtube.com/shorts/C5MHfU3Lmqw)

## Features

- **Home feed** with two book sections: Featured Books and Newest Books, each in its own scrollable list
- **Book search** against the Google Books API
- **Book details view** with cover image, rating, description, and similar-books suggestions
- **Buy / Free Preview action** that opens the book's preview link via `url_launcher`
- **Offline-first caching** with Hive — fetched books are cached locally and served from cache before hitting the network again
- **Animated splash screen**
- Loading (shimmer-style) placeholders for lists while data is fetched
- Dark theme UI with Google Fonts (Montserrat)

## Architecture

The project is structured using **Clean Architecture**, split into three layers per feature (`home` and `search`):

- **`domain`** — business logic, framework-independent: `entities`, abstract `repos`, and `use_cases`
- **`data`** — implementation details: `data_sources` (remote via Dio + Google Books API, and local via Hive), `models`, and repo implementations
- **`presentation`** — UI layer: `views`, `widgets`, and Cubits (`FeaturedBooksCubit`, `NewestBooksCubit`, `SearchBooksCubit`) that consume use cases

Cross-cutting concerns live under `core/`: error handling (`Failure` / `Either` via `dartz`), the `ApiService` (Dio wrapper), routing (`go_router`), and the `GetIt` service locator (`setup_service_locator.dart`) that wires dependencies for each feature.

## Tech Stack

| | |
|---|---|
| **Framework** | Flutter |
| **Language** | Dart (SDK ^2.17.6) |
| **Architecture** | Clean Architecture (data / domain / presentation) |
| **State Management** | `flutter_bloc` (Cubit) |
| **Networking** | `dio` |
| **API** | [Google Books API](https://developers.google.com/books) |
| **Local Caching** | `hive` / `hive_flutter` |
| **Dependency Injection** | `get_it` |
| **Routing** | `go_router` |
| **Functional Error Handling** | `dartz` (`Either<Failure, T>`) |
| **Environment Config** | `flutter_dotenv` |
| **Fonts / Icons** | `google_fonts`, `font_awesome_flutter` |
| **Other** | `cached_network_image`, `url_launcher` |

## Project Structure

```
Bookly-Clean-Arch/
├── lib/
│   ├── main.dart                       # App entry: Hive/dotenv init, DI setup, routing
│   ├── constants.dart                  # Hive box names, shared constants
│   ├── core/
│   │   ├── errors/failure.dart         # Failure types for error handling
│   │   ├── use_cases/use_case.dart     # Base use case contract
│   │   ├── utils/                      # ApiService, AppRouter, service locator, styles
│   │   └── widgets/                    # Shared reusable widgets
│   └── Features/
│       ├── Splash/presentation/        # Animated splash screen
│       ├── home/
│       │   ├── data/                   # Remote/local data sources, models, repo impl
│       │   ├── domain/                 # Entities, repo contracts, use cases
│       │   └── presentation/           # Cubits, views, widgets
│       └── search/
│           ├── data/
│           ├── domain/
│           └── presentation/
├── assets/
│   ├── fonts/
│   └── images/
└── pubspec.yaml
```

## Getting Started

### Prerequisites

- [Flutter SDK](https://docs.flutter.dev/get-started/install)
- Android Studio / VS Code with the Flutter extension
- A [Google Books API key](https://console.cloud.google.com/apis/library/books.googleapis.com)

### Setup

```bash
git clone https://github.com/MoAbdullahConQ/Bookly-Clean-Arch.git
cd Bookly-Clean-Arch
flutter pub get
```

Create a `.env` file in the project root with your API key:

```
API_KEY=your_google_books_api_key
```

Generate the Hive type adapter (already committed, but regenerate if the entity changes):

```bash
dart run build_runner build --delete-conflicting-outputs
```

Run the app:

```bash
flutter run
```

## How It Works

On launch, the app loads environment variables, initializes Hive, registers the `BookEntity` adapter, opens the featured/newest Hive boxes, and sets up the `GetIt` service locator. The home Cubits (`FeaturedBooksCubit`, `NewestBooksCubit`) call their respective use cases, which check the local Hive cache first and only fall back to the Google Books API (via `ApiService`/Dio) if no cached data is available — with fetched results saved back to Hive for next time. `SearchBooksCubit` queries the API directly for on-demand search. Tapping a book navigates (via `go_router`) to a details view showing its cover, rating, description, similar titles, and a preview/purchase link.

## Author

**Muhammed Abdullah**
- GitHub: [@MoAbdullahConQ](https://github.com/MoAbdullahConQ)
- LinkedIn: [muhammed-bn-abdullah](https://linkedin.com/in/muhammed-bn-abdullah)
- Email: muhammed.abdullah.01155849512@gmail.com

## License

This project is licensed under the [MIT License](LICENSE).
