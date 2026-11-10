# Bravehood Production Archive

Production documents, item inventories, concept images and review captures. Initial import: 2026-09-08 from the local Bravehood Unity working tree.

- [Master index](MASTER_INDEX.csv): 474 entries with original project-relative source paths, byte counts and SHA-256 checksums.
- [Master item list](02_Weapons_Equipment/MASTER_ITEM_LIST.csv): 14 items exported from Unity ItemDefinition assets. Category names come from ItemCategory. Zero values and empty descriptions are retained. Payload fields preserve Unity references; the CSV does not resolve combat stats.

The numbered folders follow the production archive structure. Knight concepts have their own character folder because the source does not identify them as Mage, Barbarian or Assassin. Empty folders are reserved with .gitkeep. No material has been assigned an Approved status during import.

## Scope

Original files are copied byte for byte. Source snapshots may include current uncommitted work and historical reports, and may contain project-relative links that require the Unity checkout. Pipeline documents are reference copies, not installed skills. The index covers imported files and the generated item list, excluding repository housekeeping and the index itself.

This import covers project design notes, PDFs, animation reports, pipeline reference documents, item definitions, item/UI sprites, development screenshots and generated images. Videos, Unity runtime models/textures/scenes, third-party packages, caches and recordings are outside this document-and-image import. No existing CSV/XLSX item list or Jira export was found in the project. The Jira folder is ready for later exports.

## Local use

The archive is stored at Bravehood-Production-Archive inside the Unity repository and registered as a Git submodule. On another machine use git submodule update --init --recursive after cloning the parent repository. Commit and push archive changes inside this folder, then commit the updated submodule reference in the parent repository.

To add material, copy it into its production category and update MASTER_INDEX.csv with the source path and checksum. Keep original sources intact.

## Research additions

- [BRAV-111 — accepted Knight body B1–B5](08_Research/Characters/Knight/BRAV-111_Body/README.md): accepted by Mustafa on 2026-09-25; image references, measured B1, prompts and Figma-ready ruler. Unrigged 3D body and master rig remain pending.
