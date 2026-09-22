# Kando Onboarding Wizard - MVP Wireframe Priorities
## Discovery Analysis & 2-Week Sprint Scope

**Document Purpose**: Recommend MVP scope and prioritized wireframes for design & engineering  
**Status**: Recommendations based on discovery responses  
**Created**: September 2026  
**Sprint Duration**: 2 weeks (1-2 engineers, 1 designer)  
**Target Delivery**: MVP with core timekeeping + payroll template selection

---

## Executive Summary

Based on discovery responses, the MVP should focus on:
- **Single timekeeping template** (Standard Office Hours) to validate the wizard flow
- **2 payroll templates** (Fixed Salary + Hourly Wage) covering 80% of target customers
- **Basic customization** (amounts only, no formula builder in MVP)
- **Template selection + preview** → minimal setup time
- **No employee creation** (separate feature)
- **No post-wizard editing restrictions** (allows flexibility)

**Expected Impact**:
- Reduce support onboarding time from 2 weeks to 3-4 days
- Enable payroll setup on day 1 with working configurations
- Validate template approach before expanding in Phase 2

---

## 1. MVP Scope - The 80% Solution

### What's Included in 2-Week Sprint

**Timekeeping** (1 template, not 5):
- ✅ Standard Office Hours (8 AM - 5 PM, single shift)
- ✅ Pre-configured with grace period, break duration, undertime penalty
- ✅ No template selection—just "configure standard hours" step
- Rationale: Most target customers (10-20 employees) use standard hours; complex shifts can wait for Phase 2

**Payroll** (2 templates):
- ✅ Fixed Salary + Benefits (corporate employees)
- ✅ Hourly Wage + Overtime (retail, hourly workers)
- ✅ Pre-configured pay types, pay groups, deductions
- ✅ Basic customization (edit amounts only)
- ✅ Sample payroll preview before confirmation
- Rationale: Covers 80% of small businesses; other templates (commission, daily rate, manufacturing) deferred to Phase 2

**Customization** (limited):
- ✅ Edit allowance amounts (e.g., "Change DA from PHP 5,000 to PHP 3,000")
- ✅ Add/remove deductions (e.g., add optional health insurance)
- ✅ Change pay frequency (weekly, bi-weekly, monthly)
- ❌ Formula builder (Phase 2)
- ❌ Advanced rule engine (Phase 2)

**Compliance** (pre-built):
- ✅ BIR tax tables (pre-populated)
- ✅ SSS, PhilHealth, PagIBIG rates (pre-filled)
- ✅ 13th month tracking checkbox
- ✅ Statutory requirements baked into templates

**Not Included (Deferred to Phase 2+)**:
- ❌ Employee bulk import (separate feature, not part of wizard)
- ❌ Leave policies (separate, post-wizard)
- ❌ Departments/cost centers (assume single dept initially)
- ❌ Access control setup (default roles auto-assigned)
- ❌ Integration setup (email, notifications)
- ❌ Multi-template timekeeping selection
- ❌ AI recommendations, industry detection
- ❌ Configuration cloning

---

## 2. Recommended Wireframes for MVP (Ordered by Priority)

Design these **5 screens** in this priority order:

---

### **WIREFRAME 1: Wizard Entry/Welcome** ⭐ MUST-BUILD (Day 1)

**Screen Name**: Onboarding Wizard - Welcome  
**Purpose**: Orient users, explain what they're about to do, set expectations

**Key UI Elements**:
- Hero section: "Welcome to Kando Setup"
- Brief value prop: "Complete your payroll setup in minutes, not weeks"
- 3-4 bullet points: What the wizard covers (timekeeping, payroll, compliance)
- Estimated time: "Takes ~15-20 minutes"
- Large CTA button: "Start Setup →"
- Skip/Exit option: "I'll come back later"

**Data Dependencies**:
- None (first screen)

**Decision Point**:
- User clicks "Start Setup" → Go to Wireframe 2
- User clicks "Skip" → Show "Save progress?" dialog

**Design Notes**:
- Keep it simple, non-intimidating
- Match Kando brand (colors, typography)
- Mobile-responsive (assume tablet/phone use)
- Show progress indicator: "Step 1 of 4"

