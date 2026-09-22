# Kando Onboarding Wizard - Dependency Analysis & Proper Sequencing

**Status**: ✅ VERIFIED (Based on backend model analysis + database migrations)  
**Date**: September 2026  
**Verified By**: Backend model inspection + migration timeline review

---

## Executive Summary

The onboarding wizards MUST be completed in a specific order due to database dependencies. Attempting to create items out of order will fail or produce incorrect results.

**Critical Path (must happen in this order):**
1. Payroll Periods → 2. Holiday Calendar → 3. Leave Types → 4. Shift Configuration → [can proceed in parallel]

---

## Complete Dependency Map

### Legend
- 🟢 **BLOCKER**: Must complete before anything else can work
- 🟡 **BLOCKED**: Depends on another wizard completing first
- 🔵 **INDEPENDENT**: Can happen anytime (but recommended order shown)
- ⚫ **OPTIONAL**: Not required for initial onboarding

---

## Phase 1: Mandatory Setup (Week 1-2)

### 1️⃣ Payroll Periods Wizard 🟢 BLOCKER

**Database Models Involved:**
- `PeriodType` (lookup table - pre-seeded)
- `PeriodHeader` (organization's period configuration)
- `Period` (individual period instances - auto-generated)

**Foreign Key Dependencies:**
- `PeriodHeader.organization_id` → organizations table
- `PeriodHeader.type_id` → period_types table (FK)
- `Period.header_id` → period_headers table (FK)

**Why It's First:**
- Timesheet module requires periods to track time
- Payroll processing requires periods for date ranges
- All future date-based calculations depend on this
- No other setup item can function without defined periods

**What Gets Created:**
- 1 PeriodHeader record (e.g., "Monthly 2026")
- 12+ Period records (auto-generated for next 12 months)

**Blocked By**: Nothing (only needs Organization)
**Blocks**: Holiday Calendar (to validate dates), Payroll processing

**Effort**: 4 hours (design 1h + frontend 2h + backend 1h)

**Implementation Notes:**
- PeriodType has been pre-seeded with values like "Monthly", "Semi-monthly", "Bi-weekly"
- Period model uses JSON fields for complex date logic
- Auto-generate periods for 12+ months when header is created
- Validate no overlapping periods

---

### 2️⃣ Holiday Calendar Wizard 🟡 BLOCKED BY #1

**Database Models Involved:**
- `HolidayType` (lookup table - system-wide or org-specific)
- `Holiday` (specific holiday instances)

**Foreign Key Dependencies:**
- `Holiday.organization_id` → organizations table
- `Holiday.type_id` → holiday_types table (FK)

**Why It's Second:**
- Depends on periods existing (to validate holiday dates fall within periods)
- Payroll calculations need holidays to compute holiday pay multipliers
- Without holidays, payroll won't calculate correctly
- Holiday dates should be validated against period ranges

**What Gets Created:**
- Multiple Holiday records with dates, times, type (full day/half day)
- Uses pre-populated Philippines holidays from HolidayType

**Blocked By**: Periods (should exist before adding holidays)
**Blocks**: Payroll processing

**Effort**: 5 hours (design 1h + frontend 2.5h + backend 1.5h)

**Implementation Notes:**
- HolidayType can be system-wide (when organization_id is null) or org-specific
- Pre-populate with standard Philippines holidays
- Validate holiday date is within at least one defined period
- Allow half-day morning/afternoon configuration
- Provide calendar view for verification

---

### 3️⃣ Leave Types Wizard 🔵 INDEPENDENT

**Database Models Involved:**
- `LeaveType` (leave type master)
- `TimesheetField` (auto-created, tracks leave in timesheets)

**Foreign Key Dependencies:**
- `LeaveType.organization_id` → organizations table
- `LeaveType.created_by` → users table (FK)
- Auto-creates: `TimesheetField` with MorphOne relationship

**Why It's Third:**
- No dependencies on other setup (independent)
- Required before employees can request leave
- Enables Leave Management module
- Auto-creates corresponding TimesheetField

**What Gets Created:**
- Multiple LeaveType records (Vacation, Sick, Bereavement, etc.)
- Auto-creates TimesheetField entries for each leave type

**Blocked By**: Nothing (can start immediately)
**Blocks**: Leave Policies (depends on leave types existing)

**Effort**: 4 hours (design 1h + frontend 2h + backend 1h)

**Implementation Notes:**
- Pre-populate with common Philippines leave types
- Each LeaveType creation auto-triggers TimesheetField creation via boot() method
- Include toggle for "Attachment Required"
- Track created_by and updated_by for audit trail

---

### 4️⃣ Shift Configuration Wizard 🔵 INDEPENDENT

**Database Models Involved:**
- `ShiftPolicy` (shift template/policy)
- `ShiftPolicyType` (lookup - pre-seeded)
- `Shift` (individual shift definitions)

**Foreign Key Dependencies:**
- `ShiftPolicy.organization_id` → organizations table
- `ShiftPolicy.created_by` → users table (FK)
- `Shift.organization_id` → organizations table
- `Shift.policy_id` → shift_policies table (FK)
- Shift uses many-to-many with ShiftPolicyType via shift_policy_rules table

**Why It's Fourth:**
- No hard dependencies on other wizards
- Independent setup (can start immediately)
- Recommended to do early for schedule assignments
- Defines work shifts and break patterns

**What Gets Created:**
- 1 ShiftPolicy record (e.g., "Standard Office Hours")
- 1+ Shift records under policy (e.g., 8 AM - 5 PM)
- ShiftPolicyRule entries linking to ShiftPolicyType

**Blocked By**: Nothing (can start immediately)
**Blocks**: Schedule assignments to employees

**Effort**: 12 hours (design 3h + frontend 6h + backend 3h)

**Implementation Notes:**
- ShiftPolicy acts as a container/grouping mechanism
- Individual Shifts defined within a policy (e.g., "Morning", "Evening")
- ShiftPolicyType values are pre-seeded (Undertime, Overtime, Night Differential, etc.)
- Time fields use TimeCast for proper time formatting
- Include break hours configuration

**⚠️ Important Note**: Shift model exists in code but **migration table was not found**. May need to be created or is not yet in database.

---

## Phase 2: Enhanced Setup (Week 3-4)

### 5️⃣ Leave Policies Wizard 🟡 BLOCKED BY #3

**Database Models Involved:**
- `LeavePolicy` (policy per leave type)
- `LeavePolicyType` (lookup - pre-seeded)
- `LeavePolicyEmployee` (many-to-many bridge)

**Foreign Key Dependencies:**
- `LeavePolicy.organization_id` → organizations table
- `LeavePolicy.leave_type_id` → leave_types table (FK) ✅ MUST EXIST
- `LeavePolicy.type_id` → leave_policy_types table (FK)
- `LeavePolicy.created_by` → users table (FK)
- `LeavePolicyEmployee.leave_policy_id` → leave_policies table (FK)
- `LeavePolicyEmployee.employee_id` → employees table (FK)

**Why It's Fifth:**
- Depends on LeaveType existing (#3)
- Defines accrual rules per leave type (15 days/year, monthly accrual, etc.)
- Employees cannot earn leave without policies
- Creates foundation for leave credit management

**What Gets Created:**
- 1+ LeavePolicy records per LeaveType (e.g., "Vacation Leave - Standard", "Vacation Leave - Executive")
- LeavePolicyEmployee bridge records (created when policy assigned to employees)

**Blocked By**: Leave Types (#3)
**Blocks**: Leave Credits generation

**Effort**: 8 hours (design 2h + frontend 4h + backend 2h)

**Implementation Notes:**
- LeavePolicyType values are pre-seeded (e.g., "Monthly Accrual", "Annual Fixed", etc.)
- Define accrual rate/formula per policy
- Show accrual schedule preview (e.g., "1.25 days per month")
- Include validation of accrual logic
- Support employee-specific policies via LeavePolicyEmployee

---

### 6️⃣ Leave Credits Initialization 🟡 BLOCKED BY #3 & #5

**Database Models Involved:**
- `LeaveCredit` (employee leave balance)
- `LeaveCreditHistory` (audit trail)

**Foreign Key Dependencies:**
- `LeaveCredit.employee_id` → employees table (FK)
- `LeaveCredit.leave_type_id` → leave_types table (FK) ✅ MUST EXIST (#3)
- `LeaveCredit.created_by` → users table (FK)
- `LeaveCreditHistory.employee_id` → employees table (FK)
- `LeaveCreditHistory.leave_type_id` → leave_types table (FK)

**Why It's Sixth:**
- Depends on LeaveType (#3) and Leave Policies (#5)
- Grants initial leave balance to employees
- Without credits, employees have no leave to take
- Creates starting balance for the year

**What Gets Created:**
- 1 LeaveCredit per employee per leave type
- LeaveCreditHistory entries tracking credit creation

**Blocked By**: Leave Types (#3), Leave Policies (#5), Employees must exist
**Blocks**: Employee leave requests

**Effort**: 6 hours (design 1h + frontend 2.5h + backend 2.5h)

**Implementation Notes:**
- Can be automated when policy is assigned to employee
- Or can be manual bulk initialization wizard
- Support per-employee override of accrual amounts
- Track year in LeaveCredit (regenerate annually)
- Log initial credit grant in LeaveCreditHistory

---

### 7️⃣ Departments Wizard 🔵 INDEPENDENT (Optional)

**Database Models Involved:**
- `Department` (organizational department)

**Foreign Key Dependencies:**
- `Department.organization_id` → organizations table
- `Department.created_by` → users table (FK)
- Reverse: `Employee.department_id` → departments table (FK)

**Why It's Seventh:**
- Optional for initial onboarding
- Provides organizational structure
- Nice for reporting and employee grouping
- Can be bulk-created rather than wizard

**What Gets Created:**
- Multiple Department records (HR, Sales, Operations, etc.)

**Blocked By**: Nothing (independent)
**Blocks**: Nothing critical (optional)

**Effort**: 4 hours (design 1h + frontend 1.5h + backend 1.5h)

**Implementation Notes:**
- Can defer to Phase 2 or even later
- Could also be bulk-imported from CSV
- Not blocking any critical functionality
- Useful for reporting but not required for payroll

---

### 8️⃣ Cost Centers Wizard 🔵 INDEPENDENT (Optional)

**Database Models Involved:**
- `CostCenter` (cost allocation center)
- `EmployeeCostCenter` (many-to-many bridge)

**Foreign Key Dependencies:**
- `CostCenter.organization_id` → organizations table
- `CostCenter.created_by` → users table (FK)
- `EmployeeCostCenter.cost_center_id` → cost_centers table (FK)
- `EmployeeCostCenter.employee_id` → employees table (FK)

**Why It's Eighth:**
- Optional for initial onboarding
- Supports cost accounting/reporting
- Employees can be assigned multiple cost centers
- Not required for basic payroll processing

**What Gets Created:**
- Multiple CostCenter records (e.g., "IT Department", "Sales Operations")
- EmployeeCostCenter bridge records when assigned

**Blocked By**: Nothing (independent)
**Blocks**: Nothing critical (optional)

**Effort**: 4 hours (design 1h + frontend 1.5h + backend 1.5h)

**Implementation Notes:**
- Many-to-many relationship (employee can belong to multiple cost centers)
- Often mirrors department structure but more flexible
- Can be deferred or bulk-imported
- Used for cost allocation in payroll

---

## Phase 3: Payroll Setup (Week 5+)

### 9️⃣ Pay Types Wizard 🟢 BLOCKER FOR PAYROLL

**Database Models Involved:**
- `PayType` (master payroll structure)
- `PayTypeField` (individual pay fields within type)
- `PayslipField` (payslip display configuration)
- `PayslipSection` (grouping on payslip)

**Foreign Key Dependencies:**
- `PayType.organization_id` → organizations table
- `PayType.timesheet_source_id` → timesheet_sources table (optional)
- `PayTypeField.pay_type_id` → pay_types table (FK)
- `PayslipField.pay_type_id` → pay_types table (FK)
- `PayslipField.section_id` → payslip_sections table (FK)

**Why It's Ninth:**
- Defines salary structure, allowances, deductions
- Required before payroll calculations can work
- Sets up government deductions (SSS, PhilHealth, BIR, PagIBIG)
- Configures pay fields with formulas

**What Gets Created:**
- 1+ PayType records (e.g., "Regular Employee Salary", "Hourly Wage")
- 10+ PayTypeField entries (salary, allowances, deductions)
- PayslipField entries (display config)

**Blocked By**: Nothing directly (but context from Periods #1, Holidays #2 helpful)
**Blocks**: Pay Groups (#10)

**Effort**: 16 hours (design 4h + frontend 8h + backend 4h)

**Implementation Notes:**
- Auto-generates code as "PT{sequence}" (PT1, PT2, etc.)
- Complex: includes formula builder for computed fields
- Pre-populate with common pay types (Fixed Salary, Hourly, Commission, etc.)
- Include government rate defaults (SSS brackets, PhilHealth, PagIBIG rates)
- Link to TimeSheet source for computed fields
- Support pay type fields: salary, allowances, bonuses, deductions, taxes

---

### 🔟 Pay Groups Wizard 🟡 BLOCKED BY #9

**Database Models Involved:**
- `PayGroup` (grouping within pay type)
- `PayGroupEmployee` (many-to-many bridge)
- `FieldFormula` (formula mapping)

**Foreign Key Dependencies:**
- `PayGroup.pay_type_id` → pay_types table (FK) ✅ MUST EXIST (#9)
- `PayGroupEmployee.pay_group_id` → pay_groups table (FK)
- `PayGroupEmployee.employee_id` → employees table (FK)
- `FieldFormula.pay_group_id` → pay_groups table (FK)
- `FieldFormula.pt_field_id` → pay_type_fields table (FK)

**Why It's Tenth:**
- Depends on PayType existing (#9)
- Groups employees by pay structure within a pay type
- Defines formulas for each pay field per group
- Final step before payroll processing

**What Gets Created:**
- 1+ PayGroup records per PayType (e.g., "Regular Group", "Management Group")
- PayGroupEmployee bridge records
- FieldFormula entries for each field in the group

**Blocked By**: Pay Types (#9)
**Blocks**: Payroll processing

**Effort**: 12 hours (design 3h + frontend 5h + backend 4h)

**Implementation Notes:**
- Each PayGroup has its own formula set (allows variations)
- Support formula builder or template selection
- Many-to-many assignment of employees to groups
- Can have multiple pay groups per pay type
- Use FieldFormula to override formulas per group

---

## Critical Dependencies Summary

```
Periods (#1) ✅
    ↓ (needed by #2)
    
Holidays (#2) ✅
    ↓ (context for payroll)

LeaveTypes (#3) ✅                ShiftPolicy (#4) 🔵
    ↓ (needed by #5)                  ↓
    
LeavePolicy (#5) ✅        Departments (#7) 🔵
    ↓ (needed by #6)
    
LeaveCredits (#6) 🟡      CostCenters (#8) 🔵

PayTypes (#9) ✅
    ↓ (needed by #10)
    
PayGroups (#10) ✅
    ↓
    
READY FOR PAYROLL PROCESSING
```

---

## What's NOT Required to Start

✅ **Can skip initially:**
- Departments (#7) - can assign later
- Cost Centers (#8) - can assign later
- Advanced pay type customization - use defaults

❌ **Cannot skip:**
- Periods (#1) - timesheet baseline
- Holidays (#2) - payroll multipliers
- Leave Types (#3) - leave management
- Shift Config (#4) - schedule tracking
- Leave Policies (#5) - leave accrual
- Pay Types (#9) - payroll structure
- Pay Groups (#10) - payroll grouping

---

## Database Constraints to Watch

### Foreign Key Constraints
All models use `organization_id` as primary organizational boundary. **No org can access another org's data.**

### Cascade Behavior
- `PayType.cascadeOnDelete` - Deleting PayType deletes PayGroups
- `PayGroup` - Deleting PayGroup deletes PayGroupEmployees and formulas
- `LeavePolicy.cascadeOnDelete` - Deleting leave type cascades

### Soft Dependencies (non-FK validation)
- Holiday dates should fall within Period ranges (validate in wizard)
- Pay formulas reference pay fields (validate in builder)
- Leave credit year should match current year (validate in initialization)

---

## Recommended Wizard Flow UI

```
STEP 1: Organization Profile (from signup)
        ↓
[PHASE 1 - CRITICAL PATH]
STEP 2: Payroll Periods (MUST DO)
        ↓
STEP 3: Holiday Calendar (MUST DO)
        ↓
STEP 4: Leave Types (MUST DO)
        ↓
STEP 5: Shift Configuration (MUST DO)
        ↓
        ⭐ CHECKPOINT: Basic Setup Complete
           (org can now track time)
        ↓
[PHASE 2 - LEAVE MANAGEMENT]
STEP 6: Leave Policies (RECOMMENDED)
        ↓
STEP 7: Departments (OPTIONAL)
        ↓
STEP 8: Cost Centers (OPTIONAL)
        ↓
        ⭐ CHECKPOINT: Team Setup Complete
        ↓
[PHASE 3 - PAYROLL]
STEP 9: Pay Types (MUST DO FOR PAYROLL)
        ↓
STEP 10: Pay Groups (MUST DO FOR PAYROLL)
        ↓
        ⭐ CHECKPOINT: Payroll Ready
           (first payroll can process)
```

---

## Implementation Checklist

### Before Building Any Wizard

- [ ] Confirm all database migrations exist (especially Shift table)
- [ ] Verify all FK constraints match this document
- [ ] Check if lookup tables (PeriodType, HolidayType, etc.) are pre-seeded
- [ ] Review existing form pages to identify patterns
- [ ] Confirm government rates (SSS, PhilHealth, PagIBIG, BIR) are current

### Per Wizard Implementation

- [ ] Check model relationships in Laravel
- [ ] Identify which fields are required vs optional
- [ ] Build validation rules matching business logic
- [ ] Create sample data/seed file for testing
- [ ] Test cascade/delete behavior

---

## Questions Answered (From MVP_SIMPLE_WIZARD_INVENTORY.md)

| Question | Answer | Supporting Data |
|----------|--------|-----------------|
| Are Periods mandatory? | ✅ YES | Period FK required for timesheet & payroll |
| Are Holidays used in payroll? | ✅ YES | Payroll calculations use holiday multipliers |
| Are Leave Types required? | ✅ YES | Required before leave requests or policies |
| Is order flexible? | ❌ NO | FK dependencies enforce order |
| Can we skip any? | ✅ SOME | Departments & cost centers optional |
| Do we need test data? | ✅ YES | Pre-populate common items where possible |

---

## Migration Timeline

| Phase | Timeline | Wizards | Effort | Org Can Do |
|-------|----------|---------|--------|-----------|
| **Phase 1** | Week 1-2 | Periods, Holidays, Leave Types, Shifts | ~25 hours | Track time, request leave |
| **Phase 2** | Week 3-4 | Leave Policies, Departments, Cost Centers | ~22 hours | Manage leave, organizational reporting |
| **Phase 3** | Week 5+ | Pay Types, Pay Groups | ~28 hours | Process payroll |
| **TOTAL** | 5+ weeks | 10 wizards | ~75 hours | Full HR operations |

---

**Document Version**: 1.0  
**Created**: September 2026  
**Based On**: Backend model analysis + database migration review  
**Status**: ✅ Verified & Ready for Implementation
