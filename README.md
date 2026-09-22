# Kando Onboarding Wizard Project

**Purpose**: Design and plan the interactive setup wizard with pre-built templates for timekeeping and payroll configuration  
**Status**: Planning & Design Phase  
**Created**: September 2026  
**Target Audience**: Product managers, engineers, UX designers

---

## Project Overview

The **Kando Onboarding Wizard** is a guided, step-by-step setup experience that enables new organizations to:
- Configure timekeeping in minutes (vs hours)
- Set up payroll with pre-built templates (vs manual configuration)
- Complete full implementation in 2-3 days (vs 4-6 weeks)
- Activate payroll processing on day 1 with working configurations

### Key Improvements
- ✅ Reduce implementation time from 4-6 weeks to 2-3 days
- ✅ Pre-validate configurations to prevent errors
- ✅ Provide better first-time user experience
- ✅ Reduce support burden for configuration questions
- ✅ Enable faster customer activation and adoption

---

## Contents

### ⭐ NEW: **WIZARD_DEPENDENCY_ORDER.md** 
**Complete dependency analysis & proper sequencing** (START HERE for technical teams)

**Sections:**
- Verified ordering (based on backend model analysis)
- All 10 wizards with dependencies
- What blocks what (FK relationships)
- Can X be skipped? (blocker vs optional)
- Implementation checklist per wizard
- Database model relationships

**Read Time**: 20-30 minutes (technical reference)

**Use When**: Planning implementation, verifying order, checking if wizard X can start before Y

---

### 1. **MVP_SIMPLE_WIZARD_INVENTORY.md** (UPDATED)
**MVP scope with verified ordering**

**Sections:**
- Recommended phases (now with verified ordering)
- All 10 wizards with proper sequencing
- What each wizard does and why
- Dependencies clearly marked (blocker vs independent)
- Implementation timeline

**Read Time**: 15-20 minutes (executive summary)

---

### 2. **ONBOARDING_WIZARD_WITH_TEMPLATES.md** (Reference)
**Original comprehensive feature specification** for the wizard

**Sections:**
- Overview & business value
- Complete wizard flow (8 steps)
- Step 4: Timekeeping Configuration with 5 templates
- Step 5: Payroll Configuration with 6 templates
- What each template includes (pre-configured pay types, pay groups, formulas)
- Customization options for advanced users
- Visual design mockups
- Benefits & success metrics
- Rollout plan (3 phases)

**Read Time**: 30-40 minutes (comprehensive reference)

---

## Quick Navigation (By Role)

### For Product Managers
1. Read: `MVP_SIMPLE_WIZARD_INVENTORY.md` → Phase overview + sequencing
2. Understand: Which wizards are blockers vs optional
3. Plan: Timeline (5+ weeks total, 2 weeks MVP, 3 weeks for full)
4. Reference: `WIZARD_DEPENDENCY_ORDER.md` → Why things are ordered this way

### For UX/Product Designers
1. Start: `WIZARD_DEPENDENCY_ORDER.md` → Section "Recommended Wizard Flow UI" (visual flow)
2. Read: `MVP_SIMPLE_WIZARD_INVENTORY.md` → All 10 wizards with UI requirements
3. Design in order: Periods → Holidays → Leave Types → Shifts (Phase 1)
4. Reference: Existing views in `/source-code/kando-frontend/src/views/` for patterns

### For Engineers
1. Start: `WIZARD_DEPENDENCY_ORDER.md` → Database model relationships section
2. Check: "Implementation Checklist" for each wizard
3. Verify: Shift table migration exists (may need to create)
4. Reference: Backend models in `/source-code/kando-backend/app/Models/Settings/`
5. Implement in order: #1 → #2 → #3 → #4 (parallelizable from #5 onward)