---

### **WIREFRAME 2: Organization Profile** ⭐ MUST-BUILD (Day 1)

**Screen Name**: Organization Profile  
**Purpose**: Capture basic company info, auto-detect defaults for templates

**Key UI Elements**:
- Form fields:
  - Company name (required, text input)
  - Industry dropdown (optional, 10-12 options: "Corporate", "Retail", "Manufacturing", etc.)
  - Employee count (required, number input or range slider: 10-20, 20-50, 50-100, 100+)
  - Country (pre-filled: "Philippines")
  - Timezone (dropdown with PH cities)
- Auto-detection message: "Based on your company size, we recommend Standard Office Hours setup"
- Next button (enabled after required fields filled)

**Data Dependencies**:
- Country = Philippines (hardcoded for MVP)
- Timezone list (static list of PH cities)

**Decision Point**:
- Employee count "10-20" triggers "Small Team" template recommendation
- Industry selection doesn't affect MVP (used for analytics/Phase 2)

**Design Notes**:
- All fields required except industry
- Show helpful tooltips: "Timezone affects payroll calculation"
- Save progress locally (browser storage) so user can return

---

### **WIREFRAME 3: Timekeeping Configuration** ⭐ MUST-BUILD (Days 2-3)

**Screen Name**: Timekeeping Setup - Standard Hours  
**Purpose**: Configure shift times, breaks, policies; no template selection in MVP

**Key UI Elements**:
- Section 1: Shift Configuration (read-only preview with edit link)
  - Shift Name: "Morning Shift" (display only, not editable)
  - Work Hours: 8:00 AM to 5:00 PM (editable time pickers)
  - Break Duration: 1 hour (editable dropdown: 30 min, 1 hr, 1.5 hrs, 2 hrs)
  - Break Time: 12:00 PM to 1:00 PM (editable, derives from duration)
  - [Show pre-filled values in light gray]

- Section 2: Attendance Policies (toggle-based)
  - ☑️ Enable grace period (e.g., "Allow 5 minutes late without penalty")
  - Grace period input: 5 minutes (editable number input)
  - ☐ Enable undertime deduction (checkbox)
  - Undertime calculation method: "Auto-deduct from salary" (dropdown)

- Section 3: Overtime Settings
  - Overtime threshold: 8 hours/day (editable dropdown)
  - Overtime multiplier: 1.5x (display, not editable in MVP)

- Preview box (read-only):
  ```
  Sample Schedule:
  Monday-Friday: 8:00 AM - 5:00 PM (8 hours)
  Break: 12:00 PM - 1:00 PM (1 hour)
  Working Hours: 40 hours/week
  ```

**Data Dependencies**:
- Timezone (from previous screen) → affects time display
- Employee count (from previous screen) → used for analytics

**Decision Point**:
- User clicks "Next" → Proceed to Wireframe 4 (Payroll Selection)
- User clicks "Edit" on any field → In-line editing mode

**Design Notes**:
- No "template selection" modal in MVP (hardcoded to Standard Hours)
- Show working hours calculation in real-time (40 hrs when 8:00-5:00 with 1 hr break)
- Time inputs: Use time pickers, not text entry
- Validation: Start time < End time, break fits within shift

---

### **WIREFRAME 4: Payroll Template Selection** ⭐⭐ MUST-BUILD (Days 3-4)

**Screen Name**: Payroll Configuration - Template Selection  
**Purpose**: Let user choose between Fixed Salary or Hourly, show what's included

**Key UI Elements**:
- Section Header: "How do you pay your employees?"
- 2 Template Cards (radio button selection):

  **Card 1: Fixed Salary + Benefits**
  - Icon: 💰 (salary bag)
  - Title: "Fixed Salary + Benefits"
  - Subtitle: "For salaried employees (default)"
  - When to use: "Corporate, office workers"
  - What's included:
    - ✓ Basic salary
    - ✓ Allowances (DA, transportation, etc.)
    - ✓ Statutory deductions (SSS, PhilHealth, PagIBIG, tax)
    - ✓ 2 pre-configured pay groups (Regular, Probationary)
  - [Learn More ↗] link (opens tooltip/inline details)
  - Radio button (default selected)

  **Card 2: Hourly Wage + Overtime**
  - Icon: ⏰ (clock)
  - Title: "Hourly Wage + Overtime"
  - Subtitle: "For hourly/wage employees"
  - When to use: "Retail, F&B, hourly workers"
  - What's included:
    - ✓ Hourly wage with variable hours
    - ✓ Overtime premium (1.5x, 2x for holidays)
    - ✓ Night shift differential
    - ✓ Statutory deductions
    - ✓ 2 pre-configured pay groups (Full-time, Part-time)
  - [Learn More ↗] link
  - Radio button

