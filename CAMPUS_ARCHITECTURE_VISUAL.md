# 🎨 Campus System - Visual Architecture

## System Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                    CampusAxis Application                        │
│                                                                  │
│  ┌────────────────────────────────────────────────────────────┐ │
│  │                 App Layout (layout.tsx)                     │ │
│  │                                                             │ │
│  │  ┌──────────────────────────────────────────────────────┐  │ │
│  │  │           CampusProvider (Context)                    │  │ │
│  │  │                                                       │  │ │
│  │  │  State:                                               │  │ │
│  │  │  • selectedCampus (Lahore/ISB/Attock)                │  │ │
│  │  │  • selectedDepartment (CS/EE/SE)                     │  │ │
│  │  │  • selectedProgram (BS/MS/PhD)                       │  │ │
│  │  │                                                       │  │ │
│  │  │  Features:                                            │  │ │
│  │  │  ✓ localStorage persistence                          │  │ │
│  │  │  ✓ Auto-loads Lahore as default                      │  │ │
│  │  │  ✓ Cascading selections                              │  │ │
│  │  └───────────────────────────────────────────────────────┘  │ │
│  │                           │                                  │ │
│  │                           │ Provides Context                 │ │
│  │                           ▼                                  │ │
│  │  ┌───────────────────────────────────────────────────────┐  │ │
│  │  │                 Header Component                       │  │ │
│  │  │                                                        │  │ │
│  │  │  [Logo]  [Nav Items]  [🗺️ LHR • CS ▼]  [🔍] [👤]   │  │ │
│  │  │                        ↑                               │  │ │
│  │  │                  Campus Selector                       │  │ │
│  │  └────────────────────────────────────────────────────────┘  │ │
│  │                                                              │ │
│  │  ┌────────────────────────────────────────────────────────┐ │ │
│  │  │              Page Components                            │ │ │
│  │  │                                                         │ │ │
│  │  │  ┌────────────────────────────────────────────────┐   │ │ │
│  │  │  │  📅 Timetable Page                              │   │ │ │
│  │  │  │  ┌──────────────────────────────────────────┐  │   │ │ │
│  │  │  │  │ 💡 CampusBanner                          │  │   │ │ │
│  │  │  │  │ "Select your campus for filtering"       │  │   │ │ │
│  │  │  │  └──────────────────────────────────────────┘  │   │ │ │
│  │  │  │  → API Call: /api/timetable-docs              │   │ │ │
│  │  │  │    ?campus_id=xxx&department_id=yyy           │   │ │ │
│  │  │  └────────────────────────────────────────────────┘   │ │ │
│  │  │                                                         │ │ │
│  │  │  ┌────────────────────────────────────────────────┐   │ │ │
│  │  │  │  📄 Past Papers Page                            │   │ │ │
│  │  │  │  → useCampus() hook                             │   │ │ │
│  │  │  │  → Filters by selectedCampus & selectedDept     │   │ │ │
│  │  │  └────────────────────────────────────────────────┘   │ │ │
│  │  │                                                         │ │ │
│  │  │  ┌────────────────────────────────────────────────┐   │ │ │
│  │  │  │  👨‍🏫 Faculty Reviews                            │   │ │ │
│  │  │  │  → Shows faculty from selected campus           │   │ │ │
│  │  │  └────────────────────────────────────────────────┘   │ │ │
│  │  │                                                         │ │ │
│  │  │  [...+7 more pages all campus-aware]                   │ │ │
│  │  └─────────────────────────────────────────────────────┘  │ │
│  │                                                             │ │
│  │  ┌─────────────────────────────────────────────────────┐  │ │
│  │  │           Reminder Components                         │ │ │
│  │  │                                                       │ │ │
│  │  │  ┌─────────────────────────────────────────────┐    │ │ │
│  │  │  │ 🔔 CampusReminder (Floating)                 │    │ │ │
│  │  │  │ • Shows after 2 seconds                      │    │ │ │
│  │  │  │ • Dismissible per session                    │    │ │ │
│  │  │  │ • Smooth fade-in animation                   │    │ │ │
│  │  │  └─────────────────────────────────────────────┘    │ │ │
│  │  │                                                       │ │ │
│  │  │  ┌─────────────────────────────────────────────┐    │ │ │
│  │  │  │ 📍 CampusIndicator (Bottom-right)           │    │ │ │
│  │  │  │ • Pulsing animation                          │    │ │ │
│  │  │  │ • Only shows when incomplete                 │    │ │ │
│  │  │  └─────────────────────────────────────────────┘    │ │ │
│  │  └───────────────────────────────────────────────────┘  │ │
│  └──────────────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────────────┘
```

## Component Interaction Flow

```
┌──────────────┐
│    User      │
└──────┬───────┘
       │
       │ 1. Clicks campus selector
       ▼
