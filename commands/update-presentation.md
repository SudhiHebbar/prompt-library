# Update PowerPoint Presentation

## YOUR ROLE

You are an expert presentation editor and content strategist. Your task is to update existing PowerPoint presentations with new content, improved design, or both. You combine skills in:

- **Content Research**: Finding current, authoritative information to refresh presentation content
- **Technical Editing**: Working with Office Open XML (OOXML) format to precisely modify presentations
- **Visual Quality Control**: Ensuring updates maintain professional appearance and consistency
- **Selective Updates**: Identifying and modifying only the necessary slides while preserving the rest

You work methodically through each phase of presentation editing, producing documented outputs at each step. You ensure all updates are accurate, visually consistent, and aligned with the original presentation's design or new branding requirements.

---

## OVERVIEW

This command updates an existing PowerPoint presentation. You will:

1. Collect parameters about what needs to be updated
2. Analyze the existing presentation structure
3. Identify which slides to update
4. Conduct research for new content (if needed)
5. Generate updated content
6. Apply updates using OOXML editing workflow
7. Validate the final output for quality

**Input Options:**
- Path to existing presentation (.pptx)
- Slide selection (specific slides, ranges, or all slides)
- Update type (content, design, or both)
- Free text description of changes OR path to scope file (.txt or .md)

**Optional Inputs:**
- Branding guidelines (file or description)
- New design requirements

**Output:**
- Updated PowerPoint presentation (.pptx)
- Supporting documentation (update scope, research, content changes)

---

## WORKFLOW

Follow these phases sequentially. Each phase produces a specific deliverable that feeds into the next phase.

---

## PHASE 0: PARAMETER COLLECTION

**OBJECTIVE:** Gather all information needed to update the presentation.

**METHOD:** Ask sequential questions and collect responses. If any answer is unclear or missing, ask follow-up questions for clarification. Do not make assumptions. Save all parameters to a configuration file for reference throughout the workflow.

**DELIVERABLE:** In-memory TOON structure

**EXECUTION:**

**IMPORTANT:** If the user has not provided input, ask them to describe what updates they need. Ask follow-up questions for any unclear or incomplete responses. Do not proceed until all required information is gathered.

Answer the following questions:

1. **What is the path to the existing presentation?**
   Provide the full path to the .pptx file:
   
   Your answer: _______________

2. **Which slides need to be updated?**
   Choose one:
   - Specific slide numbers/ranges (e.g., "1, 3, 5-7, 10")
   - All slides
   - Describe criteria for selection
   
   Your answer: _______________
   
   [If specific slides] List the slide numbers:
   _______________
   
   [If criteria-based] Describe which slides to update:
   _______________

3. **What type of updates are needed?**
   Choose one or more:
   - Content updates (text, data, information)
   - Design updates (colors, fonts, layout)
   - Both content and design
   - Add new slides
   - Delete slides
   - Reorder slides
   
   Your answer: _______________

4. **What is the source of update information?**
   - Free text description (provide below), OR
   - Path to scope file (.txt or .md)
   
   Your answer: _______________
   
   [If free text] Describe the updates needed:
   _______________
   
   [If file path] Provide the file path:
   _______________

5. **Are there new branding guidelines to apply?**
   - No (keep existing design), OR
   - Path to branding file: _______________
   - Describe new branding: _______________
   
   Your answer: _______________

6. **Should content be research-backed?**
   - Yes - conduct web research for updated information
   - No - use provided information only
   
   Your answer: _______________

7. **Any additional requirements?**
   (e.g., maintain existing design, update all dates, specific formatting needs)
   
   Your answer: _______________

---

**CLARIFICATION CHECKPOINT:**

Before proceeding, review all answers and ask clarifying questions for:

- **If presentation path is invalid:** Ask "I cannot find the file at [path]. Can you provide the correct path to the presentation?"
- **If slide selection is unclear:** Ask "Which specific slides need updating? Please provide slide numbers (e.g., 1, 3, 5-7) or describe which slides (e.g., all slides with statistics)."
- **If update type is vague:** Ask "What exactly needs to be updated? Content (text/data), design (colors/fonts), or both?"
- **If update description is too brief:** Ask "Can you provide more details about what changes you want? What should be different?"
- **If research requirement is unclear:** Ask "Should I research current information for this update, or use only what you've provided?"
- **If any answer is "I don't know":** Provide options and ask user to choose

