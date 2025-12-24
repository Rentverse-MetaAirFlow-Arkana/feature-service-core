# Property Finder - Flutter Pre-Development Document

## 📋 Project Overview

**Project Name:** Property Finder Mobile App  
**Platform:** Flutter (Frontend Only)  
**Architecture:** Clean Architecture + BLoC Pattern  
**State Management:** flutter_bloc  
**Local Storage:** Hive / SharedPreferences  
**Min SDK:** Flutter 3.16+, Dart 3.2+

---

## 🎨 Design System

### Color Palette
```dart
class AppColors {
  // Primary Colors (Teal)
  static const Color primary50 = Color(0xFFE6F7F7);
  static const Color primary100 = Color(0xFFCCEFEF);
  static const Color primary200 = Color(0xFF99DFDF);
  static const Color primary300 = Color(0xFF66CFCF);
  static const Color primary400 = Color(0xFF33BFBF);
  static const Color primary500 = Color(0xFF02A8A8);  // Main Primary
  static const Color primary600 = Color(0xFF0E687F);  // Dark
  static const Color primary700 = Color(0xFF0D6980);  // Deep
  static const Color primary800 = Color(0xFF0A5266);
  static const Color primary900 = Color(0xFF073B4D);
  
  // Neutral Colors
  static const Color white = Color(0xFFFFFFFF);
  static const Color gray50 = Color(0xFFF9FAFB);
  static const Color gray100 = Color(0xFFF3F4F6);
  static const Color gray200 = Color(0xFFE5E7EB);
  static const Color gray300 = Color(0xFFD1D5DB);
  static const Color gray400 = Color(0xFF9CA3AF);
  static const Color gray500 = Color(0xFF6B7280);
  static const Color gray600 = Color(0xFF4B5563);
  static const Color gray700 = Color(0xFF374151);
  static const Color gray800 = Color(0xFF1F2937);
  static const Color gray900 = Color(0xFF111827);
  
  // Semantic Colors
  static const Color success = Color(0xFF22C55E);
  static const Color warning = Color(0xFFF59E0B);
  static const Color error = Color(0xFFEF4444);
  static const Color info = Color(0xFF3B82F6);
}
```

### Typography
```dart
class AppTypography {
  static const String fontFamily = 'Inter';
  
  // Headings
  static TextStyle h1 = TextStyle(fontSize: 32, fontWeight: FontWeight.bold);
  static TextStyle h2 = TextStyle(fontSize: 24, fontWeight: FontWeight.bold);
  static TextStyle h3 = TextStyle(fontSize: 20, fontWeight: FontWeight.w600);
  static TextStyle h4 = TextStyle(fontSize: 18, fontWeight: FontWeight.w600);
  
  // Body
  static TextStyle bodyLarge = TextStyle(fontSize: 16, fontWeight: FontWeight.normal);
  static TextStyle bodyMedium = TextStyle(fontSize: 14, fontWeight: FontWeight.normal);
  static TextStyle bodySmall = TextStyle(fontSize: 12, fontWeight: FontWeight.normal);
  
  // Labels
  static TextStyle labelLarge = TextStyle(fontSize: 14, fontWeight: FontWeight.w500);
  static TextStyle labelMedium = TextStyle(fontSize: 12, fontWeight: FontWeight.w500);
  static TextStyle labelSmall = TextStyle(fontSize: 11, fontWeight: FontWeight.w500);
}
```

---

## 📁 Directory Structure (Clean Architecture)

