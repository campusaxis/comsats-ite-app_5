# Visual Architecture Diagrams

## 📐 System Architecture Overview

```
┌─────────────────────────────────────────────────────────────────────┐
│                          USER INTERFACE                             │
│                     (Next.js App Router)                            │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌────────────┐   │
│  │   Pages    │  │  Layouts   │  │   Routes   │  │    API     │   │
│  │  (app/)    │  │  (app/)    │  │  (app/)    │  │ (app/api/) │   │
│  └────────────┘  └────────────┘  └────────────┘  └────────────┘   │
└────────────┬────────────────────────────────────────────────────────┘
             │
             │ Uses
             ↓
┌─────────────────────────────────────────────────────────────────────┐
│                        PRESENTATION LAYER                           │
│                     (Shared Components)                             │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌────────────┐   │
│  │     UI     │  │   Layout   │  │   Common   │  │ Providers  │   │
│  │ Components │  │ Components │  │ Components │  │  (Context) │   │
│  └────────────┘  └────────────┘  └────────────┘  └────────────┘   │
└────────────┬────────────────────────────────────────────────────────┘
             │
             │ Uses
             ↓
┌─────────────────────────────────────────────────────────────────────┐
│                        FEATURE MODULES                              │
│                     (Vertical Slices)                               │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌────────────┐   │
│  │    Auth    │  │   Admin    │  │ Community  │  │ Timetable  │   │
│  │  ┌──────┐  │  │  ┌──────┐  │  │  ┌──────┐  │  │  ┌──────┐  │   │
│  │  │ API  │  │  │  │ API  │  │  │  │ API  │  │  │  │ API  │  │   │
│  │  ├──────┤  │  │  ├──────┤  │  │  ├──────┤  │  │  ├──────┤  │   │
│  │  │ UI   │  │  │  │ UI   │  │  │  │ UI   │  │  │  │ UI   │  │   │
│  │  ├──────┤  │  │  ├──────┤  │  │  ├──────┤  │  │  ├──────┤  │   │
│  │  │Hooks │  │  │  │Hooks │  │  │  │Hooks │  │  │  │Hooks │  │   │
│  │  └──────┘  │  │  └──────┘  │  │  └──────┘  │  │  └──────┘  │   │
│  └────────────┘  └────────────┘  └────────────┘  └────────────┘   │
└────────────┬────────────────────────────────────────────────────────┘
             │
             │ Uses
             ↓
┌─────────────────────────────────────────────────────────────────────┐
│                      APPLICATION LAYER                              │
│                       (Use Cases)                                   │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌────────────┐   │
│  │   Login    │  │  Register  │  │   Create   │  │   Update   │   │
│  │  UseCase   │  │  UseCase   │  │  UseCase   │  │  UseCase   │   │
│  └────────────┘  └────────────┘  └────────────┘  └────────────┘   │
└────────────┬────────────────────────────────────────────────────────┘
             │
             │ Uses
             ↓
┌─────────────────────────────────────────────────────────────────────┐
│                        DOMAIN LAYER                                 │
│                   (Business Logic)                                  │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌────────────┐   │
│  │    User    │  │ Timetable  │  │ Community  │  │  Faculty   │   │
│  │   Entity   │  │   Entity   │  │   Entity   │  │   Entity   │   │
│  ├────────────┤  ├────────────┤  ├────────────┤  ├────────────┤   │
│  │   Types    │  │   Types    │  │   Types    │  │   Types    │   │
│  ├────────────┤  ├────────────┤  ├────────────┤  ├────────────┤   │
│  │  Schemas   │  │  Schemas   │  │  Schemas   │  │  Schemas   │   │
│  └────────────┘  └────────────┘  └────────────┘  └────────────┘   │
│                                                                     │
│  ┌─────────────────────────────────────────────────────────────┐   │
│  │          Repository Interfaces (Abstractions)               │   │
│  └─────────────────────────────────────────────────────────────┘   │
└────────────┬────────────────────────────────────────────────────────┘
             │
             │ Implements
             ↓
┌─────────────────────────────────────────────────────────────────────┐
│                    INFRASTRUCTURE LAYER                             │
│                  (External Dependencies)                            │
│  ┌────────────┐  ┌────────────┐  ┌────────────┐  ┌────────────┐   │
│  │  MongoDB   │  │  Supabase  │  │   Email    │  │  Storage   │   │
│  │ Repository │  │    Auth    │  │  Service   │  │  Service   │   │
│  │    Impl    │  │  Service   │  │  (Resend)  │  │            │   │
│  └────────────┘  └────────────┘  └────────────┘  └────────────┘   │
└─────────────────────────────────────────────────────────────────────┘
```

