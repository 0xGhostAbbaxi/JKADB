# JKADB — IMPLEMENTATION ROADMAP
## Step-by-Step Development Plan

**Total Estimated Effort:** 40-60 hours  
**Priority:** Complete all CRITICAL items first, then IMPORTANT, then NICE-TO-HAVE

---

## PART 1: DATABASE FOUNDATION (Hours 1-4)

### Step 1.1: Add Post Offices Table

**File:** `src/db/schema.ts`

Add after `areas` table definition:

```typescript
export const postOffices = pgTable(
  "post_offices",
  {
    id: uuid("id").primaryKey().defaultRandom(),
    unionCouncilId: uuid("union_council_id")
      .notNull()
      .references(() => unionCouncils.id),
    tehsilId: uuid("tehsil_id")
      .notNull()
      .references(() => tehsils.id),
    districtId: uuid("district_id")
      .notNull()
      .references(() => districts.id),
    nameEn: varchar("name_en", { length: 200 }).notNull(),
    nameUr: varchar("name_ur", { length: 200 }),
    code: varchar("code", { length: 50 }),
    isActive: boolean("is_active").default(true).notNull(),
    sortOrder: integer("sort_order").default(0),
    createdAt: timestamp("created_at").defaultNow().notNull(),
    updatedAt: timestamp("updated_at").defaultNow().notNull(),
  },
  (t) => [index("post_offices_uc_idx").on(t.unionCouncilId)]
);
```

Add relations:
```typescript
export const postOfficesRelations = relations(postOffices, ({ one }) => ({
  unionCouncil: one(unionCouncils, {
    fields: [postOffices.unionCouncilId],
    references: [unionCouncils.id],
  }),
  tehsil: one(tehsils, {
    fields: [postOffices.tehsilId],
    references: [tehsils.id],
  }),
  district: one(districts, {
    fields: [postOffices.districtId],
    references: [districts.id],
  }),
}));
```

Update `unionCouncilsRelations`:
```typescript
export const unionCouncilsRelations = relations(unionCouncils, ({ one, many }) => ({
  // ... existing ...
  postOffices: many(postOffices),
}));
```

### Step 1.2: Create Drizzle Migration

**Command:**
```bash
npx drizzle-kit generate --name add_post_offices
```

**File:** Will be auto-generated in `drizzle/` folder

Verify migration includes post_offices table.

### Step 1.3: Add Permissions Schema

**File:** `src/db/schema.ts`

Add after adminUsers:

```typescript
export const permissions = pgTable("permissions", {
  id: uuid("id").primaryKey().defaultRandom(),
  key: varchar("key", { length: 100 }).notNull().unique(),
  nameEn: varchar("name_en", { length: 200 }).notNull(),
  nameUr: varchar("name_ur", { length: 200 }),
  description: text("description"),
  category: varchar("category", { length: 50 }),
  createdAt: timestamp("created_at").defaultNow().notNull(),
});

export const rolePermissions = pgTable(
  "role_permissions",
  {
    id: uuid("id").primaryKey().defaultRandom(),
    role: userRoleEnum("role").notNull(),
    permissionId: uuid("permission_id")
      .notNull()
      .references(() => permissions.id),
    createdAt: timestamp("created_at").defaultNow().notNull(),
  },
  (t) => [uniqueIndex("role_permission_idx").on(t.role, t.permissionId)]
);

export const adminPermissions = pgTable(
  "admin_permissions",
  {
    id: uuid("id").primaryKey().defaultRandom(),
    adminId: uuid("admin_id")
      .notNull()
      .references(() => adminUsers.id),
    permissionId: uuid("permission_id")
      .notNull()
      .references(() => permissions.id),
    grantedAt: timestamp("granted_at").defaultNow().notNull(),
    grantedBy: uuid("granted_by").references(() => adminUsers.id),
  },
  (t) => [uniqueIndex("admin_perm_idx").on(t.adminId, t.permissionId)]
);
```

Add relations:
```typescript
export const permissionsRelations = relations(permissions, ({ many }) => ({
  roles: many(rolePermissions),
  admins: many(adminPermissions),
}));

export const adminPermissionsRelations = relations(adminPermissions, ({ one }) => ({
  admin: one(adminUsers, {
    fields: [adminPermissions.adminId],
    references: [adminUsers.id],
  }),
  permission: one(permissions, {
    fields: [adminPermissions.permissionId],
    references: [permissions.id],
  }),
}));
```

### Step 1.4: Create Response Templates Schema

**File:** `src/db/schema.ts`

```typescript
export const responseTemplates = pgTable("response_templates", {
  id: uuid("id").primaryKey().defaultRandom(),
  titleEn: varchar("title_en", { length: 200 }).notNull(),
  titleUr: varchar("title_ur", { length: 200 }),
  contentEn: text("content_en").notNull(),
  contentUr: text("content_ur"),
  category: varchar("category", { length: 100 }).default("general"),
  isActive: boolean("is_active").default(true).notNull(),
  createdBy: uuid("created_by").references(() => adminUsers.id),
  createdAt: timestamp("created_at").defaultNow().notNull(),
  updatedAt: timestamp("updated_at").defaultNow().notNull(),
});
```

### Step 1.5: Add Quick Alerts Schema

**File:** `src/db/schema.ts`

