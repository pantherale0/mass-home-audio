# Whole Home Audio System Overview

Please forgive me, I'm not great at documentation so have used ChatGPT to assist with a technical write up. Everything you read here has been reviewed.

IMPORTANT: ChatGPT was not used in the creation of any of the technical solution whatsoever.

## Overview

This project integrates a robust whole home audio solution using **Home Assistant** with **Music Assistant** as the core controller. The system is designed for seamless music playback, media session transfers, and automated management of media players throughout the home. Below is a breakdown of the key features and configurations:

## Key Features

### 1. NFC Music Playback Start
- **NFC tags** are placed around the home to instantly start music playback by scanning with an Android device.
- The system detects the NFC tag and initiates music playback on the selected media player.
- Each NFC tag can be configured to **start a favorite playlist**, allowing you to customize music based on your preferences.
- When a favorite playlist is triggered, **Music Assistant generates a unique blend of music** based on the content of that playlist (this feature is available only for supported media sources).
  - The media provider within Music Assistant uses the songs in your favorite playlist to create a dynamic, curated listening experience tailored to your taste, ensuring fresh music recommendations every time. If you ever get bored of a playlist, just scan the same tag again and a new playlist is generated!

### 2. NFC Transfer of Media Session from Android to Music Assistant
- Media sessions playing on an Android device can be **transferred to Music Assistant** through NFC tags.
- When a session is transferred, the Android device automatically pauses its media session, and the playback resumes on the selected media player in Music Assistant.
- Providing the Android device provides an up to date status from the media session, the playback will automatically seek to the current position.

### 3. NFC Transfer of Listening Session Between Media Players
- NFC tags enable seamless **transfer of active listening sessions** between different media players controlled by Music Assistant.
- Simply scan an NFC tag at another location to transfer playback to a different media player, without interruption.

### 4. Automatic Shutdown of Idle Media Players
- The system is configured to **automatically turn off any media player** that has been idle for more than **20 minutes**.
- This helps conserve energy and ensures that no media player is left on unnecessarily.

### 5. Configuration via External JSON File
- **No hardcoded entities** are used in the automations. Instead, the entire system is configured through an **external JSON file**, making it flexible and easily adaptable to changes in the environment or setup.
- Media players, tags, users and playlists are all defined within the JSON file.

### 6. Temporary State Management Using Saver Integration
- The system leverages the **Saver integration** to store **temporary states** instead of using Home Assistant helpers.

## Technical Details
- **Home Assistant:** The central hub for home automation, managing all interactions between NFC tags, media players, and Music Assistant.
- **Music Assistant:** The controller for audio playback and media session management.
- **Saver Integration:** Used to store and recall temporary states such as active media sessions or last-used media players.
- **NFC Tags:** Trigger various actions such as starting playback, transferring sessions, and switching between media players.

# Getting started

## Dependencies

To set up and run this whole home audio system, you will need the following components:

- **Music Assistant**
  - Integration version: **2024.9.0+**
  - Music Assistant Server version: **2.2.0+**
  - This is the core controller for audio playback, playlist management, and media session handling.

- **Home Assistant**
  - The central automation hub for managing interactions between NFC tags, media players, and Music Assistant.

- **Saver Integration**
  - Latest version of the [**Saver integration**](https://github.com/PiotrMachowski/Home-Assistant-custom-components-Saver).
  - This integration is used to temporarily store and manage states without needing Home Assistant helpers.
  - This should be installed and ready to go.


- **Snapcast or other Music Assistant compatible players**
  - Make sure you have Snapcast or any other **compatible media players** already configured and integrated into Music Assistant.

- **NFC Tags**
  - Cheap NFC tags can be sourced from places like **AliExpress** to set up music playback triggers throughout the home.

- **External Configuration Storage**
  - You’ll need somewhere to store your **external JSON configuration file**. This can be stored:
    - Locally on your **Home Assistant host**.
    - Or externally, such as within **n8n** or another workflow automation platform that supports JSON storage.

- **Spare Time and a Cup of Coffee**

# TODO: Build

This has been split up into multiple stages, first navigate to the _configuration_ folder to setup the external configuration file.
