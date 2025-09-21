---
mode: "agent"
model: Claude Sonnet 4
description: "Generate a new model file following existing codebase patterns"
---

# Create New Model File

Create a new model file in `api/fastify-models/` based on requirements, following the existing codebase patterns.

## Steps:
1. **Research** existing models to understand current patterns
2. **Choose connection**: main DB (core data), mayden DB (partner data), or tools DB (background jobs)
3. **Follow existing format** used in the codebase
4. **Include healthcare compliance** for PII and audit requirements

## Current Codebase Format:
Based on existing models like `StepperNote.js`, use this structure:

```javascript
/**
 * ModelName.js
 *
 * @description :: Brief description of what this model stores
 */

module.exports = {
    attributes: {
        userId: {
            type: mongoose.Schema.Types.ObjectId,
            required: true,
        },
        coachId: {
            type: mongoose.Schema.Types.ObjectId,
            required: true,
        },
        status: {
            type: String,
            enum: ["active", "inactive", "pending"],
            required: true,
        },
        data: {
            type: Object,
            defaultsTo: {}
        },
        createdAt: {
            type: "date",
            defaultsTo: new Date(),
        },
    },

    indices: [
        {
            keys: { userId: 1, createdAt: -1 },
            options: { name: "userId_createdAt" }
        }
    ],

    // Add model methods here if needed
    getByUserId: (opts, callback) => {
        const { userId } = opts;
        
        if (_.isEmpty(userId)) {
            app.logger.error(`Missing userId in ModelName.getByUserId`);
            return callback({ message: "Missing required parameters" });
        }

        ModelName.find({ userId })
            .sort({ createdAt: -1 })
            .callbackExec((err, results) => {
                if (err) {
                    app.logger.error(`Error in ModelName.getByUserId: ${JSON.stringify(err)}`);
                }
                return callback(err, results);
            });
    },
};
```

## Key Patterns from Existing Code:
- Use `attributes` object (not Mongoose schema)
- Use `type: "date"` for dates
- Use `indices` array for indexing
- Include model methods in the same file
- Use `callbackExec()` for queries
- Use `app.logger.error` for logging
