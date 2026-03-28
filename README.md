# iOS Dumb Phone App Launcher
Combining Scriptable, iOS settings and shortcuts to create a free way to dumb down your iPhone with more features than standard apps.

## Setup
- Download the app Scriptable
- Create a new script and enter this code:

```
let fm = FileManager.local()
let folderPath = fm.bookmarkedPath("Scriptable")
let path = fm.joinPath(folderPath, "WidgetApps.txt")

let apps = [
  { name: "Fallback", url: "shortcuts://run-shortcut?name=Launch%20YouTube" }
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
