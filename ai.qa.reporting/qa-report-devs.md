I’m checking the available QA artifacts for any per-test flaky details, because the current file only exposes counts and I need to confirm whether specific flaky test names exist anywhere in the workspace.

Read [](file:///c%3A/Users/d.bukhavetski/Work/ai.qa.reporting/ai.qa.reporting/test-results-sample.json)

Searched for regex `flaky|testName|tests|cases` (`**/ai.qa.reporting/**`), 22 results

I found only a flaky count, not per-test flaky names. I’m reading the existing generated team report so the developer report stays aligned with the workspace’s current format while making that data gap explicit.

Read [](file:///c%3A/Users/d.bukhavetski/Work/ai.qa.reporting/qa-report-team.md)

**Developer Report**

Based on qa-test-results.json, Regression Sprint 20 is healthy at the suite level with 92 passed and 8 failed out of 100, but the regression signal is concentrated rather than evenly distributed. The main risk sits in Checkout and Orders, while Login and Profile are currently stable. The dataset also reports 3 flaky tests in qa-test-results.json, but it does not include the individual flaky test names, so they cannot be listed from the available data.

| Area | Passed | Failed | Total | Pass Rate | Fail Rate |
|---|---:|---:|---:|---:|---:|
| Overall | 92 | 8 | 100 | 92.0% | 8.0% |
| Checkout | 20 | 4 | 24 | 83.3% | 16.7% |
| Orders | 18 | 3 | 21 | 85.7% | 14.3% |
| Search | 15 | 1 | 16 | 93.8% | 6.3% |
| Profile | 25 | 0 | 25 | 100.0% | 0.0% |
| Login | 14 | 0 | 14 | 100.0% | 0.0% |

**Regression Areas**

- Checkout is the highest-priority regression area from qa-test-results.json, with 4 failures and the worst fail rate at 16.7%. This is the first place to inspect for recent breakage.
- Orders is the second regression area from qa-test-results.json, with 3 failures and a 14.3% fail rate. That points to a concentrated instability zone rather than random suite noise.
- Search has a minor regression signal in qa-test-results.json, with 1 failure and a 6.3% fail rate.
- Profile and Login show no current failures in qa-test-results.json and qa-test-results.json. They are useful control modules for comparing recent code changes and environment drift.
- Severity remains meaningful despite the strong overall pass rate: 2 critical, 3 major, and 4 minor defects are recorded in qa-test-results.json. That means release risk is still material even though the top-line pass rate looks good.

**ASCII Visualization**

```text
Overall pass/fail
Pass  | ############################################## 92%
Fail  | ####                                             8%

Module fail concentration
Checkout | #### 4
Orders   | ###  3
Search   | #    1
Profile  |      0
Login    |      0
```

**Flaky Tests**

- Reported flaky count: 3, from qa-test-results.json
- Named flaky tests: not available in the current JSON schema
- Data gap: the file contains only an aggregate flaky count, not a per-test list with names, IDs, or owning modules

If you want the flaky section to become actionable, the JSON needs a field such as:
```json
"flakyTests": [
  {"name": "Checkout should apply discount", "module": "Checkout"},
  {"name": "Order history refreshes after cancel", "module": "Orders"},
  {"name": "Search returns exact match first", "module": "Search"}
]
```

If useful, I can next turn this into a polished markdown report for qa-report-team.md or draft the exact JSON schema change needed to support flaky-test listings.