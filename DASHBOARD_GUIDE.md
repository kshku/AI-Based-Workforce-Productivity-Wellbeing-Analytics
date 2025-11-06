# Dashboard Implementation Guide

## 🎯 Overview

Two separate role-based dashboards have been implemented that automatically display after successful login.

## 📋 Dashboard Routes

### Member Dashboard
- **Route**: `/dashboard/member`
- **Access**: Only members can access
- **Redirect**: Automatic redirect on login for members
- **URL**: `http://localhost:5173/dashboard/member`

### Supervisor Dashboard
- **Route**: `/dashboard/supervisor`
- **Access**: Only supervisors can access
- **Redirect**: Automatic redirect on login for supervisors
- **URL**: `http://localhost:5173/dashboard/supervisor`

## 🔄 Login Flow

```
User Login
    ↓
Backend validates credentials
    ↓
Returns user with role: 'member' or 'supervisor'
    ↓
Frontend stores user in AuthContext
    ↓
Redirect to home route '/'
    ↓
App checks user role
    ↓
    ├─ role === 'member' → /dashboard/member
    └─ role === 'supervisor' → /dashboard/supervisor
```

## 📱 Member Dashboard Features

### Header Section
- **Welcome Message**: Personalized greeting with user name
- **User Menu**: 
  - Display user name and email
  - Logout button
  - User avatar with initials

### Key Metrics Cards
1. **Productivity Score**
   - Current metric placeholder
   - Status: Coming soon
   
2. **Wellness Score**
   - Current metric placeholder
   - Status: Coming soon

3. **Tasks Completed**
   - Track completed tasks
   - Status: Coming soon

4. **Health Check-ins**
   - Wellness survey count
   - Status: Coming soon

### Available Features Section
- 📊 **Productivity Tracking** - Log daily tasks and track metrics
- 💚 **Wellness Check-in** - Daily wellness surveys
- 📈 **Analytics Dashboard** - View performance insights
- 🎯 **Goal Setting** - Set and track personal/professional goals

