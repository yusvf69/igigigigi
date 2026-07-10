# Asset Library Structure

**Document ID:** AL-001  
**Version:** 1.0  
**Status:** Final  
**Classification:** Enterprise Asset Library Standard  
**Last Updated:** 2026-07-10  
**Owner:** Visual Production Directorate -- Asset Operations Division  

---

## Table of Contents

1. [Asset Library Architecture](#1-asset-library-architecture)
2. [Illustrations](#2-illustrations)
3. [Characters](#3-characters)
4. [Story](#4-story)
5. [Documentary](#5-documentary)
6. [Maps](#6-maps)
7. [Icons](#7-icons)
8. [Backgrounds](#8-backgrounds)
9. [Patterns](#9-patterns)
10. [3D Assets](#10-3d-assets)
11. [Animations](#11-animations)
12. [Videos](#12-videos)
13. [Audio](#13-audio)
14. [Narration](#14-narration)
15. [Ambient Audio](#15-ambient-audio)
16. [UI Assets](#16-ui-assets)
17. [Avatar Assets](#17-avatar-assets)
18. [KnowledgeTree Assets](#18-knowledgetree-assets)
19. [MemoryGarden Assets](#19-memorygarden-assets)
20. [Achievements](#20-achievements)
21. [Rewards](#21-rewards)
22. [Loading Assets](#22-loading-assets)
23. [Error Assets](#23-error-assets)
24. [Empty State Assets](#24-empty-state-assets)
25. [Metadata Schema](#25-metadata-schema)
26. [Asset Naming Convention](#26-asset-naming-convention)
27. [Version Control](#27-version-control)
28. [Access Control](#28-access-control)
29. [CDN Integration](#29-cdn-integration)

---

## Cross-Reference Index

| Related Document | Relationship | Key Integration Point |
|---|---|---|
| `00_Visual_Asset_Audit` | Audit identifies existing assets; this document defines organization | All audited assets must be relabeled per this convention |
| `01_Visual_Production_Bible` | Production Bible defines art direction; this document organizes the output | Every asset produced must fit one folder in this structure |
| `02_Illustration_Bible` | Illustration Bible defines illustration methodology | Illustrations stored per Section 2 hierarchy |
| `03_3D_Asset_Bible` | 3D Bible defines modeling standards | 3D assets stored per Section 10 hierarchy |
| `04_Animation_Bible` | Animation Bible defines motion standards | Animations stored per Section 11 hierarchy |
| `05_Video_Production_Bible` | Video Bible defines video production standards | Videos stored per Section 12 hierarchy |
| `06_Audio_Production_Bible` | Audio Bible defines recording and mixing standards | Audio stored per Sections 13-15 hierarchy |
| `07_Asset_Pipeline` | Pipeline defines automation for ingestion | Assets enter the library through the pipeline defined therein |
| `08_Quality_Control` | QC defines acceptance criteria | Assets must pass QC before entering approved branch |
| `09_AI_Asset_Generation` | AI generation defines synthetic asset creation | AI-generated assets stored alongside handcrafted assets |

## 1. Asset Library Architecture

### 1.1 Folder Hierarchy Design

The Asset Library is organized as a **deep directory tree** with standardized folder levels. Every asset in the production pipeline resides at a predictable, navigable path. The hierarchy follows a strict parent-child relationship: **Category -> Subcategory -> Asset -> Variant**.

The root of the Asset Library is `/Visual_Production/10_Asset_Library/`. All asset directories exist as subdirectories within this root. The top-level directory listing is the primary navigational interface for all asset operations.

### 1.2 Top-Level Categories

The Asset Library contains 23 top-level category directories. These categories correspond to the major visual asset types produced across the platform:

| Top-Level Directory | Primary Asset Type | Estimated Volume |
|---|---|---|
| `Illustrations/` | Raster and vector illustrations | 1,200+ assets |
| `Characters/` | Character models and renders | 500+ assets |
| `Story/` | Story-specific visual assets | 600+ assets |
| `Documentary/` | Documentary visual assets | 400+ assets |
| `Maps/` | Cartographic and geographic assets | 200+ assets |
| `Icons/` | Icon set across all sizes and states | 500+ assets |
| `Backgrounds/` | Background plates for all modes | 150+ assets |
| `Patterns/` | Repeating pattern designs | 100+ assets |
| `3D/` | Three-dimensional models and textures | 400+ assets |
| `Animations/` | Animation files in multiple formats | 300+ assets |
| `Videos/` | Rendered video content | 500+ assets |
| `Audio/` | Audio assets excluding narration | 400+ assets |
| `Narration/` | Voiceover and narration recordings | 800+ assets |
| `Ambient/` | Ambient environmental audio | 200+ assets |
| `UI/` | User interface graphical components | 300+ assets |
| `Avatar/` | User avatar system assets | 200+ assets |
| `KnowledgeTree/` | Knowledge Tree visualization assets | 150+ assets |
| `MemoryGarden/` | Memory Garden environment assets | 180+ assets |
| `Achievements/` | Achievement badge and icon assets | 80+ assets |
| `Rewards/` | Reward system visual assets | 100+ assets |
| `Loading/` | Loading screen and indicator assets | 60+ assets |
| `Errors/` | Error state visual assets | 40+ assets |
| `EmptyStates/` | Empty state placeholder assets | 50+ assets |

### 1.3 Sub-Folder Organization

Each top-level category follows a standardized depth convention:

- **Depth 1:** Category (e.g., `Illustrations/`)
- **Depth 2:** Subcategory (e.g., `Illustrations/Scenes/`)
- **Depth 3:** Sub-subcategory or Asset Group (e.g., `Illustrations/Scenes/Historical/`)
- **Depth 4:** Asset name or Collection (e.g., `Illustrations/Scenes/Historical/BattleOfBadr/`)
- **Depth 5:** Variants (e.g., `Illustrations/Scenes/Historical/BattleOfBadr/Variants/`)

No directory should exceed 5 levels of depth. Directories beyond 5 levels indicate a structural design flaw requiring reorganization.

### 1.4 Folder Naming Conventions

All folder names follow these rules:
- PascalCase for top-level categories (e.g., `Illustrations`, `KnowledgeTree`)
- PascalCase for subcategories (e.g., `Historical`, `MemoryGarden`)
- PascalCase with underscores only for compound names (e.g., `UI_Elements`, `GrowthAnimations`)
- Numbers at start for ordering within a collection (e.g., `01_StoryArc`, `02_Characters`)
- No spaces in folder names -- use PascalCase or underscores
- No special characters except underscore; no hyphens, periods, or ampersands
- Maximum 64 characters per folder name

### 1.5 Version Management

Asset versioning operates at the file level, not the folder level. The directory structure itself is version-agnostic. Version information is carried in:
1. The metadata schema (Version field)
2. The naming convention suffix (_V01, _V02)
3. Git LFS commit history
4. The review/tagging system in the repository

Archive folders must never be created within the library. Previous versions are retrieved through the version control system, not through manual folder duplication.

### 1.6 Metadata Schema

Every asset in the library MUST have an associated metadata file. The metadata format is JSON (`.asset.json`) placed alongside the asset file. The schema is defined in Section 25. Metadata files follow the same naming convention as the asset with `.asset.json` appended.

### 1.7 Search and Retrieval System

The asset library is designed for both manual browsing and automated search. Search indexes are built from:

1. **Metadata fields:** All fields in the JSON metadata are indexed
2. **Folder path:** The full directory path is treated as a searchable tag hierarchy
3. **File naming:** Asset filenames are tokenized for full-text search
4. **Thumbnails:** Auto-generated thumbnails stored in a parallel `_thumbnails/` directory at each level
5. **Tags:** Auto-extracted and manually curated tags in the metadata

The search index is rebuilt on every commit to the `main` branch and deployed to the internal Asset Library web interface.

### 1.8 Access Control

Access control is role-based and applies at the directory level. See Section 28 for the complete access control matrix. In summary:
- **Read access:** All production staff
- **Write access:** Artists assigned to that category
- **Delete access:** Admin only
- **Approve access:** Art Director and above

Access control is enforced through the Git LFS repository permissions and the Asset Library web interface.

### 1.9 CDN Integration

The CDN integration pipeline connects the approved asset library branch directly to the content delivery network. See Section 29 for the complete CDN integration specification. Key behaviors:
- Auto-sync from `main` branch to CDN on every approved commit
- Automatic format negotiation (WebP, AVIF)
- Responsive image set generation
- LQIP generation for all raster assets
- Cache invalidation triggered by metadata timestamp change

---

## 2. Illustrations

### 2.1 Purpose and Scope

The `Illustrations/` directory houses all 2D illustrative artwork produced for the platform. This includes narrative scene illustrations, character portraits, environmental vistas, informational graphics, decorative elements, and visual state representations. Illustrations are the primary visual storytelling medium across the platform.

### 2.2 Subfolder Structure

```
Illustrations/
|--- Achievements/ (Badges, ProgressBars, UnlockIllustrations)
|--- Animals/ (ArabianHorses, Camels, Birds, Marine, Wildlife)
|--- Architecture/
|   |--- Exteriors/ (Mosques, Palaces, Fortresses, Houses, PublicBuildings)
|   |--- Interiors/ (Libraries, StudyRooms, Courtyards, Marketplaces, Classrooms)
|--- Books/ (Manuscripts, Scrolls, OpenBooks, BookSpines, ReadingScenes)
|--- Calligraphy/ (QuranicVerses, Hadith, Poetry, Proverbs, Bismillah, NamesOfAllah)
|--- Characters/ (HistoricalFigures, Scholars, Students, CommunityMembers, FamilyGroups, Professions)
|--- EmptyStates/ (NoResults, NoBookmarks, NoProgress, FirstVisit)
|--- Environments/ (Desert, Mountain, Coastal, Forest, Agricultural, Urban)
|--- Errors/ (GenericError, Offline, Timeout, ServerError)
|--- Food/ (HistoricalDishes, Fruits, Dates, BreadAndGrains, Spices, Meals)
|--- Infographics/ (Timelines, ComparisonCharts, FlowDiagrams, DataVisualizations, ProcessMaps, Statistics)
|--- KnowledgeGraph/ (SubjectNodes, ConnectionEdges, ClusterDiagrams, RelationshipMaps, HierarchyTrees)
|--- KnowledgeTreeViz/ (TreeCanopies, RootSystems, GrowthStageVisuals, SeasonalVariants, SubjectSpecificTrees)
|--- Landscapes/ (ArabianPeninsula, Mesopotamia, Levant, NorthAfrica, Andalusia, Persia, IndianSubcontinent)
|--- Loading/ (StoryMode, QuizMode, DocumentaryMode, GeneralLoading, TransitionScreens)
|--- Maps/ (IllustratedMaps, StoryMaps, RouteMaps, ConceptMaps)
|--- MemoryViz/ (MemoryPalace, RecallVisuals, AssociationMaps, MemoryPaths, RetentionGraphs)
|--- Nature/ (Plants, Flowers, Trees, WaterBodies, Skies, NaturalPhenomena)
|--- Objects/ (Tools, Instruments, Utensils, Furniture, Lighting, Textiles, DecorativeItems)
|--- Patterns/ (GeometricPatterns, Arabesque, Floral, StarPatterns, BorderDecorations)
|--- Rewards/ (Trophies, Certificates, Medals, StarBursts, ConfettiScenes)
|--- Scenes/
|   |--- Historical/ (PreIslamic, PropheticEra, Rashidun, Umayyad, Abbasid, Andalusian, Ottoman, Mughal, Modern)
|   |--- Narrative/ (StoryArc01 through StoryArc05)
|--- Thumbnails/ (AutoGenerated, Custom)
```

### 2.3 Naming Convention

```
[IllustrationType]_[SubCategory]_[Description]_[View]_[Language]_[Variant]_[Resolution].[ext]
```

| Field | Description | Example |
|---|---|---|
| IllustrationType | Scene, Character, Environ, Object | `Scene` |
| SubCategory | Historical, Abbasid, Desert, Mosque | `Historical` |
| Description | Brief transliterated description | `BattleOfBadr` |
| View | Wide, Closeup, Aerial, Isometric | `Wide` |
| Language | EN, AR, FR, UR, blank if universal | `EN` |
| Variant | V01, V02, or color variant name | `V01` |
| Resolution | 4K, 2K, HD, Web | `4K` |
| ext | png, svg, webp | `png` |

**Examples:**
- `Scene_Historical_BattleOfBadr_Wide_EN_V01_4K.png`
- `Character_Abbasid_ScholarInLibrary_FullBody_AR_V02_HD.png`
- `Environ_Desert_OasisAtDusk_Wide_V01_4K.png`
- `Object_Astrolabe_V01_HD.png`

### 2.4 File Format Requirements

| Format | Usage | Color Space | Compression |
|---|---|---|---|
| PNG | Primary format for raster illustrations | sRGB or Adobe RGB (1998) | Lossless |
| SVG | Vector illustrations, icons, patterns | sRGB | None (text) |
| WebP | Web delivery, performance-critical | sRGB | Lossy (quality 90+) |
| PSD | Source files only -- never use in production | Adobe RGB (1998) | None |
| TIF | Archival high-res master | Adobe RGB (1998) or ProPhoto RGB | None or LZW |

### 2.5 Resolution Standards

| Designation | Pixel Dimensions | Use Case |
|---|---|---|
| Thumbnail | 150 x 150 | Search results, preview grids |
| Web | 1920 x 1080 | Standard web display |
| HD | 2560 x 1440 | Retina display, hero sections |
| 2K | 2048 x 1556 | Print, high-detail display |
| 4K | 3840 x 2160 | Master quality, video integration |
| 8K | 7680 x 4320 | Archival master (select illustrations only) |

### 2.6 Thumbnail Auto-Generation

All illustration assets automatically generate thumbnails upon ingestion into the library. The auto-generation system:
1. Reads the master file on ingestion
2. Generates a 150x150 pixel thumbnail maintaining aspect ratio
3. Places the thumbnail in the `_thumbnails/` subdirectory mirroring the source path
4. Appends `_thumb` to the filename
5. Updates the asset metadata with thumbnail path
6. Generates a blurhash string for progressive loading

### 2.7 Specialized Illustration Subcategories

| Subcategory | Visual Style Constraints | Color Palette |
|---|---|---|
| KnowledgeGraph | Clean lines, labeled nodes, hierarchical | Brand blues, gold accents, white background |
| MemoryViz | Dreamlike, atmospheric, layered | Warm tones, soft gradients, ethereal glow |
| KnowledgeTreeViz | Organic growth patterns, branching structures | Greens, earth tones, golden highlights |
| Achievements | Heraldic style, trophy aesthetics | Gold, deep blue, crimson |
| Rewards | Celebratory, sparkle effects, warm | Gold, warm white, amber |
| Loading | Engaging, calm, narrative fragment | Muted brand colors, low contrast |
| Errors | Soothing, not alarming, helpful | Warm neutrals, amber, soft red |
| EmptyStates | Inviting, aspirational, gentle | Light tones, brand accents, pastels |

---

## 3. Characters

### 3.1 Purpose and Scope

The `Characters/` directory stores all character-related assets including 3D model files, 2D renders, source files, rigging data, and evolutionary tier assets. Characters encompass avatar representations, the Knowledge Spirit companion, historical figure portraits, and fictional character designs.

### 3.2 Subfolder Structure

```
Characters/
|--- Avatars/
|   |--- Base/ (Tier01_Basic, Tier02_Refined, Tier03_Distinguished, Tier04_Illustrious)
|   |--- Expressions/ (Happy, Sad, Surprised, Thinking, Focused, Curious, Proud, Grateful, Contemplative)
|   |--- Accessories/ (Headwear, Eyewear, Jewelry, ClothingLayers, Footwear, BagsAndSatchels, WritingTools, DecorativeItems)
|   |--- Evolution/ (Tier01_Base, Tier02_Advanced, Tier03_Master, Tier04_Legendary)
|--- KnowledgeSpirit/
|   |--- BaseForms/ (Ethereal, Luminescent, Avian, Elemental)
|   |--- Expressions/
|   |--- Effects/ (Glow, ParticleTrail, Aura, Transformation)
|   |--- EvolutionStages/ (Stage01_Spark, Stage02_Flame, Stage03_Beacon, Stage04_Constellation)
|--- Character_Historical/
|   |--- Prophets/ | Caliphs/ | Rulers/ | Scientists/ | Philosophers/ | Poets/ | Explorers/ | Artists/
|   |--- Scholars/ (EarlyScholars, MedievalScholars, ModernScholars, FemaleScholars)
|--- Character_Fictional/ (StoryCharacters, EducationalMascots, ParableFigures, ScenarioPersonas)
|--- Rigging/ (SkeletonFiles, IKChains, BlendShapes, SkinWeights, AnimationControllers, RetargetingMaps)
```

### 3.3 Naming Convention

```
Char_[Type]_[Name]_[Tier]_[Expression]_[Variant].[ext]
```

| Field | Description | Example |
|---|---|---|
| Char | Fixed prefix | `Char` |
| Type | Avatar, Spirit, Historical, Fictional | `Avatar` |
| Name | Character name or identifier | `AishaScholar` |
| Tier | T01, T02, T03, T04 | `T03` |
| Expression | Happy, Sad, Focused | `Focused` |
| Variant | V01, V02, or color variant | `V01` |
| ext | fbx, png, psd | `fbx` |

**Examples:**
- `Char_Avatar_DefaultBase_T01_Happy_V01.fbx`
- `Char_Avatar_DefaultBase_T01_Happy_V01.png`
- `Char_Spirit_Ethereal_T03_Glowing_V02.png`
- `Char_Historical_IbnSina_T02_Contemplative_V01.png`

### 3.4 File Format Requirements

| Format | Usage | Notes |
|---|---|---|
| FBX | 3D character model | Include skeleton, no animation data |
| PNG | 2D character renders (production) | Transparent background |
| PSD | Source files for 2D character art | Layered, non-flattened |
| BLEND | Blender source (if applicable) | Keep in src/ alongside PSD |
| MA/MB | Maya source (if applicable) | Keep in src/ alongside PSD |

### 3.5 Rigging File Storage

Rigging files are stored separately from model files under `Characters/Rigging/`. Each subdirectory maps to a character type:
- `SkeletonFiles/`: One skeleton file per character type with standardized bone naming
- `IKChains/`: Inverse kinematics chain configurations
- `BlendShapes/`: Facial blend shape files organized by expression
- `SkinWeights/`: Vertex weight maps and skinning data
- `AnimationControllers/`: Animation retargeting controllers and state machines
- `RetargetingMaps/`: Bone-to-bone retargeting maps for animation reuse

Rigging files follow the naming convention: `Rig_[CharacterType]_[Component]_[Variant].[ext]`

### 3.6 Evolution Tier Visual Specification

| Tier | Visual Complexity | Detail Level | Effects |
|---|---|---|---|
| Tier 01 Basic | Low polygon, flat shading | Minimal details | No effects |
| Tier 02 Refined | Medium polygon, smooth shading | Moderate details | Subtle glow |
| Tier 03 Distinguished | High polygon, PBR materials | High detail | Ambient particles |
| Tier 04 Illustrious | Highest polygon, PBR+subsurface | Maximum detail | Full particle system, aura |

---

## 4. Story

### 4.1 Purpose and Scope

The `Story/` directory organizes all visual assets specific to narrative content. Stories are organized by story arc or collection, with each story having its own self-contained directory structure.

### 4.2 Subfolder Structure

```
Story/
|--- [StoryID_StoryName]/
|   |--- Scenes/ (001_SceneName, 002_SceneName, 003_SceneName...)
|   |--- Characters/ (CharacterName_StorySpecific, CharacterName_Expressions)
|   |--- Backgrounds/ (SceneBackgrounds, EnvironmentPlates)
|   |--- Overlays/ (TextOverlays, EffectOverlays, LightOverlays, AtmosphericOverlays)
|   |--- Transitions/ (SceneTransitions, FadeElements, WipeElements, AnimatedTransitions)
|   |--- Props/
|   |--- Thumbnails/
|--- ScenesMasterList.md
```

### 4.3 StoryID Convention

Each story is assigned a unique StoryID following the pattern: `STR_[ThreeDigitNumber]`. Examples: `STR_001_ProphetsOfPatience`, `STR_002_LightOfKnowledge`, `STR_003_TheGreatLibraries`.

### 4.4 Naming Convention

```
[StoryID]_[AssetType]_[SceneNumber]_[ShotNumber]_[Description]_[Variant].[ext]
```

| Field | Description | Example |
|---|---|---|
| StoryID | STR_001 | `STR_001` |
| AssetType | Scene, Char, BG, Overlay, Transition | `Scene` |
| SceneNumber | Three-digit zero-padded | `012` |
| ShotNumber | Two-digit zero-padded | `03` |
| Description | Brief shot description | `ProphetEntersCave` |
| Variant | V01, V02 | `V01` |
| ext | png, exr, webp | `png` |

**Examples:**
- `STR_001_Scene_012_03_ProphetEntersCave_V01.png`
- `STR_001_Char_012_01_YoungProphet_V02.png`
- `STR_001_BG_012_01_CaveInterior_V01.exr`

### 4.5 File Format Requirements

| Format | Usage | Quality Setting |
|---|---|---|
| PNG | Standard scene renders, character art | Lossless |
| EXR | High-quality scene renders with light data | 16-bit float, no compression |
| WebP | Web delivery and preview | Lossy quality 92 |
| PSD | Composite source files | Layered, non-flattened |

### 4.6 Scene Numbering System

- **001-010:** Establishing shots and exposition
- **011-050:** Rising action and development
- **051-080:** Climax and resolution
- **081-099:** Epilogue and transitions

---

## 5. Documentary

### 5.1 Purpose and Scope

The `Documentary/` directory organizes visual assets for documentary-style content. Documentaries are organized by series.

### 5.2 Subfolder Structure

```
Documentary/
|--- [DocID_DocumentaryName]/
|   |--- Thumbnails/ (EpisodeThumbnails, SeriesThumbnail, ChapterThumbnails)
|   |--- KeyArt/ (HeroImage, Poster, SocialMediaCards, TitleCards)
|   |--- ChapterImages/ (Chapter_01, Chapter_02, Chapter_03)
|   |--- Infographics/ (DataCharts, ComparisonGraphics, ExplanatoryGraphics, ProcessVisualizations)
|   |--- MapsUsed/ (RegionMaps, RouteMaps, BoundaryMaps)
|   |--- TimelineGraphics/ (EraTimelines, EventSequences, LifespanTimelines, DynastyCharts)
|--- DocumentaryMasterList.md
```

### 5.3 DocID Convention

Each documentary is assigned a unique DocID: `DOC_[ThreeDigitNumber]`. Examples: `DOC_001_GoldenAgeOfIslam`, `DOC_002_GreatScholarsOfBaghdad`, `DOC_003_RoutesOfKnowledge`.

### 5.4 Naming Convention

```
[DocID]_[Type]_[Description]_[Language]_[Variant].[ext]
```

**Examples:**
- `DOC_001_Thumbnail_Episode01_EN_V01.png`
- `DOC_001_KeyArt_HeroImage_EN_V02.png`
- `DOC_001_Infographic_ScientificAchievements_EN_V01.svg`

---

## 6. Maps

### 6.1 Purpose and Scope

The `Maps/` directory stores all cartographic and geographic assets, organized by region and historical period.

### 6.2 Subfolder Structure

```
Maps/
|--- ArabianPeninsula/ (PreIslamic, PropheticEra, Rashidun, Umayyad, Abbasid, Ottoman, Modern)
|--- Mesopotamia/ (Ancient, Abbasid, Modern)
|--- Levant/ (Ancient, Umayyad, Abbasid, CrusaderPeriod, Ayyubid, Mamluk, Ottoman, Modern)
|--- NorthAfrica/ (Ancient, IslamicConquest, Fatimid, Almoravid, Almohad, Ottoman, Modern)
|--- Andalusia/ (Umayyad, TaifaKingdoms, Almoravid, Almohad, Nasrid)
|--- Persia/ (PreIslamic, IslamicConquest, Safavid, Qajar, Modern)
|--- IndianSubcontinent/ (DelhiSultanate, Mughal, Modern)
|--- CentralAsia/ (SilkRoad, Timurid, Modern)
|--- SubSaharanAfrica/ (MedievalKingdoms, TradeRoutes, Modern)
|--- SoutheastAsia/ (SpreadOfIslam, Modern)
|--- WorldMaps/ (HistoricalWorld, ModernWorld, TradeRoutes, CulturalSpheres)
|--- CelestialMaps/ (StarCharts, ConstellationMaps, Zodiac, AstronomicalDiagrams)
|--- InteractiveLayers/ (PoliticalOverlay, TopographicOverlay, TradeRouteOverlay, CulturalOverlay, LanguageOverlay, ClimateOverlay, TimelineOverlay)
```

### 6.3 Map Types

| Map Type | Description | Primary Format | Secondary Format |
|---|---|---|---|
| Political | Borders, capitals, cities | SVG | PNG |
| Topographic | Elevation, terrain, waterways | TIF | PNG |
| TradeRoutes | Trade networks, ports, caravan routes | SVG | PNG |
| HistoricalBoundaries | Evolving borders across eras | SVG | PNG |
| CityPlans | Street maps, district maps | SVG | PNG |
| Celestial | Stars, constellations, astronomical | SVG | PNG |
| InteractiveLayers | Overlay-compatible base maps | JSON+SVG | PNG |

### 6.4 Naming Convention

```
Map_[Region]_[Period]_[Type]_[Scale]_[Variant].[ext]
```

**Examples:**
- `Map_ArabianPeninsula_PropheticEra_Political_Medium_V01.svg`
- `Map_Andalusia_Nasrid_Topographic_Detail_V01.tif`
- `Map_World_HighMedieval_TradeRoutes_Large_V02.png`
- `Map_Baghdad_Abbasid_CityPlan_Detail_V01.svg`

### 6.5 Georeferencing Standards

- Coordinate system: WGS 84 (EPSG:4326) for reference
- Display projection: Robinson for world maps, Lambert Conformal Conic for regional
- GeoJSON files must include `crs` property
- TIF files must include embedded georeferencing metadata

---

## 7. Icons

### 7.1 Purpose and Scope

The `Icons/` directory stores all iconographic assets organized by functional category with each asset produced at multiple sizes and states.

### 7.2 Subfolder Structure

```
Icons/
|--- Navigation/ (Home, Search, Menu, Back, Forward, Up, Down, Close, Expand, Collapse, Breadcrumb)
|--- Actions/ (Play, Pause, Stop, Record, Download, Upload, Share, Save, Edit, Delete, Copy, Cut, Paste, Add, Remove, Filter, Sort, More)
|--- Content/ (Book, Article, Video, Audio, Image, Document, PDF, Quiz, Flashcards, Timeline, Map, Podcast)
|--- Media/ (Playback, Volume, Mute, Fullscreen, Subtitles, Speed, Quality, PictureInPicture, ScreenCast)
|--- Status/ (Online, Offline, Away, Busy, Syncing, Complete, Pending, Failed, Warning, Info, Success, Error)
|--- Learning/ (KnowledgeTree, MemoryGarden, Achievements, Rewards, Progress, Streak, Level, Points, Badge, Certificate)
|--- Social/ (Profile, Friends, Messages, Notifications, Comments, Likes, Follow, Leaderboard)
|--- AI/ (AIAssistant, KnowledgeSpirit, SmartSuggest, AutoComplete, VoiceInput, Translation, Personalization)
|--- Quranic/ (Quran, Ayah, Surah, Juz, Hizb, Sajdah, Ruku, Waqf, Tajweed)
|--- Hadith/ (Hadith, Narrator, Chain, Book, Chapter)
|--- UI_Elements/ (Checkbox, RadioButton, Toggle, Dropdown, Slider, Stepper, Tab, Pagination, Tooltip, Modal, Drawer, Accordion, Card)
```

### 7.3 Naming Convention

```
Icon_[Category]_[Name]_[Size]_[State].[ext]
```

| Field | Description | Example |
|---|---|---|
| Icon | Fixed prefix | `Icon` |
| Category | Navigation, Actions, Content, Media, etc. | `Navigation` |
| Name | Icon identifier | `Home` |
| Size | 16, 20, 24, 32, 40, 48 | `24` |
| State | Default, Active, Disabled, Hover | `Default` |
| ext | svg, png | `svg` |

**Examples:**
- `Icon_Navigation_Home_24_Default.svg`
- `Icon_Navigation_Home_24_Active.svg`
- `Icon_Navigation_Home_24_Disabled.svg`
- `Icon_Actions_Play_48_Active.svg`
- `Icon_Quranic_Quran_32_Default.svg`

### 7.4 Size Specifications

| Size (px) | Grid | Use Case |
|---|---|---|
| 16 | 16 x 16 | Inline text, compact UI |
| 20 | 20 x 20 | Small buttons, list items |
| 24 | 24 x 24 | Standard icon size, most common |
| 32 | 32 x 32 | Primary actions, navigation items |
| 40 | 40 x 40 | Large touch targets, cards |
| 48 | 48 x 48 | Hero icons, empty state illustrations |

### 7.5 State Definitions

| State | Visual Treatment | Use Case |
|---|---|---|
| Default | Normal appearance, brand color | Resting state |
| Active | Highlighted, filled or weighted variation | Selected, pressed, or focused |
| Disabled | Reduced opacity (40%), no hover effects | Non-interactive state |
| Hover | Subtle fill or color shift | Mouse hover (generated from CSS when possible) |

### 7.6 SVG Quality Standards

- All paths must be closed and clean (no extraneous points)
- Use 1.5px minimum stroke width
- Stroke-to-fill conversion where appropriate
- No embedded raster images
- No external font dependencies (convert text to paths)
- Export with SVG 1.1 compliance
- Include `xmlns` attribute

---

## 8. Backgrounds

### 8.1 Purpose and Scope

The `Backgrounds/` directory stores all background image assets used across different learning modes and interface contexts.

### 8.2 Subfolder Structure

```
Backgrounds/
|--- Story/ (Light, Dark, Animated, Seasonal)
|--- Quiz/ (Light, Dark, Animated, Celebration)
|--- Flashcards/ (Light, Dark, Animated, Focused)
|--- Podcast/ (Light, Dark, Animated, Ambient)
|--- Documentary/ (Light, Dark, Animated, Atmospheric)
|--- Map/ (Light, Dark, Animated, Parchment)
|--- Timeline/ (Light, Dark, Animated, Horizontal)
|--- AI/ (Light, Dark, Animated, Ethereal)
|--- Memory/ (Light, Dark, Animated, Garden, Palace)
|--- KnowledgeTree/ (Light, Dark, Animated, Canopy, Roots)
|--- General/ (Light, Dark, Animated, Login, Dashboard, Settings, Profile, Onboarding)
|--- Common/ (Textures, Gradients, Noise, Patterns)
```

### 8.3 Naming Convention

```
BG_[Mode]_[Type]_[Variant]_[Resolution].[ext]
```

**Examples:**
- `BG_Story_Dark_LibraryStudy_HD.png`
- `BG_Quiz_Animated_Celebration_HD.webp`
- `BG_KnowledgeTree_Dark_Canopy_4K.png`

### 8.4 File Format Requirements

| Format | Usage | Specifications |
|---|---|---|
| PNG | Static backgrounds (primary) | sRGB, 8-bit |
| WebP | Static backgrounds (web delivery) | Lossy quality 90 |
| MP4 | Animated backgrounds (video) | H.264, 30fps, loop |
| Lottie JSON | Animated backgrounds (vector) | Optimized, no expressions |
| PSD | Source files | Layered for easy recoloring |

---

## 9. Patterns

### 9.1 Purpose and Scope

The `Patterns/` directory stores repeating pattern designs used as decorative elements, backgrounds, and interface textures.

### 9.2 Subfolder Structure

```
Patterns/
|--- Geometric/ (Tileable, FullSize, ColorVariants: Gold/Blue/Green/Teal/Monochrome)
|--- Arabesque/ (Tileable, FullSize, ColorVariants, Style: Ottoman/Persian/Maghrebi/IndoIslamic)
|--- Floral/ (Tileable, FullSize, ColorVariants, Style: Stylized/Realistic/Minimal/Ornate)
|--- Calligraphic/ (Tileable, FullSize, ColorVariants, Script: Kufic/Naskh/Diwani/Thuluth/Ruqah)
|--- Abstract/ (Tileable, FullSize, ColorVariants, Style: Modern/Gradient/Minimal/Textured)
```

### 9.3 Naming Convention

```
Pattern_[Type]_[Name]_[ColorVariant]_[Size].[ext]
```

**Examples:**
- `Pattern_Geometric_StarTessellation_Gold_Tileable.svg`
- `Pattern_Arabesque_OttomanScroll_Gold_FullSize.png`
- `Pattern_Calligraphic_KuficBismillah_Gold_Tileable.svg`

### 9.4 Tileability Requirements

- SVG: Pattern must tile seamlessly at 100x100px minimum, 512x512px standard
- PNG: Tileable at 512x512px standard
- No visible seam at tile boundaries
- Wrap mode: Repeat
- Test tile at 2x2 grid to verify seamlessness

---

## 10. 3D Assets

### 10.1 Purpose and Scope

The `3D/` directory stores all three-dimensional assets including models, textures, materials, prefabs, and LOD variants.

### 10.2 Subfolder Structure

```
3D/
|--- Buildings/ (Models, Textures, Materials, Prefabs, LODs, Blueprints)
|--- Locations/ (Models, Textures, Materials, Prefabs, LODs)
|--- Terrain/ (Heightmaps, Textures, Materials, Vegetation, LODs)
|--- Artifacts/ (Models, Textures, Materials, Prefabs, LODs)
|--- Books/ (Models, Textures, Materials, Prefabs, LODs)
|--- Tools/ (Models, Textures, Materials, Prefabs, LODs)
|--- KnowledgeTree/ (Models, Textures, Materials, Prefabs, LODs)
|--- Avatar/ (Models, Textures, Materials, Prefabs, LODs)
|--- Spirit/ (Models, Textures, Materials, Prefabs, LODs)
|--- MemoryGarden/ (Models, Textures, Materials, Prefabs, LODs)
|--- Simulations/ (Models, Textures, Materials, Prefabs, LODs)
|--- Materials/ (PBR_Library, SubstanceFiles, ShaderGraphs, MaterialInstances)
|--- Common/ (Skyboxes, LightProbes, ReflectionProbes, CollisionMeshes)
```

### 10.3 Naming Convention

```
3D_[Category]_[Name]_[LOD]_[Version].[ext]
```

**Examples:**
- `3D_Building_GreatMosqueKairouan_LOD0_V01.fbx`
- `3D_Building_GreatMosqueKairouan_Texture_Albedo_4K_V01.png`
- `3D_Artifact_Astrolabe_LOD0_V01.gltf`

### 10.4 LOD Specifications

| LOD Level | Triangle Budget | Texture Resolution | Usage Distance |
|---|---|---|---|
| LOD0 (Highest) | 100,000+ | 4096 x 4096 | 0-5m (close-up) |
| LOD1 (High) | 50,000 | 2048 x 2048 | 5-20m |
| LOD2 (Medium) | 20,000 | 1024 x 1024 | 20-50m |
| LOD3 (Low) | 5,000 | 512 x 512 | 50m+ |

### 10.5 Texture Map Naming

Each texture map type follows a suffix convention: `_Albedo` (sRGB), `_Normal` (Linear), `_Roughness` (Linear), `_Metallic` (Linear), `_AO` (Linear), `_Height` (Linear), `_Emissive` (sRGB), `_Opacity` (Linear), `_Mask` (Linear).

### 10.6 PBR Material Standards

All 3D assets use Physically Based Rendering (PBR) with Metallic-Roughness workflow as standard. Energy-conserving materials with real-world IOR values. Linear color space for data textures, sRGB for albedo and emissive.

---

## 11. Animations

### 11.1 Purpose and Scope

The `Animations/` directory stores all animation assets: character animations, UI micro-interactions, environmental animations, particle effects, and animated transitions.

### 11.2 Subfolder Structure

```
Animations/
|--- Avatar/ (Idle, Walk, Run, Greet, Wave, Read, Write, Think, Celebrate, AchievementUnlock, LevelUp, Evolve)
|--- Spirit/ (Idle, Float, Transform, Glow, Pulse, Orbit, Trail, Merge, Expand)
|--- KnowledgeTree/ (Grow, Bloom, Glow, BranchExtend, LeafFall, FruitAppear, SeasonalChange, TreeEvolve)
|--- Memory/ (FlowerBloom, PlantGrow, WaterRipple, LightPulse, MemoryReveal, ConnectionForm, GardenGrow)
|--- SceneTransitions/ (Fade, Wipe, Slide, Zoom, Morph, BookTurn, ScrollReveal, PageFlip, PortalOpen)
|--- Loading/ (Spinner, ProgressBar, Skeleton, PulseRing, Dots, KnowledgeOrb, BookOpening, LampLighting)
|--- Rewards/ (CoinCollect, GemBounce, StarExplosion, Burst, Confetti)
|--- Achievements/ (BadgeReveal, BadgeGlow, ProgressFill, Completion, TrophyAppear, RibbonUnfold)
|--- Environment/ (WaterFlow, GrassWave, TreeSway, CloudDrift, SandDune, FireFlicker, CandleGlow, LanternSwing, CurtainBreeze)
|--- Particles/ (Stars, Sparkles, Dust, LightOrbs, Fireflies, Petals, Leaves, Sand, Smoke, Mist, MagicalGlow)
|--- UI_Micro/ (ButtonPress, ButtonHover, CardFlip, ModalOpen, ModalClose, DrawerSlide, TabSwitch, AccordionOpen, SwitchToggle, SliderDrag, ListItemAppear, NotificationSlide, TooltipAppear, PageTransition)
```

### 11.3 Naming Convention

```
Anim_[Type]_[Name]_[Speed]_[Variant].[ext]
```

| Field | Description | Example |
|---|---|---|
| Anim | Fixed prefix | `Anim` |
| Type | Avatar, UI_Micro, Loading, Particles | `Avatar` |
| Name | Animation name | `Walk` |
| Speed | Normal, Fast, Slow | `Normal` |
| Variant | V01, V02 | `V01` |
| ext | fbx, json, mp4 | `fbx` |

**Examples:**
- `Anim_Avatar_Walk_Normal_V01.fbx`
- `Anim_UI_Micro_ButtonPress_Normal_V01.json`
- `Anim_Loading_KnowledgeOrb_Normal_V01.mp4`

### 11.4 Animation Timing Standards

| Category | Default Duration | Easing |
|---|---|---|
| UI Micro-interactions | 200-400ms | ease-in-out |
| Loading animations | 1-3s loop | linear |
| Scene transitions | 500-800ms | ease-in-out |
| Character animations | Varies | Custom |
| Environmental | 3-10s loop | Custom |
| Rewards/Celebration | 1-2s | ease-out |
| Particle systems | 2-5s loop | Custom |



## 12. Videos

### 12.1 Purpose and Scope

The `Videos/` directory stores all rendered video content: story animations, documentary episodes, educational explainers, motion graphics, and promotional content.

### 12.2 Subfolder Structure

```
Videos/
|--- Story/ ([StoryID]/ Master, Delivery, Social/Square_1x1, Portrait_9x16, Thumbnail, Previews)
|--- Documentary/ ([DocID]/ Master, Delivery, Social, Thumbnail, Previews)
|--- Educational/ ([CourseID_Topic]/ Master, Delivery, Chapters, Social, Thumbnail, Previews)
|--- Explainer/ ([Topic]/ Master, Delivery, Social, Thumbnail, Previews)
|--- Whiteboard/ ([Topic]/ Master, Delivery, Thumbnail, Templates)
|--- MotionGraphics/ ([Project]/ Master, Delivery, Thumbnail, Templates)
|--- Timeline/ ([TimelineID]/ Master, Delivery, Thumbnail, Templates)
|--- Map/ ([MapID]/ Master, Delivery, Thumbnail, Templates)
|--- AI_Generated/ ([ProjectID]/ Master, Delivery, Metadata, Previews)
|--- HistoricalReconstructions/ ([EventID]/ Master, Delivery, Thumbnail, Reference)
|--- Promos/ ([Campaign]/ Master, Delivery, Social, Thumbnail, Previews)
```

### 12.3 Naming Convention

```
Video_[Series]_[EpisodeNumber]_[Version]_[Format].[ext]
```

**Examples:**
- `Video_Story_001_005_V01_Master.mov`
- `Video_Story_001_005_V01_Delivery.mp4`
- `Video_Story_001_005_V01_Social_1x1.mp4`
- `Video_Doc_003_012_V02_Delivery.mp4`

### 12.4 Video Format Specifications

| Format | Codec | Resolution | Bitrate | Use Case |
|---|---|---|---|---|
| Master (ProRes) | ProRes 422 HQ | 3840 x 2160 | ~220 Mbps | Archival master |
| Master (ProRes) | ProRes 4444 | 3840 x 2160 | ~330 Mbps | Archival with alpha |
| Delivery (H.264) | H.264 | 1920 x 1080 | 15 Mbps | Web delivery |
| Delivery (H.264) | H.264 | 1280 x 720 | 8 Mbps | Standard web |
| Social 1x1 | H.264 | 1080 x 1080 | 10 Mbps | Social media |
| Social 9x16 | H.264 | 1080 x 1920 | 12 Mbps | Mobile/Stories |

### 12.5 Audio Specifications for Video

| Property | Master | Delivery |
|---|---|---|
| Sample rate | 48 kHz | 48 kHz |
| Bit depth | 24-bit | 16-bit |
| Channels | Stereo or 5.1 | Stereo |
| Codec | PCM uncompressed | AAC |
| Bitrate | N/A | 320 kbps |

---

## 13. Audio

### 13.1 Purpose and Scope

The `Audio/` directory stores all non-narration audio assets including sound design, music, ambient textures, interface sounds, and user feedback audio.

### 13.2 Subfolder Structure

```
Audio/
|--- Narration/ (See Section 14)
|--- Ambient/ (See Section 15)
|--- Interface/ (Buttons, Navigation, Transitions, Modals, Toggles, Sliders, Notifications)
|--- Achievements/ (BadgeUnlock, LevelUp, Streak, Milestone, Completion)
|--- Memory/ (Bloom, GardenGrowth, KnowledgeReveal, ConnectionForm, Recall)
|--- Notifications/ (General, Reminder, Message, Alert, Urgent)
|--- Feedback/ (Correct, Incorrect, Partial, Hint, TryAgain, Complete)
|--- Music/
|   |--- ByMode/ (Study, Quiz, Reflection, Exploration, Celebration, Meditation)
|   |--- ByMood/ (Calm, Focused, Inspired, Triumphant, Mysterious, Reverent, Joyful)
|   |--- ByComposer/ | Loops/ | Stings/ | FullTracks/
|--- Accessibility/ (AudioDescriptions, ScreenReader, NavigationAudio, HighContrastAudio)
|--- SoundDesign/ (Foley, BookSounds, PaperRustle, PenWrite, Water, Wind, Fire, Footsteps, Doors, Ambience)
```

### 13.3 Naming Convention

```
Audio_[Type]_[Description]_[Language]_[Version].[ext]
```

**Examples:**
- `Audio_Interface_ButtonTap_V01.wav`
- `Audio_Feedback_Correct_V01.wav`
- `Audio_Music_Study_CalmLoop_V01.wav`
- `Audio_Achievement_BadgeUnlock_V01.mp3`

### 13.4 File Format Requirements

| Format | Usage | Specifications |
|---|---|---|
| WAV | Master recordings, all critical audio | 48kHz, 24-bit, PCM |
| AAC | Web delivery, streaming | 320 kbps, 48kHz |
| MP3 | Legacy delivery, bandwidth-constrained | 256 kbps, 44.1kHz |
| FLAC | Archival lossless | 48kHz, 24-bit |

### 13.5 Audio Processing Standards

| Property | Value |
|---|---|
| Sample rate | 48 kHz (all production audio) |
| Bit depth | 24-bit (master), 16-bit (delivery) |
| Loudness (integrated) | -16 LUFS (music), -18 LUFS (interface) |
| True peak | -1 dBTP |
| Loudness range | < 10 LU |
| Noise floor | < -60 dBFS |
| Headroom | 6 dB |

---

## 14. Narration

### 14.1 Purpose and Scope

The `Narration/` directory stores all voiceover and narration audio recordings. This is a subdirectory of `Audio/` managed as a separate organizational entity.

### 14.2 Subfolder Structure

```
Audio/Narration/
|--- [Language_Code]/
|   |--- [VoiceActor_Name]/
|   |   |--- Story/ ([StoryID]/ ScriptReferences, Takes, FullSession)
|   |   |--- Documentary/ ([DocID]/ ScriptReferences, Takes, FullSession)
|   |   |--- Educational/ ([CourseID]/ ScriptReferences, Takes, FullSession)
|   |   |--- UI/ (ScriptReferences, Takes, FullSession)
|--- MultiTrack/ ([ProjectID]/ Stems, Mixed, Alternates)
|--- GlobalScripts/ (StoryScripts, DocScripts, UIScripts)
```

### 14.3 Language Code Convention

| Language | Code | Dialect Notes |
|---|---|---|
| Arabic | AR | Modern Standard Arabic (primary) |
| Arabic (Gulf) | AR-GL | Gulf dialect variants |
| Arabic (Egyptian) | AR-EG | Egyptian dialect variants |
| Arabic (Levantine) | AR-LV | Levantine dialect variants |
| English | EN | Neutral American (primary) |
| English (UK) | EN-UK | British English |
| French | FR | Standard French |
| Urdu | UR | Standard Urdu |
| Turkish | TR | Standard Turkish |
| Malay | MS | Standard Malay |
| Indonesian | ID | Standard Indonesian |
| Persian | FA | Standard Persian |
| Swahili | SW | East African |
| Hausa | HA | West African |
| Bengali | BN | Standard Bengali |

### 14.4 Naming Convention

```
Narration_[Language]_[VoiceActor]_[ContentType]_[ContentID]_[SegmentID]_[Take].[ext]
```

**Examples:**
- `Narration_AR_KhalidAlRashid_Story_STR_001_S012_03_BestTake.wav`
- `Narration_EN_SarahJohnson_Doc_DOC_003_Ch02_T01.wav`
- `Narration_FR_PierreDubois_Educational_ED_005_Lesson03_BestTake.wav`

### 14.5 Recording Standards

| Property | Value |
|---|---|
| Sample rate | 48 kHz |
| Bit depth | 24-bit |
| Microphone | Condenser (Neumann U87 or equivalent) |
| Recording environment | Vocal booth or treated room |
| Noise floor | < -65 dBFS |
| Distance from mic | 6-12 inches |
| Pop filter | Required |
| Room tone | 15 seconds required per session |

### 14.6 Take Management

- **FullSession/:** The complete raw recording session file, unedited
- **ScriptReferences/:** PDF or text files of the script read
- **Takes/:** Individual segmented takes per script section
- **BestTake:** The approved, edited, normalized version of each segment

---

## 15. Ambient Audio

### 15.1 Purpose and Scope

The `Ambient/` directory stores ambient environmental audio textures for immersive learning environments.

### 15.2 Subfolder Structure

```
Audio/Ambient/
|--- City/ (Loopable, FullExperience, Variants: Day, Night, Market)
|--- Desert/ (Loopable, FullExperience, Variants: Day, Night, Sandstorm, Oasis)
|--- Library/ (Loopable, FullExperience, Variants: Large, Small, ReadingRoom)
|--- Mosque/ (Loopable, FullExperience, Variants: Interior, Courtyard, PrayerTime)
|--- Nature/ (Loopable, FullExperience, Variants: Forest, Garden, River, Ocean, Meadow)
|--- Market/ (Loopable, FullExperience, Variants: SpiceSouk, BookMarket, TextileSouk)
|--- Classroom/ (Loopable, FullExperience, Variants: Lecture, Discussion, StudyHall)
|--- Water/ (Loopable, FullExperience, Variants: Fountain, Rain, Stream, Sea)
|--- Fire/ (Loopable, FullExperience, Variants: Campfire, Candle, Torch)
|--- Wind/ (Loopable, FullExperience, Variants: Breeze, Strong, ThroughLeaves)
|--- Night/ (Loopable, FullExperience, Variants: DesertNight, CityNight, CountryNight)
```

### 15.3 Naming Convention

```
Ambient_[Environment]_[Type]_[Variant]_[Duration].[ext]
```

**Examples:**
- `Ambient_Library_Loopable_ReadingRoom_120s.wav`
- `Ambient_Desert_FullExperience_Day_300s.wav`
- `Ambient_Mosque_Loopable_Interior_60s.aac`

### 15.4 Loopable Specifications

| Property | Value |
|---|---|
| Duration | Minimum 30 seconds, recommended 60-120 seconds |
| Loop point | Zero-crossing at both start and end |
| Crossfade | 500ms minimum for seamless transition |
| Fade type | Equal power |

---

## 16. UI Assets

### 16.1 Purpose and Scope

The `UI/` directory stores all user interface graphical assets: component graphics, screen mockups, state visuals, page templates, and shared UI elements.

### 16.2 Subfolder Structure

```
UI/
|--- Components/ (Button, Card, Input, Modal, Navigation, Progress, Avatar, Feedback)
|--- Screens/ (Login, Onboarding, Dashboard, Profile, Settings, Learning, KnowledgeTree, MemoryGarden, Achievements, Rewards, Search)
|--- States/ ([ComponentName]/ Default, Hover, Active, Focused, Disabled, Loading, Error, Success)
|--- Templates/ (PageLayouts, ContentLayouts, ListLayouts, GridLayouts, PrintLayouts)
|--- Assets/ (SharedGraphics, Dividers, Separators, Borders, Shadows, Gradients, Masks)
```

### 16.3 Naming Convention

```
UI_[Category]_[Component]_[State]_[Platform].[ext]
```

**Examples:**
- `UI_Component_Button_Primary_Default_Web.svg`
- `UI_Component_Button_Primary_Hover_Web.svg`
- `UI_Component_Button_Primary_Active_Web.svg`
- `UI_Component_Card_Achievement_Default_iOS.png`
- `UI_Screen_Dashboard_Home_Web.png`

### 16.4 Design Token Integration

UI assets reference design tokens stored in JSON format:
- Color tokens: `$color-primary-500`, `$color-neutral-100`
- Spacing tokens: `$spacing-xs`, `$spacing-md`, `$spacing-xl`
- Typography tokens: `$font-heading-xl`, `$font-body-md`
- Shadow tokens: `$shadow-card`, `$shadow-modal`
- Radius tokens: `$radius-sm`, `$radius-lg`, `$radius-full`

---

## 17. Avatar Assets

### 17.1 Purpose and Scope

The `Avatar/` directory mirrors the character system structure but is dedicated specifically to user avatar assets with evolution tiers.

### 17.2 Subfolder Structure

```
Avatar/
|--- BaseModel/ (Tier01_Novice, Tier02_Student, Tier03_Scholar, Tier04_Master, Tier05_Legendary)
|--- Expressions/ (Neutral, Happy, Sad, Surprised, Thinking, Focused, Curious, Proud, Grateful, Contemplative, Celebrating)
|--- Outfits/ (Tier01_Basic, Tier02_Refined, Tier03_Distinguished, Tier04_Master, Tier05_Legendary, Seasonal)
|--- Accessories/
|   |--- Headwear/ (Turban, Kufi, Amamah, Ghutra)
|   |--- Eyewear/ (ReadingGlasses, ScholarsGlasses, Monocle)
|   |--- Jewelry/ (Ring, Bracelet, Necklace, Brooch)
|   |--- Bags/ (BookSatchel, ScrollCase, PenCase)
|   |--- Special/ (HaloKnowledge, AuraLight, CrownWisdom)
|--- EvolutionStages/ (Stage01_Seed, Stage02_Sprout, Stage03_Sapling, Stage04_Tree, Stage05_AncientTree)
|--- Animations/ (Idle, Greet, Read, Write, Think, Celebrate, LevelUp, Evolve, AchievementUnlock, Special)
```

### 17.3 Naming Convention

```
Avatar_[Tier]_[Component]_[Type]_[Variant].[ext]
```

**Examples:**
- `Avatar_T03_BaseModel_V01.fbx`
- `Avatar_T03_Expression_Happy_V01.png`
- `Avatar_T04_Outfit_V02.fbx`
- `Avatar_T02_Accessory_Headwear_Turban_V01.fbx`

### 17.4 Evolution Tier System

| Tier | Title | Visual Complexity | Unlock Condition |
|---|---|---|---|
| T01 Novice | Seed of Knowledge | Simple, low detail | Account creation |
| T02 Student | Sprout of Wisdom | Moderate detail | 100 knowledge points |
| T03 Scholar | Sapling of Understanding | High detail | 500 knowledge points |
| T04 Master | Tree of Knowledge | Very high detail | 2000 knowledge points |
| T05 Legendary | Ancient Tree of Wisdom | Maximum detail | 10000 knowledge points |

---

## 18. KnowledgeTree Assets

### 18.1 Purpose and Scope

The `KnowledgeTree/` directory stores all assets for the Knowledge Tree visualization.

### 18.2 Subfolder Structure

```
KnowledgeTree/
|--- TreeModels/ (Stage01_Seedling through Stage05_Ancient, Seasonal, Species)
|--- NodeIcons/ (BySubject, ByLevel, ByStatus, SpecialNodes)
|--- ConnectionLines/ (Active, Inactive, Discovered, Undiscovered, Prerequisite, Related, Animated)
|--- Backgrounds/ (Light, Dark, Animated, SkyBackground, StarField, AbstractNebula)
|--- ParticleEffects/ (Leaves, LightOrbs, Sparkles, GlowDust, Petals, KnowledgeSpark)
|--- Animations/ (TreeGrowth, BranchExtend, NodeReveal, ConnectionForm, Prune, Bloom, FruitAppear, SeasonalChange, MilestoneCelebration, CompletionEffect)
```

### 18.3 Naming Convention

```
KT_[Stage]_[Component]_[Variant].[ext]
```

**Examples:**
- `KT_S03_TreeModel_Mature_V01.fbx`
- `KT_NodeIcon_Quran_Completed_V01.png`
- `KT_ConnectionLine_Active_V01.svg`
- `KT_Background_Dark_V01.png`

### 18.4 Tree Growth Stage Visual Specifications

| Stage | Visual Characteristics | Branches | Nodes | Effects |
|---|---|---|---|---|
| S01 Seedling | Single trunk, 2-3 branches | 2-3 | 5-10 | Subtle glow |
| S02 Growing | Main trunk, 5-8 branches | 5-8 | 10-25 | Leaf particles |
| S03 Mature | Full trunk, 10-15 branches | 10-15 | 25-50 | Light orbs |
| S04 Flourishing | Thick trunk, 15-20 branches | 15-20 | 50-100 | Bloom effects |
| S05 Ancient | Grand trunk, 20+ branches | 20+ | 100+ | Full particle system |

---

## 19. MemoryGarden Assets

### 19.1 Purpose and Scope

The `MemoryGarden/` directory stores all assets for the Memory Garden visual metaphor representing memorized knowledge.

### 19.2 Subfolder Structure

```
MemoryGarden/
|--- Plants/ (Rose, Tulip, Lotus, Jasmine, Lavender, Sunflower, PalmSeedling -- each with Stage01_Seed through Stage05_FullBloom)
|--- GardenEnvironments/ (Light, Dark, Sunrise, Sunset, Night, Seasonal, Themes)
|--- WaterEffects/ (Fountain, Stream, Pool, Rain, Dew, Reflection)
|--- GrowthAnimations/ (PlantSprout, LeafUnfold, FlowerOpen, PetalFall, VineGrow, GlowPulse, Transformation)
|--- Decorations/ (Pathways, Benches, Lamps, Arbors, Bridges, Fences, WaterFeatures)
```

### 19.3 Naming Convention

```
MG_[PlantType]_[Stage]_[Variant].[ext]
```

**Examples:**
- `MG_Rose_Seed_V01.png`
- `MG_Rose_Sprout_V01.png`
- `MG_Rose_Bud_V01.png`
- `MG_Rose_Bloom_Red_V01.png`
- `MG_Rose_FullBloom_Red_V01.png`

### 19.4 Plant Growth Stages

| Stage | Visual Description | Knowledge Percentage |
|---|---|---|
| Seed | Small seed in soil, barely visible | 0-10% |
| Sprout | Tiny green shoot, 1-2 leaves | 10-25% |
| Bud | Stem with developing bud | 25-50% |
| Bloom | Open flower, vibrant colors | 50-80% |
| FullBloom | Fully opened flower with glow/particles | 80-100% |

### 19.5 Plant Type to Subject Mapping

| Plant Type | Associated Subject | Color Palette |
|---|---|---|
| Rose | Quran memorization | Red, deep green |
| Tulip | Hadith memorization | Gold, orange |
| Lotus | Fiqh knowledge | Pink, soft blue |
| Jasmine | Language and literature | White, pale yellow |
| Lavender | Science and nature | Purple, soft green |
| Sunflower | History and Seerah | Yellow, warm brown |
| PalmSeedling | General knowledge progression | Green, gold |

---

## 20. Achievements

### 20.1 Purpose and Scope

The `Achievements/` directory stores all visual assets for the achievement system: icons, badges, progress indicators, and reveal animations.

### 20.2 Subfolder Structure

```
Achievements/
|--- AchievementIcons/ (ByCategory, ByRarity)
|--- Badges/ (Tier01_Bronze, Tier02_Silver, Tier03_Gold, Tier04_Platinum, Tier05_Diamond, Tier06_Legendary)
|--- ProgressIndicators/ (Circular, Linear, StepBased, MilestoneMarkers, CompletionEffects)
|--- RevealAnimations/ (BadgeReveal, AchievementPopup, ProgressComplete, MilestoneUnlock, StreakFire, LevelUp)
```

### 20.3 Naming Convention

```
Ach_[Category]_[Tier]_[Name].[ext]
```

**Examples:**
- `Ach_Learning_Legendary_FirstQuranKhatm.png`
- `Ach_Streak_Gold_30DayStreak.png`
- `Ach_Knowledge_Epic_1000Nodes.png`

### 20.4 Badge Tier Visual Specifications

| Tier | Metal | Icon Treatment | Background | Effects |
|---|---|---|---|---|
| T01 Bronze | Bronze | Single tone, no glow | Matte | None |
| T02 Silver | Silver | Two-tone, subtle shimmer | Satin | Subtle reflection |
| T03 Gold | Gold | Polished, warm glow | Metallic | Gentle shine |
| T04 Platinum | Platinum | Cool tone, bright | Iridescent | Light particles |
| T05 Diamond | Diamond | Faceted, rainbow refractions | Crystal | Sparkle effects |
| T06 Legendary | Legendary | Animated, unique design | Ethereal | Full particle aura |

---

## 21. Rewards

### 21.1 Purpose and Scope

The `Rewards/` directory stores all visual assets for the reward system: icons, celebration animations, particle effects, and certificates.

### 21.2 Subfolder Structure

```
Rewards/
|--- RewardIcons/ (Coins, Gems, Stars, PointsBadges, TreasureChests, Keys, Scrolls, SpecialRewards)
|--- Animations/ (CoinCollect, GemBounce, StarExplosion, Burst, Confetti, RibbonUnfold, GiftOpen, TreasureReveal, LevelUpEffect)
|--- Effects/
|   |--- Confetti/ (Gold, Mixed, Colorful, Sparkle)
|   |--- Stars/ (Burst, Fall, Orbit, Explosion)
|   |--- Bursts/ (Small, Medium, Large, Celebration)
|   |--- Sparkles/ (Trail, Fountain, Twinkle, Swirl)
|--- Certificates/ (Templates, Customizable, Backgrounds, Borders, Seals, Signatures)
```

### 21.3 Naming Convention

```
Reward_[Type]_[Tier]_[Variant].[ext]
```

**Examples:**
- `Reward_Icon_Coins_Gold_V01.png`
- `Reward_Animation_CoinCollect_V01.json`
- `Reward_Effect_Confetti_Gold_V01.json`
- `Reward_Certificate_Completion_V01.png`

### 21.4 Effect Animation Specifications

| Effect | Duration | Particle Count | Size | Colors |
|---|---|---|---|---|
| Confetti Gold | 3s | 200-500 | 8-32px | Gold, amber, warm white |
| Stars Burst | 1.5s | 30-80 | 16-64px | Gold, white |
| Burst Small | 0.5s | 20 | 8-16px | Single color |
| Burst Large | 1.2s | 100 | 8-32px | Multiple colors |
| Sparkle Trail | 1s | 10-20 trail | 4-8px | Gold, white |

---

## 22. Loading Assets

### 22.1 Purpose and Scope

The `Loading/` directory stores all visual assets for loading states: mode-specific screens, general indicators, and animated transitions.

### 22.2 Subfolder Structure

```
Loading/
|--- ModeSpecific/ (Story, Quiz, Flashcards, Documentary, Podcast, Map, Timeline, AI, Memory, KnowledgeTree, Garden)
|--- General/ (Spinners, ProgressBars, Skeletons, PulseRings, Dots, Wave, Orbital)
|--- Animations/ (BookOpening, LampLighting, KnowledgeOrb, StarSpin, CrescentSpin, QuillWriting, ScrollUnrolling, CompassPointing)
|--- SkeletonScreens/ (Dashboard, Content, Search, Profile)
```

### 22.3 Naming Convention

```
Loading_[Mode]_[Variant].[ext]
```

### 22.4 Loading Time Targets

| Context | Maximum Load Time | Loading Treatment |
|---|---|---|
| Instant transition | < 200ms | No loading state needed |
| Fast load | 200ms - 1s | Skeleton screen |
| Standard load | 1s - 3s | Animated loading with brand asset |
| Slow load | 3s - 10s | Animated loading with progress indication |
| Very slow load | > 10s | Animated loading with activity indicator and quote |

---

## 23. Error Assets

### 23.1 Purpose and Scope

The `Errors/` directory stores all visual assets for error states. Visual treatment must be soothing and helpful.

### 23.2 Subfolder Structure

```
Errors/
|--- Illustrations/
|   |--- 404/ (LostScholar, MissingPage, BookNotFound, BrokenBookmark)
|   |--- Offline/ (WorldDisconnected, KnowledgeIsland, LanternAlone)
|   |--- Timeout/ (Hourglass, SlowQuill, WaitingLamp)
|   |--- Generic/ (KnowledgeMishap, ScrollTorn, InkSpill)
|   |--- ServerError/ (LibraryClosed, ScrollRestoring, KnowledgeResting)
|   |--- Permission/ (LockedBook, SealedScroll, GuardedKnowledge)
|--- Animations/ (PageTurn_Error, FadeOut, Shake, GlowDim, ReconnectPulse)
|--- CTAs/ (RetryButton, GoHome, TryAgain, ContactSupport)
```

### 23.3 Naming Convention

```
Error_[Type]_[Variant].[ext]
```

**Examples:**
- `Error_404_LostScholar_V01.png`
- `Error_Offline_KnowledgeIsland_V01.png`
- `Error_Timeout_Hourglass_V01.png`

### 23.4 Error Visual Tone Guidelines

| Error Type | Tone | Color Palette | Illustration Theme |
|---|---|---|---|
| 404 | Playful, reassuring | Warm neutrals, amber | Lost in a library |
| Offline | Calm, patient | Soft blues, cool grays | Island of knowledge |
| Timeout | Gentle, unhurried | Warm earth tones | Hourglass, slow quill |
| Generic | Supportive, problem-solving | Warm brand colors | Book mishap, ink spill |
| Server Error | Reassuring, temporary | Soft golds, warm browns | Library closed for restoration |
| Permission | Respectful, aspirational | Deep blues, gold accents | Locked book, sealed knowledge |

---

## 24. Empty State Assets

### 24.1 Purpose and Scope

The `EmptyStates/` directory stores all visual assets for empty states. Must feel inviting and aspirational.

### 24.2 Subfolder Structure

```
EmptyStates/
|--- Illustrations/
|   |--- NoResults/ (EmptySearch, NoBooksFound, EmptyShelf, MagnifyingGlass)
|   |--- NoBookmarks/ (EmptyBookmark, RibbonUnplaced, OpenBook_NoMarks, ReadingRibbon)
|   |--- NoProgress/ (EmptyGarden, BareTree, NoMilestones, UntroddenPath)
|   |--- FirstVisit/ (WelcomeLamp, OpenDoor, FirstBook, Compass, LanternPath)
|   |--- NoAchievements/ (EmptyTrophyCase, ShieldUnowned, MedalNotYet)
|   |--- NoConnections/ (IsolatedTree, SingleNode, UnconnectedPath)
|--- Animations/ (GentleFloat, SubtleGlow, SoftPulse, SlowReveal, InviteGesture)
|--- CTAs/ (PrimaryAction, SecondaryAction, LearnMore, GetStarted, ImportAction, ExploreCTA)
```

### 24.3 Naming Convention

```
Empty_[Context]_[Variant].[ext]
```

**Examples:**
- `Empty_NoResults_EmptySearch_V01.png`
- `Empty_NoBookmarks_RibbonUnplaced_V01.png`
- `Empty_NoProgress_EmptyGarden_V01.png`
- `Empty_FirstVisit_WelcomeLamp_V01.png`

### 24.4 Empty State Visual Guidelines

| Context | Message | Visual Theme | CTA Strategy |
|---|---|---|---|
| No Results | Refine or expand search | Empty shelf, magnifying glass | Suggest popular searches |
| No Bookmarks | Start saving content | Unplaced ribbon, open book | Highlight bookmark feature |
| No Progress | Begin your learning journey | Empty garden, bare tree | Start first lesson CTA |
| First Visit | Welcome to knowledge | Lamp, open door, compass | Onboarding flow CTA |
| No Achievements | Start earning | Empty trophy case | Show first achievement CTA |
| No Connections | Build your network | Isolated node, single tree | Social features CTA |

---

## 25. Metadata Schema

### 25.1 Purpose and Scope

Every asset in the library MUST have an associated `.asset.json` metadata file.

### 25.2 Required Metadata Fields

| Field | Type | Required | Description | Example |
|---|---|---|---|---|
| AssetID | UUID | Yes | Universally unique identifier | `a1b2c3d4-e5f6-7890-abcd-ef1234567890` |
| Name_AR | String | Yes | Asset name in Arabic | `مشهد معركة بدر` |
| Name_EN | String | Yes | Asset name in English | `Battle of Badr Scene` |
| Category | String | Yes | Top-level category | `Illustrations` |
| Subcategory | String | Yes | Second-level category | `Scenes` |
| SubSubcategory | String | No | Third-level category | `Historical` |
| Type | String | Yes | Asset type within category | `Scene` |
| Description_AR | Text | Yes | Description in Arabic | `مشهد يصور معركة بدر من منظور واسع` |
| Description_EN | Text | Yes | Description in English | `Wide angle scene depicting the Battle of Badr` |
| Keywords_AR | Array | Yes | Search keywords in Arabic | `["معركة", "بدر", "المسلمين", "تاريخ"]` |
| Keywords_EN | Array | Yes | Search keywords in English | `["battle", "badr", "muslims", "history"]` |
| Version | String | Yes | Asset version number | `V01` |
| Author | String | Yes | Creator name or ID | `artist_name` |
| AuthorEmail | String | No | Creator email | `artist@domain.com` |
| CreationDate | Date | Yes | Date of creation | `2026-07-10` |
| LastModified | Date | Yes | Date of last modification | `2026-07-15` |
| Source | String | No | Source application | `Procreate` |
| Software | String | No | Software version | `Procreate 5.3` |
| License | String | Yes | License type | `Internal Use Only` |
| EducationalLevel | Array | No | Target educational levels | `["beginner", "intermediate"]` |
| LearningObjective | String | No | Associated learning objective | `Understand the events of the Battle of Badr` |
| HistoricalPeriod | String | No | Historical period depicted | `Prophetic Era (622-632 CE)` |
| Region | String | No | Geographic region | `Arabian Peninsula - Hejaz` |
| Language | String | No | Language of text in asset | `AR` |
| AccessibilityNotes | Text | No | Accessibility description | `High contrast, clear silhouettes` |
| FileFormat | String | Yes | File extension | `png` |
| Resolution | String | Yes | Resolution designation | `4K` |
| PixelWidth | Integer | Yes | Width in pixels | `3840` |
| PixelHeight | Integer | Yes | Height in pixels | `2160` |
| FileSize | Integer | Yes | File size in bytes | `2457600` |
| ColorSpace | String | Yes | Color space | `sRGB` |
| ColorProfile | String | No | ICC profile name | `sRGB IEC61966-2.1` |
| DPI | Integer | No | Dots per inch | `300` |
| UsageRights | String | Yes | Usage rights category | `Platform Internal` |
| Status | String | Yes | Current workflow status | `approved` |
| VariantOf | UUID | No | Parent asset ID if variant | `b2c3d4e5-f6a7-8901-bcde-f12345678901` |
| AssociatedProject | String | No | Project or story ID | `STR_001` |
| Tags_AR | Array | No | Additional Arabic tags | `["الحجاز", "السنة الثانية للهجرة"]` |
| Tags_EN | Array | No | Additional English tags | `["hejaz", "second year hijri"]` |
| ThumbnailPath | String | No | Relative path to thumbnail | `_thumbnails/Scene_BattleOfBadr_thumb.png` |
| BlurHash | String | No | BlurHash for progressive loading | `LGF5]+Yk^6#M@-5c,1J5@[or[Q6.` |
| CDNUrl | String | No | CDN URL after deployment | `https://cdn.domain.com/assets/...` |
| CDNUrls | Object | No | Map of format to CDN URL | `{"webp": "...", "avif": "..."}` |
| Checksum | String | No | File checksum (SHA-256) | `e3b0c44298fc...` |
| ReviewNotes | Text | No | Notes from review process | `Adjusted color balance per QC feedback` |
| Approver | String | No | Approver name/ID | `art_director_name` |
| ApprovalDate | Date | No | Date of approval | `2026-07-20` |

### 25.3 Status Values

| Status | Description | Visibility |
|---|---|---|
| draft | Initial creation, not ready for review | Creator only |
| review | Submitted for quality control | Art Director, Reviewer |
| approved | Passed QC, ready for production | All production staff |
| published | Deployed to CDN, live in production | All staff, public |
| archived | Deprecated or replaced | Admin only (hidden by default) |

### 25.4 Metadata Validation Rules

| Rule | Description | Error Handling |
|---|---|---|
| AssetID uniqueness | No two assets may share the same UUID | Reject on pipeline ingestion |
| Required fields | All Required=true fields must be present | Pipeline blocks ingestion |
| Status transition | Status must follow defined workflow | Invalid transition rejected |
| File extension match | FileFormat must match actual file extension | Warning on mismatch |
| Version format | Version must follow V## pattern | Pipeline enforces format |
| Date format | All dates in ISO 8601 (YYYY-MM-DD) | Pipeline validates format |
| Array fields | Keywords and Tags must be arrays | Pipeline casts to array |

### 25.5 Auto-Generated Metadata Fields

The pipeline automatically generates: AssetID (UUID v4), CreationDate, LastModified, Status (draft), FileSize, PixelWidth, PixelHeight, ColorSpace, Checksum (SHA-256), ThumbnailPath, BlurHash, CDNUrl (after deployment).

---

## 26. Asset Naming Convention

### 26.1 Universal Pattern

```
[CategoryPrefix]_[SubCategory]_[Description]_[Language]_[Variant]_[Resolution].[ext]
```

### 26.2 Rules

| Rule | Description |
|---|---|
| Case | All lowercase |
| Separator | Underscore `_` between tokens |
| Spaces | No spaces -- use underscores |
| Special characters | No special characters except underscore and period |
| Maximum length | 100 characters (excluding extension) |
| Description | Transliterated Arabic or English, no diacritics |
| Numbers | Use digits (0-9) only, no Roman numerals |
| Version suffix | Always V## format (V01, V02, V12) |
| Resolution | Use standard designations: 4K, 2K, HD, Web, Thumb |

### 26.3 Category Prefixes

| Category | Prefix | Example |
|---|---|---|
| Illustrations | (Category name as prefix) | `Scene_Historical_BattleOfBadr_Wide_EN_V01_4K.png` |
| Characters | `Char_` | `Char_Avatar_DefaultBase_T01_Happy_V01.fbx` |
| Story | `[StoryID]_` | `STR_001_Scene_012_03_ProphetEntersCave_V01.png` |
| Documentary | `[DocID]_` | `DOC_001_KeyArt_HeroImage_EN_V01.png` |
| Maps | `Map_` | `Map_ArabianPeninsula_PropheticEra_Political_Medium_V01.svg` |
| Icons | `Icon_` | `Icon_Navigation_Home_24_Default.svg` |
| Backgrounds | `BG_` | `BG_Story_Dark_LibraryStudy_HD.png` |
| Patterns | `Pattern_` | `Pattern_Geometric_StarTessellation_Gold_Tileable.svg` |
| 3D | `3D_` | `3D_Building_GreatMosqueKairouan_LOD0_V01.fbx` |
| Animations | `Anim_` | `Anim_Avatar_Walk_Normal_V01.fbx` |
| Videos | `Video_` | `Video_Story_001_005_V01_Delivery.mp4` |
| Audio | `Audio_` | `Audio_Interface_ButtonTap_V01.wav` |
| Narration | `Narration_` | `Narration_AR_KhalidAlRashid_Story_STR_001_S012_03_BestTake.wav` |
| Ambient | `Ambient_` | `Ambient_Library_Loopable_ReadingRoom_120s.wav` |
| UI | `UI_` | `UI_Component_Button_Primary_Default_Web.svg` |
| Avatar | `Avatar_` | `Avatar_T03_BaseModel_V01.fbx` |
| KnowledgeTree | `KT_` | `KT_S03_TreeModel_Mature_V01.fbx` |
| MemoryGarden | `MG_` | `MG_Rose_Bloom_Red_V01.png` |
| Achievements | `Ach_` | `Ach_Learning_Legendary_FirstQuranKhatm.png` |
| Rewards | `Reward_` | `Reward_Icon_Coins_Gold_V01.png` |
| Loading | `Loading_` | `Loading_Story_V01.png` |
| Errors | `Error_` | `Error_404_LostScholar_V01.png` |
| Empty States | `Empty_` | `Empty_NoResults_EmptySearch_V01.png` |

### 26.4 Resolution Designations

| Designation | Description | Approximate Pixels |
|---|---|---|
| `Thumb` | Thumbnail | 150 x 150 |
| `Web` | Standard web delivery | 1920 x 1080 |
| `HD` | High definition (Retina) | 2560 x 1440 |
| `2K` | 2K resolution | 2048 x 1556 |
| `4K` | 4K ultra high definition | 3840 x 2160 |
| `8K` | 8K archival master | 7680 x 4320 |
| `Tile` | Tileable pattern size | 512 x 512 |

### 26.5 Language Codes

| Code | Language |
|---|---|
| `EN` | English |
| `AR` | Arabic (Modern Standard) |
| `AR-GL` | Arabic (Gulf) |
| `AR-EG` | Arabic (Egyptian) |
| `AR-LV` | Arabic (Levantine) |
| `EN-UK` | English (British) |
| `FR` | French |
| `UR` | Urdu |
| `TR` | Turkish |
| `MS` | Malay |
| `ID` | Indonesian |
| `FA` | Persian |
| `SW` | Swahili |
| `HA` | Hausa |
| `BN` | Bengali |
| (blank) | Universal/Non-linguistic asset |

### 26.6 Naming Examples by Category

**Illustrations:** `Scene_Historical_BattleOfBadr_Wide_EN_V01_4K.png`, `Character_Abbasid_ScholarInLibrary_FullBody_AR_V02_HD.png`, `Environ_Desert_OasisAtDusk_Wide_V01_4K.png`, `Object_Astrolabe_Islamic_V01_HD.png`, `Infographic_Timeline_Caliphs_EN_V01_4K.png`

**Characters:** `Char_Avatar_DefaultBase_T01_Happy_V01.fbx`, `Char_Avatar_DefaultBase_T01_Happy_V01.png`, `Char_Spirit_Ethereal_T03_Glowing_V02.png`, `Char_Historical_IbnSina_T02_Contemplative_V01.png`

**Story:** `STR_001_Scene_012_03_ProphetEntersCave_V01.png`, `STR_001_Char_012_01_YoungProphet_V02.png`, `STR_001_BG_012_01_CaveInterior_V01.exr`

**Documentary:** `DOC_001_Thumbnail_Episode01_EN_V01.png`, `DOC_001_KeyArt_HeroImage_EN_V02.png`, `DOC_001_Infographic_ScientificAchievements_EN_V01.svg`

**Maps:** `Map_ArabianPeninsula_PropheticEra_Political_Medium_V01.svg`, `Map_Andalusia_Nasrid_Topographic_Detail_V01.tif`, `Map_World_HighMedieval_TradeRoutes_Large_V02.png`

**Icons:** `Icon_Navigation_Home_24_Default.svg`, `Icon_Navigation_Home_24_Active.svg`, `Icon_Navigation_Home_24_Disabled.svg`, `Icon_Actions_Play_48_Active.svg`

**Backgrounds:** `BG_Story_Dark_LibraryStudy_HD.png`, `BG_Quiz_Animated_Celebration_HD.webp`, `BG_KnowledgeTree_Dark_Canopy_4K.png`

**Patterns:** `Pattern_Geometric_StarTessellation_Gold_Tileable.svg`, `Pattern_Arabesque_OttomanScroll_Gold_FullSize.png`, `Pattern_Calligraphic_KuficBismillah_Gold_Tileable.svg`

**3D:** `3D_Building_GreatMosqueKairouan_LOD0_V01.fbx`, `3D_Building_GreatMosqueKairouan_Texture_Albedo_4K_V01.png`, `3D_Artifact_Astrolabe_LOD0_V01.gltf`

**Animations:** `Anim_Avatar_Walk_Normal_V01.fbx`, `Anim_UI_Micro_ButtonPress_Normal_V01.json`, `Anim_Loading_KnowledgeOrb_Normal_V01.mp4`

**Videos:** `Video_Story_001_005_V01_Master.mov`, `Video_Story_001_005_V01_Delivery.mp4`, `Video_Doc_003_012_V02_Delivery.mp4`

**Audio:** `Audio_Interface_ButtonTap_V01.wav`, `Audio_Feedback_Correct_V01.wav`, `Audio_Music_Study_CalmLoop_V01.wav`

**Narration:** `Narration_AR_KhalidAlRashid_Story_STR_001_S012_03_BestTake.wav`, `Narration_EN_SarahJohnson_Doc_DOC_003_Ch02_T01.wav`

**Ambient:** `Ambient_Library_Loopable_ReadingRoom_120s.wav`, `Ambient_Desert_FullExperience_Day_300s.wav`

---

## 27. Version Control

### 27.1 Repository Structure

The Asset Library is version-controlled using Git with Git LFS for all binary assets.

- **Repository:** `git.internal.domain.com/visual-production/asset-library`
- **Git LFS tracking:** .png, .svg, .fbx, .psd, .tif, .wav, .mp4, .mov, .exr, .blend, .ma, .mb, .sbsar, .pat, .ai, .json
- **Repository size limit:** 10 GB (soft) / 50 GB (hard) for LFS storage

### 27.2 Branching Strategy

| Branch | Purpose | Protection Rules |
|---|---|---|
| `main` | Approved, production-ready assets | Write-protected; merge only via PR with 2 approvals |
| `development` | Work in progress | Write for all artists; automated QC checks on push |
| `review` | Assets pending approval | Auto-populated from development on submission |

Additional branch types: `feature/[category]/[description]`, `fix/[category]/[description]`, `release/[version-number]`.

### 27.3 Commit Message Convention

```
[Category] [Action] [Asset Name] [Version]
```

**Examples:**
- `[Illustrations] ADD Scene_Historical_BattleOfBadr_V01`
- `[3D] UPDATE 3D_Building_GreatMosqueKairouan_LOD0 - Fixed texture mapping`
- `[Icons] BATCH Icon_Navigation_Home - Added all size variants`

### 27.4 Tagging Convention

Tags follow semantic versioning: `v[Major].[Minor].[Patch]`.
- Major: Breaking structural changes
- Minor: New asset categories or significant additions
- Patch: Asset updates, fixes, metadata corrections

Asset-specific tags: `assets/[category]/[asset-name]/v[version]`

### 27.5 Changelog Requirements

Every version tag must have a corresponding `CHANGELOG.md` entry with Added, Changed, Fixed, Deprecated, and Removed sections.

### 27.6 Rollback Procedure

1. Revert the specific commit using `git revert [commit-hash]`
2. Tag the rollback: `v[version]-rollback-[date]`
3. Notify stakeholders
4. Trigger CDN cache invalidation for reverted assets
5. Document issue and implement preventive validation

### 27.7 Git LFS Configuration

All binary asset types are tracked via `.gitattributes` with `filter=lfs diff=lfs merge=lfs -text` for each extension.

---

## 28. Access Control

### 28.1 Role-Based Access Model

| Role | Read | Create | Edit | Delete | Approve | Manage Permissions |
|---|---|---|---|---|---|---|
| Artist | Yes | Yes (own) | Yes (own) | No | No | No |
| Art Director | Yes | Yes | Yes | No | Yes | No |
| Reviewer | Yes | No | No | No | No | No |
| Approver | Yes | No | No | No | Yes | No |
| Admin | Yes | Yes | Yes | Yes | Yes | Yes |

### 28.2 Permission by Category

Each category follows the general matrix: Artists can create and edit their own assets. Art Directors have full control over all assets. Reviewers read and comment only. Approvers sign off. Admins have absolute control.

### 28.3 External Contributor Access

- Temporary account for contract duration
- Category-limited access only
- Time-bound with automatic expiry
- Review-required on all submissions
- Watermarked until approved
- No delete access

### 28.4 Access Grant Process

1. Request submitted via Asset Library web interface
2. Art Director approves category-level access
3. Admin creates/updates account
4. User notified with access details
5. Access logged in audit trail

### 28.5 Access Audit

- All access logged with timestamp, user, action, and asset
- Logs retained for minimum 2 years
- Monthly access reports generated
- Quarterly access re-validation required
- Suspicious patterns trigger automated alerts

### 28.6 Temporary Access Grants

- Maximum 72 hours duration
- Single category or specific asset set scope
- Same-day Art Director approval required
- Auto-revoke at expiry

---

## 29. CDN Integration

### 29.1 Delivery Pipeline Overview

Pipeline stages triggered on every commit to `main`:
1. Asset validation (metadata and file integrity)
2. Format generation (WebP, AVIF, responsive variants)
3. Compression per quality targets
4. LQIP generation for all raster assets
5. Upload to geo-distributed CDN nodes
6. Cache invalidation for updated assets
7. Verification of asset availability at CDN edge

### 29.2 Auto-Sync from Approved Library

| Trigger | Action | Scope |
|---|---|---|
| Commit to main | Full sync | All changed assets |
| Tag creation | Full sync | All assets (release versions) |
| Scheduled sync (daily) | Incremental sync | Assets modified in last 24h |
| Manual trigger | Selective sync | Specific assets or categories |

### 29.3 Cache Invalidation

Triggered when: metadata `LastModified` changes, new version committed, asset status changes to `archived`, manual request submitted. Invalidates exact path, all format variants, and optionally entire category.

### 29.4 Format Negotiation

The CDN delivers the optimal format based on the requesting client's `Accept` header:

| Client Support | Delivered Format | Compression vs PNG |
|---|---|---|
| AVIF supported | AVIF | 50% smaller than WebP |
| WebP supported | WebP | 30% smaller than PNG |
| Legacy browser | PNG | Baseline |
| SVG supported | SVG (for vector assets) | Scalable |

### 29.5 Compression Pipeline

| Asset Type | Format | Quality Setting | Method |
|---|---|---|---|
| Photographs/Scenes | JPEG XL / WebP | Q 85 (lossy) | Perceptual optimization |
| Illustrations | WebP / PNG | Q 90 (lossy) / Lossless | PNGQuant for PNG |
| Icons | SVG | N/A | SVGO optimization |
| 3D Textures | WebP | Q 92 (lossy) | Perceptual optimization |
| Patterns | WebP / PNG | Q 90 (lossy) / Lossless | PNGQuant for PNG |

### 29.6 LQIP Generation

All raster assets get Low Quality Image Placeholders:
- Resolution: 32 x 18 pixels (blown up with CSS)
- Format: JPEG, quality 20
- BlurHash string stored in metadata
- Base64 inline data URI for instant display

### 29.7 Responsive Image Sets

Each raster asset generates a responsive set at these widths:
- 320px (mobile small)
- 640px (mobile large)
- 768px (tablet portrait)
- 1024px (tablet landscape)
- 1440px (desktop)
- 1920px (desktop Retina)
- 3840px (Ultra HD)

### 29.8 Geo-Distribution Strategy

| Region | CDN Node Locations | Priority |
|---|---|---|
| Middle East & North Africa | Dubai, Riyadh, Cairo, Istanbul | Primary |
| Europe | London, Frankfurt, Paris, Amsterdam | Secondary |
| South Asia | Mumbai, Singapore, Jakarta | Secondary |
| North America | New York, San Francisco, Dallas | Tertiary |
| Sub-Saharan Africa | Johannesburg, Lagos, Nairobi | Tertiary |

---

## Document Control

| Version | Date | Author | Changes |
|---|---|---|---|
| 1.0 | 2026-07-10 | Visual Production Directorate | Initial document creation |