```typescript
export const quickAlertStatusEnum = pgEnum("quick_alert_status", [
  "draft",
  "scheduled",
  "active",
  "expired",
]);

export const quickAlerts = pgTable("quick_alerts", {
  id: uuid("id").primaryKey().defaultRandom(),
  titleEn: varchar("title_en", { length: 200 }).notNull(),
  titleUr: varchar("title_ur", { length: 200 }),
  messageEn: text("message_en").notNull(),
  messageUr: text("message_ur"),
  status: quickAlertStatusEnum("status").default("draft").notNull(),
  priority: varchar("priority", { length: 20 }).default("normal"),
  displayMode: varchar("display_mode", { length: 50 }).default("banner"),
  startsAt: timestamp("starts_at").notNull(),
  expiresAt: timestamp("expires_at"),
  createdBy: uuid("created_by").references(() => adminUsers.id),
  createdAt: timestamp("created_at").defaultNow().notNull(),
  updatedAt: timestamp("updated_at").defaultNow().notNull(),
});
```

### Step 1.6: Add Public Contacts Schema

**File:** `src/db/schema.ts`

```typescript
export const publicContacts = pgTable("public_contacts", {
  id: uuid("id").primaryKey().defaultRandom(),
  type: varchar("type", { length: 100 }).notNull(),
  value: varchar("value", { length: 255 }).notNull(),
  labelEn: varchar("label_en", { length: 200 }),
  labelUr: varchar("label_ur", { length: 200 }),
  description: text("description"),
  isActive: boolean("is_active").default(true).notNull(),
  sortOrder: integer("sort_order").default(0),
  createdAt: timestamp("created_at").defaultNow().notNull(),
  updatedAt: timestamp("updated_at").defaultNow().notNull(),
});
```

### Step 1.7: Add AI Metrics Schema

**File:** `src/db/schema.ts`

```typescript
export const aiRequestMetrics = pgTable(
  "ai_request_metrics",
  {
    id: uuid("id").primaryKey().defaultRandom(),
    sessionId: varchar("session_id", { length: 255 }).notNull(),
    requestCount: integer("request_count").default(1),
    successCount: integer("success_count").default(0),
    failureCount: integer("failure_count").default(0),
    averageResponseTime: integer("average_response_time"), // ms
    lastRequestAt: timestamp("last_request_at"),
    isRateLimited: boolean("is_rate_limited").default(false),
    createdAt: timestamp("created_at").defaultNow().notNull(),
    updatedAt: timestamp("updated_at").defaultNow().notNull(),
  },
  (t) => [index("ai_metrics_session_idx").on(t.sessionId)]
);
```

### Step 1.8: Create All Migrations

```bash
npx drizzle-kit generate --name add_permissions_and_templates
npx drizzle-kit generate --name add_quick_alerts
npx drizzle-kit generate --name add_public_contacts
npx drizzle-kit generate --name add_ai_metrics
```

**✅ Database Foundation Complete**

---

## PART 2: BACKEND INFRASTRUCTURE (Hours 5-10)

### Step 2.1: Create Permissions Service

**File:** `src/lib/services/permissions-service.ts`

```typescript
import { db } from "@/db";
import { adminPermissions, permissions as permissionsTable, rolePermissions } from "@/db/schema";
import { eq, and } from "drizzle-orm";

export async function checkPermission(
  adminId: string,
  permissionKey: string
): Promise<boolean> {
  // Check admin-specific permission
  const adminPerm = await db
    .select()
    .from(adminPermissions)
    .innerJoin(permissionsTable, eq(adminPermissions.permissionId, permissionsTable.id))
    .where(and(
      eq(adminPermissions.adminId, adminId),
      eq(permissionsTable.key, permissionKey)
    ))
    .limit(1);

  if (adminPerm.length > 0) return true;

  // Check role-based permission (need admin's role from adminUsers)
  // This is a simplified check; expand as needed
  return false;
}

export async function initializePermissions() {
  // Create default permissions if they don't exist
  const defaultPermissions = [
    { key: "view_complaints", nameEn: "View Complaints", category: "complaints" },
    { key: "edit_complaints", nameEn: "Edit Complaints", category: "complaints" },
    { key: "assign_complaints", nameEn: "Assign Complaints", category: "complaints" },
    // ... more permissions
  ];

  // Insert permissions
}
```

### Step 2.2: Create AI Service

**File:** `src/lib/services/ai-service.ts`

```typescript
interface AIMessage {
  role: "user" | "assistant" | "system";
  content: string;
}

interface AIResponse {
  success: boolean;
  message?: string;
  error?: string;
  responseTime?: number;
}

export class AIService {
  private apiKey: string;
  private baseUrl = "https://api.x.ai/v1";

  constructor() {
    this.apiKey = process.env.GROK_API_KEY || "";
    if (!this.apiKey) {
      console.warn("GROK_API_KEY not configured");
    }
  }

  async sendMessage(
    sessionId: string,
    userMessage: string,
    conversationHistory: AIMessage[] = []
  ): Promise<AIResponse> {
    if (!this.apiKey) {
      return {
        success: false,
        error: "AI service not configured",
      };
    }

    const startTime = Date.now();

    try {
      const messages: AIMessage[] = [
        ...conversationHistory,
        { role: "user", content: userMessage },
      ];

      const response = await fetch(`${this.baseUrl}/chat/completions`, {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
          Authorization: `Bearer ${this.apiKey}`,
        },
        body: JSON.stringify({
          model: "grok-2-1212",
          messages: messages,
          temperature: 0.7,
          max_tokens: 500,
          timeout: 30000,
        }),
      });

      if (!response.ok) {
        const error = await response.json();
        return {
          success: false,
          error: "Grok API error: " + (error.message || "Unknown error"),
          responseTime: Date.now() - startTime,
        };
      }

      const data = await response.json();
      const assistantMessage = data.choices?.[0]?.message?.content;

      if (!assistantMessage) {
        return {
          success: false,
          error: "No response from AI",
          responseTime: Date.now() - startTime,
        };
      }

      return {
        success: true,
        message: assistantMessage,
        responseTime: Date.now() - startTime,
      };
    } catch (error) {
      console.error("AI Service Error:", error);
      return {
        success: false,
        error: "Failed to reach AI service",
        responseTime: Date.now() - startTime,
      };
    }
  }

  async healthCheck(): Promise<{ status: string; message: string }> {
    if (!this.apiKey) {
      return { status: "error", message: "API key not configured" };
    }

    try {
      const response = await fetch(`${this.baseUrl}/models`, {
        headers: { Authorization: `Bearer ${this.apiKey}` },
        timeout: 5000,
      });

      return {
        status: response.ok ? "operational" : "error",
        message: response.ok ? "Grok API operational" : "Grok API error",
      };
    } catch (error) {
      return {
        status: "error",
        message: "Cannot reach Grok API",
      };
    }
  }
}

export const aiService = new AIService();
```