- Info box (conditional, shows on selection):
  - "Selected: Fixed Salary + Benefits"
  - "Next: Review and customize the salary structure"

- Navigation:
  - [← Back] button
  - [Continue →] button (enabled after selection)

**Data Dependencies**:
- Employee count (from organization profile) → doesn't affect selection in MVP
- Industry (optional, not used in MVP)

**Decision Point**:
- Fixed Salary selected → Go to Wireframe 5a (Customize Fixed Salary)
- Hourly Wage selected → Go to Wireframe 5b (Customize Hourly Wage)

**Design Notes**:
- Only 2 cards in MVP (other 4 templates deferred to Phase 2)
- "Learn More" should expand inline, not open modal
- Make selection very visual (card gets border highlight on select)
- Show estimated setup time: "~5 minutes to customize"

---

### **WIREFRAME 5A: Customize Payroll - Fixed Salary** ⭐⭐ MUST-BUILD (Days 4-5)

**Screen Name**: Payroll Setup - Fixed Salary Customization  
**Purpose**: Review and edit pay types, pay groups, deductions; preview sample calculation

**Key UI Elements**:
- Progress indicator: "Step 3 of 4" / "62.5% Complete"
- Section 1: Pay Frequency & Basics
  - Pay Frequency: Dropdown (Weekly / Bi-weekly / Monthly) [Default: Monthly]
  - Pay Day: Date picker (e.g., "25th of month" or "Last day of month")
  - Payment Method: Dropdown (Bank Transfer / Check / Cash) [Default: Bank Transfer]

- Section 2: Pay Types (Editable list)
  - Table/List with columns: Pay Type | Amount | Type | Actions
  - Row 1: Basic Salary | [editable input] | Fixed | [Delete icon]
  - Row 2: Dearness Allowance | PHP 5,000 | Fixed | [Edit icon]
  - Row 3: Transportation Allowance | PHP 3,000 | Fixed | [Edit icon]
  - Row 4: Communication Allowance | PHP 2,000 | Fixed | [Edit icon]
  - Row 5: Meal Vouchers | PHP 2,000 | Fixed | [Edit icon]
  - [+ Add New Pay Type] button
  
  - Edit inline or expand modal (simple: name + amount + type)

- Section 3: Deductions Review (Read-only, with explanation)
  - Table: Deduction Type | Formula | Rate
  - SSS Contribution | Based on salary bracket | Auto-calculated
  - PhilHealth | 3% of Basic Salary | Auto-calculated
  - PagIBIG | 1% of Basic Salary | Auto-calculated
  - Withholding Tax (BIR) | Government tax table | Auto-calculated
  - Health Insurance Premium | Optional | [Toggle to add] PHP 1,500/month
  - [View Details] link for each (shows formula/explanation)

- Section 4: Pay Groups Assignment
  - Display: "Pre-configured Pay Groups"
  - Group 1: "Regular Employees"
    - Includes: All pay types + standard deductions
    - Apply To: [Dropdown] Select role or "All employees"
    - Status: Ready
  - Group 2: "Probationary Employees"
    - Includes: Basic Salary + DA only (reduced benefits)
    - Apply To: [Dropdown] Select role
    - Status: Ready (can disable if not needed)
  - [+ Add Custom Pay Group] button (deferred to Phase 2)

