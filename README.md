<img src="assets/banner.svg" alt="Clément Segura — Infra · Sécurité · IA" width="100%">

**En reconversion vers l'administration d'infrastructures sécurisées** — titre pro RNCP37680, formation à partir de décembre 2026.
L'IA est au centre de ma façon de travailler : je monte avec elle ce qui doit tourner, puis je le décortique jusqu'à savoir expliquer chaque brique.

---

<img src="assets/architecture.jpg" alt="Workflow de développement (spec, run autonome, porte de tests, relecture, ship) et architecture du homelab Proxmox" width="100%">

### 🤖 Mon atelier IA

Une session Claude Code tourne en permanence sur mon serveur. Je la pilote depuis le PC ou le téléphone, elle écrit les cahiers des charges avec moi, lance des agents autonomes, relit leur travail et publie.

<details>
<summary>Le flux en version texte</summary>

```mermaid
flowchart LR
  me(["👤 Moi<br/>PC · téléphone"]) -- "discussion cadrée" --> brain["🧠 Session cerveau<br/>Claude Code · Opus<br/>toujours allumée"]
  vault[("📚 Mémoire<br/>vault Obsidian")] <-.-> brain
  brain -- "cahier des charges" --> run["⚙️ Run autonome<br/>agent sans droit de commit"]
  run --> gate{"🚦 Porte<br/>mécanique"}
  gate -- "rouge" --> run
  gate -- "vert" --> review["🔍 Relecture<br/>à froid"]
  review --> ship["🚀 ship<br/>scan secrets · validateurs<br/>1 commit = 1 retour arrière"]
  ship --> notif["📱 Notif"]
  ship --> gh["GitHub"]
```

</details>

- **Agents encadrés** — une run autonome ne peut ni committer ni se déclarer « finie » si la porte de tests est rouge ; kill-switch, une run à la fois
- **Un humain au bon endroit** — rien ne part sans mon « go » ; la publication suit une politique écrite par projet
- **Mémoire longue** — vault Obsidian synchronisé sur mes appareils, lu par un sous-agent léger pour ne pas saturer le contexte
- **Garde-fous automatiques** — hooks : scan de secrets, plan écrit obligatoire avant de toucher la sécurité ou les migrations
- **Bon modèle au bon poste** — le plus petit modèle qui ne fera pas recommencer : gros modèle pour le jugement, petit pour l'extraction
- **Boucles autonomes bridées** — une veille IA hebdomadaire tourne seule : lecture web uniquement, écrit dans le vault, modèle économique, kill-switch
- **Bibliothèque** — 110+ fiches de lecture et synthèses de recherche, ingérées depuis des PDF, consultables par les agents
- **Poste de travail** — PC Windows double écran, VM Windows 11 pilotée depuis le serveur pour tester les builds, session principale joignable depuis le téléphone
- **Passerelle LLM maison** — LiteLLM devant plusieurs fournisseurs, orchestration Dify et n8n pour les apps

### 🧱 Le homelab

<details>
<summary>Le homelab en version texte</summary>

```mermaid
flowchart LR
  user(["👤 Utilisateurs"]) --> cf["☁️ Cloudflare<br/>tunnel + Zero Trust"]
  admin(["🛠️ Moi · admin<br/>PC · téléphone"]) -. "Tailscale<br/>mesh WireGuard" .-> pve
  cf --> apps

  subgraph pve["🖥️ Proxmox — nœud unique"]
    direction TB
    subgraph prod["VM services · Debian · durcie"]
      llm["Passerelle LLM<br/>LiteLLM"]
      db[("PostgreSQL")]
      apps["💤 Apps web + API<br/>Dify · n8n<br/>(en veille)"]
    end
    lab["VM lab · Debian"]
    win["VM build · Windows 11"]
    fut["🔨 À venir<br/>Zones cloisonnées · firewall<br/>AD / GPO · supervision · lab cyber"]
  end

  prod -- "restic, restauration testée" --> bk[("🔒 Sauvegardes<br/>chiffrées hors site")]
  pve -- "vzdump chiffré" --> bk
  prod -. "santé · intégrité · audit" .-> phone["📱 Alertes"]

  classDef todo stroke-dasharray: 5 5,fill:none
  classDef sleep stroke-dasharray: 3 3,fill:#eeeeee,color:#777777
  class fut todo
  class apps sleep
```

</details>

- **Publication par tunnel sortant** — Cloudflare Tunnel + Zero Trust Access devant chaque service, pas de redirection de port
- **Administration à distance par réseau privé** — Tailscale (mesh WireGuard) entre PC, téléphone, hyperviseur et serveur
- **Sauvegardes vérifiées, pas supposées** — restic avec test de restauration automatique, copie froide, sauvegardes de VM chiffrées hors site
- **Secrets hors du code** — chiffrés (sops), et chaque commit passe un détecteur de secrets
- **Supervision** — tests de santé planifiés (timers systemd), alertes sur téléphone
- **Procédures de reprise écrites** — et déjà utilisées sur incident réel

