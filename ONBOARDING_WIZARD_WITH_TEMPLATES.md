# Kando Onboarding Wizard with Template Options
## Interactive Setup for Timekeeping & Payroll Configuration

**Document Purpose**: Detailed design for guided setup wizard with pre-built templates  
**Audience**: Product managers, engineers, UX designers  
**Status**: Feature Design Specification  
**Date**: September 2026

---

## Overview

The **Kando Setup Wizard** is an interactive, step-by-step guide that walks new organizations through initial configuration. At key stages, users can **select from industry-specific templates** that pre-configure payroll and timekeeping settings—eliminating manual setup and reducing implementation time from 4-6 weeks to 2-3 days.

### Business Value
- ✅ **Faster Onboarding**: 2-3 days vs 4-6 weeks
- ✅ **Reduced Errors**: Pre-validated templates prevent misconfiguration
- ✅ **Better First Experience**: Users see value immediately
- ✅ **Lower Support Burden**: Fewer configuration questions
- ✅ **Higher Activation**: Users complete setup and start using the system same day

---

## Wizard Flow Overview

```
START
  ↓
Step 1: Organization Profile
  ├─ Company name, industry, size
  ├─ Logo, timezone
  └─ Country/Region selection
  ↓
Step 2: Organizational Structure
  ├─ Departments (with templates)
  ├─ Cost centers
  └─ Reporting hierarchy
  ↓
Step 3: Employee Setup
  ├─ Bulk import template download
  ├─ Employee count verification
  └─ Role assignment preview
  ↓
Step 4: TIMEKEEPING CONFIGURATION [TEMPLATE SELECT]
  ├─ Select timekeeping template
  ├─ Shift patterns
  ├─ Schedule policies
  └─ Geolocation settings (optional)
  ↓
Step 5: PAYROLL CONFIGURATION [TEMPLATE SELECT]
  ├─ Select payroll template
  ├─ Pay frequency & cycle
  ├─ Pay types (salary, allowances, deductions)
  ├─ Pay groups (with pre-configured formulas)
  ├─ Tax & deductions setup
  └─ Compliance settings
  ↓
Step 6: Leave Policies
  ├─ Leave types (with templates)
  ├─ Accrual rules
  └─ Holiday calendar
  ↓
Step 7: Access & Permissions
  ├─ Default roles (Owner, Manager, HR Admin, Employee)
  └─ Initial user assignments
  ↓
Step 8: Integration Setup
  ├─ Email configuration
  ├─ Notification preferences
  └─ Third-party integrations
  ↓
COMPLETE + POST-SETUP CHECKLIST
```

---

## Step 4: Timekeeping Configuration with Templates

### Template Selection Screen

**Question**: "What's your company's work structure?"

#### Template Options

| Template | Industry | Shift Pattern | Use Case |
|----------|----------|---------------|----------|
| **Standard Office Hours** | Corporate, BPO | 8:00 AM - 5:00 PM | Traditional office hours, single shift |
| **Manufacturing** | Manufacturing | Multiple shifts (6am-2pm, 2pm-10pm, 10pm-6am) | 24-hour factory operations |
| **Retail & Hospitality** | Retail, F&B | Flexible shifts (4-8 hrs) | Variable shift lengths, weekend work |
| **Healthcare** | Healthcare | 12-hour shifts | Nurses, hospital staff |
| **Construction & Field** | Construction, Logistics | Site-based, flexible | On-site work with geolocation tracking |
| **Hybrid/Remote** | Tech, Startups | Flexible hours | No fixed schedule, remote work |

### What Each Template Includes

#### **Standard Office Hours Template**
```
Shift Configuration:
├─ Shift Name: "Morning Shift"
├─ Start Time: 8:00 AM
├─ End Time: 5:00 PM
├─ Break Duration: 1 hour (12:00-1:00 PM)
├─ Overtime Threshold: 8 hours/day
├─ Grace Period: 5 minutes (no penalty)
└─ Undertime Penalty: Auto-deduct from salary

Schedule Policy:
├─ Work Days: Monday-Friday
├─ Working Hours/Week: 40 hours
├─ Attendance Rules: Auto-mark as "On Time" if clocked in by 8:05 AM
└─ Late Policy: Mark as "Late" if clocked in after 8:05 AM
```