┌─────────────────────────┐
│  CampusSelector Dialog  │
│                         │
│  ┌───────┐  ┌───────┐  │
│  │ LHR   │  │ ISB   │  │  ← Campus Cards
│  │ ✓     │  │       │  │
│  └───────┘  └───────┘  │
│                         │
│  Department Grid:       │
│  ┌────┐ ┌────┐ ┌────┐  │
│  │ CS │ │ SE │ │ EE │  │  ← Department Cards
│  │ ✓  │ │    │ │    │  │
│  └────┘ └────┘ └────┘  │
└─────────┬───────────────┘
          │
          │ 2. Selects LHR campus & CS dept
          ▼
┌─────────────────────────┐
│   CampusContext         │
│                         │
│  selectedCampus = {     │
│    id: "uuid-lhr"       │
│    code: "LHR"          │
│    name: "Lahore"       │
│  }                      │
│                         │
│  selectedDepartment = { │
│    id: "uuid-cs"        │
│    code: "CS"           │
│    name: "Comp Sci"     │
│  }                      │
└─────────┬───────────────┘
          │
          │ 3. Saves to localStorage
          │    Triggers re-renders
          ▼
┌─────────────────────────────────┐
│  All Pages with useCampus()     │
│                                 │
│  useEffect(() => {              │
│    loadData()                   │
│  }, [selectedCampus,            │
│      selectedDepartment])       │
│                                 │
│  ↓ Automatically reload data    │
└────────┬────────────────────────┘
         │
         │ 4. Build filtered API URLs
         ▼
┌─────────────────────────────────┐
│  API Endpoint                   │
│                                 │
│  GET /api/timetable-docs        │
│   ?campus_id=uuid-lhr           │
│   &department_id=uuid-cs        │
│                                 │
│  Database Query:                │
│  SELECT * FROM timetable_docs   │
│   WHERE campus_id = 'uuid-lhr'  │
│   AND department_id = 'uuid-cs' │
└────────┬────────────────────────┘
         │
         │ 5. Return filtered data
         ▼
┌─────────────────────────────────┐
│  Page Component                 │
│                                 │
│  - Displays timetables          │
│  - Shows "Showing results       │
│    for LHR • CS"                │
│  - Campus-specific content      │
└─────────────────────────────────┘
```

## State Management Flow

```
┌─────────────────────────────────────────────────────┐
│              Browser Storage Layer                   │
│                                                      │
│  localStorage.getItem("campus-selection")            │
│  ↓                                                   │
│  {                                                   │
│    campus: { id, name, code, ... },                 │
│    department: { id, name, code, ... },             │
│    program: { id, name, code, ... }                 │
│  }                                                   │
└──────────────────┬──────────────────────────────────┘
                   │
                   │ Hydrates on mount
                   ▼
┌─────────────────────────────────────────────────────┐
│              React Context State                     │
│                                                      │
│  const [selectedCampus, setSelectedCampus] = ...    │
│  const [selectedDepartment, setSelectedDept] = ...  │
│  const [selectedProgram, setSelectedProgram] = ...  │
│                                                      │
│  const [campuses, setCampuses] = ...   ← from API   │
│  const [departments, setDepts] = ...   ← from API   │
│  const [programs, setPrograms] = ...   ← from API   │
└──────────────────┬──────────────────────────────────┘
                   │
                   │ Consumed by
                   ▼
┌─────────────────────────────────────────────────────┐
│           Application Components                     │
│                                                      │
│  Header ←──────────┐                                │
│  Pages ←───────────┼─ useCampus() hook              │
│  Banners ←─────────┘                                │
│  Reminders                                           │
└─────────────────────────────────────────────────────┘
```

## Database Schema

```
┌─────────────────────┐
│     campuses        │
│─────────────────────│
│ id (UUID PK)        │
│ name (TEXT)         │  "Lahore"
│ code (TEXT UNIQUE)  │  "LHR"
│ city (TEXT)         │  "Lahore"
│ full_name (TEXT)    │  "COMSATS ... Lahore Campus"
│ is_default (BOOL)   │  true
└─────────┬───────────┘
          │
          │ 1:N
          ▼
┌─────────────────────┐
│    departments      │
│─────────────────────│
│ id (UUID PK)        │
│ campus_id (FK)      │ ──→ campuses.id
│ name (TEXT)         │  "Computer Science"
│ code (TEXT)         │  "CS"
│ full_name (TEXT)    │  "Dept of Computer Science"
└─────────┬───────────┘
          │
          │ 1:N
          ▼
┌─────────────────────┐
│     programs        │
│─────────────────────│
│ id (UUID PK)        │
│ department_id (FK)  │ ──→ departments.id
│ name (TEXT)         │  "Computer Science"
│ code (TEXT)         │  "BSCS"
│ degree_type (TEXT)  │  "BS", "MS", "PhD"
└─────────────────────┘