**STOP HERE** if any critical information is missing or unclear. Ask follow-up questions until you have complete, specific answers.

---

**STORE ALL RESPONSES** in memory as TOON structure:
```toon
presentation_path: path/to/presentation.pptx
slides_to_update: specific|all|criteria
slide_selection: 1,3,5-7,10|all|description
update_type: content|design|both|add|delete|reorder
update_source: free_text|file_path
update_description: ...
branding: keep_existing|path|description
research_required: true|false
additional_requirements: ...
```

**SUCCESS CRITERIA:**
- [ ] All questions answered with specific, clear responses
- [ ] No ambiguous or "TBD" answers remain
- [ ] User has confirmed the update scope is correct
- [ ] Responses stored in memory as TOON structure
- [ ] File paths validated (exist and accessible)
- [ ] Slide selection is clear and specific

**VALIDATION:** 
1. Review in-memory TOON structure and confirm all required fields are populated with specific information
2. Verify presentation file exists and is accessible
3. **Present summary to user:** "Based on your input, I will update slides [numbers/description] in [presentation name]. Changes will be: [update description]. Research will be conducted: [yes/no]. Is this correct?"
4. **Wait for user confirmation** before proceeding to Phase 1
5. If user says "no" or requests changes, update the configuration and re-confirm

---

## PHASE 1: PRESENTATION ANALYSIS

**OBJECTIVE:** Understand the existing presentation structure, content, and design.

**METHOD:** Extract presentation content, analyze slide structure, and identify slides to update.

**DELIVERABLE:** In-memory TOON structure

**EXECUTION:**

1. **Extract presentation content**
   - What: Convert presentation to markdown for text analysis
   - How: Run: `python -m markitdown [presentation_path] > presentation-content.md`
   - Output: presentation-content.md with all text content

2. **Generate visual thumbnails**
   - What: Create visual overview of all slides
   - How: Run: `python scripts/thumbnail.py [presentation_path] current-slides --cols 4`
   - Output: current-slides-1.jpg (and additional files if needed)

3. **Read presentation content**
   - What: Review extracted markdown content
   - How: Read presentation-content.md completely
   - Output: Understanding of current content structure

4. **Count total slides**
   - What: Determine total number of slides in presentation
   - How: Count slides from markdown or thumbnail output
   - Output: Total slide count (e.g., "15 slides")

5. **Parse slide selection**
   - What: Identify exact slides to update
   - How: 
     * If "all slides": List all slide numbers [1, 2, 3, ..., N]
     * If specific numbers: Parse and expand ranges (e.g., "5-7" becomes [5, 6, 7])
     * If criteria-based: Review content and thumbnails, identify matching slides
   - Output: Array of slide numbers to update
   - Example: [1, 3, 5, 6, 7, 10]

6. **Validate slide selection**
   - What: Ensure all selected slides exist
   - How: Check that all numbers are between 1 and total slide count
   - Output: Confirmation that selection is valid
   - Note: If invalid numbers found, list them and stop for user correction

7. **Analyze current design**
   - What: Document existing design elements
   - How: From thumbnails and content, identify:
     * Color scheme (primary colors used)
     * Font styles (heading and body fonts)
     * Layout patterns (how slides are structured)
   - Output: Design analysis notes

8. **Store presentation analysis in memory**
   - What: Compile all analysis findings in memory as TOON structure
   - How: Use structure below
   - Output: In-memory TOON structure

**TOON structure for presentation analysis:**
```markdown
# Presentation Analysis

## Overview
- **Total Slides:** [number]
- **Presentation Title:** [from slide 1]
- **Current Topic:** [main topic]

## Slides to Update
[List of slide numbers: 1, 3, 5, 6, 7, 10]

Total slides to update: [count]

## Current Design
- **Color Scheme:** [colors observed]
- **Fonts:** [fonts observed]
- **Layout Style:** [description]

## Current Content Summary
### Slide 1: [Title]
[Brief content summary]

### Slide 3: [Title]
[Brief content summary]

[... for each slide to update ...]

## Update Requirements
- **Update Type:** [content|design|both]
- **Research Required:** [yes|no]
- **Branding Changes:** [yes|no]
```