#### **Manufacturing Template**
```
Shift 1 Configuration:
├─ Shift Name: "Morning Shift"
├─ Start Time: 6:00 AM
├─ End Time: 2:00 PM
├─ Break Duration: 1 hour
├─ Overtime Threshold: 8 hours/day

Shift 2 Configuration:
├─ Shift Name: "Afternoon Shift"
├─ Start Time: 2:00 PM
├─ End Time: 10:00 PM
├─ Break Duration: 1 hour
├─ Overtime Threshold: 8 hours/day

Shift 3 Configuration:
├─ Shift Name: "Night Shift"
├─ Start Time: 10:00 PM
├─ End Time: 6:00 AM
├─ Break Duration: 1 hour
├─ Overtime Threshold: 8 hours/day
├─ Night Shift Bonus: +10% pay differential
└─ Shift Rotation: Automatic rotation weekly
```

#### **Retail & Hospitality Template**
```
Shift Configuration (Flexible):
├─ Shift Types: "Morning" (6-8 hrs), "Evening" (4-6 hrs), "Full Day" (8 hrs)
├─ Shift Start Times: 6:00 AM, 9:00 AM, 12:00 PM, 3:00 PM, 6:00 PM
├─ Shift End Times: Dynamic based on start time
├─ Break Duration: Varies by shift length
└─ Overtime Threshold: 10 hours/week

Schedule Policy:
├─ Work Days: Monday-Sunday (including weekends)
├─ Scheduling: Manager-assigned weekly schedules
├─ Last-Minute Changes: Enable shift swaps
└─ Weekend Premium: +15% pay on Saturdays, +25% on Sundays
```

#### **Healthcare Template**
```
Shift Configuration:
├─ Shift Type: "12-Hour Shift"
├─ Day Shift: 7:00 AM - 7:00 PM
├─ Night Shift: 7:00 PM - 7:00 AM
├─ Break Duration: 2 hours (unpaid meal break)
├─ Overtime: Any hours beyond 12 are paid at 1.5x
├─ Night Shift Allowance: +PHP 100/shift
└─ Call-Out Premium: +50% for on-call hours

Schedule Policy:
├─ Rotation: 2-day shift, 2-day off (customizable)
├─ Min Rest Period: 8 hours between shifts
├─ Mandatory Days Off: Ensure 2 days consecutive rest
└─ Overtime Calculation: Per-shift basis
```

#### **Hybrid/Remote Template**
```
Shift Configuration:
├─ Shift Type: Flexible (No Fixed Hours)
├─ Work Days: Monday-Friday (customizable)
├─ Core Hours (Optional): 10:00 AM - 3:00 PM
├─ Flexible Start: 6:00 AM - 10:00 AM
├─ Flexible End: 3:00 PM - 7:00 PM
├─ No Geolocation Tracking: Required
├─ Attendance Mode: Trust-based (no clock-in required)
└─ Time Logging: Daily time entry by employee

Schedule Policy:
├─ Tracking: Timesheet-based reporting
├─ Overtime: Tracked on timesheet, manager-approved
└─ Location: Any (remote work)
```

### Customization After Template Selection

Users can **modify after selection**:
- ✏️ Add/remove shifts
- ✏️ Adjust shift times
- ✏️ Change break durations
- ✏️ Modify overtime thresholds
- ✏️ Adjust grace periods

**Next Button**: Proceed to Payroll Configuration

---

## Step 5: Payroll Configuration with Templates

### Template Selection Screen

**Question**: "How do you want to structure payroll?"

#### Template Options

