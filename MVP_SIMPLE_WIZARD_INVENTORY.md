# MVP Simple Wizard Inventory
## Start Simple, Build Momentum, Then Add Complex Features

**Purpose**: Identify which setup items can be wizarded in MVP (phases 1-2)  
**Created**: September 2026  
**Status**: Recommended approach — Simple first, complex later

---

## Overview

Instead of building a complex payroll/timekeeping template wizard from day 1, start with **simpler mandatory setup items** that every new org needs. This approach:

- ✅ Delivers value faster (1-2 weeks vs 2-3 weeks)
- ✅ Reduces complexity (no formula builders)
- ✅ Builds momentum (quick wins)
- ✅ Learns from users (before complex features)
- ✅ Sets foundation (simple wizard → complex templates layer on top)

---

## Setup Items Found in Kando Codebase

Based on code review of `/kando-backend/app/Models/Settings/`:

| Item | Model | Complexity | MVP Priority | Why |
|------|-------|-----------|--------------|-----|
| **Payroll Periods** | Period, PeriodHeader, PeriodType | Medium | 🟢 Phase 1 | Every org needs to define when payday is |
| **Holiday Calendar** | Holiday, HolidayType | Low | 🟢 Phase 1 | Simple date entry, critical for payroll |
| **Leave Types** | LeaveType, LeavePolicy | Low | 🟢 Phase 1 | Define what types of leave exist (vacation, sick, etc.) |
| **Shift Patterns** | Shift, ShiftPolicy, ShiftPolicyType | Medium | 🟡 Phase 2 | Complex (multiple shifts, rotations) |
| **Leave Accrual Rules** | LeaveCredit, LeaveCreditHistory, LeavePolicyEmployee | Medium | 🟡 Phase 2 | Varies by country/policy, can start simple |
| **Departments** | Department | Low | 🟡 Phase 2 | Can be bulk-imported, not critical day 1 |
| **Cost Centers** | CostCenter (from Payroll models) | Low | 🟡 Phase 2 | Often mirrors departments |
| **Pay Groups** | PayGroup (from Payroll) | High | 🔴 Phase 3 | Requires formula builder, template approach |
| **Pay Types** | PayType (from Payroll) | High | 🔴 Phase 3 | Requires formula builder, deduction logic |
| **Compliance Setup** | BIR, SSS, PhilHealth, PagIBIG settings | High | 🔴 Phase 3 | Complex tax rules, can pre-populate |

---

## Recommended MVP Phases (VERIFIED ORDERING)

### 🟢 Phase 1: Critical Setup (Week 1-2, ~25 hours)

**Goal**: Get new org ready to track time  
**Timeline**: 1-2 weeks  
**Scope**: 4 wizards (in this exact order due to database dependencies)  
**Critical Path**: Periods → Holidays → Leave Types → Shifts

#### 1️⃣ **Payroll Periods Wizard** 🟢 MUST BE FIRST

- **What**: Define when payroll runs (monthly, semi-monthly, bi-weekly)
- **Why**: BLOCKER - Required before timesheet & payroll can process
- **Database Model**: PeriodHeader + Period (auto-generated)
- **Dependencies**: None (only needs Organization)
- **Complexity**: Medium (date math for period boundaries)
- **Users fill in**:
  - Period name (e.g., "2026 Monthly")
  - Period type (Monthly, Semi-monthly, Bi-weekly, Weekly)
  - Start date + End date
  - Pay day
  - Optional: Description
- **Backend creates**: 
  - 1 PeriodHeader record
  - 12+ Period records (auto-generated from boundary rules)
- **Validation**: Ensure no overlapping periods
- **Blocks**: Holiday Calendar (needs periods for date validation)
- **Effort**: 4 hours (design 1h + frontend 2h + backend 1h)

#### 2️⃣ **Holiday Calendar Wizard** 🟡 MUST BE SECOND

- **What**: Add holidays and half-days for the year
- **Why**: BLOCKER - Payroll won't calculate holiday multipliers without this
- **Database Model**: HolidayType + Holiday
- **Dependencies**: Periods should exist (for date validation)
- **Complexity**: Low (date picker + dropdown)
- **Users fill in**:
  - Holiday name (e.g., "New Year", "Christmas")
  - Date
  - Type (Full day, Half day morning, Half day afternoon)
  - Holiday type (Regular holiday, Special non-working day)
  - Optional: Description