```
lib/
├── main.dart
├── app.dart
│
├── core/
│   ├── constants/
│   │   ├── app_constants.dart
│   │   ├── storage_keys.dart
│   │   └── api_endpoints.dart
│   │
│   ├── theme/
│   │   ├── app_colors.dart
│   │   ├── app_typography.dart
│   │   ├── app_theme.dart
│   │   └── app_spacing.dart
│   │
│   ├── utils/
│   │   ├── formatters.dart
│   │   ├── validators.dart
│   │   ├── date_utils.dart
│   │   └── extensions/
│   │       ├── string_extension.dart
│   │       ├── context_extension.dart
│   │       └── num_extension.dart
│   │
│   ├── errors/
│   │   ├── failures.dart
│   │   └── exceptions.dart
│   │
│   ├── network/
│   │   ├── api_client.dart
│   │   ├── api_interceptor.dart
│   │   └── network_info.dart
│   │
│   └── services/
│       ├── storage_service.dart
│       ├── navigation_service.dart
│       └── toast_service.dart
│
├── config/
│   ├── routes/
│   │   ├── app_routes.dart
│   │   └── route_generator.dart
│   │
│   └── injection/
│       └── injection_container.dart
│
├── features/
│   │
│   ├── auth/
│   │   ├── data/
│   │   │   ├── datasources/
│   │   │   │   └── auth_local_datasource.dart
│   │   │   ├── models/
│   │   │   │   └── user_model.dart
│   │   │   └── repositories/
│   │   │       └── auth_repository_impl.dart
│   │   │
│   │   ├── domain/
│   │   │   ├── entities/
│   │   │   │   └── user.dart
│   │   │   ├── repositories/
│   │   │   │   └── auth_repository.dart
│   │   │   └── usecases/
│   │   │       ├── login_usecase.dart
│   │   │       ├── register_usecase.dart
│   │   │       ├── logout_usecase.dart
│   │   │       └── get_current_user_usecase.dart
│   │   │
│   │   └── presentation/
│   │       ├── bloc/
│   │       │   ├── auth_bloc.dart
│   │       │   ├── auth_event.dart
│   │       │   └── auth_state.dart
│   │       ├── pages/
│   │       │   ├── login_page.dart
│   │       │   ├── register_page.dart
│   │       │   └── onboarding_page.dart
│   │       └── widgets/
│   │           ├── login_form.dart
│   │           └── register_form.dart
│   │
│   ├── property/
│   │   ├── data/
│   │   │   ├── datasources/
│   │   │   │   └── property_local_datasource.dart
│   │   │   ├── models/
│   │   │   │   ├── property_model.dart
│   │   │   │   └── property_filter_model.dart
│   │   │   └── repositories/
│   │   │       └── property_repository_impl.dart
│   │   │
│   │   ├── domain/
│   │   │   ├── entities/
│   │   │   │   ├── property.dart
│   │   │   │   └── property_filter.dart
│   │   │   ├── repositories/
│   │   │   │   └── property_repository.dart
│   │   │   └── usecases/
│   │   │       ├── get_properties_usecase.dart
│   │   │       ├── get_property_detail_usecase.dart
│   │   │       ├── search_properties_usecase.dart
│   │   │       ├── create_property_usecase.dart
│   │   │       └── get_my_properties_usecase.dart
│   │   │
│   │   └── presentation/
│   │       ├── bloc/
│   │       │   ├── property_list/
│   │       │   │   ├── property_list_bloc.dart
│   │       │   │   ├── property_list_event.dart
│   │       │   │   └── property_list_state.dart
│   │       │   ├── property_detail/
│   │       │   │   └── ...
│   │       │   └── property_create/
│   │       │       └── ...
│   │       ├── pages/
│   │       │   ├── home_page.dart
│   │       │   ├── property_detail_page.dart
│   │       │   ├── search_page.dart
│   │       │   ├── create_property_page.dart
│   │       │   └── my_properties_page.dart
│   │       └── widgets/
│   │           ├── property_card.dart
│   │           ├── property_grid.dart
│   │           ├── property_filter_sheet.dart
│   │           ├── image_carousel.dart
│   │           └── facility_chip.dart
│   │
│   ├── favorite/
│   │   ├── data/
│   │   │   ├── datasources/
│   │   │   │   └── favorite_local_datasource.dart
│   │   │   └── repositories/
│   │   │       └── favorite_repository_impl.dart
│   │   │
│   │   ├── domain/
│   │   │   ├── repositories/
│   │   │   │   └── favorite_repository.dart
│   │   │   └── usecases/
│   │   │       ├── get_favorites_usecase.dart
│   │   │       ├── add_favorite_usecase.dart
│   │   │       └── remove_favorite_usecase.dart
│   │   │
│   │   └── presentation/
│   │       ├── bloc/
│   │       │   └── favorite_bloc.dart
│   │       ├── pages/
│   │       │   └── favorites_page.dart
│   │       └── widgets/
│   │           └── favorite_button.dart
│   │
│   ├── booking/
│   │   ├── data/
│   │   │   ├── datasources/
│   │   │   │   └── booking_local_datasource.dart
│   │   │   ├── models/
│   │   │   │   ├── booking_model.dart
│   │   │   │   ├── invoice_model.dart
│   │   │   │   └── payment_model.dart
│   │   │   └── repositories/
│   │   │       └── booking_repository_impl.dart
│   │   │
│   │   ├── domain/
│   │   │   ├── entities/
│   │   │   │   ├── booking.dart
│   │   │   │   ├── invoice.dart
│   │   │   │   └── payment.dart
│   │   │   ├── repositories/
│   │   │   │   └── booking_repository.dart
│   │   │   └── usecases/
│   │   │       ├── create_booking_usecase.dart
│   │   │       ├── get_my_bookings_usecase.dart
│   │   │       ├── get_owner_bookings_usecase.dart
│   │   │       ├── approve_booking_usecase.dart
│   │   │       ├── reject_booking_usecase.dart
│   │   │       ├── process_payment_usecase.dart
│   │   │       └── get_rental_agreement_usecase.dart
│   │   │
│   │   └── presentation/
│   │       ├── bloc/
│   │       │   ├── booking_bloc.dart
│   │       │   └── owner_booking_bloc.dart
│   │       ├── pages/
│   │       │   ├── booking_page.dart
│   │       │   ├── my_bookings_page.dart
│   │       │   ├── owner_bookings_page.dart
│   │       │   └── rental_agreement_page.dart
│   │       └── widgets/
│   │           ├── booking_card.dart
│   │           ├── booking_status_badge.dart
│   │           ├── payment_method_sheet.dart
│   │           └── agreement_pdf_viewer.dart
│   │
│   ├── profile/
│   │   ├── data/
│   │   │   └── ...
│   │   ├── domain/
│   │   │   └── ...
│   │   └── presentation/
│   │       ├── bloc/
│   │       │   └── profile_bloc.dart
│   │       ├── pages/
│   │       │   └── profile_page.dart
│   │       └── widgets/
│   │           └── profile_menu_item.dart
│   │
│   ├── rating/
│   │   ├── data/
│   │   │   └── ...
│   │   ├── domain/
│   │   │   └── ...
│   │   └── presentation/
│   │       ├── bloc/
│   │       │   └── rating_bloc.dart
│   │       └── widgets/
│   │           ├── rating_stars.dart
│   │           ├── rating_input.dart
│   │           └── rating_summary.dart
│   │
│   ├── ai_prediction/
│   │   ├── data/
│   │   │   ├── datasources/
│   │   │   │   └── ai_local_datasource.dart
│   │   │   └── repositories/
│   │   │       └── ai_repository_impl.dart
│   │   │
│   │   ├── domain/
│   │   │   ├── entities/
│   │   │   │   ├── price_prediction.dart
│   │   │   │   └── listing_classification.dart
│   │   │   ├── repositories/
│   │   │   │   └── ai_repository.dart
│   │   │   └── usecases/
│   │   │       ├── predict_price_usecase.dart
│   │   │       └── classify_listing_usecase.dart
│   │   │
│   │   └── presentation/
│   │       ├── bloc/
│   │       │   └── ai_prediction_bloc.dart
│   │       └── widgets/
│   │           ├── price_prediction_card.dart
│   │           └── listing_approval_card.dart
│   │
│   └── admin/
│       ├── data/
│       │   └── ...
│       ├── domain/
│       │   └── ...
│       └── presentation/
│           ├── bloc/
│           │   └── admin_bloc.dart
│           ├── pages/
│           │   ├── admin_dashboard_page.dart
│           │   ├── admin_properties_page.dart
│           │   ├── admin_users_page.dart
│           │   ├── admin_property_types_page.dart
│           │   └── admin_amenities_page.dart
│           └── widgets/
│               ├── admin_stat_card.dart
│               ├── admin_nav_bar.dart
│               └── property_moderation_card.dart
│
└── shared/
    └── widgets/
        ├── buttons/
        │   ├── primary_button.dart
        │   ├── secondary_button.dart
        │   └── icon_button.dart
        ├── inputs/
        │   ├── text_input.dart
        │   ├── password_input.dart
        │   ├── search_input.dart
        │   └── dropdown_input.dart
        ├── cards/
        │   ├── base_card.dart
        │   └── info_card.dart
        ├── dialogs/
        │   ├── confirm_dialog.dart
        │   └── loading_dialog.dart
        ├── bottom_sheets/
        │   └── base_bottom_sheet.dart
        ├── loaders/
        │   ├── shimmer_loader.dart
        │   └── circular_loader.dart
        ├── empty_states/
        │   └── empty_state.dart
        ├── error_states/
        │   └── error_state.dart
        ├── bottom_nav/
        │   └── bottom_navigation.dart
        └── image/
            ├── cached_image.dart
            └── image_picker_widget.dart
```