| Template | Company Type | Pay Structure | Use Case |
|----------|--------------|---------------|----------|
| **Fixed Salary + Benefits** | Corporate | Fixed salary + allowances | Standard salaried employees |
| **Hourly Wage + Overtime** | Retail, F&B | Hourly rate + OT premium | Hourly workers, overtime-heavy |
| **Manufacturing & Shift Premiums** | Manufacturing | Base + shift differentials | Multi-shift operations |
| **Commission-Based** | Sales | Base salary + commission | Sales teams, variable income |
| **Daily Rate + Incentives** | Construction, Logistics | Daily rate + bonuses | Daily wage workers |
| **Flexible Remuneration** | Tech, Startups | Salary + flexible allowances | Benefits customization |

### What Each Template Includes

#### **Fixed Salary + Benefits Template**

**Pay Types Included:**
```
Basic Salary
├─ Type: Fixed Monthly Salary
├─ Formula: Employee's monthly salary (from employee record)
└─ Pay Frequency: Monthly

Allowances
├─ Dearness Allowance (DA)
│  ├─ Amount: PHP 5,000/month
│  └─ Formula: Fixed
├─ Transportation Allowance
│  ├─ Amount: PHP 3,000/month
│  └─ Formula: Fixed
├─ Communication Allowance
│  ├─ Amount: PHP 2,000/month
│  └─ Formula: Fixed
└─ Meal Vouchers
   ├─ Amount: PHP 2,000/month
   └─ Formula: Fixed

Deductions
├─ PhilHealth
│  ├─ Formula: 3% of Basic Salary (Employee), 3% (Employer)
│  └─ Deduction Type: Tax/Contribution
├─ SSS Contribution
│  ├─ Formula: Based on salary bracket
│  └─ Deduction Type: Tax/Contribution
├─ PagIBIG
│  ├─ Formula: 1% of Basic Salary (Employee), 2% (Employer)
│  └─ Deduction Type: Tax/Contribution
├─ Withholding Tax (BIR)
│  ├─ Formula: Based on tax tables (auto-calculated)
│  └─ Deduction Type: Tax
└─ Health Insurance Premium
   ├─ Amount: PHP 1,500/month (deducted from employee)
   └─ Deduction Type: Voluntary
```

**Pay Groups Included:**
```
Pay Group 1: "Regular Employees"
├─ Employee Type: Permanent, Full-time
├─ Pay Frequency: Monthly (on 25th)
├─ Includes: Basic Salary + All Allowances
├─ Deductions: PhilHealth, SSS, PagIBIG, Withholding Tax
├─ Formula Set:
│  ├─ Gross = Basic + DA + Transportation + Communication + Meals
│  ├─ Total Deductions = PhilHealth + SSS + PagIBIG + Tax + Insurance
│  └─ Net = Gross - Total Deductions
└─ Apply To: All employees with "Employee" role

Pay Group 2: "Probationary Period"
├─ Employee Type: Permanent, Full-time (Probation)
├─ Pay Frequency: Monthly (on 25th)
├─ Includes: Basic Salary + DA only (reduced benefits during probation)
├─ Deductions: PhilHealth, SSS, PagIBIG, Withholding Tax (same)
└─ Apply To: Employees in probation period (< 6 months)
```

**Customization Options:**
- ✏️ Adjust allowance amounts
- ✏️ Add/remove allowances (e.g., add "Shift Allowance" for night shifts)
- ✏️ Change deduction percentages
- ✏️ Add new pay groups (e.g., for contractors, interns)
- ✏️ Modify formulas

---

#### **Hourly Wage + Overtime Template**

**Pay Types Included:**
```
Hourly Wage
├─ Type: Hourly Rate (varies by employee)
├─ Formula: Hourly Rate × Hours Worked
└─ Pay Frequency: Weekly or Bi-weekly

Regular Hours
├─ Definition: First 8 hours/day or 40 hours/week
├─ Formula: Hourly Rate × Regular Hours
└─ Multiplier: 1x

Overtime (OT)
├─ Definition: Hours beyond 8/day or 40/week
├─ Formula: Hourly Rate × OT Hours × 1.5
└─ Multiplier: 1.5x

Holiday Work
├─ Definition: Work on declared holidays
├─ Formula: Hourly Rate × Holiday Hours × 2.0
└─ Multiplier: 2x (Regular Holiday), 3x (Special Holiday)

Night Shift Differential
├─ Definition: Work between 10 PM - 6 AM
├─ Formula: Hourly Rate × Night Hours × 1.1
└─ Multiplier: 1.1x

Deductions
├─ SSS Contribution
│  ├─ Formula: Based on salary bracket
│  └─ Deduction Type: Tax/Contribution
├─ PhilHealth
│  ├─ Formula: 3% of Total Earnings
│  └─ Deduction Type: Tax/Contribution
├─ PagIBIG
│  ├─ Formula: 1% of Total Earnings
│  └─ Deduction Type: Tax/Contribution
└─ Withholding Tax
   ├─ Formula: Based on tax tables
   └─ Deduction Type: Tax
```

