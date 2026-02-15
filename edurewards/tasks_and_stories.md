# EduRewards - Detailed User Stories & Task List

This document breaks down the project into actionable User Stories and Technical Tasks, organized by implementation phase.

## Phase 1: Foundation & Authentication

### Epic: Project Setup & Security
**Goal**: Establish a secure, scalable foundation for the application.

#### Story 1.1: Project Initialization
**As a** Developer,
**I want** to initialize the Next.js project with the correct tech stack,
**So that** development can begin on a solid base.

- **Acceptance Criteria**:
  - Next.js 14+ (App Router) project created.
  - TypeScript configured strictly.
  - Tailwind CSS installed and configured with project brand colors.
  - ESLint and Prettier configured.
  - Git repository initialized.

- **Technical Tasks**:
  - [ ] Run `npx create-next-app@latest`.
  - [ ] Configure `tailwind.config.ts` with custom colors from design (Blues, Purples, Greens).
  - [ ] Set up `lib/utils.ts` for `cn` utility (clsx + tailwind-merge).
  - [ ] create `components/ui` folder for shadcn/ui or custom components.

#### Story 1.2: Supabase Integration
**As a** Developer,
**I want** to connect the application to Supabase,
**So that** I can manage users and data.

- **Acceptance Criteria**:
  - Supabase project created.
  - Environment variables (`NEXT_PUBLIC_SUPABASE_URL`, `NEXT_PUBLIC_SUPABASE_ANON_KEY`) set locally.
  - Supabase client helper functions created for Server Components, Client Components, and Server Actions.

- **Technical Tasks**:
  - [ ] Create Supabase project.
  - [ ] Install `@supabase/ssr` and `@supabase/supabase-js`.
  - [ ] Create `utils/supabase/server.ts`, `client.ts`, `middleware.ts`.
  - [ ] Configure Middleware to refresh auth sessions.

#### Story 1.3: User Authentication (Sign Up/Login)
**As a** User (Parent/Student/Instructor),
**I want** to sign up and log in using my email,
**So that** I can access my personalized dashboard.

- **Acceptance Criteria**:
  - Sign Up form collects Email, Password, Full Name, and Role.
  - Login form accepts Email and Password.
  - OAuth (Google) option available (optional for MVP, good to have).
  - Redirects to correct dashboard based on Role upon login.

- **Technical Tasks**:
  - [ ] Create `/login` and `/signup` pages.
  - [ ] Implement `p_sign_up` server action using `supabase.auth.signUp`.
  - [ ] Implement `p_sign_in` server action using `supabase.auth.signInWithPassword`.
  - [ ] Create `profiles` table trigger to automatically create profile on auth user creation (if not handling manually).
  - [ ] Implement `AuthGuard` or middleware logic to protect routes (`/dashboard/*`).

#### Story 1.4: Database Schema & RLS
**As a** Developer,
**I want** to define the database schema and security policies,
**So that** user data is isolated and secure.

- **Acceptance Criteria**:
  - Tables created: `profiles`, `wallets`, `student_links`.
  - RLS policies enabled. Users can only see their own data (or linked data).

- **Technical Tasks**:
  - [ ] Write SQL migration for `profiles` table (handle roles enum).
  - [ ] Write SQL for `wallets` table.
  - [ ] Write SQL for `student_links` table.
  - [ ] Enable RLS on all tables.
  - [ ] Create Policy: "Users can allow select on own profile".
  - [ ] Create Policy: "Parents can select linked student profiles".

---

## Phase 2: Core Database & Admin Panel

### Epic: Platform Administration
**Goal**: Enable platform owners to manage content and users.

#### Story 2.1: Admin Dashboard Layout
**As an** Admin,
**I want** a dedicated dashboard view,
**So that** I can see an overview of platform activity.

- **Acceptance Criteria**:
  - dedicated `/admin` route.
  - Sidebar navigation for Admin functions.
  - "Stat Cards" showing: Total Revenue, Pending Approvals, Total Users.

- **Technical Tasks**:
  - [ ] Create `app/admin/layout.tsx`.
  - [ ] Create `app/admin/page.tsx`.
  - [ ] Implement `AdminStats` component fetching counts from DB.

#### Story 2.2: Course Approval Workflow
**As an** Admin,
**I want** to view courses submitted by instructors,
**So that** I can approve or reject them for the marketplace.

- **Acceptance Criteria**:
  - List of courses with status `pending_approval`.
  - "Approve" button changes status to `active`.
  - "Reject" button changes status to `draft` (optionally with reason).

- **Technical Tasks**:
  - [ ] Create `courses` table (if not done).
  - [ ] Create `app/admin/courses/page.tsx` fetching pending courses.
  - [ ] Create Server Action `approveCourse(courseId)`.
  - [ ] Create Server Action `rejectCourse(courseId)`.

#### Story 2.3: Reward Approval Queue
**As an** Admin,
**I want** to review high-value reward redemptions,
**So that** I can prevent fraud.

- **Acceptance Criteria**:
  - List of redemptions > $500 (configurable threshold).
  - Detail view showing Student Name, Course, Score, and Redemption Amount.
  - Approve/Reject actions.

- **Technical Tasks**:
  - [ ] Create `redemptions` table.
  - [ ] Create `app/admin/rewards/page.tsx`.
  - [ ] Implement locking mechanism (prevent double approval).

---

## Phase 3: Instructor Studio

### Epic: Course Creation
**Goal**: Populate the platform with learning content.

#### Story 3.1: Instructor Dashboard
**As an** Instructor,
**I want** to see my courses and student stats,
**So that** I can track my content's performance.

- **Acceptance Criteria**:
  - Grid view of "My Courses".
  - Summary stats: Active Students, Total Revenue (if revenue share implemented).