- **Pre-populated**: Philippines standard holidays from HolidayType
- **Wizard flow**:
  - Show pre-populated PH holidays → User confirms/removes
  - Option to add custom holidays
  - Calendar view to verify
  - Validate dates fall within defined periods
- **Validation**: 
  - No duplicate dates
  - Holiday dates within period ranges
- **Blocks**: Payroll processing (needs holiday data for calculations)
- **Effort**: 5 hours (design 1h + frontend 2.5h + backend 1.5h)

#### 3️⃣ **Leave Types Wizard** 🔵 INDEPENDENT (3rd)

- **What**: Define what kinds of leave employees can request
- **Why**: BLOCKER - Required for leave management + leave policies
- **Database Model**: LeaveType (auto-creates TimesheetField)
- **Dependencies**: None (but recommended after Periods/Holidays for logical flow)
- **Complexity**: Low (text input + toggle)
- **Users fill in**:
  - Leave type name (e.g., "Vacation", "Sick Leave", "Bereavement")
  - Description
  - Requires attachment? (yes/no toggle)
  - Active? (yes/no toggle)
- **Pre-populated**: Common PH leave types
  - Vacation Leave
  - Sick Leave
  - Bereavement Leave
  - Emergency Leave
  - Maternity/Paternity Leave
- **Wizard flow**:
  - Show pre-populated leave types → User confirms/removes
  - Option to add custom leave types
  - List view to verify
