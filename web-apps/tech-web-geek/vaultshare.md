# VaultShare

VaultShare is a secure, ephemeral file-sharing web tool designed for individuals and teams who need to send sensitive documents with confidence. Users can upload any file, encrypt it directly in their browser using a password they provide, and then generate a unique, time-limited shareable link. The recipient uses the same out-of-band password to decrypt and download the file. Key features include configurable link expiration, a one-time view option that invalidates the link after first use, and read receipts to confirm delivery. This eliminates the risks associated with email attachments and insecure cloud storage, providing a simple yet robust solution for confidential data exchange.

## Technical prompt

```md
You are tasked with building a web application called 'VaultShare'. The application should be a React frontend with Tailwind CSS for styling, connected to a simple Node.js/Express backend. Follow these steps:

1.  **Project Setup (Frontend):**
    *   Initialize a new React project using Vite (`npm create vite@latest my-vaultshare-frontend --template react`).
    *   Configure Tailwind CSS for the React project.
    *   Install necessary frontend libraries: `lucide-react` for icons, `react-router-dom` for routing, `react-hot-toast` for notifications, `axios` for API requests, `react-dropzone` for file upload (or implement a basic drag-and-drop).

2.  **Project Setup (Backend):**
    *   Create a new Node.js project (`npm init -y`) in a `backend` directory.
    *   Install necessary backend libraries: `express`, `cors`, `multer` (for handling file uploads), `uuid` (for generating unique IDs).

3.  **Frontend Components & Pages:**
    *   **`App.jsx`**: Set up `react-router-dom` to handle the following routes:
        *   `/`: Home page for file upload and encryption.
        *   `/share/:id`: Page to display the generated shareable link and options.
        *   `/decrypt/:id`: Page for recipients to decrypt and download files.
    *   **`HomePage.jsx` (Upload/Encrypt Interface):**
        *   A clean, centered layout using Tailwind CSS.
        *   A `react-dropzone` component (or custom drag-and-drop area) for users to upload files.
        *   A standard file input fallback.
        *   An input field for the user to set an encryption password.
        *   A button: 'Encrypt & Share'.
        *   Display encryption progress (e.g., a simple text indicator).
        *   Use `lucide-react` for relevant icons (e.g., Upload, Lock).
    *   **`SharePage.jsx` (Link Display & Options):**
        *   Displays the generated unique shareable URL (e.g., `http://localhost:3000/decrypt/:id`).
        *   A button to copy the link to the clipboard.
        *   Dropdowns or radio buttons for configuring link behavior:
            *   Expiration: '1 Hour', '1 Day', '7 Days', 'Never'.
            *   One-Time View: A checkbox that, if checked, invalidates the link after the first successful decryption/download.
        *   A simple visual indicator for 'read receipts' (initially 'Not Viewed', updates after decryption).
        *   Use `lucide-react` for icons (e.g., Link, Clock, Eye).
    *   **`DecryptPage.jsx` (Recipient Interface):**
        *   An input field for the recipient to enter the decryption password.
        *   A button: 'Decrypt & Download'.
        *   Display decryption progress/status.
        *   Upon successful decryption, provide a button to download the original file.
        *   Handle cases where the link is expired or already used (for one-time view).
        *   Use `lucide-react` for icons (e.g., Unlock, Download).
    *   **`ToastContainer`**: Include `react-hot-toast`'s `ToastContainer` for global notifications.

4.  **Frontend Logic (Client-Side Encryption/Decryption):**
    *   **Encryption Process:**
        *   When a file is uploaded and password provided:
            *   Read the file content as an `ArrayBuffer`.
            *   Use `window.crypto.subtle` (Web Crypto API) for encryption.
            *   Generate a random `salt` (e.g., 16 bytes).
            *   Derive an encryption key from the user's password and `salt` using PBKDF2 (`window.crypto.subtle.importKey`, `window.crypto.subtle.deriveKey` with AES-GCM).
            *   Generate a random `IV` (Initialization Vector, 12 bytes for AES-GCM).
            *   Encrypt the file content using the derived key and IV (`window.crypto.subtle.encrypt`).
            *   The encrypted data, `IV`, `salt`, and original `filename` should be sent to the backend.
    *   **Decryption Process:**
        *   When a recipient enters a password on `/decrypt/:id`:
            *   Retrieve the encrypted data, `IV`, `salt`, and `filename` from the backend.
            *   Derive the decryption key from the provided password and the received `salt` (PBKDF2, same as encryption).
            *   Decrypt the data using the derived key and `IV` (`window.crypto.subtle.decrypt`).
            *   Create a `Blob` from the decrypted `ArrayBuffer` and trigger a download (`URL.createObjectURL`, `<a>` tag `download` attribute).
    *   **Important Security Note**: The raw encryption key must *never* be sent to or stored on the backend. All encryption and decryption must happen exclusively client-side.

5.  **Backend Logic (Node.js/Express):**
    *   **CORS**: Enable `cors` for the frontend to communicate with the backend.
    *   **File Storage**: For this demonstration, store the *encrypted* file data temporarily on the server's disk (e.g., in a `temp_uploads` folder created by `multer`) and link metadata in a simple JSON file (`links.json`) or an in-memory `Map` (e.g., `const links = new Map();`) for simplicity.
    *   **Routes:**
        *   `POST /api/upload`:
            *   Receives the encrypted file blob (via `multer`) and metadata (original filename, IV, salt, expiration details, one-time view flag).
            *   Generates a unique `linkId` using `uuid`.
            *   Saves the encrypted file blob to the temporary `temp_uploads` directory.
            *   Stores link metadata (link ID, filename, path to encrypted file, IV, salt, expiration timestamp, one-time view status, read status) in `links.json` or the `Map`.
            *   Returns the `linkId` to the frontend.
        *   `GET /api/link/:id`:
            *   Retrieves metadata associated with `:id`.
            *   Checks for expiration and one-time view status. If expired or already viewed (and one-time), return an error.
            *   If valid, read the encrypted file data from the temporary storage.
            *   Return the encrypted file data, IV, salt, and filename to the frontend.
            *   **Read Receipt/One-Time View Logic**: If the link is set for one-time view, update its status immediately after this fetch to prevent subsequent access. Also, update the read receipt status.
        *   `POST /api/mark-read/:id` (Optional, if `GET` logic becomes too complex for AI to implement all at once): An endpoint to specifically mark a link as 'read' or 'used'.

6.  **Styling & UI/UX:**
    *   Apply Tailwind CSS extensively to all components for a modern and responsive design.
    *   Provide clear loading states and error messages using `react-hot-toast`.
    *   Ensure the drag-and-drop area is visually distinct.
    *   Implement basic form validation (e.g., password required).

7.  **Error Handling:**
    *   Implement robust error handling for API calls and encryption/decryption processes, displaying user-friendly messages using `react-hot-toast`.
    *   Handle cases where a link ID is not found, or the link is expired/used.
```