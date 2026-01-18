# keploy-demo
This repository shows how Keploy can automatically generate API test cases and mocks by simply observing real traffic — no manual test writing required.
The goal of this project was to understand Keploy end-to-end in a local setup and explain it in a beginner-friendly, DevRel-style way.

What This Project Does
I used a minimal Node.js + Express app so Keploy’s behavior is easy to see.

APIs

- POST /users — create a user
- GET /users/:id — fetch a user

Users are stored in memory (no database).

Why This Setup?

- Minimal app → fewer moving parts
- Easier to see how and when Keploy creates tests
- Great for understanding Keploy without noise

Getting Started
Prerequisites
node >= 20
npm


( Docker is optional and not required here )

Run the App Normally
node index.js

You should see:
Server running on port 3000

This step is important, always verify your app works before adding Keploy.

Recording Traffic with Keploy

Start the app in record mode:
keploy record -c "node index.js"

Keploy now runs alongside your app and waits for traffic.

In another terminal, hit the APIs:

curl -X POST http://localhost:3000/users \
-H "Content-Type: application/json" \
-d '{"id":"1","name":"Shivani"}'

curl http://localhost:3000/users/1

Keploy generates tests only when real traffic hits the app.

What Keploy Generates

After stopping Keploy, you’ll see new files:

keploy/
keploy.yml
keploy_agent.log

This means Keploy successfully:

- Captured API requests & responses
- Generated test cases and mocks
- Created config for replay
- Replaying Tests

Run Keploy in test mode:
keploy test -c "node index.js"

What happens here:

- The app starts again
- Keploy replays recorded requests
- Responses are compared with recorded behavior

If everything matches → ✅ tests pass.

How Keploy Works

- Record mode → watches real traffic
- Traffic hits app → tests get created
- Mocks generated → replay is deterministic
- Test mode → traffic is replayed, responses validated
- No test code written by hand.

Key Learnings

- No traffic = no tests
- Smaller projects make learning Keploy easier
- Always run the app independently first
- Keploy shines for API-heavy services

Repository Structure
.
├── index.js              # Express app
├── keploy/               # Auto-generated tests & mocks
├── keploy.yml            # Keploy config
├── package.json
└── README.md

Files generated only during local execution are excluded to keep the repo clean and reproducible.

Final Thoughts

This project helped me clearly understand how Keploy:

- captures real API traffic
- turns it into test cases automatically
- replays those tests to validate behavior

A simple setup made the learning experience much smoother, and honestly, pretty satisfying.
