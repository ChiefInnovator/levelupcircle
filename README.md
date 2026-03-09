# **Level-Up Circle Generator**

*Interactive tool for visualizing your interconnected goals!* 🎯

**🚀 Try it now:** [Live Demo](https://chiefinnovator.github.io/levelupcircle/levelup-circle-generator.html)

---

## ✨ Features
- Create a vibrant "Level-Up Circle" with **up to 36 labeled points** arranged radially.
- Experience **smart color generation** for distinct and visually appealing designs.
- Toggle between **dark and light themes** for a personalized look.
- Customize **glow effects** for titles, points, labels, and connections to enhance visibility.
- Enjoy **multi-line smart label wrapping** for improved readability.
- Adjust settings such as **point size**, **label offsets**, and **font sizes** to suit your style.
- Export high-quality PNG images in resolutions of **1080, 2048, and 3840 square** for various uses.
- Share your configurations effortlessly with **deep linking** and compressed URL state sharing.
- Present your circle in **fullscreen mode** for an immersive experience.
- Utilize **social sharing** features for easy sharing on platforms like Twitter and Facebook.
- Get personalized advice from **ChatGPT** for strategic insights on your focus areas.
- **Auto-save** your progress in `localStorage` with every change.
- Enjoy a responsive design with **mobile-friendly controls** for on-the-go use.

---

## 🚀 Getting Started

### Prerequisites
- A modern web browser (Chrome, Edge, Safari, Firefox).

### Installation
No installation is required. Simply download or clone the repository.

### Setup Steps
1. Open `levelup-circle-generator.html` in your browser.
2. Adjust the controls in the left panel to customize your circle.
3. Click **Export PNG** to download a high-resolution image of your design.
4. Use the share buttons to copy a link or share your circle on social media.

---

## 🏗️ Architecture

### Project Structure
```
.
├── PLAN.md
├── README.md
├── docs
│   └── ask-chatgpt-feature.md
├── Rich
│   ├── level-up-circle-3840x3840 no lines.jpg
│   ├── level-up-circle-3840x3840 no lines.png
│   ├── level-up-circle-3840x3840 w lines.jpg
│   └── level-up-circle-3840x3840 w lines.png
├── index.html
├── level_up_circle_blog.md
└── levelup-circle-generator.html
```

### Tech Stack
- HTML5
- CSS3
- JavaScript

### Key Design Decisions
- The application is designed to work entirely client-side with no server interaction, allowing for fast loading times and offline capabilities.
- Utilizes `localStorage` for saving user configurations, ensuring a seamless user experience across sessions.

### Local Development
You can run the application locally by simply opening `levelup-circle-generator.html` in a web browser. No server setup is necessary.

---

## 📤 Sharing & Deep Links
- **Copy Share Link**: Generates a URL that contains your configuration, allowing others to see your exact circle.
- **Fullscreen Mode**: Hide controls to focus solely on the circle, with share links preserving this mode.
- **Social Sharing**: Share your designs directly to Twitter and Facebook with pre-filled text for ease.

---

## 🎨 Core Controls
| Control                | Description                                          |
|-----------------------|------------------------------------------------------|
| Title                 | Text at the top (glow optional)                      |
| Theme                 | Choose between Dark or Light background               |
| Number of Points      | Set 1–36 nodes around the circle                      |
| Export Size           | Select output canvas dimension (square)              |
| Point Size            | Adjust the radius of each goal node                   |
| Label Spacing         | Control the distance of label boxes from point edges  |
| Label Font Size       | Modify size of point label text                       |
| Title Font Size       | Adjust size of the title text                         |
| Neon Glow Effects     | Toggle glow for title, points, labels, and connections|
| Show Connection Lines  | Toggle full interconnection mesh on/off              |
| Random Colors         | Regenerate colors with different spacing modes        |
| Load Example          | Reset to a seeded demo state                          |
| Save State            | Persist current configuration to `localStorage`       |
| Clear State           | Clear saved state and reload example                  |

---

## ✍️ Contributing
We welcome contributions! Open an issue or submit a pull request with focused changes (feature, refactor, or documentation updates).

---

## 📜 License
No license is granted. All rights reserved. The source code, images, and generated outputs may not be copied, modified, redistributed, published, or used without prior written permission.

**Copyright © 2025 Richard Crane. All Rights Reserved.**

---

### Related Links
- [Richard Crane's Website](https://inventingfirewith.ai)
- [Microsoft MVP Profile](https://mvp.microsoft.com/en-US/MVP/profile/10ce0bc0-7536-43f6-b28c-e9601a4a0d0d)
- [Inventing Fire with AI Podcast](https://inventingfirewith.ai)

---

<sub>Powered by [Inventing Fire with AI](https://inventingfirewith.ai)</sub>


---

<sub>Powered by [RepoBeacon](https://repobeacon.com)</sub>
