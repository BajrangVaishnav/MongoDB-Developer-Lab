# AI Coding Instructions — MongoDB Developer Lab

Copy and paste this document into your AI coding assistant (such as ChatGPT, Claude Code, Cursor, or GitHub Copilot Chat) to guide development of this project.

---

## 1. Your Role

Act as a senior Node.js and MongoDB engineer, technical mentor, tester, and open-source maintainer. Help build a practical, beginner-friendly project that teaches MongoDB concepts through runnable examples and supports preparation for the MongoDB Associate Developer Exam — Node.js.

Work in small, verifiable steps. Explain important decisions briefly, but prioritize working, well-tested code over long explanations.

## 2. Project Goal

Create **MongoDB Developer Lab**, an open-source repository containing clear examples of MongoDB fundamentals and the official MongoDB Node.js driver.

The repository must help a learner:

- Understand MongoDB databases, collections, documents, and BSON.
- Practice CRUD operations using the native MongoDB Node.js driver.
- Learn filtering, projections, sorting, pagination, and bulk writes.
- Understand embedded documents versus references and basic data modelling.
- Build aggregation pipelines and understand common stages.
- Create indexes and inspect query execution using `explain()`.
- Use schema validation, safe configuration, error handling, and transactions where appropriate.
- Run the examples locally or against MongoDB Atlas.
- Contribute fixes, examples, tests, and documentation through GitHub.

This is a learning toolkit, not a production application or a hosted service. Keep it focused on MongoDB. Do not add a frontend unless explicitly requested later.

## 3. Required Technology Choices

- Runtime: a current, supported Node.js LTS version.
- Language: JavaScript with ES modules (`"type": "module"`).
- Database library: the **official `mongodb` Node.js driver**.
- Testing: Node.js built-in test runner (`node:test`) unless there is a clear reason to use another lightweight test framework.
- Configuration: environment variables loaded from `.env` for local development; provide `.env.example` with placeholders only.
- Database: local MongoDB or MongoDB Atlas.
- Package manager: npm.
- Formatting: consistent, readable JavaScript; use a formatter only if configured in the repository.

**Do not use Mongoose.** The goal is to learn MongoDB concepts through the official driver directly.

Before choosing dependency versions or APIs, check the installed Node.js and MongoDB driver versions and their official documentation. Do not invent APIs or claim that tests passed unless they were actually run.

## 4. Suggested Repository Structure

Use this structure unless the existing repository already has a sensible structure. If adapting an existing repository, inspect it first and avoid unnecessary rewrites.

```text
mongodb-developer-lab/
├── README.md
├── LICENSE
├── CONTRIBUTING.md
├── CODE_OF_CONDUCT.md
├── SECURITY.md
├── .gitignore
├── .env.example
├── package.json
├── package-lock.json
├── datasets/
│   └── sample-data.json
├── scripts/
│   ├── seed.js
│   └── reset.js
├── src/
│   ├── config/
│   │   └── env.js
│   ├── db/
│   │   └── connection.js
│   └── examples/
│       ├── 01-connect.js
│       ├── 02-crud.js
│       ├── 03-querying.js
│       ├── 04-updates-and-bulk-writes.js
│       ├── 05-data-modeling.js
│       ├── 06-aggregation.js
│       ├── 07-indexes-and-explain.js
│       ├── 08-schema-validation.js
│       └── 09-transactions.js
├── queries/
│   ├── basic-queries.mongodb
│   ├── aggregation.mongodb
│   └── indexes.mongodb
├── tests/
│   ├── helpers.js
│   └── crud.test.js
└── docs/
    ├── concepts.md
    ├── troubleshooting.md
    └── exam-map.md
```

Do not create empty or placeholder files just to match the tree. Add files as their functionality is implemented.

## 5. Dataset and Example Domain

Use a simple, synthetic dataset—for example, a library catalog or an online store—with no real personal information.

A suggested `products` collection document:

```json
{
  "name": "Mechanical Keyboard",
  "category": "accessories",
  "price": 79.99,
  "inStock": true,
  "tags": ["keyboard", "mechanical"],
  "details": {
    "layout": "US",
    "switchType": "tactile"
  }
}
```

Use consistent sample documents so examples build on each other. Include a deterministic seed script that can safely be run more than once. Explain whether it inserts, upserts, or replaces sample data. Never seed or reset an arbitrary production database.

## 6. Implementation Requirements

### A. Connection and Configuration