- Section 5: Preview Sample Calculation
  - Inline preview box (read-only):
    ```
    SAMPLE PAYROLL CALCULATION
    Employee: Sample Employee
    Pay Period: September 2026
    
    EARNINGS
    Basic Salary: PHP 30,000
    Dearness Allowance: PHP 5,000
    Transportation: PHP 3,000
    Communication: PHP 2,000
    Meal Vouchers: PHP 2,000
    ─────────────
    TOTAL GROSS: PHP 42,000
    
    DEDUCTIONS
    SSS: PHP 1,575
    PhilHealth (3%): PHP 1,260
    PagIBIG (1%): PHP 420
    Withholding Tax: PHP 2,850
    ─────────────
    TOTAL DEDUCTIONS: PHP 6,105
    
    NET PAY: PHP 35,895
    ```
  - [Update Preview] button (recalculates if any values changed)

- Navigation:
  - [← Back] button
  - [Next / Continue →] button
  - [Skip Customization] link (keep defaults and proceed)

**Data Dependencies**:
- Basic Salary (from employee record, editable for template)
- Government tax tables (BIR, SSS, PhilHealth, PagIBIG rates pre-loaded)
- Template selected: "Fixed Salary + Benefits"

**Decision Point**:
- User clicks "Next" → Go to Wizard Confirmation (Wireframe 6)
- User clicks "Skip Customization" → Go directly to Confirmation
- User adds/edits pay type → Preview updates automatically

**Design Notes**:
- All editable fields have inline edit mode (click to edit, enter to save)
- Deductions are read-only but show "Why?" tooltips
- Preview calculation updates in real-time as user edits
- Validation: Salary >= 0, allowance amounts >= 0
- Save progress to session storage

---

### **WIREFRAME 5B: Customize Payroll - Hourly Wage** ⭐⭐ MUST-BUILD (Days 5-6)

**Screen Name**: Payroll Setup - Hourly Wage Customization  
**Purpose**: Configure hourly rates, OT multipliers, deductions; preview

**Key UI Elements**:
- Progress indicator: "Step 3 of 4" / "62.5% Complete"
- Section 1: Pay Frequency & Basics
  - Pay Frequency: Dropdown (Weekly / Bi-weekly) [Default: Weekly]
  - Pay Day: Dropdown (Every Friday / Every 2 weeks on Friday / etc.)
  - Payment Method: Dropdown (Bank Transfer / Check / Cash) [Default: Bank Transfer]

- Section 2: Hourly Rates & Multipliers (Read-only reference, not editable in MVP)
  - Base Hourly Rate: [Note: "Set per employee, not in wizard"]
  - Regular Hours Definition: 8 hours/day or 40 hours/week (checkbox to select)
  - Multiplier Reference Table (read-only):
    | Type | Multiplier |
    |------|-----------|
    | Regular Hours | 1.0x |
    | Overtime (> 8/day or 40/week) | 1.5x |
    | Holiday Work | 2.0x (Regular Holiday), 3.0x (Special Holiday) |
    | Night Shift (10 PM - 6 AM) | 1.1x |

- Section 3: Pay Types (Display only, not editable in MVP)
  - Table: Pay Type | Calculation | Enabled
  - Regular Hours | Hours × Rate × 1.0x | ☑️
  - Overtime | Hours × Rate × 1.5x | ☑️
  - Holiday Premium | Hours × Rate × 2.0x | ☑️
  - Night Shift Differential | Hours × Rate × 1.1x | ☑️
  - [Note: "These are calculated automatically from timekeeping data"]

- Section 4: Deductions Review (Same as Wireframe 5A)
  - SSS, PhilHealth, PagIBIG, Withholding Tax (auto-calculated)
  - Health Insurance Premium (optional toggle)

- Section 5: Pay Groups Assignment
  - Group 1: "Full-Time Hourly Workers"
    - Includes: Regular + OT + Holiday + Night Shift Diff
    - Apply To: All hourly employees
    - Status: Ready
  - Group 2: "Part-Time Workers"
    - Includes: Regular hours only (no OT multiplier)
    - Apply To: Part-time staff
    - Status: Ready (can disable)

