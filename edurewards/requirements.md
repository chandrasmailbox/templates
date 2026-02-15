# EduRewards - Project Requirements & Implementation Plan

## 1. Project Overview
**EduRewards** is a "Learn & Earn" platform where parents invest in courses for their children, and students earn back a percentage of the course fee (up to 100% plus scholarships) based on their performance. The platform incentivizes learning through financial rewards.

## 2. Technology Stack
- **Frontend Framework**: React 18+ (via Next.js)
- **Backend/Fullstack Framework**: Next.js 14+ (App Router)
- **Language**: TypeScript
- **Styling**: Tailwind CSS
- **Database**: Supabase (PostgreSQL)
- **Authentication**: Supabase Auth
- **State Management**: React Context / Zustand / TanStack Query
- **Charts**: Recharts or Chart.js (via react-chartjs-2)
- **Icons**: Lucide React / FontAwesome
- **Animations**: Framer Motion

## 3. Core Functional Requirements

### 3.1 Authentication & Roles
- **Multi-role Support**:
  - **Parent**: Manages wallet, assigns courses, tracks child progress.
  - **Student**: Takes courses, tracks progress, earns rewards.
  - **Instructor**: Creates courses, manages content.
  - **Admin**: Approves courses, approves high-value rewards, monitors platform.
- **Secure Login/Signup**: Email/Password, potential Social Auth (Google).

### 3.2 Parent Dashboard
- **Wallet Management**:
  - View current balance.
  - Add funds securely (Stripe/Payment Gateway integration simulation).
  - View transaction history (deposits, purchases, refunds).
- **Student Management**:
  - Link child accounts (via email invitation).
  - View child specific stats (active courses, total earned).
- **Course Purchasing**:
  - Browse course catalog.
  - Purchase courses for specific children.
  - View active course assignments.
- **Spending Analytics**:
  - Visual breakdown of spending by child/category.

### 3.3 Student Dashboard
- **Learning Hub**:
  - Access assigned courses.
  - View progress bars and current grades.
- **Performance Tracking**:
  - "Performance Score" visualization (Radar chart).
  - Metrics: Homework, Quiz Scores, Completion, Engagement, Improvement.
- **Rewards System**:
  - Real-time earnings calculation based on score.
  - Reward Tier system (Bronze, Silver, Gold, Platinum).
  - **Redemption**: Request output to Gift Cards (Amazon, Target, etc.) or Course Credits.
- **Reward Calculator**:
  - Interactive tool to estimate potential returns based on target score.

### 3.4 Instructor Dashboard (Studio)
- **Course Management**:
  - Create new courses (Form: Title, Price, Duration, Description, Category).
  - View active courses and enrollment stats.
- **Analytics**:
  - Total students, Average performance across courses, Revenue generated.

### 3.5 Admin Dashboard
- **Approvals Queue**:
  - Review and approve new courses.
  - Review and approve high-value reward redemptions (e.g., >$500).
- **Platform Analytics**:
  - Total Revenue vs. Rewards Paid (Line chart).
  - User growth metrics.

## 4. Implementation Phases

### Phase 1: Foundation & Authentication
- **Objective**: Set up the project structure, database connection, and user authentication.
- **Tasks**:
  - Initialize Next.js project with TypeScript & Tailwind.
  - Setup Supabase project and configure environment variables.
  - Implement Auth System (Login/Signup pages).
  - Create Database Schema (Users, Roles, Profiles).
  - Implement RLS Policies for basic security.

### Phase 2: Core Database & Admin Panel
- **Objective**: Establish the data backbone and administrative controls.
- **Tasks**:
  - Define tables: `courses`, `enrollments`, `transactions`, `wallets`.
  - Build Admin Dashboard Layout.
  - Implement Course Approval Workflow (Backend & UI).
  - Implement Reward Approval Workflow.
  - Create Platform Analytics charts.

### Phase 3: Instructor Studio & Course Management
- **Objective**: Enable content creation.
- **Tasks**:
  - Build Instructor Dashboard.
  - Create "Add Course" form and server actions.
  - Implement Instructor Analytics views.
  - Course listing and filtering logic.

### Phase 4: Parent Portal & Wallet System
- **Objective**: Enable transactions and student management.
- **Tasks**:
  - detailed Parent Dashboard UI.
  - Implement Wallet System (Add Funds logic, Transaction logging).
  - build "Link Student" flow (Invite logic).
  - Course Catalog for Parents (Browse & Purchase flow).
  - Implement Atomic Transactions for purchasing (Deduct Wallet -> Create Enrollment).

### Phase 5: Student Experience & Learning Engine
- **Objective**: The core value proposition for the end-user.
- **Tasks**:
  - Student Dashboard UI.
  - "My Courses" view with progress tracking.
  - Performance Score logic (Mock grading engine or real implementation).
  - Real-time Reward Calculation engine.
  - Interactive Charts (Radar Score, Progress).

### Phase 6: Rewards & Redemptions
- **Objective**: Closing the loop on the "Earn" aspect.
- **Tasks**:
  - Redemption interface (Gift Card selection).
  - Redemption Request creation.
  - Integration with Parent/Admin approval workflows.
  - Final Polish: Animations (Framer Motion), loading states, error handling.

### Phase 7: Testing & Launch
- **Objective**: Ensure reliability and security.
- **Tasks**:
  - End-to-End Testing (Playwright/Cypress).
  - Security Audit (RLS policies check).
  - Performance Optimization (Image optimization, Code splitting).
  - Deployment to Vercel.
