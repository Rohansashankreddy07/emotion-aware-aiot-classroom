# Emotion-Aware AIoT — Classroom Attendance and Expression Analysis

> **Status: Implementation in progress · Partially working system reported by the owner**

This project combines edge computer vision, attendance records, experimental facial-expression analysis, and a web dashboard. The current direction uses a **Raspberry Pi 5**, a USB camera, Python, FastAPI, OpenCV, ONNX Runtime, and SQLite.

The project handoff describes integrated recognition, inference, streaming, backend, and dashboard components. The owner reports partial operation; accuracy, tracking stability, and runtime performance still need improvement.

## Contents

- [The problem](#the-problem)
- [Project idea](#project-idea)
- [System architecture](#system-architecture)
- [Technology stack](#technology-stack)
- [Understanding the outputs](#understanding-the-outputs)
- [Current development status](#current-development-status)
- [Validation priorities](#validation-priorities)
- [Development roadmap](#development-roadmap)
- [Repository purpose](#repository-purpose)

## The problem

Classroom monitoring combines several different technical problems: finding a face in a frame, recognizing a registered person, recording attendance without duplicates, maintaining identities across time, and presenting reliable information to a teacher.

An expression-analysis layer introduces additional uncertainty. A facial-expression label can be affected by lighting, pose, occlusion, and model limitations. The interface must communicate those limits instead of presenting an inference as a fact about a student.

## Project idea

Run as much of the vision pipeline as practical on local edge hardware, connect it to an API and database, and provide a live dashboard. Attendance and experimental expression outputs should remain distinguishable in both storage and presentation.

The immediate priority is to make the existing pipeline reliable and measurable before introducing larger models or a new architecture.

## System architecture

The documented processing flow is:

1. A USB camera supplies frames to OpenCV.
2. Face detection and tracking locate people across frames.
3. LBPH recognition attempts to match faces to registered students.
4. FER+ through ONNX Runtime produces experimental expression predictions.
5. The application records attendance and relevant analysis information.
6. FastAPI connects processing, database operations, and dashboard access.
7. The dashboard displays live video and monitoring information, including an MJPEG stream.

Detection, recognition, tracking, and expression inference each need their own error handling and evaluation. A failure in one stage should not silently become a confident downstream result.

## Technology stack

| Layer | Technology in the project handoff |
|---|---|
| Language and API | Python, FastAPI |
| Frame processing | OpenCV, MediaPipe |
| Recognition | LBPH |
| Expression model | FER+ with ONNX Runtime |
| Data work | NumPy, Pandas |
| Storage | SQLite |
| Edge hardware | Raspberry Pi 5 and USB camera |
| Monitoring | Web dashboard and MJPEG video stream |

PostgreSQL is a possible future direction. It is not declared the current database. Exact dependency versions and deployment commands need to be recovered from the implementation source.

## Understanding the outputs

| Output | What it can represent | What it does not establish |
|---|---|---|
| Face detection | A candidate face region | The person's identity |
| Recognition | A match estimate against registered examples | Guaranteed identity in all conditions |
| Attendance | A recorded presence event based on defined rules | Participation or learning |
| Expression prediction | A model's estimate of visible facial expression | A person's internal emotional state |
| Engagement analysis | A proposed higher-level interpretation over time | A reliable conclusion from one facial expression |

Expression predictions should not be used to diagnose a student or drive punitive decisions. Practical deployment also needs appropriate participation consent, restricted access, and clear retention and deletion behavior for face and attendance data.

## Current development status

- [x] Edge-first architecture and technology choices documented.
- [x] LBPH, FER+, FastAPI, streaming, and dashboard integration reported in the handoff.
- [x] Partial operation and remaining accuracy/performance gaps recorded.
- [ ] Current implementation source uploaded to this repository.
- [ ] Reproducible Raspberry Pi setup documented.
- [ ] Recognition and unknown-person behavior measured.
- [ ] Multi-person tracking stability evaluated.
- [ ] End-to-end latency and sustained runtime characterized.
- [ ] Attendance rules and duplicate handling validated.
- [ ] Privacy controls and real deployment conditions documented.

These checkpoints distinguish reported development from independently reproduced results. No accuracy percentage, frame rate, or classroom deployment count is claimed.

## Validation priorities

### Recognition and tracking

Measure known-person matches, incorrect matches, missed detections, and unknown-person handling under varied lighting, distance, pose, and occlusion. Evaluate multiple people together and record identity switches over time.

### Attendance correctness

Define when a person is marked present, how repeated sightings are handled, and how an uncertain match is reviewed. Check database behavior after reconnects and service restarts.

### Edge performance

Measure processing latency, sustained frame rate, memory use, CPU load, and temperature on the actual Raspberry Pi configuration. Camera resolution alone is not evidence of usable inference performance.

### Expression interpretation

Keep uncertainty visible and evaluate temporal behavior separately from single-frame predictions. A model label should remain a model output, even when shown in a polished dashboard.

## Development roadmap

| Phase | Focus | Completion evidence |
|---|---|---|
| 1 — Recover baseline | Upload source and pin the current environment | Reproducible setup and startup instructions |
| 2 — Measure | Collect separate recognition, tracking, and latency results | Evaluation method and recorded outputs |
| 3 — Improve | Address the largest observed failure modes | Before/after comparisons |
| 4 — Reliability | Attendance rules, restarts, and dashboard consistency | Repeatable end-to-end checks |
| 5 — Deployment review | Access, consent, retention, and appropriate interpretation | Documented operating conditions |

## Repository purpose

This repository contains the project brief and editable `project.json`. The working application, model files, and dependency lockfiles have not been supplied here. Face images, student records, and attendance databases are not included. Add implementation code when available, with a setup guide that matches the actual files.

## Author

**Rohan Sashank Reddy**  
[GitHub](https://github.com/Rohansashankreddy07) · [LinkedIn](https://www.linkedin.com/in/rohan-sashank-reddy-chilukuri-aa169336a/) · [Instagram](https://www.instagram.com/rohansashankreddy/)

## Maintaining this repository

Keep this README and `project.json` aligned as the project develops. Record evidence when a planned feature becomes implemented or a target becomes a measured result. Preserve the project ID so future portfolio updates can link to the same project.
