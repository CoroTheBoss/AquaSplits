# AquaSplit Backend

> **This repository documents the AquaSplits structure and architecture. The source code is private. If you'd like more details, feel free to contact me.**

AquaSplit Backend is a NestJS application that fetches swimming race data from external sources (FICR and future sources), parses it into structured MongoDB schemas, and exposes REST APIs for frontend consumption.

---

## 🏗️ Architecture

The project follows a clean, simple architecture:

```
┌─────────────────────────────────────────────────────────┐
│                    External Sources                     │
│              (FICR, future sources...)                  │
└────────────────────┬────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────┐
│                   Sources Layer                         │
│  ┌──────────────────────────────────────────────────┐   │
│  │  sources/ficr/                                   │   │
│  │  ├── ficr.client.ts    (HTTP calls)              │   │
│  │  ├── ficr.parser.ts    (DTO → Schema)            │   │
│  │  └── ficr.service.ts   (orchestrates)            │   │
│  └──────────────────────────────────────────────────┘   │
└────────────────────┬────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────┐
│                 Ingestion Layer                         │
│  ┌──────────────────────────────────────────────────┐   │
│  │  ingestion.service.ts  (fetch + save)            │   │
│  │  ingestion.controller.ts  (HTTP endpoints)       │   │
│  └──────────────────────────────────────────────────┘   │
└────────────────────┬────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────┐
│                  Database Layer                         │
│  ┌──────────────────────────────────────────────────┐   │
│  │  database/                                       │   │
│  │  ├── schemas/     (Mongoose schemas)             │   │
│  │  └── repository/  (database operations)          │   │
│  └──────────────────────────────────────────────────┘   │
└────────────────────┬────────────────────────────────────┘
                     │
┌────────────────────▼────────────────────────────────────┐
│                    API Layer                            │
│  ┌──────────────────────────────────────────────────┐   │
│  │  api/                                            │   │
│  │  ├── athlete/    (query endpoints)               │   │
│  │  ├── race/       (query endpoints)               │   │
│  │  └── result/     (query endpoints)               │   │
│  └──────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

## 🔄 Data Flow

### 1. Fetch Data from Source
```typescript
FICR API → ficr.client.ts → Raw DTOs
```

### 2. Parse to Schemas
```typescript
Raw DTOs → ficr.parser.ts → Mongoose Schema Objects
```

### 3. Save to Database
```typescript
Schema Objects → ingestion.service.ts → repository.bulkUpsert() → MongoDB
```

### 4. Query via API
```typescript
Frontend → API Controller → Service → Repository → MongoDB → Response
```

---

## 📝 License

Private project - All rights reserved

---

## 🤝 Contributing

This is a private project. For questions or suggestions, please contact the maintainer.

---

**Built with ❤️ using NestJS**
