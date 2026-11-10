# BRAV-148 — Çıkışlar revizyon şartnamesi
2026-10-03. Kaynak Mustafa **10803, 10801, 10732, 10710, 10698, 10592**; docs origin/main **7ca7c6f**, sheets/F1_Phase1_AssetSheets_v001.md §4. E4 merkezini eski sheet yerine 10732/10803 belirler.

## Korunacak
**BRAV-148_E6_States_DRAFT_v004.png aynen korunacak**: haçsız sade oak/stone/iron; kapalı unlit, açık white-gold beacon/ring, kullanımda tek oyuncu channel. E6 araba içermez. v003 ve eski cross-bearing overall E6 görselleri superseded. Yeni E6 durum tasarımı yapılmayacak; yalnız eksik frame/leaf refs ve ölçü paftası tamamlanacak. Onaylı Jira attachment11950 yerel immutable kaynak olarak indirildi; aşağıdaki path/hash korunacak. Eski v002/v003 E6 tasarımları kullanılmayacak.

## Ortak kesin kurallar
Ring R **2.5 F**, yaklaşık20m okunabilirlik; white-gold yalnız açık/erişilebilir durumda; in-use oyuncu channel. Hareketli parçalar ayrı görüntü. Kapalı/açık/kullanımda paftaları altı çıkış için, E6 için onaylı v004'ü koruyarak.

| Çıkış | Parça / ölçü | Merkez x,z / faz |
|---|---|---|
| E1 | Gate_Obsidian_01 gri taş varyantı; hedef yaklaşık6.9W ×6–8H P; yeniden gate tasarlama | 0,−11.5 / Collapse |
| E2 | zemine oturan chute frame, 1.8×1.8 açıklık, lid0.15P | −112.7,105.8 / Descent |
| E3 | hearse frame/doors yaklaşık4.6W×4.5H P; buyerdesk2.0×0.8×1.0P; ledger; BRAV-153 aynı iki tekerlekli dead-cart static copy |40.25,126.5 / Contest |
| E4 | dış BRAV-335 bell tower, Signal_Bell_01 gri/dullmetal recolor Ø≈1.2P; rope Chapel batı duvarından içeri, floorpull |≈−6.9,66.7 / Contest |
| E5 | eğimli çift kanatlı bodrum frame+leaves yaklaşık2.3×2.3P |103.5,51.75 / Descent |
| E6 | v004 gate yaklaşık4.6W×5.5H P, ringR2.5F |0,158.7 / Collapse |
E3 floor−7.419; E5 floor−1.548; diğerleri0. E4 küçük bağımsız çan standı yok; climb yok. E4 opening/pull yakın görünüşü ve dışkule→duvar→çekmenoktası bağlantı paftası şart.

## Tamamlanabilir teslim
- E1–E5 üç durum paftası; E6 v004 byte-for-byte preserved + companion ölçü notu.
- E1 mevcut asset frame/leaf eksikleri ve recolor reference only.
- E2 ayrı hatch frame/lid 45.
- E3 ayrı gateframe/leaf, buyerdesk/ledger; cart BRAV-153 revizyonuyla aynı kaynak.
- E4 bell/ropepull ayrı45 ve ropewallopening detail (kule335 ayrı tasarımını kopyalama).
- E5 slopedframe/leaf ayrı45; simetrik birleaf ikinciyi temsil ediyorsa paftada belirt.
- E6 ayrı frame ve leaf45; basit oak ve hiçbirhaç.
- Yaklaşık P gate ölçülerini final fit verified olarak sunma;20m readability ve açıklık/hinge/rope teknik testleri laterQA.

## Onaylı E6 kaynak kilidi
Yerel dosya: `BRAV-148_E6_States_APPROVED_v004.png` (Jira attachment11950, Mustafa10801).
SHA256: `FC3AF869A6DD5B82574FA7A4A1AA1A70A25AC63E706735213F9D6A9EE351BFD0`. Kaynak dosyaya edit/overwrite yok; E6 frame/leaf bu kaynaktan ayrı türetilecek.

## Ortak teslim sınırı
Bu dosya yorum analizi ve üretim planıdır; yeni görsel onayı verilmiş değildir. Tripo/Unity üretimi veya Jira durum değişikliği başlatmaz. Yeni 45° görseller izole beyaz fonda, düz ışıkta, metinsiz, tek parça olacak; ölçü notları ayrı paftada kalacak. AI/ART_DIRECTION.md §4–§6 ve stil panosu her yeni üretimde kullanılacak. Haç ve yerine başka dinî amblem kullanılmayacak. Ölçüler m; F sabit, D türetilmiş, P tasarım önerisi. Nihai mesh/yerleşim uygunluğu henüz doğrulanmış değildir.


