# Overview

InBack/Clickback is a Flask-based real estate platform focused on cashback services for new construction properties in the Krasnodar region. It connects buyers with developers, provides property listings, streamlines applications, and integrates CRM tools. The platform aims to offer unique cashback incentives, an intuitive user experience, smart property search with interactive maps, residential complex comparisons, user favorites, a manager dashboard for client and cashback tracking, and robust notification and document generation.

# User Preferences

Preferred communication style: Simple, everyday language.

# System Architecture

## Frontend

The frontend uses server-side rendered HTML with Jinja2 and CDN-based TailwindCSS for a mobile-first, responsive design. Interactivity is handled by modular vanilla JavaScript, including smart search, real-time filtering, Yandex Maps integration, property comparison, and PDF generation.

## Backend

Built with Flask 2.3.3, the backend follows an MVC pattern with blueprints. SQLAlchemy with PostgreSQL is used for ORM. It includes Flask-Login for session management and RBAC (Regular Users, Managers, Admins), robust security features, and custom parsing for Russian address formats. The system supports phone verification and manager-to-client presentation delivery. Data is structured around normalized Developers → Residential Complexes → Properties schemas and utilizes a Repository Pattern for data access.

## Data Storage

PostgreSQL, managed via SQLAlchemy 2.0.32, is the primary database. The schema includes Users, Managers, Properties, Residential Complexes, Developers, transactional records, and search analytics. Flask-Caching is used for performance optimization. Search history and analytics are stored to generate real-time popular searches. `residential_complexes` table includes rich content fields for descriptions, nearby amenities, ceiling height, advantages, infrastructure, and multimedia (videos).

## Authentication & Authorization

The system supports three user types (Regular Users, Managers, Admins) via a unified Flask-Login system, with dynamic user model loading.

## Intelligent Address Parsing & Search System

This system uses DaData.ru for address normalization and Yandex Maps Geocoder API for geocoding. It includes auto-enrichment for new properties, optimized batch processing, smart caching, and a Krasnodar region bias. The database schema supports granular search and filtering.

## UI/UX and Feature Specifications

Key features include:
-   **AJAX-powered Sorting and Filtering:** Property listings use an AJAX API for fast, scalable sorting and filtering, with infinite scroll and view mode persistence.
-   **Residential Complex Image Sliders:** Multi-photo sliders with fallback to property photos.
-   **Comparison System:** PostgreSQL-backed comparison for properties and complexes with strict limits.
-   **Interactive Map Pages:** Full-height Leaflet and Yandex Maps displaying residential complexes and properties with color-coded markers, grouping, server-side filtering, sticky search/filter bars, and mobile-optimized bottom sheets.
-   **Mobile Sticky Search Bar (`/residential-complexes`):** Avito-style compact sticky search bar with backdrop blur, bidirectional search input sync, and integrated controls. Features include:
  - **Sticky Positioning:** Search bar uses `sticky top-0` to remain visible during scroll, header appears above search bar
  - **Fullscreen Filter Overlay:** Filters open as fullscreen modal (z-index: 9999) with visible close button (X), sticky header, scrollable content, and apply button at bottom
  - **Working Search:** `applyComplexFilters()` checks both mobile and desktop inputs using optional chaining, enabling search from either field
  - **z-index Hierarchy:** Mobile header (50) → filters overlay (9999, covers entire page including header) → search bar (30, sticky within page flow)
  - **Breadcrumbs Hidden:** Breadcrumbs visible only on desktop (md+) to save mobile space
  - **Mobile Fullscreen Class:** `mobile-fullscreen` class applied via toggleFilters() triggers fullscreen modal with body scroll lock
  - **Advanced Filters:** Includes room type filters (студия, 1-3-4+ комнатные) and price range (млн ₽) with auto-apply on change
  - **Default Sorting:** Price ascending (дешевле first) set as default sort order
-   **Fullscreen Map Modals (Mobile):** Interactive Yandex Maps modals for both residential complexes and properties, featuring compact filter toolbars, bottom sheet quick filters, advanced filter modals, real-time filter sync, and "Список" buttons for returning to list view.
-   **Saved Search Feature:** Fullscreen mobile modal for saving property searches, including authentication checks, filter previews, and smart search name generation.
-   **Mobile-Optimized Authentication Prompts:** Uses `alert()` dialogs and automatic modal opening for authentication-required features.
-   **Smart Search with Database-Backed History:** Real-time suggestions via SuperSearch, supporting flexible query formats. Suggestions include room types, districts, developers, and residential complexes. Mobile search includes history and popular search buttons. All searches are tracked for analytics.
-   **Dynamic Results Counter:** Updates property count with proper Russian word declension.
-   **Property Alert Notification System:** Comprehensive system for saved searches with instant, daily, and weekly alerts via email and Telegram, configurable frequency, delivery channels, and one-click unsubscribe.

# External Dependencies

## Third-Party APIs

-   **SendGrid**: Email sending (currently disabled, falls back to SMTP logging).
-   **OpenAI**: Smart search and content generation.
-   **Telegram Bot API**: User notifications and communication.
-   **Yandex Maps API**: Interactive maps, geocoding, and location visualization.
-   **DaData.ru**: Address normalization, suggestions, and geocoding.
-   **SMS.ru, SMSC.ru**: Russian SMS services for phone verification.
-   **Google Analytics**: User behavior tracking.
-   **LaunchDarkly**: Feature flagging.
-   **Chaport**: Chat widget.
-   **reCAPTCHA**: Spam and bot prevention.

## Web Scraping Infrastructure

-   `selenium`, `playwright`, `beautifulsoup4`, `undetected-chromedriver`: Used for automated data collection.

## PDF Generation

-   `weasyprint`, `reportlab`: Used for generating property detail sheets, comparison reports, and cashback calculations.

## Image Processing

-   `Pillow`: Used for image resizing, compression, WebP conversion, and QR code generation.