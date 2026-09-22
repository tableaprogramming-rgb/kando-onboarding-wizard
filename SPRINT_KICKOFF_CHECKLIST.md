# Kando Wizard MVP - Sprint Kickoff Checklist
## Use This to Launch the 2-Week Sprint

---

## 🎯 Pre-Kickoff (Complete These Before Meeting)

### Product/PM
- [ ] **Confirm critical decisions** (from DISCOVERY_ANALYSIS_SUMMARY.md):
  - [ ] Post-wizard editing: allowed or locked?
  - [ ] Basic salary in wizard: editable or example?
  - [ ] Custom pay types: in MVP or Phase 2?
- [ ] **Confirm target customer**: 10-20 employees, Philippines, new organizations
- [ ] **Confirm Phase 2 templates**: Commission, Daily Rate, Manufacturing, Flexible
- [ ] **Get approval** from leadership for 2-week timeline + team sizing

### Design Lead
- [ ] **Read** `MVP_WIREFRAME_PRIORITIES.md` (full document)
- [ ] **Download/set up** Figma file for component library
- [ ] **Identify** existing Kando design system (colors, typography, components)
- [ ] **Prepare** list of questions for kickoff (interactions, animations, accessibility)

### Tech Lead / Engineering Manager
- [ ] **Read** `MVP_WIREFRAME_PRIORITIES.md` (Sections 1-3, 5-6)
- [ ] **Review** data dependencies map (Section 3)
- [ ] **Confirm** team: 1–2 frontend engineers + 1 backend engineer (+ design + PM)
- [ ] **Check** project dependencies: Vue 3 version, Pinia, Axios, Vuelidate
- [ ] **Plan** repo structure: create feature branch for wizard

### All Team Members
- [ ] **Read** appropriate sections for your role (see README.md navigation)
- [ ] **Review** visual artifact: https://claude.ai/code/artifact/11bbfa9a-2077-4bc5-8965-aeff549a9bd7

---

## 📋 Kickoff Meeting Agenda (90 minutes)

### Opening (10 min)
- **Overview**: This 2-week sprint delivers a wizard to reduce onboarding from 2 weeks to 3-4 days
- **Scope**: 6 wireframes, 2 payroll templates, 1 timekeeping template
- **Success**: MVP validates template approach; Phase 2 expands with feedback

### Context (10 min)
- **Problem**: Support spends 2 weeks guiding each new org through payroll setup
- **Target**: Small companies (10-20 employees) that need HR process guidance
- **Solution**: Pre-built templates + wizard = guided setup in 3-4 days
- **Ask**: Does everyone understand the problem we're solving?

### Scope Review (15 min)
- **Walk through** visual artifact: 6 wireframes, timeline, success metrics
- **What IS in MVP**: 1 timekeeping template, 2 payroll templates, basic customization
- **What's NOT**: Employee import, leave policies, formula builder, all 6 templates
- **Ask**: Any scope concerns? Should we cut or add anything?

### MVP Decisions Explained (10 min)
- **Why 1 timekeeping template?** (10-20 employee companies mostly use standard hours; complex shifts Phase 2)
- **Why 2 payroll templates?** (Fixed Salary + Hourly cover 80%; others in Phase 2)
- **Why customization amounts only?** (80% of users just adjust numbers, not formulas)
- **Critical questions answered**: Post-wizard editing (allow), basic salary (example only), custom pay types (yes, simple form)
- **Ask**: Do you agree with these trade-offs?

### Wireframe Deep Dive (15 min)
- **Show** Wireframe 1 (Welcome) → Purpose: Orient users
- **Show** Wireframe 2 (Org Profile) → Purpose: Capture basics, auto-recommendations
- **Show** Wireframes 3-6 (Config → Customization → Confirmation) → Purpose: Guided setup with preview
- **Data flow**: Show how each screen builds on previous (diagram from Section 3)
- **Ask**: Any wireframes unclear? Design concerns?