- Section 6: Preview Sample Calculation
  - Inline preview box (read-only):
    ```
    SAMPLE PAYROLL CALCULATION
    Employee: Sample Hourly Worker
    Pay Period: Sept 1-7, 2026 (Weekly)
    Hourly Rate: PHP 250/hour
    
    HOURS WORKED
    Regular Hours: 40 hrs × PHP 250 = PHP 10,000
    Overtime: 5 hrs × PHP 250 × 1.5 = PHP 1,875
    Night Shift Diff: 8 hrs × PHP 250 × 1.1 = PHP 2,200
    ─────────────
    TOTAL GROSS: PHP 14,075
    
    DEDUCTIONS
    SSS: PHP 875
    PhilHealth (3%): PHP 422
    PagIBIG (1%): PHP 141
    Withholding Tax: PHP 500
    ─────────────
    TOTAL DEDUCTIONS: PHP 1,938
    
    NET PAY: PHP 12,137
    ```
  - [Update Preview] button

- Navigation:
  - [← Back] button
  - [Next / Continue →] button
  - [Skip Customization] link

**Data Dependencies**:
- Template selected: "Hourly Wage + Overtime"
- Timekeeping configuration (needed for OT calculation rules)
- Government rates (auto-loaded)

**Decision Point**:
- User clicks "Next" → Go to Wizard Confirmation (Wireframe 6)
- User clicks "Skip Customization" → Go directly to Confirmation

**Design Notes**:
- Hourly rate NOT editable in wizard (set per-employee later)
- OT thresholds based on timekeeping setup (e.g., 8 hrs/day from Wireframe 3)
- All multipliers read-only (configured by system)
- Preview shows realistic weekly payroll example

---

### **WIREFRAME 6: Wizard Summary & Confirmation** ⭐⭐ MUST-BUILD (Days 6-7)

**Screen Name**: Setup Summary - Review & Confirm  
**Purpose**: Final review of all choices before saving; clear confirmation of what's ready

**Key UI Elements**:
- Section 1: Setup Summary (Read-only, collapsible sections)
  - **Timekeeping Configuration**
    - Work Hours: 8:00 AM - 5:00 PM, Monday-Friday
    - Break: 1 hour (12:00 PM - 1:00 PM)
    - Grace Period: 5 minutes
    - [Edit ✏️] link (returns to Wireframe 3)

  - **Payroll Configuration**
    - Template: Fixed Salary + Benefits (or Hourly Wage + Overtime)
    - Pay Frequency: Monthly on 25th (or Weekly every Friday)
    - Pay Groups: Regular Employees, Probationary
    - Deductions: SSS, PhilHealth, PagIBIG, Tax, Insurance
    - [Edit ✏️] link (returns to Wireframe 5A/5B)

  - **Compliance Settings**
    - Country: Philippines
    - Tax Mode: Standard (13th Month enabled)
    - Government Rates: Pre-loaded (BIR, SSS, PhilHealth, PagIBIG)
    - [View Details] link

- Section 2: Post-Setup Checklist (To-do list, outside wizard)
  - ☐ Upload Employee List (CSV bulk import) - [Guide ↗]
  - ☐ Set Individual Salary/Rates (for each employee) - [Guide ↗]
  - ☐ Assign Departments (optional) - [Guide ↗]
  - ☐ Set Up Leave Policies - [Guide ↗]
  - ☐ Process First Payroll - [Guide ↗]
  - Expected timeline: "Complete these in 2-3 days, ready to run payroll by Sept 15"

- Section 3: What's Ready Now
  - ✅ Timekeeping configured (employees can start clocking in)
  - ✅ Payroll structure ready (awaiting employee setup)
  - ✅ Tax/compliance rules loaded
  - ✅ Sample payroll validated

- Section 4: Confirmation
  - Large checkbox: ☑️ "I've reviewed the setup and it looks correct"
  - Info box: "This can be edited anytime after setup is complete"
  - Large CTA button: [Confirm & Finish Setup →]
  - [← Back to Edit] link

**Data Dependencies**:
- All previous screens (timekeeping + payroll selections)
- Summary text from templates

**Decision Point**:
- User clicks "Confirm & Finish Setup" → Save wizard data to DB, show completion screen
- User clicks "Edit" on any section → Return to that wireframe
- User clicks "Back" → Return to previous screen