### For Tech Lead / Architect
1. Read: `WIZARD_DEPENDENCY_ORDER.md` → Complete dependency map
2. Review: Foreign key constraints + cascade behavior
3. Plan: Database migrations needed (especially Shift table)
4. Verify: Government rate lookups pre-seeded (SSS, PhilHealth, BIR, PagIBIG)
5. Timeline: 5+ weeks (Phase 1: 2 weeks strict order, Phase 2: 1-2 parallel, Phase 3: 2-3)

### For HR/Compliance
1. Review: `WIZARD_DEPENDENCY_ORDER.md` → Payroll & Leave sections (#9, #10, #5, #6)
2. Verify: Government deduction rates (SSS brackets, PhilHealth 3%, PagIBIG 1/2%)
3. Check: Philippines leave types pre-populated (Vacation, Sick, Bereavement, etc.)
4. Confirm: BIR withholding tax rates current

---

## Timekeeping Templates Available

| Template | Best For | Shift Type |
|----------|----------|-----------|
| **Standard Office Hours** | Corporate, BPO | 8 AM - 5 PM (single shift) |
| **Manufacturing** | Factory operations | 3 shifts, 24-hour operations |
| **Retail & Hospitality** | Stores, restaurants | Flexible 4-8 hour shifts |
| **Healthcare** | Hospitals, clinics | 12-hour shifts with rotation |
| **Hybrid/Remote** | Tech, startups | Flexible, no fixed hours |

---

## Payroll Templates Available

| Template | Best For | Pay Structure |
|----------|----------|---------------|
| **Fixed Salary + Benefits** | Corporate employees | Fixed + allowances |
| **Hourly Wage + Overtime** | Retail, F&B workers | Hourly rate + OT premium |
| **Manufacturing & Shift Premiums** | Factory workers | Base + shift differentials |
| **Commission-Based** | Sales teams | Base + commission + bonus |
| **Daily Rate + Incentives** | Construction, logistics | Daily rate + incentives |
| **Flexible Remuneration** | Tech companies | Base + flexible allowances |

---

## What Each Payroll Template Includes

### Pre-Configured Pay Types
- ✅ Salary/Hourly wage (with formulas)
- ✅ Allowances (DA, transportation, communication, meals)
- ✅ Bonuses/incentives (performance, attendance)
- ✅ Deductions (SSS, PhilHealth, PagIBIG, tax withholding)
- ✅ Shift differentials/overtime premiums

### Pre-Configured Pay Groups
- ✅ "Regular Employees" pay group
- ✅ "Probationary" pay group
- ✅ Industry-specific groups (e.g., "Night Shift", "Hourly Workers")
- ✅ Ready-to-use formulas
- ✅ Auto-calculated deductions

### Compliance Built-In
- ✅ Philippines tax rates (BIR withholding)
- ✅ SSS contribution rates by salary bracket
- ✅ PhilHealth (3% employee, 3% employer)
- ✅ PagIBIG (1% employee, 2% employer)
- ✅ 13th month pay tracking

### Sample Calculations
Each template includes a **payroll preview** showing:
- Gross earnings (salary + allowances + bonuses)
- Total deductions (tax + contributions)
- Net pay calculation

---

## User Flow

```
START SETUP
    ↓
1. Organization Profile (company name, industry, size)
    ↓
2. Organizational Structure (departments, cost centers)
    ↓
3. Employee Setup (bulk import template)
    ↓
4. TIMEKEEPING CONFIGURATION [SELECT TEMPLATE]
   └─ Preview shift configuration
    ↓
5. PAYROLL CONFIGURATION [SELECT TEMPLATE]
   ├─ Preview pay types
   ├─ Preview pay groups
   ├─ Customize amounts (optional)
   └─ Preview sample payroll calculation
    ↓
6. Leave Policies (leave types, accrual rules)
    ↓
7. Access & Permissions (roles assignment)
    ↓
8. Integration Setup (email, notifications)
    ↓
POST-SETUP CHECKLIST
    ↓
READY TO PROCESS FIRST PAYROLL
```

---

## Key Features

### 🎯 Template Selection
Users see 5-6 templates per category with:
- Clear industry/company type indicator
- What's included summary
- "Learn More" link for details

### ✏️ Customization
After template selection, users can:
- Edit allowance amounts
- Add/remove pay types
- Create additional pay groups
- Modify deduction percentages
- View and edit formulas

### 👁️ Preview
Before confirming, users see:
- Sample payroll calculation (with real numbers)
- Pay slip preview
- Deduction breakdown
- Gross to net calculation

### ⚡ Smart Defaults
- Auto-detects company size from employee count
- Recommends templates based on industry
- Pre-fills government requirements by country
- Validates all configurations before saving

---

## Success Metrics

| Metric | Target | Status |
|--------|--------|--------|
| Implementation time | 2-3 days (vs 4-6 weeks) | TBD |
| Configuration errors | < 5% | TBD |
| Wizard completion rate | > 85% | TBD |
| First payroll success | > 90% | TBD |
| Support tickets reduction | -30% | TBD |
| Customer activation | +40% within week 1 | TBD |

---

## Rollout Plan

### Phase 1: MVP (Week 1-2)
- Timekeeping templates (3-4 options)
- Payroll templates (3 options)
- Basic customization

### Phase 2: Enhanced (Week 3-4)
- Expand to 6 payroll templates
- Advanced formula builder
- Employee bulk assignment

### Phase 3: Premium (Week 5-6)
- AI-powered recommendations
- Industry-specific deep dives
- Configuration cloning

---

## 📋 Quick Navigation (All Files)

### 1. **START HERE: MVP Scope & Wireframes** 
**File**: `MVP_WIREFRAME_PRIORITIES.md` (52 pages, comprehensive)
- **Read if**: You want the complete MVP spec with all 6 wireframes detailed
- **Time**: 30-40 minutes
- **Includes**: Wireframe specs, data dependencies, timeline, success criteria, open questions
- **Best for**: Engineers, designers, detailed planning

### 2. **QUICK OVERVIEW: Discovery Analysis Summary**
**File**: `DISCOVERY_ANALYSIS_SUMMARY.md` (2-3 pages, strategic)
- **Read if**: You want the reasoning behind MVP decisions
- **Time**: 10-15 minutes
- **Includes**: Discovery responses analyzed, why we chose 1 timekeeping + 2 payroll templates, unknowns answered with recommendations
- **Best for**: Product managers, decision-makers, stakeholders

### 3. **VISUAL REFERENCE: Interactive MVP Summary** 
**Link**: [Claude Artifact](https://claude.ai/code/artifact/11bbfa9a-2077-4bc5-8965-aeff549a9bd7) (1 page, visual)
- **Read if**: You prefer a visual overview with cards and timeline
- **Time**: 5-10 minutes
- **Includes**: Scope summary, 6 wireframes at a glance, 2-week timeline, success metrics
- **Best for**: Quick stakeholder briefings, executive summaries

### 4. **DETAILED SPEC: Onboarding Wizard with Templates**
**File**: `ONBOARDING_WIZARD_WITH_TEMPLATES.md` (original spec, 854 lines)
- **Read if**: You want the full feature specification with all template details
- **Time**: 60 minutes
- **Includes**: All 8 wizard steps, all 6 payroll template definitions (before MVP decisions)
- **Best for**: Reference, Phase 2 planning

### 5. **DISCOVERY QUESTIONS: Raw Responses**
**File**: `DISCOVERY_QUESTIONS.md` (145 lines)
- **Read if**: You want to see original discovery questions and team answers
- **Time**: 10 minutes
- **Includes**: 23 discovery questions, answers from team, open questions
- **Best for**: Understanding context, filling in gaps

---

## 📊 Document Reading Order by Role (UPDATED)

### **For Product Managers** (20 minutes total)
1. Start: `MVP_SIMPLE_WIZARD_INVENTORY.md` → MVP Phases + Sequencing (5 min)
2. Understand: `WIZARD_DEPENDENCY_ORDER.md` → Section "Critical Dependencies Summary" (5 min)
3. Plan: Timeline from "Migration Timeline" table (5 min)
4. Reference: `DISCOVERY_ANALYSIS_SUMMARY.md` → Business context (optional, 5 min)

### **For UX/Product Designers** (90 minutes total)
1. Start: `MVP_SIMPLE_WIZARD_INVENTORY.md` → Phases 1-3 overview (10 min)
2. Sequence: `WIZARD_DEPENDENCY_ORDER.md` → Section "Recommended Wizard Flow UI" (5 min)
3. Deep dive: Each wizard section in `WIZARD_DEPENDENCY_ORDER.md` (40 min)
   - Read: Payroll Periods, Holiday Calendar, Leave Types, Shift Configuration
4. Design: Create wireframes in order (Phase 1 first: 1→2→3→4)
5. Reference: Check existing UI patterns in `/src/views/` (30 min)

### **For Engineers** (60 minutes total)
1. Start: `WIZARD_DEPENDENCY_ORDER.md` → Full document (40 min)
   - Focus: Database models, FK relationships, blockers
2. Verify: Database schema check (10 min)
   - [ ] Shift table migration exists?
   - [ ] Lookup tables pre-seeded?
3. Reference: Backend models in `/app/Models/Settings/` (10 min)
4. Planning: Create implementation checklist per wizard

### **For Tech Lead / Architect** (90 minutes total)
1. Deep review: `WIZARD_DEPENDENCY_ORDER.md` → Complete analysis (40 min)
2. Verify: Database state (20 min)
   - [ ] All migrations in place
   - [ ] Foreign key constraints
   - [ ] Cascade behavior correct
   - [ ] Lookup tables populated
3. Timeline: Map out 5-week implementation plan (20 min)
4. Risk check: Shift table, government rates, employee setup (10 min)

### **For Leadership/Exec** (10 minutes total)
1. Quick summary: `MVP_SIMPLE_WIZARD_INVENTORY.md` → Phases + Timeline table (5 min)
2. Impact: What org can do after each phase (5 min)

---

## 🎯 MVP Summary (TL;DR)

| Aspect | Details |
|--------|---------|
| **Timekeeping** | 1 template: Standard Office Hours (8 AM–5 PM) |
| **Payroll** | 2 templates: Fixed Salary + Benefits, Hourly Wage + Overtime |
| **Customization** | Edit amounts only (no formula builder in MVP) |
| **Scope** | 6 wireframes over 2 weeks (1 designer + 1–2 engineers) |
| **Business Goal** | Reduce onboarding from 2 weeks to 3–4 days |
| **Success Metric** | > 85% wizard completion, < 5% post-setup errors |
| **Not Included** | Employee import, leave policies, departments, advanced formulas |

---

## 🚀 Next Steps (For Team)

**Before Kickoff Meeting:**
- [ ] Read appropriate section above for your role
- [ ] Answer the 3 critical questions in `DISCOVERY_ANALYSIS_SUMMARY.md` (Section "Open Questions")
- [ ] Confirm you agree with MVP scope

**At Kickoff Meeting:**
- [ ] Review all 6 wireframes together
- [ ] Confirm data dependencies and tech stack
- [ ] Assign tasks: Design (Wireframes 1-3 first), Backend (state management), Frontend (structure)
- [ ] Schedule design review before engineering starts building

**Week 1:**
- Design: High-fidelity mockups (Wireframes 1-4)
- Backend: DB schema, Pinia store setup, template models
- Frontend: Component library, form inputs, routing structure

**Week 2:**
- Design: Polish, iterate on feedback
- Frontend: Implement all 6 wireframes, connect to backend
- Integration: End-to-end testing, mobile responsive, accessibility

---

## 🔗 Related Documents

- **Strategic Features Document**: `/Users/ericmagto/Projects/kando/research/kando-future-features/KANDO_STRATEGIC_FEATURES_2026.md` (Section 6.5 - Onboarding & Initial Setup)
- **Kando HRIS Expansion**: `/Users/ericmagto/Projects/kando/research/kando-hris/` (context on customer onboarding challenges)
- **Access Control System**: `/Users/ericmagto/Projects/kando/access-group/` (post-setup permissions)

---

**Document Version**: 3.0 (Dependency Analysis Complete)  
**Created**: September 2026  
**Last Updated**: September 2026  
**Status**: 🟢 Ready for Implementation Planning  
**Maintained By**: Product & Engineering Team

---

## ✅ What's Changed in v3.0

**NEW**: Complete dependency analysis and proper sequencing verified through code review
- Added `WIZARD_DEPENDENCY_ORDER.md` with technical verification
- Updated `MVP_SIMPLE_WIZARD_INVENTORY.md` with correct ordering (10 wizards instead of earlier confusion)
- Phases reorganized: Phase 1 (4 wizards strict order), Phase 2 (4 parallel), Phase 3 (2 sequential)
- Timeline updated: 5+ weeks total (2 weeks Phase 1, 1-2 weeks Phase 2, 2-3 weeks Phase 3)

**KEY FINDING**: Wizard order is NOT flexible due to foreign key constraints
- Must complete: Periods → Holidays → Leave Types → (Shifts) → Leave Policies → Pay Types → Pay Groups
- Can parallelize: Departments & Cost Centers (not blocking)
- Can defer: Shift configuration depends on DB migration verification

---

## 📝 Files in This Project

```
/Users/ericmagto/Projects/kando/research/kando-onboarding-wizard/
│
├── README.md                              ← Navigation guide (you are here)
│
├── ⭐ WIZARD_DEPENDENCY_ORDER.md          ← TECHNICAL REFERENCE (START HERE)
│   └── Complete dependency analysis for all 10 wizards
│       Database models, FK relationships, implementation checklist
│       Read for: Engineering, Architecture, Implementation Planning
│
├── MVP_SIMPLE_WIZARD_INVENTORY.md         ← UPDATED (verified ordering)
│   └── MVP phases with correct sequencing
│       Phase 1 (4 wizards), Phase 2 (4 wizards), Phase 3 (2 wizards)
│       Read for: Product managers, designers, quick overview
│
├── DESIGN_SYSTEM.md                       ← Design system for HTML prototypes
├── discovery_questions.md                 ← Raw discovery responses
├── DISCOVERY_ANALYSIS_SUMMARY.md          ← Discovery findings & reasoning
│
├── MVP_WIREFRAME_PRIORITIES.md            ← Original wireframe approach (reference)
├── ONBOARDING_WIZARD_WITH_TEMPLATES.md    ← Full feature spec (reference)
│
├── SPRINT_KICKOFF_CHECKLIST.md            ← Sprint execution checklist
│
└── HTML Prototypes (in-progress)
    ├── index.html                         ← Main entry point
    ├── payroll-periods-wizard.html        ← Periods wizard prototype
    ├── holiday-calendar-wizard.html       ← Holiday wizard prototype
    └── leave-types-wizard.html            ← Leave types wizard prototype
```

---

### Key File Updates

✅ **WIZARD_DEPENDENCY_ORDER.md** (NEW)
- Complete technical dependency analysis
- All 10 wizards with proper sequencing verified
- Database model relationships & FK constraints
- Implementation checklist per wizard
- Migration timeline

✅ **MVP_SIMPLE_WIZARD_INVENTORY.md** (UPDATED)
- Phases updated with verified ordering
- All wizards marked as blocker/blocked/independent
- Dependencies clearly explained
- Phase 2 & 3 properly sequenced

✅ **README.md** (UPDATED)
- Navigation by role with new dependency doc first
- Document reading order updated
- References to new dependency analysis