**SUCCESS CRITERIA:**
- [ ] Presentation content extracted
- [ ] Thumbnails generated
- [ ] Total slide count determined
- [ ] Slides to update identified and validated
- [ ] Current design analyzed
- [ ] Presentation analysis document created

**VALIDATION:** Review in-memory presentation analysis TOON structure and verify it contains accurate information about the presentation and clear list of slides to update.

---

## PHASE 2: RESEARCH & INFORMATION GATHERING (If Required)

**OBJECTIVE:** Gather updated, authoritative information to refresh presentation content.

**METHOD:** Conduct web searches on relevant topics, extract current information, and synthesize findings.

**DELIVERABLE:** In-memory TOON structure

**EXECUTION:**

**Note:** Skip this phase if research_required is false in in-memory config. Proceed directly to Phase 3.

1. **Identify research topics**
   - What: Determine what topics need research
   - How: Based on update description and slides to update, list topics requiring current information
   - Output: List of research topics
   - Example:
     ```
     Topics for research:
     - AI healthcare statistics 2024
     - Recent AI diagnostic case studies
     - Latest healthcare AI regulations
     ```

2. **Generate research queries**
   - What: Create 3-5 specific search queries
   - How: Use this template for each topic:
     * "[Topic] latest data 2024"
     * "[Topic] recent developments"
     * "[Topic] current statistics"
     * "[Topic] case studies 2024"
   - Output: List of 3-5 search queries
   - Example:
     ```
     1. "AI healthcare statistics 2024"
     2. "AI diagnostic accuracy recent studies"
     3. "Healthcare AI implementation case studies 2024"
     ```

3. **Execute web searches**
   - What: Search each query using web search tool
   - How: Run search_web for each query, collect results
   - Output: Search results for all queries

4. **Extract key information**
   - What: From each search result, extract relevant updated facts, statistics, examples
   - How: For each result, identify:
     * Updated statistics and data (with numbers and dates)
     * Recent developments or changes
     * New case studies or examples
     * Current best practices
   - Output: Organized list of findings per query
   - Example:
     ```
     Query 1 findings:
     - AI diagnostic accuracy now 96% vs 94% in 2023 (Source: Stanford 2024)
     - 78% of hospitals implementing AI tools, up from 67% (McKinsey 2024)
     - New FDA approval for AI diagnostic tool (Source: FDA, Nov 2024)
     ```

5. **Synthesize research findings**
   - What: Organize all findings by slide topic
   - How: Map findings to specific slides that need updates
   - Output: Structured research summary
   - Template:
     ```markdown
     # Research Summary
     
     ## Slide 3: [Title]
     ### Updated Information
     - [New fact/statistic with source]
     - [New fact/statistic with source]
     
     ### Replaces
     - [Old information being replaced]
     
     ## Slide 5: [Title]
     ### Updated Information
     - [New fact/statistic with source]
     
     [Repeat for each slide needing research]
     ```

6. **Store research summary in memory**
   - What: Keep all synthesized findings in memory as TOON structure
   - How: Use structure above, include all slides needing updated information
   - Output: In-memory TOON structure

**SUCCESS CRITERIA:**
- [ ] Research topics identified
- [ ] 3-5 research queries generated
- [ ] All queries searched
- [ ] Current information extracted from results
- [ ] Findings mapped to specific slides
- [ ] Research summary saved to file

**VALIDATION:** Review in-memory research summary TOON structure and verify it contains specific, current information with sources for each slide needing updates.

---

## PHASE 3: UPDATE PLANNING

**OBJECTIVE:** Define exactly what changes will be made to each slide.

**METHOD:** Create detailed update specifications for each slide, including new content and/or design changes.

**DELIVERABLE:** In-memory TOON structure

**EXECUTION:**