### Step 2.3: Create Auth Middleware

**File:** `src/lib/middleware/auth-middleware.ts`

```typescript
import { jwtVerify } from "jose";
import { cookies } from "next/headers";

const secret = new TextEncoder().encode(process.env.JWT_SECRET || "your-secret-key");

export async function getAuthenticatedAdmin() {
  const cookieStore = await cookies();
  const token = cookieStore.get("adminToken")?.value;

  if (!token) {
    return null;
  }

  try {
    const verified = await jwtVerify(token, secret);
    return verified.payload as any;
  } catch (error) {
    return null;
  }
}

export async function requireAuth() {
  const admin = await getAuthenticatedAdmin();
  if (!admin) {
    throw new Error("Unauthorized");
  }
  return admin;
}
```

### Step 2.4: Create Audit Service

**File:** `src/lib/services/audit-service.ts`

```typescript
import { db } from "@/db";
import { auditLogs } from "@/db/schema";

export async function logAudit({
  actorId,
  actorName,
  actorRole,
  action,
  targetType,
  targetId,
  targetDescription,
  previousValue,
  newValue,
  metadata,
  ipAddress,
}: any) {
  await db.insert(auditLogs).values({
    actorId: actorId || undefined,
    actorName: actorName || "System",
    actorRole: actorRole || "system",
    action,
    targetType,
    targetId,
    targetDescription,
    previousValue: previousValue || undefined,
    newValue: newValue || undefined,
    metadata: metadata || undefined,
    ipAddress: ipAddress || undefined,
  });
}
```

### Step 2.5: Create Database Utility Functions

**File:** `src/lib/db-utils.ts`

```typescript
import { db } from "@/db";
import { complaints, statusHistory } from "@/db/schema";
import { eq } from "drizzle-orm";

export async function generateComplaintId(): Promise<string> {
  const year = new Date().getFullYear();
  const lastComplaint = await db.query.complaints.findFirst({
    orderBy: (c) => c.createdAt,
    where: (c) => /* filter by year */ true,
  });

  const nextNum = (lastComplaint ? parseInt((lastComplaint as any).id.split("-").pop()) : 0) + 1;
  return `JKADB-${year}-${String(nextNum).padStart(6, "0")}`;
}

export async function updateComplaintStatus(
  complaintId: string,
  newStatus: string,
  reason?: string,
  actorId?: string
) {
  // Update complaint status
  await db
    .update(complaints)
    .set({ status: newStatus as any })
    .where(eq(complaints.id, complaintId));

  // Create status history entry
  await db.insert(statusHistory).values({
    complaintId,
    previousStatus: "unknown", // Get actual previous status
    newStatus,
    reason: reason || undefined,
    changedBy: actorId || undefined,
  });
}
```

### Step 2.6: Add Environment Variables Template

**File:** `.env.local` (update)

```env
# Database
DATABASE_URL=postgresql://user:password@localhost:5432/jkadb

# JWT/Sessions
JWT_SECRET=your-super-secret-jwt-key-change-in-production

# AI/Grok
AI_PROVIDER=grok
GROK_API_KEY=xai-xxx-your-actual-key
AI_REQUEST_TIMEOUT=30000
AI_RATE_LIMIT_RPM=60

# File Storage
STORAGE_TYPE=local  # or 's3'
STORAGE_PATH=./uploads

# Notifications
ENABLE_EMAIL_NOTIFICATIONS=false
ENABLE_SMS_NOTIFICATIONS=false
```

**✅ Backend Infrastructure Complete**

---

## PART 3: API ENDPOINTS (Hours 11-20)

### Step 3.1: Authentication Endpoints

**File:** `src/app/api/admin/login/route.ts` (update)

Ensure:
- Password hashing with bcryptjs
- JWT token generation
- Secure cookie setup
- Rate limiting
- Audit logging

### Step 3.2: Create Complaint API

**File:** `src/app/api/complaints/submit/route.ts` (verify/update)

