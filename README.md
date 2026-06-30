# Where is Lola? | 8-Bit Adventure Game in SM83 Assembly

<p align="center">
  <img src="https://img.shields.io/badge/Language-SM83%20%2F%20Z80%20Assembly-red.svg?style=for-the-badge" alt="SM83 Assembly">
  <img src="https://img.shields.io/badge/Platform-Nintendo%20Game%20Boy-green.svg?style=for-the-badge&logo=nintendo" alt="Game Boy">
  <img src="https://img.shields.io/badge/Tools-RGBDS-blue.svg?style=for-the-badge" alt="RGBDS">
</p>

## 🕹️ Overview
Where is Lola? is an original 8-bit adventure game developed entirely in **SM83 Assembly from scratch** for the classic Game Boy hardware. Operating within intense hardware boundaries, the game features dynamic procedural entity generation, tile-based navigation grids similar to Pac-Man, and custom hardware register management.

---

## 📸 Gameplay & Media
<img width="45%" alt="Main Menu" src="https://github.com/user-attachments/assets/ca576d3e-1189-4090-a1fe-192b7c9b99d1" />
<img width="45%" alt="Gameplay" src="https://github.com/user-attachments/assets/701597d9-fca7-4dd8-a79d-9b520d4f0fee" />




---

## 🛠️ Technical Achievements & Low-Level Architecture

### 🎲 Procedural Entity & Cow Generation
Generating dynamic elements on a CPU with no built-in random math instructions required custom mathematical setups:
*   **Low-Overhead Procedural Logic:** Developed a lightweight pseudo-random number generator (PRNG) routine in assembly based on hardware register values (like tracking the `DIV` register or frame cycles) to determine dynamic spawn locations.
*   **Constraint Checking:** The assembly routine cross-references the generated coordinates instantly with the background tile matrix to ensure entities never spawn out of bounds or inside solid walls.

### 🗺️ Grid-Based Movement & Navigation (Pac-Man Style)
*   **Tile-Bound Grid Constraints:** Programmed a discrete tile-by-tile coordinate mapping structure. Movement inputs are locked to the hardware's 8x8 pixel tile grid, ensuring precise orthogonal navigation rules just like classic arcade maze games.
*   **Optimized Tile Collisions:** Implemented rapid indexing checks where the game looks up specific memory addresses in the background map array (`VRAM` background maps) to instantly block or allow movement vectors before a sprite starts moving.

### 🎨 Efficient VRAM & Sprite Sheet Architecture
*   **Hardware Mirroring Hacks:** Maximize limited video memory by mirroring tile assets via software OAM flags (`X/Y flip` bits). This allowed complex sprite variations while reducing the total file size footprint on the cartridge ROM.
*   **Strict Register Manipulation:** Manual configuration of the `LCDC` (LCD Control) and `STAT` registers to handle smooth rendering and uninterrupted main-loop operations during active game scenes.

---

## ⚡ The Engineering Challenge: VRAM Bandwidth & Timing Limitations
**The Problem:** On the Game Boy, Video RAM (VRAM) and Object Attribute Memory (OAM) cannot be accessed by the CPU while the PPU (Pixel Processing Unit) is actively drawing the screen. Writing to VRAM at the wrong microsecond triggers a hard crash or nasty visual glitches.

**The Solution:** 
*   **V-Blank Optimization:** Structured all critical animation swaps and position updates to occur strictly within the tight **Vertical Blanking (V-Blank)** period (a tiny window of ~1.1 milliseconds per frame).
*   **OAM DMA Transfers:** Implemented a dedicated high-speed DMA (Direct Memory Access) routine copied into Internal HRAM to blast sprite data into hardware registers efficiently without freezing the gameplay loop.
*   **Result:** A perfectly stable, flicker-free 60FPS retro title running flawlessly inside hardware limitations.

---

## 📂 Project Structure
*   **Language:** SM83 / Z80 Assembly
*   **Assembler/Linker:** RGBDS (Rednex Game Boy Development System)
*   **Target Hardware:** Game Boy DMG-01 (or accurate emulators)
## 🕹️ Play the Game
**The latest stable build is available on itch.io:** 👉 [**Where Is Lola? on itch.io**](https://pixelvault.itch.io/where-is-lola)
