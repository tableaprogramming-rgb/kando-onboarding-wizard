# Kando Onboarding Wizard - Discovery Analysis Summary
## From Responses to MVP Recommendations

**Document Purpose**: Bridge discovery questions to final MVP scope and wireframe priorities  
**Status**: Analysis complete, ready for sprint kickoff  
**Created**: September 2026

---

## Discovery Responses: The Key Insights

### What We Learned (Directly from Team Answers)

#### Problem & Context ✅ Clear
- **Pain Point**: New customers struggle with Kando complexity → 2-week support-heavy onboarding
- **Root Cause**: No templated setups, no guidance → customers ask basic HR questions
- **Opportunity**: Pre-built templates + wizard guidance → reduce support load, faster activation
- **Source**: Support team + Sales (both requesting this feature)

#### Target Audience ✅ Clear
- **Primary User**: HR admin/people responsible for org setup (not sales, not support)
- **Target Company Size**: 10-20 employees (initially; expand later)
- **Use Case**: Net-new customers (not existing customers reconfiguring)
- **Timeline Goal**: 2-3 days setup (vs current 2 weeks)

#### Current State ✅ Clear
- **Timekeeping Templates**: Don't exist (all new functionality)
- **Payroll Templates**: Don't exist (all new functionality)
- **Compliance Data**: Pre-built in Kando (BIR, SSS, PhilHealth, PagIBIG)
  - Will pre-populate in templates for convenience
- **Employee Creation**: Separate from wizard (not part of this feature)

#### Unknowns ⚠️ (Answered with Recommendations)

| Question | Discovery Answer | MVP Recommendation | Rationale |
|----------|------------------|-------------------|-----------|
| Which payroll templates first? | Not answered | 2 templates: Fixed Salary + Hourly | Covers 80% of small biz; others in Phase 2 |
| Customization in MVP? | Uncertain | Edit amounts only; formula builder Phase 2 | Keeps MVP focused; 80% of users only change numbers |
| Post-wizard editing? | Not answered | Allow editing anytime | Lowest risk, most flexible, meets customer needs |
| Prevent editing after? | Not answered | No restrictions | Users may need to adjust; adding locks over-engineers |
| Basic salary in wizard? | Not answered | Read-only example; set per-employee | Keeps template focused on structure, not individuals |
| Custom pay types? | Not answered | Allow via "Add Custom" button | Gives flexibility without formula complexity |
| Employee CSV import? | Separate feature | Post-wizard (not in wizard) | Keeps concerns separated; simplifies MVP scope |
| Which timekeeping template? | Not answered | 1 template: Standard Office Hours | Covers 80% of small companies; complex shifts Phase 2 |
| Industries prioritized? | Not specified | No prioritization in MVP | All templates work for any industry; industry selection for Phase 2+ analytics |

---

## MVP Decisions: Why These Choices

### Single Timekeeping Template (Standard Office Hours)
**Why not offer template selection in MVP?**
- Small target companies (10-20 employees) almost all use standard office hours
- Hardcoding to 8 AM - 5 PM removes a decision point → faster setup
- Complex shifts (3-shift factory, 12-hr healthcare) are < 10% of target customers
- **Phase 2:** Add template selection when we have manufacturing/retail customers

**Impact:**
- Wireframe 3 becomes configuration screen (not selection screen)
- No "Which template?" modal
- Faster flow (1 step instead of 2)

### Two Payroll Templates (Not Six)
**Why not all 6 templates in MVP?**
- Fixed Salary + Hourly cover ~80% of small business payroll structures
- Other 4 templates (commission, daily rate, manufacturing, flexible) are edge cases
- Building templates for edge cases = slower MVP, more testing, less time for bugs

**Phase 1 MVP:**
- Fixed Salary + Benefits (corporate salaried employees)
- Hourly Wage + Overtime (retail, hourly workers)

**Phase 2+ (Expand):**
- Manufacturing & Shift Premiums (factory, 3-shift)
- Commission-Based (sales teams)
- Daily Rate + Incentives (construction, logistics)
- Flexible Remuneration (tech, startups)

**Impact:**
- Wireframe 4 shows 2 cards (not 6)
- Fewer integration points, fewer formulas to validate
- Faster to get to beta testing

### Basic Customization Only (Amounts, No Formulas)
**Why not formula builder in MVP?**
- 80% of small businesses only change: salary amounts, allowance amounts, pay frequency
- Formula builder requires UI + validation + error messaging (complexity)
- Most customers use pre-built templates as-is
- **Phase 2:** Add formula builder when we see demand

**What IS customizable in MVP:**
- Pay types: Edit amount (PHP 5,000 → PHP 3,000)
- Deductions: Toggle (add health insurance: yes/no)
- Pay frequency: Dropdown (monthly vs weekly)
- Pay day: Date picker (25th vs last day)

