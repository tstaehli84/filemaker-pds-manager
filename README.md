# PDS Manager for FileMaker

A FileMaker add-on to view, copy, add, update, and delete Persistent Data Store entries through a simple web-based interface.

Built by **Thomas Staehli** · [Contact](mailto:thomas.staehli@mac.com)

## Features

- Select a PDS Name and one of its Instance IDs.
- View the current stored text or code in a read-only, selectable text area.
- Copy content to the clipboard.
- Upload a new version of an existing entry.
- Add a new entry by entering an Instance ID and choosing a source file.
- Delete entries with a dedicated button.
- Protect the manager's own HTML from deletion through the manager.
- Resize the content area with the Web Viewer.
- Read the built-in instructions for adding another PDS Name.

The manager's own code is stored at **`addons` / `PDS-Manager.HTML`**. No data table is required by the add-on.

## Requirements

- **FileMaker Pro 26.0.3 or newer**, on macOS or Windows.
- A developer account with Full Access for installation and PDS management.

The interface is designed for desktop use. It displays stored text/code; it does not preview or decode binary/container files. Uploading currently supports HTML, JavaScript, and CSS source files.

## Downloads

Choose **one** installation method:

| File | Platform | Purpose |
| --- | --- | --- |
| [PDS_Manager.fmaddon](./PDS_Manager.fmaddon) | FileMaker add-on package | Native distribution package; manual installation is available if opening it fails. |
| [PDS_Manager-Windows-Installer.zip](./PDS_Manager-Windows-Installer.zip) | Windows | Includes an installer script that copies the add-on to the correct folder. |
| [PDS_Manager-Manual-Install.zip](./PDS_Manager-Manual-Install.zip) | macOS and Windows | Contains the add-on folder for manual installation. |

Both ZIP downloads contain the same add-on. The Windows installer adds `Install.cmd` for convenience. It installs the local add-on library files; it does not modify your FileMaker solution automatically.

## Installation

### Native .fmaddon package

Double-click **PDS_Manager.fmaddon** to install it in the local add-on library. If FileMaker displays a file-format/translator error, use either ZIP method above.

### Windows — installer

1. Download and extract the entire **Windows Installer ZIP**.
2. Close FileMaker Pro.
3. Double-click **Install.cmd** and follow the prompts. Administrator access is not required for this per-user installation.
4. Reopen FileMaker and follow **Add it to a FileMaker solution** below.

If your organization blocks downloaded scripts, use the manual method instead.

### macOS or Windows — manual installation

1. Download and extract **PDS_Manager-Manual-Install.zip**.
2. Close FileMaker Pro.
3. Copy the extracted **PDS_Manager** folder into the location for your platform:

**macOS** — in Finder, choose **Go → Go to Folder** and enter:

```text
~/Library/Application Support/FileMaker/Extensions/AddonModules/
```

**Windows** — press **Win + R** and enter:

```text
%LOCALAPPDATA%\FileMaker\Extensions\AddonModules\
```

Create `AddonModules` if needed. Place the folder so its structure is `AddonModules/PDS_Manager/info.json`, without an extra enclosing folder.

4. Reopen FileMaker Pro.

### Add it to a FileMaker solution

1. Open the destination file with Full Access.
2. Enter **Layout mode** and open the **Add-ons** pane.
3. Click **+**, select **PDS Manager**, and click **Choose**.
4. Drag the add-on onto your layout.

## Use

1. Select a **PDS Name**.
2. Select an **Instance ID** to view its current content.
3. Choose **Copy**, **Upload new version**, or **Delete**.

To add an entry, select a PDS Name, click **New entry**, enter a new Instance ID, and click **Add file**.

The content area is intentionally read-only: upload a replacement file to change the stored content. The manager refreshes the current content and ID list after its actions.

Deletion of **`addons` / `PDS-Manager.HTML`** is blocked by the manager to protect its own code. Other scripts that access the PDS directly remain responsible for their own safeguards.

## Add another PDS Name

The initial names are `widgets` and `addons`.

1. In the Web Viewer's address calculation, extend the list at the beginning of the `While` loop:

   ```filemaker
   names = List ( "widgets" ; "addons" ; "NEW VALUE" )
   ```

2. In **PDS.Value.Update** and **PDS.Value.Delete**, find the comment about adding additional PDS Names. Copy an existing `Else If` branch and change **both** its name comparison and the literal name in its **Configure Persistent Data** step.
3. Save the changes and reopen the manager layout so the Web Viewer address is evaluated again.

The HTML does not need changing to add a name.

## Updating and uninstalling

FileMaker may retain existing PDS values when an add-on is uninstalled and reinstalled. Do not assume reinstalling replaces the manager's stored HTML.

When updating, verify that **`addons` / `PDS-Manager.HTML`** contains the intended version; upload the new manager HTML explicitly if required. If completely removing the manager, remove that entry explicitly through a developer script when appropriate. Preserve unrelated PDS entries.

Replacing files in the local `AddonModules` folder updates the available package, not an add-on already installed in a FileMaker solution.

## Scripts

- **PDS.Manager.Action** — the only script called directly by the manager's JavaScript. Handles list/read requests and coordinates creation, upload, and deletion.
- **PDS.Value.Update** — selects a source file and creates or updates a PDS entry.
- **PDS.Value.Delete** — deletes a PDS entry.
- **PDS.Value.Read** — a separate production helper for retrieving native PDS values; the manager does not call it. Returns the native value on success and an error JSON object on failure.

## License

MIT License. See [LICENSE](./LICENSE).

Copyright © 2026 Thomas Staehli.

FileMaker is a Claris product. This is an independent add-on and is not an official Claris product.
