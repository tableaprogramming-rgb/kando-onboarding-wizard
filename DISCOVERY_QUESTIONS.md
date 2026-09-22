# Kando Onboarding Wizard - Discovery Questions

**Purpose**: Strategic questions to deepen understanding of scope, priorities, and implementation approach  
**Status**: Open for answers  
**Created**: September 2026

---

## Business & Problem Context

1. **Why now?** What triggered this project? 
   - e.g., customer feedback, churn due to long onboarding, competitive pressure, sales feedback
   - **Answer**: 
    - Our main challenge is potential client's or our new customer's adoption into Kando. Due to Kando's complex nature, they keep on reaching out for support. This will pose as a personnel cost on our end. We figured that if there is a proper guidance or a ready made setups, this will lessen the time the support team needed to guide the users. 
    - One reason why we also thinking of providing the users with templated setups like: paygroup with the formula, compliance with mapped fields, Pay info fields, timesheet > periods and etc, because we realized that our current target businesses: the micro to small ones does not really have a very solid foundation on their HR processes. So they are reaching out to ask and asking basic HR processes or policies making us function like as an HR consultant. We are thinking of ways where we can guide them on the app directly, lowering the time for  our support team to spend for each of the new users.

2. **What's the pain point today?** How do new Kando customers currently set up timekeeping/payroll? Who does it (support team, customer, consultant)?
   - **Answer**: 
    - Please refer to my answer on number 1

3. **Current implementation timeline:** How long do onboarding/setup consultations actually take today? 
   - (You mentioned 4-6 weeks—is that accurate for all customers or just complex ones?)
   - **Answer**: 
    - Estimated around 2 weeks before the users get comfortable in using the application without the constant guidance with the Kando Support team

4. **Who asked for this?** Whose idea was this—sales, support, product, customers?
   - **Answer**: 
    - Our Support Team as well as our Accounts Manager/Sales

---

## Target Audience & Scope

5. **Who uses the wizard?** 
   - HR admin setting up for their org?
   - Sales/CS person during customer onboarding?
   - Both?
   - **Answer**: 
    - Primarily the HR, or the people that is responsible for setting up the organization.

6. **Who are we targeting first?** Is this for:
   - Net-new customers only?
   - Existing customers needing to reconfigure?
   - Both?
   - **Answer**: 
    - I guess net-new customer? I'm not so sure what does net-new customer really means. 

7. **Org size sweet spot:** The 5-6 templates assume different company sizes. What's your target customer profile for this feature? 
   - e.g., 50-500 employees, specific industries
   - **Answer**: 
    - for now, we can settle for 10 to 20 employees. industries and not specified.

---

## Current Kando Capabilities

8. **Do timekeeping templates exist today?** Can customers already set up shifts/schedules, or is this all new functionality?
   - **Answer**: 
    - new functionality. Right now, when a company creates a new organition into kando, Kando does not have an automated script or function that places pre-made dummy setups where they can use to jumpstart their experience.

9. **Do payroll templates exist today?** Can customers already create pay groups/formulas, or do we need to build the entire pay types/deductions system from scratch?
   - **Answer**:
    - same answer to question 8. 

10. **What's the "manual" setup process today?** Do you have a documented procedure/checklist for onboarding an org? 
    - (This would be the baseline.)
    - **Answer**: 
     - I allow you to look into our sourcode to have an idea how much effort the users need to do to properly setup their organization. please look into source-code folder.

11. **Compliance data:** Are BIR tax tables, SSS rates, etc., already coded in Kando, or do we need to build that too?
    - **Answer**: 
     - Yes, users can add their own BIR and statutoriy benefit but we figured that we will provide this in advance for convenience.

---

## MVP Scope & Priorities

12. **Which templates should ship first?** You mentioned 6 payroll templates—which 2-3 would give us 80% of customer value?
    - **Answer**: 

13. **Which industries are highest priority?** 
    - e.g., BPO, manufacturing, retail, startups
    - **Answer**: 
     - no definite answer right now

14. **Is customization required in MVP?** Or do we start with "take it or leave it" templates and add customization later?
    - **Answer**: 
     - im not sure how to answer this. I'll leave this for later.

15. **What about employee data?** Do users upload a CSV of employees during setup, or is employee creation separate?
    - **Answer**: 
     - employee creation will be separate for now.

---

## Integration & Technical

16. **Does this integrate with existing Kando features?**
    - Does the selected timekeeping template auto-create shifts in the scheduling system?
    - Does the payroll template auto-create pay types/groups that payroll processing uses?
    - Are there any data dependencies we need to handle?
    - **Answer**: 
     - No answer for now

17. **What's the tech stack?** Does this live in the Vue 3 frontend or need backend changes? 
    - (I assume both?)
    - **Answer**: 
     -  Can't answer on this as of the moment.

18. **State/validation:** Do we need to prevent editing timekeeping/payroll after the wizard (to avoid breaking things), or allow changes anytime?
    - **Answer**: 
     - No answer yet.

---

## Success & Metrics

19. **How will we measure success?** Do you have baseline data on:
    - Current onboarding support hours per customer?
    - Current error/misconfiguration rate?
    - Time to first successful payroll run?
    - **Answer**: 
     - based on the time onboarding happened to the customers

20. **Post-launch validation:** How will we validate the templates work? 
    - e.g., test with 10 beta customers first, or launch to all new signups?
    - **Answer**: 
     - No answer for this for now.

---

## Team & Timeline

21. **Who's building this?** How many engineers, designers? What's the timeline?
    - **Answer**: 

22. **Blockers or dependencies?** Any features we need to build first before the wizard makes sense?
    - **Answer**: 

23. **Go-live date?** When do you want this live? 
    - e.g., Q4 2026, Q1 2027?
    - **Answer**: 

---

## Summary & Next Steps

**Questions Answered**: [ ] / 23

**Key Assumptions to Validate**:
- [ ] 
- [ ] 
- [ ] 

**Critical Blockers Identified**:
- [ ] 
- [ ] 

**Priority Areas for Deep Dive**:
1. 
2. 
3. 

---

**Document Version**: 1.0  
**Last Updated**: September 2026  
**Related Documents**: 
- README.md (Project overview)
- ONBOARDING_WIZARD_WITH_TEMPLATES.md (Feature specification)