```typescript
import { NextRequest, NextResponse } from "next/server";
import { db } from "@/db";
import { complaints, attachments } from "@/db/schema";
import { generateComplaintId, updateComplaintStatus } from "@/lib/db-utils";
import { logAudit } from "@/lib/services/audit-service";

export async function POST(request: NextRequest) {
  try {
    const body = await request.json();

    // Validate required fields
    if (!body.phone || !body.category) {
      return NextResponse.json(
        { error: "Missing required fields" },
        { status: 400 }
      );
    }

    // Generate unique complaint ID
    const complaintId = await generateComplaintId();

    // Create complaint record
    const complaint = await db
      .insert(complaints)
      .values({
        trackingNumber: complaintId,
        phone: body.phone,
        email: body.email || undefined,
        fullName: body.fullName,
        fatherName: body.fatherName,
        cnic: body.cnic,
        categoryId: body.categoryId,
        subcategoryId: body.subcategoryId || undefined,
        districtId: body.districtId,
        tehsilId: body.tehsilId,
        unionCouncilId: body.unionCouncilId,
        constituencyId: body.constituencyId || undefined,
        description: body.description,
        moreDetail: body.moreDetail || undefined,
        status: "submitted",
        priority: "normal",
      })
      .returning();

    // Log to audit
    await logAudit({
      action: "complaint_submitted",
      targetType: "complaint",
      targetId: complaint[0].id,
      targetDescription: `Complaint ${complaintId} by ${body.phone}`,
      metadata: { complaintId },
    });

    return NextResponse.json({
      success: true,
      complaintId,
      id: complaint[0].id,
    });
  } catch (error) {
    console.error("Error submitting complaint:", error);
    return NextResponse.json(
      { error: "Failed to submit complaint" },
      { status: 500 }
    );
  }
}
```

### Step 3.3: Track Complaint API

**File:** `src/app/api/complaints/track/route.ts` (create)

```typescript
import { NextRequest, NextResponse } from "next/server";
import { db } from "@/db";
import { complaints } from "@/db/schema";
import { eq } from "drizzle-orm";

export async function POST(request: NextRequest) {
  try {
    const { trackingNumber, phone } = await request.json();

    if (!trackingNumber || !phone) {
      return NextResponse.json(
        { error: "Tracking number and phone required" },
        { status: 400 }
      );
    }

    const complaint = await db.query.complaints.findFirst({
      where: (c) => eq(c.trackingNumber, trackingNumber),
    });

    if (!complaint) {
      return NextResponse.json(
        { error: "Complaint not found" },
        { status: 404 }
      );
    }

    // Verify phone number matches (security)
    if (complaint.phone !== phone) {
      return NextResponse.json(
        { error: "Phone number does not match" },
        { status: 403 }
      );
    }

    // Return complaint details
    return NextResponse.json({
      success: true,
      complaint: complaint,
    });
  } catch (error) {
    console.error("Error tracking complaint:", error);
    return NextResponse.json(
      { error: "Failed to track complaint" },
      { status: 500 }
    );
  }
}
```

### Step 3.4: AI Chat API

**File:** `src/app/api/ai/chat/route.ts` (create)

```typescript
import { NextRequest, NextResponse } from "next/server";
import { aiService } from "@/lib/services/ai-service";
import { db } from "@/db";
import { aiRequestMetrics } from "@/db/schema";

export async function POST(request: NextRequest) {
  try {
    const { sessionId, message, conversationHistory = [] } = await request.json();

    if (!sessionId || !message) {
      return NextResponse.json(
        { error: "sessionId and message required" },
        { status: 400 }
      );
    }

    // Check rate limit (60 per minute per session)
    const recentMetrics = await db.query.aiRequestMetrics.findFirst({
      where: (m) => eq(m.sessionId, sessionId),
    });

    if (recentMetrics && recentMetrics.requestCount > 60) {
      return NextResponse.json(
        { error: "Rate limited" },
        { status: 429 }
      );
    }

    // Send message to Grok
    const response = await aiService.sendMessage(
      sessionId,
      message,
      conversationHistory
    );

    if (!response.success) {
      return NextResponse.json(
        { error: response.error },
        { status: 503 }
      );
    }

    // Log metrics
    await db.insert(aiRequestMetrics).values({
      sessionId,
      requestCount: (recentMetrics?.requestCount || 0) + 1,
      successCount: (recentMetrics?.successCount || 0) + 1,
      averageResponseTime: response.responseTime,
      lastRequestAt: new Date(),
    });

    return NextResponse.json({
      success: true,
      message: response.message,
      responseTime: response.responseTime,
    });
  } catch (error) {
    console.error("AI Chat Error:", error);
    return NextResponse.json(
      { error: "AI service error" },
      { status: 500 }
    );
  }
}
```

### Step 3.5: Admin Complaint List API

**File:** `src/app/api/admin/complaints/route.ts` (verify/update)

Ensure:
- Real database queries
- Filtering by status
- Pagination (limit + offset)
- Permission checks
- No mock data

### Step 3.6: Public Contacts API

**File:** `src/app/api/public/contacts/route.ts` (create)

```typescript
import { NextResponse } from "next/server";
import { db } from "@/db";
import { publicContacts } from "@/db/schema";
import { eq } from "drizzle-orm";

export async function GET() {
  try {
    const contacts = await db.query.publicContacts.findMany({
      where: eq(publicContacts.isActive, true),
      orderBy: publicContacts.sortOrder,
    });

    return NextResponse.json({ contacts });
  } catch (error) {
    return NextResponse.json(
      { error: "Failed to fetch contacts" },
      { status: 500 }
    );
  }
}
```

### Step 3.7: Quick Alerts API

**File:** `src/app/api/public/quick-alerts/route.ts` (create)