---

## ✅ Feature Todo List

### Phase 1: Core Setup & Authentication
- [ ] Project initialization & dependencies
- [ ] Clean architecture folder structure
- [ ] Theme & design system setup
- [ ] Dependency injection (get_it)
- [ ] Local storage service (Hive)
- [ ] Navigation & routing setup
- [ ] Splash screen
- [ ] Onboarding screens (3 slides)
- [ ] Login page & form validation
- [ ] Register page & form validation
- [ ] Auth BLoC implementation
- [ ] Session management

### Phase 2: Property Listing
- [ ] Home page layout
- [ ] Property card widget
- [ ] Property list BLoC
- [ ] Property type filter chips
- [ ] Search page with filters
- [ ] Search BLoC with debounce
- [ ] Property detail page
- [ ] Image carousel/gallery
- [ ] Facility chips display
- [ ] Owner contact section
- [ ] Share property functionality

### Phase 3: Favorites & Rating
- [ ] Favorite button widget (animated)
- [ ] Favorites page
- [ ] Favorite BLoC
- [ ] Add/remove favorite logic
- [ ] Rating stars widget
- [ ] Rating input modal
- [ ] Submit rating functionality
- [ ] Rating summary display

### Phase 4: Create Property
- [ ] Create property form (multi-step)
- [ ] Image picker integration
- [ ] Location input
- [ ] Facility selection
- [ ] Price input with formatting
- [ ] Form validation
- [ ] Submit property BLoC
- [ ] My properties page
- [ ] Edit property functionality
- [ ] Delete property with confirmation

