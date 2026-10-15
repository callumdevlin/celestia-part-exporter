# Changelog: Celestia Part Exporter

A record of every version of the Celestia Part Exporter, newest first. Each released version lists **New Features** and **Bug Fixes**.

## How this file works

- **Released versions** are listed below the Planned section, newest first.
- **The Planned section at the top is the working list for the next version.** Add what you want changed as you think of it. Tick items off once they ship.
- **When building a new version (for Claude):** read the Planned section first. Ask about anything unclear or listed under *Open questions*, build every item, then move the entries into a new released section and clear Planned.

**Release checklist**

1. Move the Planned entries into a new released section and start a fresh Planned section.
2. Bump `APP_VERSION` in the script.
3. Update `latest_version` (and `message`) in `update.json` so users get the update popup.
4. Rebuild the EXE and share it.

---

## Planned: v1.16 (not yet released)

> Working list for the next version. Edit freely.

### New Features

- [ ] **Multi-folder selection.** Scan more than one source folder and pool the files into the single "Files to Process" list.
  - *Design TBC.* Suggested approach: make each scan additive (it adds to the list instead of replacing it), add a **Clear list** button, and show the source folder on each row so identical part numbers from different folders can be told apart.

- [ ] **Smarter revision defaults for parts and drawings.** Check the revision per file type (part/assembly STEP vs drawing PDF) instead of one highest revision across the whole folder:
  - Exporting **only the drawing** when the part is already in the folder: default to the **part's current revision** (the same revision, not one above).
  - Exporting the **part and drawing together**: default to the **next revision** for both.
  - A part and its drawing in the same export **must always share the same revision**. They cannot be exported at different versions.

### Bug Fixes

- [ ] *(Suggested, please confirm)* **Keep part and drawing paired when resolving a revision clash.** At the moment "Bump to next rev" is applied per file, so it can push the part and drawing onto different revisions. Bumping should move the pair together.

### Open questions

- If only the **part** is being exported and the **drawing** already exists, should the part default to the drawing's revision (the mirror of the rule above)?
- If the part and drawing are **already at different revisions** in the folder and only one is exported, which one should it follow?
- Does the same pairing apply to **assembly + drawing** (SLDASM treated like a part)?

---

## v1.15

### New Features

- **Larger "Files to Process" panel and smaller Activity Log.** The two panels are now a fixed ~70/30 split, so more files are visible at once.
- **Removed the CSV upload history.** There is no need to log history. The in-app Activity Log is unchanged.
- **New Release revision format: `Rev01`, `Rev02`, `Rev03`…** (previously `Rev1.0`). Review revisions are unchanged (`Rev0.01`, `Rev0.02`…). Existing `Rev1.0`-style files are still recognised when working out the next revision.
- **New output option: "Don't keep a local copy".** Exports to a temporary location, files into Design Records, then deletes the temporary files. It sits alongside **Next to source files** (still the default) and **Custom folder**.

### Bug Fixes

- **Fixed SolidWorks hanging then crashing when exporting files "submitted for checking" in PDM (read-only).** Files are now opened silently and read-only, matching the manual "ignore the read-only popup" workflow. *(To be confirmed in testing on real checked-in files.)*
- **Fixed the Convert & Upload and Dry Run buttons being cut off** at the bottom of the window.
- **Fixed the Activity Log's left corners not being rounded** to match the right.
- **Fixed a startup error when the window was closed while the update check was still running.**

---

## v1.14

### New Features

- **Optional comment on the notification email.** A comment box sits below the recipient list in the confirmation window and is added to the email under "Comments:".
- **Exporting to the source folder no longer creates a sub-folder.** Converted files are saved directly beside the original file in the PDM folder, replacing any existing file of the same name.
- **Exporting to the source folder no longer adds a file-type suffix.** The file keeps the exact same name as the original with only the extension changed (for example `CPN00169.step`).
- **Update and notice popup.** On startup the app checks a small file hosted on GitHub. If the published version differs from the installed one, a dismissible popup shows the message (info, warning or critical). It fails silently when offline.
- **Assembly workbooks (RW/AW) handled when a new part folder is created from the template.** For assemblies the workbooks are kept and renamed to the part number. For parts they are removed. Only the new copy is changed, never the template.

### Bug Fixes

None.

---

## v1.13

### New Features

- **Standard email subject with the send date**, so notifications are easy to find: `SolidWorks Part Release - 19 Jun 2026` (Review exports use `SolidWorks Part Review - <date>`).
- **The email now states the status of each item**, for example `CPN00169  Rev1.0 - Release` or `CPN00169  Rev0.02 - Review`.
- **Prompt to create a missing part folder.** If a part number has no folder in Design Records, you are asked whether to create one from the standard template. This is checked for every part number in the export (Yes = create it, No = skip that part, Cancel = stop the run).
- **Notification emails are now available on Review exports**, not just Releases. Recipients are still chosen per run.
- **Celestia "C" icon** for the window, taskbar and EXE, plus an EXE build script and build guide.

### Bug Fixes

None.

---

## v1.0 – v1.12: Foundation

*Earlier versions were not tracked individually. This summarises what the tool did before changelog tracking began.*

### New Features

- Scan a PDM folder and list parts (`.SLDPRT`), assemblies (`.SLDASM`) and drawings (`.SLDDRW`). Tick the files to export, and correct a part number if the filename isn't valid.
- Converts through SolidWorks: parts and assemblies to **STEP** (AP203 by default, AP214/AP242 selectable) and drawings to **PDF** (all sheets in one PDF).
- Automatic revision numbering for **Review** (`Rev0.0X`) and **Release**. Revisions are suggested from what's already filed and can be edited before running.
- Files are placed in each part's Design Records folder (`01 Review` / `02 Release`) with consistent naming.
- Overwrite protection (bump to the next revision, overwrite, or skip), **Dry Run** preview, **Cancel** during a run, and **Open Folder** after a run.
- Persistent per-user settings. The Design Records path defaults from the Windows username.
- Notification emails with opt-in recipient selection, sent through Outlook automatically with fallbacks to a draft or a copy-and-paste window.
- Celestia-branded dark interface.

### Bug Fixes

- Replaced the SolidWorks Task Scheduler approach, which wasn't processing jobs, with direct SolidWorks automation.
- Fixed "Type mismatch" and "PyOleMissing" errors when opening and exporting files.
- Fixed the Confirm and Cancel buttons being hidden in the revision confirmation window.
- Fixed notification emails not opening, by adding Outlook integration with fallbacks.
