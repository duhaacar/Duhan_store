# Duha_store

PS3 için Türkçe homebrew mağaza uygulaması (PSL1GHT + tiny3d + libfont).

- Kumanda: Yukarı/Aşağı gezin, X indir, O çık
- Derleme: GitHub Actions (`.github/workflows/build.yml`) -> Actions sekmesi -> Artifacts -> `Duha_store-pkg`
- Kurulum: `.pkg` dosyasını USB'ye koy, PS3'te Game > Package Manager > Install Package Files
- Varlıkları yeniden üretmek için: `python3 tools/make_assets.py`

Yalnızca kendi yaptığınız veya lisansı uygun homebrew paketlerini dağıtın.