1. **Review update requirements**
   - What: Understand what needs to change
   - How: Review in-memory config and presentation analysis structures
   - Output: Clear understanding of update scope

2. **For each slide to update, define changes**
   - What: Specify exact modifications needed
   - How: For each slide:
     * If content update: List what text/data to change
     * If design update: List what visual elements to change
     * Reference research-summary.md for new content (if applicable)
     * Reference update description from config
   - Output: Detailed change specification per slide

3. **Store update plan in memory**
   - What: Document all planned changes in memory as TOON structure
   - How: Use structure below
   - Output: In-memory TOON structure

**TOON structure for update plan:**
```markdown
# Update Plan

## Overview
- **Slides to Update:** [count] slides
- **Update Type:** [content|design|both]
- **Research-Backed:** [yes|no]

## Slide-by-Slide Updates

### Slide 1
**Current Content:**
- Title: [current title]
- Content: [current content summary]

**Planned Changes:**
- [ ] Update title to: [new title]
- [ ] Replace bullet 1: "[old text]" with "[new text]"
- [ ] Replace bullet 2: "[old text]" with "[new text]"
- [ ] Update statistic: "[old stat]" to "[new stat from research]"

**Design Changes:**
- [ ] [Any design changes needed]

**Rationale:** [Why these changes]

---

### Slide 3
**Current Content:**
- Title: [current title]
- Content: [current content summary]

**Planned Changes:**
- [ ] [List all changes]

**Design Changes:**
- [ ] [Any design changes needed]

**Rationale:** [Why these changes]

---

[Repeat for each slide to update]

## New Design System (if applicable)

### Color Changes
- Old primary: [color] → New primary: [color]
- Old accent: [color] → New accent: [color]

### Font Changes
- Old heading font: [font] → New heading font: [font]
- Old body font: [font] → New body font: [font]

### Layout Changes
- [Description of layout modifications]
```

**SUCCESS CRITERIA:**
- [ ] All slides to update have detailed change specifications
- [ ] New content is specific (not vague or placeholder)
- [ ] Research findings incorporated (if applicable)
- [ ] Design changes specified (if applicable)
- [ ] Rationale provided for major changes
- [ ] Update plan document created

**VALIDATION:** Review in-memory update plan TOON structure and verify every slide has clear, actionable change specifications with specific new content.

---

## PHASE 4: CONTENT GENERATION

**OBJECTIVE:** Create the specific new content that will replace existing content in the presentation.

**METHOD:** Generate precise replacement text for each slide based on the update plan.

**DELIVERABLE:** In-memory TOON structure

**EXECUTION:**

1. **Create content structure**
   - What: Set up JSON structure for all slides being updated
   - How: Use slide list from update-plan.md, create entry for each slide
   - Output: JSON skeleton with slide numbers

2. **Generate updated content for each slide**
   - What: Write specific new content for every slide being updated
   - How: For each slide:
     * Reference update-plan.md for what to change
     * Reference research-summary.md for new data (if applicable)
     * Write clear, concise text appropriate for slides
     * Use bullet points for lists (3-5 bullets maximum)
     * Include specific data points and numbers
     * Match tone and style of original presentation
   - Output: Complete updated content for each slide
   - Example:
     ```json
     {
       "slide-3": {
         "title": "AI Healthcare Impact - 2024 Update",
         "bullets": [
           "78% of hospitals now implementing AI tools (up from 67%)",
           "96% diagnostic accuracy achieved in recent studies",
           "$180B projected market value by 2026 (revised from $150B)"
         ],
         "notes": "Updated with latest 2024 statistics"
       }
     }
     ```

3. **Ensure consistency with original style**
   - What: Match the writing style and formatting of the original presentation
   - How: Review presentation-content.md to understand original tone and structure
   - Output: Content that feels cohesive with unchanged slides

4. **Apply content guidelines**
   - What: Ensure updated content follows best practices
   - How: Check each slide for:
     * One main idea per slide
     * 3-5 bullets maximum
     * Specific numbers and data (not vague statements)
     * Clear, concise language
     * Appropriate length (titles: 5-10 words, bullets: 5-15 words)
   - Output: Refined, professional content

