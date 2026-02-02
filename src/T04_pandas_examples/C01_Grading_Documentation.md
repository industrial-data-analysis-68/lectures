# Data Processing Steps: Grading Student Assignments

## Overview

This notebook processes student roster, group assignment, and grading data to generate individual student grades from group assignments. The workflow involves data cleaning, validation, merging, and error correction.

---

## 1. Data Loading

### 1.1 Load Roster Data

- **File**: `roster.xlsx`
- **Action**: Read student roster with section numbers stored as strings
- **Columns**: `student_id`, `sec`, and other student information
- **Validation**: Check for duplicate entries (none found)

### 1.2 Load Student Groups

- **File**: `student_groups.xlsx`
- **Action**: Read group assignment data
- **Expected**: Null values are acceptable since not all students form groups
- **Validation**: Check for duplicate entries (none found)

---

## 2. Data Cleaning

### 2.1 Group Name Normalization

**Problem**: Group names from user input may contain:

- Leading/trailing whitespace
- Multiple consecutive spaces
- Inconsistent formatting

**Solution**: Apply `formatGroupName()` function

- Strip leading/trailing spaces
- Replace multiple spaces with single space using regex: `\s+` → ` `

### 2.2 Create Group Key

**Purpose**: Enable robust merging that's resilient to spacing and capitalization variations

**Implementation**: `makeGroupKey()` function

- Remove all whitespace using regex: `\s+` → `""`
- Convert to lowercase
- Use as primary merge key instead of raw group name

---

## 3. Data Merging and Validation

### 3.1 Initial Merge: Roster + Groups

**Action**: Left merge roster with group assignments

- **Left table**: `dfRos` (roster)
- **Right table**: `dfGroup[["student_id", "group_name", "group_key"]]`
- **Key**: `student_id`
- **Type**: Left join (preserve all roster students)

### 3.2 First Validation: Missing Groups

**Check**: Identify students in sections 003, 006, 803, 806 without groups

- **Result**: Found students with null `group_name`

---

## 4. Data Quality Issues and Corrections

### 4.1 Issue 1: Mismatched Student IDs

**Problem**: Student ID `943301355` in group data not found in roster

**Investigation Steps**:

1. Filter groups with non-empty names
2. Check which student IDs aren't in roster
3. Verify with actual student

**Correction**:

- Update incorrect ID `943301355` → `228248149`
- Re-run merge and validation
- **Result**: No more null groups for required sections

### 4.2 Issue 2: Empty Group Names

**Problem**: Student `543046351` has empty string for `group_name`

**Investigation Steps**:

1. Locate student in group data
2. Contact student for correct group name

**Correction**:

- Update `group_name` to `"Sec3: no 123"`
- Update `group_key` to `"sec3:no123"`
- Re-run merge and validation

---

## 5. Grade Assignment

### 5.1 Load Group Grades

- **File**: `group_grade.xlsx`
- **Columns**: `group_name`, `sec`, grading columns including `total`

### 5.2 Process Group Grade Data

Apply same cleaning functions used for student groups:

1. Fill null group names with empty string
2. Apply `formatGroupName()` to normalize spacing
3. Apply `makeGroupKey()` to create merge key

### 5.3 Final Merge: Students + Grades

**Action**: Merge student-roster data with group grades

- **Left table**: `dfrs` (roster + groups)
- **Right table**: `dfGroupGrade`
- **Key**: `group_key`
- **Type**: Left join
- **Suffixes**: `("", "_y")` to handle duplicate column names
- **Cleanup**: Drop duplicate columns (`sec_y`, `group_name_y`)

### 5.4 Final Validation

**Check**: Verify all students in sections 003, 006, 803, 806 have grades

- Filter by target sections
- Check for null values in `total` column
- **Result**: No null values found - all students have grades

---

## 6. Export Results

### Output File

- **File**: `out_stu_grade.xlsx`
- **Content**: Complete student roster with individual grades from group assignments
- **Format**: Excel file without index column

---

## Key Functions

### `formatGroupName(text)`

```python
def formatGroupName(text):
    out = text.strip()
    out = re.sub(r"\s+", " ", out)
    return out
```

**Purpose**: Normalize group names by removing extra whitespace

### `makeGroupKey(text)`

```python
def makeGroupKey(text):
    out = re.sub(r"\s+", "", text)
    out = out.lower()
    return out
```

**Purpose**: Create standardized keys for reliable data merging

---

## Data Quality Checks Performed

1. ✓ Duplicate detection in roster
2. ✓ Duplicate detection in group assignments
3. ✓ Verification of group assignments for required sections
4. ✓ Student ID consistency between datasets
5. ✓ Empty group name detection
6. ✓ Grade assignment completeness
7. ✓ Final grade availability for all required students

---

## Notes

- The notebook uses an iterative validation approach: merge → check → fix → re-merge
- All corrections require verification with actual students before applying
- Left joins preserve all students even if they don't have group assignments
- The `group_key` approach makes the pipeline robust against user input variations