## 🔄 Data Flow Diagram

### Read Flow (Query)

```
┌─────────┐
│  User   │
└────┬────┘
     │ 1. Request
     ↓
┌──────────────┐
│   Next.js    │
│     Page     │
└──────┬───────┘
       │ 2. Calls
       ↓
┌──────────────┐
│   Feature    │
│  Component   │
└──────┬───────┘
       │ 3. Uses
       ↓
┌──────────────┐
│   Custom     │
│     Hook     │
└──────┬───────┘
       │ 4. Executes
       ↓
┌──────────────┐
│   Use Case   │
│ (Business    │
│   Logic)     │
└──────┬───────┘
       │ 5. Queries
       ↓
┌──────────────┐
│  Repository  │
│  Interface   │
└──────┬───────┘
       │ 6. Implements
       ↓
┌──────────────┐
│  Repository  │
│     Impl     │
└──────┬───────┘
       │ 7. Queries
       ↓
┌──────────────┐
│   Database   │
└──────┬───────┘
       │ 8. Returns Data
       ↓
     (Back up the chain to User)
```

### Write Flow (Command)

```
┌─────────┐
│  User   │
└────┬────┘
     │ 1. Action
     ↓
┌──────────────┐
│     Form     │
│  Component   │
└──────┬───────┘
       │ 2. Submit
       ↓
┌──────────────┐
│  API Route   │
│   Handler    │
└──────┬───────┘
       │ 3. Validates & Calls
       ↓
┌──────────────┐
│   Use Case   │
│  (Business   │
│   Logic +    │
│  Validation) │
└──────┬───────┘
       │ 4. Commands
       ↓
┌──────────────┐
│  Repository  │
│  Interface   │
└──────┬───────┘
       │ 5. Implements
       ↓
┌──────────────┐
│  Repository  │
│     Impl     │
└──────┬───────┘
       │ 6. Writes
       ↓
┌──────────────┐
│   Database   │
└──────┬───────┘
       │ 7. Success/Error
       ↓
     (Back up the chain to User)
```

## 🗂️ Directory Tree Visual

