# PoC Plan: VLM-AutoYOLO

## Project Classification
- **Type:** web-app (with ML backend)
- **Key Technologies:** Python 3.12, FastAPI, PyTorch, Ultralytics YOLO, SAM2, React, PostgreSQL, Nginx
- **ODH Relevance:** Demonstrates end-to-end ML-powered auto-labeling pipeline on OpenShift AI, combining VLMs with YOLO for automated image annotation workflows.

## PoC Objectives
1. Validate the backend API server starts and serves health endpoints on OpenShift
2. Confirm the frontend static assets are served correctly via nginx
3. Test API endpoint availability without GPU model loading
4. Demonstrate the multi-component deployment pattern (backend + frontend + database)

## Infrastructure Requirements
- **Resource Profile:** medium (1Gi RAM, 500m CPU for backend without GPU models)
- **GPU Required:** No (testing API layer only, model loading skipped)
- **Persistent Storage:** None for PoC (database in ephemeral pod)
- **Deployment Model:** deployment (long-running services)
- **Listens on Port:** Backend 8080, Frontend 8080
- **Needs LLM API:** No

## Components for PoC
- **backend** - FastAPI server (CPU-only mode, no model loading)
- **frontend** - React app served via nginx

## Test Scenarios

### Scenario 1: Backend Health Check
- **Type:** http
- **Endpoint:** /api/health
- **Expected:** Returns 200 OK
- **Timeout:** 30s

### Scenario 2: Frontend Static Assets
- **Type:** http
- **Endpoint:** /
- **Expected:** Returns 200 with HTML content
- **Timeout:** 30s

### Scenario 3: API Docs
- **Type:** http
- **Endpoint:** /docs
- **Expected:** Returns 200 with Swagger UI
- **Timeout:** 30s

## Dockerfile Considerations
- Backend: Use UBI Python 3.12, install CPU-only PyTorch, skip SAM2/GPU deps
- Frontend: Multi-stage build with Node.js for building, UBI nginx for serving
- Backend needs PostgreSQL connection (use embedded SQLite for PoC simplicity)