### Phase 5: Booking System
- [ ] Booking page with date picker
- [ ] Booking form validation
- [ ] Create booking BLoC
- [ ] My bookings page (tenant view)
- [ ] Booking card widget
- [ ] Booking status badges
- [ ] Booking detail modal
- [ ] Owner bookings page
- [ ] Approve/reject booking actions
- [ ] Invoice generation
- [ ] Payment method selection
- [ ] Payment processing (simulated)
- [ ] Rental agreement view
- [ ] PDF generation & download

### Phase 6: AI Features (Dummy)
- [ ] Price prediction service
- [ ] Price prediction card widget
- [ ] Listing classification service
- [ ] Approval prediction card
- [ ] AI loading animations

### Phase 7: Admin Panel
- [ ] Admin dashboard page
- [ ] Admin bottom navigation
- [ ] Stats cards with animations
- [ ] Property moderation page
- [ ] Approve/reject property
- [ ] User management page
- [ ] Toggle user status
- [ ] Property types management
- [ ] Amenities management
- [ ] CRUD operations for master data

### Phase 8: Polish & Optimization
- [ ] Pull to refresh
- [ ] Infinite scroll pagination
- [ ] Skeleton loaders
- [ ] Error handling & retry
- [ ] Offline indicator
- [ ] Toast notifications
- [ ] Animations & transitions
- [ ] Performance optimization
- [ ] Accessibility improvements

