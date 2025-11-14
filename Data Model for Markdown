Here is a **clean, GitHub-ready data model in Markdown**, covering **entities, schemas, relationships, and ERD-style descriptions** for the vertical shorts / microdrama platform.

If you'd like JSON schemas, SQL schema files, or a diagram in Mermaid, I can generate those too.

---

```markdown
# Data Model (Detailed)

This document defines the core data model for the Vertical Stories Platform, including tables, fields, relationships, and constraints.

---

# 1. Entity Overview

Core entities:

1. User  
2. UserProfile  
3. ContentSeries  
4. Episode  
5. EpisodeVariant (A/B testing)  
6. EpisodeAsset (video, thumbnail, subtitles)  
7. Genre  
8. SeriesGenreMap  
9. UserEpisodeProgress  
10. PaymentTransaction  
11. Wallet  
12. TokenPurchasePackage  
13. Subscription  
14. Creator (Phase 2)  
15. CreatorContentSubmission (Phase 2)  
16. RecommendationEvent  
17. Notification  
18. AnalyticsEvent (generic event tracking)

---

# 2. Schema Definitions

## 2.1 User
```

Table: users

* id (PK, UUID)
* email (unique)
* password_hash
* auth_provider (native, google, apple)
* created_at (timestamp)
* updated_at (timestamp)
* status (active, banned, deleted)

```

## 2.2 UserProfile
```

Table: user_profiles

* id (PK, UUID)
* user_id (FK → users.id)
* username
* avatar_url
* preferred_genres (array of genre_ids)
* language_preference
* region
* created_at
* updated_at

```

---

## 2.3 ContentSeries
Represents a microdrama series.

```

Table: content_series

* id (PK, UUID)
* title
* synopsis
* description
* primary_genre_id (FK → genres.id)
* created_at
* updated_at
* release_status (draft, scheduled, published, archived)
* trailer_asset_id (FK → episode_assets.id, nullable)
* is_premium (boolean)
* studio_label (optional string)

```

---

## 2.4 Episode
Canonical episode entity (not tied to A/B variants).

```

Table: episodes

* id (PK, UUID)
* series_id (FK → content_series.id)
* episode_number (int)
* title
* synopsis
* duration_seconds (int)
* release_date
* created_at
* updated_at
* base_price_tokens (int)
* is_free (boolean)

```

---

## 2.5 EpisodeVariant (A/B test versions of an episode)
Each episode can have multiple variants.

```

Table: episode_variants

* id (PK, UUID)
* episode_id (FK → episodes.id)
* variant_label (e.g., "A", "B", "C")
* is_winner (boolean)
* created_at
* updated_at
* experiment_group (string)

```

---

## 2.6 EpisodeAsset (video, thumbnails, captions)
Stores all media assets.

```

Table: episode_assets

* id (PK, UUID)
* variant_id (FK → episode_variants.id)
* asset_type (video, thumbnail, subtitle)
* file_url
* file_format
* resolution (1080x1920, etc.)
* created_at

```

---

## 2.7 Genre
```

Table: genres

* id (PK, UUID)
* name (string)
* description
* created_at

```

---

## 2.8 SeriesGenreMap (many-to-many mapping)
```

Table: series_genre_map

* id (PK, UUID)
* series_id (FK → content_series.id)
* genre_id (FK → genres.id)

```

---

## 2.9 UserEpisodeProgress
Tracks progress, completion, and unlock state.

```

Table: user_episode_progress

* id (PK, UUID)
* user_id (FK → users.id)
* episode_id (FK → episodes.id)
* variant_id (FK → episode_variants.id, nullable)
* is_unlocked (boolean)
* is_completed (boolean)
* last_position_seconds (int)
* updated_at

```

---

## 2.10 Wallet
Holds token counts for each user.

```

Table: wallets

* id (PK, UUID)
* user_id (FK → users.id)
* balance_tokens (int)
* updated_at

```

---

## 2.11 TokenPurchasePackage
Predefined purchase bundles.

```

