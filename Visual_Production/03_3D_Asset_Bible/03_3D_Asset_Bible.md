# 3D Asset Bible

**Document Version:** 1.0.0
**Last Updated:** 2026-07-10
**Classification:** Enterprise Internal — 3D Production Reference
**Owner:** Director of 3D Production / Technical Art Team

---

## Table of Contents

1. [3D Production Philosophy](#1-3d-production-philosophy)
2. [Technical Specifications](#2-technical-specifications)
3. [Historical Buildings](#3-historical-buildings)
4. [Historical Locations](#4-historical-locations)
5. [Maps & Terrain](#5-maps--terrain)
6. [Terrain & Landscape](#6-terrain--landscape)
7. [Artifacts](#7-artifacts)
8. [Books & Scrolls](#8-books--scrolls)
9. [Libraries & Museums](#9-libraries--museums)
10. [Museum Objects](#10-museum-objects)
11. [Interactive Models](#11-interactive-models)
12. [Knowledge Tree (3D)](#12-knowledge-tree-3d)
13. [Avatar (3D)](#13-avatar-3d)
14. [Knowledge Spirit (3D)](#14-knowledge-spirit-3d)
15. [Memory Garden (3D)](#15-memory-garden-3d)
16. [Educational Simulations](#16-educational-simulations)
17. [Material Library](#17-material-library)
18. [Lighting Guide](#18-lighting-guide)
19. [Optimization Guide](#19-optimization-guide)
20. [Production Pipeline](#20-production-pipeline)

**Cross-References:**
- Visual_Production/00_Visual_Asset_Audit/00_Visual_Asset_Audit.md
- Visual_Production/01_Visual_Production_Bible/01_Visual_Production_Bible.md
- Visual_Production/02_Illustration_Bible/02_Illustration_Bible.md
- Visual_Production/04_Animation_Bible/04_Animation_Bible.md
- Visual_Production/05_Video_Production_Bible/05_Video_Production_Bible.md
- Visual_Production/06_Audio_Production_Bible/06_Audio_Production_Bible.md
- Visual_Production/07_Asset_Pipeline/07_Asset_Pipeline.md
- Visual_Production/08_Quality_Control/08_Quality_Control.md
- Visual_Production/09_AI_Asset_Generation/09_AI_Asset_Generation.md
- Visual_Production/10_Asset_Library/10_Asset_Library.md
- UX_Design/07_Illustration_Bible/07_Illustration_Bible.md

---

## 1. 3D Production Philosophy

### 1.1 Purpose of 3D in the Platform

3D assets serve five core pedagogical and experiential purposes within the platform. Immersion draws learners into historically accurate environments that cannot be physically visited, creating presence across time and geography. Exploration enables self-directed discovery within 3D spaces where learners navigate, inspect, and interact with environments and objects at their own pace. Visualization transforms abstract historical, scientific, and mathematical concepts into tangible three-dimensional forms that can be rotated, dissected, and examined from any angle. Personalization manifests the learner's progress, identity, and achievements through the avatar, knowledge spirit, knowledge tree, and memory garden — each evolving uniquely per user. Demonstration powers educational simulations where learners observe and manipulate mechanical, astronomical, anatomical, and chemical systems in real-time 3D.

Every 3D asset on the platform must map to at least one of these five purposes. Assets that serve only decorative function are rejected during the concept review phase. The 3D team evaluates each proposed asset against the purpose matrix before any modeling begins.

### 1.2 Balance Between Visual Quality and Performance

The platform operates on a mobile-first principle. 3D assets are authored for high-end desktop but must function seamlessly on mobile devices with GPU constraints, thermal limits, and battery considerations. Quality targets are defined per asset tier with strict poly count, texture resolution, and draw call budgets. The rule is: author in high fidelity, optimize down to target tier. Never author below target quality expecting to add detail later — detail is easier to remove than to create.

The visual target is not photorealism. Photorealism requires unbounded compute and creates uncanny-valley risks for character assets. Instead, the platform targets a warm, inviting stylized-realistic hybrid: believable form and proportion with optimized surface treatment. This stylized approach delivers higher perceived quality at lower polygon counts because the brain accepts stylized representations more readily than imperfect realism. A stylized mosque with clean geometry and warm materials reads as more beautiful than a photorealistic mosque with noisy textures and visible polygon edges.

### 1.3 Stylized Aesthetic: Warm, Inviting 3D World

The platform's 3D aesthetic occupies a defined visual territory. Form follows historical reference with artistic interpretation — buildings are structurally accurate but simplified for readability. Materials use PBR (Physically Based Rendering) workflows but with exaggerated warm undertones. The world feels curated and gentle rather than harsh or sterile. Light is always warm. Shadows are soft. Colors are saturated but not garish. The aesthetic communicates: this is a place of learning, beauty, and welcome.

Stylization guidelines: edge bevels are visible (1-3 degree chamfers) to catch light and define silhouette. Texture detail is present but not noisy — clear readable surfaces over hyper-detailed grunge. Atmospheric fog is warm-tinted and present in all exterior scenes. The color palette derives from the design system: amber, terracotta, sage, teal, cream, and gold as primary 3D material colors. Pure black is never used for shadows or materials — deepest shadows use desaturated warm brown (#2A1A0A) or deep teal (#1A2A3A).

### 1.4 Cultural Authenticity Mandate

All historical 3D assets must be culturally and historically accurate. This is not optional. Every building, artifact, environment, and character asset must be based on verifiable historical references — archaeological photographs, architectural drawings, museum documentation, academic papers, and consultation with subject matter experts. The 3D team maintains a reference library organized by period, region, and building type.

Cultural sensitivity review is required for all assets depicting religious structures, sacred objects, or culturally significant sites. The review process includes internal cultural advisors and, where possible, external consultants from relevant cultural heritage organizations. Inscriptions, calligraphy, and decorative patterns must be accurately rendered — generic Islamic-looking decoration is strictly prohibited. Specific historical patterns, verses, and motifs must be used with correct placement and orientation.

### 1.5 Accessibility in 3D

3D assets must be designed with accessibility considerations. Color contrast must be sufficient for visually impaired users in all UI-over-3D scenarios. Motion intensity must respect reduced-motion settings — idle animations, camera rotations, and particle effects should have configurable intensity. Text descriptions must accompany all significant 3D objects for screen reader compatibility. Navigation paths in 3D environments must accommodate users who cannot perform fine-grained touch or mouse interactions. Hotspots and interactive zones must have minimum target sizes of 48x48 effective pixels.

---

## 2. Technical Specifications

### 2.1 Target Platforms

| Platform | Min Spec | Target Spec | 3D Renderer | Considerations |
|----------|----------|-------------|-------------|---------------|
| iOS | iPhone 8 (A11 Bionic) | iPhone 13+ (A15+) | Metal via Unity | Thermal throttling, battery drain |
| Android | Snapdragon 845 / Mali-G72 | Snapdragon 8 Gen 1+ / Mali-G710 | Vulkan via Unity | Device fragmentation, driver bugs |
| Web (Desktop) | WebGL 2.0, 4GB RAM | WebGL 2.0, 8GB RAM | Three.js / WebGPU | No native compression, download size |
| Web (Mobile) | WebGL 2.0, 3GB RAM | WebGL 2.0, 6GB RAM | Three.js / WebGPU | Bundle size <15MB, texture streaming |

Unity is the primary 3D engine for native mobile builds. Three.js / WebGL is the secondary runtime for web-based experiences. USDZ is used for AR Quick Look on iOS. glTF is the interchange format between tools and runtimes. FBX is the authoring format from DCC tools (Blender, Maya, 3ds Max) into the pipeline.

### 2.2 Poly Count Budgets Per Asset Tier

Five asset tiers define polygon budgets across all 3D content. Tiers are assigned during concept review based on the asset's screen presence, interactive importance, and proximity to the camera.

| Tier | Classification | Max Polygons (Tris) | Example Assets | LOD Count | Screen Coverage |
|------|---------------|---------------------|----------------|-----------|-----------------|
| Tier 1 | Hero | 50,000 | Avatar character, Hero buildings, Interactive artifacts | 4 LODs | >30% screen |
| Tier 2 | Important | 20,000 | Historical buildings, Key artifacts, NPC characters | 4 LODs | 15-30% screen |
| Tier 3 | Standard | 8,000 | Secondary buildings, Environment props, Furniture | 3 LODs | 5-15% screen |
| Tier 4 | Background | 3,000 | Distant buildings, Terrain chunks, Crowd characters | 2 LODs | 2-5% screen |
| Tier 5 | Distant | 500 | Billboard replacements, Impostor geometry, Sky elements | 1 LOD | <2% screen |

Polygon budgets are measured in triangles (tris). Quad-based modeling is converted to tris for budget counting. A single triangle is the atomic unit — vertex counts should be monitored but triangle count is the primary metric. Budgets include all geometry within the asset including collision meshes, LOD meshes are additional but each LOD must stay within progressively reduced budgets (LOD1: 50% of base, LOD2: 20% of base, LOD3: 5% of base, LOD4: 1% of base).

### 2.3 Texture Resolution Guidelines

Texture resolution is governed by asset tier and texture type. All textures are power-of-two (POT) dimensions. Non-power-of-two textures are rejected during import QA.

| Asset Tier | Albedo | Normal | ORM (Occlusion/Roughness/Metalness) | Emission | Max Total Memory |
|------------|--------|--------|--------------------------------------|----------|-----------------|
| Tier 1 Hero | 2048x2048 | 2048x2048 | 2048x2048 | 1024x1024 | 32MB |
| Tier 2 Important | 1024x1024 | 1024x1024 | 1024x1024 | 512x512 | 8MB |
| Tier 3 Standard | 1024x1024 | 512x512 | 512x512 | 256x256 | 4MB |
| Tier 4 Background | 512x512 | 256x256 | 256x256 | None | 1MB |
| Tier 5 Distant | 256x256 | None | None | None | 0.25MB |

Texture format: ASTC 4x4 (mobile), BC7/BC3 (desktop), ETC2 (fallback). Textures are authored as 16-bit TIF source files, compressed during build. sRGB is enabled for albedo and emission maps; linear space for normal and ORM maps. Texture streaming is enabled for large environments — only textures in the current viewport are loaded at full resolution. Mipmaps are generated for all textures with a mip bias of -0.5 (slightly sharper) for high-end devices and +0.5 (slightly blurrier, better performance) for low-end devices.

### 2.4 Draw Call Budgets

| Scene Type | Max Draw Calls | Max Batches | Max Triangles Rendered |
|------------|---------------|-------------|----------------------|
| Museum/Interior (single room) | 150 | 50 | 200,000 |
| City environment (wide view) | 300 | 80 | 500,000 |
| Historical location (full scene) | 250 | 60 | 350,000 |
| Garden/Memory Garden | 100 | 30 | 150,000 |
| Interactive model/simulation | 80 | 25 | 100,000 |
| Avatar customization screen | 50 | 15 | 80,000 |

Static batching is enabled for all non-moving geometry. Dynamic batching is used for moving objects with shared materials. GPU instancing is used for repeated elements (columns, tiles, trees, crowd characters). Material atlasing is required for environments with more than 20 unique materials. Reduction target: combine materials wherever possible without sacrificing visual quality.

### 2.5 Material Complexity Limits

| Platform | Max Materials Per Asset | Max Shader Variants | Max Texture Samples Per Material |
|----------|------------------------|---------------------|----------------------------------|
| iOS (Metal) | 3 | 50 | 5 |
| Android (Vulkan) | 3 | 50 | 5 |
| Web (Three.js) | 2 | 30 | 4 |

Each material should use the Universal Render Pipeline (URP) Lit shader as the base. Custom shaders are permitted only for hero assets and must be approved by the Technical Art Director. All custom shaders must have a fallback to URP Lit. Material instances are preferred over unique materials — share material assets across objects that use the same surface properties, varying only via texture maps.

### 2.6 File Format Standards

| Stage | Format | Version | Compression | Notes |
|-------|--------|---------|-------------|-------|
| Authoring (source) | .blend, .mb, .max | Blender 3.6+, Maya 2024+, 3ds Max 2024+ | Uncompressed | Source files stored in Perforce |
| Interchange | .fbx | FBX 2020+ | Binary, embedded media | Universal pipeline transfer |
| Runtime (Web) | .gltf / .glb | glTF 2.0 | Draco mesh compression | WebGL/Mobile optimized |
| Runtime (iOS AR) | .usdz | USDZ 0.8+ | Uncompressed | AR Quick Look |
| Runtime (Unity) | .asset / .prefab | Unity 2022 LTS+ | Asset bundles | Addressable assets |
| Collision | .fbx (separate) | FBX 2020+ | Binary | Named _collision suffix |
| Rig/Animation | .fbx (separate) | FBX 2020+ | Binary | Named _rig and _anim suffixes |

Export settings are standardized: forward axis -Z, up axis +Y, units in meters, scale 1.0. All transforms must be baked before export. No vertex color data unless specifically required. No animation curves in static asset FBX files.

### 2.7 Unity Engine Standards

The platform uses Unity 2022 LTS (Long Term Support) as the primary 3D engine. Unreal Engine 5 is used for cinematic rendering of promotional materials and high-fidelity pre-rendered sequences only — not for runtime 3D.

Unity project structure for 3D assets:
```
Assets/
  03_3D_Assets/
    {Category}/
      {AssetName}/
        Models/          -- FBX and prefab files
        Materials/       -- Material instances
        Textures/        -- Texture maps
        Animations/      -- Animation clips
        Shaders/         -- Custom shaders (if approved)
        Prefabs/         -- Runtime prefab variants
```

Unity settings: Universal Render Pipeline (URP) 14+, Linear color space, Metal/Vulkan graphics APIs. Mesh compression: High for runtime meshes, Off for source meshes. Animation compression: Keyframe reduction with 1% tolerance.

### 2.8 LOD System Requirements

Level of Detail (LOD) is a mandatory system for all Tier 1-4 assets. The LOD system ensures that distant geometry consumes fewer resources while maintaining visual quality at the distances it is viewed.

LOD transition distances (relative to camera, assuming 60-degree FOV on a 6-inch mobile screen at 30cm viewing distance):

| Tier | LOD0 (Full) | LOD1 (Medium) | LOD2 (Low) | LOD3 (Very Low) | LOD4 (Impostor) |
|------|-------------|---------------|------------|-----------------|-----------------|
| Tier 1 | 0-10m | 10-25m | 25-50m | 50-100m | 100m+ |
| Tier 2 | 0-15m | 15-40m | 40-80m | 80-150m | 150m+ |
| Tier 3 | 0-20m | 20-50m | 50-120m | 120m+ | -- |
| Tier 4 | 0-30m | 30-80m | 80m+ | -- | -- |
| Tier 5 | Full only | -- | -- | -- | -- |

LOD transition must be cross-faded (dithering or alpha blending over 0.5 seconds) to prevent popping. LOD bias is adjustable per platform via the quality settings. LOD0 and LOD1 must have no visual difference detectable at normal viewing distance. LOD2 may have minor simplification (reduced mesh subdivisions, simplified materials). LOD3 may use flat-shaded geometry with baked normal detail into the albedo. LOD4 is a billboard quad with a rendered impostor texture, updated when the camera stops moving.

---

## 3. Historical Buildings

### 3.1 Building Classification System

All historical building assets are classified by architectural period, geographical region, building type, and tier assignment. This classification determines modeling approach, texture detail targets, and research requirements.

| Classification | Period Years | Regions | Key Architectural Features | Reference Priority |
|----------------|-------------|---------|--------------------------|-------------------|
| Umayyad | 661-750 | Syria, Spain, Andalusia | Horseshoe arches, large domes on squinches, mosaic decoration, hypostyle prayer halls | High |
| Abbasid | 750-1258 | Iraq, Persia, Central Asia | Pointed arches, muqarnas vaulting, massive scale, brick construction, stucco decoration | High |
| Fatimid | 909-1171 | Egypt, North Africa, Sicily | Keel arches, monumental portals, carved stone, wood ceilings, mausoleum domes | High |
| Mamluk | 1250-1517 | Egypt, Syria, Hejaz | Stone ablaq (color banding), complex dome profiles, minarets with balconies, madrasa-mosque complexes | High |
| Ottoman | 1299-1922 | Turkey, Balkans, Hejaz | Central dome with semi-domes, pencil minarets, tile decoration (Iznik), elaborate courtyards | High |
| Persian (Safavid) | 1501-1736 | Iran, Central Asia | Iwans facing courtyard, tile mosaic (haft rangi), pointed domes with tilework, reflecting pools | High |
| Mughal | 1526-1857 | India, Pakistan, Bangladesh | Onion domes, symmetrical gardens, red sandstone/marble, intricate inlay, chatris | High |
| Maghrebi | 789-1500 | Morocco, Algeria, Tunisia | Square minarets, courtyard gardens, geometric tile (zellij), carved stucco, cedar wood | High |
| Seljuk | 1037-1194 | Persia, Anatolia, Central Asia | Stalactite portals, brick patterning, conical tombs/turbes, stone carving | Medium |
| Nasrid | 1232-1492 | Andalusia (Granada) | Stucco arabesques, intricate tile dados, courtyard architecture, muqarnas ceilings | High |

### 3.2 Mosque Modeling Specifications

Mosque assets are the highest-priority building type. Each mosque requires a minimum of four reference sources: architectural plans, elevation drawings, historical photographs, and academic written descriptions. Modeling proceeds from ground plan outward.

**Ground Plan Construction:**
- Build from verified architectural plans scaled to meters
- Plan geometry is extruded to wall height with correct thickness proportions
- Wall thickness: 0.5-2.0 meters depending on period and material
- Foundation is not modeled (below ground plane) unless cross-section view is required
- Courtyard (sahn) dimensions must match reference: typical ratio 1:1 to 1:1.5 width to depth
- Courtyard arcades: column spacing 3-5 meters center to center, arch span equal to column spacing

**Wall and Facade Modeling:**
- Wall geometry: extruded base mesh with beveled edges (2-5mm bevel for mobile, 1-2mm for hero)
- Exterior wall detail: blind arches, niches, string courses, cornices modeled as separate geometry (not texture-baked) for Tier 1-2
- For Tier 3-4, wall detail is baked from high-poly to low-poly normal maps
- Ablaq (color banding in Mamluk architecture): alternating stone colors modeled as material ID variations on wall segments
- Tile decoration areas: flat planar sections with UV space reserved for tile atlas textures
- Stucco/plaster areas: modeled with medium detail, normal maps for fine arabesque patterns

**Column and Pillar Specifications:**

| Type | Period | Geometry | Poly Count (Hero) | Material | Capital Type |
|------|--------|----------|-------------------|----------|-------------|
| Classical column | Umayyad | Fluted cylinder + capital + base | 800 tris | Marble/limestone | Corinthian/Composite |
| Brick pillar | Abbasid | Square/octagonal pillar with engaged columns | 1200 tris | Brick with stucco | Stucco muqarnas |
| Marble column | Fatimid | Smooth cylinder with collar | 600 tris | Marble (veined) | Leaf capital |
| Stone pillar | Mamluk | Octagonal shaft with ablaq bands | 1000 tris | Stone (banded) | Muqarnas capital |
| Ottoman column | Ottoman | Cylindrical with drum rings | 500 tris | Marble/granite | Muqarnas/pendant |
| Persian column | Safavid | Slender cylinder on animal base | 900 tris | Marble/tile | Bull/animal capital |
| Mughal column | Mughal | Multi-lobed shaft | 700 tris | Red sandstone/marble | Lotus capital |

Column placement follows the hypostyle hall grid. Row spacing is typically 4-6 meters. Columns are instanced — a single column asset is placed N times with GPU instancing. Column capitals are separate sub-meshes to allow material variation.

**Arch Specifications:**

| Arch Type | Geometry Profile | Segments (Hero) | Segments (Mobile) | Used In |
|-----------|-----------------|-----------------|-------------------|---------|
| Horseshoe (Umayyad) | Circular arc exceeding 180 degrees | 24 | 12 | Umayyad, Maghrebi, Mughal |
| Pointed (Abbasid) | Two intersecting arcs | 20 | 10 | Abbasid, Persian, Seljuk |
| Keel (Fatimid) | Carina-shaped arc | 22 | 10 | Fatimid |
| Ogee (Mamluk) | Double-curved S-shape | 26 | 12 | Mamluk, Persian, Mughal |
| Ottoman pointed | Wide pointed arch | 18 | 8 | Ottoman |
| Multifoil (Maghrebi) | Multiple lobes | 30 | 16 | Maghrebi, Nasrid |
| Perforated (Mughal) | Pointed with scalloped edge | 28 | 14 | Mughal |

Arch geometry: single mesh with depth (extruded 0.3-0.8 meters). Arch soffit (underside) is modeled for arches visible from below. Intrados and extrados curves both defined. Voussoir (stone wedge) lines are UV-mapped, not individually modeled, for all tiers except hero.

**Dome Specifications:**

| Dome Type | Cross-section Profile | Geometry Method | Tier 1 Poly Count | Tier 3 Poly Count |
|-----------|----------------------|-----------------|-------------------|-------------------|
| Shallow dome | Broad arc (low rise) | Lathe/spin + subdivision | 3000 tris | 800 tris |
| Pointed dome | Near-spherical with apex | Lathe + modified arc | 3500 tris | 1000 tris |
| Onion dome | Bulbous curve | Lathe with custom profile | 4000 tris | 1200 tris |
| Double dome | Inner + outer shells | Two lathe meshes | 6000 tris | 2000 tris |
| Melon dome | Ribbed segments | Lathe + vertical division | 5000 tris | 1500 tris |
| Conical dome | Straight profile | Low-poly cone | 1500 tris | 500 tris |
| Bulbous dome | Swelling profile | Lathe with inflection | 4500 tris | 1200 tris |

Dome construction: base dome is created via lathe/spin modifier from a profile curve matching historical reference. Subdivision surface modifier is applied (2 levels for hero, 1 level for standard). Dome ribs are modeled as separate geometry only for hero tier. For Tier 3-4, rib detail is normal-mapped. The dome drum (transition zone) is modeled separately with windows, blind arches, and muqarnas cornice. The finial (top ornament) is a separate asset.

**Minaret Specifications:**

| Minaret Style | Cross-section | Body Profile | Balconies | Cap/Crown | Period |
|---------------|--------------|--------------|-----------|-----------|--------|
| Umayyad square | Square | Straight | 0-1 | Small dome | Umayyad, Maghrebi |
| Abbasid spiral | Circular/spiral | Tapered spiral ramp | None | Small pavilion | Abbasid (Samarra) |
| Mamluk octagonal | Octagon | Tapered, banded | 2-3 | Pear-shaped dome | Mamluk |
| Ottoman pencil | Circular/octagonal | Very slender | 1-2 | Sharp conical | Ottoman |
| Persian | Circular | Slender tapering | 1-2 | Cylindrical/tile cap | Safavid, Seljuk |
| Mughal | Circular | Tapered, fluted | 2-3 | Open pavilion + spire | Mughal |
| Fatimid | Square/octagonal | Flaring at top | 1-2 | Ribbed dome | Fatimid |

Minaret modeling: base section (projecting from building wall), shaft (main body with possible decorative bands and balcony supports), balcony (projecting platform with balustrade), upper shaft, and crown. Each balcony is a separate modeled element with corbel/muqarnas support.

**Muqarnas (Stalactite Vaulting) Specifications:**

Muqarnas is the most geometrically complex element in Islamic architecture. It must be modeled correctly, not approximated.

| Muqarnas Type | Cell Shape | Layers | Uses | Detail Method |
|---------------|-----------|--------|------|--------------|
| Simple tiered | Rectangular prisms with niches | 3-5 | Cornices, capital transitions | Geometry (Tier 1-2) |
| Squinch muqarnas | Niche cells filling corner | 4-8 | Dome transitions, iwan entrances | Geometry (Tier 1), Normal map (Tier 2-3) |
| Full vault muqarnas | Complex cell grid | 8-20 | Portal vaults, mihrab hoods | Geometry only |
| Portal muqarnas | Half-dome cell arrangement | 6-15 | Main entrance portals | Geometry (Tier 1-2), Normal (Tier 3+) |
| Pier muqarnas | Vertical cell stacking | 3-6 | Column capital transitions | Geometry (Tier 1), Normal (Tier 2-3) |

Muqarnas geometry construction: cells are individual mesh elements arranged in a grid following the plan of the surface. Each cell is a shallow niche with faceted surfaces. The grid follows a 2D plan that is projected onto the 3D surface. For hero assets, each cell is individually modeled. For standard assets, a repeating muqarnas tile is used with tileable UVs.

**Mashrabiya (Wooden Lattice) Specifications:**

Mashrabiya is modeled as an alpha-masked quad mesh for all tiers except hero. For hero, individual lattice pieces are modeled geometry.

| Detail Level | Method | Poly Count Per M2 | Texture |
|-------------|--------|-------------------|---------|
| Hero | Individual turned wood pieces | 5000 tris/m2 | 2048 atlas |
| Standard | Alpha-masked quad mesh | 200 tris/m2 | 1024 alpha texture |
| Background | Simple tiled alpha plane | 50 tris/m2 | 512 alpha texture |

Lattice patterns: geometric star patterns (4-point, 6-point, 8-point, 12-point) are the most common. Each pattern is constructed from repeating units. The pattern geometry is modeled with correct Islamic geometric construction methods.

### 3.3 Palace Modeling Specifications

Palace complexes are multi-building environments requiring modular construction approaches.

**Alhambra-Style (Nasrid) Palace:**
- Courtyard-first layout: all rooms arranged around rectangular courtyards with reflecting pools
- Arcade geometry: 124 slender marble columns supporting pointed horseshoe arches
- Stucco decoration walls: modeled as flat planes with normal-mapped arabesque patterns
- Tile dado (zellij): geometric tilework up to 1.2m height, tileable atlas texture
- Muqarnas ceilings: high-detail geometry for key halls
- Wooden ceilings (artesonado): geometric wood panel construction, modeled as intersecting beams
- Fountain geometry: central basin, water surface plane, water spout geometry

**Topkapi-Style (Ottoman) Palace:**
- Series of courtyards (First Court through Fourth Court) with increasing privacy
- Gate geometry: monumental portals with muqarnas niches
- Pavilion (kiosk) geometry: single-dome structure with open arcades, elevated on terrace
- Harem complex: labyrinthine corridors, small rooms, tiled interiors, multiple levels
- Tile panel geometry: Iznik tile panels as textured quad sets

### 3.4 Fortress and City Wall Specifications

Fortress and wall construction follows archaeological measurements and published plans. Walls are modeled as modular segments that can be assembled into any perimeter.

| Wall Type | Width | Height | Walkway | Battlements | Interval Towers |
|-----------|-------|--------|---------|-------------|-----------------|
| City wall (early) | 2-3m | 6-10m | Yes, 1-2m wide | Merlon/crenel pattern | Every 20-30m |
| City wall (late) | 3-5m | 10-15m | Yes, 2-4m wide | Arrow slits + merlons | Every 30-50m |
| Fortress curtain | 3-6m | 8-20m | Yes, 2-5m wide | Various | Every 15-25m |
| Citadel wall | 3-8m | 10-30m | Yes, 2-6m wide | Machicolations | Every 10-20m |

### 3.5 Observatory Specifications

| Observatory | Period | Key Structures | Instruments |
|-------------|--------|----------------|-------------|
| Maragheh | Ilkhanid (1259) | Main building, library, instrument platform | Armillary sphere, quadrant mural, astrolabe |
| Samarkand | Timurid (1420) | Trilithon trench, main building | Fakhri sextant (40m radius), armillary |
| Istanbul | Ottoman (1577) | Multi-level building with roof platform | Quadrant, astrolabe, celestial globe |
| Damascus | Umayyad (700s) | Minaret-based observation platform | Shadow instruments, gnomon |
| Cairo (Muqattam) | Mamluk (1120s) | Hilltop platform with wind breaks | Mural quadrant, azimuth instrument |

### 3.6 Bimaristan (Hospital) Specifications

Bimaristans are modeled as courtyard-centered complexes with distinct functional zones.

| Bimaristan | Location | Courtyard | Key Features | Unique Elements |
|------------|----------|-----------|--------------|-----------------|
| Nur al-Din | Damascus (1154) | 15x20m | Four iwans, fountain, patient rooms | Muqarnas portal, 8-point star fountain |
| Al-Mansuri | Cairo (1284) | Four iwans only | 4 iwans around courtyard, central fountain | Integrated pharmacy, lecture hall |
| Divrigi | Sivas (1229) | Asymmetrical | Carved stone portal, covered hall | Seljuk stone carving masterpiece |
| Al-Qayrawan | Kairouan (830) | Rectangular | Arcaded courtyard, prayer hall | Earliest surviving example |

### 3.7 Hammam (Bathhouse) Specifications

Hammam modeling follows the sequence: undressing room (camekan), cold room (sogukluk), warm room (tepidarium, hararet), hot room (sicaklik), furnace area (kulhan).
- Camekan: large domed entrance hall with raised seating platforms, central fountain
- Warm room: hypostyle hall with small domes, marble basins at walls, curved lounging niches
- Hot room: high-temperature room with central marble belly stone (gobek tasi) on raised platform
- Dome geometry: each room has a dome with glass eye-holes (fil gozu) for light
- Marble surfaces: UV-mapped with veined marble textures, polished

### 3.8 Caravanserai Specifications

Caravanserai modeling: massive rectangular fortress-like structure with single entrance, central courtyard, and stables/rooms around the perimeter.

| Caravanserai | Location | Period | Dimensions | Key Features |
|-------------|----------|--------|------------|--------------|
| Khan al-Umdan | Acre | Ottoman | 60x40m | 3 stories, clock tower |
| Buyuk Han | Nicosia | Ottoman | 50x35m | 68 rooms, mosque in courtyard |
| Sultan Han | Aksaray | Seljuk | 100x60m | Covered + open sections, monumental portal |
| Ribat-i Sharaf | Khorasan | Seljuk | 48x44m | Baked brick, elaborate portal |

---

## 4. Historical Locations

### 4.1 Complete Environment Reconstruction Standards

Historical location environments are complete 3D reconstructions of cities or districts at specific historical periods. These are the largest single 3D assets on the platform.

**Requirements for all historical location reconstructions:**
- Ground plane: 200x200m minimum (single district) to 2x2km (entire city)
- Period date specified to within 25 years
- Building plan verified by archaeological or historical plan
- Street width, layout, and paving materials historically accurate
- Vegetation appropriate to period (crops, gardens, trees)
- Water features (rivers, canals, fountains, wells) included where present
- Skybox/dome with period-appropriate atmosphere
- Ambient lighting matched to season and time of day
- 2-5 key landmark buildings at Tier 1-2 detail
- Supporting buildings at Tier 3-4 detail
- Distant buildings as Tier 5 billboard facades

### 4.2 Baghdad (Madinat al-Salam) — Round City

Period: 762-800 CE (Abbasid founding through early peak). Location extent: 2.7km diameter round city plus surrounding suburbs (Karkh, Harbiyya) plus east bank (Rusafa).

Key landmarks: city walls with four concentric rings and equidistant gates (Kufa, Basra, Khorasan, Damascus gates), Golden Gate Palace (Qasr al-Dhahab) at center, Grand Mosque, canals from Euphrates. Modeling approach: four concentric circular walls with 40m wide avenues between rings, modular residential complexes with courtyard houses, palace at Tier 1 hero with iwan throne room and green dome.

### 4.3 Cordoba (Medina Azahara)

Period: 936-1010 CE (Caliphal period). Location extent: 1.5km x 0.75km palace-city on the slopes of Jabal al-Arus.

Key landmarks: three descending terrace levels (official residence, gardens, mosque), Salon Rico reception hall, Great Mosque of Cordoba (180x130m), terraced gardens with pools and fountains. Modeling approach: stepped terrain mesh for three terrace levels, signature double-tiered arches (horseshoe arches on columns, second tier of arches above) for the mosque.

### 4.4 Cairo (Fatimid to Mamluk Periods)

Period: 969-1517 CE. Location extent: 3km x 2km.

Key landmarks: Fatimid city walls with three monumental gates (Bab al-Futuh, Bab al-Nasr, Bab Zuweila), Al-Azhar Mosque, Al-Hakim Mosque, Citadel of Saladin, Sultan Hassan Madrasa-Mosque (140m tall portal), Qalawun Complex, City of the Dead. Modeling approach: reconstructed city walls with gate complexes at Tier 1, citadel following hill contours, Sultan Hassan with muqarnas vault portal.

### 4.5 Damascus (Umayyad Period)

Period: 661-750 CE. Location extent: 2.5km x 2km.

Key landmarks: Umayyad Mosque (90x60m, three minarets, mosaic-covered walls), treasury octagonal structure, Roman temenos walls, Suq al-Hamidiyya. Modeling approach: Umayyad Mosque at Tier 1 with hypostyle plan and mosaic materials, courtyard arcades with double-tiered columns, three distinct minaret styles.

### 4.6 Jerusalem (Haram al-Sharif)

Period: 638-1517 CE. Location extent: 500m x 400m Haram platform plus Old City.

Key landmarks: Dome of the Rock (691 CE, octagonal plan, 54m diameter dome), Al-Aqsa Mosque, Haram platform, 11 gates, Sabil Qaytbay fountain. Modeling approach: Dome of the Rock at Tier 1 with octagonal outer wall, double colonnade ambulatory, 24,000 gold-anodized tile panels modeled as atlas-textured geometry.

### 4.7 Samarkand (Registan)

Period: 1370-1500 CE (Timurid period). Location extent: 1.5km x 1.5km.

Key landmarks: Registan ensemble with three madrasas (Ulugh Beg, Sher-Dor, Tilya-Kori), Gur-e-Amir Mausoleum (ribbed blue-tiled dome), Bibi-Khanym Mosque (130x100m, 400 marble columns), Shah-i-Zinda street of mausoleums. Modeling approach: massive iwans with muqarnas vaulting, measurement-accurate girih tile patterns, ribbed Timurid domes on high cylindrical drums.

### 4.8 Fez (Old City / Fes el-Bali)

Period: 809-1300 CE. Location extent: 3km x 2km.

Key landmarks: Qarawiyyin Mosque (270 columns, 17 aisles), Qarawiyyin University, Bou Inania Madrasa, Attarine Madrasa, Chouara Tanneries. Modeling approach: dense urban fabric with 10m max street width, Qarawiyyin with dense column forest and painted wooden ceilings, madrasas with zellij tile dados.

### 4.9 Istanbul (Constantinople)

Period: 1453-1600 CE (Early Ottoman). Location extent: 4km x 3km peninsula within Theodosian Walls.

Key landmarks: Hagia Sophia (31m dome diameter, 55m height), Topkapi Palace (700x500m, four courts), Sultan Ahmed Mosque (6 minarets), Suleymaniye Mosque (60m dome, 73m minarets), Hippodrome, Theodosian Walls (5.7km, 96 towers). Modeling approach: massive central domes on pendentives, semi-domes on four sides, modular wall segments with tower blocks at 50m intervals.

---

## 5. Maps & Terrain

### 5.1 3D Terrain Generation Pipeline

Terrain for historical maps is generated procedurally with manual art direction pass. Pipeline stages: heightmap acquisition from 30m SRTM or Copernicus DEM data, historical coastline adjustment, river network extraction via hydrology-based generation, manual cleanup removing modern features, vegetation layer for historical biomes, settlement placement with period date filtering, route network for trade and pilgrimage, climate zone coloring.

### 5.2 Height Map Specifications

| Property | Specification |
|----------|--------------|
| Resolution | 4097x4097 (2^12+1 for Unity Terrain) |
| Bit depth | 16-bit grayscale |
| Vertical resolution | 0.1m minimum elevation change |
| Vertical range | -200m to 8000m |
| Format | RAW, TIF, or EXR |
| Tiling | 4x4 tile set for large regions |
| Coordinate system | Geographic (lat/lon) with UTM zones |

Heightmaps are authored in World Machine or Gaea for procedural generation. Historical sea level adjustments: Mediterranean basin in 1000 CE is approximately 0.8m higher than modern.

### 5.3 Procedural Terrain Texturing

| Layer | Texture Resolution | Tiling Size | Blend Method | Regions |
|-------|--------------------|-------------|--------------|---------|
| Sand (desert) | 1024x1024 | 10x10m | Height-based with slope mask | Sahara, Arabian, Syrian deserts |
| Rock (mountain) | 1024x1024 | 5x5m | Slope >30 degrees | Hijaz, Alborz, Atlas, Taurus |
| Grass (steppe) | 512x512 | 8x8m | Height + moisture blend | Anatolia, Central Asia |
| Forest floor | 1024x1024 | 6x6m | Biome mask | Caspian coast, Balkans |
| River sediment | 512x512 | 4x4m | Proximity to river network | Nile valley, Tigris/Euphrates |
| Snow | 512x512 | 20x20m | Height >4000m | High mountain peaks |
| Cultivated | 256x256 | 3x3m | Historical agricultural areas | Mesopotamia, Andalusia |
| Urban | 256x256 | 2x2m | City footprint | Historical city locations |

Terrain texturing uses 4-layer splatmap blending (RGB + Alpha to control 4 materials per terrain tile). Maximum 8 materials per scene.

### 5.4 Historical Coastline Changes

| Region | Change Since 1000 CE | Modeling Method |
|--------|---------------------|-----------------|
| Mediterranean | +0.8m sea level rise | Adjust heightmap sea level + erosion simulation |
| Nile Delta | 4-8km coastline retreat | Manual terrain sculpting |
| Mesopotamian marshlands | 60% reduction | Extend marsh areas via lower elevation + water plane |
| Persian Gulf | 2m sea level rise | Adjust coastline inland ~5km |
| Caspian Sea | +3m level in Parthian period | Raise Caspian level, flood coastal areas |

### 5.5 Trade Routes as 3D Paths

Trade routes are modeled as 3D spline geometry. Path geometry: width-extruded spline with 0.5m height above terrain. Path surface material: packed earth for land routes, dashed line for maritime.

| Route | Length | Waypoints | Path Width | Period | Color |
|-------|--------|-----------|------------|--------|-------|
| Silk Road (land) | 12,000km | 50+ | 5-10m | 100 BCE - 1400 CE | #C4924A (amber) |
| Incense Route | 3,400km | 20+ | 5-8m | 500 BCE - 600 CE | #D4785C (terracotta) |
| Trans-Saharan | 4,500km | 15+ | 10-30m | 800-1600 CE | #C47A4A (copper) |
| Spice Route (maritime) | 15,000km | 30+ | N/A (dashed) | 900-1500 CE | #3D7A6E (teal) |
| Hajj Route (Egyptian) | 1,500km | 15+ | 10-20m | 600-1900 CE | #7A9E8A (sage) |
| Hajj Route (Damascus) | 1,600km | 12+ | 10-20m | 600-1900 CE | #7A9E8A (sage) |

### 5.6 City Placement on Terrain

Cities are placed on terrain using verified historical coordinates. Workflow: load historical city database, convert to 3D world coordinates via UTM projection, snap to terrain surface, apply city footprint mask scaled by historical population estimate, generate city label billboard, tier city based on historical importance.

### 5.7 Interactive Elevation Visualization

Elevation visualization uses color-coded height bands. Mode is activated by toggle. It replaces terrain material with elevation-shaded overlay at 50% opacity over base terrain. Contour lines (50m interval) can be optionally displayed as line geometry.

### 5.8 Climate Zone Visualization

Climate zones are layered over terrain as semi-transparent colored regions. Data source: historical climate reconstruction datasets. Each zone boundary is approximately 5-25km accuracy. The overlay includes animated wind arrows and precipitation indicators.

---

## 6. Terrain & Landscape

### 6.1 Desert Terrain

Sand dune types: Barchan (crescent, 20-100m length, 5-30m height), Transverse (linear, 500m-5km), Star (radial, 50-500m), Parabolic (50-500m), Seif (longitudinal, 500m-10km), Compound/complex (1-10km). Dune generation: heightmap-based with procedural dune field generation. Each dune is a mesh with 200-2000 triangles depending on distance.

Oasis generation: water body as shallow pool (20-200m diameter), vegetation with palm trees (10-50 per oasis), building with well structure, transition zone splatmap from sand to moist soil to vegetation. Water material with turquoise/green tint, high translucency.

### 6.2 Mountain Terrain

Hijaz Mountains (1000-3000m): dark red-black granite and basalt, sharp wadi incisions. Alborz Mountains (2000-5670m): folded sedimentary rock with volcanic peaks, snow above 4000m. Atlas Mountains (1000-4167m): red-brown rock, pine forests on lower slopes, three parallel ranges.

Mountain terrain material blend: base rock with color variation per range, cliff face normal maps, scree/talus slope-based material at 30-45 degrees, snow cover with height-based mask, biome-based vegetation distribution.

### 6.3 River Valley Terrain

Nile Valley: 3-20km floodplain, seasonal flood zone animation, 150km wide delta with branching distributaries, rectangular field pattern. Tigris and Euphrates: 5-50km valley, 3000km historical irrigation canal network as line geometry. Indus Valley: 10-100km valley, shifting river course with multiple paleochannels.

River geometry: flat plane at water level with gradual flow UV animation, depth visible as darker water center, banks at 0.5-2m elevation. River simplifies to single plane at distance.

### 6.4 Coastal Terrain

Coastal terrain combines land, beach, and water with transition zones: sandy beach (10-200m), rocky coast (5-50m) with cliffs and sea stacks, mangrove/estuary (100-1000m), urban coast (0-10m) with seawalls, delta coast (1-20km). Coastal water: shallow water visibility with wave vertex displacement, shore foam as particle system or texture mask.

### 6.5 Terrain LOD System

| LOD Level | Terrain Resolution | Vertex Spacing | Triangles per Tile | Draw Distance |
|-----------|-------------------|---------------|-------------------|---------------|
| LOD0 (Full) | 512x512 per tile | 0.25m | 524,288 | 0-100m |
| LOD1 (Medium) | 256x256 per tile | 0.5m | 131,072 | 100-500m |
| LOD2 (Low) | 128x128 per tile | 1.0m | 32,768 | 500-1500m |
| LOD3 (Very Low) | 64x64 per tile | 2.0m | 8,192 | 1500-3000m |
| LOD4 (Impostor) | 32x32 per tile | 4.0m | 2,048 | 3000-5000m |
| LOD5 (Distance) | Flat quad | N/A | 2 | 5000m+ |

### 6.6 Terrain Texturing Layers

Each terrain tile has a maximum of 4 material layers blended via splatmap at 512x512 RGBA. Standard layer presets: Desert sand (#D4B080, roughness 0.9), Rocky ground (#8A7A5A, roughness 0.8), Grassland (#7A9A5A, roughness 0.7), Forest floor (#5A7A4A, roughness 0.8), Cultivated soil (#8A7A4A, roughness 0.7), Mud/sediment (#6A5A4A, roughness 0.6), Stone pavement (#9A9A8A, roughness 0.5), Snow (#E8E8E8, roughness 0.3).

---

## 7. Artifacts

### 7.1 Artifact Classification and Tier Assignment

Artifacts are museum-quality 3D assets intended for close inspection and rotation. Categories: Astrolabes (Hero, 30K-50K tris, 2048 textures), Celestial globes (Hero, 25K-40K tris), Automata (Hero, 35K-50K tris), Surgical instruments (Standard, 3K-8K tris), Pottery (Standard, 2K-5K tris), Coins (Small, 500-2K tris).

### 7.2 Astrolabe Specifications

Components: Mater (base plate, circular rim with suspension ring, lathe/spin geometry), Tympan (latitude plate with elevation lines), Rete (star map with openwork framework and 16-24 star pointers), Alidade (sighting arm), Pin/horse (center pin), Suspension ring. Engraved markings rendered via signed distance field alpha texture.

Construction hierarchy: mater (static), tympan (rotates for latitude), rete (rotates independently for annual sun position), alidade (rotates for sighting). All astrolabes include stereographic projection for specific historical latitude.

### 7.3 Celestial Globe Specifications

Sphere at 128 segments with 256 rings, constellation lines and 50-100 star positions from Al-Sufi's Book of Fixed Stars (964 CE). Horizon ring with degree markings, meridian ring, decorative stand. Stars rendered as magnitude-varied point size.

### 7.4 Quadrants and Sextants

Mural quadrant: 40cm quarter-circle frame with plumb line, 1-degree subdivisions. Portable quadrant: 15cm handheld frame with sighting vane, 5-degree subdivisions. Wooden sextant: 30cm mahogany frame with brass fittings, 0.5-degree subdivisions. Precision brass sextant: 20cm with telescope, 0.1-degree subdivisions.

### 7.5 Water Clock Specifications

Al-Jazari elephant clock components: elephant figure (16K tris), water basin (4K tris), bucket mechanism (3K tris), dragon figures (6K tris), ball drop mechanism (2K tris), water particle system (200 particles). Total assembly: 45K tris. All water clocks include animated float mechanisms and timed sequences.

### 7.6 Automata (Al-Jazari Machines)

| Machine | Poly Budget | Moving Parts | Animation |
|---------|------------|-------------|-----------|
| Peacock fountain | 25,000 | Head rotation, water flow | Fountain particle + head rotation |
| Washing machine | 30,000 | Crank, water lift, basin fill | Gear rotation + water level change |
| Bloodletting instrument | 15,000 | Float, scalpel | Linear mechanism animation |
| Water pump with gears | 35,000 | Gears, piston, water pipe | Continuous cycle |
| Castle clock | 50,000 | 11 figures, doors, water | Multi-sequence timed animation |

Gear modeling: each gear with correct involute tooth profile (24-48 segments per gear). Gear ratios maintain historical accuracy. All gear animations loop seamlessly.

### 7.7 Surgical Instruments (Al-Zahrawi)

Over 200 instruments from Kitab al-Tasrif: scalpels (12 shapes, 500-1000 tris each), cauteries (6 types, 300-800 tris), speculums (5 sizes, 800-2000 tris), forceps (8 types, 400-1000 tris), catheters (3 types, 200-500 tris), bone saws (4 sizes, 800-1500 tris), syringes (3 types, 300-600 tris), needles (10 types, 100-200 tris). Materials: steel/iron with wood or bone handles, polished silver, brass, glass.

### 7.8 Chemical Apparatus

Alembic (3K tris, glass/ceramic), cucurbit gourd flask (1K tris, glass), aludel sublimation vessel (4K tris, unglazed ceramic), athanor furnace (6K tris, brick/stone), evaporation dish (500 tris, ceramic), mortar and pestle (1.5K tris, brass/stone), balance scales (3K tris, brass/iron), storage jars (500-2K tris, glazed ceramic).

### 7.9 Optical Devices (Ibn al-Haytham)

Camera obscura (5K tris, with inverted image screen), pinhole array (2K tris, light ray visualization), magnifying lens (500 tris, refraction shader), spherical mirror (1.5K tris, reflection environment map), parabolic mirror (2K tris, focused light beam particles), refraction apparatus (3K tris, light ray bending visualization).

### 7.10 Manuscripts (Open and Closed)

Closed book: 500 tris, rectangular block with cover, spine, bookmark ribbon. Open book: 800 tris, V-shape geometry with visible pages. Illuminated page: 200 tris quad at 2048 texture resolution. Scroll (rolled): 300 tris cylinder with spiral end. Scroll (unrolled): 600 tris flat sheet with rolled ends, 2048 content texture.

### 7.11 Pottery and Ceramics

Bowls (1.5K tris, hemisphere on foot), storage jars (2.5K tris, ovoid body with handles), oil lamps (1K tris, closed body with spout), plates (800 tris), cups (1K tris), vases (3K tris, elaborate profile), geometric tiles (200 tris), water jugs (2K tris). Ceramic material: albedo with glaze pattern, smoothness 0.6-0.8, metallic 0.

### 7.12 Metalwork

Brass tray (2K tris, engraved normal), copper ewer (3.5K tris, embossed normal), silver jewelry (5K tris, filigree), gold pendant (3K tris, enamel color mask), steel sword (4K tris, damascus pattern), iron armor (8K tris, chainmail alpha), bronze door (6K tris, relief normal), brass candlestick (2.5K tris). Metal parameters: metallic = 1.0, smoothness varies by finish (polished 0.9, hammered 0.5-0.7, oxidized 0.2-0.4).

### 7.13 Textiles and Carpets

Hanging carpet (1K tris, 2048 woven pattern), floor carpet (2K tris, subdivided for folds, 2048 tileable pattern), silk textile (3K tris, cloth simulation), tapestry (500 tris), embroidered textile (500 tris, stitch normal map), draped clothing (5K tris, cloth simulation), woven basket (3K tris). Carpet patterns match historical designs (Isfahan, Kashan, Tabriz, Ottoman court, Mamluk).

### 7.14 Coinage

Gold dinar (500 tris, 19-22mm, Umayyad/Abbasid/Fatimid), silver dirham (600 tris, 24-27mm), copper fals (400 tris, 15-30mm), silver akce (300 tris, 12-15mm, Ottoman). Coin modeling: cylinder with appropriate thickness/diameter ratio, embossed legend via signed distance field alpha texture, edge detail matching historical minting method.

---

## 8. Books & Scrolls

### 8.1 Interactive 3D Book Specifications

Interactive books open, close, and turn pages in response to user interaction. Poly budget: 1K tris closed, 3K tris open static, 6K tris open animating (with page subdivision). Cover texture: 2048 atlas. Page interior: 2048 per spread. Max 200 pages (100 double-sided folios). 3 LOD levels.

### 8.2 Book Geometry Construction

Closed book: two rectangular cover boards with rounded spine, page block slightly inset, hinges, ribbon bookmark, optional clasps. Open book: V-shape cross-section at 120-160 degrees, spine interior visible, 5-10 visible page layers per side, page edges with subtle stepped appearance.

Page turning animation: subdivided plane (16x12 segments), page peels from edge, curls upward, folds across center. Duration 0.8-1.5 seconds, ease-in-out cubic. Bezier curve controlled page curl during turn.

### 8.3 Cover Design Specifications

Period-specific binding: Abbasid (dark brown stamped leather, blind tooling), Mamluk (red-brown leather with envelope flap, gold tooled), Safavid (lacquer painted with floral miniatures), Ottoman (maroon leather with central medallion), Mughal (green leather with floral inlay), Maghrebi (brown leather with geometric blind tooling). Cover UV: front 50%, back 40%, spine 8%, inside covers 2%.

### 8.4 Illuminated Page Interiors

Page texture: parchment base (#F0E0C0) with aging noise at 2048. Text block separate layer. Illumination gold at 1024 with metallic specular. Border decoration at 2048. Miniature painting at 2048 with color-accurate reproduction. Page normal map for paper surface texture (0.2-0.5mm variation). Gold illumination areas masked via texture alpha for metallic = 1.0.

### 8.5 Scroll Unrolling Animation

Papyrus scroll: 5cm diameter x 30cm rolled, up to 10m unrolled. Parchment scroll: 8cm x 40cm rolled. Animation: two roller cylinders, right rotates clockwise, left counter-clockwise, scroll body extends between. Speed: 10cm per second. Content via UV offset on scroll body mesh.

### 8.6 Book Stacking and Shelving

Single book: 1K tris. Stack (3-5 books): 3K-5K tris. Bookshelf row (20-40 books): 20K-40K tris. Bookshelf unit with frame: 8K-15K tris. Books stand vertically with 0-5 degree lean variation, 1-3mm gaps, 8-15 cover colors per shelf, sizes varied +/-20%.

### 8.7 Reading Stand Integration

X-frame rahle (2K tris, wood, folding mechanism), sloping desk (3K tris, wood inlaid, angle adjustment), cushioned stand (2.5K tris, wood with textile), floor stand (3.5K tris, turned wood with brass feet). Book parented to stand surface with gravity-sitting rotation.

---

## 9. Libraries & Museums

### 9.1 3D Library Environments

Libraries are multi-room environments with shelving, reading areas, manuscript storage, and period-appropriate architectural decoration. Key libraries: House of Wisdom (Baghdad, Abbasid, 8 rooms), Al-Qarawiyyin (Fez, Marinid, 6 rooms), Al-Azhar (Cairo, Fatimid, 10 rooms), Al-Zaytuna (Tunis, Hafsid, 5 rooms), Cordoba Library (Umayyad, 12 rooms, 400,000 volumes), Suleymaniye Library (Istanbul, Ottoman, 7 rooms).

### 9.2 Shelving Systems

Wall shelf (200x100x30cm, 5 shelves, 50 books), Floor-to-ceiling (400x120x40cm, 8 shelves, 120 books), Central bookcase double-sided (300x200x60cm, 8 shelves per side, 240 books), Recessed shelf (150x80x35cm, 4 shelves, 30 books), Display case with glass front (150x100x50cm, 20 display books). Shelving frames extruded from profile curves with decorative corbels and moldings.

### 9.3 Reading Areas

Reading tables 200x100x75cm seating 4-6, carved wood with inlaid decoration. Oil lamps or candles with dynamic light. Period-appropriate seating (wooden chairs for Ottoman, floor cushions for earlier periods). Writing materials: inkwell, pen tray, paper.

### 9.4 Manuscript Display

Open manuscript on stand (Tier 1-2), display case with single page (Tier 1), wall-mounted illuminated page frames (Tier 2-3), horizontal scroll case 30-50% unrolled (Tier 1-2). Each includes informational hotspot.

### 9.5 Interactive Book Retrieval

Camera raycast to book spine, book highlights with emissive glow (0.5s), book slides from shelf (0.3s), floats to reading position (0.5s ease-out), opens to selected page (0.5s ease-in-out), camera transitions (0.3s). Reverse on close.

### 9.6 Museum Gallery Spaces

Main hall (20x40x10m, 30 artifacts, top ambient + spotlights), Side gallery (10x20x6m, 15 artifacts, track lighting), Rotunda (12m diameter, centerpiece + 8 exhibits, dramatic top light), Period room (8x12x5m, 5 artifacts, period-accurate candle-equivalent lighting), Outdoor court (15x15m, 5-8 large artifacts, natural daylight).

### 9.7 Artifact Pedestals and Displays

Classic pedestal (100x40x40cm, marble/wood, 1K tris), glass case (150x60x100cm, glass + brass, 3K tris), wall mount (30x30x15cm, brass/wood, 500 tris), turntable (40x40x20cm, black acrylic, 1.5K tris), vertical case (200x50x50cm, glass + wood, 2.5K tris), plinth (50x50x100cm, stone, 800 tris). Pedestal marble: smoothness 0.7-0.9. Display glass: 80% transmission, 2% reflectivity, fresnel effect.

---

## 10. Museum Objects

### 10.1 Exhibition-Quality 3D Presentation

Camera distance: 0.5-3m adjustable zoom. Rotation: X tilt and Y rotate. Min FOV: 10 degrees (macro detail). Max FOV: 60 degrees (full object). Anti-aliasing: MSAA 4x mobile, TAA desktop/web. Background: warm cream to deep teal gradient or transparent.

### 10.2 Turntable Animation

360 degrees per 20-30 seconds constant speed. Auto-rotation pauses on user drag, resumes after 5 seconds inactivity. Rotation around Y axis. Subtle axis line visible on turntable base.

### 10.3 Zoom-to-Detail Interaction

Pinch/drag zoom: 2x to 20x magnification. Zoom limit at 1:1 pixel ratio. Magnifying glass overlay optional. Depth of field at high zoom (desktop only). Pre-defined zoom hotspots for key artifact details.

### 10.4 Information Hotspots

3D markers on artifact components. Visual: 16px radius circle with pulse animation. Color: warm gold (#C49A3A) with white rim. Hotspot scales with camera distance. Tap opens tooltip with artifact name, period, material, dimensions, function, historical significance. Optional audio narration (1-3 minutes). 3-8 hotspots per artifact.

### 10.5 Contextual Environment

Small room environment (8x8x5m) with period architecture matching artifact origin. Wall color, floor, lighting period-accurate. Secondary props, atmospheric dust particles, warm light shafts, lamp flicker animation. Period-appropriate ambient audio.

### 10.6 Conservation/Restoration Visualization

Current state (default) and original state (reconstructed) with morphing animation (1-2 second cross-fade). Missing pigment areas filled, broken sections reconstructed as semi-transparent in current view. Conserved areas highlighted with colored overlay.

### 10.7 Cross-Section Views

Adjustable cut plane with slider control. Cut surface material: warm gray (#C0B0A0). Interior structure modeled for visible cross-section. Cutaway and exploded view modes. Full cut (50% removal), quarter cut (25%), layer removal (sequential). Supported for water clocks, automata, astrolabes, ceramic vessels, celestial globes, surgical instruments.

---

## 11. Interactive Models

### 11.1 Educational Interactive 3D Principles

Every interactive model includes: clear learning objective, intuitive controls (touch drag, pinch, tap), visual feedback for all interactions, reset button, step-by-step mode, free exploration mode, information labels on all major components, optional audio narration.

### 11.2 Astronomical Models

Orrery: central sun (2K tris), planets Mercury through Saturn (300-2K tris each), orbital wireframe rings at correct relative speeds via gear drive. Planetary spheres (Ptolemaic/Islamic): nested sphere system with deferent and epicycle for each planet. Earth-centered with visible celestial sphere.

### 11.3 Mechanical Models

Gear system demonstrator: driver gear (1.5K tris, 10 teeth), driven gear (2.5K tris, 20 teeth), idler gear (2K tris), compound gear (4K tris), rack and pinion (3K tris), worm gear (2.5K tris), bevel gears (3K tris). User drags crank, gear train rotates at correct ratios. Water wheel: noria (8K tris), pots/buckets filling animation, gravity-driven water channel. Clock mechanism: gear train (10K tris for 5-8 gears), escapement (2K tris), balance wheel/pendulum, hands.

### 11.4 Architectural Models

Cross-section building models: mosque (100K tris, 25% cutaway), palace (150K tris, layer removal), fortress (80K tris, vertical cross-section), house (50K tris, roof removal), hammam (60K tris, horizontal cut). Cut faces filled with slice material (#C0B0A0). User toggles floor levels on/off.

### 11.5 Anatomical Models

Human skeleton (50K tris, 206 bones grouped), muscular system (80K tris, 12 groups, layer reveal), circulatory system (60K tris, blood flow animation), nervous system (40K tris, signal pulse animation), eye (30K tris, 8 layers cross-section), ear (25K tris, 6 regions, sound wave visualization). Stylized realistic matching platform aesthetic.

### 11.6 Mathematical Models

Platonic solids (500-2K tris each, unfold net), Archimedean solids (1K-5K tris, face count), conic sections (10K tris, adjustable cut plane), spherical geometry (8K tris, great circles), muqarnas geometry construction (20K tris, step-by-step), geometric star patterns (5K-15K tris, 4/6/8/10/12/16-point). Construction geometry visible in wireframe mode.

---

## 12. Knowledge Tree (3D)

### 12.1 Tree Modeling Specifications

Central 3D visualization representing the learner's acquired knowledge. Coordinate system: Y-up centered at origin. Scene scale: 15x15x20 units. 4 LOD levels. Shadow casting: real-time for LOD0-1, baked for LOD2, none for LOD3.

### 12.2 Base Trunk and Root System

Trunk: 24-sided cylinder, 16 height segments, irregular cross-section with 12 control points. Tapers from 0.8 units radius at base to 0.24 at top (30%). Height: 5-8 units (scales with node count). Bark normal map with vertical flow pattern. Roots: 3-8 main roots, 1.5-3 unit spread, bezier curve extrusion with 8 segments per root. Total root tris: 3K-8K LOD0.

### 12.3 Procedural Branch Generation

L-system with 4-6 hierarchy levels. Branch count: min 20, max 200 (scales with nodes). Branch angle: 30-60 degrees from parent. Rotation: 90-180 degrees between consecutive branches (phyllotaxis-based). Length reduces 50% per level. Radius reduces 60% per level. Branch curve with slight sag (0.1-0.3 unit per meter). Each branch is a 6-sided cylinder extruded along bezier curve.

### 12.4 Node Placement Rules

1 node per completed topic, up to 10,000+. Placement at branch tips and along branches. Minimum spacing 0.5 units. Weighted toward upper canopy (newer knowledge). Category-based clustering via attraction force. Node geometry: sphere with 16 segments (500 tris), 0.15-0.4 units scaled by mastery level. Category-mapped color with 20% emissive glow.

### 12.5 Connection Line Geometry

Bezier curves with 3 control points. Width: 0.02-0.05 units. 12-24 segments per curve. Midpoint rises 0.2-0.5 units above node plane. Translucent material at 30-60% opacity. All connection lines rendered as single batch mesh.

### 12.6 Leaf/Fruit Particles

Leaves: 500-2000 quads (8x8px sprites), 0.05-0.15 units, gentle sway with wind simulation, occasional drop. Knowledge fruits: 50-500 spheres (12 segments), 0.08-0.2 units, gold (#D4A043) with glow pulse. Flower blooms: 10-100, 5-petal geometry, slow open/close cycle. GPU particles on mobile.

### 12.7 Growth Animation Rig

Triggered when new nodes added. Duration: 3-10 seconds. Branches extend from trunk at 0.5m/s. Nodes grow from 0 to full size over 1 second. Leaves fade in over 2 seconds. Roots extend at 0.2m/s. 50-200 celebration particles at completion.

### 12.8 LOD System

LOD0 (Full): 50K tris, 0-10m range, full geometry and particles. LOD1 (Medium): 25K tris, 10-25m, tertiary branches as strips. LOD2 (Low): 10K tris, 25-50m, secondary strips, tertiary lines. LOD3 (Billboard): 2K tris, 50-100m, branch silhouette. LOD4 (Cull): disabled beyond 100m.

### 12.9 Glow and Energy Effects

Canopy ambient glow via hemisphere light at 30% warm gold. Node pulse emissive at 20-80%. Connection shimmer via vertex animation. Ambient particles (50-200). Bloom post-process for desktop only; mobile uses emissive materials with additive blending.

---

## 13. Avatar (3D)

### 13.1 Character Modeling Specifications

The 3D avatar is the learner's self-representation. Style: stylized realistic, warm and approachable. Scale: 1.7m height. Base mesh: quad-based, animation-ready. 4 LOD levels. 65-80 bones. 30+ blend shapes. Humanoid Mecanim-compatible rig. Texture set: Albedo, Normal, ORM, Emission (face).

### 13.2 Base Mesh Topology

Head: 2,500-3,500 quads with edge loops following facial anatomy. Eye area: 16-20 segments. Mouth: 12-16 segments. Nose: 8-12 segments. Torso: 2K-3K quads. Arms: 800-1,200 quads each. Hands: 500-800 quads each (individual fingers). Legs: 1,200-1,800 quads each. Total body: 7K-10K quads. Clothing: 2K-4K quads per garment.

### 13.3 Rigging/Hierarchy

Skeleton: pelvis root, 4-6 spine vertebrae, clavicles, arms (upper/forearm/hand with finger chain), legs (upper/lower/foot/toe), 10-15 facial bones (jaw, eyes, brows, cheeks, lips). Bone naming follows hierarchy convention. Weight painting: max 4 bone influences per vertex, smooth 15-20 vertex overlap at joints, rigid at hands/feet/head.

### 13.4 Blend Shapes for Expressions

30+ blend shapes including: Joy_Smile, Joy_Eye, Sad_Frown, Sad_Brow, Surprise_Brow, Surprise_Mouth, Anger_Brow, Anger_Mouth, Disgust_Nose, Disgust_Lip, Fear_Brow, Fear_Eye, Think_Brow, Think_Mouth, Talk_Open (phoneme A), Talk_Pursed (phoneme O/U), Talk_Smile (phoneme E/I), Blink, Wink_L, Wink_R. Max 1mm vertex offset. Naming pattern: BS_{Category}_{Name}.

### 13.5 Clothing Topology

Thobe/dishdasha (2.5K verts, ankle-length drapes), Abaya (3K verts, loose drape), Hijab (800 verts, neck drape), Turban (1.2K verts, fold detail, tail), Taqiyah/kufi (400 verts, dome, edge pattern), Western shirt (2K verts, collar cuffs), Pants/sirwal (1.5K verts, baggy cut), Coat/jubba (2.5K verts, pocket detail). UV seams hidden in natural garment seams.

### 13.6 Accessory Attachment Points

Glasses attached to Head bone at eye level, hijab overrides hair, turban wraps above ears, necklace hangs at clavicle, backpack at spine_03 behind back, belt at pelvis, held book at Hand_R with 0.1m palm offset, pen behind ear or in hand. Accessories are separate meshes parented to rig bones.

### 13.7 Hair/Fabric Simulation

Hair: 3K-8K quads, 10-30 control bones, spring-damper follow at 10-20% of main movement, normal map for strand detail, anisotropic specular. Fabric: 3-8 bone chains per drape area, 30% position follow, 70% rotation follow, damping 0.5. Sphere colliders at 0.02m radius prevent interpenetration. Wind sine wave at 0.005-0.01m amplitude, 1-2Hz.

### 13.8 LOD Levels

LOD0 (Full): 25K-35K tris, full face, 5 fingers, 3 clothing layers, full blend shapes. LOD1 (Medium): 15K-20K tris, reduced edge loops, 3 fingers + thumb, 2 layers, key blend shapes. LOD2 (Low): 8K-12K tris, simplified face, mitten hand, 1-2 layers, face blend shapes only. LOD3 (Very Low): 4K-6K tris, decal-based face, simple hand, 1 layer, no blend shapes.

### 13.9 Mobile-Optimized Version

For low-end devices: 4K-6K tris total. Face: 800 quads with decal texture (painted features, no separate eye geometry). Hair: single alpha-textured mesh. Clothing: 1 layer, no simulation. Hands: closed fist. Rig: 30 bones (no facial or finger bones). Single 1024 atlas texture. No blend shapes. 4 key animation poses (idle, pulse, reading, celebration).

---

## 14. Knowledge Spirit (3D)

### 14.1 Spirit Modeling Specifications

Ethereal companion entity representing the AI mentor. Polygon budget: 500-3,000 tris (evolves with learner level). 3 LOD levels. Scale: 0.5-2.5 units. Transparent render queue (3000+). Double-sided culling.

### 14.2 Translucent Material Setup

Base color: #6366F1 (indigo). Opacity: 0.6-0.9 core, 0.2-0.4 edges via fresnel. Emission: #8B8CF1 at 20-60% intensity, 2-5 nits. Smoothness 0.3. Metallic 0.0. Fresnel power 2-4, fresnel color white. Custom shader requirements: vertex animation for gentle float displacement (0.1-0.3 unit amplitude), opacity fresnel, emission pulse (sine 2-4s period, 20-30% variation). No shadow casting. Depth write off.

### 14.3 Glow/Emission Settings

Core glow: emissive material at 60%. Edge glow: fresnel emission white at 80%. Bloom post-process: 0.5 threshold, 2.0 strength (desktop only). Mobile glow: additive sprite overlay at 50%. Aura ripple: vertex offset at 0.05 unit amplitude.

### 14.4 Particle System for Trail/Aura

Trail particles: 50-200 count, 1-3s lifetime, 0.2-0.5 units/s speed, 0.02-0.08 unit size, indigo to transparent gradient, additive render mode. Aura particles: 30-100 count, 0.5-1.5 unit orbit radius, 0.1-0.3 rev/s orbit speed, 0.03-0.1 unit size, white to indigo gradient, infinite lifetime.

### 14.5 Evolution Stage Geometry Changes

Stage 1 (Levels 1-9): Icosahedron, 500 tris, 0.5 units. Stage 2 (10-24): Octahedron, 1K tris, 1.0 units, 1 glow ring. Stage 3 (25-49): Dodecahedron, 1.5K tris, 1.5 units, 2 rings. Stage 4 (50-74): Geometric flower 6-petal, 2K tris, 2.0 units, 3 rings. Stage 5 (75-99): Rhombic triacontahedron, 2.5K tris, 2.3 units, 4 rings. Stage 6 (100+): Legendary custom form, 3K tris, 2.5 units, 5 rings.

### 14.6 Float Animation Rig

Vertical oscillation: 0.5-2 unit amplitude at 4-8s period. Horizontal drift: 0.2-0.5 unit radius circular path. Rotation: 15-45 degrees/s around Y. Bob: 5-10 degree tilt oscillation on X and Z. Children elements have 0.2-0.5s phase offset.

### 14.7 Size Relative to Avatar

Dashboard: 0.5x avatar height, 0.5m above/right. Learning mode: 0.3x avatar height, above shoulder. Knowledge tree: 0.8x tree height. Museum view: 0.2x camera FOV. Garden: 0.5m absolute above center.

### 14.8 Color/Material Changes Per Evolution

Stage 1: #6366F1 (indigo) primary, #8B8CF1 emission, opacity 0.7. Stage 2: #7C3AED (violet), blue ring. Stage 3: #8B5CF6 (purple), amber + emerald rings. Stage 4: #D946EF (fuchsia), multi-color rings. Stage 5: #F43F5E (rose), multi ring set. Stage 6: #FBBF24 (gold), gold rings with sparkle.

---

## 15. Memory Garden (3D)

### 15.1 Garden Environment Specifications

Total area: 40x40 units (expandable to 100x100). Ground plane: 20x20 segments, 0.5 unit max undulation. Isometric camera (45 degrees) or free orbit. 3 LOD levels. Max plants: 500 beginner to 2000+ advanced. Performance target: 30fps mobile with 500 plants.

### 15.2 Terrain/Ground Plane

40x40 unit plane with 20x20 vertex grid, gentle sine undulation (0.3 unit amplitude, 10 unit wavelength). Base grass color #7A9E5A with variation. Pathways as dirt material channel (#9A8A6A). Terrain splatmap: grass 70%, dirt/pathways 15%, flower beds 10%, stone/decorations 5%.

### 15.3 Water Channel Geometry

Channel width: 0.5-1.5 units. Depth 0.2 units below terrain. Total length 5-30 units curvilinear. 128x128 water plane with animated flow. Translucent teal (#3A8A7A, opacity 60-80%). Channel bank 0.05 unit raised edge.

### 15.4 Pathway Generation

Main path: 1.5-2 units wide, entrance to center. Secondary paths: 0.8-1.2 units, connecting clusters. Path material: compacted earth (#9A8A6A) with pebble normal map. 0.03 unit raised border. Generated procedurally based on plant placement.

### 15.5 Plant Placement System

Plants placed at grid positions with 1-3 unit spacing. Clustering: same-category plants form groups of 3-8. Path proximity: plants within 0.5 units of paths avoided. Growth: newly placed plants start at 30% scale, grow to 100% over 24-72 hours (or instant in education mode).

### 15.6 Growth Animation Per Plant Type

Seed-to-plant growth: 3-10 seconds animation, stem extends at 0.2 units/s, leaves unfurl at 0.5s per leaf, bloom opens over 2 seconds. Growth triggered on new topic completion.

### 15.7 Interactive Watering/Tending

Watering: particle spray from tap point, plant responds with 0.5s growth pulse, glow effect (2s fade). Tending: pruning removes dead leaves (particle dissolve), fertilizing triggers growth burst (1.5x speed for 10s).

### 15.8 Garden Decorations

Benches (1.5K tris, wood/stone, 3 seat variants), fountains (3K tris, water particle, Tier 1-2), pavilions (5K tris, open structure, Tier 1), lanterns (500 tris, warm emission), stepping stones (200 tris each, placed path-adjacent), decorative urns (1K tris, with/without plants).

### 15.9 Lighting (Golden Hour Consistent)

Directional light: 15 degrees above horizon, azimuth 45 degrees from camera default. Color: warm (#FFD080). Intensity: 0.8. Ambient: warm sky (#FFE8C0) at 0.3 intensity, ground (#C0A080) at 0.1. Shadows: soft, 1024 shadow map, 2-unit bias. Fog: warm cream (#F5E8D0) at 30-50 unit distance. Always golden hour — never changes.

---

## 16. Educational Simulations

### 16.1 3D Interactive Learning Simulation Standards

All simulations include: learning objective displayed before start, step-by-step guided mode, free exploration mode, adjustable speed control, information labels on all components, reset to initial state, progress indicators, optional assessment questions at checkpoints. Minimum 30fps on target mobile devices.

### 16.2 Astronomy Simulations

**Solar System Orrery:** Sun-centered model with all known historical planets. Orbital speeds accurate to relative proportions. Planet labels with size, distance, orbital period. Gear mechanism visible when zoomed. User can grab and drag planets to adjust time scale.

**Celestial Sphere Navigation:** Star sphere with 48 classical constellations from Al-Sufi. User rotates sphere to see how stars move through night sky. Celestial coordinate grid (equatorial and ecliptic). Star pointer tool identifies stars on tap. Altitude/azimuth display.

**Lunar Phase Simulation:** Moon orbiting Earth with phase-appropriate illumination. Sun direction indicator. Phase name displayed. User adjusts time to see phase progression over 29.5 day cycle.

### 16.3 Engineering Simulations

**Gear Systems:** Interactive gear train with 2-8 gears. User rotates crank to see power transmission. Gear ratio calculation displayed. Torque and speed visualization. Exploded view option. Historical context: Al-Jazari's gear mechanisms.

**Water Wheels:** Noria model showing water lifting mechanism. Water flow particles from source to destination. Efficiency display. Historical context: Syrian and Mesopotamian water wheels.

**Clock Mechanisms:** Weight-driven or water-driven clock with escapement. Gear train visible through transparent cover. Pendulum animation. Hour striking mechanism. Historical context: Al-Jazari's castle clock.

### 16.4 Medicine Simulations

**Human Anatomy Layers:** 5-8 layer system (skin, muscle, skeleton, organs, circulatory, nervous). User peels layers via slider or tap. Layer labels with descriptions. Historical context: Ibn Sina's Canon illustrations.

**Surgical Procedure Visualization:** Step-by-step historical surgical procedure (cauterization, bloodletting, wound suturing). Instruments displayed with labels. Precautions and historical context. Al-Zahrawi's techniques.

### 16.5 Mathematics Simulations

**Geometric Solid Manipulation:** Platonic and Archimedean solids user can rotate, scale, and deconstruct. Unfold net animation shows 2D pattern. Face, edge, vertex counts displayed. Euler's formula visualization.

**Conic Sections:** Cone with adjustable cut plane angle. Cross-section shape updates in real-time (circle, ellipse, parabola, hyperbola). Degenerate cases shown. Historical context: Ibn al-Haytham's optics.

### 16.6 Physics Simulations

**Optics Experiments:** Light ray reflection and refraction with adjustable angle. Mirror, lens, and prism models. Focal point visualization. Historical context: Ibn al-Haytham's Book of Optics, camera obscura demonstration.

**Mechanics Demonstrations:** Lever systems (three classes), pulley systems, inclined plane. Force vectors displayed. Mechanical advantage calculation. Historical context: Banu Musa's Book of Ingenious Devices.

### 16.7 Chemistry Simulations

**Alchemical Apparatus:** Distillation (alembic heating, vapor condensation collection), sublimation (solid to vapor to solid), calcination (furnace with temperature indicator). Step labels with chemical process descriptions. Historical context: Jabir ibn Hayyan experiments.

**Distillation Process:** Heat source under cucurbit, vapor rises through alembic, condenses in cooling tube, collected in receiver. Temperature-sensitive color changes. Safety indicators.

### 16.8 Simulation Technical Specifications

| Property | Specification |
|----------|--------------|
| Target framerate | 30fps minimum on mobile, 60fps desktop |
| Interaction latency | <100ms from input to visual response |
| Step count per simulation | 5-15 guided steps |
| Free exploration time | Unlimited (user-controlled) |
| Reset time | <1 second |
| Label count per simulation | 10-30 component labels |
| Animation complexity | 2-10 simultaneous animated elements |
| Physics accuracy | Real-time approximate (not real-time simulation) |
| Particle systems | 1-3 per simulation (water, fire, light rays) |
| Audio cues | Component click, step complete, success chime |

### 16.9 Simulation Interaction Design

All educational simulations follow a consistent interaction pattern:
- Enter simulation: camera positions at overview angle showing full apparatus
- Step panel: left side shows numbered steps, current step highlighted
- Component highlight: current interactive element pulses with emissive glow
- Drag/tap interaction: user manipulates the highlighted component
- Success feedback: component snaps to correct position, green checkmark, audio chime
- Auto-advance: after successful interaction, next step highlights after 1.5s delay
- Info button: each step has optional detailed explanation panel
- Speed control: 0.5x, 1x, 2x speed for animated sequences
- Reset: returns all components to initial state with 1-second transition

### 16.10 Simulation Performance Budgets

| Simulation Type | Max Tris (All Objects) | Max Draw Calls | Max Texture Memory | Max Particles |
|----------------|----------------------|---------------|-------------------|---------------|
| Astronomy | 80,000 | 60 | 32MB | 500 |
| Engineering | 100,000 | 80 | 48MB | 1000 |
| Medicine | 120,000 | 70 | 64MB | 200 |
| Mathematics | 50,000 | 40 | 16MB | 100 |
| Physics | 60,000 | 50 | 24MB | 800 |
| Chemistry | 70,000 | 55 | 32MB | 1500 |

### 16.11 Simulation Testing Protocol

Each simulation must pass the following tests before release:
- Functional test: all steps complete successfully, all interactions respond
- Performance test: framerate meets target on reference devices (iPhone 13, Pixel 6, desktop Chrome)
- Edge case test: rapid clicking, dragging outside bounds, reset during animation
- Accessibility test: screen reader compatibility, reduced motion support, color blind friendly
- Educational test: content expert verifies accuracy of all information and animations
- Localization test: all labels and text display correctly in supported languages (English, Arabic, Turkish, Urdu, Malay, French, Spanish)

**Alchemical Apparatus:** Distillation (alembic heating, vapor condensation collection), sublimation (solid to vapor to solid), calcination (furnace with temperature indicator). Step labels with chemical process descriptions. Historical context: Jabir ibn Hayyan experiments.

**Distillation Process:** Heat source under cucurbit, vapor rises through alembic, condenses in cooling tube, collected in receiver. Temperature-sensitive color changes. Safety indicators.

---

## 17. Material Library

### 17.1 Material Library Overview

The material library is a centralized repository of all authorized PBR materials used across the 3D platform. Each material is a Unity Material asset based on the URP Lit shader, with consistent parameter naming and organization. Materials are stored in `Assets/03_3D_Assets/Materials/` organized by category (Stone, Metal, Ceramic, Glass, Textile, Wood, Paper, Water). Each material has a source .sbsar (Substance Designer) or .spp (Substance Painter) file for procedural generation, and baked output textures at standardized resolutions.

### 17.2 Material Library Standards

All materials follow PBR (Physically Based Rendering) standards compatible with Unity URP and Three.js. Each material is defined by: Albedo (base color), Normal map (surface detail), ORM map (Occlusion/Roughness/Metalness packed), and optional Emission map. Materials are stored as Unity Material assets with URP Lit shader as base.

### 17.2 Stone Materials

| Material | Albedo Base | Roughness | Normal Strength | Occlusion | Notes |
|----------|-------------|-----------|-----------------|-----------|-------|
| Marble (white) | #E8E0D0 | 0.3-0.5 | 0.4 | 0.8 | Subtle veining in normal |
| Marble (green) | #A0B090 | 0.3-0.5 | 0.4 | 0.8 | Verde antico variant |
| Marble (red) | #C07060 | 0.3-0.5 | 0.4 | 0.8 | Rosso antico variant |
| Limestone | #D0C8B8 | 0.6-0.8 | 0.5 | 0.9 | Slight fossil noise |
| Sandstone | #C0A880 | 0.7-0.9 | 0.6 | 0.9 | Grainy surface noise |
| Granite | #8A8878 | 0.4-0.6 | 0.7 | 0.8 | Speckled pattern |
| Basalt | #4A4840 | 0.6-0.8 | 0.8 | 0.9 | Fine grain, dark |
| Brick (fired) | #B06040 | 0.7-0.9 | 0.5 | 0.8 | Mortar color separate |
| Plaster/stucco | #E0D8C8 | 0.6-0.8 | 0.3 | 0.7 | Fine texture |

### 17.3 Metal Materials

| Material | Albedo Color | Metallic | Smoothness | Normal Strength | Notes |
|----------|-------------|----------|------------|-----------------|-------|
| Brass (polished) | #C8A040 | 1.0 | 0.8-0.9 | 0.3 | Warm gold reflection |
| Brass (aged) | #8A7030 | 1.0 | 0.3-0.5 | 0.5 | Patina variations |
| Bronze | #C07040 | 1.0 | 0.5-0.7 | 0.4 | Copper-tin alloy |
| Bronze (patina) | #4A7A5A | 1.0 | 0.2-0.4 | 0.5 | Green oxidation areas |
| Copper | #C07040 | 1.0 | 0.6-0.8 | 0.3 | Reddish metallic |
| Copper (aged) | #5A7A6A | 1.0 | 0.3-0.5 | 0.5 | Green-blue patina |
| Gold | #E0C040 | 1.0 | 0.8-0.9 | 0.2 | Bright yellow reflection |
| Silver | #D0D0E0 | 1.0 | 0.8-0.9 | 0.2 | Cool white reflection |
| Silver (tarnished) | #8A8A9A | 1.0 | 0.3-0.5 | 0.4 | Darkened areas |
| Iron | #807070 | 1.0 | 0.3-0.5 | 0.5 | Dark grey metallic |
| Steel | #909090 | 1.0 | 0.5-0.7 | 0.4 | Mid grey, slight blue |
| Steel (Damascus) | #7A7A7A | 1.0 | 0.6-0.8 | 0.8 | Pattern weld in normal |

### 17.4 Ceramic Materials

| Material | Albedo Base | Smoothness | Metallic | Normal | Notes |
|----------|-------------|------------|----------|--------|-------|
| Glazed tile | #varies by color | 0.6-0.8 | 0.0 | 0.3 | High gloss surface |
| Unglazed pottery | #C0A080 | 0.3-0.5 | 0.0 | 0.6 | Matte clay texture |
| Porcelain | #F0E8E0 | 0.7-0.9 | 0.0 | 0.2 | Translucent white |
| Terracotta | #C07050 | 0.4-0.6 | 0.0 | 0.5 | Reddish clay |
| Faience | #80B0A0 | 0.6-0.8 | 0.0 | 0.3 | Egyptian blue-green |
| Iznik tile | #varies | 0.6-0.8 | 0.0 | 0.4 | Multi-color pattern |

### 17.5 Glass Materials

| Material | Albedo | Opacity | Smoothness | Metallic | Notes |
|----------|--------|---------|------------|----------|-------|
| Clear glass | #E8F0F8 | 0.85 | 0.9 | 0.0 | Subtle blue tint |
| Stained glass | #varies | 0.6-0.8 | 0.8 | 0.0 | Saturated color |
| Faceted crystal | #F0F0FF | 0.9 | 0.95 | 0.0 | Prismatic refraction |
| Frosted glass | #E8E8F0 | 0.5-0.7 | 0.3 | 0.0 | Rough normal |
| Glass with gold | #varies + gold | 0.4-0.6 | mixed | mixed | Gold leaf mask |

### 17.6 Textile Materials

| Material | Albedo Pattern | Roughness | Normal | Sheen | Notes |
|----------|---------------|-----------|--------|-------|-------|
| Silk | #varies, saturated | 0.2-0.4 | 0.2 | Anisotropic | Sheen direction |
| Cotton | #varies, muted | 0.6-0.8 | 0.4 | None | Matte weave |
| Wool | #varies, warm | 0.7-0.9 | 0.6 | None | Fibrous normal |
| Carpet (woven) | geometric pattern | 0.7-0.8 | 0.7 | None | Pile direction |
| Velvet | #deep saturated | 0.8-0.9 | 0.5 | Anisotropic | Nap directional |
| Brocade | gold pattern on base | 0.3-0.5 | 0.5 | Anisotropic | Gold thread |
| Tapestry | narrative pattern | 0.7-0.8 | 0.6 | None | Thread visible |

### 17.7 Wood Materials

| Material | Albedo Base | Roughness | Normal | Grain | Notes |
|----------|-------------|-----------|--------|-------|-------|
| Oak (carved) | #8A7050 | 0.5-0.7 | 0.6 | Straight | For carving |
| Cedar | #A08060 | 0.5-0.7 | 0.5 | Fine straight | Aromatic |
| Walnut | #5A4030 | 0.4-0.6 | 0.5 | Wavy | Dark rich |
| Teak | #7A6040 | 0.4-0.6 | 0.5 | Moderate | Water resistant |
| Mahogany | #6A4030 | 0.3-0.5 | 0.4 | Interlocked | Red-brown |
| Inlaid wood | #varies | 0.4-0.6 | 0.4 | Mixed | Multiple species |
| Paneled | #varies | 0.4-0.6 | 0.5 | Framed | With frame detail |

### 17.8 Paper Materials

| Material | Albedo | Roughness | Normal | Notes |
|----------|--------|-----------|--------|-------|
| Parchment | #F0E0C0 | 0.7-0.8 | 0.5 | Aged yellowing |
| Papyrus | #D4C0A0 | 0.7-0.9 | 0.7 | Fibrous texture |
| Aged paper | #E8D8B8 | 0.6-0.8 | 0.4 | Foxing spots, edge wear |
| Illuminated | #F0E8D0 + gold | 0.5-0.7 | 0.3 | Gold leaf areas metallic |
| Modern paper | #F8F8F0 | 0.5-0.7 | 0.2 | Clean bright |

### 17.9 Water Materials

| Material | Base Color | Opacity | Smoothness | Normal | Notes |
|----------|-----------|---------|------------|--------|-------|
| Clear water | #3A8A8A | 0.7-0.9 | 0.8 | 0.3 | Pool/fountain |
| Ocean water | #2A6A7A | 0.6-0.8 | 0.7 | 0.5 | Wave normal animation |
| River water | #4A8A6A | 0.6-0.8 | 0.7 | 0.4 | Flow direction |
| Canal water | #5A8A6A | 0.6-0.8 | 0.6 | 0.3 | Calm flow |
| Reflecting pool | #3A7A7A | 0.7-0.9 | 0.9 | 0.2 | Still, mirror-like |

---

## 18. Lighting Guide

### 18.1 Lighting Philosophy

Lighting on the platform serves three purposes: establishing mood (warm, inviting, scholarly), directing attention (highlighting important elements), and communicating time/place (historical period, time of day). The lighting style is warm and gentle — never harsh, cold, or dramatic outside of intentional educational demonstrations.

### 18.2 Single Primary Light Source Direction

All exterior scenes use a single directional light as the primary illumination source. The light direction is consistent across the platform: 45 degrees azimuth (southwest, top-left relative to default camera), 30-45 degrees elevation. This creates consistent shadow direction and highlight placement across all assets. Secondary fill light is a hemisphere ambient at 20-30% intensity.

### 18.3 Warm Temperature Specifications

| Context | Color Temperature | Kelvin Range | Light Color | Notes |
|---------|-------------------|-------------|-------------|-------|
| Exterior (golden hour) | Warm | 3500-4500K | #FFD080 to #FFC060 | Default exterior lighting |
| Interior (daylight) | Neutral-warm | 4000-5000K | #FFE0A0 | Through windows/open doors |
| Interior (lamp/candle) | Very warm | 2000-3000K | #FFA040 to #FF8030 | Point lights on furniture |
| Interior (oil lamp) | Warm | 2500-3500K | #FFB050 | Point light, 1-3m range |
| Museum display | Warm accent | 3500-4500K | #FFD090 | Spotlight on artifacts |
| Night exterior | Cool-warm | 4500-5500K | #C0D0FF | Moonlight, blue-tinted |
| Dawn | Warm-cool | 4000-5000K | #FFC0A0 | Transitional |

### 18.4 Natural Lighting for Exteriors

Time of day system for historical environments: adjustable from dawn (5:00) to dusk (19:00), default at 16:00 (golden afternoon). Sun azimuth rotates 360 degrees over 24 hours. Sun elevation from -10 degrees (night) to 80 degrees (noon) depending on latitude. Latitude of scene location affects sun angle and day length. Cloud cover affects light intensity (20-50% reduction in overcast). Skybox transitions through color gradients matched to time.

### 18.5 Candle/Lamp Lighting for Interiors

Interior scenes without windows use point lights at light source locations. Candle light: 1-2m range, 2000-3000K, intensity 2-5 nits. Lamp (oil): 2-4m range, 2500-3500K, intensity 5-10 nits. Lantern: 3-6m range, 3000-4000K, intensity 10-20 nits. Multiple light sources in a room: 2-8 point lights depending on room size. Light flicker: subtle intensity variation (5-15% at 1-3Hz) for flame sources.

### 18.6 Rim Light for Character Separation

Characters (avatar, NPCs) receive a rim light from behind at 180 degrees opposite the primary light. Rim light intensity: 30-50% of primary. Rim light color: warm (matching environment) for exterior, cool (matching ambient) for interior. Rim light width: 30-60 degrees from edge. Rim light creates silhouette definition against backgrounds.

### 18.7 Ambient Occlusion for Depth

Screen-space ambient occlusion (SSAO) is enabled for desktop builds at medium quality. Mobile: baked ambient occlusion is used. AO intensity: 0.5-0.8. AO radius: 0.5-2m. Baked AO is pre-computed into the ORM map occlusion channel. Distant objects use distance-based AO fade to reduce banding.

### 18.8 Baked Lighting for Mobile Performance

All static geometry receives baked lightmaps for mobile builds. Lightmap resolution: 512-2048 texels per unit depending on scene size. Lightmap padding: 4 texels minimum. Lightmap format: RGBA half-float. UV2 channel (lightmap UVs) must be generated for all static meshes. Baked lighting includes: direct light, indirect bounce, ambient occlusion. Dynamic objects receive real-time lighting from primary directional light only.

### 18.9 Dynamic Lighting for Interactive Elements

Interactive elements with dynamic lighting: selection highlights (emissive pulse on interactive objects), particle systems (self-illuminated, no light cast), nameplates/labels (unlit or self-illuminated). Dynamic point lights are used sparingly — maximum 2 per scene for mobile, 8 for desktop. Dynamic light range: 2-8m. Dynamic lights do not cast shadows on mobile.

### 18.10 Light Color Moods Per Learning Mode

| Learning Mode | Primary Light | Secondary Light | Mood | Color Strategy |
|--------------|--------------|-----------------|------|---------------|
| Exploration | Golden hour (3500K) | Warm ambient | Inviting, curious | Amber high, teal shadows |
| Focus/Reading | Warm lamp (2800K) | Focused spot on area | Calm, concentrated | Warm cream, low saturation |
| Simulation | Neutral (4500K) | Task-specific | Alert, engaged | White-balanced for science |
| Museum | Warm spot (3500K) | Cool ambient (5000K) | Reverent, focused | Warm on artifact, cool surround |
| Garden | Golden hour (3500K) | Warm ambient | Peaceful, content | Warm green, amber light |
| Achievement | Warm gold (3000K) | Particle glow | Celebratory, proud | Gold hues, sparkle |

---

## 19. Optimization Guide

### 19.1 Poly Count Budgets Per Asset Tier

| Tier | Classification | Max Tris | LOD Levels | Use Case |
|------|---------------|----------|-----------|----------|
| Tier 1 | Hero | 50,000 | 4 | Avatar, hero buildings, hero artifacts |
| Tier 2 | Important | 20,000 | 4 | Historical buildings, key artifacts, NPCs |
| Tier 3 | Standard | 8,000 | 3 | Secondary buildings, props, furniture |
| Tier 4 | Background | 3,000 | 2 | Distant buildings, terrain chunks |
| Tier 5 | Distant | 500 | 1 | Billboard impostors, sky elements |

LOD reduction rules: LOD1 = 50% of base tris, LOD2 = 20%, LOD3 = 5%, LOD4 = 1%. Each reduction must maintain visual silhouette.

### 19.2 Texture Budgets

| Asset Tier | Max Texture Resolution | Max Total Memory | Texture Count |
|------------|----------------------|-----------------|---------------|
| Hero | 2048x2048 | 32MB | 4 (Albedo, Normal, ORM, Emission) |
| Standard | 1024x1024 | 4-8MB | 2-3 |
| Small | 512x512 | 1MB | 1-2 |
| Tiny | 256x256 | 0.25MB | 1 |

Texture compression: ASTC 4x4 for mobile, BC7/BC3 for desktop. ETC2 fallback for legacy devices. All textures must have mipmaps. Texture streaming enabled for environments.

### 19.3 Draw Call Budgets Per Scene

Museum interior: 150 max, city wide: 300 max, historical location: 250 max, garden: 100 max, simulation: 80 max, avatar screen: 50 max. Achieve via static batching, GPU instancing for repeated elements, material atlasing for environments with 20+ materials.

### 19.4 Atlas Textures

Texture atlasing is required when an environment has more than 20 unique materials. Atlas size: 4096x4096 maximum. Atlas packing: auto-pack via tool (TexturePacker or similar). Elements must have 2 texel padding. Atlas includes: building facades, tile patterns, vegetation, ground detail. Atlas updates require full rebuild of dependent asset bundles.

### 19.5 LOD Switching Distances

| Asset Scale | LOD0 | LOD1 | LOD2 | LOD3 |
|-------------|------|------|------|------|
| Large (building 20m+) | 0-50m | 50-100m | 100-200m | 200m+ |
| Medium (human 1.7m) | 0-10m | 10-25m | 25-50m | 50m+ |
| Small (artifact 0.5m) | 0-3m | 3-8m | 8-20m | 20m+ |
| Tiny (coin 0.03m) | 0-1m | 1-3m | 3-8m | 8m+ |

Cross-fade duration: 0.3-0.5 seconds using dithering or alpha blend.

### 19.6 Mobile-Specific Optimizations

| Optimization | Technique | Savings | Impact |
|-------------|-----------|---------|--------|
| Decal rendering | Face features as decal texture | 2K tris saved | Minimal at viewing distance |
| Impostors | Billboard renders for distant objects | 90% tris reduction | Acceptable beyond 100m |
| Texture streaming | Load textures by distance | 50% memory reduction | None visible |
| Vertex budget | Reduce subdivision on mobile | 30% tris reduction | LOD2 quality target |
| Particle cap | Max 200 particles (vs 1000 desktop) | 80% GPU savings | Reduced density |
| No post-processing | Disable bloom, SSAO, shadows | 30% GPU savings | Flat appearance |
| Single pass instancing | SRP Batcher enabled | 50% draw call reduction | None |
| GPU skinning | Compute shader skinning | CPU savings | None |

### 19.7 WebGL Optimization Tips

| Concern | Solution | Implementation |
|---------|----------|---------------|
| Bundle size | Draco mesh compression | Compress all glTF meshes with Draco |
| Texture memory | Basis Universal textures | Compress to .ktx2 with ETC1S |
| Draw calls | Merge meshes by material | Pre-merge static environment meshes |
| Shader complexity | Limit to 4 texture samples | Use combined ORM maps |
| Animation | Remove unused bones | Export only deforming bones |
| Loading time | Progressive loading | Load visible assets first |
| Mobile WebGL | Reduce overdraw | Disable transparent objects in distance |
| Shadows | Disable or 512 map | Only on hero assets |

### 19.8 Mobile GPU Performance Targets

| GPU Family | Min FPS | Quality Level | Features Enabled | Features Disabled |
|-----------|--------|--------------|-----------------|-------------------|
| Apple A15+ (iPhone 13+) | 60 | High | Full LODs, 2048 textures, shadows, particles | None |
| Apple A13-A14 (iPhone 11-12) | 45 | Medium | LOD0-2, 1024 textures, baked shadows, reduced particles | Real-time shadows, bloom |
| Apple A11-A12 (iPhone 8-XS) | 30 | Low | LOD1-3, 512 textures, no shadows, 50% particles | Real-time shadows, post-process, reflections |
| Snapdragon 8 Gen 1+ | 60 | High | Full LODs, 2048 textures, shadows, particles | None |
| Snapdragon 888-8 Gen 1 | 45 | Medium | LOD0-2, 1024 textures, baked shadows, reduced particles | Real-time shadows, bloom |
| Snapdragon 845-865 | 30 | Low | LOD1-3, 512 textures, no shadows, 50% particles | Real-time shadows, post-process, reflections |
| Mali-G710+ | 45 | Medium | LOD0-2, 1024 textures, baked shadows | Real-time shadows, bloom |
| Mali-G72-G610 | 30 | Low | LOD1-3, 512 textures, no shadows | All post-processing |

### 19.9 Memory Budgets Per Scene

| Scene Type | Total RAM Budget | GPU Memory Budget | Texture Memory | Mesh Memory | Audio Memory |
|------------|-----------------|-------------------|---------------|-------------|-------------|
| Museum interior | 200MB | 150MB | 80MB | 40MB | 30MB |
| City environment | 400MB | 300MB | 150MB | 100MB | 50MB |
| Historical location | 350MB | 250MB | 120MB | 80MB | 50MB |
| Garden/Memory Garden | 150MB | 100MB | 50MB | 30MB | 20MB |
| Interactive simulation | 200MB | 120MB | 60MB | 40MB | 20MB |
| Avatar customization | 100MB | 80MB | 40MB | 30MB | 10MB |

Memory budgets include the base application overhead of approximately 80-120MB. Texture streaming reduces peak memory by loading only visible textures at full resolution. Animation data is loaded on-demand and unloaded when not in use.

### 19.10 Performance Profiling Tools

| Platform | Tool | Key Metrics | Target Values |
|----------|------|-------------|---------------|
| iOS | Xcode Instruments GPU Capture | Frame time, draw calls, GPU utilization | <33ms frame time |
| iOS | Metal Debugger | Shader performance, texture bandwidth | <500MB/s bandwidth |
| Android | Snapdragon Profiler | GPU load, memory, draw calls | <50% GPU load |
| Android | Android GPU Inspector | Frame breakdown, shader cycles | <16ms GPU frame |
| Unity | Profiler (Editor) | Draw calls, tris, setpass calls | Per-scene budgets |
| Unity | Frame Debugger | Draw call breakdown, batching | Static batch ratio >70% |
| Web | Chrome DevTools Performance | FPS, memory, GPU time | >30fps sustained |
| Web | Three.js Inspector | Draw calls, geometry, textures | Per-scene budgets |

### 19.11 Optimization Checklist

Before release, every scene must pass optimization review:
- [ ] All meshes within tier polygon budget
- [ ] LODs generated and correctly configured
- [ ] Textures at correct resolution and compression
- [ ] Draw calls within scene budget
- [ ] Static batching applied
- [ ] GPU instancing for repeated elements
- [ ] Occlusion culling data generated
- [ ] Lightmaps baked
- [ ] No missing or placeholder textures
- [ ] No unused assets in build
- [ ] Mobile device profile tested (30fps minimum)
- [ ] WebGL bundle size verified

---

## 20. Production Pipeline

### 20.1 Pipeline Overview

The 3D asset production pipeline consists of 12 sequential stages. Each stage must be completed and signed off before the next begins. Stage gates prevent wasted work on assets that have not passed prerequisite reviews.

### 20.2 Pipeline Stages

**Stage 1: Concept**
- Task: Create concept art, collect reference images, define asset scope
- Deliverable: Concept sheet with 3+ views, reference board, written brief
- Approver: Art Director
- Duration: 2-5 business days
- Gate: Concept approved, tier assigned, period/region verified

**Stage 2: Modeling**
- Task: Create 3D mesh from concept, base topology, subdivision
- Deliverable: High-poly mesh (if baking), low-poly mesh, wireframe screenshots
- Approver: Lead Modeler
- Duration: 5-20 business days (complexity dependent)
- Gate: Mesh within poly budget, topology clean, proportions verified against references

**Stage 3: UV Mapping**
- Task: Unwrap UVs, layout UV shells, pack within 0-1 space
- Deliverable: UV layout screenshot, UV overlap check, texel density verification
- Approver: Technical Artist
- Duration: 1-3 business days
- Gate: No overlapping UVs, no stretching >5%, texel density consistent, padding sufficient

**Stage 4: Texturing**
- Task: Create albedo, normal, ORM, emission textures in Substance Painter/Designer
- Deliverable: All texture maps at correct resolution, PBR material asset
- Approver: Lead Texture Artist
- Duration: 3-10 business days
- Gate: All maps present, correct resolution, color-accurate, PBR values verified

**Stage 5: Rigging (if animated)**
- Task: Create skeleton, bind mesh, weight paint
- Deliverable: Rigged mesh, bone hierarchy, weight paint screenshots
- Approver: Technical Animator
- Duration: 2-5 business days
- Gate: Clean deformations, bone naming correct, max 4 weights per vertex

**Stage 6: Animation**
- Task: Create animation clips (idle, interaction, growth, etc.)
- Deliverable: Animation FBX files, animation list
- Approver: Lead Animator
- Duration: 3-10 business days
- Gate: Smooth motion, no foot sliding, correct timing

**Stage 7: LOD Creation**
- Task: Generate LOD meshes, decimate or retopologize
- Deliverable: LOD0-LOD3 meshes, LOD transition distances
- Approver: Technical Artist
- Duration: 1-3 business days
- Gate: LODs within budget, no visible popping at transition distances

**Stage 8: Export**
- Task: Export to FBX/glTF, verify import settings, check transforms
- Deliverable: FBX with correct settings (forward -Z, up +Y, meters, scale 1.0)
- Approver: Technical Artist
- Duration: 0.5-1 business day
- Gate: All transforms baked, no vertex color data, correct axis orientation

**Stage 9: Import to Engine**
- Task: Import FBX into Unity, create prefab, assign materials, configure LOD group
- Deliverable: Unity prefab in correct directory, material assignments verified
- Approver: Technical Artist
- Duration: 0.5-1 business day
- Gate: Prefab renders correctly, materials display correctly, LODs functional

**Stage 10: Lighting Test**
- Task: Place asset in scene lighting conditions, verify look
- Deliverable: Screenshots in scene context, lightmap bake test
- Approver: Lighting Artist
- Duration: 0.5-1 business day
- Gate: Asset looks correct in scene lighting, shadows cast correctly

**Stage 11: Optimization Pass**
- Task: Verify poly budget, texture memory, draw calls, LODs
- Deliverable: Optimization report, frame time measurements
- Approver: Technical Art Director
- Duration: 1-2 business days
- Gate: All optimization targets met, mobile performance verified

**Stage 12: QA Review**
- Task: Visual inspection, functional testing, platform testing
- Deliverable: QA sign-off document
- Approver: QA Lead
- Duration: 1-3 business days
- Gate: No visual errors, no performance issues, all features functional, accessibility verified

### 20.3 Software Specifications

| Software | Version | Purpose | License Type |
|----------|---------|---------|-------------|
| Blender | 3.6+ / 4.0+ | Primary modeling, UV, rigging | Open source |
| Maya | 2024+ | Secondary modeling, complex rigging | Per-seat license |
| 3ds Max | 2024+ | Architectural modeling (legacy) | Per-seat license |
| ZBrush | 2023+ | High-poly sculpting, detail baking | Per-seat license |
| Substance Painter | 2023+ | PBR texturing | Per-seat license |
| Substance Designer | 2023+ | Procedural material authoring | Per-seat license |
| Photoshop | 2024+ | Texture compositing, editing | Per-seat license |
| Unity | 2022 LTS | Primary engine, prefab assembly | Per-seat license |
| Marmoset Toolbag | 2023+ | Baking, portfolio renders | Per-seat license |
| SpeedTree | 2023+ | Vegetation generation | Per-seat license |
| World Machine | 2023+ | Terrain generation | Per-seat license |
| Houdini | 19.5+ | Procedural asset generation | Per-seat license |

### 20.4 File Naming Conventions

All 3D asset files follow strict naming conventions for organization and version control.

**Pattern:** `{Category}_{AssetName}_{Variant}_{Type}_v{version}.{ext}`

**Category Codes:**
- `BLD` — Buildings (mosques, palaces, fortresses)
- `LOC` — Historical locations (city environments)
- `ART` — Artifacts (astrolabes, instruments, pottery)
- `TER` — Terrain and landscape
- `BOK` — Books and scrolls
- `LIB` — Libraries and museums
- `INT` — Interactive models
- `TRE` — Knowledge tree
- `AVA` — Avatar
- `SPR` — Knowledge spirit
- `GRD` — Memory garden
- `SIM` — Educational simulations
- `MAT` — Materials
- `PRO` — Props and decorations

**Type Suffixes:**
- `_HP` — High-poly mesh (sculpting source)
- `_LP` — Low-poly mesh (game-ready)
- `_UV` — UV layout reference
- `_COL` — Collision mesh
- `_RIG` — Rigged mesh
- `_ANIM` — Animation file
- `_LOD0` through `_LOD4` — LOD meshes
- `_ALB` — Albedo texture
- `_NRM` — Normal map
- `_ORM` — Occlusion/Roughness/Metalness
- `_EMI` — Emission map
- `_MSK` — Mask texture
- `_PRE` — Prefab

**Examples:**
- `BLD_UmayyadMosque_Damascus_LP_v01.fbx`
- `ART_Astrolabe_Baghdad_LOD0_v02.fbx`
- `AVA_Avatar_Base_HP_v03.ztl`
- `BLD_UmayyadMosque_Damascus_ALB_v01.tif`

### 20.5 Version Control for 3D Assets

All 3D assets are stored in Perforce Helix Core (version control). Unity assets are additionally managed via Unity Version Control (formerly Plastic SCM).

**Repository Structure:**
```
//depot/3D_Assets/
  {Category}/
    {AssetName}/
      source/           -- .blend, .mb, .ztl source files
      export/           -- .fbx, .glb export files
      textures/         -- .tif, .png texture sources
      materials/        -- .mat material files
      prefabs/          -- .prefab Unity prefabs
      reference/        -- reference images, plans
      reviews/          -- review screenshots, feedback
```

**Versioning Rules:**
- Major version (v01, v02): significant mesh/UV changes
- Minor version (v01a, v01b): texture-only changes, minor tweaks
- Submit with description of changes
- Lock files when working to prevent conflicts
- Branch for experimental variants
- Tag releases with milestone names
- All binary files with LFS (Large File Storage)

### 20.6 Asset Approval Sign-Off Form

Each asset requires a formal sign-off at the completion of each pipeline stage. The sign-off form includes: asset name, unique asset ID, artist name, reviewer name, stage name, approval status (Approved / Approved with Changes / Rejected), reviewer comments, date stamp. Sign-off forms are stored in the asset's `reviews/` directory as PDF files with the naming pattern: `{AssetID}_SignOff_{Stage}_v{version}.pdf`.

### 20.7 Emergency Pipeline Variants

Two emergency pipeline variants exist for time-critical assets. Fast-track pipeline: concept approval combined with modeling start, UV and texturing overlap by 1-2 days, QA reduced to 1 day. 25% time reduction, applies to Tier 3-4 assets only. Critical hotfix pipeline: model directly from reference (no concept), UV from existing similar asset modified, LOD generation automated. 50% time reduction, applies only to Tier 4-5 assets or bug-fix geometry changes. Both require Art Director and Production Manager approval before initiation.

---

## Appendix A: 3D Asset Bible Change Log

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0.0 | 2026-07-10 | 3D Production Team | Initial release — complete 20-section 3D production bible covering all asset categories, technical specifications, production pipeline, and optimization guidelines |

## Appendix B: Platform-Wide 3D Asset Audit Cross-Reference

All 3D assets produced under this bible are tracked in the Visual Asset Audit (00_Visual_Asset_Audit.md) with their tier assignment, production status, and optimization review results. Refer to the audit for current production progress and backlog. New asset requests should follow the process defined in Visual_Production/07_Asset_Pipeline/07_Asset_Pipeline.md.
