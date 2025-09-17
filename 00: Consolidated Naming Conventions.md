## Apex

### Classes and Interfaces
- Format: UpperCamelCase (PascalCase)
- Semantics: Nouns or noun phrases
- Examples: `Character`, `ImmutableList`
- Test classes: Same, ending with `Test` (e.g., `AccountTriggerTest`)

### Methods
- Format: lowerCamelCase
- Semantics: Verbs or verb phrases
- Example: `sendMessage`

### Constants
- Format: CONSTANT_CASE (UPPER_WITH_UNDERSCORES)
- Example: `NUMBER_OF_ACCOUNTS`

### Variables and Parameters
- Format: lowerCamelCase
- Semantics: Descriptive names
- Notes: Avoid single-character names except standard loop iterators
- Example: `computedValues`

### Properties
- Format: UpperCamelCase

---

## Declarative: Flows and Resources

### Flow Names
- Recommended format: `Domain_Type_Action`
  - Domain: typically the object (e.g., `Account`)
  - Type: Flow context (e.g., `BeforeSave`, `AfterUpdate`, `ScreenFlow`)
  - Action: Purpose (e.g., `SetClientNumber`)
- Example: `Account_BeforeSave_SetClientNumber`

### Flow Variables
- Prefixes to indicate type:
  - `var_` for generic variables
  - `sObj_` for a single record variable
  - `coll_` for a collection
- Include an indicator of data type where helpful
- Example: `var_sObj_coll_Accounts`

### Flow Elements
- Prescriptive prefixes by element type:
  - `Get_` for Get Records
  - `Update_` for Update Records
  - `Create_` for Create Records

---

## Lightning Web Components (LWC)

### Component Bundle and Files
- Bundle (folder) name: camelCase (e.g., `myFirstComponent`)
- HTML file name: camelCase (e.g., `myFirstComponent.html`)
- JavaScript class name: PascalCase (e.g., `MyFirstComponent`)

### JavaScript vs. Template Attributes
- JS properties: camelCase (e.g., `itemName`)
- HTML template usage: kebab-case (e.g., `item-name`)

### CSS with SLDS
- Use `slds-` namespace
- Follow BEM (Block, Element, Modifier) to avoid conflicts

---

## Data Modeling and Platform Metadata

### Custom Objects (API Names)
- Format: UpperCamelCase
- No underscores or spaces in the core name
- Suffix: `__c`
- Example: Label “Opportunity Competitor” → `OpportunityCompetitor__c`

### Custom Fields (API Names)
- Format: UpperCamelCase (PascalCase)
- Suffix: `__c`
- Type-specific conventions:
  - Boolean: Prefix with `Is`, `Are`, or `Has`
    - Example: `IsActive__c`, `HasOpportunities__c`
  - Date: Prefix with `Date_`
    - Example: `Date_Signed__c`
  - Date/Time: Prefix with `DateTime_`
    - Example: `DateTime_Sent__c`
  - Formula: Suffix with `_f`
    - Example: `TotalAmount_f__c`
  - Roll-Up Summary: Suffix with `_RU`
    - Example: `TotalAmount_RU__c`
  - Percent: Include `Percent` in the name
    - Example: `DiscountPercent__c`
  - Lookup/Master-Detail: Suffix with `Id` (or `_L`, `_Md`)
    - Examples: `AccountId__c`, `CaseStatusHistory_L__c`

### Relationships
- Lookup/Master-Detail fields end with `Id` (preferred) or use `_L`, `_Md` to convey type

---

## Custom Settings and Other Metadata

- Follow object/field conventions for API names:
  - Objects (List or Hierarchy Custom Settings): UpperCamelCase + `__c`
    - Example: `OrgPreferences__c`
  - Fields: UpperCamelCase + `__c`
    - Apply type-specific prefixes/suffixes as above (e.g., `IsEnabled__c`, `Date_Expires__c`)
- For metadata that behaves like configuration or rules repositories:
  - Prefer descriptive, self-documenting names aligned to domain and purpose
  - Use externalized configuration objects or Custom Metadata Types with clear API names
    - Example Custom Metadata Type: `DiscountRule__mdt`
    - Example Fields: `IsActive__c`, `PercentValue__c`, `Date_Effective__c`
- Packageable metadata (e.g., handler classes referenced by Custom Metadata):
  - Name the metadata records to clearly reflect the target object and action
    - Example: `Account_BeforeUpdate_AssignTier`

---

## Summary Table of Key Prefixes/Suffixes

- Boolean fields: `Is...`, `Are...`, `Has...`
- Date fields: `Date_...`
- DateTime fields: `DateTime_...`
- Formula fields: `..._f`
- Roll-Up fields: `..._RU`
- Relationship fields: `...Id` (or `_L`, `_Md`)
- Flow variables: `var_`, `sObj_`, `coll_`
- Flow elements: `Get_`, `Create_`, `Update_`

---

## Notes on Consistency

- Use consistent casing and prefixes across teams and packages to support readability,
  maintainability, and searchability.
- Apply the same conventions in docs, code comments, and metadata labels where feasible
  to reinforce the “naming as meta-documentation” principle.
