# N-Gauge Architect

N-Gauge Architect is the ultimate digital companion for N-scale model train enthusiasts. This intuitive web tool empowers hobbyists to design, plan, and manage their intricate layouts with unparalleled precision and ease. From beginners struggling with their first loop to seasoned veterans optimizing a multi-level empire, N-Gauge Architect provides a drag-and-drop interface with a comprehensive library of N-scale track pieces, allowing users to visualize their dream railroad before laying a single tie. Beyond design, it offers robust inventory management for locomotives, rolling stock, and structures, ensuring every piece of your collection is cataloged and easily accessible. Say goodbye to guesswork and wasted materials; N-Gauge Architect transforms layout planning into an enjoyable, efficient, and highly creative process, helping you build the perfect miniature world.

## Technical prompt

```md
Build a React.js single-page application named 'N-Gauge Architect' using Vite for bundling and Tailwind CSS for styling. The application will be a client-side tool with data persistence handled via IndexedDB (using `idb` library) to store user layouts and inventory.

**1. Project Setup:**
   - Initialize a new React project with Vite (`npm create vite@latest N-Gauge-Architect -- --template react`).
   - Install Tailwind CSS, PostCSS, and Autoprefixer and configure them (`npx tailwindcss init -p`).
   - Install necessary dependencies: `lucide-react` for icons, `react-router-dom` for navigation, `konva`, `react-konva` for the canvas-based layout designer, `dnd-kit/core`, `dnd-kit/modifiers`, `dnd-kit/utilities` for drag-and-drop, and `idb` for IndexedDB interactions.

**2. Core Application Structure:**
   - `src/App.jsx`: Main application component with React Router setup.
   - `src/index.css`: Import Tailwind base, components, and utilities.
   - `src/components/`: Directory for reusable UI components (Header, Sidebar, Modal, etc.).
   - `src/pages/`: Directory for main application views (DesignerPage, InventoryPage).
   - `src/lib/`: Directory for utility functions, data models, and IndexedDB service.
   - `src/assets/`: Placeholder for static assets like track piece SVG icons.

**3. Data Models (`src/lib/dataModels.js` or similar):**
   - `TrackPiece`: `{ id: string, name: string, type: 'straight' | 'curve' | 'turnout' | 'crossing' | 'flex', length_mm: number, radius_mm?: number, angle_deg?: number, endpoints: Array<{ x: number, y: number, direction_deg: number }>, svgPath: string }` (predefined static data).
   - `LayoutPieceInstance`: `{ id: string, trackPieceId: string, x: number, y: number, rotation_deg: number, connections: Array<{ endpointIndex: number, connectedToPieceId: string, connectedToEndpointIndex: number }> }` (instance on canvas).
   - `LayoutSchema`: `{ id: string, name: string, pieces: LayoutPieceInstance[] }`.
   - `InventoryItem`: `{ id: string, type: 'locomotive' | 'freight_car' | 'passenger_car' | 'structure' | 'accessory', manufacturer: string, model: string, roadNumber?: string, era?: string, dccReady?: boolean, condition: string, notes?: string, quantity: number, imageUrl?: string }`.

**4. IndexedDB Service (`src/lib/dbService.js`):**
   - Create a service using `idb` to manage two object stores: `layouts` and `inventory`.
   - Implement functions: `openDB()`, `addLayout(layout)`, `getLayout(id)`, `getAllLayouts()`, `updateLayout(layout)`, `deleteLayout(id)`. Similar CRUD operations for `inventory` items.

**5. Header Component (`src/components/Header.jsx`):**
   - Tailwind-styled navigation bar.
   - App title 'N-Gauge Architect' and logo (Lucide icon).
   - Navigation links using `react-router-dom/NavLink` for 'Designer' and 'Inventory'.

**6. Layout Designer Page (`src/pages/DesignerPage.jsx`):**
   - **Layout:** Three-column layout with Tailwind: left Sidebar (Track Piece Palette), middle Workspace (Konva Canvas), and optional right panel for selected piece properties.
   - **Track Piece Palette (Left Sidebar):**
     - Display a categorized list of predefined N-scale track pieces (`src/lib/staticTrackPieces.js`). Use SVG representations for each piece. Categories like 'Straights', 'Curves', 'Turnouts', 'Flex Track'.
     - Each piece in the palette should be a draggable element using `dnd-kit`'s `useDraggable` or a similar pattern that allows dropping onto the Konva canvas.
     - Include a search/filter functionality for track pieces.
   - **Workspace (Konva Canvas - Middle):**
     - Use `react-konva` for the main drawing area. `Stage`, `Layer`, `Rect` for grid, `Group` for track pieces.
     - **Grid System:** Implement a toggleable N-scale grid overlay (e.g., 25mm spacing, representing approx. 4ft real-world N-scale). Snap placed pieces to this grid.
     - **Drag & Drop:** Implement dropping from the palette onto the Konva stage. When a piece is dropped, create a `Konva.Group` component representing the `LayoutPieceInstance` and add it to the stage.
     - **Selection & Manipulation:**
       - Click on a `LayoutPieceInstance` to select it (add a highlight or border).
       - Drag selected piece to move it (`draggable` property on Konva `Group`).
       - Rotation controls: Add buttons to rotate the selected piece in 45-degree increments. Update `rotation_deg` of the `LayoutPieceInstance`.
       - Delete button for selected piece.
     - **Connection Snapping (Critical):**
       - For each `TrackPiece`, define precise endpoint coordinates and directions (e.g., 0, 90, 180, 270 degrees). These points will be used for snapping.
       - When a piece is dragged close to another piece's endpoint, if their endpoints align within a certain pixel tolerance and their directions are compatible (e.g., one points East, the other West), automatically snap them together.
       - Visually indicate snapped connections (e.g., a small circle at connected endpoints).
     - **Connectivity Checker:** A button that, when pressed, iterates through all `LayoutPieceInstance`s and highlights any track endpoints that are not connected to another piece. Log these unconnected points.
     - **Dimension Display:** Calculate and display the overall bounding box dimensions (width and height in N-scale millimeters/inches) of the entire layout.
     - **Save/Load Functionality:** Buttons to 'Save Layout' (persist current state to IndexedDB with a user-defined name) and 'Load Layout' (display a list of saved layouts from IndexedDB for selection).

**7. Inventory Manager Page (`src/pages/InventoryPage.jsx`):**
   - **Layout:** Search/filter controls at the top, 'Add Item' button, and a display area for inventory items (table or card view).
   - **Add/Edit Inventory Item Modal (`src/components/InventoryModal.jsx`):**
     - A reusable Tailwind/headless UI modal component.
     - Form fields for `InventoryItem` schema: Type (dropdown), Manufacturer, Model, Road Number, Era, DCC Ready (checkbox), Condition (dropdown), Notes (textarea), Quantity, and an Image Upload (store as Data URL/base64 in IndexedDB for simplicity or a simple text URL).
     - Implement validation for required fields.
   - **Inventory Display:**
     - A responsive table or grid of cards showing all inventory items retrieved from IndexedDB.
     - Each item should have 'Edit' and 'Delete' buttons.
   - **Search & Filter:** Implement client-side search by 'Manufacturer', 'Model', and 'Road Number'. Add filters for 'Type' and 'Era'.
   - **Data Persistence:** All inventory additions, edits, and deletions must be persisted to IndexedDB immediately.

**8. State Management:**
   - Use React's `useState` and `useContext` for local component state and global state (e.g., current layout state, inventory list, selected track piece) where appropriate.

**9. Styling:**
   - Apply Tailwind CSS classes exclusively for all UI elements.
   - Ensure a clean, modern, and responsive design across different screen sizes.

**10. Initial Data:**
    - Provide a `src/lib/staticTrackPieces.js` file with a reasonable selection of common N-scale track pieces (e.g., various lengths of straight track, curves of different radii/angles, a left and right turnout, a crossing). Each entry should include its `id`, `name`, `type`, `length_mm`, `radius_mm` (if applicable), `angle_deg` (if applicable), defined `endpoints` (e.g., `{ x: 0, y: 0, direction_deg: 0 }` and `{ x: 50, y: 0, direction_deg: 180 }` for a 50mm straight), and a simple `svgPath` for visual representation on the canvas.
```