- **Technical Tasks**:
  - [ ] Create `app/instructor/page.tsx`.
  - [ ] Create `InstructorStats` component.

#### Story 3.2: Create New Course
**As an** Instructor,
**I want** a form to input course details,
**So that** I can submit a new course for approval.

- **Acceptance Criteria**:
  - Multi-step form or long form.
  - Fields: Title, Description, Price, Duration, Category, Cover Image (Upload).
  - Validation: Price must be positive, Title required.
  - Submission sets status to `pending_approval`.

- **Technical Tasks**:
  - [ ] Create `app/instructor/courses/new/page.tsx`.
  - [ ] Implement file upload for Cover Image (Supabase Storage bucket `course-assets`).
  - [ ] Create Server Action `createCourse`.
  - [ ] Use `zod` for form validation.

---

## Phase 4: Parent Portal & Operations

### Epic: Family Management
**Goal**: Allow parents to manage their children's education and finances.

#### Story 4.1: Wallet System (Add Funds)
**As a** Parent,
**I want** to add funds to my wallet,
**So that** I can purchase courses.

- **Acceptance Criteria**:
  - "Add Funds" modal.
  - Select amount ($50, $100, custom).
  - Simulate payment processing (update `wallets` table, log `transaction`).

- **Technical Tasks**:
  - [ ] Create `WalletCard` component displaying balance.
  - [ ] Create `AddFundsModal`.
  - [ ] Create Server Action `addFunds(amount)`.
  - [ ] Ensure DB transaction: Update Wallet + Insert Transaction record atomically.

#### Story 4.2: Link Student Account
**As a** Parent,
**I want** to link a student account via email,
**So that** I can assign courses to them.

- **Acceptance Criteria**:
  - Input form for Student Email.
  - System checks if user exists.
  - Creates a `student_links` record. (For MVP, auto-accept or simple invite status).

- **Technical Tasks**:
  - [ ] Create `LinkStudentModal`.
  - [ ] Server Action `inviteStudent(email)`.
  - [ ] Update RLS to allow Parents to see profiles of linked students.

#### Story 4.3: Browse & Purchase Course
**As a** Parent,
**I want** to select a course and assign it to a specific child,
**So that** they can start learning.

- **Acceptance Criteria**:
  - Course Catalog view with filters (Subject, Price).
  - "Purchase" flow: Select Course -> Select Child -> Confirm Payment.
  - Deduct from Wallet -> Create `enrollment` record.

- **Technical Tasks**:
  - [ ] Create `app/parent/catalog/page.tsx`.
  - [ ] Create `CourseCard` component.
  - [ ] Create `PurchaseModal` wizard (Step 1: Details, Step 2: Student Select, Step 3: Confirm).
  - [ ] Server Action `purchaseCourse(courseId, studentId)`.
  - [ ] Handle "Insufficient Funds" error state.

---

## Phase 5: Student Learning Experience

### Epic: Learning & Progress
**Goal**: The core "Play" loop for the student.

#### Story 5.1: My Learning Hub
**As a** Student,
**I want** to see all my assigned courses,
**So that** I can access my content.

- **Acceptance Criteria**:
  - Grid of enrolled courses (Status: Active vs Completed).
  - Progress bar for each.

- **Technical Tasks**:
  - [ ] Create `app/student/dashboard/page.tsx`.
  - [ ] Fetch enrollments joined with course details.

#### Story 5.2: Performance Scoring (Gamification)
**As a** Student,
**I want** to see my "Performance Score" calculated from my work,
**So that** I know how much reward I'm earning.

- **Acceptance Criteria**:
  - Visual Radar Chart displaying metrics (Homework, Quiz, Engagement).
  - Calculated "Current Score" (0-100).
  - Dynamic "Reward Potential" display updating as score changes.

- **Technical Tasks**:
  - [ ] Install `recharts` or `chart.js`.
  - [ ] Build `PerformanceRadar` component.
  - [ ] Create utility function `calculateReward(coursePrice, score)`.
  - [ ] Implement logic to Mock grade updates (for demo purposes) or real granular tracking.

---

## Phase 6: Rewards & Redemption

### Epic: Cashing Out
**Goal**: Realize the financial incentive.

#### Story 6.1: Redemption Request
**As a** User (Parent/Student),
**I want** to redeem my earned balance for a Gift Card,
**So that** I can use my rewards.

- **Acceptance Criteria**:
  - "Redeem" button in Wallet/Earnings view.
  - Select Reward Type (Amazon, Target, etc.).
  - Input Amount (cannot exceed available confirmed earnings).
  - Create `redemption` record.

- **Technical Tasks**:
  - [ ] Create `RedeemModal`.
  - [ ] Server Action `requestRedemption(amount, method)`.
  - [ ] Validation: Ensure `earned_amount` in enrollments is accessible to `wallet` logic (or transfer earnings to wallet upon course completion).

---

## Phase 7: Polish & Launch

### Epic: Non-Functional Requirements
**Goal**: Make the app production-ready.

#### Story 7.1: UI Polish & Animations
**As a** User,
**I want** smooth transitions and interactive elements,
**So that** the app feels premium and modern.

- **Technical Tasks**:
  - [ ] Add `framer-motion` for page transitions.
  - [ ] Add micro-interactions (hover states, button clicks).
  - [ ] Add Loading Skeletons for all data-fetching components.
  - [ ] Implement Toast Notifications for all server actions (Success/Error).

#### Story 7.2: End-to-End Testing
**As a** Developer,
**I want** to verify critical flows,
**So that** I don't deploy broken features.

- **Technical Tasks**:
  - [ ] Install Playwright.
  - [ ] Write test: "Parent Sign Up -> Add Funds -> Purchase Course".
  - [ ] Write test: "Student Login -> View Course -> Check Rewards".