Table: token_purchase_packages

* id (PK, UUID)
* name
* token_amount
* price_usd
* created_at

```

---

## 2.12 PaymentTransaction
Stores purchases, subscriptions, and episode unlocks.

```

Table: payment_transactions

* id (PK, UUID)
* user_id (FK → users.id)
* transaction_type (token_purchase, subscription, episode_unlock)
* package_id (FK → token_purchase_packages.id, nullable)
* episode_id (FK → episodes.id, nullable)
* amount_usd (decimal)
* tokens_used (int)
* currency (string)
* status (success, failed, pending)
* created_at

```

---

## 2.13 Subscription
```

Table: subscriptions

* id (PK, UUID)
* user_id (FK → users.id)
* plan_type (monthly, annual, genre_pass)
* start_date
* end_date
* auto_renew (boolean)
* status (active, expired, canceled)

```

---

## 2.14 Creator (Phase 2)
```

Table: creators

* id (PK, UUID)
* user_id (FK → users.id, nullable)
* display_name
* profile_image_url
* bio
* created_at

```

---

## 2.15 CreatorContentSubmission (Phase 2)
```

Table: creator_content_submissions

* id (PK, UUID)
* creator_id (FK → creators.id)
* script_text
* generated_video_url (nullable)
* status (draft, submitted, approved, rejected)
* notes
* created_at
* updated_at

```

---

## 2.16 RecommendationEvent
For training the recommendation engine.

```

Table: recommendation_events

* id (PK, UUID)
* user_id (FK → users.id)
* episode_id (FK → episodes.id)
* event_type (view, click, skip, complete)
* engagement_seconds (int)
* created_at

```

---

## 2.17 Notification
```

Table: notifications

* id (PK, UUID)
* user_id (FK → users.id)
* type (new_episode, recommendation, reminder)
* title
* message
* is_read (boolean)
* created_at

```

---

## 2.18 AnalyticsEvent
Generic event tracking (Mixpanel-style).

```

Table: analytics_events

* id (PK, UUID)
* user_id (FK → users.id, nullable)
* event_name
* attributes (JSONB)
* created_at

````

---

# 3. Relationships Summary

- A **User** has one **UserProfile**.  
- A **User** has one **Wallet**.  
- A **ContentSeries** has many **Episodes**.  
- An **Episode** has many **EpisodeVariants**.  
- A **Variant** has many **EpisodeAssets**.  
- A **Series** has many **Genres** through `series_genre_map`.  
- A **User** has many **UserEpisodeProgress** records.  
- A **User** has many **PaymentTransactions**.  
- A **User** has many **Subscriptions**.  
- A **Creator** may be linked to a User account.  
- A **Creator** has many **CreatorContentSubmissions**.  

---

# 4. ERD (Mermaid)

```mermaid
erDiagram

    users ||--|{ user_profiles : has
    users ||--|{ wallets : has
    users ||--|{ user_episode_progress : progresses
    users ||--|{ payment_transactions : makes
    users ||--|{ subscriptions : subscribes
    users ||--|{ analytics_events : triggers
    users ||--|{ recommendation_events : generates

    content_series ||--|{ episodes : contains
    episodes ||--|{ episode_variants : variations
    episode_variants ||--|{ episode_assets : assets

    content_series ||--|{ series_genre_map : maps
    genres ||--|{ series_genre_map : categorizes

    creators ||--|{ creator_content_submissions : submits
    users ||--|{ creators : links

````

---

# 5. Notes and Next Steps

* Add caching layer definitions (Redis keys for tokens, sessions, feed caching).
* Define event taxonomy for analytics.
* Add localization and multi-language tables if needed.
* Decide if episodes should be region-restricted.
* For AI tools, add tables for script generation logs and asset generation workflows.

```

---

If you'd like:
- SQL `.sql` schema files  
- JSON Schema for each entity  
- A GraphQL schema version  
- An even more normalized or more denormalized data model  

Just tell me!
```