### Styling
- **Color Scheme**: Purple gradient header (primary colors)
- **Layout**: Responsive grid (1 col on mobile, 2 on tablet, 4 on desktop)
- **Background**: Light gray (#fafafa)

---

## 👔 Supervisor Dashboard Features

### Header Section
- **Welcome Message**: "Welcome, [Name]! 👋" with supervisor context
- **Role Badge**: "Supervisor" chip displayed
- **User Menu**:
  - Display user name, email, and department
  - Logout button
  - User avatar with initials
- **Gradient Header**: Purple gradient background distinct from member dashboard

### Key Metrics Cards
1. **Team Members**
   - Count of active team members
   - Status: Coming soon

2. **Average Productivity**
   - Team average productivity score
   - Status: Coming soon

3. **Wellness Score**
   - Overall team wellbeing metric
   - Status: Coming soon

4. **Reports Generated**
   - Monthly report count
   - Status: Coming soon

### Management Features Section
Six feature cards for supervisor capabilities:

1. **👥 Team Management**
   - View and manage team members
   - Track performance and assignments

2. **📊 Analytics & Reports**
   - Generate detailed reports
   - Productivity and wellness metrics

3. **💬 Feedback & Insights**
   - Provide team feedback
   - Monitor wellness responses

4. **🎯 Goal Tracking**
   - Set team goals
   - Real-time progress monitoring

5. **📈 Performance Insights**
   - Deep dive into trends
   - Individual and team performance

6. **⚙️ Settings & Configuration**
   - Configure team settings
   - Manage policies and preferences

### Quick Info Section
- Informational card about supervisor capabilities
- Note about upcoming features
- Instructions to explore dashboard

### Styling
- **Color Scheme**: Purple gradient header (distinct gradient)
- **Layout**: Responsive grid (1 col on mobile, 2 on tablet)
- **Background**: Light gray (#fafafa)
- **Info Card**: Light blue background for quick info section

---

## 🛡️ Protected Routes

### Route Protection Logic

```typescript
<Route
  path="/dashboard/member"
  element={user && user.role === 'member' ? <MemberDashboard /> : <Navigate to="/login" replace />}
/>

<Route
  path="/dashboard/supervisor"
  element={user && user.role === 'supervisor' ? <SupervisorDashboard /> : <Navigate to="/login" replace />}
/>
```

- If user tries to access supervisor dashboard as member → redirects to login
- If user tries to access member dashboard as supervisor → redirects to login
- If user not logged in → redirects to login
- Home route '/' automatically redirects based on role

---

## 🔐 Authentication Context Updates

### UserRole Type Changed
```typescript
// OLD
export type UserRole = 'supervisor' | 'employee';

// NEW
export type UserRole = 'supervisor' | 'member';
```

This aligns with backend registration where role can be:
- `'supervisor'` - Admin/manager accounts
- `'member'` - Employee accounts

---

## 📂 File Structure

```
app/frontend/src/pages/
├── Login.tsx                    # Login page (existing)
├── SupervisorRegister.tsx       # Supervisor registration (existing)
├── MemberRegister.tsx           # Member registration (existing)
├── MemberDashboard.tsx          # NEW - Member dashboard
└── SupervisorDashboard.tsx      # NEW - Supervisor dashboard

app/frontend/src/context/
└── AuthContext.tsx              # UPDATED - UserRole type changed

app/frontend/src/
└── App.tsx                       # UPDATED - Added dashboard routes
```

---

## 🎨 UI Components Used

### Common Components
- **AppBar + Toolbar**: Navigation header
- **Container**: Responsive content container
- **Card + CardContent**: Content sections
- **Stack**: Vertical layout
- **Box**: Flexible container
- **Typography**: Text elements
- **Avatar**: User profile picture
- **Menu + MenuItem**: User menu dropdown
- **Chip**: Role badge on supervisor dashboard

---

## 🚀 How to Test

### Test Member Dashboard
1. Open http://localhost:5173
2. Click "Create Account"
3. Choose "Member Login"
4. Fill registration form:
   - Name: John Employee
   - Email: member@example.com
   - Password: TestPassword123
5. Submit → Redirects to login
6. Login with credentials → Auto-redirects to `/dashboard/member`

### Test Supervisor Dashboard
1. Open http://localhost:5173
2. Click "Create Account"
3. Choose "Supervisor Login"
4. Fill registration form:
   - Name: Jane Manager
   - Email: supervisor@example.com
   - Password: TestPassword123
   - Department: HR
5. Submit → Redirects to login
6. Login with credentials → Auto-redirects to `/dashboard/supervisor`

### Test Route Protection
1. Login as member
2. Manually type URL: http://localhost:5173/dashboard/supervisor
3. Result: Redirects to login page
4. Try opposite with supervisor account
5. Result: Also redirects to login

---

## 📊 Component Hierarchy

### Member Dashboard Component Tree
```
MemberDashboard (Protected Route)
├── AppBar
│   └── Toolbar
│       ├── Logo + Title
│       └── User Avatar Menu
├── Container
│   └── Stack (Main Content)
│       ├── Welcome Card (Gradient)
│       ├── Stats Grid
│       │   ├── Card (Productivity)
│       │   ├── Card (Wellness)
│       │   ├── Card (Tasks)
│       │   └── Card (Health Check-ins)
│       └── Features Section
│           └── Grid of Feature Cards
```

### Supervisor Dashboard Component Tree
```
SupervisorDashboard (Protected Route)
├── AppBar
│   └── Toolbar
│       ├── Logo + Title
│       └── User Avatar Menu
├── Container
│   └── Stack (Main Content)
│       ├── Welcome Card (Gradient with Badge)
│       ├── Key Metrics Section
│       │   └── Grid of 4 Metric Cards
│       ├── Management Features Section
│       │   └── Grid of 6 Feature Cards
│       └── Quick Info Card
```

---

## 🔄 Navigation Flow

### Complete User Journey
```
Home (/) → Check if logged in
    ↓
No → Redirect to /login
    ↓
Yes → Check role
    ├─ Member → /dashboard/member
    └─ Supervisor → /dashboard/supervisor

Logout → Clear auth state → Redirect to /login
```

---

## 🎯 Future Enhancements

- [ ] Add sidebar navigation for dashboard features
- [ ] Implement actual metrics display (currently placeholders)
- [ ] Add notifications dropdown in header
- [ ] Create individual feature pages for each dashboard item
- [ ] Add search/filter functionality for team members (supervisor)
- [ ] Implement data refresh/auto-refresh for metrics
- [ ] Add theme toggle (light/dark mode)
- [ ] Create mobile-responsive sidebar
- [ ] Add breadcrumb navigation

---

## 🛠️ Troubleshooting

### Issue: User stays on login page after successful login

**Solution**: Check AuthContext updates:
- Verify `user` state is set after login
- Check browser console for errors
- Verify localStorage has `authUser` key

### Issue: Wrong dashboard displayed

**Solution**:
- Clear browser cache and localStorage
- Verify backend returns correct `role` value
- Check browser console for role value
- Verify UserRole type matches backend roles

### Issue: Cannot access dashboard URL directly

**Solution**: This is expected behavior (protected routes)
- Login first
- Dashboard should be inaccessible without authentication
- Attempting access redirects to login

---

## 📝 Code Examples

### Accessing User in Dashboard
```typescript
const { user, logout } = useAuth();

if (!user) {
  navigate('/login');
}

// Display user info
<Typography>{user.name}</Typography>
<Typography>{user.email}</Typography>
```

### Logout Handler
```typescript
const handleLogout = () => {
  logout();
  navigate('/login');
};
```

### Check Role (in App.tsx)
```typescript
<Route
  path="/dashboard/member"
  element={user && user.role === 'member' ? <MemberDashboard /> : <Navigate to="/login" replace />}
/>
```

---

## ✅ Testing Checklist

- [ ] Member can register and login
- [ ] Supervisor can register and login
- [ ] Member redirects to member dashboard
- [ ] Supervisor redirects to supervisor dashboard
- [ ] Member cannot access supervisor dashboard
- [ ] Supervisor cannot access member dashboard
- [ ] Logout works and redirects to login
- [ ] Dashboard persists on page refresh (auth stored)
- [ ] Dashboard is responsive on mobile/tablet/desktop
- [ ] User info displays correctly
- [ ] All cards and metrics display properly
- [ ] Menu opens/closes correctly