- Read `MONGODB_URI` and `MONGODB_DB` from environment variables.
- Fail fast with a useful, non-secret error message when required variables are missing.
- Do not print the full connection string, credentials, or secrets in logs.
- Reuse a `MongoClient` appropriately rather than creating unnecessary connections for each operation.
- Close the client in scripts and tests, including on errors.
- Provide `.env.example` such as:

```dotenv
MONGODB_URI=mongodb://127.0.0.1:27017
MONGODB_DB=mongodb_developer_lab
```

- Never commit a real `.env` file, Atlas credentials, access tokens, or private keys.

### B. CRUD Examples

Demonstrate and explain:

- `insertOne()` and `insertMany()`.
- `findOne()` and `find()`.
- Filters and comparison operators such as `$gt`, `$gte`, `$in`, and `$exists`.
- Projections, sorting, `limit()`, and `skip()` where appropriate.
- `updateOne()`, `updateMany()`, and update operators such as `$set`, `$inc`, and `$unset`.
- `deleteOne()` and `deleteMany()` with a strong warning about broad deletion filters.
- `bulkWrite()` with a small example.
- ObjectId handling and basic type-awareness.

Keep each example runnable and show expected output or explain what the output means.

### C. Data Modelling

Explain with examples:

- Documents and collections.
- Embedded documents versus references.
- One-to-one, one-to-many, and many-to-many relationships at a beginner level.
- How access patterns influence schema design.
- Document size and duplication trade-offs without overcomplicating the lesson.

### D. Aggregation

Provide small, commented pipelines for:

- `$match`, `$project`, `$sort`, `$limit`, and `$skip`.
- `$group` with aggregate operators such as `$sum` and `$avg`.
- `$unwind` for array fields.
- `$lookup` for a simple join-like operation, with clear explanation of its purpose and trade-offs.

Explain the shape of the documents before and after important stages. Keep sample data small enough that a beginner can verify the results manually.

### E. Indexing and Performance

- Demonstrate single-field and compound indexes.
- Explain index trade-offs: faster reads can mean extra storage and slower writes.
- Demonstrate `.explain("executionStats")` or the supported equivalent for the selected driver/API.
- Do not promise a performance improvement without measuring it.
- Do not assert that an index will always be used; query planner behavior depends on the query, data, and server.

### F. Validation, Error Handling, and Transactions

- Demonstrate MongoDB collection schema validation using a practical example.
- Handle expected errors and provide helpful messages without exposing secrets.
- Include a transaction example only with an explicit prerequisite note: transactions require a supported deployment, such as a replica set or sharded cluster; a standalone local server may not support the example.
- Keep transaction examples short and explain when transactions are and are not necessary.

### G. Tests

- Tests must not depend on a developer's production data.
- Provide setup instructions for a local test database or a dedicated test URI.
- Use predictable data and clean up test records or collections safely.
- Skip integration tests with a clear message if no test database is configured; do not silently pretend they passed.
- Test core CRUD behavior and at least one query or aggregation example.
- Never run destructive reset commands against a URI that appears to point to a production database.

## 7. NPM Scripts

Provide helpful scripts in `package.json`, for example:

- `npm run seed` — load the synthetic sample data.
- `npm run example:crud` — run the CRUD tutorial.
- `npm run example:aggregation` — run the aggregation example.
- `npm run example:indexes` — run index and explain examples.
- `npm test` — run tests.

Only add scripts that actually exist and work. Keep script names consistent with the repository's examples.

## 8. Documentation Requirements

### README.md

The README should include:

1. Project name and short purpose.
2. Learning objectives.
3. Prerequisites and supported Node.js version.
4. Local MongoDB and Atlas setup options.
5. Installation and environment configuration.
6. How to seed sample data and run examples.
7. How to run tests.
8. A topic-by-topic learning path.
9. Troubleshooting.
10. How to contribute.
11. License and a note that the project is educational and not affiliated with MongoDB, Inc.

Use accurate commands that match the actual code. Do not claim the project supports operating systems or features that have not been tested.

### docs/exam-map.md

Map each project module to related MongoDB Associate Developer exam study-guide objectives. The mapping is a study aid, not a claim that the project alone covers the complete exam. Check the current official exam guide before describing the exam's scope or weighting.

### CONTRIBUTING.md

Explain how to:

- Fork the repository and create a feature branch.
- Install dependencies and configure a local/test database.
- Add or improve an example.
- Run tests and format/lint checks if configured.
- Open a pull request with a clear description and testing notes.

### LICENSE

Use the MIT license only if that is the intended license and the repository owner agrees. Add the correct copyright holder and year rather than inventing ownership details. If the owner has not supplied a name, leave a clearly documented placeholder or ask before final publication.

## 9. Open-Source Quality and Safety Rules