┌─────────────────────────────────────┐
│      Content Tables                 │
│     (timetable_docs, past_papers,   │
│      faculty, lost_found, etc.)     │
│─────────────────────────────────────│
│ id (UUID PK)                        │
│ campus_id (FK)      │ ──→ campuses.id
│ department_id (FK)  │ ──→ departments.id
│ ... other fields                    │
└─────────────────────────────────────┘
```

## UI Component Hierarchy

```
App Layout
│
├─ Header
│  ├─ Logo
│  ├─ Navigation
│  └─ CampusSelector ←─────┐
│     │                    │
│     └─ Dialog            │
│        ├─ Campus Cards   │  User clicks here
│        └─ Dept Cards     │  to change selection
│                          │
├─ Page Content            │
│  │                       │
│  ├─ CampusBanner ←───────┤
│  │  └─ Quick selector    │  Inline reminder
│  │                       │
│  └─ Feature Content      │
│     └─ Filtered by       │
│        campus selection  │
│                          │
└─ Floating Components     │
   │                       │
   ├─ CampusReminder ←─────┤
   │  └─ Popup after 2s    │  Gentle nudge
   │                       │
   └─ CampusIndicator ←────┘
      └─ Pulsing button       Bottom-right
```

## API Request Flow

```
Component
   │
   │ const { selectedCampus, selectedDepartment } = useCampus()
   │
   ├─ Build query params
   │  const params = new URLSearchParams()
   │  if (selectedCampus?.id) params.append('campus_id', selectedCampus.id)
   │  if (selectedDepartment?.id) params.append('department_id', selectedDepartment.id)
   │
   └─ Fetch with params
      fetch(`/api/timetable-docs?${params}`)
         │
         ▼
      API Route
         │
         ├─ Extract params
         │  const campusId = searchParams.get('campus_id')
         │  const departmentId = searchParams.get('department_id')
         │
         ├─ Build query
         │  let query = supabase.from('timetable_docs').select('*')
         │  if (campusId) query = query.eq('campus_id', campusId)
         │  if (departmentId) query = query.eq('department_id', departmentId)
         │
         └─ Execute & return
            const { data } = await query
            return NextResponse.json({ data })
```

## User Journey

```
┌──────────────────────────────────────────────────────┐
│ 1. User visits site (first time)                     │
│    → Context auto-loads Lahore as default            │
│    → Shows floating reminder after 2s                │
└──────────────────────┬───────────────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────────────┐
│ 2. User sees reminder                                │
│    ┌────────────────────────────────────────┐       │
│    │ 💡 Select Your Campus                  │       │
│    │ Get personalized content               │       │
│    │ [Select Campus] [Maybe Later]          │       │
│    └────────────────────────────────────────┘       │
└──────────────────────┬───────────────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────────────┐
│ 3. User clicks "Select Campus"                       │
│    → Dialog opens                                    │
│    → Shows all campuses (LHR, ISB, ATK, etc.)       │
└──────────────────────┬───────────────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────────────┐
│ 4. User selects "Lahore"                             │
│    → Departments load for Lahore                     │
│    → Shows CS, SE, EE, etc.                          │
└──────────────────────┬───────────────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────────────┐
│ 5. User selects "CS" department                      │
│    → Context updates                                 │
│    → Saves to localStorage                           │
│    → Dialog closes                                   │
└──────────────────────┬───────────────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────────────┐
│ 6. All pages automatically reload                    │
│    → Timetables for LHR-CS only                      │
│    → Past papers for LHR-CS only                     │
│    → Faculty from LHR-CS only                        │
│    → etc.                                            │
└──────────────────────┬───────────────────────────────┘
                       │
                       ▼
┌──────────────────────────────────────────────────────┐
│ 7. User navigates to Timetable page                  │
│    → Sees banner: "Showing timetables for LHR • CS"  │
│    → Content is filtered                             │
│    → Can change selection anytime from header        │
└──────────────────────────────────────────────────────┘
```

## Performance Optimizations

```
┌──────────────────────────────────────────────────────┐
│ 1. Context Level                                     │
│    • useCallback for functions                       │
│    • useMemo for derived data                        │
│    • Debounced API calls (if rapid changes)          │
└──────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────┐
│ 2. Storage Level                                     │
│    • localStorage for persistence                    │
│    • Reduces API calls on mount                      │
│    • Session-based dismissals                        │
└──────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────┐
│ 3. Database Level                                    │
│    • Indexes on campus_id and department_id          │
│    • Efficient WHERE clauses                         │
│    • Only fetch needed fields                        │
└──────────────────────────────────────────────────────┘

┌──────────────────────────────────────────────────────┐
│ 4. Component Level                                   │
│    • Lazy load departments (only when campus select) │
│    • Conditional rendering of reminders              │
│    • Memoized filter calculations                    │
└──────────────────────────────────────────────────────┘
```

---

This visual guide shows the complete architecture of the multi-campus system!
