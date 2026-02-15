# EduRewards - Database Schema

## Overview
This schema is designed for Supabase (PostgreSQL). It utilizes Supabase Auth for user management and Row Level Security (RLS) for data protection.

## Tables

### 1. `profiles`
Extends the default Supabase `auth.users` table.
- `id` (uuid, PK): References `auth.users.id`
- `email` (text): User email
- `full_name` (text): User's full name
- `role` (text): Enum ['admin', 'parent', 'student', 'instructor']
- `avatar_url` (text): URL to avatar image
- `created_at` (timestamp)
- `updated_at` (timestamp)

### 2. `wallets`
Stores financial balance for users (primarily Parents and Students).
- `id` (uuid, PK)
- `user_id` (uuid, FK): References `profiles.id`
- `balance` (decimal): Current available funds. Default 0.00
- `currency` (text): Default 'USD'
- `created_at` (timestamp)
- `updated_at` (timestamp)

### 3. `student_links`
Connects Students to Parents.
- `id` (uuid, PK)
- `parent_id` (uuid, FK): References `profiles.id`
- `student_id` (uuid, FK): References `profiles.id`
- `status` (text): Enum ['pending', 'active', 'rejected']
- `created_at` (timestamp)

### 4. `courses`
Educational content created by Instructors.
- `id` (uuid, PK)
- `instructor_id` (uuid, FK): References `profiles.id`
- `title` (text): Course title
- `description` (text): Detailed description
- `price` (decimal): Cost of enrollment
- `duration` (text): e.g., "8 weeks"
- `category` (text): e.g., "Mathematics", "Science"
- `image_url` (text): Cover image
- `status` (text): Enum ['draft', 'pending_approval', 'active', 'archived']
- `student_count` (int): Cached count of enrolled students
- `avg_rating` (decimal): 1.0 - 5.0
- `created_at` (timestamp)
- `updated_at` (timestamp)

### 5. `enrollments`
Tracks a student's participation in a course.
- `id` (uuid, PK)
- `student_id` (uuid, FK): References `profiles.id`
- `course_id` (uuid, FK): References `courses.id`
- `purchase_date` (timestamp)
- `progress` (int): 0-100 percentage
- `current_score` (int): 0-100 grade
- `earned_amount` (decimal): Money earned back based on performance
- `status` (text): Enum ['active', 'completed', 'dropped']
- `last_activity_at` (timestamp)

### 6. `transactions`
Financial log for all money movements.
- `id` (uuid, PK)
- `user_id` (uuid, FK): References `profiles.id`
- `type` (text): Enum ['deposit', 'purchase', 'refund', 'withdrawal', 'reward_payout']
- `amount` (decimal): Positive for earnings/deposits, negative for spending
- `description` (text): Human readable description
- `reference_id` (uuid): Optional link to `enrollments.id` or `courses.id`
- `status` (text): Enum ['pending', 'completed', 'failed']
- `created_at` (timestamp)

### 7. `redemptions`
Requests for withdrawing earned rewards.
- `id` (uuid, PK)
- `user_id` (uuid, FK): References `profiles.id`
- `amount` (decimal): Amount to redeem
- `method` (text): Enum ['amazon_gift_card', 'target_gift_card', 'visa_prepaid', 'course_credit']
- `status` (text): Enum ['pending', 'approved', 'rejected']
- `admin_notes` (text): Internal comments
- `created_at` (timestamp)
- `updated_at` (timestamp)

## Security (RLS Policies)

- **Profiles**: Users can read their own profile. Admins can read all.
- **Wallets**: Users can read their own wallet. Parents can read linked students' wallets.
- **Courses**: Public can read 'active' courses. Instructors can read their own. Admins can read all.
- **Enrollments**: Students read own. Parents read linked students'. Instructors read enrollments for their courses.
- **Transactions**: Users read own. Parents read linked students'.
