# crazyflie
# Crazyflie @ UIUC Center for Autonomy

Central index for all Crazyflie 2.1 work at the UIUC Coordinated Science Laboratory (CSL), Center for Autonomy (CfA): showcase demos, ECE484 course material, lab research, hardware setup, and safety procedures.

> **Start here.** This repo holds no flight code. It points you to the right repo, the right person, and the right setup doc.

---

## Table of Contents

1. [Who This Is For](#who-this-is-for)
2. [Repository Map](#repository-map)
3. [Research Projects](#research-projects)
4. [Hardware Inventory](#hardware-inventory)
5. [Lab Infrastructure](#lab-infrastructure)
6. [Getting Started](#getting-started)
7. [Safety and Pre-Flight Runbook](#safety-and-pre-flight-runbook)
8. [Running the Showcase Demo](#running-the-showcase-demo)
9. [Conventions](#conventions)
10. [Contributing a Repo](#contributing-a-repo)
11. [Troubleshooting](#troubleshooting)
12. [Contacts](#contacts)
13. [Changelog](#changelog)

---

## Who This Is For

| You are... | Go to |
|---|---|
| New lab member onboarding onto Crazyflies | [Getting Started](#getting-started), then [Safety](#safety-and-pre-flight-runbook) |
| ECE484 TA or course staff | `fa26-drone-track-release` in [Repository Map](#repository-map) |
| Running a demo for visitors or students | [Running the Showcase Demo](#running-the-showcase-demo) |
| Researcher looking for existing work | [Research Projects](#research-projects) |
| Adding your own Crazyflie repo | [Contributing a Repo](#contributing-a-repo) |

---

## Repository Map

| Repo | Purpose | Visibility | Owner | Status |
|---|---|---|---|---|
| [`crazyflie-hub`](.) | This index | Private (lab) | Amey Gavale | Active |
| `crazyflie-demo` | Showcase flight package for CfA visitors and ECE484 demos | Private (staff only) | Amey Gavale | Active |
| `safeautonomy-illinois-students/fa26-drone-track-release` | ECE484 Fall '26 student starter code | Course org | Amey Gavale / ECE484 staff | Active |
| `<repo-name>` | AI Deck camera streaming and vision stack | `<TBD>` | Teodor Tchalakov | Active |
| `<repo-name>` | Vision-based Crazyflie controllers | `<TBD>` | Krishna, Chenxi | Active |

**Visibility rules**
- **Staff only:** `crazyflie-demo` is built on a past top student project. Do not share it with current ECE484 students.
- **Students:** get code only through the course org starter repo.
- **External visitors:** get a live flight and the demo video, not repo access. Exceptions need PI approval.

---

## Research Projects

| Project | Lead | Summary | Repo | Status |
|---|---|---|---|---|
| Showcase demo package | Amey | Three-lap autonomous track run with OptiTrack localization | `crazyflie-demo` | Active |
| AI Deck vision stack | Teodor | Onboard camera streaming and perception | `<link>` | Active |
| Vision-based control | Krishna, Chenxi | Learned controllers deployed on CF hardware | `<link>` | Active |
| Self-righting after crash | Amey | Autonomous flip recovery | `<TBD>` | Exploratory |

---

## Hardware Inventory

| Item | Details | Notes |
|---|---|---|
| Crazyflie 2.1 | `cf1`, `cf2` | Each has its own mocap name, TOML config, and radio address |
| Crazyradio PA | Radio ID 2, channel 100 | Confirm channel before every session |
| AI Deck | `<count>` | Used by the vision stack |
| Batteries | `<count>` | See battery rules in the runbook |
| Spare props and motors | `<location>` | |

Hardware build questions and new drone builds: see [Contacts](#contacts).

---

## Lab Infrastructure

| System | Details |
|---|---|
| Motion capture | OptiTrack, streamed over VRPN |
| VRPN host | `192.168.1.114:3883` |
| Flight space | `<room / building>` |
| Lab access | `<process / link>` |

---

## Getting Started

### 1. Prerequisites
- Ubuntu `<version>`
- ROS 2 `<distro>`
- [Pixi](https://pixi.sh) package manager
- Crazyradio USB permissions (udev rules)

### 2. Clone and set up
```bash
git clone git@github.com:UIUC-Robotics/<repo-name>.git
cd <repo-name>
pixi install
pixi shell -e deploy
```

### 3. Verify the radio
```bash
# <command to scan for Crazyflies>
```

### 4. Verify mocap
```bash
# <command to check VRPN stream and rigid body names>
```

### 5. First flight
Do not fly until you have completed all three phases of the [Pre-Flight Runbook](#safety-and-pre-flight-runbook) with a lab member present.

---

## Safety and Pre-Flight Runbook

**Rule zero:** props-off full-stack bench testing is required before any live flight.

| Phase | State | Checks |
|---|---|---|
| A | Unpowered | Mocap rigid body visible, signed frame verification (move the drone by hand, confirm axis signs match), correct `cfX` config loaded |
| B | Powered, props off | Radio link stable, stale-pose watchdog triggers correctly, kill switch tested and working |
| C | Props on | Short hover at low altitude, observer at the kill switch, battery above threshold |

**Key safety notes**
- **Signed frame verification beats "does it move" checks.** A drone that moves the wrong way still moves.
- **Kill switch:** Wayland can block global keyboard capture. Use Xorg, or verify the kill switch works on your session before flight.
- **Batteries:** `<voltage threshold, charging rules, storage rules>`
- **Never fly alone.**

Full runbook: `<link to runbook doc>`

---

## Running the Showcase Demo

1. Confirm OptiTrack is calibrated and streaming.
2. Pick the drone (`cf1` or `cf2`) and load its config.
3. Complete runbook Phases A, B, C.
4. Run:
   ```bash
   cd crazyflie-demo
   pixi shell -e deploy
   python deploy.py --drone cfX
   ```
5. Keep one person on the kill switch for the full run.

Demo video: `<link>`
Visitor talking points: `<link>`

---

## Conventions

- **Drone naming:** refer to drones as `cfX` in docs. Real names are `cf1`, `cf2`.
- **Python package name:** keep `ece484_fly` unchanged to avoid breaking imports, even if the repo name differs.
- **Repo naming:** `crazyflie-<purpose>` (for example `crazyflie-demo`, `crazyflie-vision`).
- **Topic tag:** every Crazyflie repo in this org gets the GitHub topic `crazyflie`.
- **No student data:** strip GitHub Classroom badges, student Slack links, and personal info before pushing to this org.

---

## Contributing a Repo

1. Talk to the hub maintainer first.
2. Choose: transfer your repo into `UIUC-Robotics`, or keep it where it is and link it here.
3. Add the `crazyflie` topic to your repo.
4. Make sure your repo README has: purpose, setup steps, how to run, and an owner.
5. Open a PR on this hub adding a row to [Repository Map](#repository-map) and [Research Projects](#research-projects).

---

## Troubleshooting

| Symptom | Likely cause | Fix |
|---|---|---|
| Drone not found on radio scan | Wrong channel or address | Check the `cfX` TOML config and radio ID |
| No pose data | VRPN not streaming or wrong rigid body name | Check the mocap host and the rigid body name for that drone |
| Drone drifts the wrong way | Frame sign mismatch | Redo signed frame verification |
| Kill switch does nothing | Wayland session | Switch to Xorg |

---

## Contacts

| Topic | Person |
|---|---|
| Hub maintainer, demo package, ECE484 drone track | Amey Gavale |
| PI, demo milestones | Prof. Sayan Mitra |
| Drone builds, hardware | Abhishek Pai |
| Vision stack, AI Deck | Teodor Tchalakov |
| Vision-based controllers | Krishna, Chenxi |
| Lab access, sim-to-real support | JD Lee |

---

## Changelog

| Date | Change |
|---|---|
| 2026-10-16 | Hub created |# Crazyflie @ UIUC Center for Autonomy

Central index for all Crazyflie 2.1 work at the UIUC Coordinated Science Laboratory (CSL), Center for Autonomy (CfA): showcase demos, ECE484 course material, lab research, hardware setup, and safety procedures.

> **Start here.** This repo holds no flight code. It points you to the right repo, the right person, and the right setup doc.

---

## Table of Contents

1. [Who This Is For](#who-this-is-for)
2. [Repository Map](#repository-map)
3. [Research Projects](#research-projects)
4. [Hardware Inventory](#hardware-inventory)
5. [Lab Infrastructure](#lab-infrastructure)
6. [Getting Started](#getting-started)
7. [Safety and Pre-Flight Runbook](#safety-and-pre-flight-runbook)
8. [Running the Showcase Demo](#running-the-showcase-demo)
9. [Conventions](#conventions)
10. [Contributing a Repo](#contributing-a-repo)
11. [Troubleshooting](#troubleshooting)
12. [Contacts](#contacts)
13. [Changelog](#changelog)

---

## Who This Is For

| You are... | Go to |
|---|---|
| New lab member onboarding onto Crazyflies | [Getting Started](#getting-started), then [Safety](#safety-and-pre-flight-runbook) |
| ECE484 TA or course staff | `fa26-drone-track-release` in [Repository Map](#repository-map) |
| Running a demo for visitors or students | [Running the Showcase Demo](#running-the-showcase-demo) |
| Researcher looking for existing work | [Research Projects](#research-projects) |
| Adding your own Crazyflie repo | [Contributing a Repo](#contributing-a-repo) |

---

## Repository Map

| Repo | Purpose | Visibility | Owner | Status |
|---|---|---|---|---|
| [`crazyflie-hub`](.) | This index | Private (lab) | Amey Gavale | Active |
| `crazyflie-demo` | Showcase flight package for CfA visitors and ECE484 demos | Private (staff only) | Amey Gavale | Active |
| `safeautonomy-illinois-students/fa26-drone-track-release` | ECE484 Fall '26 student starter code | Course org | Amey Gavale / ECE484 staff | Active |
| `<repo-name>` | AI Deck camera streaming and vision stack | `<TBD>` | Teodor Tchalakov | Active |
| `<repo-name>` | Vision-based Crazyflie controllers | `<TBD>` | Krishna, Chenxi | Active |

**Visibility rules**
- **Staff only:** `crazyflie-demo` is built on a past top student project. Do not share it with current ECE484 students.
- **Students:** get code only through the course org starter repo.
- **External visitors:** get a live flight and the demo video, not repo access. Exceptions need PI approval.

---

## Research Projects

| Project | Lead | Summary | Repo | Status |
|---|---|---|---|---|
| Showcase demo package | Amey | Three-lap autonomous track run with OptiTrack localization | `crazyflie-demo` | Active |
| AI Deck vision stack | Teodor | Onboard camera streaming and perception | `<link>` | Active |
| Vision-based control | Krishna, Chenxi | Learned controllers deployed on CF hardware | `<link>` | Active |
| Self-righting after crash | Amey | Autonomous flip recovery | `<TBD>` | Exploratory |

---

## Hardware Inventory

| Item | Details | Notes |
|---|---|---|
| Crazyflie 2.1 | `cf1`, `cf2` | Each has its own mocap name, TOML config, and radio address |
| Crazyradio PA | Radio ID 2, channel 100 | Confirm channel before every session |
| AI Deck | `<count>` | Used by the vision stack |
| Batteries | `<count>` | See battery rules in the runbook |
| Spare props and motors | `<location>` | |

Hardware build questions and new drone builds: see [Contacts](#contacts).

---

## Lab Infrastructure

| System | Details |
|---|---|
| Motion capture | OptiTrack, streamed over VRPN |
| VRPN host | `192.168.1.114:3883` |
| Flight space | `<room / building>` |
| Lab access | `<process / link>` |

---

## Getting Started

### 1. Prerequisites
- Ubuntu `<version>`
- ROS 2 `<distro>`
- [Pixi](https://pixi.sh) package manager
- Crazyradio USB permissions (udev rules)

### 2. Clone and set up
```bash
git clone git@github.com:UIUC-Robotics/<repo-name>.git
cd <repo-name>
pixi install
pixi shell -e deploy
```

### 3. Verify the radio
```bash
# <command to scan for Crazyflies>
```

### 4. Verify mocap
```bash
# <command to check VRPN stream and rigid body names>
```

### 5. First flight
Do not fly until you have completed all three phases of the [Pre-Flight Runbook](#safety-and-pre-flight-runbook) with a lab member present.

---

## Safety and Pre-Flight Runbook

**Rule zero:** props-off full-stack bench testing is required before any live flight.

| Phase | State | Checks |
|---|---|---|
| A | Unpowered | Mocap rigid body visible, signed frame verification (move the drone by hand, confirm axis signs match), correct `cfX` config loaded |
| B | Powered, props off | Radio link stable, stale-pose watchdog triggers correctly, kill switch tested and working |
| C | Props on | Short hover at low altitude, observer at the kill switch, battery above threshold |

**Key safety notes**
- **Signed frame verification beats "does it move" checks.** A drone that moves the wrong way still moves.
- **Kill switch:** Wayland can block global keyboard capture. Use Xorg, or verify the kill switch works on your session before flight.
- **Batteries:** `<voltage threshold, charging rules, storage rules>`
- **Never fly alone.**

Full runbook: `<link to runbook doc>`

---

## Running the Showcase Demo

1. Confirm OptiTrack is calibrated and streaming.
2. Pick the drone (`cf1` or `cf2`) and load its config.
3. Complete runbook Phases A, B, C.
4. Run:
   ```bash
   cd crazyflie-demo
   pixi shell -e deploy
   python deploy.py --drone cfX
   ```
5. Keep one person on the kill switch for the full run.

Demo video: `<link>`
Visitor talking points: `<link>`

---

## Conventions

- **Drone naming:** refer to drones as `cfX` in docs. Real names are `cf1`, `cf2`.
- **Python package name:** keep `ece484_fly` unchanged to avoid breaking imports, even if the repo name differs.
- **Repo naming:** `crazyflie-<purpose>` (for example `crazyflie-demo`, `crazyflie-vision`).
- **Topic tag:** every Crazyflie repo in this org gets the GitHub topic `crazyflie`.
- **No student data:** strip GitHub Classroom badges, student Slack links, and personal info before pushing to this org.

---

## Contributing a Repo

1. Talk to the hub maintainer first.
2. Choose: transfer your repo into `UIUC-Robotics`, or keep it where it is and link it here.
3. Add the `crazyflie` topic to your repo.
4. Make sure your repo README has: purpose, setup steps, how to run, and an owner.
5. Open a PR on this hub adding a row to [Repository Map](#repository-map) and [Research Projects](#research-projects).

---

## Troubleshooting

| Symptom | Likely cause | Fix |
|---|---|---|
| Drone not found on radio scan | Wrong channel or address | Check the `cfX` TOML config and radio ID |
| No pose data | VRPN not streaming or wrong rigid body name | Check the mocap host and the rigid body name for that drone |
| Drone drifts the wrong way | Frame sign mismatch | Redo signed frame verification |
| Kill switch does nothing | Wayland session | Switch to Xorg |

---

## Contacts

| Topic | Person |
|---|---|
| Hub maintainer, demo package, ECE484 drone track | Amey Gavale |
| PI, demo milestones | Prof. Sayan Mitra |
| Drone builds, hardware | Abhishek Pai |
| Vision stack, AI Deck | Teodor Tchalakov |
| Vision-based controllers | Krishna, Chenxi |
| Lab access, sim-to-real support | JD Lee |

---

## Changelog

| Date | Change |
|---|---|
| 2026-10-16 | Hub created |