```typescript
import { NextResponse } from "next/server";
import { db } from "@/db";
import { quickAlerts } from "@/db/schema";
import { and, eq, gt, lt } from "drizzle-orm";

export async function GET() {
  try {
    const now = new Date();

    const active = await db.query.quickAlerts.findMany({
      where: and(
        eq(quickAlerts.status, "active"),
        gt(quickAlerts.expiresAt, now)
      ),
      orderBy: quickAlerts.priority,
    });

    return NextResponse.json({ alerts: active });
  } catch (error) {
    return NextResponse.json(
      { error: "Failed to fetch alerts" },
      { status: 500 }
    );
  }
}
```

### Step 3.8: Health Check API

**File:** `src/app/api/health/route.ts` (create/update)

```typescript
import { NextResponse } from "next/server";
import { db } from "@/db";
import { aiService } from "@/lib/services/ai-service";

export async function GET() {
  try {
    // Check database
    const dbOk = await db.query.systemSettings.findFirst().then(() => true).catch(() => false);

    // Check AI
    const aiHealth = await aiService.healthCheck();

    return NextResponse.json({
      status: "ok",
      database: dbOk ? "operational" : "error",
      ai: aiHealth.status,
      timestamp: new Date().toISOString(),
    });
  } catch (error) {
    return NextResponse.json(
      { error: "Health check failed" },
      { status: 500 }
    );
  }
}
```

**✅ API Endpoints Complete (Core)**

---

## PART 4: FRONTEND COMPONENTS (Hours 21-35)

### Step 4.1: Startup Animation

**File:** `src/components/SplashScreen.tsx` (update)

```typescript
"use client";

import { useEffect, useState } from "react";
import { motion } from "framer-motion";

export default function SplashScreen({ onComplete }: { onComplete: () => void }) {
  const [phase, setPhase] = useState(0);

  useEffect(() => {
    const timings = [500, 1000, 1500, 2000, 2500, 3000, 3500];
    const intervals = timings.map((time) =>
      setTimeout(() => {
        setPhase((p) => p + 1);
      }, time)
    );

    const completeTimer = setTimeout(onComplete, 4000);

    return () => {
      intervals.forEach(clearTimeout);
      clearTimeout(completeTimer);
    };
  }, [onComplete]);

  return (
    <div className="fixed inset-0 bg-gradient-to-br from-green-50 to-green-100 flex items-center justify-center">
      <motion.div className="text-center">
        {/* Open Hand */}
        {phase >= 0 && (
          <motion.div
            initial={{ scale: 0, opacity: 0 }}
            animate={{ scale: 1, opacity: 1 }}
            exit={{ scale: 0, opacity: 0 }}
            transition={{ duration: 0.6 }}
            className="text-7xl mb-8"
          >
            ✋
          </motion.div>
        )}

        {/* JKADB */}
        {phase >= 1 && (
          <motion.h1
            initial={{ opacity: 0, y: 20 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 0.5 }}
            className="text-5xl font-bold text-green-700 mb-4"
          >
            JKADB
          </motion.h1>
        )}

        {/* English Text */}
        {phase >= 2 && (
          <motion.p
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
            transition={{ duration: 0.5 }}
            className="text-xl text-gray-700 mb-2"
          >
            Jammu Kashmir Awami Dast-o-Bazo
          </motion.p>
        )}

        {/* Urdu Text */}
        {phase >= 3 && (
          <motion.p
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
            transition={{ duration: 0.5 }}
            className="text-2xl text-green-700 mb-8 font-urdu"
          >
            جموں کشمیر عوامی دست و بازو
          </motion.p>
        )}

        {/* Organization */}
        {phase >= 4 && (
          <motion.p
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
            transition={{ duration: 0.5 }}
            className="text-lg text-gray-600 mb-2"
          >
            From: MAJOR FORCE Narakot
          </motion.p>
        )}

        {/* Developer */}
        {phase >= 5 && (
          <motion.p
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
            transition={{ duration: 0.5 }}
            className="text-lg text-gray-600"
          >
            Built by: Hozafa Mehmood
          </motion.p>
        )}
      </motion.div>
    </div>
  );
}
```

### Step 4.2: AI Assistant Component

**File:** `src/components/AIAssistant/AIBubble.tsx` (create)

```typescript
"use client";

import { useState } from "react";
import { MessageCircle, X, RotateCcw } from "lucide-react";
import AIChatWindow from "./AIChatWindow";

export default function AIBubble() {
  const [open, setOpen] = useState(false);

  return (
    <div className="fixed bottom-6 right-6 z-50">
      {open ? (
        <AIChatWindow onClose={() => setOpen(false)} />
      ) : (
        <button
          onClick={() => setOpen(true)}
          className="w-14 h-14 bg-green-600 text-white rounded-full shadow-lg hover:bg-green-700 flex items-center justify-center transition-all"
          aria-label="Open AI Assistant"
        >
          <MessageCircle size={24} />
        </button>
      )}
    </div>
  );
}
```

**File:** `src/components/AIAssistant/AIChatWindow.tsx` (create)