---

## 📦 Dependencies

```yaml
dependencies:
  flutter:
    sdk: flutter
  
  # State Management
  flutter_bloc: ^8.1.3
  equatable: ^2.0.5
  
  # Dependency Injection
  get_it: ^7.6.4
  injectable: ^2.3.2
  
  # Local Storage
  hive: ^2.2.3
  hive_flutter: ^1.1.0
  shared_preferences: ^2.2.2
  
  # Navigation
  go_router: ^12.1.3
  
  # Network (for future API integration)
  dio: ^5.4.0
  connectivity_plus: ^5.0.2
  
  # UI Components
  cached_network_image: ^3.3.0
  shimmer: ^3.0.0
  flutter_svg: ^2.0.9
  carousel_slider: ^4.2.1
  smooth_page_indicator: ^1.1.0
  
  # Forms & Validation
  flutter_form_builder: ^9.1.1
  form_builder_validators: ^9.1.0
  
  # Date & Time
  intl: ^0.18.1
  
  # Image Picker
  image_picker: ^1.0.4
  
  # PDF Generation
  pdf: ^3.10.7
  printing: ^5.11.1
  
  # Utils
  dartz: ^0.10.1
  uuid: ^4.2.1
  
  # Icons
  iconsax: ^0.0.8

dev_dependencies:
  flutter_test:
    sdk: flutter
  flutter_lints: ^3.0.1
  build_runner: ^2.4.7
  injectable_generator: ^2.4.1
  hive_generator: ^2.0.1
  bloc_test: ^9.1.5
  mocktail: ^1.0.1
```

---

## 🗄️ Data Models

### User Entity
```dart
class User {
  final String id;
  final String name;
  final String email;
  final String phone;
  final String? avatar;
  final UserRole role;
  final bool isActive;
  final DateTime createdAt;
}

enum UserRole { user, admin }
```

### Property Entity
```dart
class Property {
  final String id;
  final String title;
  final String description;
  final double price;
  final String location;
  final PropertyType type;
  final int bedrooms;
  final int bathrooms;
  final double landArea;
  final double buildingArea;
  final List<String> facilities;
  final List<String> images;
  final String ownerId;
  final String ownerName;
  final String ownerPhone;
  final double averageRating;
  final int totalRatings;
  final int viewCount;
  final int favoriteCount;
  final ListingStatus listingStatus;
  final DateTime createdAt;
}

enum PropertyType { house, villa, apartment, land, commercial }
enum ListingStatus { draft, pendingReview, approved, rejected }
```

### Booking Entity
```dart
class Booking {
  final String id;
  final String bookingCode;
  final String propertyId;
  final String propertyTitle;
  final String propertyImage;
  final String propertyLocation;
  final String tenantId;
  final String tenantName;
  final String tenantEmail;
  final String tenantPhone;
  final String landlordId;
  final String landlordName;
  final DateTime startDate;
  final DateTime endDate;
  final double rentAmount;
  final String? notes;
  final BookingStatus status;
  final String? rejectionReason;
  final Invoice? invoice;
  final Payment? payment;
  final DateTime createdAt;
}

enum BookingStatus { pending, approved, rejected, active, completed }
```

---

## 🔐 Storage Keys

```dart
class StorageKeys {
  static const String currentUser = 'current_user';
  static const String users = 'users';
  static const String properties = 'properties';
  static const String favorites = 'favorites';
  static const String bookings = 'bookings';
  static const String ratings = 'ratings';
  static const String propertyTypes = 'property_types';
  static const String amenities = 'amenities';
  static const String onboardingComplete = 'onboarding_complete';
  static const String themeMode = 'theme_mode';
}
```

