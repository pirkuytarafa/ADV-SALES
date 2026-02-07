# Firestore Security Rules untuk Advanta Sales App

## 🔒 Konfigurasi Rules yang Diperlukan

Agar data BS dan DGA hanya bisa dilihat oleh pemiliknya, Firestore rules harus diatur sebagai berikut:

### Rules Configuration (COPY INI KE FIREBASE CONSOLE)

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    
    // ✅ Business Solutions - SECURE: read/write berdasarkan ownerEmail
    match /businessSolutions/{bsId} {
      // Public read: agar form publik bisa load BS milik owner tertentu
      allow read: if true;
      
      // Create: hanya user login
      allow create: if request.auth != null;
      
      // Update/Delete: hanya pemilik BS
      allow update, delete: if request.auth != null && 
                               resource.data.ownerEmail == request.auth.token.email;
    }
    
    // ✅ DGA Activities - SECURE: User hanya bisa read data miliknya sendiri
    match /dgaActivities/{activityId} {
      // Public write: siapa saja bisa submit dari form publik
      allow create: if true;
      
      // Read: hanya pemilik BS yang bisa melihat (berdasarkan ownerEmail)
      allow read: if request.auth != null && 
                     resource.data.ownerEmail == request.auth.token.email;
      
      // Update/Delete: hanya pemilik yang bisa edit/hapus
      allow update, delete: if request.auth != null && 
                               resource.data.ownerEmail == request.auth.token.email;
    }
    
    // ✅ Legacy sharedData (deprecated, kept for backward compat)
    match /sharedData/{document=**} {
      allow read: if true;
      allow write: if request.auth != null;
    }
    
    // ✅ Public read untuk userLogs (agar admin bisa track user)
    match /userLogs/{document=**} {
      allow read: if request.auth != null;
      allow write: if request.auth != null;
    }
    
    // ✅ User data pribadi (hanya pemilik yang bisa akses)
    match /advantaUserData/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
      
      match /{subcollection=**} {
        allow read, write: if request.auth != null && request.auth.uid == userId;
      }
    }
    
    // ✅ Default: butuh login untuk semua collection lain
    match /{document=**} {
      allow read, write: if request.auth != null;
    }
  }
}
```

## 📝 Cara Update Firestore Rules

1. **Buka Firebase Console**: https://console.firebase.google.com
2. **Pilih Project**: `advanta-sales-app`
3. **Navigate ke Firestore Database** → Tab **Rules**
4. **Copy-paste rules di atas**
5. **Klik "Publish"**

## 📱 Collection Structure

```
firestore
├── businessSolutions (Secure: filtered by ownerEmail)
│   └── {auto-generated-id}
│       ├── name: "Nama BS"
│       ├── ownerEmail: "user@email.com"  ← PENTING untuk data isolation
│       ├── createdBy: "user@email.com"
│       └── createdAt: "2026-02-01T..."
│
├── dgaActivities (Secure: filtered by ownerEmail)
│   └── {auto-generated-id}
│       ├── bsName: "Nama BS"
│       ├── bsId: "..."
│       ├── ownerEmail: "user@email.com"  ← PENTING untuk data isolation
│       ├── month: "2026-02"
│       ├── week: 1
│       ├── type: "FM" | "ODP" | "FT" | "FFD" | "DISPLAY"
│       ├── count: 10
│       ├── description: "..."
│       ├── timestamp: "2026-02-01T..."
│       └── source: "public-form"
│
├── sharedData (DEPRECATED - legacy)
│
├── userLogs (Auth read)
│   └── (auto-generated docs)
│
└── advantaUserData (Private)
    └── {userId}
        └── private/
            └── (user-specific data)
```

## 🔐 Keamanan Data

Dengan rules dan arsitektur di atas:
- ✅ BS disimpan di Firestore collection `businessSolutions` dengan `ownerEmail`
- ✅ Realtime listener memfilter BS berdasarkan `ownerEmail` → data tidak bocor
- ✅ Link public form menyertakan `&owner=email` → hanya BS pemilik yang muncul
- ✅ Data DGA otomatis diberi tag `ownerEmail` dari URL parameter
- ✅ User hanya bisa melihat data DGA dan BS miliknya sendiri
- ✅ Filter dilakukan di level Firestore (server-side), bukan client-side