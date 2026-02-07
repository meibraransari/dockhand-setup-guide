# 🐳 Dockhand - Modern Docker Management

![Dockhand Banner](https://dockhand.pro/images/logo-dark.webp)

> **Modern Docker management for everyone.** A powerful, intuitive Docker platform that makes container management simple, efficient, and beautiful. Perfect for homelabs, growing teams, and enterprises.

---

## 🚀 What is Dockhand?

Dockhand is a **modern, self-hosted Docker management platform** that provides real-time container management, Compose stacks, Git deployments, and SSO authentication - all in one beautiful interface. Say goodbye to YAML indentation headaches and hello to streamlined Docker operations!

🔗 **Website:** [https://dockhand.pro/](https://dockhand.pro/)  
📦 **Source Code:** [GitHub Repository](https://github.com/Finsys/dockhand)

---

## ✨ Why Dockhand?

### 🏠 Perfect for Your Homelab
- **SQLite by default** - No complex database setup required
- **Runs on Raspberry Pi** - Lightweight and efficient
- **Zero telemetry** - Your data stays yours
- **Free forever** - No bait-and-switch pricing

### 👥 Built for Growing Teams
- **OIDC/SSO included free** - Enterprise auth without enterprise prices
- **Container activity logging** - Track every change
- **Git-based deployments** - Modern DevOps workflows
- **Premium support available** - Get help when you need it

### 🏢 Enterprise-Grade Control
- **RBAC** - Role-Based Access Control for fine-grained permissions
- **LDAP/AD Integration** - Connect with existing directory services
- **Compliance-grade audit logging** - Meet regulatory requirements
- **Priority support** - Fast response times

---

## ⚡ Quick Start

Get Dockhand running in **30 seconds** with a single command:

### 🐋 Docker Run

```bash
docker run -d \
  --name dockhand \
  --restart unless-stopped \
  -p 3000:3000 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v dockhand_data:/app/data \
  fnsys/dockhand:latest
```

Then open **[http://localhost:3000](http://localhost:3000)** in your browser!

### 📋 Docker Compose (SQLite)

```yaml
services:
  dockhand:
    image: fnsys/dockhand:latest
    container_name: dockhand
    restart: unless-stopped
    ports:
      - 3000:3000
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - dockhand_data:/app/data

volumes:
  dockhand_data:
```

### 🗄️ Docker Compose (PostgreSQL)

```yaml
services:
  postgres:
    image: postgres:16-alpine
    environment:
      POSTGRES_USER: dockhand
      POSTGRES_PASSWORD: changeme
      POSTGRES_DB: dockhand
    volumes:
      - postgres_data:/var/lib/postgresql/data

  dockhand:
    image: fnsys/dockhand:latest
    ports:
      - 3000:3000
    environment:
      DATABASE_URL: postgres://dockhand:changeme@postgres:5432/dockhand
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - dockhand_data:/app/data
    depends_on:
      - postgres

volumes:
  postgres_data:
  dockhand_data:
```

###  🐳 Enable Docker Remote API "Direct Connection"

```bash
# ✏️ Edit Docker systemd service
sudo nano /lib/systemd/system/docker.service

# 🔧 Update ExecStart
ExecStart=/usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
to
ExecStart=/usr/bin/dockerd -H fd:// -H tcp://0.0.0.0:2375 --containerd=/run/containerd/containerd.sock

# 🔄 Reload & Restart Docker
sudo systemctl daemon-reload
sudo systemctl restart docker

# 🔍 Verify Docker is listening on port 2375
netstat -tlpn | grep 2375

# 🧪 Test Docker Remote API
# ℹ️ Get Docker info (remote)
curl http://localhost:2375/info
# 📦 List Docker images (local)
curl http://localhost:2375/images/json
```

###  🐳 Enable Docker Remote API "Standard mode:"
```bash
docker run -d \
  --name hawser \
  --restart unless-stopped \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -p 2376:2376 \
  -e TOKEN=your-secret-token \
  ghcr.io/finsys/hawser:latest
```

###  🐳 Enable Docker Remote API "Edge mode:"
```bash
docker run -d \
  --name hawser \
  --restart unless-stopped \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -e DOCKHAND_SERVER_URL=wss://your-dockhand.example.com/api/hawser/connect \
  -e TOKEN=your-agent-token \
  -e AGENT_NAME=my-server \
  ghcr.io/finsys/hawser:latest
```
---

## 🎯 Top Features

### 🔧 Container Operations
Comprehensive container lifecycle management with powerful controls:
- **Start, stop, restart, remove** containers with one click
- **Create containers** with advanced configuration options
- **View processes, environment variables**, and real-time resource usage
- **Interactive web terminal** - No SSH needed!
- **Browse and transfer files** directly in the interface

### 📦 Compose & Stacks
Deploy and manage complex applications effortlessly:
- **Visual Compose editor** - Say goodbye to YAML headaches
- **Deploy stacks from Git repositories** - Modern GitOps workflow
- **Auto-sync on push** with webhook integration
- **Adopt from other container managers** - Easy migration
- **Scheduled deployments and updates** - Automation made simple

### 📊 Observability
Monitor everything in real-time:
- **Live CPU & memory metrics** per container
- **Real-time log streaming** with ANSI color support
- **Container activity log** - Track all operations
- **Disk usage monitoring** - Stay ahead of storage issues
- **Email and webhook notifications** - Get alerted when it matters

### 🔐 Security
Enterprise-grade security features:
- **OIDC/SSO with any provider** - Free in all editions!
- **LDAP/Active Directory integration** - Enterprise ready
- **Multi-factor authentication (TOTP)** - Extra layer of protection
- **Role-based access control** - Fine-grained permissions
- **Vulnerability scanning** - Grype/Trivy integration

### 🌐 Multi-Host
Manage Docker across multiple environments:
- **Local Docker socket** connection
- **Remote TCP connections** with TLS encryption
- **Hawser agent** for NAT/firewall traversal
- **Environment switching** in one click
- **Dashboard tiles per environment** - Organized view

### 🔄 GitOps
Modern infrastructure as code:
- **Deploy stacks from Git repositories**
- **SSH and HTTPS authentication** support
- **Webhook triggers** for automatic deployments
- **Scheduled sync intervals** - Keep everything up-to-date
- **Multiple repository support** - Manage diverse projects

### 🎨 Customization
Make it yours:
- **Light and dark themes** - Easy on the eyes
- **Configurable font sizes** - Accessibility matters
- **Hide, show, and reorder columns** - Your workflow, your way
- **Resizable dashboard tiles** - Perfect layout every time
- **Persistent per-user preferences** - Saved automatically

### 🔍 Transparency
Open and trustworthy:
- **[Full source code on GitHub](https://github.com/Finsys/dockhand)** - No secrets
- **Licensed under BSL 1.1** - Fair and clear
- **Converts to Apache 2.0 in 2029** - Future-proof licensing
- **Trust, but verify** - You can inspect everything!

---

### 🆚 Dockhand vs Portainer — Feature & Benefit Comparison

| Feature / Aspect                 | **Dockhand**                                                                                                        | **Portainer**                                                                       | **Why Dockhand Could Be Better**                                                      |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| **Primary Focus**                | Docker & Docker-Compose-centric management with stacks, metrics, logs, terminal, and file browsing. ([Dockhand][1]) | Docker + Docker Swarm + Kubernetes multi-environment platform. ([Portainer][2])     | Dockhand stays *simple and focused* on Docker without extra complexity unless needed. |
| **Ease of Setup**                | One-command install, no config files required (very quick). ([Dockhand][1])                                         | Easy install but broader enterprise setup (agents/clusters). ([Portainer][2])       | Faster “zero friction” start for homelabs.                                            |
| **Telemetry / Privacy**          | Zero telemetry and self-hosted only, so no data leaves your network. ([Dockhand][1])                                | Uses agent-based connection; may involve more built-in networking. ([Portainer][2]) | Better for privacy-conscious environments.                                            |
| **User Interface (UI)**          | Modern, intuitive, built-from-scratch web UI. ([Dockhand][1])                                                       | GUI designed for broader orchestration tasks. ([Portainer][2])                      | Many users find Dockhand more *streamlined* and responsive.                           |
| **Container Operations**         | Start/stop/restart, interactive web terminal, process/env view, file browser. ([Dockhand][1])                       | Equivalent container control with logs and metrics. ([Portainer][2])                | Dockhand’s terminal and file transfers are highly integrated without SSH.             |
| **Docker Compose & Stacks**      | Visual editor, adopt external stacks, Git sync with webhooks. ([Dockhand][1])                                       | Stack management & templates, but config stored in DB. ([ITSC News][3])             | Dockhand’s *adopt stacks* and Git-first workflow can be simpler and more transparent. |
| **GitOps / Auto-Deploy**         | Deploy from Git repos with auto-sync. ([Dockhand][1])                                                               | GitOps built-in for Docker/K8s. ([Portainer][2])                                    | Both have Git integration; Dockhand keeps it focused on Docker.                       |
| **Security Features**            | Free OIDC/SSO, MFA, RBAC (Enterprise), LDAP/AD. ([Dockhand][1])                                                     | RBAC, SSO/OIDC, policy controls. ([Portainer][2])                                   | Similar security surface; Dockhand’s defaults aim to be comprehensive without cloud.  |
| **Multi-host / Remote**          | Hawser agent for firewall/NAT traversal. ([Dockhand][1])                                                            | Agent-based multi-environment management. ([Portainer][2])                          | Both support remote; Dockhand’s Hawser focuses on simplicity and firewall traversal.  |
| **Kubernetes Support**           | **No native Kubernetes support** (Docker-only). —                                                                   | **Full Kubernetes & multi-cluster** support. ([Portainer][2])                       | Portainer wins if you need Kubernetes — but Dockhand is lighter if you *don’t*.       |
| **Resource Usage / Lightweight** | Very lightweight; minimal dependencies. ([Dockhand][1])                                                             | More components and backend (DB, broader ecosystem). ([ITSC News][3])               | Dockhand generally uses fewer resources and is faster on small hardware.              |
| **Telemetry & Cloud Lock-in**    | Zero telemetry; strictly self-hosted. ([Dockhand][1])                                                               | Agent model with optional remote control. ([Portainer][2])                          | Better for environments that *must stay local and private*.                           |

---

### 🚀 Why Users Prefer Dockhand (Real Sentiment)

- Many users report Dockhand feels more modern, intuitive, and *just works* compared to Portainer. ([Reddit][4])
- Community feedback suggests easier navigation, quicker operations (prune, search, deploy), and responsive UI. ([Reddit][5])
- Several users are switching from Portainer to Dockhand based on UX, performance, and features. ([Reddit][6])



[1]: https://dockhand.pro/ "Dockhand - Modern Docker Management"
[2]: https://www.portainer.io/ "Kubernetes, Docker and Podman Container Management Platform"
[3]: https://www.itscnews.com/container/top-container-management-tools/ "Top Container Management Tools | ITSC"
[4]: https://www.reddit.com//r/selfhosted/comments/1q4txvu/kudos_to_dockhand_docker_management_done_right/ "Kudos to Dockhand! Docker Management done right."
[5]: https://www.reddit.com//r/docker/comments/1qib1uo/migrate_from_portainer_to_dockhand/ "Migrate from Portainer to Dockhand"
[6]: https://www.reddit.com//r/selfhosted/comments/1qfhwy2/portainer_alternatives/ "Portainer Alternatives?"


---

### 🏁 Summary

**Dockhand is better than Portainer if you want:**

* A *modern, focused Docker manager* without Kubernetes overhead.
* Simple setup, fast UI, and local first privacy.
* Git-centric workflows and compose adoption without hidden YAML storage.

**Portainer might still be better if you need:**

* Kubernetes or multi-cluster orchestration.
* Built-in enterprise governance at scale.


---

## 💰 Pricing

### 🆓 Free Edition
**Free forever. No bait-and-switch.**

- ✅ Unlimited environments
- ✅ Full container & stack management
- ✅ Git integration & webhooks
- ✅ Vulnerability scanning
- ✅ OIDC/SSO authentication
- ✅ Multi-factor authentication (TOTP)
- ✅ Local user accounts
- ✅ Notifications & container activity log

**Price:** $0/month  
🎁 [Buy me a coffee](https://buymeacoffee.com/dockhand) to support development!

---

### 💼 SMB Edition
**For commercial use. Growing teams, happy CFOs.**

Everything in Free, plus:
- ✅ Commercial usage license
- ✅ Premium email support
- ✅ Priority bug fixes

**Price:** $499/host/year  
🛒 [Buy License](https://buy.stripe.com/8x24gA93CeJZg6cb8y6oo01)

---

### 🏢 Enterprise Edition
**When compliance asks "is it enterprise-ready?" and you want to say yes.**

Everything in SMB, plus:
- ✅ LDAP/Active Directory integration
- ✅ Role-based access control
- ✅ Environment-scoped permissions
- ✅ Audit logging (compliance-grade)
- ✅ Priority email support

**Price:** $1,499/host/year  
🛒 [Buy License](https://buy.stripe.com/6oUfZi93C59pg6c7Wm6oo00)

📧 **Questions?** Contact [enterprise@dockhand.pro](mailto:enterprise@dockhand.pro) for custom quotes!

---

## 🎬 Key Highlights for Your Video

1. **⚡ Lightning Setup** - Up and running in 30 seconds with one command
2. **🎨 Beautiful UI** - Modern, intuitive interface with dark/light themes
3. **🔓 Free SSO** - Enterprise authentication included in the free tier
4. **🐧 Raspberry Pi Ready** - Lightweight enough for homelab setups
5. **🔍 Open Source** - Full transparency with code on GitHub (BSL 1.1)
6. **🚀 GitOps Native** - Deploy from Git with automatic webhooks
7. **🔐 Security First** - Vulnerability scanning, RBAC, and MFA built-in
8. **📊 Real-time Monitoring** - Live metrics, logs, and notifications
9. **🌐 Multi-host Management** - Control Docker across multiple servers
10. **💰 Fair Pricing** - Free forever for personal use, reasonable commercial pricing

---

## 🛣️ Roadmap

Dockhand is actively developed with new features being added regularly. Check the [official roadmap](https://dockhand.pro/#roadmap) for upcoming features and improvements!

---

## 📚 Resources

- 🌐 **Website:** [https://dockhand.pro/](https://dockhand.pro/)
- 🌐 **Hawser Modes:** [https://dockhand.pro/manual/#hawser-modes](https://dockhand.pro/manual/#hawser-modes)
- 🌐 **RBAC:** [https://dockhand.pro/manual/#enterprise-rbac](https://dockhand.pro/manual/#enterprise-rbac)
- 🌐 **OIDC:** [https://dockhand.pro/manual/#appendix-oidc](https://dockhand.pro/manual/#appendix-oidc)
- 🌐 **LDAP:** [https://dockhand.pro/manual/#appendix-ldap](https://dockhand.pro/manual/#appendix-ldap)
- 🌐 **API Reference:** [https://dockhand.pro/manual/#api-reference](https://dockhand.pro/manual/#api-reference)
- 📦 **GitHub:** [https://github.com/Finsys/dockhand](https://github.com/Finsys/dockhand)
- 📧 **Enterprise Support:** [enterprise@dockhand.pro](mailto:enterprise@dockhand.pro)
- ☕ **Support Development:** [Buy me a coffee](https://buymeacoffee.com/dockhand)

---

## 🎉 Conclusion

Dockhand is the **modern Docker management platform** that combines power, simplicity, and transparency. Whether you're running a homelab on a Raspberry Pi or managing enterprise containers across multiple environments, Dockhand has you covered.

**Stop pretending you enjoy YAML indentation. Try Dockhand today!** 🚀

---
## 📝 License

This guide is provided as-is for educational and professional use.

## 🤝 Contributing
Feel free to suggest improvements or report issues via pull requests or the issues tab.

## 💼 Connect with Me 👇😊

*   🔥 [**YouTube**](https://www.youtube.com/@DevOpsinAction?sub_confirmation=1)
*   ✍️ [**Blog**](https://ibraransari.blogspot.com/)
*   💼 [**LinkedIn**](https://www.linkedin.com/in/ansariibrar/)
*   👨‍💻 [**GitHub**](https://github.com/meibraransari?tab=repositories)
*   💬 [**Telegram**](https://t.me/DevOpsinActionTelegram)
*   🐳 [**Docker Hub**](https://hub.docker.com/u/ibraransaridocker)

### ⭐ If You Found This Helpful...

***Please star the repo and share it! Thanks a lot!*** 🌟
