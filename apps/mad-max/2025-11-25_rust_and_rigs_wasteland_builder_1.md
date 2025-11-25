# Rust & Rigs: Wasteland Builder

## Concept

Rust & Rigs is a unique web-based vehicle builder and resource manager designed for the ultimate post-apocalyptic survivor. Users can meticulously craft their ideal wasteland war rigs or nimble desert scouts by selecting various chassis types, armor plating, formidable weaponry (mounted turrets, flamethrowers), engine modifications, and essential utility components like oversized fuel tanks or water purifiers. The application provides real-time tracking of crucial stats such as cumulative weight, fuel efficiency, offensive and defensive ratings, and the total scrap/resource cost for each custom build. With a visual interface and detailed component statistics, Rust & Rigs empowers users to optimize their designs for specific purposes – be it high-stakes trading, daring raids, or long-distance scavenging – ensuring they're always prepared for the chrome-plated chaos of the desert.

## Technical prompt

```md
You are tasked with building a web application called 'Rust & Rigs: Wasteland Builder'. This is a React application styled entirely with Tailwind CSS. Follow these step-by-step instructions:

1.  **Project Setup:**
    *   Initialize a new React project using Vite: `npm create vite@latest rust-n-rigs -- --template react`
    *   Navigate into the project directory: `cd rust-n-rigs`
    *   Install Tailwind CSS and its dependencies: `npm install -D tailwindcss postcss autoprefixer`
    *   Initialize Tailwind CSS: `npx tailwindcss init -p`
    *   Configure `tailwind.config.cjs` to purge React files: `content: ["./index.html", "./src/**/*.{js,ts,jsx,tsx}"],`
    *   Add Tailwind directives to `src/index.css`.
    *   Install `lucide-react` for icons: `npm install lucide-react`

2.  **Data Structures (src/data/):**
    *   Create `src/data/chassis.json`:
        [
          { "id": "chassis-buggy", "name": "Dune Buggy", "imgSrc": "/img/buggy.png", "baseWeight": 500, "cargoCapacity": 50, "maxWeaponSlots": 1, "maxArmorSlots": 2, "baseFuelEfficiency": 0.8, "description": "Fast and light, ideal for scouting." },
          { "id": "chassis-sedan", "name": "Armored Sedan", "imgSrc": "/img/sedan.png", "baseWeight": 1200, "cargoCapacity": 100, "maxWeaponSlots": 2, "maxArmorSlots": 4, "baseFuelEfficiency": 0.6, "description": "Balanced protection and speed." },
          { "id": "chassis-truck", "name": "Cargo Truck", "imgSrc": "/img/truck.png", "baseWeight": 3000, "cargoCapacity": 500, "maxWeaponSlots": 2, "maxArmorSlots": 6, "baseFuelEfficiency": 0.4, "description": "Heavy hauler with significant storage." },
          { "id": "chassis-war-rig", "name": "War Rig", "imgSrc": "/img/warrig.png", "baseWeight": 8000, "cargoCapacity": 1000, "maxWeaponSlots": 4, "maxArmorSlots": 8, "baseFuelEfficiency": 0.2, "description": "The ultimate desert fortress, slow but deadly." }
        ]
    *   Create `src/data/components.json`:
        [
          { "id": "comp-light-armor", "name": "Light Plating", "type": "Armor", "weight": 50, "cost": 20, "fuelImpact": -0.01, "offensiveBonus": 0, "defensiveBonus": 5, "description": "Basic protection." },
          { "id": "comp-reinforced-armor", "name": "Reinforced Steel", "type": "Armor", "weight": 150, "cost": 70, "fuelImpact": -0.03, "offensiveBonus": 0, "defensiveBonus": 15, "description": "Solid defense against most threats." },
          { "id": "comp-spiked-armor", "name": "Spiked Armor", "type": "Armor", "weight": 200, "cost": 100, "fuelImpact": -0.04, "offensiveBonus": 5, "defensiveBonus": 20, "description": "Deters boarders and rams." },
          { "id": "comp-machine-gun", "name": "Mounted Machine Gun", "type": "Weapon", "weight": 80, "cost": 120, "fuelImpact": -0.02, "offensiveBonus": 20, "defensiveBonus": 0, "description": "Standard offensive weapon." },
          { "id": "comp-flamethrower", "name": "Flamethrower", "type": "Weapon", "weight": 100, "cost": 180, "fuelImpact": -0.03, "offensiveBonus": 35, "defensiveBonus": 0, "description": "Ignites foes at close range." },
          { "id": "comp-rocket-launcher", "name": "Rocket Launcher", "type": "Weapon", "weight": 150, "cost": 300, "fuelImpact": -0.05, "offensiveBonus": 50, "defensiveBonus": 0, "description": "Devastating against heavy targets." },
          { "id": "comp-standard-engine", "name": "Standard Engine", "type": "Engine", "weight": 200, "cost": 100, "fuelImpact": 0, "offensiveBonus": 0, "defensiveBonus": 0, "description": "Reliable power source." },
          { "id": "comp-turbo-engine", "name": "Turbocharged Engine", "type": "Engine", "weight": 250, "cost": 250, "fuelImpact": -0.10, "offensiveBonus": 0, "defensiveBonus": 0, "description": "Boosts speed at a fuel cost." },
          { "id": "comp-fuel-tank", "name": "Extra Fuel Tank", "type": "Utility", "weight": 70, "cost": 80, "fuelImpact": 0.15, "offensiveBonus": 0, "defensiveBonus": 0, "description": "Increases fuel range." },
          { "id": "comp-water-purifier", "name": "Water Purifier", "type": "Utility", "weight": 40, "cost": 60, "fuelImpact": -0.01, "offensiveBonus": 0, "defensiveBonus": 0, "description": "Generates potable water." },
          { "id": "comp-cargo-hold", "name": "Expanded Cargo Hold", "type": "Utility", "weight": 100, "cost": 150, "fuelImpact": -0.02, "offensiveBonus": 0, "defensiveBonus": 0, "description": "Adds significant storage capacity." },
          { "id": "comp-all-terrain-tires", "name": "All-Terrain Tires", "type": "Wheels", "weight": 60, "cost": 50, "fuelImpact": 0, "offensiveBonus": 0, "defensiveBonus": 0, "description": "Improved traction." }
        ]
    *   Create a placeholder image folder `public/img/` and add simple images named `buggy.png`, `sedan.png`, `truck.png`, `warrig.png` for chassis previews.

3.  **App Component (`src/App.jsx`):**
    *   Set up a `useState` for `selectedChassis` (initially `null` or the first chassis from `chassis.json`).
    *   Set up a `useState` for `selectedComponents` (an array, initially empty `[]`).
    *   Implement functions to calculate `totalWeight`, `totalFuelEfficiency`, `totalOffensiveRating`, `totalDefensiveRating`, and `totalResourceCost` based on `selectedChassis` and `selectedComponents`. These should be memoized using `useMemo` for performance.
    *   Create `handleAddComponent(component)`: Adds a component to `selectedComponents` if slots are available based on `selectedChassis` (e.g., check `maxWeaponSlots` vs. current weapon count). Also handle `Engine` and `Wheels` as unique categories (only one of each allowed).
    *   Create `handleRemoveComponent(componentId)`: Removes a component from `selectedComponents`.
    *   Provide a dark, post-apocalyptic theme using Tailwind CSS (e.g., `bg-gray-900`, `text-yellow-400` for accents, `text-gray-200` for primary text).
    *   Use a responsive layout (e.g., `flex` or `grid`) to structure the main sections.

4.  **Components (`src/components/`):**
    *   **`Header.jsx`:**
        *   Displays the app name: "Rust & Rigs: Wasteland Builder".
        *   Use `text-4xl font-bold text-yellow-400 p-4 border-b border-yellow-600` for styling.
    *   **`ChassisSelector.jsx`:**
        *   Receives `chassisData` and `onSelectChassis` props.
        *   Displays each chassis as a card (e.g., `bg-gray-800 p-4 rounded-lg shadow-md hover:ring-2 ring-yellow-500 cursor-pointer`).
        *   Show chassis image, name, and a brief description/key stats.
        *   Clicking a card sets the `selectedChassis` in `App.jsx`.
        *   Visually highlight the currently selected chassis.
    *   **`ComponentCategorySelector.jsx`:**
        *   Receives `componentTypes` (e.g., `['Armor', 'Weapon', 'Engine', 'Utility', 'Wheels']`) and `onSelectCategory` props.
        *   Renders buttons or tabs for each category. Clicking a category button filters the list of components displayed in `ComponentList`.
        *   Use `bg-gray-700 text-gray-100 hover:bg-yellow-600 hover:text-gray-900` for button styling.
    *   **`ComponentList.jsx`:**
        *   Receives `components` (filtered by category), `onAddComponent` props, and `selectedChassis` for slot checks.
        *   Displays each component as a card. Show name, type, weight, cost, and description.
        *   Include an "Add" button for each component. Disable the button if the `selectedChassis` doesn't have available slots for that component type or if it's a unique component (Engine, Wheels) and one is already selected.
        *   Use `Lucide-react` icons (e.g., `Weight`, `Fuel`, `Sword`, `Shield`) next to relevant stats.
    *   **`VehiclePreview.jsx`:**
        *   Receives `selectedChassis` and `selectedComponents` props.
        *   Displays the `imgSrc` of the `selectedChassis` (if available).
        *   Below the image, list the `name` of each component in `selectedComponents` with its `type`.
        *   Use `flex flex-col items-center p-4 bg-gray-800 rounded-lg`.
    *   **`BuildSummary.jsx`:**
        *   Receives `calculatedStats` (total weight, fuel efficiency, etc.) and `selectedComponents` and `onRemoveComponent` props.
        *   Displays all the calculated stats clearly (e.g., `Total Weight: X kg`, `Fuel Efficiency: Y L/100km`, `Offensive Rating: Z`).
        *   List each currently selected component with a "Remove" button next to it. Clicking "Remove" calls `onRemoveComponent`.
        *   Style with `bg-gray-800 p-4 rounded-lg text-gray-200`.

5.  **Styling & Responsiveness:**
    *   All styling must be done using Tailwind CSS classes. Avoid inline styles unless absolutely necessary for dynamic values.
    *   Ensure the layout is responsive and looks good on both mobile and desktop screens. Use flexbox/grid for adaptability.
    *   Emphasize a rugged, industrial, post-apocalyptic aesthetic with dark backgrounds, rusty-looking borders, and bright, utilitarian accents.

6.  **Functionality Details:**
    *   **Unique Components:** Ensure only one 'Engine' and one 'Wheels' component can be selected at a time. If a new one is added, it replaces the existing one.
    *   **Slot Management:** For 'Armor' and 'Weapon' components, check `selectedChassis.maxArmorSlots` and `selectedChassis.maxWeaponSlots` before adding more. Display a message if slots are full.
    *   **Dynamic Stats:** All stats in `BuildSummary` must update instantly as components are added or removed.

7.  **Image Handling:** Place placeholder images for chassis in `public/img/`. Reference them in `chassis.json` with relative paths (`/img/buggy.png`).
```