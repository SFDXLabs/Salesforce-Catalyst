## The Naming as Meta-Documentation Principle

Naming conventions are more than just a matter of style; they are a form of "meta-documentation" that makes a system self-describing. In a declarative environment like Salesforce, where there is no underlying code structure to convey purpose, a standardized naming convention provides immediate clarity and understanding. When a consistent naming convention is used, developers and administrators can understand a field's type and purpose at a glance without needing to inspect its properties. This practice is a crucial aspect of an intentional and maintainable solution, directly supporting the "Easy" principle of the Salesforce Well-Architected Framework.

## Custom Objects and Fields

### API Naming

- Custom Objects: The API name for a custom object should be a clean UpperCamelCase representation of the label, with no underscores or spaces (e.g., Opportunity Competitor -> OpportunityCompetitor__c).
- Custom Fields: Custom field API names should use UpperCamelCase (Pascal casing).

### Type-Specific Prefixes and Suffixes

The following table outlines type-specific naming conventions that provide essential context about a field.

#### Table: Custom Field Naming Conventions

| Field Type           | Convention                                 | Example API Name                    | Rationale                                                                                     |
|---------------------|---------------------------------------------|-------------------------------------|-----------------------------------------------------------------------------------------------|
| Boolean             | Prefix with Is, Are, or Has.                | IsActive__c or HasOpportunities__c  | Indicates a boolean field that answers a yes/no question.                                     |
| Date                | Prefix with Date_.                          | Date_Signed__c                      | Clearly identifies a field storing a date value.                                              |
| Date/Time           | Prefix with DateTime_.                      | DateTime_Sent__c                    | Clearly identifies a field storing a date and time value.                                     |
| Formula             | Suffix with _f.                             | TotalAmount_f__c                    | The suffix immediately indicates that the field is a formula.                                 |
| Roll-Up Summary     | Suffix with _RU.                            | TotalAmount_RU__c                   | The suffix identifies the field as a roll-up summary.                                         |
| Percent             | Include Percent in the name.                | DiscountPercent__c                  | Avoids confusion that can arise from Discount% labels which create generic API names.         |
| Lookup/Master-Detail| Suffix with Id (or _L, _Md).                | AccountId__c or CaseStatusHistory_L__c | Indicates the field holds a record Id and is a relationship field.                         |

## Flows and Resources

- Flows: A standardized flow naming convention is critical for organizational clarity. A recommended format is Domain_Type_Action, where Domain is typically the object, Type indicates the flow context (e.g., BeforeSave, AfterUpdate, ScreenFlow), and Action describes the purpose (e.g., Account_BeforeSave_SetClientNumber).
- Flow Variables: Variables should be prefixed to indicate their type. A common convention is var_ for a generic variable, sObj_ for a single record variable, and coll_ for a collection, followed by an indicator of the data type (e.g., var_sObj_coll_Accounts).
- Flow Elements: Naming conventions for flow elements should be prescriptive to indicate their purpose. Examples include Get_ for a Get Records element, Update_ for an Update Records element, and Create_ for a Create Records element.

## Lightning Web Components (LWC)

LWC adheres to modern web standards, which dictates specific naming conventions for its different components.

- Component Bundle and File Naming: The component bundle folder name and the HTML file name should be in camelCase (e.g., myFirstComponent). The JavaScript class name should be in
- PascalCase (e.g., MyFirstComponent).
- JavaScript Properties and HTML Attributes: This is a crucial distinction. In JavaScript, properties are named in camelCase (e.g., itemName), while in the HTML template, they are referenced using kebab-case (e.g., item-name) to align with standard HTML practices.
- CSS Classes: When using the Salesforce Lightning Design System (SLDS), CSS classes should use the slds- namespace and follow a BEM (Block, Element, Modifier) naming convention to prevent naming conflicts with other frameworks.
