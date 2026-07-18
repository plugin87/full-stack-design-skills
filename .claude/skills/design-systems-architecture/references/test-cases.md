# Design Systems Architecture - Light-Weight Test Cases

A set of 6 practical scenarios to validate the skill works well for real-world design system challenges.

## Test Case 1: Component Hierarchy Decision

**Prompt:**
```
I'm building a design system and need to decide on component organization. 
I have:
- Button (simple)
- TextField (label + input + error message)
- ContactForm (3 TextFields + Button + Submit handler)
- ContactFormPage (ContactForm + Header + Footer + Real data)

Where should each live in my component hierarchy? 
Should I use Atomic Design or something else?
```

**Expected Output:**
- Correctly classifies Button as Atom
- Classifies TextField as Molecule (label + input + error = multiple atoms)
- Classifies ContactForm as Organism (molecules + atoms + business logic)
- Classifies ContactFormPage as Page (template + real data from app)
- Recommends Atomic Design folder structure
- Or suggests flatter structure if team prefers
- Explains the trade-offs of each approach
- References atomicity principle (components should be reusable, composable)

**Criteria:** Skill applies component hierarchy knowledge, recognizes Atomic Design principles, provides actionable folder structure.

---

## Test Case 2: Token Organization Strategy

**Prompt:**
```
I'm building a design token system. Currently I have:
- Raw colors: #2563eb, #10b981, #ef4444, #f59e0b
- Raw typography: Helvetica 16px, 20px, 24px
- Raw spacing: 4px, 8px, 16px, 24px, 32px

How should I organize these into a design token hierarchy? 
Do I need multiple levels, or can I just export them all?
```

**Expected Output:**
- Recommends Layer 1 (Primitives): raw values with generic names
- Recommends Layer 2 (Semantic): colors with meaning (primary, success, danger, etc.)
- Explains why layering matters (maintainability, team communication)
- Provides example naming convention (color-primary, color-success, etc.)
- Suggests DTCG standard format (optional but valuable)
- References token organization principle from SKILL.md

**Criteria:** Skill applies token architecture knowledge, explains layering rationale, provides specific naming examples.

---

## Test Case 3: Versioning & Breaking Change

**Prompt:**
```
I want to rename Button's "color" prop to "variant" 
(e.g., color="red" → variant="danger"). 
This is a breaking change. 
How do I communicate this to teams without breaking their code?

Also, what version number should the change be?
```

**Expected Output:**
- Recommends deprecation cycle: announce → warn (2-3 releases) → remove in major version
- Version strategy: v1.5.0 (deprecate with console warning) → v2.0.0 (remove)
- Provides example console warning message
- Recommends communication plan (release notes, team meetings, migration guide)
- Explains MAJOR version bump (semantic versioning)
- Provides mapping table (color-red → variant-danger, etc.)
- Suggests timeline (give teams 2-3 months to migrate)

**Criteria:** Skill applies versioning and deprecation knowledge, provides communication strategy, explains user impact.

---

## Test Case 4: Multi-Brand Support

**Prompt:**
```
We're expanding to 3 brands, but don't want to duplicate components. 
Current system has 30 components, all tied to brand A's color scheme. 
How do I support brands B and C without forking everything?

What's the simplest approach that avoids duplication?
```

**Expected Output:**
- Recognizes need to separate tokens from components
- Recommends token-level customization (keep components, override tokens)
- Suggests multiple token sets (brand-a.json, brand-b.json, brand-c.json)
- Provides CSS variable or theme provider implementation idea
- Warns against component duplication (maintenance nightmare)
- Recommends monorepo structure (optional, if teams need significant differences)
- References scaling strategy from SKILL.md

**Criteria:** Skill applies multi-brand architecture knowledge, avoids duplication, provides practical implementation path.

---

## Test Case 5: Governance Model Decision

**Prompt:**
```
We're launching a design system for 5 engineering teams. 
Should we have:
A) A dedicated design systems team that approves all changes
B) Allow teams to contribute, but a steering committee reviews
C) Open contribution (anyone can submit PRs)

Which model is best? What are the trade-offs?
```

**Expected Output:**
- Recommends Model A (Centralized) for starting out (simplest, most consistent)
- Explains when to move to Model B (Federated) as system matures
- Explains Model C (Open Source) is rare for internal systems; better for public
- Provides pros/cons of each:
  - A: Consistent but slow, requires dedicated people
  - B: Faster but requires discipline, needs clear process
  - C: Great participation but harder to maintain quality
- Recommends starting small, growing as usage increases
- References governance models in SKILL.md

**Criteria:** Skill applies governance knowledge, provides trade-off analysis, recommends staged approach.

---

## Test Case 6: Component API Design

**Prompt:**
```
I'm designing a Card component with:
- Image
- Title
- Description
- Footer with actions

Currently I have 20 props (imageUrl, imageAlt, title, description, footerText, footerIcon, etc.).
Teams complain it's hard to use. What's wrong and how do I fix it?
```

**Expected Output:**
- Identifies over-parameterization (too many props)
- Recommends composition/children pattern instead
- Suggests slots: header, children, footer (instead of individual props)
- Provides example refactored API:
  ```tsx
  <Card>
    <CardImage src="..." alt="..." />
    <CardTitle>Title</CardTitle>
    <CardDescription>Desc</CardDescription>
    <CardFooter>Actions</CardFooter>
  </Card>
  ```
- Alternatively: render function props (headless component)
- Explains how this is more flexible and easier to maintain
- References composition patterns from references

**Criteria:** Skill identifies API anti-patterns, recommends composition approach, provides refactored example.

---

## Success Criteria

All test cases should result in:
- ✅ Practical, architectural advice
- ✅ References to design system principles from SKILL.md
- ✅ Explicit trade-offs (no single "right answer")
- ✅ Staging or phased approach (don't do everything at once)
- ✅ Explanation of user/team impact
- ✅ Actionable next steps

---

## Evaluation Notes

This skill should:
- **Never** prescribe specific tools (Figma, Storybook, npm) without caveats
- **Always** focus on principles, not tools
- **Always** consider scalability and maintenance
- **Always** recommend communicating changes to users (teams)
- **Always** provide rationale, not just rules
- **Always** acknowledge complexity (systems are hard!)

Test success = User understands the architecture decision and can implement it without tool-specific guidance.