**Design Notes**:
- This is the "commit point" — once confirmed, data is saved
- Show clear message: "Setup complete! You can modify these settings anytime"
- Provide links to post-setup guides (not in scope, but wireframe should account)
- Save all wizard state before this screen (auto-save during previous steps)

---

## 3. Data Dependencies Map

```
Wireframe 1: Welcome
  └─ No dependencies

Wireframe 2: Organization Profile
  └─ Input: Company name, employee count, industry, timezone
  └─ Output: used by Wireframes 3, 4, 5

Wireframe 3: Timekeeping Config
  └─ Depends on: Organization profile (timezone)
  └─ Outputs: Shift times, break duration, OT threshold
  └─ Used by: Wireframe 5A/5B (for OT calculation preview)

Wireframe 4: Payroll Template Selection
  └─ Depends on: Organization profile (employee count)
  └─ Outputs: Selected template ("Fixed Salary" or "Hourly Wage")
  └─ Routes to: Wireframe 5A or 5B

Wireframe 5A: Customize Fixed Salary
  └─ Depends on: Template selection (Fixed Salary)
  └─ Depends on: Government rates (BIR, SSS, PhilHealth, PagIBIG)
  └─ Outputs: Pay types, deductions, pay groups
  └─ Used by: Wireframe 6 (summary)

Wireframe 5B: Customize Hourly Wage
  └─ Depends on: Template selection (Hourly Wage)
  └─ Depends on: Timekeeping config (OT threshold, night shift hours)
  └─ Depends on: Government rates
  └─ Outputs: Pay types, multipliers, deductions
  └─ Used by: Wireframe 6 (summary)

Wireframe 6: Confirmation
  └─ Depends on: All previous wireframes
  └─ Output: Triggers save to database
```

---

## 4. Open Questions & Blockers

### Critical (Must Answer Before Build Starts)
1. **Post-wizard editing restrictions**: Can users edit payroll after confirmation, or is it locked?
   - Current answer from discovery: "Not answered yet"
   - **Recommendation for MVP**: Allow editing anytime (lowest risk, most flexible)
   - Design implication: Don't add "locked" UI, treat as editable post-setup

2. **Basic Salary input in wizard**: Should the wizard ask for default/sample basic salary, or is that only per-employee later?
   - Current answer: "Not answered yet"
   - **Recommendation for MVP**: Show as "Example: PHP 30,000" (not editable in wizard, set per-employee)
   - Design implication: Wireframe 5A/5B should show this as read-only in preview

3. **Multi-organization support**: Can users create multiple organizations in the wizard, or is this single-org per login?
   - **Recommendation for MVP**: Single org per wizard run; multiple orgs handled elsewhere
   - Design implication: "Create Another Org?" link after completion

4. **Timezone impact on payroll**: How does timezone affect calculations? Is this just for display or does it trigger different tax rules?
   - **Recommendation**: Timezone = display only in MVP (all companies in PH, same tax rules)
   - Design implication: No tax/calculation changes based on timezone

### High Priority (Should Answer Before Design Freeze)
5. **Government rate updates**: Who maintains BIR/SSS rates? Should the wizard show last-updated date?
   - **Recommendation**: Show "Rates last updated: Sept 2026" in compliance section
   - Design implication: Add versioning to templates

6. **Customization scope**: Can users add completely custom pay types (e.g., "Team Lunch Stipend"), or only modify pre-built ones?
   - Current answer: "Not answered yet"
   - **Recommendation for MVP**: Add custom pay types only after selecting template; basic UI to name + amount + formula (simple)
   - Design implication: "Add Custom Pay Type" button with modal

7. **Multiple shift assignments**: In timekeeping, can employees work different shifts, or is the wizard assuming all on "Standard Hours"?
   - **Recommendation for MVP**: All on standard hours; shift rotation handled post-setup
   - Design implication: Wireframe 3 is "setup default shift", not per-employee assignment

8. **Pay group auto-assignment**: When the wizard creates pay groups, how do employees get assigned? (Manual, role-based, department-based?)
   - **Recommendation for MVP**: Manual assignment during employee bulk import (separate feature)
   - Design implication: Wireframe 6 shows "Next: Upload employees and assign pay groups"

