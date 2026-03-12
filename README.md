# 📱 Connecto: Professional Social Networking Architecture
**A Full-Stack Case Study on Scalable UI, Real-Time Communication, and Secure Verification Systems**

*Note: The core source code for this application is hosted in a private repository to protect proprietary business logic and upcoming market iterations. This document serves as a technical teardown of the system architecture, database design, and key feature implementations.*

## 📌 Project Overview
Connecto is a professional social media mobile application prototype. It was engineered to demonstrate modern full-stack application development principles, focusing on secure user verification workflows, real-time database management, and scalable system design.

Unlike standard social platforms, Connecto implements a strict **Education-Based User Verification Engine**, allowing users to verify their profiles via institutional credentials to establish immediate trust and credibility in professional networking environments.

## 🛠️ Tech Stack and Infrastructure
![Flutter](https://img.shields.io/badge/Flutter-%2302569B.svg?style=for-the-badge&logo=Flutter&logoColor=white)
![Dart](https://img.shields.io/badge/dart-%230175C2.svg?style=for-the-badge&logo=dart&logoColor=white)
![Supabase](https://img.shields.io/badge/Supabase-3ECF8E?style=for-the-badge&logo=supabase&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/postgresql-4169e1?style=for-the-badge&logo=postgresql&logoColor=white)

* **Frontend:** Flutter (Cross-platform mobile UI)
* **Backend as a Service (BaaS):** Supabase
* **Database:** PostgreSQL (Relational data modeling)
* **Authentication:** Supabase Auth (Email/Password and Institutional Verification)
* **Real-Time:** Supabase Realtime (Live chat and messaging synchronization)

## 🚀 Core Technical Features
1. **Education-Based Verification:** A dual-pathway verification system utilizing email-based institutional credential checks and manual document submission workflows.
2. **Real-Time Communication:** Live messaging architecture utilizing backend Remote Procedure Calls (RPC) to instantly sync read/unread states across devices.
3. **Role-Based Access Control (RBAC):** Distinct data access policies for verified vs. unverified users.
