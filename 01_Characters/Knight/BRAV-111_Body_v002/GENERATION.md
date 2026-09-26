# Knight base-body rebuild — BRAV-111 / BRAV-114

Requested 2026-09-26 after missing armpit surfaces were reported. Replacement task fd5df216-fb3e-41cd-b5ca-417d59b4eafc. Existing rig and live assets preserved.

Inputs: approved Knight_Body_B1_FrontA / B3_LeftProfileA / B2_BackA PNGs under .source/00_Project/ArtDirection/Concepts/Characters/Knight/Body, supplied to Front / Left / Back. Right empty. Smart Mesh P2.0, Triangle, target 20000, one generation, Private, no HD/style/auto-rig/pose conversion. No symmetry control exposed. Geometry output 17346 triangles, 8624 viewer geometry vertices. Standard 2K texture, Remove Lighting OFF, 23304 textured viewer vertices. Simple segmentation requested before FBX export.

QA context: old raw, processed and equipped FBXs retain identical 248 triangles including winding in bilateral normalized armpit region |x| .07–.22, z .58–.82. No welded boundary edges in tighter region |x| .09–.205, z .60–.79. Processing did not remove or reverse these faces. The reported visual defect remains unlocalized; static topology is not a deformation test.

Status: APPROVED by Mustafa on 2026-09-26 as the new Knight base model. Knight_Body_v2.fbx replaces Knight_Body_v1.fbx for subsequent work. Static armpit check passed; game-rig deformation remains untested. Tripo:13 named components, PBR pass completed,17346 triangles /19487 final viewer vertices,155 credits. Export options FBX /Blender /2k Current /Pack UV OFF; manual download completed after Chrome blocked the automated download.


## Download and preparation completed 2026-09-26
User downloaded low-poly+male+character+3d+model.zip (3,770,078 bytes); original retained as raw/Tripo_fd5df216.zip. All 13 named components present. This actual download contains basecolor and normal maps only, despite the earlier Tripo PBR pass; no metallic/roughness maps are claimed.

processed/Knight_Body_v2.fbx contains exactly Knight_Head, Knight_Torso and Knight_Legs. Geometric counts: Head 7568 vertices /15199 triangles; Torso 813 /1530; Legs 444 /741. Total 8825 vertices /17470 triangles. Added triangles come from collar and waist cuts/overlap. Height normalized to1, facing -Y, shared floor pivots, collar .80, inner waist overlap .54–.58. No armpit face deletions or repair were applied. No rig, scene or live equipment changes.

Six 2048-square PNG atlases consolidate the delivered basecolor/normal maps. Large 2048 source tiles were reduced to1016 pixels to fit a combined2K atlas; original source maps remain archived unchanged. One material per final part. Flat shading follows the project workflow; it can increase rendered vertex counts significantly.

Source density caveat: the two eye components consume13723 of17346 source triangles; the torso core has378. A higher total count did not improve body topology evenly. Approved model replacement; fitting and validation on the current rig remain pending.

Raw armpit QA:271 edges examined in bilateral |x|.07–.22, normalized z.58–.82; zero boundary edges, nonmanifold edges or winding conflicts after diagnostic positional welding. Front/back underside renders show closed surfaces. Posed deformation and Unity fit remain untested. Editable source .source/Characters/Knight_BRAV114_fd5df216.blend.


Degenerate cleanup: removed69 zero/near-zero-area eye/beard triangles (area below1e-12); no torso or leg faces removed. Export checks use MeshLoopTriangle.area as well as double-precision cross-product area.


Final static export QA PASS; estimated rendered vertices35375. See QA.md for criteria and limits. Posed/master-rig acceptance pending.