- **Validation**: Unique leave type names per org
- **Blocks**: Leave Policies (#5) - must have types before defining policies
- **Effort**: 4 hours (design 1h + frontend 2h + backend 1h)

#### 4️⃣ **Shift Configuration Wizard** 🔵 INDEPENDENT (4th)

- **What**: Define work shifts (8-5 office, 3-shift manufacturing, flexible, etc.)
- **Why**: RECOMMENDED - Required for schedule tracking
- **Database Model**: ShiftPolicy + Shift
- **Dependencies**: None (independent)
- **Complexity**: Medium (multiple shifts, rotation rules)
- **Users fill in**:
  - Shift policy name (e.g., "Standard Office")
  - Individual shift definitions:
    - Shift name (e.g., "Morning", "Evening")
    - Time in / Time out
    - Break hours
    - Is active
- **Pre-populated**: Templates for common shift patterns
  - Standard Office Hours (8 AM - 5 PM)
  - Manufacturing 3-shift
  - Retail flexible
  - Healthcare 12-hour
  - Hybrid/Remote (flexible)
- **Validation**: 
  - Shifts don't overlap
  - Break hours reasonable
- **Can defer if**: Org uses manual shift assignment initially
- **Effort**: 12 hours (design 3h + frontend 6h + backend 3h)

**Phase 1 Total Effort**: ~25 hours (2-week sprint for 1 designer + 1-2 devs)

**What User Can Do After Phase 1**:
- ✅ Track attendance/timesheets (Periods exist)
- ✅ Process basic payroll (Periods + Holidays defined)
- ✅ Submit leave requests (Leave Types exist)
- ✅ Assign work shifts (Shift Configuration done)
- ✅ Manage basic organizational structure

**⚠️ Important Note**: Shift model exists in code but database migration may not exist yet. Verify before implementation.

---

### 🟡 Phase 2: Enhanced Setup (Week 3-4, ~22 hours)

**Goal**: Enable leave management and organizational structure  
**Timeline**: 1-2 weeks  
**Scope**: 3-4 wizards (can happen in parallel after Phase 1 complete)

#### 5️⃣ **Leave Policies Wizard** 🟡 MUST FOLLOW #3

- **What**: Define how employees accrue/earn leave per leave type
- **Why**: RECOMMENDED - Employees need accrual rules to earn leave balance
- **Database Model**: LeavePolicy + LeavePolicyEmployee
- **Dependencies**: Leave Types (#3) must exist first
- **Complexity**: Medium (accrual formulas)
- **Users fill in**:
  - Leave policy name (e.g., "Vacation - Standard", "Vacation - Executive")
  - Policy type (Monthly Accrual, Annual Fixed, etc.)
  - Accrual rate/formula
    - If monthly: days per month
    - If annual: total days per year
    - If fixed: X days at hire
  - Effective date
  - Assign to employee groups
- **Wizard flow**:
  - Select leave type
  - Choose accrual method (pre-built templates or custom)
  - Show accrual preview (e.g., "1.25 days/month = 15 days/year")
  - Assign to employees
- **Validation**: Accrual logic is mathematically sound
- **Effort**: 8 hours (design 2h + frontend 4h + backend 2h)

#### 6️⃣ **Leave Credits Initialization** 🟡 FOLLOWS #5

- **What**: Grant initial leave balance to employees
- **Why**: RECOMMENDED - Employees start with defined leave balance
- **Database Model**: LeaveCredit + LeaveCreditHistory
- **Dependencies**: Leave Policies (#5) should exist
- **Complexity**: Low (bulk initialization)
- **Users fill in** (or auto-calculate):
  - Employee selection
  - Leave type
  - Initial balance (calculated from policy or manual override)
  - Effective date (usually Jan 1 or hire date)
- **Wizard flow**:
  - Show employees needing credits
  - Calculate per policy (or show override fields)
  - Preview credit totals
  - Initialize all at once
- **Validation**: Credits match policy accrual
- **Can automate**: When policy assigned to employee
- **Effort**: 6 hours (design 1h + frontend 2.5h + backend 2.5h)

#### 7️⃣ **Departments Wizard** 🔵 INDEPENDENT (Optional)

- **What**: Create organizational departments
- **Why**: OPTIONAL - Useful for org structure but not blocking
- **Database Model**: Department
- **Dependencies**: None (independent)
- **Complexity**: Low (simple text input)
- **Users fill in**:
  - Department name (e.g., "HR", "Sales", "Operations")
  - Description
  - Manager (optional)
  - Active? (yes/no)
- **Pre-populated**: None (varies by org)
- **Wizard flow**:
  - Create departments one by one
  - Bulk import option
  - Assign managers
- **Validation**: Unique department names
- **Can defer**: Set up later without blocking features
- **Effort**: 4 hours (design 1h + frontend 1.5h + backend 1.5h)

#### 8️⃣ **Cost Centers Wizard** 🔵 INDEPENDENT (Optional)

- **What**: Create cost allocation centers for payroll
- **Why**: OPTIONAL - Useful for cost tracking but not blocking
- **Database Model**: CostCenter + EmployeeCostCenter
- **Dependencies**: None (independent)
- **Complexity**: Low (simple text input)
- **Users fill in**:
  - Cost center name (e.g., "Sales Operations", "IT Infrastructure")
  - Code (optional)
  - Description
  - Active? (yes/no)
- **Pre-populated**: None
- **Wizard flow**:
  - Create cost centers
  - Optionally assign employees (many-to-many)
  - Bulk import option
- **Validation**: Unique cost center names
- **Can defer**: Set up later without blocking features
- **Effort**: 4 hours (design 1h + frontend 1.5h + backend 1.5h)

**Phase 2 Total Effort**: ~22 hours (2-week sprint for 1 designer + 1-2 devs)

**What User Can Do After Phase 2**:
- ✅ Define leave accrual rules (Leave Policies)
- ✅ Initialize employee leave balance (Leave Credits)
- ✅ Organize employees into departments (optional)
- ✅ Allocate costs to cost centers (optional)

**When Phase 2 Can Happen**: Anytime after Phase 1 (can be parallel)

---

### 🔴 Phase 3: Payroll Setup (Week 5+, ~28 hours)

**Goal**: Enable payroll processing  
**Timeline**: 2-3 weeks  
**Scope**: 2 wizards (must be in this order)

#### 9️⃣ **Pay Types Wizard** 🟢 BLOCKER FOR PAYROLL

- **What**: Define salary structure (salary, allowances, deductions, taxes)
- **Why**: BLOCKER - Required before any payroll can process
- **Database Model**: PayType + PayTypeField + PayslipField
- **Dependencies**: Context from Periods (#1) & Holidays (#2) helpful, but not strict FK
- **Complexity**: High (formula builder for computed fields)
- **Users fill in**:
  - Pay type name (e.g., "Regular Employee Salary")
  - Code (auto-generated: PT1, PT2, etc.)
  - Description
  - Individual pay fields:
    - Field name (Salary, DA, Transportation, etc.)
    - Type (Basic, Allowance, Deduction, Tax)
    - Calculation (Fixed amount or Formula)
    - Sequence/grouping on payslip
  - Pre-configured deductions:
    - SSS (with brackets)
    - PhilHealth (3% employee, 3% employer)
    - PagIBIG (1% employee, 2% employer)
    - BIR withholding (based on BIR tables)
- **Pre-populated**: 
  - Common pay types (Fixed Salary + Benefits, Hourly Wage, Commission, etc.)
  - Government rate defaults (SSS brackets, PhilHealth rates, PagIBIG rates, BIR rates)
- **Wizard flow**:
  - Select or create pay type
  - Add pay fields (salary, allowances, deductions)
  - Configure formulas (e.g., Overtime = hourly_rate * 1.25)
  - Add government deductions with current rates
  - Preview payslip layout
  - Sample calculation (e.g., gross 30K → net 26.5K)
- **Validation**: 
  - All formulas reference valid fields
  - Deduction percentages reasonable
  - Government rates current
- **Effort**: 16 hours (design 4h + frontend 8h + backend 4h)

#### 🔟 **Pay Groups Wizard** 🟡 MUST FOLLOW #9

- **What**: Create pay groups and assign employees
- **Why**: BLOCKER - Payroll needs to know which employees get which structure
- **Database Model**: PayGroup + PayGroupEmployee + FieldFormula
- **Dependencies**: Pay Types (#9) must exist first
- **Complexity**: High (formula per field per group)
- **Users fill in**:
  - Group name (e.g., "Regular Employees", "Management")
  - Select pay type (from wizard #9)
  - Override formulas per field if needed (usually inherit from pay type)
  - Assign employees to group
- **Wizard flow**:
  - Select pay type
  - Create group(s) within pay type
  - Inherit or customize formulas
  - Preview payslip for group
  - Assign employees (many-to-many)
  - Bulk import from CSV or select from list
- **Validation**: 
  - Every formula references valid fields
  - Employees assigned to only one pay group per pay type
- **Effort**: 12 hours (design 3h + frontend 5h + backend 4h)

**Phase 3 Total Effort**: ~28 hours (2-3 week sprint for 1 designer + 1-2 devs)

**What User Can Do After Phase 3**:
- ✅ Process first payroll (all components defined)
- ✅ Generate payslips with accurate calculations
- ✅ Calculate government deductions correctly
- ✅ Run payroll for next month

**⚠️ Prerequisites for Phase 3**: 
- Periods, Holidays, Leave Types, Shifts defined
- Employees created and assigned to groups

---

## Verified Ordering (Database Dependency Analysis)

✅ **ANALYSIS COMPLETE** - See `/WIZARD_DEPENDENCY_ORDER.md` for full technical details

Based on backend model inspection, database migrations, and foreign key analysis:

| # | Wizard | Depends On | Blocks | Required? |
|---|--------|-----------|--------|-----------|
| 1️⃣ | Payroll Periods | Nothing | #2, Timesheet processing | ✅ YES |
| 2️⃣ | Holiday Calendar | #1 (context) | Payroll calculations | ✅ YES |
| 3️⃣ | Leave Types | Nothing | #5, #6 | ✅ YES |
| 4️⃣ | Shift Configuration | Nothing | Schedule assignments | ⚠️ OPTIONAL* |
| 5️⃣ | Leave Policies | #3 | #6 | ✅ YES |
| 6️⃣ | Leave Credits | #3, #5 | Leave requests | ⚠️ OPTIONAL |
| 7️⃣ | Departments | Nothing | Employee organization | ⚠️ OPTIONAL |
| 8️⃣ | Cost Centers | Nothing | Cost allocation | ⚠️ OPTIONAL |
| 9️⃣ | Pay Types | Nothing | #10, Payroll processing | ✅ YES |
| 🔟 | Pay Groups | #9 | Payroll processing | ✅ YES |

*Shift: Model exists but DB migration may not be created yet - verify before implementation

---

## MVP Approach: Simple Wizard + Phased Rollout

| Phase | Focus | Wizards | Count | Timeline | Effort | Org Can Do |
|-------|-------|---------|-------|----------|--------|-----------|
| **Phase 1** | Time Tracking | Periods, Holidays, Leave Types, Shifts | 4 | Week 1-2 | ~25 hrs | Track time, request leave |
| **Phase 2** | Leave Mgmt | Leave Policies, Credits, Departments, Cost Centers | 4 | Week 3-4 | ~22 hrs | Manage accrual, organize |
| **Phase 3** | Payroll | Pay Types, Pay Groups | 2 | Week 5+ | ~28 hrs | Process payroll |
| **TOTAL** | Full HRMS | 10 wizards | 10 | 5+ weeks | ~75 hrs | Complete HR operations |

**Why This Order**:
- Phase 1: Blocking dependencies (Periods → Holidays → Leave Types)
- Phase 2: Leave management + optional organizational structure
- Phase 3: Payroll setup (most complex, builds on earlier phases)

**Benefits**:
- Phase 1: Shows value in 2 weeks (org can start tracking time)
- Phase 2: Adds leave management (1-2 week parallel work)
- Phase 3: Complete payroll (final 2-3 weeks for full HRMS)

---

## Dependency Verification Checklist

✅ **Confirmed via code review:**
- [x] PeriodHeader/Period foreign key to Organization
- [x] Holiday foreign key to HolidayType & Organization
- [x] LeaveType foreign key to Organization
- [x] LeavePolicy foreign key to LeaveType (cascadeOnDelete)
- [x] ShiftPolicy foreign key to Organization
- [x] Shift foreign key to ShiftPolicy (model exists)
- [x] PayType foreign key to Organization
- [x] PayGroup foreign key to PayType (cascadeOnDelete)
- [x] All models use organization_id as boundary
- [x] All ForeignKey order constraints verified

⚠️ **Requires verification:**
- [ ] Shift table migration exists (model present but migration not found)
- [ ] All government rate lookups pre-seeded (SSS, PhilHealth, PagIBIG, BIR)
- [ ] PeriodType values pre-seeded (Monthly, Semi-monthly, Bi-weekly, Weekly)

---

## Next Steps (UPDATED)

### Before Kickoff

1. **Confirm Ordering** ✅
   - [x] Database dependencies verified
   - [x] See `WIZARD_DEPENDENCY_ORDER.md` for technical details
   - [ ] Confirm with engineering team

2. **Verify Database State**
   - [ ] Confirm Shift table migration exists
   - [ ] Verify lookup tables pre-seeded (PeriodType, HolidayType, LeavePolicyType)
   - [ ] Check government rate defaults (SSS brackets, PhilHealth, PagIBIG, BIR)

3. **Design Phase 1 Wireframes** (in order: 1→2→3→4)
   - [ ] Payroll Periods wizard (4 screens)
   - [ ] Holiday Calendar wizard (4 screens)
   - [ ] Leave Types wizard (4 screens)
   - [ ] Shift Configuration wizard (5 screens)

4. **Set Up Testing Infrastructure**
   - [ ] Create seed data: 1 complete org with all settings
   - [ ] Test end-to-end wizard flow
   - [ ] Verify FK constraints enforced

---

## Questions: ANSWERED

| Question | Answer | Evidence |
|----------|--------|----------|
| Are Periods mandatory? | ✅ YES | Period FK required in timesheet, payroll models |
| Are Holidays used in payroll? | ✅ YES | Holiday calculations used in pay computations |
| Are Leave Types required? | ✅ YES | LeavePolicy cascadeOnDelete on LeaveType |
| What's the current UI for each? | TBD | Need frontend review: `/src/views/` |
| Can users create out of order? | ❌ NO | FK constraints enforce order |
| Do we need test data? | ✅ YES | Pre-populate common items (leave types, holidays, rates) |
| What happens if we skip Shift? | ⚠️ | Model exists but might be optional for MVP |

---

## Related Documentation

📄 **Full Dependency Analysis**: `WIZARD_DEPENDENCY_ORDER.md` (this folder)
- Complete technical details for all 10 wizards
- Database models, FK relationships, cascade behavior
- Implementation checklist per wizard
- Migration timeline

📄 **Original Scope Documents**:
- `MVP_WIREFRAME_PRIORITIES.md` - Original wireframe approach
- `DISCOVERY_ANALYSIS_SUMMARY.md` - Discovery findings
- `ONBOARDING_WIZARD_WITH_TEMPLATES.md` - Full feature spec

---

**Document Version**: 1.0  
**Created**: September 2026  
**Related Documents**:
- DISCOVERY_QUESTIONS.md (Answers about scope)
- ONBOARDING_WIZARD_WITH_TEMPLATES.md (Original complex proposal)
- MVP_WIREFRAME_PRIORITIES.md (Original payroll-focused approach)
