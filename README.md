# Nyesem Firebase Rules

Bu repo **nyesem-app** Firebase projesinin tek doğru kaynağı (single source of truth) güvenlik kurallarını ve indexlerini barındırır.

## İçerik

- `firestore.rules` — Firestore güvenlik kuralları
- `firestore.indexes.json` — Firestore composite indexes
- `storage.rules` — Cloud Storage güvenlik kuralları

## Kullanım

Bu repo iki ana projeye **git submodule** olarak bağlıdır:

| Repo | Submodule yolu |
| --- | --- |
| `ysfhn/nyesem-admin` | `firebase-rules/` |
| `ysfhn/NyesemApp` | `firebase-rules/` |

İlgili `firebase.json` dosyalarında dosya yolları `firebase-rules/firestore.rules` gibi gösterilir.

## Düzenleme akışı

1. Bu repoyu klonla veya submodule içinden değişiklik yap
2. Değişikliği commit + push et (`main`)
3. Ana repo (`nyesem-admin` veya `NyesemApp`) içinde:
   ```bash
   cd firebase-rules
   git pull origin main
   cd ..
   git add firebase-rules
   git commit -m "chore: bump firebase-rules submodule"
   git push
   ```
4. Deploy:
   ```bash
   npx firebase-tools deploy --only firestore:rules,storage,firestore:indexes
   ```

## ⚠️ ÖNEMLİ

- **Asla** ana repolarda eski `firestore.rules` / `storage.rules` / `firestore.indexes.json` dosyaları bırakma — sadece submodule içindeki kullanılır.
- Deploy yapan kişi en son submodule'ü çekmiş olmalı (`git submodule update --remote`).
- Admin email listesi `storage.rules` içindeki `isStorageAdmin()` fonksiyonunda hardcoded — değişirse buradan güncelle.