### Medium Priority (Can Be Addressed in Phase 2)
9. **Formula validation**: Should the MVP prevent "impossible" formulas (e.g., negative salary), or allow free-form entry?
   - **Recommendation**: Validate min/max ranges (salary >= PHP 8,000 minimum wage)
   - Design implication: Add validation error messages

10. **Compliance flags**: Should the wizard warn if missing required fields (e.g., BIR registration number)?
    - **Recommendation for MVP**: Pre-populate from government data, allow edit; warning if empty
    - Design implication: Wireframe 6 includes "Compliance Settings" section with edit link

---

## 5. Sprint Timeline - 2-Week Breakdown

### Week 1: Design & Setup

**Days 1-2: Discovery & Planning**
- Review all wireframes with PM/Design/Engineering
- Create Figma/design file with component library
- Set up frontend repo structure (Vue 3 Composition API)
- Design DB schema for wizard state + template storage

**Days 3-4: Wireframe 1 & 2**
- Design: Welcome screen + Organization Profile form
- Frontend: Implement Wireframe 1 (static, no logic)
- Frontend: Implement Wireframe 2 (form validation, local storage)
- Engineering: Create wizard state management store (Pinia)

**Day 5: Wireframe 3**
- Design: Timekeeping config screen (time pickers, toggles)
- Frontend: Implement Wireframe 3 (form logic, time validation)
- Engineering: Create timekeeping template model

### Week 2: Implementation & Testing

**Day 6: Wireframe 4 & 5A**
- Design: Template selection cards (visual polish)
- Frontend: Implement Wireframe 4 (radio selection, routing)
- Frontend: Implement Wireframe 5A (editable table, real-time preview)
- Engineering: Create payroll template models (fixed salary)

**Day 7: Wireframe 5B & 6**
- Frontend: Implement Wireframe 5B (hourly wage customization)
- Frontend: Implement Wireframe 6 (summary, confirmation flow)
- Engineering: Build wizard completion API endpoint

**Days 8-10: Integration & Testing**
- Connect wizard to backend: Save wizard state after each step
- Add error handling + validation messages
- Test complete flow end-to-end (all 6 screens)
- Mobile/responsive testing
- Design polish pass (colors, spacing, animations)

**Days 11-12: Buffer & Fixes**
- Bug fixes from testing
- Accessibility review (keyboard nav, screen reader)
- Performance optimization
- Final QA

**Days 13-14: Documentation & Handoff**
- Write component documentation (for future phases)
- Create setup guide for post-wizard (employee import, etc.)
- Prepare for Phase 2 expansion (documentation for 4 new templates)
- Demo to stakeholders

---

## 6. Success Criteria for MVP

### Launch Criteria
- ✅ All 6 wireframes fully implemented and functional
- ✅ End-to-end wizard flow works (Wireframe 1 → 6)
- ✅ Mobile responsive (tablet/phone)
- ✅ Wizard state persists (can return later)
- ✅ Sample payroll calculations accurate
- ✅ No hard-stop bugs

### Quality Metrics
- ✅ Accessibility: WCAG 2.1 AA (keyboard nav, screen reader)
- ✅ Performance: <2s page load on 4G
- ✅ Error handling: Clear validation messages
- ✅ Mobile: Works on iOS Safari, Android Chrome

### Business Impact (Post-Launch)
- Reduce avg. onboarding time from 2 weeks to 3-4 days
- > 70% of new customers complete wizard without support
- Collect customer feedback for Phase 2 templates
- Support team reports "easier setup" in customer interviews

---

## 7. Phase 2+ Roadmap (Beyond 2-Week MVP)

### What Comes Next (Not in MVP)
1. **Expand Timekeeping Templates** (Week 3-4)
   - Add Manufacturing (3-shift), Retail (flexible), Healthcare (12-hr)
   - Template selection screen (like Wireframe 4, but for timekeeping)

2. **Expand Payroll Templates** (Week 3-4)
   - Add Commission, Daily Rate, Flexible Remuneration
   - Commission-specific flows (tiered rates, performance bonus)