### Timeline & Tasks (15 min)
- **Week 1, Days 1-2**: Discovery & Planning (design kickoff, repo setup, DB schema)
- **Week 1, Days 3-4**: Wireframes 1-2 (design + frontend + state mgmt)
- **Week 1, Day 5**: Wireframe 3 (design + timekeeping form)
- **Week 2, Days 6-7**: Wireframes 4-5 (template selection + customization)
- **Week 2, Days 8-10**: Integration & Testing (end-to-end, mobile, accessibility)
- **Week 2, Days 11-14**: Buffer & Handoff
- **Assign roles**: Who's leading design? Frontend? Backend? QA?

### Open Questions (10 min)
- **Run through** 3 critical questions (already answered; confirm team agrees)
- **Ask**: Any blockers? Dependencies? Risks?
- **Decision points**: 
  - Do we need to build employee bulk import as part of wizard? (No—post-wizard)
  - Do we need advanced deduction rules? (No—Phase 2)
  - Do we need multi-currency support? (No—Phase 1 Philippines only)

### Success Criteria (5 min)
- **Launch Criteria**: All 6 wireframes working, mobile responsive, no hard-stop bugs
- **Quality**: WCAG AA accessibility, < 2s load on 4G, clear error messages
- **Business Impact**: > 85% wizard completion, < 5% post-setup errors
- **Data Collection**: How we'll measure success post-launch

### Close & Confirmations (5 min)
- **Confirm**: Everyone knows their role and what they're building
- **Confirm**: Timeline is realistic (2 weeks is tight, but achievable with focus)
- **Confirm**: We have no blockers to starting Monday
- **Next meeting**: Daily standups (15 min) at [time], design review end of Day 4

---

## ✅ Immediate Action Items (Assign Owners)

| Task | Owner | Due | Notes |
|------|-------|-----|-------|
| Answer 3 critical questions | PM | Before Kickoff | See DISCOVERY_ANALYSIS_SUMMARY.md |
| Confirm team (design, front-end, backend) | Tech Lead | Before Kickoff | 1 designer + 1-2 engineers + PM |
| Create Figma file with component library | Design | Day 1 | Reference existing Kando design system |
| Set up feature branch + repo structure | Frontend Lead | Day 1 | Vue 3 + Pinia + Axios |
| Design DB schema for wizard state + templates | Backend Lead | Day 1 | Template versioning + progress storage |
| High-fidelity mockups: Wireframes 1-3 | Design | End of Day 4 | Get feedback before frontend builds |
| Build Pinia store for wizard state | Frontend | Days 3-4 | Auto-save progress after each step |
| Implement timekeeping template model | Backend | Days 3-4 | Pre-load government rates (BIR, SSS, etc.) |
| Schedule design review with team | PM | End of Day 4 | Frontend waiting on mockups to build |

---

## 🔧 Technical Setup Checklist

### Frontend Setup (Vue 3 + Pinia)
- [ ] Create feature branch: `feature/onboarding-wizard`
- [ ] Create folder structure:
  ```
  src/components/wizard/
    ├── WizardWelcome.vue
    ├── WizardOrgProfile.vue
    ├── WizardTimekeeper.vue
    ├── WizardPayrollSelect.vue
    ├── WizardPayrollCustomize.vue
    ├── WizardConfirmation.vue
    └── WizardContainer.vue (main wizard wrapper)
  
  src/stores/
    └── wizardStore.js (Pinia store: state + actions + getters)
  
  src/pages/
    └── Wizard.vue (router page)
  ```
- [ ] Add routes to wizard pages (Vue Router config)
- [ ] Set up form validation (Vuelidate or similar)
- [ ] Create reusable form components (input, dropdown, time picker, etc.)
- [ ] Plan responsive breakpoints (mobile, tablet, desktop)