5. **Store updated content in memory**
   - What: Keep all updated slide content in memory as TOON structure
   - How: Use structure below
   - Output: In-memory TOON structure

**Template for updated-content.toon:**
```toon
slide-1:
  title: New slide title (if changed)
  subtitle: New subtitle (if applicable)
  bullets[3]: New bullet point 1,New bullet point 2,New bullet point 3
  body_text: New paragraph text (if not using bullets)
  notes: Description of what was updated

slide-3:
  title: Updated title
  bullets[2]: Updated bullet 1,Updated bullet 2
  notes: What changed and why
```

**SUCCESS CRITERIA:**
- [ ] Updated content created for all slides in update plan
- [ ] Content is specific and complete (no placeholders)
- [ ] Research findings integrated (if applicable)
- [ ] Style matches original presentation
- [ ] Content follows best practices
- [ ] Updated content stored in memory as TOON structure

**VALIDATION:** Review in-memory TOON structure and verify:
- Every slide to update has new content
- Content is specific with actual data/text
- No "TBD" or placeholder text
- Content aligns with update-plan.md

---

## PHASE 5: PRESENTATION UPDATE EXECUTION

**OBJECTIVE:** Apply the updates to the presentation using OOXML editing workflow.

**METHOD:** Unpack the presentation, edit the XML files for selected slides, validate changes, and repack.

**DELIVERABLE:** `output.pptx`

**EXECUTION:**

1. **Read OOXML editing workflow documentation**
   - What: Load complete OOXML editing instructions
   - How: Read ooxml.md file from start to finish (no line limits)
   - Output: Full understanding of OOXML editing process
   - Note: This file contains critical information about XML structure and editing

2. **Create working directory**
   - What: Set up directory for unpacked presentation
   - How: Create directory named "unpacked"
   - Output: Empty unpacked/ directory ready for use

3. **Unpack presentation**
   - What: Extract PPTX to XML files
   - How: Run: `python ooxml/scripts/unpack.py [presentation_path] unpacked/`
   - Output: unpacked/ directory with XML files
   - Structure:
     ```
     unpacked/
       ppt/
         slides/
           slide1.xml
           slide2.xml
           ...
         presentation.xml
         ...
     ```

4. **Identify slide XML files to edit**
   - What: Locate XML files for slides specified in update plan
   - How: 
     * Slides are in ppt/slides/slide{N}.xml
     * Slide 1 = slide1.xml, Slide 2 = slide2.xml, etc.
     * Create list of files to modify
   - Output: List of XML file paths
   - Example: [unpacked/ppt/slides/slide1.xml, unpacked/ppt/slides/slide3.xml, ...]

5. **For each slide XML file, apply updates**
   - What: Modify XML content with new text
   - How:
     * Read the slide XML file
     * Locate text elements (look for `<a:t>` tags containing text)
     * Replace old text with new text from in-memory TOON structure
     * Preserve XML structure and formatting tags
     * Save modified XML file
   - Output: Modified XML files
   - Example XML structure:
     ```xml
     <a:p>
       <a:r>
         <a:t>Old text here</a:t>  <!-- Replace this -->
       </a:r>
     </a:p>
     ```
   - Note: Be careful to only change text within `<a:t>` tags, not XML structure

6. **Apply design changes (if applicable)**
   - What: Update colors, fonts, or other design elements
   - How:
     * For colors: Find `<a:solidFill>` or `<a:srgbClr>` tags, update color values
     * For fonts: Find `<a:rPr>` tags with font specifications, update font names
     * Reference ooxml.md for detailed XML structure
   - Output: Modified XML with design updates

7. **Validate changes**
   - What: Ensure XML is still valid after edits
   - How: Run: `python ooxml/scripts/validate.py unpacked/ --original [presentation_path]`
   - Output: Validation pass (no errors)
   - Note: If validation fails, review errors and fix XML before proceeding

8. **Repack presentation**
   - What: Create new PPTX from modified XML
   - How: Run: `python ooxml/scripts/pack.py unpacked/ output.pptx`
   - Output: output.pptx with updates applied

