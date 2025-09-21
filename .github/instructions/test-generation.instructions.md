# TEST GENERATION INSTRUCTIONS

## TEST SETUP

-   Use `.env.test` with `MONGODB_URI` (e.g., `MONGODB_URI=mongodb://localhost:27017/touchkin-test`)
-   Bootstrap: `test/bootstrap.test.js` lifts Sails before all tests
-   Structure: `test/unit/` (services, models, controllers) + `test/integration/`
-   Manual globals setup: `global._ = require('lodash')`

## Repository Test Structure

The test directory is organized as follows:

```
test/
├── bootstrap.test.js               # Test environment setup (Sails bootstrap)
├── bootstrap_ci.test.js            # CI-specific test bootstrap
├── mocha.opts                      # Mocha configuration options
├── setup/                          # Test setup and mocking utilities
│   ├── DynamoDbMock.js             # DynamoDB service mocking
│   ├── KMSMock.js                  # AWS KMS service mocking
│   ├── RequestMock.js              # HTTP request mocking
│   ├── SQSMock.js                  # AWS SQS service mocking
│   ├── SlackWebhookMock.js         # Slack webhook mocking
│   └── SocketChatServiceMock.js    # Socket chat service mocking
├── unit/                           # Unit tests
│   ├── controllers/                # Controller unit tests
│   │   └── EmergencyDetailsController.test.js
│   ├── models/                     # Model unit tests
│   │   ├── ActiveUserSession.test.js    # Session management tests
│   │   ├── AgendaJobs.test.js           # Background job tests
│   │   ├── Appointment.test.js          # Appointment model tests
│   │   ├── Assessment.test.js           # Assessment model tests
│   │   ├── AuditTrail.test.js           # Audit logging tests
│   │   ├── Coach.test.js                # Coach model tests
│   │   ├── CoachAgreementHash.test.js   # Coach agreement tests
│   │   ├── CoachCalendarEvent.test.js   # Coach calendar tests
│   │   ├── CoachChat.test.js            # Coach chat tests
│   │   ├── CoachFile.test.js            # Coach file management tests
│   │   ├── CoachGroupConfigs.test.js    # Coach permissions tests
│   │   ├── CoachLeave.test.js           # Coach leave management tests
│   │   ├── CoachLogin.test.js           # Coach authentication tests
│   │   ├── CoachMessageTemplate.test.js # Message template tests
│   │   ├── CoachPIISuggestion.test.js   # PII suggestion tests
│   │   ├── CoachProfile.test.js         # Coach profile tests
│   │   ├── CoachSlot.test.js            # Coach scheduling tests
│   │   ├── Consent.test.js              # Consent management tests
│   │   ├── Event.test.js                # Event tracking tests
│   │   ├── FileACL.test.js              # File access control tests
│   │   ├── LastUserInteraction.test.js  # User interaction tests
│   │   ├── ListEntry.test.js            # List management tests
│   │   ├── Popup.test.js                # Popup management tests
│   │   ├── QueueMessage.test.js         # Message queue tests
│   │   ├── ReportedIssue.test.js        # Issue reporting tests
│   │   ├── RequestLog.test.js           # Request logging tests
│   │   ├── Review.test.js               # Review system tests
│   │   ├── Segment.test.js              # User segmentation tests
│   │   ├── SessionCreditEvent.test.js   # Credit system event tests
│   │   ├── SessionCreditState.test.js   # Credit system state tests
│   │   ├── StepperNote.test.js          # Stepper note tests
│   │   ├── StepperNoteDetail.test.js    # Stepper note detail tests
│   │   ├── Subscription.test.js         # Subscription management tests
│   │   ├── TcReferral.test.js           # Referral system tests
│   │   ├── TcReferralAuditTrail.test.js # Referral audit tests
│   │   ├── TcReferralPII.test.js        # Referral PII tests
│   │   ├── User.test.js                 # User model tests
│   │   ├── UserAlert.test.js            # User alert tests
│   │   ├── UserCoachMapping.test.js     # User-coach relationship tests
│   │   ├── UserUnreadCache.test.js      # Unread message cache tests
│   │   ├── Value.test.js                # Value model tests
│   │   ├── ZoomMeetings.test.js         # Zoom integration tests
│   │   └── functions/                   # Model function tests
│   ├── services/                   # Service unit tests
│   │   ├── AuditTrailService.test.js         # Audit trail service tests
│   │   ├── CoachService.test.js              # Coach service tests
│   │   ├── EmergencyDetailsService.test.js   # Emergency service tests
│   │   ├── GenerateProgressNoteService.test.js # Progress note generation tests
│   │   ├── InformedConsentService.test.js    # Consent service tests
│   │   ├── LanguageConfigService.test.js     # Language config tests
│   │   ├── OnboardingService.test.js         # User onboarding tests
│   │   ├── PatientDischargeService.test.js   # Patient discharge tests
│   │   ├── RateLimitService.test.js          # Rate limiting tests
│   │   ├── RegistryService.test.js           # Registry service tests
│   │   ├── ReportGenerationService.test.js   # Report generation tests
│   │   ├── SMSDetail.test.js                 # SMS detail tests
│   │   ├── SMSService.test.js                # SMS service tests
│   │   ├── StepperNoteService.test.js        # Stepper note service tests
│   │   ├── TcReferralService.test.js         # Referral service tests
│   │   ├── TwilioMeeting.test.js             # Twilio meeting tests
│   │   ├── USReService.test.js               # US healthcare billing tests
│   │   ├── UserService.test.js               # User service tests
│   │   └── UtilService.test.js               # Utility service tests
│   ├── policies/                   # Policy unit tests
│   ├── helpers/                    # Test helper utilities
│   └── datasets/                   # Unit test datasets
├── integration/                    # Integration tests
│   ├── ESLint.test.js              # Code quality integration tests
│   ├── controllers/                # Controller integration tests
│   ├── models/                     # Model integration tests
│   ├── services/                   # Service integration tests
│   ├── policies/                   # Policy integration tests
│   ├── helpers/                    # Integration test helpers
│   └── datasets/                   # Integration test datasets
```

### Test File Naming Convention

-   **Unit Tests**: `[ComponentName].test.js` in respective `test/unit/` subdirectories
-   **Integration Tests**: `[ComponentName].test.js` in respective `test/integration/` subdirectories
-   **Mock Files**: `[ServiceName]Mock.js` in `test/setup/`

## TEST COMMANDS

```bash
npm test                    # Run tests
```

## Test Patterns

-   Group related tests in `describe` blocks
-   Use `before`/`after` for suite-level setup/cleanup (database cleanup)
-   Use `beforeEach`/`afterEach` for test-level setup/cleanup (mocking, test data)
-   Mock external services and databases for unit tests
-   Use real database for integration tests (test database)

## TEST STRUCTURE

```js
describe("FileName", () => {
	before(async () => {
		// Suite-level setup: database cleanup, global setup
		await cleanupDB();
	});

	after(async () => {
		// Suite-level cleanup: database cleanup
		await cleanupDB();
	});

	describe("functionName", () => {
		it("should handle valid input", (done) => {
			// Test implementation
			done();
		});

		it("should handle error cases", (done) => {
			// Error handling tests
			done();
		});
	});
});
```

## TESTING GUIDELINES

-   Test both success and error paths
-   Include edge cases and boundary conditions
-   Mock external dependencies
-   Use descriptive test names
-   Clean up test data after each test