**Pay Groups Included:**
```
Pay Group 1: "Hourly Workers - Regular Rate"
├─ Employee Type: Hourly, Full-time
├─ Pay Frequency: Weekly (every Friday)
├─ Includes: Regular Hours + OT + Night Shift Diff
├─ Deductions: SSS, PhilHealth, PagIBIG, Tax
├─ Formula Set:
│  ├─ Gross = (Regular Hours × Rate) + (OT × 1.5) + (Night Diff)
│  ├─ Total Deductions = SSS + PhilHealth + PagIBIG + Tax
│  └─ Net = Gross - Total Deductions
└─ Apply To: All hourly workers

Pay Group 2: "Part-Time Workers"
├─ Employee Type: Hourly, Part-time
├─ Pay Frequency: Weekly (every Friday)
├─ Includes: Regular Hours only (no OT multiplier)
├─ Deductions: Minimal (below benefit threshold)
└─ Formula: (Hours Worked × Rate) - Minimal Deductions
```

**Special Handling:**
- ✏️ Holiday multipliers (2x or 3x depending on holiday type)
- ✏️ Auto-calculate OT based on timekeeping data
- ✏️ Handle split shifts and night shift tracking

---

#### **Manufacturing & Shift Premiums Template**

**Pay Types Included:**
```
Basic Salary
├─ Type: Fixed Base Pay
├─ Formula: Employee's monthly salary
└─ Pay Frequency: Monthly

Shift Differential Allowances
├─ Morning Shift (6 AM - 2 PM)
│  ├─ Amount: PHP 2,000/month
│  └─ Condition: Only if assigned to morning shift
├─ Afternoon Shift (2 PM - 10 PM)
│  ├─ Amount: PHP 3,000/month (higher for evening)
│  └─ Condition: Only if assigned to afternoon shift
└─ Night Shift (10 PM - 6 AM)
   ├─ Amount: PHP 5,000/month (highest for night)
   ├─ Condition: Only if assigned to night shift
   └─ Automatic: Based on shift assignment

Overtime Premiums
├─ Daily OT (> 8 hrs/day)
│  ├─ Formula: Hourly Rate × OT Hours × 1.25
│  └─ Multiplier: 1.25x
├─ Weekly OT (> 40 hrs/week)
│  ├─ Formula: Hourly Rate × OT Hours × 1.5
│  └─ Multiplier: 1.5x
└─ Rest Day Work
   ├─ Formula: Hourly Rate × Hours × 1.5
   └─ Multiplier: 1.5x (or 2x for holidays)

Production Incentive Bonus (Optional)
├─ Type: Variable Bonus
├─ Formula: Units Produced × Rate per Unit
├─ Payment: Monthly, if production target met
└─ Customizable: Yes

Deductions
├─ SSS, PhilHealth, PagIBIG (standard)
├─ Union Dues (if applicable)
│  ├─ Amount: PHP 500/month
│  └─ Condition: Only for union members
└─ Loan Deductions
   ├─ Amount: Variable by employee
   └─ Deduction Type: Personal
```