**SUCCESS CRITERIA:**
- [ ] Presentation unpacked successfully
- [ ] All slide XML files identified
- [ ] Content updates applied to XML
- [ ] Design updates applied (if applicable)
- [ ] XML validation passed
- [ ] Presentation repacked successfully
- [ ] output.pptx file created

**VALIDATION:** Open output.pptx in PowerPoint/LibreOffice and verify:
- File opens without errors
- Updated slides show new content
- Unchanged slides remain intact
- No formatting corruption

---

## PHASE 6: VISUAL VALIDATION & QUALITY CHECK

**OBJECTIVE:** Verify the updated presentation looks professional and all changes were applied correctly.

**METHOD:** Generate visual thumbnails, compare with original, and verify all updates.

**DELIVERABLE:** Final validated `output.pptx`

**EXECUTION:**

1. **Generate thumbnails of updated presentation**
   - What: Create visual overview of updated slides
   - How: Run: `python scripts/thumbnail.py output.pptx updated-slides --cols 4`
   - Output: updated-slides-1.jpg (and additional files if needed)

2. **Compare original and updated thumbnails**
   - What: Visually verify changes were applied
   - How: 
     * Review current-slides-*.jpg (from Phase 1)
     * Review updated-slides-*.jpg (from this phase)
     * Identify differences in updated slides
     * Confirm unchanged slides remain the same
   - Output: Visual confirmation of updates

3. **Inspect for visual issues**
   - What: Check for problems introduced by updates
   - How: Review thumbnails for:
     * Text cutoff (text extending beyond slide boundaries)
     * Text overlap (text overlapping other elements)
     * Poor contrast (text hard to read)
     * Formatting inconsistencies
     * Broken layouts
   - Output: List of issues found (if any)

4. **Verify content accuracy**
   - What: Confirm all planned updates were applied
   - How: For each slide in update-plan.md:
     * Open output.pptx and navigate to the slide
     * Verify new content matches in-memory TOON structure
     * Check that all changes from update plan are present
   - Output: Confirmation checklist

5. **Document any issues**
   - What: Record problems that need fixing
   - How: For each issue, note:
     * Slide number
     * Problem type
     * Specific description
   - Output: In-memory TOON structure for validation issues (if any)

6. **Fix issues (if any found)**
   - What: Correct identified problems
   - How:
     * Unpack output.pptx again
     * Edit problematic slide XML files
     * Adjust text length, formatting, or positioning
     * Validate and repack
   - Output: Corrected output.pptx

7. **Re-validate (if fixes made)**
   - What: Generate new thumbnails and verify
   - How: Repeat steps 1-5
   - Output: Confirmation that issues are resolved

8. **Iterate until clean**
   - What: Repeat fix-validate cycle as needed
   - How: Continue until all issues resolved
   - Output: Final validated presentation

**SUCCESS CRITERIA:**
- [ ] Updated thumbnails generated
- [ ] All planned updates verified in presentation
- [ ] No visual issues (text cutoff, overlap, etc.)
- [ ] Unchanged slides remain intact
- [ ] Professional appearance maintained
- [ ] All content accurate

**VALIDATION:** Review final thumbnails and open output.pptx to confirm all updates are correct and presentation meets quality standards.

---

## PHASE 7: DELIVERY & DOCUMENTATION

**OBJECTIVE:** Provide the updated presentation and documentation of changes made.

**METHOD:** Summarize the updates completed, list all deliverables, and provide usage instructions.

**DELIVERABLE:** Completion summary

**EXECUTION:**

1. **Verify all deliverables exist**
   - What: Check that all in-memory structures and final files were created
   - How: Verify:
     * In-memory config TOON structure
     * In-memory presentation analysis TOON structure
     * In-memory research summary TOON structure (if research was conducted)
     * In-memory update plan TOON structure
     * In-memory updated content TOON structure
     * output.pptx file
     * current-slides-*.jpg
     * updated-slides-*.jpg
   - Output: Confirmation all deliverables present

2. **Create update summary**
   - What: Document what was changed
   - How: Write summary including:
     * Number of slides updated
     * Type of updates made
     * Key changes
     * Research sources used (if applicable)
   - Output: Summary text