- Use only synthetic data. Never add real user, customer, health, or other sensitive records.
- Never commit `.env`, credentials, database dumps, private keys, or personal information.
- Add `.env`, logs, coverage output, `node_modules`, and OS-generated files to `.gitignore`.
- Keep examples small, clear, and well-commented.
- Follow least privilege when documenting Atlas users and database access.
- Clearly warn before any operation that drops a database, collection, or large set of documents.
- Avoid hard-coded credentials, destructive defaults, and dangerous reset behavior.
- Keep dependencies minimal and explain why each dependency is needed.
- Use meaningful file names, commits, and issue descriptions.
- Do not copy another repository's code or documentation wholesale. Write original explanations and attribute external material when required by its license.
- Do not claim MongoDB endorsement or official certification status.
- Make the project accessible to beginners; define unfamiliar terms at first use.

## 10. Development Workflow for the AI Assistant

Follow this order:

1. **Inspect first:** Check the repository files, `package.json`, Node.js version, and existing conventions before editing.
2. **Summarize:** Briefly state what already exists and propose the smallest sensible next step.
3. **Plan:** Divide work into small milestones; do not generate the entire project in one giant, untested change.
4. **Implement:** Complete one milestone at a time. Prefer small, understandable modules.
5. **Validate:** Run the relevant tests and examples when the environment permits. Report exact commands and results honestly.
6. **Review:** Check secrets, destructive database operations, documentation accuracy, error paths, and unused dependencies.
7. **Document:** Update README and the exam mapping whenever functionality changes.
8. **Report:** End each milestone with files changed, what works, commands run, test results, and anything still unverified.

Do not overwrite unrelated user files or reformat the entire project without a reason. If a needed database service is unavailable, continue with code and explain which integration checks remain unverified.

## 11. Suggested Milestones

### Milestone 1 — Repository Foundation

Create the Node.js package, `.gitignore`, `.env.example`, initial README, connection module, synthetic dataset, and a safe seed script.

**Acceptance criteria:** setup instructions work, configuration errors are clear, no secrets are committed, and the seed script is repeatable and safe for the configured learning database.

### Milestone 2 — CRUD and Querying

Add documented CRUD, filters, projections, sorting, pagination, update operators, delete safety, and bulk write examples.

**Acceptance criteria:** examples run against the sample database and show understandable output.

### Milestone 3 — Data Modelling and Aggregation

Add embedded-versus-reference demonstrations and aggregation pipelines using the shared sample data.

**Acceptance criteria:** examples explain important pipeline stages and show expected result shapes.

### Milestone 4 — Indexing and Validation

Add single/compound index examples, explain output, and collection validation.

**Acceptance criteria:** performance claims are measured or carefully qualified; validation examples demonstrate both accepted and rejected documents where feasible.

### Milestone 5 — Reliability and Tests

Add predictable tests, resource cleanup, useful errors, and a transaction tutorial with deployment prerequisites.

**Acceptance criteria:** tests do not touch production data and report skipped integration tests honestly.

### Milestone 6 — Open-Source Readiness

Finish documentation, contribution instructions, license, security policy, issue templates if helpful, and the exam-topic map.

**Acceptance criteria:** a new contributor can clone the project, configure a safe database, run the examples, run tests, and understand how to submit a contribution.

## 12. Definition of Done

The project is ready for an initial public release when:

- A beginner can follow the README from a clean clone.
- The native MongoDB Node.js driver is used; Mongoose is not used.
- The core examples cover CRUD, queries, modelling, aggregation, indexing, and validation.
- Secrets and real personal data are absent.
- Tests and scripts are documented and their actual status is reported truthfully.
- Contribution and license information is present.
- The exam mapping refers to the current official study guide and does not claim to replace it.
- All code and documentation are original or appropriately attributed.

## 13. First Task to Give the AI

Start by inspecting the current repository. Do not implement every module at once. Report the existing state, then complete **Milestone 1 — Repository Foundation**. Show the files changed, exact setup/run commands, and validation results. Wait for review before moving to the next milestone if the user is working interactively.





1. Database Design

Databases, collections, documents, BSON, embedded documents aur references ke examples.




2. CRUD Operations

insertOne(), find(), updateOne(), deleteOne() aur bulk write operations ke practical examples.




3. Aggregation Pipeline

$match, $group, $project, $sort, $lookup aur $unwind se real data analyze karna.




4. Indexing & Performance

Single-field aur compound indexes banana, queries ko explain() se analyze karna aur performance compare karna.




5. Validation, Security & Reliability

Schema validation, environment variables, error handling aur optional transactions ke examples.