```
comsats-ite-app_5/
│
├── 📱 app/                          # Next.js App Router
│   ├── (auth)/                      # Route groups
│   ├── (dashboard)/
│   ├── api/                         # API routes
│   ├── layout.tsx
│   └── page.tsx
│
├── 🎨 components/                   # Shared UI
│   ├── ui/                          # Base components
│   │   ├── button.tsx
│   │   ├── input.tsx
│   │   └── ...
│   ├── layout/                      # Layout components
│   │   ├── header/
│   │   ├── footer/
│   │   └── sidebar/
│   ├── common/                      # Common components
│   └── providers/                   # Context providers
│
├── 🧠 src/                          # Source code (NEW)
│   │
│   ├── 💎 core/                     # Core business logic
│   │   ├── domain/                  # Domain models
│   │   │   ├── user/
│   │   │   │   ├── user.entity.ts
│   │   │   │   ├── user.types.ts
│   │   │   │   └── user.schema.ts
│   │   │   ├── timetable/
│   │   │   ├── community/
│   │   │   └── faculty/
│   │   │
│   │   ├── use-cases/               # Business logic
│   │   │   ├── auth/
│   │   │   │   ├── login.use-case.ts
│   │   │   │   └── register.use-case.ts
│   │   │   ├── admin/
│   │   │   └── community/
│   │   │
│   │   └── repositories/            # Data access interfaces
│   │       ├── user.repository.ts
│   │       └── ...
│   │
│   ├── 🔌 infrastructure/           # External services
│   │   ├── database/
│   │   │   ├── mongodb/
│   │   │   │   ├── connection.ts
│   │   │   │   ├── models/
│   │   │   │   └── repositories/
│   │   │   └── supabase/
│   │   ├── email/
│   │   │   └── resend/
│   │   └── storage/
│   │
│   ├── 🎯 features/                 # Feature modules
│   │   ├── auth/
│   │   │   ├── api/
│   │   │   ├── components/
│   │   │   ├── hooks/
│   │   │   ├── utils/
│   │   │   ├── types/
│   │   │   └── index.ts
│   │   ├── admin/
│   │   ├── community/
│   │   ├── timetable/
│   │   └── ...
│   │
│   ├── 🛠️ lib/                      # Shared utilities
│   │   ├── utils/                   # Pure functions
│   │   ├── helpers/                 # Helper functions
│   │   ├── constants/               # Constants
│   │   └── types/                   # Shared types
│   │
│   └── ⚙️ config/                   # Configuration
│       ├── app.config.ts
│       ├── seo.config.ts
│       └── database.config.ts
│
├── 🧪 tests/                        # Test files
│   ├── unit/
│   ├── integration/
│   ├── e2e/
│   ├── fixtures/
│   └── mocks/
│
├── 📚 docs/                         # Documentation
│   ├── architecture/
│   ├── guides/
│   ├── api/
│   └── features/
│
├── 🪝 hooks/                        # Shared hooks
│
├── 🎭 contexts/                     # React contexts
│
├── 📦 public/                       # Static assets
│
├── 🔧 scripts/                      # Build scripts
│
└── 📝 Configuration files
    ├── package.json
    ├── tsconfig.json
    ├── next.config.mjs
    └── ...
```

## 🔗 Dependency Graph

```
┌─────────────────────────────────────────────────────────┐
│                    Dependency Rules                     │
│                                                         │
│  ✅ ALLOWED                    ❌ FORBIDDEN             │
│  ─────────────                 ────────────             │
│  Infrastructure → Core         Core → Infrastructure    │
│  Features → Core               Core → Features          │
│  Features → Infrastructure     Infrastructure → Features│
│  App → Features                                         │
│  Components → Hooks                                     │
│  Hooks → Contexts                                       │
└─────────────────────────────────────────────────────────┘

Visual Dependency Direction:

    ┌───────────┐
    │    App    │ ─────┐
    └───────────┘      │
                       ↓
    ┌───────────┐   ┌───────────┐
    │Components │   │ Features  │ ─────┐
    └───────────┘   └───────────┘      │
         │                              │
         ↓                              ↓
    ┌───────────┐              ┌───────────────┐
    │   Hooks   │              │Infrastructure │ ───┐
    └───────────┘              └───────────────┘    │
                                                     │
                                                     ↓
                                              ┌───────────┐
                                              │   Core    │
                                              │ (No Deps) │
                                              └───────────┘
```

## 🎭 Feature Module Structure

```
src/features/[feature-name]/
│
├── 📁 api/                      # API route handlers
│   ├── handlers.ts
│   └── validators.ts
│
├── 🎨 components/               # UI components
│   ├── FeatureComponent.tsx
│   ├── FeatureForm.tsx
│   └── FeatureList.tsx
│
├── 🪝 hooks/                    # Custom hooks
│   ├── use-feature.ts
│   ├── use-feature-query.ts
│   └── use-feature-mutation.ts
│
├── 🛠️ utils/                    # Utilities
│   ├── feature.utils.ts
│   └── feature.helpers.ts
│
├── 📝 types/                    # Type definitions
│   ├── feature.types.ts
│   └── feature.interfaces.ts
│
└── 📄 index.ts                  # Public API
    │
    └── Exports:
        ├── Components (selected)
        ├── Hooks (selected)
        └── Types (selected)
        
        Internal utilities NOT exported
        (encapsulation)
```

## 🔄 Import Pattern Examples

### ✅ Good Import Patterns

