# BRAV-153 — Dead-cart tipper revizyon şartnamesi
2026-10-03. Kaynak Mustafa **10807,10719,10597**; docs origin/main **7ca7c6f**, F1_AssetSheets_Part2_v001.md §9. Eski fourwheel/longchain taslak geçersiz.

## Ölçüler ve mekanizma
Cart uzunluk **3.0F** (bed2.0+shafts1.0P), wheels üzerindenW **1.2F**, uprighttop **3.16F**. Yaklaşıkdepth1.5P, wheelØ0.9P, iki woodenwheel forgedirontyre; 2–3 shroudedloads ropeilecartınmeshinde. Hinge static cradle **0.5×1.3×0.16F**, localpivot(0,0.16,0). Hinge world(40.25,−7.419,123.90), yaw90; local−x=world+z ringeyön, localz=world+x hingeline. +90° localz tip **0.8sF**. E3ring(40.25,126.5),R2.5; hinge ringmerkezinden2.6south.
Open side ringe bakar, tippedpose yüzüstü rimüstüne iner, wheelsup; minimumfloorheight≥0. Formül: tippedheight=0.16+uprightlocalx; rimx−0.15P →0.01D, bedx≈+0.55P →0.71D. Buformül paftada doğrulanacak; gerçekmeshQA later.
Hinge toothedquadrant+pawl; StopPinringpull. Leverpost0.3×0.3×0.9P, drumØ0.3P; Lever0.7P grip≈1.2–1.4P, pull35°P. Lever/Drum/Cart ayrırefs. **Uzunaskızinciri yok**. Sheet'teki1.3m floor guardchannel drivechainP kısa transmisyon önerisidir; yeni mekanizma kararı uydurma, chain gerekmiyorsa görüntüde dominant yapma.

## Korunacak kararlar
Sade Renaissance dead-cart, oak/forgediron/linen; eskiBRAV126 görünüşü onaylı değil, yalnızhinge&lever fikri. E3buyer aynıcartınstaticcopy. Cycle E3blocked90s→hauledback3s→leverreset60sF; 0.8stipdeğişmez.

## Tamamlanabilir teslim
1. Hazır/devrilirken/devrilmiş/gerikaldırılmış dört durum paftası, tekcart&mechanism tutarlılığı; pivotarc/+90°,ringØ5 ve floorline görünür.
2. Ayrı45 Cart(two wheels),HingeBracket(quadrant/pawl interface),Lever,Drum; staticLeverPost gerektiğinde ayrıref,StopPin yakın detail.
3. Ölçülü front/side/plan ve hingecontact/top3.16 notu; no-floorpenetration analyticalcheck, actual meshverifieddeme.
4. Değişiklik listesi:4→2wheels,longchain→hingeratchet,face-downorientation,sharedE3cart.

## Ortak teslim sınırı
Bu dosya yorum analizi ve üretim planıdır; yeni görsel onayı verilmiş değildir. Tripo/Unity üretimi veya Jira durum değişikliği başlatmaz. Yeni 45° görseller izole beyaz fonda, düz ışıkta, metinsiz, tek parça olacak; ölçü notları ayrı paftada kalacak. AI/ART_DIRECTION.md §4–§6 ve stil panosu her yeni üretimde kullanılacak. Haç ve yerine başka dinî amblem kullanılmayacak. Ölçüler m; F sabit, D türetilmiş, P tasarım önerisi. Nihai mesh/yerleşim uygunluğu henüz doğrulanmış değildir.

