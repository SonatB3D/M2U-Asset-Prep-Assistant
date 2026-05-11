# -*- coding: utf-8 -*-
"""
M2U Asset Prep Assistant - Standalone Maya Installer

What this installer does:
    1. Creates a separate install folder inside the Maya user scripts directory.
    2. Writes m2u_asset_prep_assistant.py into that folder.
    3. Writes a friendly launcher file named "M2U Asset Prep Assistant.py".
    4. Copies m2u_asset_prep_icon.png if the icon is next to this installer.
    5. Creates the M2U_Tools shelf if it does not exist.
    6. Adds/updates the M2U Prep shelf button using the custom icon when available.

Update v04:
    - Removed the in-UI "Open M2U Studio Custom" integration button.
    - Verifies that m2u_asset_prep_assistant.py is physically written to disk.
    - Writes README_INSTALL_LOCATION.txt into the install folder with the exact paths.

Recommended package folder before running this installer:
    install_m2u_asset_prep_assistant.py
    m2u_asset_prep_icon.png

Run in Maya Script Editor, Python tab.
"""

from __future__ import print_function

import base64
import os
import shutil
import sys
import traceback
import zlib

import maya.cmds as cmds
import maya.mel as mel


INSTALLER_BUILD_ID = "v04_asset_prep_standalone_installer_disk_verified"
SHELF_NAME = "M2U_Tools"
SHELF_BUTTON_LABEL = "M2U Prep"
SHELF_BUTTON_ANNOTATION_ID = "M2U_ASSET_PREP_ASSISTANT_BUTTON"
INSTALL_FOLDER_NAME = "M2U_Asset_Prep_Assistant"
TOOL_MODULE_NAME = "m2u_asset_prep_assistant"
TOOL_FILE_NAME = "m2u_asset_prep_assistant.py"
FRIENDLY_LAUNCHER_FILE_NAME = "M2U Asset Prep Assistant.py"
ICON_FILE_NAME = "m2u_asset_prep_icon.png"
INSTALL_LOCATION_NOTE_FILE_NAME = "README_INSTALL_LOCATION.txt"

TOOL_SCRIPT_B64_ZLIB = """
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
"""

FRIENDLY_LAUNCHER_B64_ZLIB = """
eNqNUE1LxDAQvc+vGOqlFRtkT1LwIOhBWFkRPYdsm9qBNgmTKbr/3mS7XfayYC7z5s2bj5cbrG9rbH1H7rvBWfr6ITNQFMXWzK4dLGPvGd82X/gUoxV8ZxsypCjGiUpC6NlPqHU/y8xWa6QpeBYMTE4S61oh7+DE+riieIgAn7vdVj+/fuBjqqhgZFAdsTOTLdfc7GOOZdpAY5pfVUA9nhudFySXpx3lDWB6a6bIRctS3t+dGyoA4cMiO12yhJH2l+S0mbXJlnVIljNcLF9oUodiO3rTldfUFdjf1gbBl2NIH9H8awlcq6g4+J+ygj9s9Jek
"""


def _decode_payload(encoded_text):
    encoded_text = "".join(encoded_text.split())
    return zlib.decompress(base64.b64decode(encoded_text)).decode("utf-8")


def _norm(path):
    return os.path.abspath(os.path.normpath(path))


def _maya_user_script_dir():
    try:
        path = cmds.internalVar(userScriptDir=True)
        if path:
            return _norm(path)
    except Exception:
        pass
    return _norm(os.path.join(os.path.expanduser("~"), "maya", "scripts"))


def _safe_makedirs(path):
    if not os.path.isdir(path):
        os.makedirs(path)


def _write_utf8(path, text):
    folder = os.path.dirname(path)
    if folder:
        _safe_makedirs(folder)
    with open(path, "w", encoding="utf-8") as handle:
        handle.write(text)


def _assert_file_written(path, label):
    if not os.path.isfile(path):
        raise RuntimeError("{0} was not created on disk: {1}".format(label, path))
    try:
        if os.path.getsize(path) <= 0:
            raise RuntimeError("{0} was created but is empty: {1}".format(label, path))
    except OSError as exc:
        raise RuntimeError("{0} could not be verified: {1} | {2}".format(label, path, exc))


