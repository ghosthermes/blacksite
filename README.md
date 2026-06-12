# blacksite

Playwright for things companies don't realize are broken.

Blacksite is a browser automation harness for detecting silent failures in analytics, consent management, authentication flows, and client-side integrations. 

Most QA suites focus on the functional UI. They check if a button click leads to the right page or if a form submission returns a success message. Blacksite is different. It investigates the hidden side effects of a page load. It finds the failures that don't trigger an error code but still create legal, privacy, or data integrity risks.

### The Problem: Silent Failures

A broken marketing tag won't crash your website. A cookie banner that fires trackers before a user clicks "Accept" won't show up in your application logs. These are silent failures. They persist for months because nobody is looking for them during standard feature testing. 

I built this as a spiritual successor to `adtech-forensics-engine`. While that tool was designed for legal evidence gathering, Blacksite is designed for continuous monitoring. It ensures your site actually obeys your privacy and data policies in real time.

### How it works

Blacksite uses a headless Playwright instance to mimic complex user behavior while monitoring the underlying network and DOM state. 

*   **Event Triggering:** It doesn't just load a page. It triggers specific edge-case events: scroll-depth thresholds, rapid-fire clicks, and "mouse-out" movements that often trigger intrusive scripts.
*   **Network Interception:** The tool captures every outgoing request. It compares these requests against a whitelist of approved domains and data types.
*   **State Verification:** It checks if the "Consent" state in local storage matches the actual behavior of the trackers on the page.
*   **Regression Testing for Privacy:** If a developer accidentally re-introduces a tracking pixel that was supposed to be removed, Blacksite catches it.

### MVP Capabilities

1.  **Headless Navigation:** Crawls deep links to test compliance consistency across an entire domain.
2.  **Network Auditing:** Captures and analyzes HAR files to find unauthorized data exfiltration.
3.  **Visual Regression:** Detects when "dark patterns" or deceptive UI elements are added to consent flows.
4.  **Policy Comparison:** Compares observed behavior against a JSON-based expected behavior manifest.

### Usage

You define your compliance rules in a simple config file and run Blacksite against your staging or production URL.

```bash
python3 blacksite.py --config rules.json --target https://site.com
```

### Why this exists

Compliance is usually treated as a one-time check during an audit. That approach fails as soon as the next deployment goes live. Blacksite treats privacy and analytics as a core part of the technical stack that requires automated testing just like any other feature. 

If your marketing department adds a new script through Google Tag Manager, you need to know exactly what it's doing. Blacksite tells you.