```typescript
// External dependencies first
import React from 'react'
import { useRouter } from 'next/navigation'

// Absolute imports using aliases (organized by layer)
import { User } from '@/core/domain/user'
import { LoginUseCase } from '@/core/use-cases/auth'
import { createClient } from '@/infrastructure/database/supabase'
import { LoginForm } from '@/features/auth/components'
import { useAuth } from '@/features/auth/hooks'
import { formatDate } from '@/lib/utils/date.utils'
import { Button } from '@/components/ui/button'

// Relative imports (only for co-located files)
import { helper } from './helper'
import type { Props } from './types'
```

### ❌ Bad Import Patterns

```typescript
// ❌ Deep imports from feature internals
import { LoginForm } from '@/features/auth/components/login-form'

// ❌ Circular dependencies
import { UserComponent } from '@/features/user'
// ... in user feature importing from auth

// ❌ Core importing from infrastructure
// In src/core/use-cases/auth/login.use-case.ts:
import { MongoUserRepository } from '@/infrastructure/database/mongodb'

// ❌ Mixed relative and absolute imports randomly
import Button from '../../../components/ui/button'
import { User } from '@/core/domain/user'
import Card from '../../shared/card'
```

## 🧩 Component Communication

```
┌─────────────────────────────────────────────────────────┐
│              Component Communication                    │
└─────────────────────────────────────────────────────────┘

Parent Component
       │
       ├─── Props ────→ Child Component
       │
       ├─── Context ──→ Deep Child
       │
       └─── Events ←── Child (callbacks)


Feature Module A              Feature Module B
       │                             │
       └─── Shared State ←──→ ───────┘
            (Context/Store)


Component                    Backend
       │                        │
       ├── API Call ──────────→ │
       │                        │
       └── Response ←────────── │
```

## 📊 Testing Pyramid

```
                    ╱╲
                   ╱  ╲
                  ╱ E2E╲              ← Few, slow, expensive
                 ╱______╲                 Critical user flows
                ╱        ╲
               ╱Integration╲          ← Some, medium speed
              ╱____________╲             API routes, DB ops
             ╱              ╲
            ╱  Unit Tests    ╲        ← Many, fast, cheap
           ╱__________________╲          Pure functions, logic

tests/
├── e2e/                 # Top of pyramid (few tests)
│   └── auth.spec.ts
│
├── integration/         # Middle (moderate tests)
│   └── api/
│       └── auth.test.ts
│
└── unit/               # Base (many tests)
    ├── core/
    ├── features/
    └── lib/
```

## 🎯 Development Workflow

```
┌──────────────┐
│  New Task    │
└──────┬───────┘
       │
       ↓
┌──────────────────────────────────────┐
│ 1. Create/Find Feature Module        │
└──────┬───────────────────────────────┘
       │
       ↓
┌──────────────────────────────────────┐
│ 2. Define Domain Model (if needed)   │
│    src/core/domain/[entity]/         │
└──────┬───────────────────────────────┘
       │
       ↓
┌──────────────────────────────────────┐
│ 3. Create Use Case (business logic)  │
│    src/core/use-cases/[feature]/     │
└──────┬───────────────────────────────┘
       │
       ↓
┌──────────────────────────────────────┐
│ 4. Implement UI Components           │
│    src/features/[feature]/components/│
└──────┬───────────────────────────────┘
       │
       ↓
┌──────────────────────────────────────┐
│ 5. Add Custom Hooks                  │
│    src/features/[feature]/hooks/     │
└──────┬───────────────────────────────┘
       │
       ↓
┌──────────────────────────────────────┐
│ 6. Create API Routes (if needed)     │
│    app/api/[feature]/ or             │
│    src/features/[feature]/api/       │
└──────┬───────────────────────────────┘
       │
       ↓
┌──────────────────────────────────────┐
│ 7. Write Tests                       │
│    tests/unit/features/[feature]/    │
└──────┬───────────────────────────────┘
       │
       ↓
┌──────────────────────────────────────┐
│ 8. Update Feature's index.ts         │
│    (Public API)                      │
└──────┬───────────────────────────────┘
       │
       ↓
┌──────────────┐
│   Complete   │
└──────────────┘
```

---

**Use these diagrams as visual reference while developing!** 📐

Save this file and refer to it when you need to understand:
- System architecture
- Data flow
- Directory organization
- Dependency rules
- Import patterns
- Development workflow
