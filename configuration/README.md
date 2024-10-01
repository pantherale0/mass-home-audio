# User Configuration

This system allows you to configure user-specific access and control over the home audio setup, including music playback, notifications, and the ability to transfer media from personal devices to shared speakers. Below is a breakdown of the JSON configuration and how each key functions.

## Configuration Structure

The JSON file contains the following sections:

### 1. `config`
This section defines the **user-specific settings** for each Home Assistant user. The user's ID is used as the key, and the configuration details are stored as nested properties.

#### Example:
```json
"config": {
    "HASS USER ID": {
        "name": "NAMEOFUSER",
        "music_access": true,
        "music_notification": true,
        "mobile_transfer_music_access": true,
        "mobile_transfer_music_entity": "HA MOBILE MEDIA SESSION ENTITY ID HERE",
        "music_notification_service": "HA MOBILE NOTIFICATION SERVICE HERE",
        "music_notification_level": 2,
        "music_entities": {
            "switch": "INPUT BOOL TO STORE SESSION ON/OFF STATE",
            "current_player": "INPUT TEXT TO STORE CURRENT MEDIA PLAYER ENTITY ID",
            "playlist": "INPUT TEXT STORING NAME OF PLAYLIST TO PLAY"
        }
    }
}
```

#### Key Details:

- **`name`**: Name of the user. Helps in identifying each user's configuration.
  
- **`music_access`**: Defines if the user is allowed to access the music system (true/false).
  
- **`music_notification`**: Defines if the user will receive a notification on their mobile device, which allows them to control playback (true/false).
  
- **`mobile_transfer_music_access`**: Defines if the user is permitted to transfer music from their mobile device to the system's speakers (true/false).
  
- **`mobile_transfer_music_entity`**: The **entity ID** of the media session from Home Assistant for the user's mobile device. This is where the media session is controlled when transferring music.

- **`music_notification_service`**: The **notification service** for the user’s mobile device in Home Assistant. Example: `notify.samsung_s23u`.

- **`music_notification_level`**: Defines the behavior when clicking the notification:
  - `2`: Opens the Music Assistant frontend from the notification.
  - `1`: Does nothing when the notification is clicked.

- **`music_entities`**: A collection of Home Assistant entities used to manage the session:
  - **`switch`**: An **input boolean** used to store the session's on/off state.
  - **`current_player`**: A **text entity** used to store the current media player’s entity ID.
  - **`playlist`**: A **text entity** storing the name of the playlist to play.

### 2. `mass_tags`
This section links **NFC tags** with **media players**. When a tag is scanned, the corresponding media player will be triggered to play.

#### Key Details:

- **`TAG ID FROM HOME ASSISTANT`**: The unique identifier of an NFC tag, as recognized by Home Assistant.
- **`MEDIA PLAYER ENTITY ID`**: The entity ID of the media player that should be activated when the NFC tag is scanned.
- **`HASS USER ID`**: The user ID for the user taken from within the Home Assistant users setting menu.

## How to Use

1. **Configure User Access**: In the `config` section, define the permissions and controls for each user by providing their Home Assistant user ID and the relevant details such as media session entity and notification preferences.
   
2. **Link NFC Tags**: Use the `mass_tags` section to map NFC tags to specific media players. When a user scans an NFC tag, the associated media player will start playing the defined playlist.

3. **Control Via Mobile Notifications**: If `music_notification` is set to `true`, users will receive a mobile notification that allows them to control the playback. If `music_notification_level` is set to `2`, clicking the notification will open the Music Assistant frontend.

# Home Assistant: Setting up a sensor

You will need to setup a sensor within Home Assistant to read this file. There are multiple tutorials online on how to set this up, I have linked a couple below:

[**Using jq (command line)**](https://www.petekeen.net/static-json-in-home-assistant/)

[**Using REST sensor**](https://community.home-assistant.io/t/how-do-i-get-a-local-json-file-to-load-using-rest-or-file/635033)

Either way, for `json_attributes` you will need to configure as provided in the following example.

## Example configuration.yaml entry

```
sensor:
  - platform: rest
    resource: http://iamultrasecure.com/config.json
    scan_interval: 60
    name: NFC Scanned Tag Configuration
    value_template: "{{ value_json.config | length }}"
    json_attributes:
    - "config"
    - "mass_tags"
    - "mass_current_players"
```