```typescript
"use client";

import { useState, useRef, useEffect } from "react";
import { X, Send, RotateCcw, Globe } from "lucide-react";

interface Message {
  id: string;
  role: "user" | "assistant";
  content: string;
  timestamp: Date;
}

export default function AIChatWindow({ onClose }: { onClose: () => void }) {
  const [messages, setMessages] = useState<Message[]>([
    {
      id: "1",
      role: "assistant",
      content: "میں آپ کی مدد کے لیے حاضر ہوں۔ آپ کیا پوچھنا چاہتے ہیں؟",
      timestamp: new Date(),
    },
  ]);
  const [input, setInput] = useState("");
  const [loading, setLoading] = useState(false);
  const [language, setLanguage] = useState<"en" | "ur">("ur");
  const [sessionId] = useState(() => `session_${Date.now()}_${Math.random()}`);
  const messagesEndRef = useRef<HTMLDivElement>(null);

  const scrollToBottom = () => {
    messagesEndRef.current?.scrollIntoView({ behavior: "smooth" });
  };

  useEffect(() => {
    scrollToBottom();
  }, [messages]);

  const handleSend = async () => {
    if (!input.trim()) return;

    // Add user message
    const userMessage: Message = {
      id: `msg_${Date.now()}`,
      role: "user",
      content: input,
      timestamp: new Date(),
    };

    setMessages((prev) => [...prev, userMessage]);
    setInput("");
    setLoading(true);

    try {
      const response = await fetch("/api/ai/chat", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({
          sessionId,
          message: input,
          conversationHistory: messages.map((m) => ({
            role: m.role,
            content: m.content,
          })),
        }),
      });

      const data = await response.json();

      if (data.success) {
        const assistantMessage: Message = {
          id: `msg_${Date.now()}_ai`,
          role: "assistant",
          content: data.message,
          timestamp: new Date(),
        };
        setMessages((prev) => [...prev, assistantMessage]);
      } else {
        setMessages((prev) => [
          ...prev,
          {
            id: `msg_${Date.now()}_error`,
            role: "assistant",
            content: "معافی چاہتا ہوں، میں ابھی مدد فراہم نہیں کر سکتا۔",
            timestamp: new Date(),
          },
        ]);
      }
    } catch (error) {
      console.error("Chat error:", error);
      setMessages((prev) => [
        ...prev,
        {
          id: `msg_${Date.now()}_error`,
          role: "assistant",
          content: "معافی چاہتا ہوں، ایک خرابی کا سامنا ہو رہا ہے۔",
          timestamp: new Date(),
        },
      ]);
    } finally {
      setLoading(false);
    }
  };

  const handleReset = () => {
    setMessages([
      {
        id: "1",
        role: "assistant",
        content: "میں آپ کی مدد کے لیے حاضر ہوں۔ آپ کیا پوچھنا چاہتے ہیں؟",
        timestamp: new Date(),
      },
    ]);
  };

  return (
    <div className="fixed bottom-6 right-6 w-96 h-screen-3/4 bg-white rounded-lg shadow-2xl flex flex-col overflow-hidden z-50">
      {/* Header */}
      <div className="bg-gradient-to-r from-green-600 to-green-700 text-white p-4 flex justify-between items-center">
        <div>
          <h3 className="font-bold">JKADB Assistant</h3>
          <p className="text-sm opacity-90">سہائی دستاویز</p>
        </div>
        <div className="flex gap-2">
          <button
            onClick={handleReset}
            className="hover:bg-green-800 p-2 rounded"
            aria-label="Reset chat"
          >
            <RotateCcw size={18} />
          </button>
          <button
            onClick={onClose}
            className="hover:bg-green-800 p-2 rounded"
            aria-label="Close"
          >
            <X size={18} />
          </button>
        </div>
      </div>

      {/* Messages */}
      <div className="flex-1 overflow-y-auto p-4 bg-gray-50">
        {messages.map((msg) => (
          <div
            key={msg.id}
            className={`mb-4 flex ${msg.role === "user" ? "justify-end" : "justify-start"}`}
          >
            <div
              className={`max-w-xs p-3 rounded-lg ${
                msg.role === "user"
                  ? "bg-green-600 text-white"
                  : "bg-white text-gray-900 border border-gray-200"
              }`}
            >
              <p className="text-sm">{msg.content}</p>
            </div>
          </div>
        ))}
        {loading && (
          <div className="flex justify-start mb-4">
            <div className="bg-white text-gray-900 border border-gray-200 p-3 rounded-lg">
              <p className="text-sm">لکھ رہے ہیں...</p>
            </div>
          </div>
        )}
        <div ref={messagesEndRef} />
      </div>

      {/* Input */}
      <div className="border-t p-4 space-y-2">
        <div className="flex gap-2">
          <input
            type="text"
            value={input}
            onChange={(e) => setInput(e.target.value)}
            onKeyPress={(e) => e.key === "Enter" && handleSend()}
            placeholder="اپنا سوال لکھیں..."
            className="flex-1 border border-gray-300 rounded px-3 py-2 text-sm focus:outline-none focus:border-green-600"
            disabled={loading}
          />
          <button
            onClick={handleSend}
            disabled={loading || !input.trim()}
            className="bg-green-600 text-white p-2 rounded hover:bg-green-700 disabled:opacity-50"
            aria-label="Send message"
          >
            <Send size={18} />
          </button>
        </div>
      </div>
    </div>
  );
}
```

### Step 4.3: Settings Page

**File:** `src/app/settings/page.tsx` (create)

