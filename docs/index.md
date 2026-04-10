# Welcome to Lyncis Security Platform

Lyncis is a modern, on-premise client/server platform designed to centralize and automate system security audits.
It leverages the industry-standard `lynis` auditing tool to evaluate the security posture of your infrastructure,
aggregate the findings, and present them in a highly actionable dashboard.

## Purpose and Scope

Managing security compliance across multiple servers, virtual machines, and homelab environments can quickly become
chaotic. Running manual audits yields fragmented text files that are hard to track over time.

Lyncis solves this by providing:

* **Centralized Command:** Trigger scans across your entire fleet from a single web interface.
* **Trend Analysis:** Track your Hardening Index over time to see if your security posture is improving or degrading.
* **Actionable Insights:** Aggregate critical warnings globally, creating an immediate to-do list for administrators without needing to dig through raw logs.
* **Automated execution:** Agents operate on a secure outbound-polling heartbeat, requiring no open firewall ports on the target machines.

## Source Repositories

Lyncis is designed as a distributed, microservice-like architecture divided into three distinct repositories to ensure
clean separation of concerns and independent CI/CD lifecycles:

1. **[lyncis-backend](https://github.com/marcelhaerle/lyncis-backend):** The core REST API written in Go using the Fiber framework. It manages the PostgreSQL database, orchestrates agent tasks, and processes incoming scan data.
2. **[lyncis-agent](https://github.com/marcelhaerle/lyncis-agent):** A lightweight, statically compiled Go binary deployed to target hosts. It executes the Lynis audits and securely reports back to the backend.
3. **[lyncis-ui](https://github.com/marcelhaerle/lyncis-ui):** The frontend dashboard built with React, Vite, and Tailwind CSS, providing a modern dark-mode interface for monitoring and control.
