
### About it
- Open Source, workflow automation platform
- Built on Node.js using JS for platform internals and user workflow logic
- Connect apps and services for task automation
- Commonly deployed primary configs:
    - Self-hosted: on-prem or private cloud deployed on Orgs for data control 
    - Cloud hosted: managed, with shared infrastructure
    - Internal automation tool: used as a tool to automated business processes internally between internal and external systems

### Architecture
- Workflow execution engine: core computational component responsible for orchestration and workflow execution
- Expression Evaluation System: Process dynamic expressions wrapped in {{}} evaluation as JS during executation
- Code Nodes: custom JS or python code for users to write
- 400+ pre-built API and service connectors for workflow


#### Exploit
[CVE-2025-68613](https://nvd.nist.gov/vuln/detail/CVE-2025-68613), a critical vulnerability in n8n that was published on December 19, 2025, with a CVSS score of 9.9
[Github detection](https://github.com/wioui/n8n-CVE-2025-68613-exploit)

Versions 0.211.0 through 1.120.3 contain a critical Remote Code Execution (RCE) vulnerability within the workflow expression evaluation system. If exploited, this flaw enables an authenticated attacker to execute system-level commands, potentially leading to data breaches, service disruptions, or full system compromise, all with the privileges assigned to the n8n process.

This vulnerability has been addressed in versions 1.120.4, 1.121.1, and 1.122.0.

`{{ (function(){ return this.process.mainModule.require('child_process').execSync('cat flag.txt').toString() })() }}`

### Detection
Set up a proxy solution to mamage request coming to n8n application
Send proxy logs to detection solution
https://docs.n8n.io/hosting/logging-monitoring/logging/

```
keywords:
    # Strong indicators of this n8n expression injection RCE
    - "this.process.mainModule.require('child_process')"
    - ".execSync("
    - "={{ (function(){"
    - "toString() })()"
    ```