### Backend Setup
- [ ] Database migrations for:
  - `wizard_progress` table (user_id, step, state, created_at, updated_at)
  - `payroll_templates` table (id, name, pay_types, pay_groups, deductions, version)
  - `timekeeping_templates` table (id, name, shift_times, breaks, policies, version)
  - `government_rates` table (year, rate_type, value, last_updated)
- [ ] API endpoints:
  - `POST /api/wizard/start` → Create wizard session
  - `POST /api/wizard/step/:step` → Save step progress
  - `GET /api/wizard/state` → Load wizard state
  - `POST /api/wizard/complete` → Finalize setup
  - `GET /api/templates/payroll` → List payroll templates
  - `GET /api/templates/timekeeping` → List timekeeping templates
  - `GET /api/government-rates` → Load BIR, SSS, etc.
- [ ] Sample payroll calculator (engine to compute gross/net based on template + values)

### Design System Audit
- [ ] Document existing colors, typography, spacing scale
- [ ] Identify which components exist (buttons, inputs, modals, etc.)
- [ ] Note any accessibility issues with existing components
- [ ] Create design tokens file (CSS variables or design system export)

---

## 📅 Week 1 Daily Check-In Questions

### Monday (Days 1-2): Planning & Setup
- [ ] Design: Figma file created, component library started?
- [ ] Frontend: Repo structure set up, routing configured?
- [ ] Backend: DB schema drafted, API endpoints planned?
- [ ] **Blocker?** Any setup issues preventing progress?

### Tuesday-Wednesday (Days 3-4): Wireframes 1-2 & Mockups
- [ ] Design: High-fidelity mockups for Wireframes 1-2 ready for review?
- [ ] Frontend: Wireframe 1 (Welcome) buildable from mockups?
- [ ] Frontend: Pinia store for wizard state working (auto-save test)?
- [ ] Backend: Government rates loaded into DB?
- [ ] **Blocker?** Any design questions blocking development?

### Thursday (Day 5): Wireframe 3
- [ ] Design: Wireframe 3 (Timekeeping) mockup done?
- [ ] Frontend: Wireframes 1-2 functional, can save state?
- [ ] Frontend: Time picker component built/integrated?
- [ ] Backend: Timekeeping template model ready?
- [ ] **Blocker?** Still on track for 2-week finish?

### Friday (End of Week 1): Design Review
- [ ] Design: All Wireframes 1-4 mockups reviewed by team?
- [ ] Frontend: Wireframes 1-3 built and working?
- [ ] Backend: API endpoints tested, sample data working?
- [ ] **Demo**: Show working wizard progress to team
- [ ] **Adjust**: Any scope cuts needed to stay on schedule?

---

## 📅 Week 2 Daily Check-In Questions

### Monday-Tuesday (Days 6-7): Wireframes 4-5
- [ ] Frontend: Wireframes 4 (template select) + 5A/5B (customize) built?
- [ ] Backend: Payroll template models loaded, sample preview working?
- [ ] Integration: Can user select template and see customization form?
- [ ] **Blocker?** Any API integration issues?

### Wednesday-Thursday (Days 8-10): Integration & Testing
- [ ] End-to-end: Full wizard flow works (Wireframe 1 → 6)?
- [ ] Mobile: Responsive tested on iPad + phone?
- [ ] Accessibility: Keyboard nav + screen reader tested?
- [ ] Validation: Form errors clear and helpful?
- [ ] **Bugs**: Track critical vs nice-to-have fixes

### Friday (Days 11-12): Buffer & Fixes
- [ ] All critical bugs fixed?
- [ ] Performance checked (< 2s load time)?
- [ ] Documentation started (component stories, API docs)?
- [ ] **Go/No-Go**: Is MVP shippable?

### Monday-Tuesday (Days 13-14): Handoff & Docs
- [ ] Documentation complete (components, API, setup guide)?
- [ ] Demo prepared for stakeholders?
- [ ] Phase 2 planning started (identify needed templates)?
- [ ] **Launch**: Deploy to staging or production?