def _write_install_location_note(install_dir, tool_path, launcher_path, icon_path):
    note_path = os.path.join(install_dir, INSTALL_LOCATION_NOTE_FILE_NAME)
    lines = [
        "M2U Asset Prep Assistant install location",
        "",
        "Main tool module:",
        tool_path,
        "",
        "Friendly launcher:",
        launcher_path,
        "",
        "Icon:",
        icon_path or "Icon not installed / default Maya icon used",
        "",
        "Shelf:",
        SHELF_NAME,
        "",
        "Shelf button:",
        SHELF_BUTTON_LABEL,
        "",
        "Note:",
        "The shelf button imports m2u_asset_prep_assistant from this folder.",
    ]
    _write_utf8(note_path, "\n".join(lines))
    _assert_file_written(note_path, "Install location note")
    return note_path


def _get_installer_folder_candidates():
    candidates = []

    try:
        this_file = __file__
        if this_file and not str(this_file).startswith("<"):
            candidates.append(os.path.dirname(_norm(this_file)))
    except Exception:
        pass

    try:
        candidates.append(_norm(os.getcwd()))
    except Exception:
        pass

    try:
        candidates.append(_maya_user_script_dir())
    except Exception:
        pass

    clean = []
    seen = set()
    for path in candidates:
        if not path:
            continue
        path = _norm(path)
        if path in seen:
            continue
        seen.add(path)
        clean.append(path)
    return clean


def _find_icon_source(install_dir):
    installed_icon = os.path.join(install_dir, ICON_FILE_NAME)
    if os.path.isfile(installed_icon):
        return installed_icon

    for folder in _get_installer_folder_candidates():
        exact = os.path.join(folder, ICON_FILE_NAME)
        if os.path.isfile(exact):
            return exact

        no_ext = os.path.join(folder, os.path.splitext(ICON_FILE_NAME)[0])
        if os.path.isfile(no_ext):
            return no_ext

    return None


def _copy_icon_if_available(install_dir):
    icon_source = _find_icon_source(install_dir)
    if not icon_source:
        return None, "Icon not found. Expected file next to installer: {0}".format(ICON_FILE_NAME)

    icon_dest = os.path.join(install_dir, ICON_FILE_NAME)
    try:
        if _norm(icon_source) != _norm(icon_dest):
            shutil.copy2(icon_source, icon_dest)
        return icon_dest, "Icon installed: {0}".format(icon_dest)
    except Exception as exc:
        return None, "Icon copy failed: {0}".format(exc)


def _clean_python_cache(install_dir):
    removed = []
    pycache = os.path.join(install_dir, "__pycache__")
    if os.path.isdir(pycache):
        for file_name in os.listdir(pycache):
            lower_name = file_name.lower()
            if lower_name.startswith(TOOL_MODULE_NAME.lower()) and lower_name.endswith((".pyc", ".pyo")):
                path = os.path.join(pycache, file_name)
                try:
                    os.remove(path)
                    removed.append(path)
                except Exception:
                    pass
    return removed


def _add_to_sys_path(path):
    path = _norm(path)
    if path not in sys.path:
        sys.path.insert(0, path)


def _remove_loaded_tool_module():
    if TOOL_MODULE_NAME in sys.modules:
        try:
            del sys.modules[TOOL_MODULE_NAME]
        except Exception:
            pass


def _get_shelf_top_level():
    try:
        return mel.eval("global string $gShelfTopLevel; $tmp = $gShelfTopLevel;")
    except Exception:
        return "ShelfLayout"


def _ensure_shelf():
    shelf_top = _get_shelf_top_level()
    if not cmds.shelfLayout(SHELF_NAME, exists=True):
        cmds.shelfLayout(SHELF_NAME, parent=shelf_top)
    return shelf_top


def _delete_existing_prep_buttons():
    if not cmds.shelfLayout(SHELF_NAME, exists=True):
        return 0

    deleted = 0
    children = cmds.shelfLayout(SHELF_NAME, q=True, childArray=True) or []
    for child in children:
        try:
            if not cmds.shelfButton(child, exists=True):
                continue
            label = cmds.shelfButton(child, q=True, label=True) or ""
            annotation = cmds.shelfButton(child, q=True, annotation=True) or ""
            if label == SHELF_BUTTON_LABEL or SHELF_BUTTON_ANNOTATION_ID in annotation:
                cmds.deleteUI(child)
                deleted += 1
        except Exception:
            pass
    return deleted


def _make_shelf_command(install_dir):
    install_dir = install_dir.replace("\\", "/")
    return """import os
import sys

TOOL_DIR = r"{tool_dir}"
MODULE_NAME = "{module_name}"

if TOOL_DIR not in sys.path:
    sys.path.insert(0, TOOL_DIR)

try:
    import importlib
    if MODULE_NAME in sys.modules:
        module = importlib.reload(sys.modules[MODULE_NAME])
    else:
        module = __import__(MODULE_NAME)
except Exception:
    module = __import__(MODULE_NAME)

module.show()
""".format(tool_dir=install_dir, module_name=TOOL_MODULE_NAME)


