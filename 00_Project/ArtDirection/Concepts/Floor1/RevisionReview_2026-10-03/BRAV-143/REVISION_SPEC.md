# BRAV-143 revizyon ölçü ve üretim planı
Kaynak: jira-evidence.json Mustafa #10805, #10722; Part2 §12.
## Ölçüler ve değişiklikler
- Dik boş elli A-pose 1.80 m P; eski 1.70 kaldır. Hunch yalnız animasyonda. Master rig oranları: çene1.50, collar1.44, shoulder1.40/width0.56, belt1.01, hip0.90, knee0.45, wrist0.86 m D. Knight rig/avatarı değişmez; düşmana kendi avatarı, 45-bone düzeni.
- Başlık ve insan yüz ailesi: kare düzlemsel burun, köşeli çene, kalın kaş, küçük göz; düzensiz geniş yüzeyler. Baş/hood collar'da biter, torso kollar+elleri içerir; legs çıplak ayak. Çizme yok. Diz hizasına yakın etek (sheet hem≈0.54m, knee0.45m), iki yandan yırtmaç, ön/arka panel; opsiyonel kısa cowl. Kalkan yok.
- Buhurdan Ø0.30 ×0.45 m F; body0.40+top ring0.05. Demir, ventler, kapalı dome kapak tek mesh. Pivot top ring; Socket_Ember body centre ringden≈0.20m aşağı P. Ward-green kor, orange değil; unlit ref, emissive/VFX ayrı not.
- Zincir toplam1.1 m P, serbest0.45; kalanı sağ yumruk/önkola sarılır. ≈14 link 0.08×0.045, wireØ0.015; grip ringØ0.08 R_Hand. Zincir Blender işi, image generation/Tripo mesh değil. Projectile aynı buhurdan zincirsiz. Censer≤1.5k,chain≤0.5k tris P; beden bütçesi unverified.
## Gerekli referanslar
Body_45 (tam boy dik boş elli A-pose) ve Censer_45 ayrı. Ölçülü paftada lob wind-up ve kasıtlı backpedal gösterilebilir; BRAV-139 rig/animasyon kapsamına değişiklik yok. Body part names Head/Torso/Legs/Cloth_Skirt/optional Cowl üretim notu; bu aşamada ayrı head torso render şartı değil.

## Ortak teslim ve kontrol
Son Mustafa yorumu + bravehood-docs origin/main 7ca7c6f asset sheet esas. F=fixed, D=derived, P=sheet proposal; son 2026-10-03 revizyon yorumu kabul edilmiş ölçü yönünü belirler, yeni görsel onayı değildir. Ölçülü pafta ve ayrı 45° tam görünür beyaz fonlu yazısız parça referansları, kısa değişiklik listesi. Paftada Knight 1.59 m ölçek karşılaştırması. Environment BOARD ve ART_DIRECTION §4/5 her üretimde; BRAV-143 beden için Character BOARD ve §3/5. Faceted düz boyama, muted palet, haç/heraldry yok; ışık/VFX referans meshine pişirilmez. Minimum piksel kuralı yok; büyütme yok. Tripo, Unity, rig/scene/placement yapılmayacak. Her yeni referans mevcut onaylı tasarımla karşılaştırılacak; teknik runtime kontrolleri tamamlanmış sayılmayacak.