**Pay Groups Included:**
```
Pay Group 1: "Morning Shift Workers"
├─ Shift Assigned: Morning (6 AM - 2 PM)
├─ Includes: Basic + Morning Allowance + OT + Bonus
├─ Formula:
│  ├─ Gross = Basic + Shift Allowance + OT Premium + Bonus
│  └─ Net = Gross - Deductions
└─ Automation: Shift allowance auto-applies based on shift assignment

Pay Group 2: "Afternoon Shift Workers"
├─ Shift Assigned: Afternoon (2 PM - 10 PM)
├─ Includes: Basic + Afternoon Allowance + OT + Bonus
└─ Shift Allowance: PHP 3,000/month

Pay Group 3: "Night Shift Workers"
├─ Shift Assigned: Night (10 PM - 6 AM)
├─ Includes: Basic + Night Allowance + OT + Bonus
├─ Shift Allowance: PHP 5,000/month
└─ Auto-Deduction: Deduct 10% for night work difficulty (optional)
```

---

#### **Commission-Based Template**

**Pay Types Included:**
```
Base Salary
├─ Type: Fixed Base
├─ Amount: Varies by employee (entry-level vs senior)
└─ Pay Frequency: Monthly

Commission
├─ Type: Variable, based on sales
├─ Formula: Total Sales × Commission Rate (e.g., 5-10%)
├─ Frequency: Monthly or Per-Transaction
└─ Threshold: Minimum sales to earn commission

Performance Bonus
├─ Type: Variable Bonus
├─ Formula: If sales > target → bonus
├─ Percentage: 10-50% of commission
└─ Condition: Only if commission >= threshold

Sales Incentive
├─ Type: Tiered Incentive
├─ Tier 1: Sales 0-100K → 5% commission
├─ Tier 2: Sales 100K-300K → 7% commission
├─ Tier 3: Sales 300K+ → 10% commission
└─ Calculation: Progressive (tiered)

Deductions
├─ SSS, PhilHealth, PagIBIG (standard)
├─ Sales Advance Recovery
│  ├─ Type: Loan repayment
│  └─ Amount: Variable
└─ Uniform/Equipment Fee
   ├─ Type: One-time or recurring
   └─ Amount: PHP 500/month
```

**Pay Groups Included:**
```
Pay Group 1: "Junior Sales Representatives"
├─ Base Salary: PHP 15,000/month
├─ Commission: 5% of net sales
├─ Performance Bonus: 10% if sales > PHP 200K
├─ Formula:
│  ├─ Gross = Base + Commission + Bonus
│  └─ Net = Gross - Deductions
└─ Target: PHP 200K/month

Pay Group 2: "Senior Sales Representatives"
├─ Base Salary: PHP 25,000/month
├─ Commission: 7% of net sales
├─ Performance Bonus: 15% if sales > PHP 500K
└─ Target: PHP 500K/month

Pay Group 3: "Sales Managers"
├─ Base Salary: PHP 40,000/month
├─ Commission: 3% (lower, they manage team commission)
├─ Team Bonus: 5% if team hits target
└─ Plus: Direct Reports = higher bonus multiplier
```

---

#### **Daily Rate + Incentives Template**

**Pay Types Included:**
```
Daily Rate Wage
├─ Type: Fixed Daily Rate
├─ Amount: Varies by employee (e.g., PHP 500-1000/day)
├─ Basis: Per day worked
└─ Calculation: Daily Rate × Days Worked

Overtime Allowance
├─ Definition: Work beyond 8 hours/day
├─ Formula: Hourly Rate × OT Hours × 1.5
└─ Automatic: Calculated from timekeeping

Holiday Work Allowance
├─ Definition: Work on declared holidays
├─ Formula: Daily Rate × 2.0 (or 3.0 for special holidays)
└─ Automatic: Based on holiday calendar

Productivity Bonus
├─ Type: Variable Incentive
├─ Formula: If productivity > target → bonus
├─ Amount: PHP 500-2000 depending on performance
└─ Condition: Manager-approved

Attendance Bonus
├─ Type: Incentive for perfect attendance
├─ Amount: PHP 1,000/month if no absences
├─ Condition: Full month with no late/absent days
└─ Frequency: Monthly

Deductions
├─ Minimal deductions (daily workers often below tax threshold)
├─ SSS (if earning > minimum wage)
├─ PhilHealth (mandatory)
└─ Voluntary Loans
   ├─ Type: Emergency/salary advance
   └─ Repayment: Deducted from salary
```

