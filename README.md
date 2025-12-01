# PokedexWebApp
A responsive web app using Svelte, HTML, CSS, and JavaScript. Integrated REST APIs for live Pokémon data, implemented search, filtering, and animations, and designed an engaging user interface with interactive elements. Showcased frontend development, UI/UX design, and modern web architecture skills.

# Technical Report

## 1. Component Architecture

### Component Breakdown

**`+page.svelte` (Main Controller)**
- Manages all state (pokemon list, selected pokemon, search/filter)
- Handles API calls via `fetchList()` and `fetchDetails()`
- Coordinates data flow between components

**`PokedexList.svelte`**
- Renders search box, generation filter, and scrollable pokemon list
- Dispatches events for user interactions (select, search, filter)
- Pure presentation - no data management

**`GenSelect.svelte`**
- Generation filter dropdown (All, Gen 1-9)
- Extracted for reusability and clean separation

**`PokemonDetails.svelte`**
- Main content area orchestrating detail display
- Handles placeholder state when nothing selected
- Coordinates image, types, and entry sub-components

**`PokemonImageBox.svelte`**
- Normal/shiny image toggle with star button
- Manages image switching animations
- Encapsulates complex toggle logic

**`TypeBadge.svelte`**
- Individual type badges with proper colors/icons
- Handles 18 different type color schemes
- Highly reusable across multiple types per pokemon

**`PokedexEntry.svelte`**
- Displays pokedex description
- Handles loading/error states
- Formats text (converts `\n` to `<br/>`)

### CSS Strategy

**Global (`app.css`)**: Fonts, body styles, container layout, shared button/list styles, type colors, animations, responsive breakpoints. These were ported from Part A and apply across multiple components.

**Local**: Component-specific tweaks (pokeball animation, minor positioning). Prevents style leakage and maintains encapsulation.

**Justification**: This structure maintains consistency with Part A while embracing Svelte's component-scoped styling where appropriate.

---

## 2. Data Fetching

**Pokemon List** - Fetched in `+page.svelte`:
- On mount and when generation changes
- Enables single source of truth, easy filtering updates

**Pokemon Details** - Fetched in `+page.svelte`:
- Triggered by list item clicks
- Centralized for consistent loading/error handling

**Why top-level fetching?**
- Predictable data flow (props down, events up)
- Simplified child components (purely presentational)
- Easier debugging and state management

---

## 3. Extra Features

### A) Server-Side Generation Filtering

- `<GenSelect>` dropdown dispatches `changegen` event
- `+page.svelte` updates `gen`, clears selection, calls `fetchList(gen)`
- Fetch URL: `/api/pokedex?gen={number}`
- Server filters data; client applies search afterward

**Benefit**: Reduces data transfer, demonstrates proper API query parameters

### B) Pokemon Cry Sound Effects

- `<audio>` element bound to `audioEl` in `PokemonDetails`
- Custom pokeball button with horizontal bounce animation
- `playCry()` resets audio, plays cry, stops animation at click position
- Animation restarts when new pokemon selected

**Creative touches**: Interactive pokeball "lands" where clicked, disabled state when no pokemon selected

---

## 4. GenAI Usage

**Model**: Claude Sonnet 4.5 (via GitHub Copilot)

**Most Helpful For**:
- Component architecture decisions
- Svelte syntax (event dispatchers, reactive statements)
- API integration patterns
- Quick CSS debugging

**Challenges**:
- Browser compatibility issues (`:has()` selector needed fallback)
- Occasionally over-engineered solutions
- Lost architectural context on complex features
- Required manual tweaking for Part A aesthetic matching

**Key Takeaway**: GenAI dramatically boosted productivity for boilerplate and syntax, but human judgment was essential for architecture, testing, and polish. Best used as a "pair programming" assistant rather than autonomous developer.
