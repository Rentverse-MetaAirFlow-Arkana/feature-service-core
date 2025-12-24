# 🏠 Rentverse Backend - Dokumentasi Fitur

Dokumentasi lengkap fitur dan workflow aplikasi Rentverse Backend - platform rental property marketplace.

## 📋 Daftar Isi

- [Overview](#overview)
- [Modul Utama](#modul-utama)
- [Role & Hak Akses](#role--hak-akses)
- [Workflow](#workflow)
- [API Endpoints](#api-endpoints)

---

## Overview

Rentverse adalah backend untuk aplikasi marketplace sewa properti yang dibangun dengan:

- **Runtime:** Node.js + Express
- **Database:** PostgreSQL + PostGIS (untuk data geografis)
- **ORM:** Prisma
- **Authentication:** JWT + OAuth (Google)

---

## Modul Utama

| Modul | Deskripsi |
|-------|-----------|
| **Authentication** | Register, Login, OAuth (Google), JWT Token |
| **Properties** | CRUD properti, pencarian, filter, GeoJSON untuk peta |
| **Bookings** | Sistem booking/sewa properti |
| **Amenities** | Master data fasilitas (AC, Pool, Gym, dll) |
| **Property Types** | Master data jenis properti (Apartment, House, dll) |
| **Predictions** | Prediksi harga properti (proxy ke AI model eksternal) |
| **Property Views** | Analytics view, rating, dan favorites |
| **Upload** | Upload file/gambar ke storage |

---

## Fitur Tambahan

| Fitur | Status | Deskripsi |
|-------|--------|-----------|
| **GeoJSON API** | ✅ Implemented | Data properti dalam format GeoJSON untuk rendering peta |
| **Property Views Analytics** | ✅ Implemented | Tracking view, unique viewers, statistik per periode |
| **Rating System** | ✅ Implemented | Rating 1-5 bintang dengan distribusi dan review text |
| **Favorites** | ✅ Implemented | Simpan properti favorit dengan toggle dan statistik |
| **Rental Agreement PDF** | ✅ Implemented | Generate dokumen perjanjian sewa otomatis (EJS template) |
| **Price Prediction** | ⚠️ Proxy Only | Proxy ke external AI model (tidak ada ML internal) |

---

## Role & Hak Akses

Sistem memiliki 2 role utama: **USER** dan **ADMIN**.

### 1. USER (Tenant / Landlord)

User dapat berperan sebagai **Tenant** (penyewa) atau **Landlord** (pemilik properti).

#### Fitur Umum

| Fitur | Deskripsi |
|-------|-----------|
| Register & Login | Daftar akun dengan email/password atau Google OAuth |
| Profile Management | Update profil, foto profil, tanggal lahir, nomor telepon |
| Link/Unlink OAuth | Hubungkan atau lepas akun Google |

#### Fitur Landlord (Pemilik Properti)

| Fitur | Deskripsi |
|-------|-----------|
| Create Property | Buat listing properti baru (status awal: `PENDING_REVIEW`) |
| My Properties | Lihat semua properti milik sendiri dengan summary status |
| Update Property | Edit detail properti (judul, harga, gambar, amenities, dll) |
| Delete Property | Hapus properti milik sendiri |
| View Statistics | Lihat statistik view properti (total views, unique viewers) |
| Manage Bookings | Lihat, approve, atau reject booking dari tenant |
| Rental Agreement | Generate dan download PDF perjanjian sewa |

#### Fitur Tenant (Penyewa)

| Fitur | Deskripsi |
|-------|-----------|
| Browse Properties | Lihat semua properti yang sudah `APPROVED` |
| Search & Filter | Filter berdasarkan city, price range, bedrooms, property type, dll |
| View Property Detail | Lihat detail lengkap properti termasuk amenities dan owner info |
| GeoJSON Map | Lihat properti dalam format peta dengan koordinat |
| Create Booking | Ajukan booking/sewa properti dengan tanggal dan catatan |
| My Bookings | Lihat status semua booking yang diajukan |
| Rate Property | Beri rating (1-5 bintang) dan review untuk properti |
| Favorite Property | Simpan properti ke daftar favorites |
| Download Agreement | Download PDF perjanjian sewa setelah booking disetujui |

---

### 2. ADMIN

Admin memiliki semua fitur USER ditambah fitur administrasi berikut:

#### Property Moderation

| Fitur | Deskripsi |
|-------|-----------|
| Pending Approvals | Lihat daftar properti dengan status `PENDING_REVIEW` |
| Approve Property | Setujui listing properti (ubah status ke `APPROVED`) |
| Reject Property | Tolak listing dengan alasan/catatan (ubah status ke `REJECTED`) |
| Auto-Approve Toggle | Aktifkan/nonaktifkan mode auto-approve untuk properti baru |
| Approval History | Lihat riwayat approval untuk setiap properti |
| Fix Inconsistency | Perbaiki data approval yang tidak konsisten |

#### Master Data Management

| Fitur | Deskripsi |
|-------|-----------|
| Property Types | CRUD jenis properti (Apartment, House, Villa, Studio, dll) |
| Amenities | CRUD fasilitas (WiFi, AC, Swimming Pool, Gym, Parking, dll) |

#### User Management

| Fitur | Deskripsi |
|-------|-----------|
| View All Users | Lihat semua user dengan pagination dan filter role |
| Create User | Buat user baru dengan role tertentu |
| Update User | Update data user termasuk role dan status aktif |
| Delete User | Hapus user dari sistem |

---

## Workflow

### A. Property Listing Flow

```
┌─────────────────────────────────────────────────────────────┐
│                    LANDLORD                                 │
│                       │                                     │
│                       ▼                                     │
│              Create Property                                │
│                       │                                     │
│                       ▼                                     │
│              ┌───────────────┐                              │
│              │ PENDING_REVIEW│                              │
│              └───────┬───────┘                              │
│                      │                                      │
│                      ▼                                      │
│               ADMIN Review                                  │
│                 /        \                                  │
│                /          \                                 │
│               ▼            ▼                                │
│        ┌──────────┐  ┌──────────┐                           │
│        │ APPROVED │  │ REJECTED │                           │
│        └────┬─────┘  └────┬─────┘                           │
│             │             │                                 │
│             ▼             ▼                                 │
│      Visible to      Not Visible                            │
│        Public       (with rejection notes)                  │
└─────────────────────────────────────────────────────────────┘
```

> **Note:** Jika Auto-Approve diaktifkan oleh Admin, properti baru langsung berstatus `APPROVED`.

---

### B. Booking / Rental Flow

```
┌─────────────────────────────────────────────────────────────┐
│                      TENANT                                 │
│                        │                                    │
│                        ▼                                    │
│               Browse Properties                             │
│                        │                                    │
│                        ▼                                    │
│               Select Property                               │
│                        │                                    │
│                        ▼                                    │
│               Create Booking                                │
│          (startDate, endDate, rentAmount)                   │
│                        │                                    │
│                        ▼                                    │
│               ┌─────────────┐                               │
│               │   PENDING   │                               │
│               └──────┬──────┘                               │
│                      │                                      │
│                      ▼                                      │
│              LANDLORD Review                                │
│                 /        \                                  │
│                /          \                                 │
│               ▼            ▼                                │
│        ┌──────────┐  ┌──────────┐                           │
│        │ APPROVED │  │ REJECTED │                           │
│        └────┬─────┘  └────┬─────┘                           │
│             │             │                                 │
│             ▼             ▼                                 │
│     Generate Rental    End Flow                             │
│     Agreement PDF    (with reason)                          │
│             │                                               │
│             ▼                                               │
│        ┌──────────┐                                         │
│        │  ACTIVE  │                                         │
│        └────┬─────┘                                         │
│             │                                               │
│             ▼                                               │
│        ┌───────────┐                                        │
│        │ COMPLETED │                                        │
│        └───────────┘                                        │
└─────────────────────────────────────────────────────────────┘
```

---

### C. Invoice & Payment Flow

```
┌─────────────────────────────────────────────────────────────┐
│                                                             │
│                  Lease ACTIVE                               │
│                       │                                     │
│                       ▼                                     │
│              Invoice Created                                │
│               (status: DUE)                                 │
│                       │                                     │
│                       ▼                                     │
│               Tenant Payment                                │
│          (BANK_TRANSFER / EWALLET /                         │
│           CREDIT_CARD / CASH)                               │
│                       │                                     │
│                       ▼                                     │
│              Payment Verified                               │
│           (status: COMPLETED)                               │
│                       │                                     │
│                       ▼                                     │
│               Invoice PAID                                  │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## API Endpoints

### Authentication

| Method | Endpoint | Deskripsi | Auth |
|--------|----------|-----------|------|
| POST | `/api/v1/auth/register` | Register user baru | - |
| POST | `/api/v1/auth/login` | Login user | - |
| GET | `/api/v1/auth/me` | Get current user profile | ✅ |
| POST | `/api/v1/auth/check-email` | Cek apakah email sudah terdaftar | - |
| GET | `/api/v1/auth/google` | Initiate Google OAuth | - |
| GET | `/api/v1/auth/google/callback` | Google OAuth callback | - |
| POST | `/api/v1/auth/oauth/link` | Link OAuth account | ✅ |
| POST | `/api/v1/auth/oauth/unlink` | Unlink OAuth account | ✅ |

### Users

| Method | Endpoint | Deskripsi | Auth | Role |
|--------|----------|-----------|------|------|
| GET | `/api/v1/users` | Get all users | ✅ | ADMIN |
| POST | `/api/v1/users` | Create user | ✅ | ADMIN |
| GET | `/api/v1/users/profile` | Get own profile | ✅ | - |
| PATCH | `/api/v1/users/profile` | Update own profile | ✅ | - |
| GET | `/api/v1/users/:id` | Get user by ID | ✅ | - |
| PATCH | `/api/v1/users/:id` | Update user | ✅ | - |
| DELETE | `/api/v1/users/:id` | Delete user | ✅ | ADMIN |

### Properties

| Method | Endpoint | Deskripsi | Auth | Role |
|--------|----------|-----------|------|------|
| GET | `/api/v1/properties` | Get all properties | - | - |
| GET | `/api/v1/properties/featured` | Get featured properties | - | - |
| GET | `/api/v1/properties/favorites` | Get user's favorites | ✅ | - |
| GET | `/api/v1/properties/geojson` | Get properties as GeoJSON | - | - |
| GET | `/api/v1/properties/my-properties` | Get own properties | ✅ | - |
| GET | `/api/v1/properties/pending-approval` | Get pending properties | ✅ | ADMIN |
| GET | `/api/v1/properties/property/:code` | Get property by code | - | - |
| GET | `/api/v1/properties/:id` | Get property by ID | - | - |
| POST | `/api/v1/properties` | Create property | ✅ | USER/ADMIN |
| PUT | `/api/v1/properties/:id` | Update property | ✅ | USER/ADMIN |
| DELETE | `/api/v1/properties/:id` | Delete property | ✅ | USER/ADMIN |
| POST | `/api/v1/properties/:id/view` | Log property view | - | - |
| GET | `/api/v1/properties/:id/view-stats` | Get view statistics | ✅ | USER/ADMIN |
| POST | `/api/v1/properties/:id/rating` | Create/update rating | ✅ | - |
| DELETE | `/api/v1/properties/:id/rating` | Delete rating | ✅ | - |
| GET | `/api/v1/properties/:id/ratings` | Get property ratings | - | - |
| GET | `/api/v1/properties/:id/my-rating` | Get own rating | ✅ | - |
| GET | `/api/v1/properties/:id/rating-stats` | Get rating statistics | - | - |
| POST | `/api/v1/properties/:id/favorite` | Toggle favorite | ✅ | - |
| GET | `/api/v1/properties/:id/favorite-status` | Get favorite status | ✅ | - |
| GET | `/api/v1/properties/:id/favorite-stats` | Get favorite stats | - | - |
| POST | `/api/v1/properties/:id/approve` | Approve property | ✅ | ADMIN |
| POST | `/api/v1/properties/:id/reject` | Reject property | ✅ | ADMIN |
| GET | `/api/v1/properties/:id/approval-history` | Get approval history | ✅ | - |
| POST | `/api/v1/properties/auto-approve/toggle` | Toggle auto-approve | ✅ | ADMIN |
| GET | `/api/v1/properties/auto-approve/status` | Get auto-approve status | ✅ | - |

### Bookings

| Method | Endpoint | Deskripsi | Auth |
|--------|----------|-----------|------|
| POST | `/api/v1/bookings` | Create booking | ✅ |
| GET | `/api/v1/bookings/my-bookings` | Get own bookings (tenant) | ✅ |
| GET | `/api/v1/bookings/owner-bookings` | Get bookings (landlord) | ✅ |
| GET | `/api/v1/bookings/property/:id/booked-periods` | Get booked periods | - |
| GET | `/api/v1/bookings/:id` | Get booking by ID | ✅ |
| POST | `/api/v1/bookings/:id/approve` | Approve booking | ✅ |
| POST | `/api/v1/bookings/:id/reject` | Reject booking | ✅ |
| GET | `/api/v1/bookings/:id/rental-agreement` | Get rental agreement | ✅ |
| GET | `/api/v1/bookings/:id/rental-agreement/download` | Download PDF | ✅ |

### Property Types

| Method | Endpoint | Deskripsi | Auth | Role |
|--------|----------|-----------|------|------|
| GET | `/api/v1/property-types` | Get all property types | - | - |
| GET | `/api/v1/property-types/:id` | Get property type by ID | - | - |
| POST | `/api/v1/property-types` | Create property type | ✅ | ADMIN |
| PUT | `/api/v1/property-types/:id` | Update property type | ✅ | ADMIN |
| DELETE | `/api/v1/property-types/:id` | Delete property type | ✅ | ADMIN |

### Amenities

| Method | Endpoint | Deskripsi | Auth | Role |
|--------|----------|-----------|------|------|
| GET | `/api/v1/amenities` | Get all amenities | - | - |
| GET | `/api/v1/amenities/categories` | Get amenity categories | - | - |
| GET | `/api/v1/amenities/:id` | Get amenity by ID | - | - |
| POST | `/api/v1/amenities` | Create amenity | ✅ | ADMIN |
| PUT | `/api/v1/amenities/:id` | Update amenity | ✅ | ADMIN |
| DELETE | `/api/v1/amenities/:id` | Delete amenity | ✅ | ADMIN |

### Predictions (Price Prediction)

| Method | Endpoint | Deskripsi | Auth | Role |
|--------|----------|-----------|------|------|
| GET | `/api/v1/predictions/status` | Get prediction service status | ✅ | - |
| POST | `/api/v1/predictions/toggle` | Toggle prediction service ON/OFF | ✅ | ADMIN |
| POST | `/api/v1/predictions/predict` | Predict property price (proxy ke AI model) | ✅ | - |

> **Note:** Fitur prediction saat ini berfungsi sebagai proxy ke external AI model service. Admin dapat mengaktifkan/menonaktifkan service ini.

### Upload

| Method | Endpoint | Deskripsi | Auth |
|--------|----------|-----------|------|
| POST | `/api/v1/upload` | Upload file (images) | ✅ |

---

## Status Enums

### Listing Status
- `PENDING_REVIEW` - Menunggu review admin
- `APPROVED` - Disetujui, visible ke publik
- `REJECTED` - Ditolak

### Lease/Booking Status
- `PENDING` - Menunggu review landlord
- `APPROVED` - Disetujui landlord
- `REJECTED` - Ditolak landlord
- `ACTIVE` - Sewa sedang berlangsung
- `COMPLETED` - Sewa selesai

### Invoice Status
- `DUE` - Belum dibayar
- `PAID` - Sudah dibayar
- `VOID` - Dibatalkan
- `REFUNDED` - Dikembalikan

### Payment Method
- `BANK_TRANSFER`
- `CASH`
- `EWALLET`
- `CREDIT_CARD`

### Payment Status
- `PENDING` - Menunggu verifikasi
- `COMPLETED` - Berhasil
- `FAILED` - Gagal
- `REFUNDED` - Dikembalikan
