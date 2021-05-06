---
title: "Architecture"
description: "YIO is providing several API Services. To find out more about the different APIs follow the documentation beneth."
menu:
  developers:
    parent: "home"
weight: 20
---

<div class="alert alert-primary" role="alert">
  This topic is currently being created and worked on!
</div>

# Technologies

- UI is written in [Qt QML](https://doc.qt.io/qt-5.12/qtqml-index.html).
- Device drivers and services are mostly in Qt C++ (v11 and updating to v14).
- Web configurator: JavaScript and transitioning to [Vue.js](https://vuejs.org/)
- Captive portal for WiFi configuration: PHP
  - [Node.js](https://nodejs.org/) will be investigated soon in the developer Buildroot image.
