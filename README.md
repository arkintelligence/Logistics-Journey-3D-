# ARK — 3D Logistics Journey

An interactive, scroll-driven 3D logistics experience that follows a shipping container through a complete multimodal journey—from a global port network and inland depot to road transport, ocean freight, and air cargo.

The website is built as a self-contained static prototype using HTML, JavaScript, React, and Three.js. No backend or database is required.

## Features

- Interactive 3D logistics environment
- Scroll-controlled cinematic storytelling
- Optional autoplay mode
- Animated global shipping network
- Inland container depot and gantry crane sequence
- Truck journey from depot to port
- Port gate and barrier animation
- Ship-to-shore crane loading sequence
- Ocean freight animation
- Air cargo flyover
- Live shipment status and progress HUD
- Responsive desktop and mobile layout
- Procedurally generated 3D vehicles and environments
- Support for optional production-ready GLB models
- Static hosting support for GitHub and Hostinger

## Journey Chapters

The experience is controlled by a single progress value between `0` and `1`.

| Progress | Chapter | Description |
|---|---|---|
| `0.00–0.16` | Network | Global logistics network and zoom into Singapore |
| `0.16–0.36` | Inland Depot | Gantry crane collects the container and places it on a truck |
| `0.36–0.50` | Road | Truck travels from the depot through Gate 02 to the berth |
| `0.50–0.65` | Port | Ship-to-shore crane loads the container onto the vessel |
| `0.65–0.80` | Sea | Container ship begins its ocean journey |
| `0.80–0.93` | Air | Air-freight aircraft flies over the network |
| `0.93–1.00` | Outro | Final services and call-to-action section |

## Technology

- HTML5
- CSS3
- JavaScript
- React
- Three.js
- WebGL
- TopoJSON
- Google Fonts
- Custom DC runtime and design-system bundle

The project loads some libraries and data from external CDNs, so an internet connection is required for the complete experience.

## Project Structure

```text
Logistics-Journey-3D-/
│
├── assets/
│   ├── ark-logo.png
│   └── favicon.svg
│
├── uploads/
│
├── index.html
├── package-lock.json
└── support.js

## How to Download and Run Locally

### 1. Download from GitHub

1. Open this GitHub repository.
2. Click the **Code** button.
3. Click **Download ZIP**.
4. Extract the downloaded ZIP file.

### 2. Open in VS Code

1. Open **VS Code**.
2. Go to **File → Open Folder**.
3. Select the extracted project folder.

### 3. Run with Live Server

1. Open the **Extensions** section in VS Code.
2. Search for **Live Server**.
3. Install the **Live Server** extension.
4. Open `index.html`.
5. Right-click on `index.html`.
6. Select **Open with Live Server**.
7. The website will open in your browser.