**Pay Groups Included:**
```
Pay Group 1: "Daily Wage Workers"
├─ Daily Rate: PHP 600-800 (customizable by employee)
├─ Pay Frequency: Daily, Weekly, or Bi-weekly
├─ Includes: Daily Rate + OT + Holiday Pay + Bonuses
├─ Deductions: Minimal (only if above threshold)
├─ Formula:
│  ├─ Gross = (Daily Rate × Days Worked) + OT + Holiday + Bonus
│  └─ Net = Gross - Deductions
└─ Flexibility: No fixed shift, work as assigned

Pay Group 2: "Project-Based Workers"
├─ Daily Rate: PHP 800-1000 (project-dependent)
├─ Pay Frequency: Per project completion
├─ Includes: Daily Rate + Completion Bonus
├─ Bonus: 10% if project completed on-time
└─ Customizable: By project type
```

---

#### **Flexible Remuneration Template**

**Pay Types Included:**
```
Base Salary
├─ Type: Fixed
├─ Amount: Employee's base salary
└─ Frequency: Monthly

Flexible Allowances (Employee Chooses)
├─ Transportation Allowance
│  ├─ Option 1: PHP 3,000/month (cash)
│  ├─ Option 2: Free public transit card
│  └─ Employee Selection: Self-service
├─ Communication Allowance
│  ├─ Option 1: PHP 2,000/month (cash)
│  ├─ Option 2: Corporate phone + plan
│  └─ Employee Selection: Self-service
├─ Meal Allowance
│  ├─ Option 1: PHP 3,000/month (cash)
│  ├─ Option 2: Meal vouchers (cafeteria)
│  └─ Employee Selection: Self-service
└─ Health & Wellness
   ├─ Option 1: Gym membership (PHP 2,500)
   ├─ Option 2: Health insurance premium (PHP 3,500)
   └─ Employee Selection: Self-service

Professional Development
├─ Training Budget: PHP 10,000/year
├─ Conference Allowance: PHP 5,000/year
└─ Employee Request: Submit for approval

Deductions
├─ SSS, PhilHealth, PagIBIG (standard)
└─ Benefits Premium (if selected by employee)
```

**Pay Groups Included:**
```
Pay Group 1: "Tech Team - Flexible Comp"
├─ Base Salary: PHP 60,000/month
├─ Flexible Allowances: Total PHP 8,000 pool (employee chooses mix)
├─ PTO: Unlimited (with guidelines)
├─ Professional Development: PHP 10,000/year
├─ Formula:
│  ├─ Gross = Base + Selected Allowances
│  └─ Net = Gross - Standard Deductions
└─ Customization: Annual review

Pay Group 2: "Executive - Premium Flex"
├─ Base Salary: PHP 120,000/month
├─ Flexible Allowances: Total PHP 15,000 pool
├─ Car Allowance: PHP 20,000/month (optional)
├─ Professional Development: PHP 20,000/year
└─ Stock Options: (Optional, external system)
```

---

## Step 5b: Post-Template Customization

After selecting a payroll template, users see:

### Customization Checklist

**Section 1: Basic Settings**
- [ ] Pay Frequency (Monthly, Bi-weekly, Weekly)
- [ ] Pay Cycle Start Date
- [ ] Salary Payment Method (Bank Transfer, Check, Cash)
- [ ] Pay Day (e.g., 25th of month)

**Section 2: Pay Types Review**
- ✏️ Edit: Adjust allowance amounts
- ✏️ Add: New pay types (e.g., special bonus)
- ✏️ Delete: Remove unused pay types
- ✏️ View Formula: Show how each pay type is calculated

**Section 3: Pay Groups Review**
- ✏️ Edit: Modify pay group names, criteria
- ✏️ Add: Create custom pay groups (e.g., "Interns", "Contractors")
- ✏️ Delete: Remove unused pay groups
- ✏️ Assign: Manually assign employees to pay groups (or auto-assign based on role)

