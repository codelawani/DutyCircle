# Database Relationships and onDelete Behavior

**1. Parent-Child Relationship:**
In relational databases, the relationship between tables (or models, as they're referred to in Prisma) often manifests as a parent-child relationship. The "parent" has a primary key, and the "child" has a foreign key that references the primary key of the "parent."

**2. `onDelete` Behavior:**
The `onDelete` behavior is specified to determine what should happen to the child records when the parent record is deleted. Think of it as instructions for how the system should handle the "orphaned" children.

**3. Where to Specify:**
While in Prisma, you can specify the `onDelete` behavior on either the parent model or the child model; the effect is always with respect to the child. Regardless of where you set it, the behavior dictates what happens to the child records when the parent is deleted.

**4. Simplified Analogy:**
Imagine a class (parent) and students (children). If the class gets canceled (parent is deleted):

- `Cascade`: All students are removed (children are deleted).
- `SetNull`: The students are still there, but they no longer belong to any class (the foreign key is set to null).
- `Restrict`: You can't cancel the class if there are still students enrolled.

**5. Children's Effect on Parent:**
The deletion of a child record doesn't affect the parent record, regardless of the `onDelete` behavior.

**In Summary:**
The `onDelete` directive tells the system how to handle child records when a parent record is deleted. It's a safeguard against leaving records in an inconsistent state. And while it can be specified on either the parent or child model in Prisma, the behavior always pertains to the child records in relation to their parent.
