# Knight base body v002 — replaces v1 delivery

**APPROVED by Mustafa on 2026-09-26 as the new Knight base. Knight_Body_v2.fbx replaces Knight_Body_v1.fbx for subsequent work.** Earlier files remain historical. Rig integration remains pending; this publication does not change the live game rig.

[Download FBX + PNG + QA package](Knight_Body_v2_Review.zip)

Three meshes: Knight_Head, Knight_Torso, Knight_Legs. 17,470 triangles /8,825 geometric vertices. Six2K PNG basecolor/normal atlases. Unrigged, height1, facing−Y. Tripo source task fd5df216-fb3e-41cd-b5ca-417d59b4eafc, Smart Mesh Triangle20000,2K texture, Remove Lighting OFF.

Static mesh QA passed: both underarms closed, all169 tested armpit faces preserved, zero degenerate triangles after cleanup. See QA.md and underside images. The old reported visible defect was not reproduced by static tests; posed deformation remains untested. Rig fitting, Unity measurement and runtime acceptance are still pending.

Estimated render vertices35,375, above the earlier18–25k target. Source eye parts consume most geometry. Original download preserved as Tripo_fd5df216.zip; source textures were retained there unchanged. Consolidation reduced large tiles to fit2K atlases. Actual download supplied only basecolor/normal, despite the Tripo PBR pass.

Tickets: [BRAV-111](https://blbrn-team.atlassian.net/browse/BRAV-111), [BRAV-114](https://blbrn-team.atlassian.net/browse/BRAV-114).