```typescript
"use client";

import { useState } from "react";
import { useTheme } from "next-themes";
import Link from "next/link";
import { Moon, Sun, Globe, LogOut } from "lucide-react";

export default function SettingsPage() {
  const { theme, setTheme } = useTheme();
  const [language, setLanguage] = useState("en");

  return (
    <div className="min-h-screen bg-gray-50">
      {/* Header */}
      <div className="bg-white border-b p-6">
        <h1 className="text-3xl font-bold text-gray-900">Settings</h1>
        <p className="text-gray-600">سیٹنگز</p>
      </div>

      <div className="max-w-2xl mx-auto p-6 space-y-6">
        {/* Theme */}
        <div className="bg-white rounded-lg shadow p-6">
          <h2 className="text-xl font-bold mb-4 flex items-center gap-2">
            {theme === "dark" ? <Moon /> : <Sun />} Display Theme
          </h2>
          <div className="space-y-2">
            <button
              onClick={() => setTheme("light")}
              className={`w-full p-3 border rounded-lg text-left ${
                theme === "light" ? "border-green-600 bg-green-50" : "border-gray-200"
              }`}
            >
              Light Theme
            </button>
            <button
              onClick={() => setTheme("dark")}
              className={`w-full p-3 border rounded-lg text-left ${
                theme === "dark" ? "border-green-600 bg-green-50" : "border-gray-200"
              }`}
            >
              Dark Theme
            </button>
          </div>
        </div>

        {/* Language */}
        <div className="bg-white rounded-lg shadow p-6">
          <h2 className="text-xl font-bold mb-4 flex items-center gap-2">
            <Globe /> Language
          </h2>
          <div className="space-y-2">
            <button
              onClick={() => setLanguage("en")}
              className={`w-full p-3 border rounded-lg text-left ${
                language === "en" ? "border-green-600 bg-green-50" : "border-gray-200"
              }`}
            >
              English
            </button>
            <button
              onClick={() => setLanguage("ur")}
              className={`w-full p-3 border rounded-lg text-left ${
                language === "ur" ? "border-green-600 bg-green-50" : "border-gray-200"
              }`}
            >
              اردو
            </button>
          </div>
        </div>

        {/* Admin Login */}
        <div className="bg-white rounded-lg shadow p-6">
          <h2 className="text-xl font-bold mb-4">Admin Access</h2>
          <Link
            href="/admin/login"
            className="block w-full bg-green-600 text-white p-3 rounded-lg text-center hover:bg-green-700 font-medium"
          >
            Admin Login →
          </Link>
        </div>

        {/* Logout (if admin) */}
        <div className="bg-white rounded-lg shadow p-6">
          <button className="w-full flex items-center justify-center gap-2 bg-red-600 text-white p-3 rounded-lg hover:bg-red-700 font-medium">
            <LogOut size={20} /> Logout
          </button>
        </div>
      </div>
    </div>
  );
}
```

### Step 4.4: Update Citizen Homepage

**File:** `src/components/CitizenHome.tsx` (update)

- Add professional hero
- Add quick action cards
- Connect to announcements
- Connect to quick alerts
- Add proper navigation

### Step 4.5: Update Navigation

**File:** `src/components/CitizenNav.tsx` (update)

Add Settings link with gear icon

**✅ Frontend Components Complete (Core)**

---

## PART 5: ADMIN FEATURES (Hours 36-45)

### Step 5.1: Admin Dashboard Enhancement

**File:** `src/app/admin/dashboard/page.tsx` (update)

```typescript
"use client";

import { useEffect, useState } from "react";

interface DashboardData {
  total: number;
  new: number;
  unread: number;
  pending: number;
  urgent: number;
  critical: number;
  overdue: number;
  resolved: number;
}

export default function AdminDashboard() {
  const [data, setData] = useState<DashboardData | null>(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetchDashboardData();
  }, []);

  async function fetchDashboardData() {
    try {
      const res = await fetch("/api/admin/dashboard/counts");
      const json = await res.json();
      setData(json);
    } catch (error) {
      console.error("Error fetching dashboard:", error);
    } finally {
      setLoading(false);
    }
  }

  if (loading) return <div>Loading...</div>;
  if (!data) return <div>Error loading dashboard</div>;

  const counts = [
    { label: "Total", value: data.total },
    { label: "New", value: data.new },
    { label: "Unread", value: data.unread },
    { label: "Pending", value: data.pending },
    { label: "Critical", value: data.critical, color: "text-red-600" },
    { label: "Overdue", value: data.overdue, color: "text-orange-600" },
    { label: "Resolved", value: data.resolved, color: "text-green-600" },
  ];

  return (
    <div className="p-8 space-y-8">
      <h1 className="text-4xl font-bold">Admin Dashboard</h1>

      {/* Stats Grid */}
      <div className="grid grid-cols-4 gap-4">
        {counts.map((stat) => (
          <div key={stat.label} className="bg-white rounded-lg shadow p-6">
            <p className="text-gray-600 text-sm">{stat.label}</p>
            <p className={`text-3xl font-bold ${stat.color || "text-green-600"}`}>
              {stat.value}
            </p>
          </div>
        ))}
      </div>

      {/* Quick Actions */}
      <div className="grid grid-cols-2 gap-6">
        <div className="bg-white rounded-lg shadow p-6">
          <h2 className="text-lg font-bold mb-4">Recent Requests</h2>
          {/* Add request list here */}
        </div>

        <div className="bg-white rounded-lg shadow p-6">
          <h2 className="text-lg font-bold mb-4">SLA Monitoring</h2>
          {/* Add SLA info here */}
        </div>
      </div>
    </div>
  );
}
```

### Step 5.2: Admin Complaints List Verification

**File:** `src/app/admin/complaints/page.tsx` (verify)

Ensure:
- Query shows REAL database data
- Counts are accurate
- Filters work
- Pagination implemented
- No mock data

### Step 5.3: Admin Requests Detail Page

**File:** `src/app/admin/complaints/[id]/page.tsx` (verify/enhance)

Ensure:
- All complaint information displayed
- Messaging system working
- Response ability
- Status change
- Accept/Decline workflow

### Step 5.4: Response Templates Page

