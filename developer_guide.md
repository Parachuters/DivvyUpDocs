---
layout: default
title: Developer Guide
---
# Developer Guide

Welcome to the DivvyUp developer guide! This document provides an overview of the project architecture and instructions for setting up your development environment.

## Table of Contents

*   [1. Architecture Overview](#1-architecture-overview)
*   [2. Getting Started](#2-getting-started)
*   [3. Running the Project](#3-running-the-project)
*   [4. Workspaces](#4-workspaces)
*   [5. Contribution Workflow](#5-contribution-workflow)
*   [6. Database viewing](#6-database-viewing)

## 1. Architecture Overview

DivvyUp is a monorepo managed by npm workspaces, consisting of two main packages:

*   `mobile`: React Native mobile application.
*   `backend`: Node.js Express server.

### 1.1 Tech Stack

| Component | Technology | Description |
| :--- | :--- | :--- |
| **Mobile** | [Expo](https://expo.dev/) | React Native framework with managed workflow. |
| | [React Navigation](https://reactnavigation.org/docs/getting-started/) | File-based routing for the mobile app. |
| **Backend** | [Node.js](https://nodejs.org/) | Runtime environment. |
| | [Express](https://expressjs.com/) | Web framework for the API. |
| | [Knex.js](https://knexjs.org/) | SQL Query Builder and Migration tool. |
| **Database** | [PostgreSQL](https://www.postgresql.org/) | Relational database. |
| **Infrastructure** | [Docker](https://www.docker.com/) | Containerization for the database. |
| **Auth** | [Firebase](https://firebase.google.com/) | Authentication service. |

### 1.2 Architecture Diagram

<div class="mermaid">
```graph TD
    User[User Mobile Device] -->|API Requests| API[Backend API (Express)]
    API -->|Query| DB[(PostgreSQL)]
    User -->|Auth| Firebase[Firebase Auth]
    API -->|Verify Token| Firebase
```
</div>

## 2. Getting Started

### 2.1 Prerequisites

Ensure you have the following installed:

*   **Node.js**: LTS version (v18 or v20+ recommended).
*   **Docker Desktop**: Required for running the PostgreSQL database.
*   **Expo Go**: Install the app on your iOS or Android device to run the mobile app locally (optional, can use emulators).
*   **Git**: Version control system.

### 2.2 Project Setup

1.  **Clone the repository**:
    Go to `https://github.com/Parachuters/DivvyUp/` and clone the repository locally.

    ```bash
    git clone https://github.com/Parachuters/DivvyUp.git
    cd DivvyUp
    ```

2.  **Set your Git identity (if needed)**:
    Ensure your commits are associated with your account.
    ```bash
    git config --global user.name "Your Name"
    git config --global user.email "you@example.com"
    ```

3.  **Install dependencies**:
    We use npm workspaces, so running `npm install` in the root installs dependencies for both backend and mobile.
    ```bash
    npm install
    ```

5.  **Environment Setup**:
    -   Copy `.env.example` to `.env` in the `backend` directory (if available) or create one with DB credentials.
    -   Ensure Docker is running.

## 3. Running the Project

Open 2 different terminals, navigate to `./backend` and `./mobile` respectively.

*   **Backend**: Located in `./backend`.
    *   `npm run dev`: Run only the backend. (Usually on localhost:3000)
    *   Ensure Docker Desktop is running and your Postgres container is up.
    *   Check the `/health` path to verify a valid connection.
*   **Mobile**: Located in `./mobile`.
    *   `npx expo start`: Run only the mobile app.
    *   If you get TypeError, run `npx expo start -c` then `npx expo start`.


## 4. Workspaces

*   **Backend**: Located in `./backend`.
    *   `npm run dev:backend`: Run only the backend.
    *   `npm run migrate`: Run database migrations.
*   **Mobile**: Located in `./mobile`.
    *   `npm run dev:mobile`: Run only the mobile app.


## 5. Contribution Workflow

We follow a **Branching Workflow** where contributors create feature branches against the main repository and open pull requests from those branches.

1.  **Sync with `main`**:
    Always start by making sure your local `main` is up to date.
    ```bash
    git checkout main
    git pull origin main
    ```

2.  **Create a Branch**:
    Name it descriptively (e.g., `feat/login-screen`, `fix/db-connection`).
    ```bash
    git checkout -b feat/your-feature-name
    ```

3.  **Make Changes & Commit**:
    Write clean, documented code and commit your changes.

4.  **Push the branch**:
    ```bash
    git push origin feat/your-feature-name
    ```

5.  **Create a Pull Request**:
    Go to the [GitHub repository](https://github.com/Parachuters/DivvyUp/) and create a PR from your branch into `main`. Add a clear description, link to any related issues, and request reviewers.

Note: If you do not have push permissions to the main repository, you may still fork and submit PRs from your fork. The branching workflow assumes you have permission to push branches to the repository.

## 6. Database viewing

1.  **Find your container name or ID**
    `docker ps`

2.  **Connect using psql**
    `docker exec -it <container_name> psql -U <username> -d <database_name>`

    If you followed my setup, it should be

    `docker exec -it divvyup-db psql -U divvyup `

3.  **Once connected use `\l` to list databases, `\dt` to list all tables in current database**

4.  **Run your desired SQL query!**



---
Happy Coding!
