# iOS Dumb Phone App Launcher
Combining Scriptable, iOS settings and shortcuts to create a free way to dumb down your iPhone with more features than standard apps.

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

## Setting Up Apps
1. Open the WdgetApps txt file using Paper to edit it
2. Create a JSON array of dictionaries where each dictionary has the keys "name" and "url".
    - The value of name will be the text to represent the app on the widget and the value of url will be the link that directs you to that app.
### Example of JSON:

```
[
  { "name": "YouTube", "url": "shortcuts://run-shortcut?name=Launch%20Youtube" },
  { "name": "Spotify", "url": "spotify://" },
  { "name": "Reddit", "url": "shortcuts://run-shortcut?name=Launch%20Reddit" },
  { "name": "Photos", "url": "photos-redirect://" },
  { "name": "Maps", "url": "maps://" }
]
```
