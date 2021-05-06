---
title: "YIO Remote"
description: "Beautifully crafted, do-it-yourself open source touchscreen remote made to work with home automation systems. Running on a Raspberry Pi Zero W."
type: "docs"
layout: "single"
menu:
  docs:
    parent: "home"
weight: 1
---

<img class="img-fluid" src="https://ksr-ugc.imgix.net/assets/027/324/439/48d70a00287c06ba316639e1cb50ed23_original.jpg?ixlib=rb-2.1.0&crop=faces&w=1552&h=873&fit=crop&v=1574614151&auto=format&frame=1&q=92&s=05569929cda0488e7951aca2af8be208"/>

## What is the YIO Remote

YIO Remote is an open-source, Raspberry Pi Zero W based, high quality DIY touchscreen remote.

### User interface.

The user interface is designed to be adoptable by anyone that can use a smartphone app. Our goal is to provide UI choices that look clean and simple while offering as many features as posible.

{{< img src="ui-example.png" alt="YIO User Interface" caption="<em>YIO User Interface</em>" class="border-0" >}}

1. Notification indicator
2. Current time
3. Battery status indicator
4. Active page name
5. Group name
6. Group power switch
7. Entity
8. Entity power switch
9. Active page selection
10. Page selection.

### Pages

The interface selection is based on pages that can be swiped left and right. A page can either be the favourite, settings page or custom pages. Pages have an image header providing more visual information to a page. A page could be used to segment rooms, areas, a specific purpose or anything else that makes sense to you.

{{< img src="ui-pages.png" alt="YIO Pages" caption="<em>YIO Pages</em>" class="border-0" >}}

### Groups

A custom page consists of one or more groups. Groups are a set of entities that can be defined by the user. Groups are another way of organizing entities. A group can be configured to have a group switch, this switch can turn on/off every device within the group at once.

{{< img src="ui-groups.png" alt="YIO Groups" caption="<em>YIO Groups</em>" class="border-0" >}}

### Favorites page

The favorites page is a reserved page that do not show groups but entities directly. The favorites page is a optional page that like all pages can be sorted at any place.

{{< img src="ui-pages.png" alt="YIO Favorites" caption="<em>YIO Favorites</em>" class="border-0" >}}

### Settings page

The settings page is a reserved page that offers all user settings. The settings page is a optional page that like all pages can be sorted at any place.

{{< img src="ui-settings.png" alt="YIO Settings" caption="<em>YIO Settings</em>" class="border-0" >}}

### Profiles

Profiles can be selected by holding the status bar for 1 second. A profile consist of a group of pages.
Every user can have their own UI preferences. Configurate profiles through the web configurator.

{{< img src="ui-profiles.png" alt="YIO Profiles" caption="<em>YIO Profiles</em>" class="border-0" >}}

#### Customization

The YIO interface can be configured by using the web configurator. Here the user can add Profiles, Pages, Groups and manage their integrations, settings and entities.