def _create_shelf_button(install_dir, icon_path=None):
    _ensure_shelf()
    _delete_existing_prep_buttons()

    kwargs = {
        "parent": SHELF_NAME,
        "label": SHELF_BUTTON_LABEL,
        "annotation": "{0} | Opens M2U Asset Prep Assistant".format(SHELF_BUTTON_ANNOTATION_ID),
        "command": _make_shelf_command(install_dir),
        "sourceType": "python",
        "style": "iconAndTextVertical",
        "imageOverlayLabel": "Prep",
    }

    if icon_path and os.path.isfile(icon_path):
        kwargs["image1"] = icon_path.replace("\\", "/")
    else:
        kwargs["image1"] = "commandButton.png"

    cmds.shelfButton(**kwargs)

    try:
        mel.eval("global string $gShelfTopLevel; saveAllShelves($gShelfTopLevel);")
    except Exception:
        try:
            mel.eval("saveAllShelves $gShelfTopLevel;")
        except Exception:
            pass


def _verify_tool_import(install_dir):
    _add_to_sys_path(install_dir)
    _remove_loaded_tool_module()
    module = __import__(TOOL_MODULE_NAME)
    if not hasattr(module, "show"):
        raise RuntimeError("Installed module has no show() function.")
    return getattr(module, "M2U_PREP_BUILD_ID", "unknown")


def install_m2u_asset_prep_assistant(open_after_install=False):
    user_script_dir = _maya_user_script_dir()
    install_dir = os.path.join(user_script_dir, INSTALL_FOLDER_NAME)
    _safe_makedirs(install_dir)

    tool_code = _decode_payload(TOOL_SCRIPT_B64_ZLIB)
    launcher_code = _decode_payload(FRIENDLY_LAUNCHER_B64_ZLIB)

    tool_path = os.path.join(install_dir, TOOL_FILE_NAME)
    launcher_path = os.path.join(install_dir, FRIENDLY_LAUNCHER_FILE_NAME)

    _write_utf8(tool_path, tool_code)
    _write_utf8(launcher_path, launcher_code)
    _assert_file_written(tool_path, "Main tool file")
    _assert_file_written(launcher_path, "Friendly launcher file")

    icon_path, icon_message = _copy_icon_if_available(install_dir)
    note_path = _write_install_location_note(install_dir, tool_path, launcher_path, icon_path)
    removed_cache = _clean_python_cache(install_dir)

    _add_to_sys_path(install_dir)
    build_id = _verify_tool_import(install_dir)
    _create_shelf_button(install_dir, icon_path=icon_path)

    message_lines = [
        "M2U Asset Prep Assistant installed successfully.",
        "",
        "Installer Build: {0}".format(INSTALLER_BUILD_ID),
        "Tool Build: {0}".format(build_id),
        "Install Folder: {0}".format(install_dir),
        "Tool File: {0}".format(tool_path),
        "Launcher File: {0}".format(launcher_path),
        "Install Location Note: {0}".format(note_path),
        "Shelf: {0}".format(SHELF_NAME),
        "Shelf Button: {0}".format(SHELF_BUTTON_LABEL),
        icon_message,
    ]

    if removed_cache:
        message_lines.append("Old cache files removed: {0}".format(len(removed_cache)))

    final_message = "\n".join(message_lines)
    print(final_message)

    try:
        cmds.confirmDialog(
            title="M2U Asset Prep Assistant Installer",
            message=final_message,
            button=["OK"],
            defaultButton="OK"
        )
    except Exception:
        pass

    if open_after_install:
        module = sys.modules.get(TOOL_MODULE_NAME) or __import__(TOOL_MODULE_NAME)
        module.show()

    return {
        "install_dir": install_dir,
        "tool_path": tool_path,
        "launcher_path": launcher_path,
        "icon_path": icon_path,
        "install_location_note": note_path,
        "shelf": SHELF_NAME,
        "button": SHELF_BUTTON_LABEL,
        "tool_build": build_id,
    }


try:
    INSTALL_RESULT = install_m2u_asset_prep_assistant(open_after_install=False)
except Exception as exc:
    error_text = "M2U Asset Prep Assistant install failed:\n{0}\n\n{1}".format(exc, traceback.format_exc())
    print(error_text)
    try:
        cmds.confirmDialog(
            title="M2U Asset Prep Assistant Installer Error",
            message=error_text,
            button=["OK"],
            defaultButton="OK"
        )
    except Exception:
        pass
    raise