**What's NOT customizable:**
- Formula logic (e.g., "SSS = 11.5% of salary" is pre-built, not editable)
- Tax calculation (BIR tables pre-loaded, locked for compliance)
- Structure (pay groups, deduction types are from template, can't change structure)

**Impact:**
- Wireframe 5A/5B = simpler, faster to build
- No formula validator needed
- Error surface much smaller

---

## Data Dependencies & Technical Implications

### Wizard State Flow
```
Wireframe 1: Welcome
  ↓ (no data collected)
  
Wireframe 2: Organization Profile
  ├─ Input: Company name, employee count, industry, timezone
  └─ Used by: Wireframes 3-6 (for defaults, auto-recommendations)
  
Wireframe 3: Timekeeping Config
  ├─ Input: Shift times, break duration, grace period, OT threshold
  ├─ Depends on: Organization profile (timezone)
  └─ Used by: Wireframe 5B (OT calculation preview)
  
Wireframe 4: Payroll Template Selection
  ├─ Input: Selected template ("Fixed Salary" or "Hourly Wage")
  ├─ Routes to: Wireframe 5A or 5B
  └─ Affects: Which customization screen user sees
  
Wireframe 5A/5B: Customize Payroll
  ├─ Input: Pay types amounts, pay frequency, deductions
  ├─ Depends on: Government rates (pre-loaded: BIR, SSS, PhilHealth, PagIBIG)
  ├─ Depends on: Selected template (determines default structure)
  └─ Output: Complete payroll configuration
  
Wireframe 6: Confirmation
  ├─ Input: User confirmation checkbox
  └─ Output: Save all wizard data to DB
```

### Backend Requirements (MVP Minimal)
- **Template Storage**: Store Fixed Salary + Hourly templates with versioning
- **Government Rates**: Pre-load BIR, SSS, PhilHealth, PagIBIG (update quarterly)
- **Wizard State Store**: Save progress after each step (auto-save)
- **Validation Engine**: Validate formulas (only for read-only formulas, not custom)
- **Payroll Preview**: Calculate sample payroll based on template values

**NOT needed in MVP:**
- Template cloning
- AI-powered recommendations
- Formula builder validation
- Multi-template timekeeping engine
- Advanced deduction rules

---

## Success Criteria: How We Know MVP Worked

### Launch Criteria (Must Have)
- ✅ All 6 wireframes fully functional end-to-end
- ✅ Wizard state persists (can return later, pick up where left off)
- ✅ Sample payroll calculation accurate for both templates
- ✅ Mobile responsive (tablet/phone tested)
- ✅ No hard-stop bugs

### Quality Metrics
- ✅ Accessibility: WCAG 2.1 AA (keyboard nav, screen reader support)
- ✅ Performance: < 2 second page load on 4G
- ✅ Error handling: Clear validation messages for all fields
- ✅ Mobile: Works on iOS Safari, Android Chrome

### Business Impact (Post-Launch)
- Reduce avg. onboarding support time from 2 weeks to 3-4 days
- > 70% of new customers complete wizard without contacting support
- < 5% of completed setups need post-launch corrections
- Collect customer feedback to validate Phase 2 templates

---

## What Customers Will See (MVP User Journey)

### Happy Path (Ideal Case)
```
1. User signs up → "Start Setup" button
2. Fill org profile (name, size, timezone) → 2 min
3. Review recommended timekeeping → Accept defaults → 1 min
4. Choose payroll template → Select "Fixed Salary" → 1 min
5. Customize amounts (optional) → Change DA from 5K to 3K → 5 min
6. Review sample payroll → "Looks good!" → 2 min
7. Confirm → Setup complete → 1 min

Total: ~12 minutes from signup to payroll ready
```

### Real-World Case (With Customization)
```
1-4. Same as above → ~4 min
5. Customize payroll (add health insurance, change pay day) → 10 min
6. Review sample payroll → "Need to adjust SSS" → Ask support
7. (Support helps via chat) → Adjust and confirm → 15 min

Total: ~30 min with support interaction (vs 2 weeks without wizard)
```

### What Support Will See (Post-MVP)
- **Before Wizard**: Customer calls → "How do I set up payroll?" → 2-week guidance
- **After Wizard**: Customer calls → "Sample payroll shows PHP 35K, but we want PHP 38K" → 5-min fix

→ Support still needed, but for customization, not basic setup

---

## Open Questions: Final Clarifications

### Must Answer Before Design Freeze
1. **Post-wizard editing**: Can users edit payroll configs after wizard confirmation?
   - **Assume for MVP**: Yes, allow editing anytime
   
2. **Basic salary field**: Should wizard collect sample salary, or just show example?
   - **Assume for MVP**: Read-only example in preview (actual salary set per-employee)

3. **Custom pay types**: Can users add brand-new pay types, or only modify template amounts?
   - **Assume for MVP**: Yes, allow "Add Custom Pay Type" with simple form (name + amount)

### Good to Know (Can Clarify in Week 1)
4. **Timezone impact**: Is timezone for display only, or does it affect tax/compliance calculations?
   - **Assume for MVP**: Display only (all companies in PH, same tax rules)

5. **Multi-organization**: Can users set up multiple orgs in one wizard session?
   - **Assume for MVP**: One wizard run = one org; create another org as separate flow

6. **Government rate updates**: Who maintains BIR/SSS/PhilHealth rates? How often updated?
   - **Assume for MVP**: Engineering maintains; reviewed quarterly

---

## Discovery to Wireframe Mapping

| Discovery Response | → | MVP Decision | → | Wireframe Impact |
|-------------------|---|--------------|---|------------------|
| Support team pain: too much onboarding | | Focus on template guidance + preview | | Wireframes 4-6: template selection + customization + preview |
| "Ready-made setups" needed | | Pre-built payroll + timekeeping templates | | Wireframes 4-5: Show template contents before user customizes |
| Basic HR process guidance needed | | Provide defaults, tooltips, help links | | Wireframe 5A/5B: Explanations for deductions, formulas |
| Target: 10-20 employees | | Use Standard Office Hours only | | Wireframe 3: Simplified timekeeping (no multi-shift) |
| 2-week setup → 2-3 days goal | | Fast flow (6 screens, 15-20 min to complete) | | Wireframes 1-6: Progressive disclosure, not overwhelming |
| Compliance data pre-coded in Kando | | Pre-populate templates with government rates | | Wireframe 5A/5B: Read-only deduction reference (government-managed) |
| Employee creation separate | | Don't include in wizard | | Post-setup checklist in Wireframe 6 (link to employee import) |
| New customers priority | | Wizard shown on first login | | Wireframe 1: Welcome screen for new orgs only |

---

## Why These Wireframes Won't Change Post-Discovery

Each wireframe directly maps to a discovery need:

1. **Wireframe 1 (Welcome)**: Customers don't know what to expect → Orient them
2. **Wireframe 2 (Org Profile)**: Need to capture basics → Simple form
3. **Wireframe 3 (Timekeeping)**: Customers confused by shift config → Provide defaults
4. **Wireframe 4 (Template Selection)**: Too many choices overwhelm → Show 2 templates only
5. **Wireframe 5A/5B (Customization)**: Users need to adjust values → Editable amounts with preview
6. **Wireframe 6 (Confirmation)**: Users want assurance before saving → Summary + checklist

---

## Timeline: Recommendation for Kickoff

### Recommended Sprint Start
- **Week 1**: Design (Wireframes 1-4, high-fidelity mockups) + Backend setup
- **Week 2**: Frontend build + Integration
- **Week 3 (Optional)**: Buffer for bugs, polish, testing

### Team Sizing
- **1 UX Designer** (Design Wireframes 1-6, iterate on feedback)
- **1-2 Frontend Engineers** (Vue 3 implementation, component building)
- **1 Backend Engineer** (Pinia state management, API endpoints, template storage)

### Go/No-Go Decision
- **After Week 2**: Decide if MVP is shippable, or if more time needed
- **Ship to**: 3-5 beta customers (internal + trusted partners)
- **Collect**: Feedback on templates, customization, UX flow
- **Plan**: Phase 2 based on feedback

---

## Assumption: Why We're Confident This MVP Will Work

1. **Validates the Core Insight**: The "template-based wizard" approach is unproven; MVP tests if customers accept pre-built setups without customization
   - If YES → Expand templates in Phase 2
   - If NO → Pivot to more customization in wizard (not formula builder, but more flexibility)

2. **Minimum Viable Complexity**: By focusing on 1 timekeeping + 2 payroll templates, we can test the wizard in 2 weeks vs 4-6 weeks if we built all templates
   - Faster feedback loop
   - Identify blockers early
   - Smaller scope = higher quality MVP

3. **Covers Target Customer**: 10-20 person companies in Philippines mostly:
   - Use standard office hours (8-5)
   - Are either salaried (fixed) or hourly (retail/services)
   - Want compliance automation (BIR, SSS pre-built)
   - Need support but appreciate being handed defaults to customize

4. **Clear Deferral Strategy**: Phase 2 is well-defined (4 more templates, advanced customization), so team knows roadmap isn't a void → confidence the MVP is truly "minimal"

---

## Next: From This Document to Sprinting

**For the Team to Review:**
1. **Technical Leads**: Review data dependencies map, backend requirements
2. **Design Lead**: Review all 6 wireframes, confirm component system needs
3. **Product Manager**: Answer the 3 "Must Answer" questions, confirm target customers
4. **Engineering Manager**: Confirm team sizing, timeline, dependencies

**Before Design Kickoff:**
- [ ] All answers to critical questions documented
- [ ] Figma file created (component library started)
- [ ] Repo structure set up (frontend + backend)
- [ ] DB schema drafted (wizard state, templates)
- [ ] Team alignment on success metrics

---

**Document Version**: 1.0  
**Created**: September 2026  
**Status**: Ready for Team Review  
**Next Step**: Schedule kickoff meeting, confirm critical question answers
