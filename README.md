M2U ASSET PREP ASSISTANT
README

## Screenshots

<p align="center">
  <img src="M2U Asset Prep Assistant images/m2u_apa.png" alt="M2U Asset Prep Assistant Screenshot 1" width="900">
</p>

<p align="center">
  <img src="M2U Asset Prep Assistant images/m2u_apa2.png" alt="M2U Asset Prep Assistant Screenshot 2" width="900">
</p>

<p align="center">
  <img src="M2U Asset Prep Assistant images/m2u_apa3.png" alt="M2U Asset Prep Assistant Screenshot 3" width="900">
</p>

<p align="center">
   <img src="M2U Asset Prep Assistant images/m2u_apa4.png" alt="M2U Asset Prep Assistant Screenshot 4" width="900">
</p>

M2U Asset Prep Assistant
Standalone Installer v04 - Disk Verified

Author / Brand:
Sonat Birdane
M2U Studio Toolkit


OVERVIEW

M2U Asset Prep Assistant is a companion cleanup tool for Autodesk Maya static mesh workflows.
It is designed to prepare selected Maya mesh assets before final validation and FBX export with
M2U Studio Custom.

This tool focuses on safe, artist-controlled preparation steps such as:

- freeze transform
- delete construction history
- pivot placement
- grid alignment analysis
- optional grid snapping
- naming prefix preparation
- asset name sanitizing
- visibility and locked transform attribute checks
- hidden backup creation before fixing

M2U Asset Prep Assistant does not export FBX files. Final validation, UCX collision checking,
FBX export, JSON reporting, and export blocking logic are handled by M2U Studio Custom.


M2U STUDIO CUSTOM COMPATIBILITY

M2U Asset Prep Assistant is made to work cleanly with M2U Studio Custom.

Recommended workflow:

1. Select your static mesh asset or assets in Maya.
2. Open M2U Asset Prep Assistant from the M2U_Tools shelf.
3. Run Analyze Selected Assets.
4. Enable only the fix options you want.
5. Run Fix Selected Assets Using Enabled Options.
6. Open M2U Studio Custom.
7. Run Validate Selected Assets.
8. Export Selected Assets from M2U Studio Custom.

The two tools are compatible because they follow the same general production logic:

- Maya Y axis is treated as height / up.
- Front Axis is limited to +X, -X, +Z, -Z.
- Pivot target behavior is designed to match the M2U Studio Custom workflow.
- UCX_* collision meshes are ignored as base assets in Asset Prep.
- Asset Prep prepares meshes before M2U Studio Custom performs final checks.
- M2U Studio Custom remains responsible for UCX validation and FBX export.

This makes Asset Prep Assistant a pre-validation and cleanup stage for M2U Studio Custom.


INSTALLATION

Recommended installer package folder:

    install_m2u_asset_prep_assistant.py
    m2u_asset_prep_icon.png

The icon file is optional, but recommended. If the PNG icon is placed next to the installer,
the installer copies it into the tool install folder and uses it for the shelf button.

Run the installer in Autodesk Maya:

1. Open Maya.
2. Open Script Editor.
3. Switch to the Python tab.
4. Paste or run the installer script. 
(or copy from inside install_m2u_asset_prep_assistant.txt file, paste in script editor
and run it)
5. The installer creates the tool files and adds the shelf button.

The installer creates a separate folder inside the Maya user scripts directory.

Example Maya path:

    C:\Users\....\Documents\maya\....\scripts\M2U_Asset_Prep_Assistant\

The exact path can vary depending on Maya version, user settings, and Windows Documents /
OneDrive configuration.


INSTALLED FILES

After installation, the folder should contain files similar to:

    M2U_Asset_Prep_Assistant/
        m2u_asset_prep_assistant.py
        M2U Asset Prep Assistant.py
        m2u_asset_prep_icon.png
        README_INSTALL_LOCATION.txt

Main tool module:

    m2u_asset_prep_assistant.py