---

## 🚨 Red Flags (Stop & Escalate If You See These)

| Flag | Action | Owner |
|------|--------|-------|
| Design mockups delayed > 2 days | Extend timeline or cut scope (fewer wireframes) | PM + Design Lead |
| API integration blocked by backend | Async mock API so frontend can continue | Tech Lead |
| Scope creep (requests for formula builder, 3+ templates) | Defer to Phase 2; document for later | PM |
| Critical accessibility bug found | Fix before shipping; may extend timeline | Engineering Lead |
| Gov rate data not loading | Escalate to backend; use hardcoded defaults for MVP | Backend Lead |
| Mobile performance < 4G acceptable | Optimize or cut features; performance is non-negotiable | Frontend Lead |

---

## 🎉 Success Celebration Criteria (At End of Sprint)

### If MVP Ships Successfully:
- ✅ All 6 wireframes fully functional
- ✅ Wizard saves state (user can return later)
- ✅ Sample payroll calculations accurate
- ✅ Mobile responsive tested
- ✅ No hard-stop bugs
- **Celebration**: Demo to customer, plan Phase 2 with team

### If MVP Ships with Compromises:
- ✅ Core flow works (Wireframes 1 → 6)
- ⚠️ Some polish/customization deferred to patch
- ⚠️ Mobile optimized for key screens only
- **Next**: Ship Phase 1 patch in Week 3

### If MVP Doesn't Ship:
- 🔴 Core flow broken or too many bugs to launch
- **Next**: Extended Week 3, troubleshoot, replan Phase 1+2 timeline

---

## 📚 Reference Docs (Keep Linked & Accessible)

**Always available for team reference:**
- MVP_WIREFRAME_PRIORITIES.md (full spec)
- DISCOVERY_ANALYSIS_SUMMARY.md (decision reasoning)
- ONBOARDING_WIZARD_WITH_TEMPLATES.md (template details)
- Visual Artifact (quick overview): https://claude.ai/code/artifact/11bbfa9a-2077-4bc5-8965-aeff549a9bd7

**During development:**
- Component stories/Figma (design reference)
- DB schema docs (backend reference)
- API docs (frontend/backend sync)

---

## ✉️ Communication Plan

### Daily
- **15-min standup** (same time every day): What we did, what we're doing, blockers

### Weekly (End of Friday)
- **30-min design review** with whole team
- **Quick decision on** scope adjustments, timeline risks, Phase 2 planning

### Bi-weekly (End of Week 2)
- **Stakeholder demo** (PM + leadership): Show working MVP
- **Retrospective** (team only): What went well, what to improve for Phase 2

---

## 🎯 Final Confirmation Before Kickoff

**Everyone on this checklist should be able to answer YES:**

- [ ] I understand the problem we're solving (support burden on 2-week onboarding)
- [ ] I understand what's in MVP (1 timekeeping + 2 payroll templates, basic customization)
- [ ] I understand what's NOT in MVP (employee import, leave policies, formula builder)
- [ ] I understand the timeline (2 weeks is tight but achievable)
- [ ] I know my role and what I'm building/designing
- [ ] I know where to find answers (README.md, MVP_WIREFRAME_PRIORITIES.md, artifact)
- [ ] I can raise blockers/concerns early (daily standups, design review)
- [ ] I'm committed to shipping this MVP in 2 weeks

**If you answered NO to any of these → Ask in kickoff meeting BEFORE sprint starts.**

---

**Sprint Starts**: [Date]  
**Sprint Ends**: 2 weeks later  
**Demo to Stakeholders**: End of Day 14  
**Go/No-Go Decision**: End of Day 14  
**Phase 2 Planning**: Week 3 (if MVP ships)

---

**Document Version**: 1.0  
**Created**: September 2026  
**Status**: Ready to Use  
**Print & Post**: On team wall / Slack pinned message / Project wiki