---

## 🧭 Routes

```dart
class AppRoutes {
  // Auth
  static const String splash = '/';
  static const String onboarding = '/onboarding';
  static const String login = '/login';
  static const String register = '/register';
  
  // Main
  static const String home = '/home';
  static const String search = '/search';
  static const String create = '/create';
  static const String favorites = '/favorites';
  static const String profile = '/profile';
  
  // Property
  static const String propertyDetail = '/property/:id';
  static const String myProperties = '/my-properties';
  static const String editProperty = '/property/:id/edit';
  
  // Booking
  static const String booking = '/booking/:propertyId';
  static const String myBookings = '/my-bookings';
  static const String ownerBookings = '/owner-bookings';
  static const String rentalAgreement = '/agreement/:bookingId';
  
  // Admin
  static const String adminDashboard = '/admin';
  static const String adminProperties = '/admin/properties';
  static const String adminUsers = '/admin/users';
  static const String adminPropertyTypes = '/admin/property-types';
  static const String adminAmenities = '/admin/amenities';
}
```

---

## 🧪 Testing Strategy

### Unit Tests
- [ ] Use cases tests
- [ ] Repository tests
- [ ] BLoC tests
- [ ] Utility functions tests

### Widget Tests
- [ ] Form validation tests
- [ ] Button interaction tests
- [ ] Card rendering tests

### Integration Tests
- [ ] Auth flow test
- [ ] Booking flow test
- [ ] Property creation flow test

---

## 📱 Screen List

| No | Screen | Route | Description |
|----|--------|-------|-------------|
| 1 | Splash | `/` | App loading & auth check |
| 2 | Onboarding | `/onboarding` | 3-slide intro |
| 3 | Login | `/login` | Email & password login |
| 4 | Register | `/register` | User registration |
| 5 | Home | `/home` | Property listing |
| 6 | Search | `/search` | Search with filters |
| 7 | Property Detail | `/property/:id` | Full property info |
| 8 | Create Property | `/create` | Multi-step form |
| 9 | Favorites | `/favorites` | Saved properties |
| 10 | Profile | `/profile` | User settings |
| 11 | My Properties | `/my-properties` | Owner's listings |
| 12 | Booking | `/booking/:id` | Create booking |
| 13 | My Bookings | `/my-bookings` | Tenant bookings |
| 14 | Owner Bookings | `/owner-bookings` | Manage requests |
| 15 | Rental Agreement | `/agreement/:id` | PDF view |
| 16 | Admin Dashboard | `/admin` | Admin home |
| 17 | Admin Properties | `/admin/properties` | Moderation |
| 18 | Admin Users | `/admin/users` | User management |
| 19 | Admin Types | `/admin/property-types` | Master data |
| 20 | Admin Amenities | `/admin/amenities` | Master data |

---

## 🚀 Getting Started

```bash
# Create project
flutter create property_finder --org com.yourcompany

# Navigate to project
cd property_finder

# Get dependencies
flutter pub get

# Generate files (Hive, Injectable)
flutter pub run build_runner build --delete-conflicting-outputs

# Run app
flutter run
```

---

## 📝 Development Guidelines

### Code Style
- Follow official Dart style guide
- Use meaningful variable/function names
- Add documentation comments for public APIs
- Keep functions small and focused

### Git Workflow
- Feature branches: `feature/feature-name`
- Bug fixes: `fix/bug-description`
- Commit messages: `type: description`
  - `feat:` new feature
  - `fix:` bug fix
  - `refactor:` code refactoring
  - `docs:` documentation
  - `style:` formatting
  - `test:` adding tests

### BLoC Guidelines
- One BLoC per feature/page
- Keep events simple and descriptive
- States should be immutable
- Use `Equatable` for state comparison

---

## 👥 Demo Accounts

| Role | Email | Password |
|------|-------|----------|
| Admin | admin@propertyfinder.com | admin123 |
| User | user@demo.com | demo1234 |

---

## 📄 License

MIT License - Property Finder Mobile App