**File:** `src/app/admin/response-templates/page.tsx` (create)

Create CRUD interface for response templates

### Step 5.5: Announcements Management

**File:** `src/app/admin/announcements/page.tsx` (verify/enhance)

Ensure full CRUD for announcements with:
- Create/Edit form
- Preview
- Scheduling
- Status management

### Step 5.6: Quick Alerts Management

**File:** `src/app/admin/quick-alerts/page.tsx` (create)

Similar to announcements but for quick alerts

### Step 5.7: Public Contacts Management

**File:** `src/app/admin/public-contacts/page.tsx` (create)

CRUD for public contact information

### Step 5.8: Officers Management Enhancement

**File:** `src/app/admin/officers/page.tsx` (verify/enhance)

Add officer workload and performance metrics

### Step 5.9: Categories Management

**File:** `src/app/admin/categories/page.tsx` (verify/enhance)

Add full CRUD for categories and subcategories

### Step 5.10: Locations Management Pages

Create/verify pages for:
- Districts
- Tehsils
- Union Councils
- Post Offices
- Constituencies
- General Areas

**✅ Admin Features Complete (Core)**

---

## PART 6: SECURITY & TESTING (Hours 46-55)

### Step 6.1: Implement Security Headers

**File:** `src/app/api/middleware.ts` or `next.config.ts` (create/update)

```typescript
// Add security headers
export const securityHeaders = {
  "Strict-Transport-Security": "max-age=31536000; includeSubDomains",
  "X-Frame-Options": "DENY",
  "X-Content-Type-Options": "nosniff",
  "X-XSS-Protection": "1; mode=block",
  "Content-Security-Policy":
    "default-src 'self'; script-src 'self' 'unsafe-inline'; style-src 'self' 'unsafe-inline'",
};
```

### Step 6.2: Implement RBAC Enforcement

Add permission checks to every API endpoint:

```typescript
async function requirePermission(adminId: string, permission: string) {
  const has = await checkPermission(adminId, permission);
  if (!has) {
    throw new Error("Unauthorized");
  }
}
```

### Step 6.3: Implement Rate Limiting

```bash
npm install express-rate-limit
```

Add to critical endpoints (login, complaint submission, AI chat)

### Step 6.4: Test Complete Workflows

Test end-to-end:
1. Citizen complaint submission → database → admin sees it
2. Admin accepts/declines → citizen notification
3. Admin resolves → citizen feedback
4. AI chat → Grok API → response
5. Announcements → database → citizen display
6. Search & filters → working

### Step 6.5: Security Testing

Test for:
- IDOR (change complaint ID)
- XSS (inject HTML)
- SQL injection (special characters)
- Unauthorized access (without auth)
- Permission bypass (direct API calls)
- File upload exploits

### Step 6.6: Database Testing

Verify:
- Foreign keys work
- Constraints enforced
- Migrations reversible
- Data integrity

**✅ Security & Testing Complete**

---

## PART 7: FINAL POLISH & DEPLOYMENT (Hours 56-60)

### Step 7.1: Theme & Styling System

Ensure consistent:
- Colors (green primary, white, neutral grays)
- Typography (readable fonts)
- Spacing (consistent padding/margins)
- Icons (unified icon library)
- Status badges (consistent colors)

### Step 7.2: Mobile Responsive Testing

Test on:
- Mobile (390px) — should stack vertically
- Tablet (768px) — 2-column layout
- Desktop (1024px+) — full layout

### Step 7.3: Urdu & RTL Support

Ensure:
- Urdu text displays properly
- RTL layout when in Urdu mode
- Forms accept Urdu input
- Chat supports Urdu

### Step 7.4: Accessibility Audit

Test:
- Keyboard navigation (Tab key)
- Screen reader compatibility
- Color contrast (4.5:1 for text)
- Focus indicators visible

### Step 7.5: Performance Optimization

- Image lazy loading
- Code splitting
- Bundle size optimization
- Database query optimization
- API response caching

### Step 7.6: Deployment Preparation

- Set environment variables
- Test in production-like environment
- Prepare deployment guide
- Create backup strategy
- Document API endpoints

### Step 7.7: Final Documentation

- README with setup instructions
- API documentation
- Admin guide
- Citizen guide
- Troubleshooting

---

## COMPLETION CRITERIA

Project is **COMPLETE** when:

✅ All database tables created and migrated  
✅ All API endpoints implemented and tested  
✅ Authentication working with security hardening  
✅ RBAC enforced server-side  
✅ Citizen workflow complete (submit → track → feedback)  
✅ Admin workflow complete (dashboard → manage → respond)  
✅ Real complaint submission & persistence verified  
✅ Real admin request list with actual counts  
✅ Real notifications working  
✅ Real AI integration with Grok (not mock)  
✅ Announcements from database  
✅ Quick Alerts system working  
✅ SLA tracking & automatic escalation  
✅ Professional UI with consistent design  
✅ Mobile responsive  
✅ Urdu/RTL support  
✅ Security hardening implemented  
✅ All audit logging working  
✅ No fake data, no mock responses  
✅ Complete end-to-end testing passed  

---

**Estimated Timeline:**  
- Phase 1 (Database): 4 hours
- Phase 2 (Backend): 6 hours
- Phase 3 (APIs): 10 hours
- Phase 4 (Frontend): 15 hours
- Phase 5 (Admin Features): 10 hours
- Phase 6 (Security): 10 hours
- Phase 7 (Polish): 5 hours

**Total: 60 hours**

Next step: Begin Phase 1 — Database implementation.
