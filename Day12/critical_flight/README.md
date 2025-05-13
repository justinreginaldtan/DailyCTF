# 🧠 Critical Flight
> *Hack The Box* 
> *Date Solved: 2025-05-12*  
> *Difficulty: Easy*
> *Category: Hardware*
---

## 🧩 Overview
We need to solve HTB’s “Critical Flight” hardware challenge.
The challenge gives us a folder of Gerber (.gbr) files.
Gerber files describe PCB layers (copper, soldermask, silkscreen, drill, outline, etc.).
Our goal is to find the hidden flag printed on different layers.
---

## 🛠️ Tools & Techniques
- Zip
- Gerber Viewer

---

## 🔍 Approach

    1. Load the files
        Zip up the entire challenge folder.
        Upload to an online Gerber viewer (I used PCBWay’s Gerber Viewer).

    2. Identify each layer

        Copper traces connect components.
        Soldermask protects copper and prevents shorts (“the green layer”).
        Silkscreen labels parts (“ink layer”).
        Solderpaste shows where components mount.
        Drill marks holes for vias and mounting.
        Outline defines the board shape.

    3. Inspect top and bottom
        Top layer: nothing useful.
        Bottom silkscreen: found the first half of the flag text. Hold to a mirror to read it.

    4. Reveal hidden copper text
        Disable soldermask, silkscreen, solderpaste layers in the viewer.
        Leave only copper layers (*_F.gbr and _B.gbr).
        Zoom in on copper traces.
        Spot etched text for the second half of the flag.

    5. Combine both halves
        First half from silkscreen.
        Second half from copper.

The full flag was given afterwards.

---

## 🧠 Takeaways
    Hardware category CTFs are interesting because I have to learn about the layers and components of PCBs.
    - Gerber files map PCB structure layer by layer.
    - Online viewers speed up layer analysis.
    - Silkscreen is good for visible text.
    - Copper layers can hide secrets under soldermask.
    - Always toggle off non-copper layers to reveal etched details.
