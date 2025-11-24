# DataBloom

DataBloom is an intelligent file management and analysis web tool designed to help users understand, optimize, and declutter their digital storage. It goes beyond simple file explorers by scanning local or cloud directories to provide deep insights into file metadata, identify redundant data, and suggest actionable recommendations for archiving or deletion based on user-defined policies or AI-driven usage patterns. With interactive visualizations, DataBloom empowers individuals and teams to reclaim valuable disk space, improve data accessibility, and ensure data hygiene, transforming chaotic file systems into well-organized and efficient digital workspaces.

## Technical prompt

```md
Develop a modern, single-page React application named 'DataBloom' using Vite for fast development, styled with Tailwind CSS, and leveraging specific React libraries for enhanced functionality and data visualization. The application will simulate file analysis for an initial MVP, with clear placeholders for future backend integration.

Project Setup:
1. Initialize a new React project using Vite: `npm create vite@latest databloom --template react`.
2. Install Tailwind CSS: Follow the official Tailwind CSS installation guide for React (PostCSS setup).
3. Install required libraries:
    *   `lucide-react` for icons.
    *   `recharts` for data visualization.
    *   `react-dropzone` for file upload.
    *   `uuid` for generating unique IDs (if needed for mock data).

Core Features & Components:

1. Main Layout (`src/components/Layout.jsx`):
    *   A responsive layout featuring a header with the app name 'DataBloom' and a simple navigation (e.g., 'Dashboard', 'Settings' - initially just 'Dashboard').
    *   A main content area.
    *   A minimalistic footer.
    *   Use Tailwind CSS for all styling (e.g., `flex`, `grid`, `bg-gray-900`, `text-white`, `p-4`).

2. File Upload/Analysis Initiation (`src/components/FileUploadSection.jsx`):
    *   Implement a prominent drag-and-drop area using `react-dropzone`.
    *   Allow users to drop a single `.zip` file.
    *   Display a clear message like "Drag 'n' drop a .zip file here, or click to select files" along with a `lucide-react` icon (e.g., `Upload` or `FolderOpen`).
    *   Upon file drop, trigger a simulated 'analysis' process. For this MVP, this simulation will:
        *   Extract metadata from the uploaded ZIP (or use dummy data representing the ZIP's contents if actual ZIP processing is out of scope for the AI assistant).
        *   Display a loading state.
        *   Transition to the `Dashboard` component with the processed (or dummy) data.
    *   Provide visual feedback for drag-over and file selection.

3. DataBloom Dashboard (`src/pages/Dashboard.jsx`):
    *   This will be the central hub for displaying file analysis results.
    *   It should conditionally render based on whether analysis data is available. If not, display the `FileUploadSection`.
    *   Once data is available, display a grid-based layout (Tailwind CSS `grid`) containing the following sections:
        *   **Overview/Summary Statistics:** Total files, total size, number of duplicates.
        *   **Disk Usage Breakdown (Treemap):** A `recharts` `Treemap` component visualizing directory sizes. Each rectangle should represent a folder, with its size determining its area. Hovering should show folder name and size.
        *   **File Type Distribution (Bar Chart):** A `recharts` `BarChart` showing the count or size contribution of different file types (e.g., `.pdf`, `.jpg`, `.docx`, `.txt`).
        *   **Files by Age (Line Chart):** A `recharts` `LineChart` illustrating the distribution of files by their modification date (e.g., files created/modified in the last 7 days, 30 days, 90 days, older).
        *   **Duplicate & Suggested Files List:** A table (simple HTML table with Tailwind styling) listing identified duplicates or suggested files for review. Each row should include:
            *   File Name.
            *   Size.
            *   Type.
            *   Last Modified Date.
            *   Actions: A button for 'Delete' (simulated, perhaps just removes from list) and 'Ignore' (simulated). Use `lucide-react` icons for action buttons (e.g., `Trash2`, `X`).

4. Data Generation (Mock Data Service):
    *   Create a utility file (`src/utils/mockDataService.js`) that generates realistic-looking mock data representing file analysis results.
    *   This service should generate:
        *   A flat list of 'files' with properties: `id`, `name`, `path`, `size` (in bytes), `type` (extension), `lastModified` (timestamp), `isDuplicate` (boolean), `duplicateGroupId` (string for linking duplicates).
        *   Aggregated data suitable for the `recharts` components (e.g., nested structure for Treemap, arrays of objects for BarChart/LineChart).
    *   The `FileUploadSection` will call this service after a delay to simulate processing.

5. Styling & Responsiveness:
    *   Utilize Tailwind CSS extensively for all layout, typography, colors, spacing, and responsive breakpoints.
    *   Ensure the application looks good and is functional on desktop and mobile viewports.
    *   Use meaningful Tailwind classes (e.g., `flex`, `grid`, `gap-4`, `p-6`, `rounded-lg`, `shadow-md`, `bg-gray-800`, `text-gray-200`).
    *   Interactive elements (buttons, hover states) should have clear visual feedback.

Implementation Details & Considerations:
*   **State Management:** Use React's `useState` and `useContext` (if needed for global data like analysis results) for managing application state.
*   **Error Handling:** Basic error handling for file upload (e.g., only accept `.zip` files).
*   **Loading Indicators:** Display a `LoadingSpinner` component during the simulated analysis process.
*   **Code Structure:** Organize components logically within the `src/components` and `src/pages` directories. Use `src/utils` for helper functions and mock data.

Backend Placeholder (for future development):
*   While the initial implementation will simulate file analysis, clearly indicate where actual API calls would be made (e.g., `// TODO: Replace with actual backend API call for file analysis`). A real backend would handle ZIP file unzipping, actual file system scanning, content hashing, and sophisticated duplicate/redundancy detection algorithms.
```