# iOS Dumb Phone App Launcher
Combining Scriptable, iOS settings and shortcuts to create a free way to dumb down your iPhone with more features than standard apps.

## Table of Contents
- [Scriptable Setup](#scriptable-setup)
    - [Setting Up Apps](#setting-up-apps)
    - [URL Guide](#url-guide)
    - [Setting Up the Widget](#setting-up-the-widget)
    - [Good to Go](#good-to-go)
- [General Setup Information](#general-setup-information)
- [Incorporating Shortcuts and Automations](#incorporating-shortcuts-and-automations)

## Scriptable Setup
1. Download the app [Scriptable](https://apps.apple.com/us/app/scriptable/id1405459188)
2. In the iOS files app, navigate to the On My iPhone section and create a new folder named "Scriptable"
3. In that folder create a new file named WidgetApps.txt
    - To edit txt files on iOS download the app [Paper](https://apps.apple.com/us/app/paper-writing-app-notes/id1476984841)
4. In Scriptable:
    - Open Settings
    - Go to 'File Bookmarks'
    - Add a new file bookmark folder that points to the Scriptable folder made in step 2
    - Create a new script and enter this code:


```
let fm = FileManager.local()
let folderPath = fm.bookmarkedPath("Scriptable")
let path = fm.joinPath(folderPath, "WidgetApps.txt")

let apps = [
  { name: "Something went wrong", url: "" }
]


if (fm.fileExists(path)) {
  try {
    let content = fm.readString(path)
    apps = JSON.parse(content)
  } catch (e) {
    // silently use fallback if JSON is invalid
  }
}

let widget = new ListWidget()
widget.setPadding(12, 12, 12, 12)


for (let app of apps) {
  let row = widget.addStack()
  row.url = app.url
  row.layoutHorizontally()
  row.centerAlignContent()
  row.setPadding(0, 16, 16, 16)

  let text = row.addText(app.name)
  text.font = Font.boldSystemFont(42)
  text.leftAlignText()

  row.addSpacer()
  widget.addSpacer(1)
}

Script.setWidget(widget)
Script.complete()
```

### Setting Up Apps
1. Open the WdgetApps txt file using Paper to edit it
2. Create a JSON array of dictionaries where each dictionary has the keys "name" and "url".
    - The value of name will be the text to represent the app on the widget and the value of url will be the link that directs you to that app.
#### Example of JSON:

```
[
  { "name": "YouTube", "url": "shortcuts://run-shortcut?name=Launch%20Youtube" },
  { "name": "Spotify", "url": "spotify://" },
  { "name": "Reddit", "url": "shortcuts://run-shortcut?name=Launch%20Reddit" },
  { "name": "Photos", "url": "photos-redirect://" },
  { "name": "Maps", "url": "maps://" }
]
```

- You can have as many apps in the array as you would like, but there is a limitation by the size of the Scriptable widget. If you put in more, you will have to adjust the padding and the text size in the given code. For the given code, 5 apps is the advisable amount.

### URL Guide

1. #### Specialty URLs
    - Apple has some app specific short urls that make things easier, a list of these can be found [here](https://github.com/bhagyas/app-urls)

2. #### Other URLs
    - If the app you want on the launcher isn't in that list, then you need to create a shortcut in the Shortcuts app that will launch the app

    #### Creating a Shortcut to Launch an App
    1. Open the Shortcuts app and create a new shortcut
    2. Search for 'Open App' — some apps may have their own specific open app shortcut, so use the one pictured below:

    3. In that action, choose the app that you want to open
    4. Rename this shortcut to something memorable like "Launch {App}"
    5. In the url field of the app in WidgetApps.txt, you can now use this url: "shortcuts://run-shortcut?name=" where after name you fill in the name of the shortcut you created. If there is a space in the name of the shortcut, you must replace it with "%20"

### Setting Up the Widget
1. Edit your home screen and add the large widget from Scriptable
2. Long press on the widget to edit it
3. In the settings select the script that you created and select "Run Script" in the "When Interacting" section

### Good to Go
The app launcher should now be able to work, in Scriptable run the script and see if the widget updates with the apps setup in the WidgetApps text file. If it doesn't do it immediately, Scriptable widgets should refresh each time your phone is turned on, so simply turning it off and back on again might work.

## General Setup Information
The script is setup to determine what apps to display from a text file rather than being built directly into its code because it opens the door for even more functionality by incorporating iOS shortcuts and automations. For instance, you may want a different set of apps at home than you do while at work. You can use automations and shortcuts to edit the WidgetApps txt file and refresh the widget to get your different apps.

## Incorporating Shortcuts and Automations