Friendly launcher file:

    M2U Asset Prep Assistant.py

Icon file:

    m2u_asset_prep_icon.png

Install location note:

    README_INSTALL_LOCATION.txt


SHELF INSTALLATION

The installer uses the M2U_Tools shelf.

If M2U_Tools does not exist, the installer creates it.
If M2U_Tools already exists, the installer adds or updates the M2U Prep button inside it.

Shelf button:

    M2U Prep

The button imports and runs:

    m2u_asset_prep_assistant.show()

The shelf command also reloads the module so updates are reflected more reliably after
reinstalling the tool.


MAYA VERSION NOTES

The installer installs into the active Maya version's user scripts folder.

For example, when running inside Maya 2024, the tool may be installed here:

    Documents/maya/2024/scripts/M2U_Asset_Prep_Assistant/

If you want to use the tool in Maya 2025, Maya 2026, or another version, run the installer
from that Maya version's Script Editor as well.


BASIC USAGE

1. Select one or more mesh transforms.
2. Click M2U Prep from the M2U_Tools shelf.
3. Choose the Front Axis that matches your asset.
4. Run Analyze Selected Assets.
5. Review warnings and manual review notes.
6. Enable only the desired fix options.
7. Run Fix Selected Assets Using Enabled Options.
8. Run Analyze Again if needed.
9. Continue with M2U Studio Custom for final validation/export.


DEFAULT SAFETY BEHAVIOR

The tool is designed to avoid destructive automatic behavior.

Safe defaults include:

- Duplicate Backup Before Fix is enabled.
- Hide Backups is enabled.
- Freeze Transform is enabled.
- Delete Construction History is enabled.
- Move Pivot To Target is enabled.
- Add Prefix If Missing is disabled by default.
- Snap Bounds Min To Grid is disabled by default.
- Snap Pivot To Grid is disabled by default.
- Unlock Transform Attributes During Fix is disabled by default.
- Make Selected Assets Visible During Fix is disabled by default.

This keeps the artist in control of changes that may affect scene organization, layout,
or intentional locked/hidden states.


WHAT THIS TOOL DOES NOT DO

M2U Asset Prep Assistant does not:

- export FBX files
- import anything into Unreal Engine
- create UCX collision automatically
- validate final UCX export selection
- write final export JSON reports
- resize geometry automatically
- guarantee that an asset will pass every M2U Studio Custom rule

Use M2U Studio Custom after this tool for final validation and export.


RECOMMENDED PIPELINE

For static mesh assets targeting Unreal Engine:

    Asset modeling in Maya
    -> M2U Asset Prep Assistant
    -> M2U Studio Custom validation
    -> M2U Studio Custom FBX export
    -> Unreal Engine import

This keeps preparation and final export validation separated but compatible.


UNINSTALL

To manually remove the tool:

1. Delete the folder:

    M2U_Asset_Prep_Assistant

from the Maya user scripts directory.

2. Remove the M2U Prep shelf button from the M2U_Tools shelf if desired.

If other M2U tools are installed, do not delete the entire M2U_Tools shelf unless you want to
remove those buttons as well.


TROUBLESHOOTING

If the shelf button does not show:

- Make sure you ran the installer in the Maya Python tab.
- Check that M2U_Tools shelf exists.
- Restart Maya if the shelf UI did not refresh.

If the icon does not show:

- Make sure m2u_asset_prep_icon.png is placed next to the installer before running it.
- Re-run the installer.
- The tool still works without the icon; Maya will use a default shelf icon.

If you cannot find the installed .py file:

- Check the version-specific Maya scripts folder.
- Example: Documents/maya/2024/scripts/
- The installer may not use the general Documents/maya/scripts/ folder.

If the tool opens but you do not see the expected files:

- Run this in Maya Python tab:

    import maya.cmds as cmds
    print(cmds.internalVar(userScriptDir=True))

This shows the active Maya user scripts directory.