**Section 4: Deductions Review**
- ✏️ Verify: SSS, PhilHealth, PagIBIG rates
- ✏️ Add: Additional deductions (loans, cooperative, etc.)
- ✏️ Customize: Tax withholding settings per employee

**Section 5: Compliance Settings**
- [ ] BIR Registration Number
- [ ] SSS Employer Number
- [ ] PhilHealth Code
- [ ] PagIBIG Account Number
- [ ] Tax Compliance Mode (13th Month, Overtime Tracking, etc.)

**Section 6: Formula Validation**
- ✅ System validates all formulas
- ✅ Shows warnings if formulas are missing
- ✅ Provides formula templates for manual entry

### Preview & Confirmation

**Sample Payroll Calculation** (using template configuration):
```
Employee: Maria Santos
Pay Group: Regular Employees
Pay Period: September 1-30, 2026

EARNINGS
├─ Basic Salary: PHP 30,000
├─ Dearness Allowance: PHP 5,000
├─ Transportation: PHP 3,000
├─ Communication: PHP 2,000
├─ Meal Vouchers: PHP 2,000
├─ Overtime (5 hrs × PHP 250/hr × 1.5): PHP 1,875
├─ Bonus (attendance): PHP 1,000
└─ **TOTAL GROSS**: PHP 44,875

DEDUCTIONS
├─ PhilHealth (3%): PHP 1,346
├─ SSS Contribution: PHP 1,575
├─ PagIBIG (1%): PHP 449
├─ Withholding Tax (BIR): PHP 2,850
├─ Health Insurance: PHP 1,500
├─ Loan Repayment: PHP 2,000
└─ **TOTAL DEDUCTIONS**: PHP 9,720

**NET PAY**: PHP 35,155
```

**User Actions:**
- ✅ Confirm Setup (proceed to next step)
- ✏️ Edit Formula (if not satisfied with calculation)
- ❓ Get Help (video tutorial or live chat)

---

## Visual Design Mockup

### Wizard Header
```
┌─────────────────────────────────────────────────┐
│  Kando Onboarding Wizard                        │
│  ═════════════════════════════════════════════  │
│  Step 5 of 8: Payroll Configuration             │
│                                                  │
│  [████████░░░░░░░░░░] 62.5% Complete           │
└─────────────────────────────────────────────────┘
```

### Template Selection Card
```
┌─────────────────────────────────────────────┐
│  📋 Select Your Payroll Template             │
│  (Pre-configured with best practices)        │
│                                              │
│  ○ Fixed Salary + Benefits                  │
│    └─ For: Corporate, Salaried employees    │
│       Includes: Basic + Allowances           │
│       Deductions: Tax, SSS, PhilHealth       │
│                                              │
│  ◉ Hourly Wage + Overtime                   │
│    └─ For: Retail, Hourly workers           │
│       Includes: Hourly + OT Premium         │
│       Deductions: Tax, SSS, PhilHealth       │
│                                              │
│  ○ Manufacturing & Shift Premiums            │
│    └─ For: Factory, Multi-shift operations  │
│       Includes: Base + Shift Differential    │
│       Deductions: Tax, Union Dues            │
│                                              │
│  ○ Commission-Based                          │
│    └─ For: Sales teams                       │
│       Includes: Base + Commission + Bonus    │
│       Deductions: Standard                    │
│                                              │
│  [Learn More ↗] [Continue →]                │
└─────────────────────────────────────────────┘
```

