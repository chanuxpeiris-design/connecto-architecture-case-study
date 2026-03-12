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

## 🗄️ Database Architecture and Security

### 1. Relational Database Design
The application utilizes a fully relational PostgreSQL database managed via Supabase. The schema is designed for high read/write efficiency, specifically optimized for real-time social feeds and instant messaging. 

Key architectural components include:
* **User Profiles and Verification:** Dual-state user tables tracking standard authentication alongside institutional verification statuses (e.g., pending document review, university email verified).
* **Communication Nodes:** An optimized messaging structure utilizing a dedicated `messages2` table linked to chat summary trackers.

### 2. Row Level Security (RLS) Implementation
To ensure absolute data privacy and structural integrity, security is enforced at the database level rather than relying solely on client-side logic. Strict **Row Level Security (RLS) policies** are applied across all tables:
* **Chat Privacy:** Users can only query and retrieve messages from the `messages2` table where their specific `user_id` matches the sender or receiver columns.
* **Verification Protection:** Profile verification flags can only be altered by authenticated backend administrative roles, preventing client-side manipulation.

**Case Example: Secure Authentication and Error Handling**
Client-side security is just as critical as database rules. When handling sensitive operations like password updates, the application utilizes strict validation gates before communicating with the Supabase Auth instance, ensuring unauthorized or malformed requests never hit the server:

```dart
// Secure password update flow handling Supabase Auth exceptions
Future<String?> updateSecurePassword(String newPassword) async {
  // 1. Client-side validation gate (e.g., length, character requirements)
  if (!isValidPassword(newPassword)) {
    return "Password does not meet security requirements.";
  }

  try {
    // 2. Secure BaaS communication
    await Supabase.instance.client.auth.updateUser(
      UserAttributes(password: newPassword),
    );
    return null; // Success state
  } on AuthException catch (e) {
    // 3. Graceful error handling preventing app crashes
    debugPrint('Auth Update Failed: ${e.message}');
    return e.message; 
  }
}

### 3. High-Performance Backend Logic (RPCs)
Instead of forcing the frontend to execute heavy database loops, complex or repetitive logic is offloaded directly to the PostgreSQL server using **Remote Procedure Calls (RPC)**. 

**Case Example: Real-Time Read Receipts**
When a user opens a chat, the app does not individually update dozens of unread message rows from the client. Instead, it triggers a backend stored procedure:

```dart
// Frontend trigger initiating the backend RPC call
Future<void> triggerMarkRead(String myId, String otherUserId) async {
  try {
    await Supabase.instance.client.rpc('mark_messages_read', params: {
      'my_id': myId,
      'other_user_id': otherUserId,
    });
    // This securely updates all unread flags in the 'messages2' table 
    // and toggles the 'last_message_read' summary flag in a single, atomic transaction.
  } catch (e) {
    debugPrint('RPC Execution Error: $e');
  }
}

## 🧩 Product Strategy & User Experience (UX)

Building a technically sound backend is only half the challenge. Connecto was designed with a heavy emphasis on user psychology, retention strategies, and seamless verification flows to ensure a high-quality user base.

### 1. The Education-Based Verification Engine
To solve the "bot and spam" problem prevalent in modern social networks, Connecto implements a strict, dual-pathway verification engine. 

* **Automated Pathway:** Users register with an institutional `.edu` or university-specific email domain. The system utilizes a backend trigger to automatically grant "Verified" status upon email confirmation.
* **Manual Pathway:** Users without an institutional email can utilize the secure document upload portal (handled via Supabase Storage buckets). This triggers a "Pending Review" state in the database, restricting specific platform access until an administrative override is applied.
* **Visual Trust Indicators:** Verified users receive cryptographic UI badges that carry across all platform interactions (posts, comments, messaging), instantly establishing professional credibility.

### 2. Engagement Architecture (The Dopamine Cycle)
Based on comprehensive platform analysis and feedback summaries from beta testers, the UI was structured to optimize user retention through deliberate engagement loops:
* **Variable Reward Mechanisms:** The social feed algorithm and notification delivery systems were mapped against standard dopamine cycle models to ensure users receive variable, high-value interactions rather than predictable, low-value spam.
* **Frictionless Onboarding:** The initial account creation minimizes cognitive load, deferring the heavy verification steps until the user has already experienced the core value proposition of the platform.



### 3. Cross-Platform Frontend (Flutter)
The entire client-side application was built using the Flutter SDK (Dart), ensuring a native-feeling experience across both iOS and Android from a single codebase.
* **State Management:** Efficient localized state handling ensures that real-time RPC calls (like read receipts) update the UI instantaneously without requiring full page rebuilds.
* **Modular Widget Design:** The UI architecture relies on highly reusable, custom-built Flutter widgets to maintain brand consistency and reduce code duplication across the application.

## 📚 Strategy & Research Documentation
The technical architecture of Connecto is backed by extensive market research and behavioral analysis. Below are the core strategy documents that dictated the platform's development:

* [📄 Platform & Competitive Analysis](./Platform_Analysis.pdf) - *Market gap identification and feature prioritization.*
* [📄 The Dopamine Cycle System](./Dopamine_Cycle.pdf) - *Research on user retention, variable reward mechanisms, and engagement loops.*
* [📄 Beta Testing & Feedback Summary](./Feedback_Summary.pdf) - *Data-driven insights from initial user testing that led to the platform's current structural pivot.*
* [📄 Prototype Functional Scope](./prototype.pdf) - *Detailed breakdown of the essential features and verification logic.*

*(Click the links above to view the full PDF documents)*