**En construction — `socle`**, selon quatre règles : les frontières réseau sont *appliquées*, pas supposées · tout est *reconstructible*, et ça a été prouvé · on sait que c'est cassé *avant* l'utilisateur · chaque décision est écrite, le *pourquoi* plutôt que le *quoi*.
Première décision sur la table : Tailscale passe par-dessus tout cloisonnement — son statut se tranche *avant* de poser les zones, pas après.

### 🛡️ Sécurité

**Sur le serveur**
- SSH par clé uniquement, port non standard · fail2ban · pare-feu ufw · mises à jour de sécurité automatiques
- **auditd**, **AIDE** (intégrité des fichiers, contrôle hebdo) et **Lynis** (audit système hebdo) — alertes sur téléphone
- Conteneurs durcis (capabilities réduites au strict nécessaire), socket Docker derrière un proxy
- **Bac à sable** pour tout code tiers : système de fichiers isolé, sorties réseau sur liste blanche

**Dans les applications** (AcademIA)
- Sessions opaques Redis + CSRF, mots de passe Argon2id, CSP en mode bloquant, MFA TOTP (secret chiffré au repos)
- LLM : nettoyage des données personnelles, isolation entre utilisateurs, limites de débit et de coût
- Cloudflare : WAF, HSTS, DMARC · dépendances surveillées (Dependabot + audit en CI)
- **RGPD** : analyse d'impact, registre des traitements, endpoints de demande d'accès et d'effacement

### 📦 Projets

<!-- vitrine:projets:debut -->
| | Projet | Ce que c'est | État |
|---|---|---|---|
| 🧱 | **socle** | Refonte du homelab : zones cloisonnées, AD/GPO, supervision, terrain cyber | ![en construction](https://img.shields.io/badge/en_construction-f0883e?style=flat-square) |
| 🎓 | **Formation AIS** | Préparation du titre RNCP37680, suivi de progression maison | ![démarrage déc. 2026](https://img.shields.io/badge/démarrage_déc._2026-58a6ff?style=flat-square) |
| 🎮 | **ColdBoot** | Jeu de hacking narratif (Godot), façon Uplink / Hacknet — construit par agents autonomes | ![actif](https://img.shields.io/badge/actif-3fb950?style=flat-square) |
| 🗣️ | **AcademIA** | Apprentissage des langues piloté par LLM, pédagogie adaptative | ![en pause](https://img.shields.io/badge/en_pause-8b949e?style=flat-square) |
| 🏎️ | **RaceIA** | Ingénieur de course IA pour Assetto Corsa : télémétrie, analyse des virages, briefing | ![en pause](https://img.shields.io/badge/en_pause-8b949e?style=flat-square) |
| 🐉 | **MJIA** | Copilote IA de maître du jeu D&D 5e sur Foundry VTT auto-hébergé | ![en pause](https://img.shields.io/badge/en_pause-8b949e?style=flat-square) |
| ✅ | **Eisenday** | App mobile de productivité (matrice d'Eisenhower), 13 langues | ![en pause](https://img.shields.io/badge/en_pause-8b949e?style=flat-square) |
<!-- vitrine:projets:fin -->

### 🛠 Outils

**Infra** ![Proxmox](https://img.shields.io/badge/Proxmox-E57000?style=flat-square&logo=proxmox&logoColor=white) ![Debian](https://img.shields.io/badge/Debian-A81D33?style=flat-square&logo=debian&logoColor=white) ![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white) ![Cloudflare Zero Trust](https://img.shields.io/badge/Cloudflare_Zero_Trust-F38020?style=flat-square&logo=cloudflare&logoColor=white) ![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white) ![restic](https://img.shields.io/badge/restic-2F3640?style=flat-square) ![sops](https://img.shields.io/badge/sops-2F3640?style=flat-square) ![Tailscale](https://img.shields.io/badge/Tailscale-242424?style=flat-square&logo=tailscale&logoColor=white) ![Windows 11](https://img.shields.io/badge/Windows_11-0078D4?style=flat-square&logo=windows&logoColor=white)

**IA** ![Claude Code](https://img.shields.io/badge/Claude_Code-D97757?style=flat-square&logo=claude&logoColor=white) ![LiteLLM](https://img.shields.io/badge/LiteLLM-4B0082?style=flat-square) ![Dify](https://img.shields.io/badge/Dify-1C64F2?style=flat-square) ![n8n](https://img.shields.io/badge/n8n-EA4B71?style=flat-square&logo=n8n&logoColor=white) ![Obsidian](https://img.shields.io/badge/Obsidian-7C3AED?style=flat-square&logo=obsidian&logoColor=white)

**Dev** ![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white) ![Bash](https://img.shields.io/badge/Bash-4EAA25?style=flat-square&logo=gnubash&logoColor=white) ![SvelteKit](https://img.shields.io/badge/SvelteKit-FF3E00?style=flat-square&logo=svelte&logoColor=white) ![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white) ![Godot](https://img.shields.io/badge/Godot-478CBF?style=flat-square&logo=godotengine&logoColor=white)

### 📍 Contact

`sinseproduction@gmail.com` · Nîmes (Gard) — bassin montpelliérain
