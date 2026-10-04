# TORQUE IQ - AI-Assisted Green Fleet Planning & Optimization

Maritime fleet planning and decision-support platform built for **Smart India Hackathon 2026**
(Problem Statement **SIH26138**: Quantum-Inspired Fuel Consumption Prediction and Green Fleet Optimization; Theme: Smart Vehicles; Category: Software; Proposer: Egreen Quanta; Team: TORQUE IQ).

A human planner makes the final decision. Nothing controls a vessel. Demo mode uses a fictional fleet, simulated weather and illustrative fuel prices.

## The idea in one workflow
Cargo order -> possible fleet plans -> fuel estimation -> optimization -> constraint check -> plan comparison -> trade-off explanation -> human decision.

## Quick start
```bash
npm install
npm run dev            # http://localhost:5173   (demo mode: runs fully in the browser)
npm run typecheck
npm test               # engine tests: fuel model, plans, optimizer vs exact optimum

# optional backend (target API boundary)
cd backend
python -m venv .venv && source .venv/bin/activate     # Windows: .venv\Scripts\activate
pip install -r requirements.txt
python tests/test_engine.py                            # parity with the TypeScript engine + optimizer tests
uvicorn app.main:app --reload --port 8000              # Vite proxies /api to this port
```
Optional: add a licensed photo at `public/images/hero-ship.jpg` for the landing hero (see `public/images/README.md`).

## Demo script (3 minutes)
1. Landing -> Open planning workspace.
2. **Overview**: map, KPIs, planning attention.
3. **Voyage Planner**: walk the 8 steps -> Generate plans -> change Objective priorities -> open **Why this plan?** -> Decide.
4. **What-If**: set speed 15 kn -> 13 kn and read the fuel / cost / ETA deltas.
5. **Optimization Lab**: Run all methods and compare; open Technical view and the Benchmark. Point out that the genetic algorithm currently beats the quantum-inspired variants, and that this is reported honestly.

## Structure
```
src/domain/      types, demo data, fuel model, constraints, plan generator, explanations
src/optimizer/   algorithms, exact DP reference, synthetic benchmark data
src/state/       shared scenario state (planner, what-if, overview, copilot use the same state)
src/pages/       Landing, Overview, VoyagePlanner, OptimizationLab, WhatIf, Fleet, RoutesPage, Green, DataAssumptions, Planned
backend/app/     FastAPI app, Python engine (fuel.py, optimizer.py), pydantic models
backend/benchmark/ Python benchmark scripts and results
docs/            product architecture, page map, user journey, design system, data model, technical architecture, implemented vs planned
```

## Honesty rules built into the product
Data badges (DEMO / SIMULATED / PUBLIC / LIVE / PLANNED / DATA PENDING) on every metric source; lifecycle emission factors are DATA PENDING; "recommended under current objectives" instead of "best"; no certificates; no live-data claims. See `docs/07-implemented-vs-planned.md`.

## Build note
The web app uses Vite 5, React 18, TypeScript, Tailwind 3 and Recharts. In the sandbox where this was assembled there was no internet, so the TypeScript check (`tsc`), the engine tests and a browser run were done with an esbuild bundle; run `npm install && npm run build` on your machine to produce the production build.
