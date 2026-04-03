<p align="center">
    <img src="/assets/dumbPhone Logo.svg" alt="dumbPhone Logo" width="400"/>
</p>

# iOS Dumb Phone App Launcher
A free way to make your iPhone "dumber" and less distracting using Scriptable widgets, iOS Shortcuts, automations, and built-in settings. It works as a free alternative to apps like Dumb Phone (dp), Blank Spaces Launcher, and Minimalist Launcher, while also going further: with Shortcuts and automations, the apps displayed in the widget launcher can change automatically based on specific events.

## Table of Contents
- [Scriptable Setup](#scriptable-setup)
    - [Setting Up Apps](#setting-up-apps)
    - [URL Guide](#url-guide)
    - [Setting Up the Widget](#setting-up-the-widget)
    - [Good to Go](#good-to-go)
- [Setup Explanation](#setup-explanation)
- [Incorporating Shortcuts and Automations](#incorporating-shortcuts-and-automations)
    - [Shortcut Setup](#shortcut-setup)
    - [Automations](#automations)
- [Other Settings and Tips to Dumb Your iPhone](#other-settings-and-tips-to-dumb-your-iphone)

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



if (fm.fileExists(path)) {
  try {
    let content = fm.readString(path)
    apps = JSON.parse(content)
  } catch (e) {
    console.log("Couldn't find WidgetApps.txt")
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
        - This list isn't completely comprehensive, so if the app you want to launch isn't on the list try the standard formula "{app name}://". Example: 'strava://'

2. #### Other URLs
    - If the app you want on the launcher isn't in that list and the standard formula for the url doesn't work, then you need to create a shortcut in the Shortcuts app that will launch the app

    #### Creating a Shortcut to Launch an App
    1. Open the Shortcuts app and create a new shortcut
    2. Search for 'Open App' — some apps may have their own specific open app shortcut, so use the one pictured below:
    
    <p align="center">
      <img src="/assets/openAppAction.jpg" alt="Open App Action" width="400"/>
    </p>

    3. In that action, choose the app that you want to open
    4. Rename this shortcut to something memorable like "Launch {App}"
    5. In the url field of the app in WidgetApps.txt, you can now use this url: "shortcuts://run-shortcut?name=" where after name you fill in the name of the shortcut you created. If there is a space in the name of the shortcut, you must replace it with "%20"

### Setting Up the Widget
1. Edit your home screen and add the large widget from Scriptable
2. Long press on the widget to edit it
3. In the settings select the script that you created and select "Run Script" in the "When Interacting" section

### Good to Go
The app launcher should now be able to work, in Scriptable run the script and see if the widget updates with the apps setup in the WidgetApps txt file. If it doesn't do it immediately, Scriptable widgets should refresh each time your phone is turned on, so simply turning it off and back on again might work.

## Setup Explanation
The script is setup to determine what apps to display from a txt file rather than being built directly into its code because it opens the door for even more functionality by incorporating iOS shortcuts and automations. For instance, you may want a different set of apps at home than you do while at work. You can use automations and shortcuts to edit the WidgetApps txt file and refresh the widget to get your different apps.

## Incorporating Shortcuts and Automations

### Shortcut Setup
The WidgetApps txt file can be edited using shortcuts, allowing for the apps within the widget to be changed automatically.
1. Create a new shortcut in the Shortcuts app
2. Add the text action and input a new JSON array for a set of apps for the widget
3. Add the Save File action
    - Disable the Ask Where to Save toggle, it should now say "Save text to Shortcuts"
    - Tap on Shortcuts and select the Scriptable folder previously created
    - Fill in WidgetApps.txt in the Subpath option
    - Turn on the Overwrite File if Exists toggle
4. Add the Run Script action from Scriptable
    - Select the script that you created earlier
    - Clear the parameter "Saved File"
5. Add the Refresh All Widgets action from Scriptable
6. (Optional) Add the Go to Home Screen action

#### Example:
<p align="center">
  <img
    src="/assets/Edit WidgetApps Shortcut Example.jpg"
    alt="Edit WidgetApps Shortcut Example"
    width="200"
  >
</p>

<p align="center">
  <a href="https://www.icloud.com/shortcuts/7d55c5abd8f1413ea2953fd811055a03">Get this exact shortcut</a>.
</p>


### Automations
You can use the shortcut to change the WidgetApps txt file in combination with automations to have the apps in the widget change when something specific happens. This example shows how to setup an automation for arriving somewhere of your choosing.

1. In the Shortcuts app go to the automations tab and create a new automation
2. Select Arrive
    - Enter the address of the location you want
    - (Optional) put in a specific time range
    - Select "Run Immediately"
3. Select the shortcut you created that corresponds with the apps that you want at this location

## Other Settings and Tips to Dumb Your iPhone
For this dumb app launcher to be effective, remove all apps from your home screen. They don't necessarily need to be deleted, but at least removed.

- Make use of the Today View screen (this is the screen to the left of the main home screen)
    - Important apps that may not have been able to make it to your launcher probably have widgets that can be fit into this screen. For instance some useful ones are Clock, Maps, Weather, Fitness, etc.
- Disable suggested apps in search
    - Settings → Apple Intelligence & Siri → Suggest Apps Before Searching (disable)
    - (Not related to dumbing of your phone) While here I would disable Apple Intelligence entirely and make sure in the apps section that the setting "Learn from this App" is disabled for each one
- Disable apps from appearing in search
    - Settings → Apps → {App you want disabled from search} → Search → Show App in Search (disable) and Show Content in Search (disable)
- Disable apps from being suggested in App Library
    - Settings → Apps → {App you want disabled from search} → Apple Intelligence & Siri → Suggest App (disable)
- Make new apps not download to home screen
    - Settings → Home Screen & App Library → App Library Only (select)
- Reduce color on screen (two ways)
    - Settings → Accessibility → Display & Text Size → Reduce White Point (enable) → Select intensity level
    - Settings → Accessibility → Display & Text Size → Color Filters → Color Filters (enable) → Grayscale (select)
    - A good way to use these is to combine them with automations. You can toggle these settings on and off in shortcuts with the Set White Point and Set Color Filters actions respectively.
        - The automation I use is that whenever any app is closed, I determine if I am home and if I am I turn on the color filters and white point. Alternatively, if there are any apps in which I would want normal color (like photos) I have another automation that turns these settings off when those apps are opened.