3. **Advanced Customization** (Week 5-6)
   - Formula builder UI (drag-drop or text-based)
   - Custom pay type creation with full formula support
   - Advanced deduction rules (e.g., "deduct 5% for union members only")

4. **AI Recommendations** (Phase 3)
   - Auto-detect company industry from name/description
   - Recommend templates based on industry best practices
   - Suggest default values (salary ranges, allowance levels)

5. **Configuration Cloning** (Phase 3)
   - Copy wizard results from similar organizations
   - Pre-fill forms based on industry/size

6. **Post-Wizard Flows** (Ongoing)
   - Employee bulk import (CSV uploader)
   - Leave policy setup
   - Department/cost center creation
   - Access control assignment

---

## Appendix: Wireframe Checklist for Designers

### Before Handing Off to Development

- [ ] **Wireframe 1 (Welcome)**
  - [ ] Hero image/icon placeholder identified
  - [ ] CTA button size/color defined
  - [ ] Mobile layout verified (single column)

- [ ] **Wireframe 2 (Organization Profile)**
  - [ ] Form field styles consistent (input height, spacing)
  - [ ] Dropdown options clearly visible (max 12 items)
  - [ ] Error state designed (red border, error message)
  - [ ] Tooltip styles defined

- [ ] **Wireframe 3 (Timekeeping Config)**
  - [ ] Time picker component designed (mobile-friendly)
  - [ ] Toggle switch styles (on/off states)
  - [ ] Editable fields vs. read-only visual distinction clear
  - [ ] Real-time validation feedback designed

- [ ] **Wireframe 4 (Payroll Template Selection)**
  - [ ] Card design (border, shadow, hover state)
  - [ ] Radio button styling
  - [ ] "Learn More" tooltip design
  - [ ] Responsive: 2 cards stack on mobile

- [ ] **Wireframe 5A/5B (Customization)**
  - [ ] Table design (striped rows, sortable headers?)
  - [ ] Inline edit UI (click-to-edit, save on blur)
  - [ ] Preview box styling (background color, monospace font for calculations)
  - [ ] Modal for "Add New Pay Type" (if needed)

- [ ] **Wireframe 6 (Confirmation)**
  - [ ] Collapsible section design (expand/collapse arrow)
  - [ ] Checkbox styling
  - [ ] Post-setup checklist visual distinction (outside wizard)
  - [ ] Success state design (for after confirmation)

### Design System Requirements
- [ ] Color palette: primary, secondary, success, error, warning
- [ ] Typography: heading sizes (H1-H4), body text, code font
- [ ] Spacing scale: 4px, 8px, 16px, 24px, 32px (used consistently)
- [ ] Button states: default, hover, active, disabled
- [ ] Form field states: default, focus, error, disabled
- [ ] Icons: 24x24px SVG library (at least 10 icons: Save, Edit, Add, Delete, Back, etc.)

---

## Summary Table: Wireframe Priorities & Effort

| # | Wireframe | Priority | Effort (hrs) | Day | Design First? |
|---|-----------|----------|--------------|-----|---------------|
| 1 | Welcome | HIGH | 4 | 1-2 | ✅ |
| 2 | Org Profile | HIGH | 6 | 3-4 | ✅ |
| 3 | Timekeeping Config | HIGH | 8 | 5-6 | ✅ |
| 4 | Payroll Template Selection | HIGH | 6 | 6-7 | ✅ |
| 5A | Customize Fixed Salary | HIGH | 10 | 7-8 | ✅ |
| 5B | Customize Hourly Wage | HIGH | 10 | 8-9 | ✅ |
| 6 | Confirmation | HIGH | 8 | 10-11 | ✅ |

**Total Estimated Effort**: 52 hours (design + frontend + backend)
- Design: ~16 hours (2 days)
- Frontend: ~28 hours (3-4 days)
- Backend/State Management: ~8 hours (1-2 days)

**Team Assumption**: 1 UX Designer + 1-2 Frontend Engineers + 1 Backend Engineer
**Timeline**: 2 weeks (10 working days) ✅ Achievable

---

**Document Version**: 1.0  
**Created**: September 2026  
**Status**: Ready for Design Kickoff  
**Next Action**: Design team reviews wireframes, frontend team sets up repo structure