### Customization Screen
```
┌──────────────────────────────────────────────────┐
│  ✏️  Customize Your Payroll Template             │
│                                                   │
│  PAY TYPES                                       │
│  ├─ Basic Salary        ✏️ Delete              │
│  ├─ Dearness Allowance  ✏️ Edit: PHP 5,000     │
│  ├─ Transportation      ✏️ Edit: PHP 3,000     │
│  ├─ [+ Add Pay Type]                           │
│                                                   │
│  PAY GROUPS                                      │
│  ├─ Regular Employees       [Apply] ✏️ Edit    │
│  ├─ Probationary Employees  [Apply] ✏️ Edit    │
│  ├─ [+ Add Pay Group]                          │
│                                                   │
│  DEDUCTIONS                                      │
│  ├─ PhilHealth (3%)        ✏️ Edit              │
│  ├─ SSS Contribution       ✏️ Edit              │
│  ├─ PagIBIG (1%)           ✏️ Edit              │
│  ├─ Withholding Tax (Auto) [View Formula]      │
│  ├─ Health Insurance       ✏️ Edit: PHP 1,500  │
│                                                   │
│  FORMULA PREVIEW                                │
│  │ Net Pay = (Basic + Allowances) - Deductions │
│                                                   │
│  [← Back]  [Skip Customization]  [Continue →]  │
└──────────────────────────────────────────────────┘
```

---

## Benefits of Template-Based Wizard

| Benefit | Impact | Users Affected |
|---------|--------|-----------------|
| **Faster Setup** | Reduce implementation from 4-6 weeks to 2-3 days | All new organizations |
| **Error Prevention** | Pre-validated configurations prevent common mistakes | HR admins, CFOs |
| **Better UX** | Intuitive choices vs. manual form filling | Less technical users |
| **Compliance Ready** | Templates include PH tax/compliance rules out-of-box | HR/Compliance teams |
| **Reduced Support** | Fewer configuration questions to support team | Support team, customers |
| **Higher Activation** | Users see working payroll on day 1 | Product adoption |

---

## Technical Implementation Notes

### Backend Considerations
- **Template Library**: Store templates in database with versioning
- **Formula Engine**: Validate all formulas before saving
- **Employee Assignment**: Bulk assign employees to pay groups based on criteria
- **Tax Calculator**: Use government tax tables (BIR, SSS rates)
- **Automation**: Auto-detect company size/industry to recommend templates

### Frontend Considerations
- **Wizard State Management**: Track step progress, allow back/forward navigation
- **Inline Editing**: Edit configuration without leaving wizard
- **Real-Time Validation**: Show errors/warnings as user customizes
- **Preview**: Show sample payroll calculation before confirming
- **Mobile Responsive**: Ensure wizard works on tablets/phones

### Data Validation
- ✅ All mandatory fields filled before proceeding
- ✅ Formula syntax validated
- ✅ Deduction percentages reasonable (e.g., tax % between 0-15%)
- ✅ At least one pay type selected
- ✅ At least one pay group created

---

## Success Metrics

After implementing this wizard:

| Metric | Target | Measurement |
|--------|--------|-------------|
| **Implementation Time** | 2-3 days (vs 4-6 weeks) | Time from signup to first payroll run |
| **Configuration Errors** | < 5% | % of orgs that need post-setup corrections |
| **Wizard Completion Rate** | > 85% | % of orgs completing full wizard |
| **First Payroll Success** | > 90% | % of first payroll runs with < 3 errors |
| **Support Tickets** | -30% reduction | Payroll setup-related tickets decrease |
| **Customer Activation** | +40% | Active users after setup week 1 |
| **NPS Impact** | +15 points | Easier setup improves satisfaction |

---

## Rollout Plan

### Phase 1: MVP (Week 1-2)
- **Timekeeping Template Selection** (3-4 templates)
- **Payroll Template Selection** (3 templates: Fixed Salary, Hourly, Shift Premium)
- **Basic Customization** (edit amounts, add deductions)

### Phase 2: Enhanced (Week 3-4)
- **Expansion: 6 Payroll Templates** (add Commission, Daily Rate, Flexible)
- **Advanced Customization**: Formula builder with validation
- **Employee Bulk Assignment**: Auto-assign based on criteria

### Phase 3: Premium (Week 5-6)
- **AI Recommendations**: Auto-detect company size/industry
- **Industry-Specific Templates**: Healthcare, Retail, Manufacturing deep dives
- **Configuration Cloning**: Copy settings from similar organizations

---

**Document Version**: 1.0  
**Status**: Ready for Design Review  
**Next Step**: UX Designer builds mockups & prototype  