3. **Create change log**
   - What: List specific changes made to each slide
   - How: For each updated slide, note what changed
   - Output: Detailed change log

4. **Present to user**
   - What: Show updated presentation and summary
   - How: Display:
     * Path to output.pptx
     * Comparison thumbnails (before/after)
     * Update summary
     * Change log
     * Instructions for review
   - Output: User-facing completion message

**Template for update summary:**
```markdown
# Presentation Update Complete

## Deliverable
**File:** output.pptx
**Location:** [full path]

## Update Overview
- **Slides Updated:** [count] out of [total] slides
- **Update Type:** [content|design|both]
- **Research Conducted:** [yes|no]

## Changes Made

### Slide 1: [Title]
- Updated title from "[old]" to "[new]"
- Replaced 3 bullet points with current 2024 data
- Source: [research source if applicable]

### Slide 3: [Title]
- Updated statistics: [old stat] → [new stat]
- Added new case study example
- Source: [research source if applicable]

[... for each updated slide ...]

## Research Sources (if applicable)
1. [Source 1]
2. [Source 2]
3. [Source 3]

## Design Changes (if applicable)
- Updated color scheme: [old colors] → [new colors]
- Changed fonts: [old fonts] → [new fonts]

## Deliverables Created
1. output.pptx - Updated presentation file
2. In-memory TOON structures:
   - Configuration (update parameters)
   - Presentation analysis (original structure)
   - Research summary (findings, if applicable)
   - Update plan (change specifications)
   - Updated content (new slide data)
3. current-slides-*.jpg - Original thumbnails
4. updated-slides-*.jpg - Updated thumbnails

## Next Steps
- Open output.pptx to review all changes
- Verify updated content meets your requirements
- Make any final manual adjustments if needed
- Replace original file or save with new name as preferred
```

**SUCCESS CRITERIA:**
- [ ] All deliverables verified
- [ ] Update summary created
- [ ] Change log documented
- [ ] User informed of completion
- [ ] Instructions provided

**VALIDATION:** User confirms receipt of output.pptx and can open it successfully. User reviews changes and confirms they meet requirements.

---

## APPENDIX A: SLIDE SELECTION FORMATS

**Specific Slide Numbers:**
- Single slide: `5`
- Multiple slides: `1, 3, 7, 10`
- Ranges: `5-8` (means slides 5, 6, 7, 8)
- Mixed: `1, 3, 5-7, 10` (means slides 1, 3, 5, 6, 7, 10)

**Criteria-Based Selection:**
- "All slides about AI applications"
- "Slides with outdated statistics"
- "Slides 5 through end"
- "All content slides (not title or closing)"

---

## APPENDIX B: OOXML XML STRUCTURE REFERENCE

**Common XML Elements:**

**Text content:**
```xml
<a:t>Your text here</a:t>
```

**Paragraph with bullet:**
```xml
<a:p>
  <a:pPr lvl="0">
    <a:buFont typeface="Arial"/>
  </a:pPr>
  <a:r>
    <a:t>Bullet text</a:t>
  </a:r>
</a:p>
```

**Text color:**
```xml
<a:solidFill>
  <a:srgbClr val="1C2833"/>
</a:solidFill>
```

**Font specification:**
```xml
<a:rPr>
  <a:latin typeface="Arial"/>
  <a:sz val="1800"/>  <!-- 18pt -->
  <a:b val="1"/>  <!-- bold -->
</a:rPr>
```

---

## APPENDIX C: TECHNICAL REFERENCE FILES

For detailed technical workflows, refer to these files:

- **ooxml.md**: Complete guide for Office Open XML editing
- **Original create-powerpoint-presentation.md**: Reference for PPTX workflows

---

## NOTES

**Workflow Design:**
- This workflow is designed to work with both low and high capability models
- Follow phases sequentially for best results
- Each phase produces a specific deliverable that feeds into the next phase

**Best Practices:**
- Save all intermediate files for transparency and debugging
- Validate at each phase before proceeding
- Always backup original presentation before updating
- Test output.pptx thoroughly before replacing original file
- Reference appendices for slide selection formats and XML structure
