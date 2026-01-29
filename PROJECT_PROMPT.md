# Appointment Manager PWA - Complete Prompt Request

## Project Overview
Create a Progressive Web App (PWA) appointment management system with Google Calendar integration, designed for professional coaching/consulting businesses with E³ Leadership branding.

---

## Core Requirements

### 1. **Appointment Management**
- Create, edit, and delete appointments
- Store appointment details:
  - Client name (required)
  - Email address
  - Phone number
  - Service type (consultation, meeting, follow-up, other)
  - Date and time
  - Duration (15min to 2hr options)
  - Status (pending, confirmed, cancelled)
  - Notes
- Local storage for offline access
- Form validation

### 2. **Google Calendar Integration**
- OAuth 2.0 authentication with Google
- Two-way sync with Google Calendar
- Support for multiple Google Calendars simultaneously
- Real-time event fetching
- Create appointments directly to Google Calendar
- View Google Calendar events in the app
- Color-coded calendar identification
- Handle both timed events and all-day events
- Proper timezone handling (use local timezone, not UTC)

### 3. **Calendar Features**
- Mini calendar widget showing current month
- Navigate previous/next months
- Highlight today's date
- Show days with appointments
- Click dates to filter appointments
- Visual indicators for appointment density

### 4. **Views & Filtering**
- **Dashboard View**: Statistics cards (today, this week, this month, connected calendars)
- **All Appointments View**: Complete list with filters
- **Today View**: Focus on current day's schedule
- Filter by:
  - Calendar source (local vs Google)
  - Status (pending, confirmed, cancelled)
  - Service type
  - Search by client name/email

### 5. **Progressive Web App (PWA) Features**
- **Installable**: Add to home screen on iOS, Android, Desktop
- **Offline Mode**: 
  - Service worker caching
  - View appointments without internet
  - Sync when connection restored
  - Offline indicator
- **Push Notifications**:
  - 1 hour before appointment
  - 15 minutes before appointment
  - Click notification to open app
  - Permission request prompt
- **App Manifest**: 
  - Standalone display mode
  - Custom app icon
  - Splash screen
  - Shortcuts (New Appointment, Today's Schedule)

### 6. **Mobile Optimization**
- **Responsive Design**:
  - Mobile-first approach
  - Base font size: 14px mobile, 16px desktop
  - Breakpoints: 480px, 768px, 968px
  - Touch-friendly buttons (minimum 44px touch targets)
  - No accidental zoom (user-scalable=no)
  - Compact spacing on mobile
  - Stacked layouts for small screens
  
- **Mobile-Specific**:
  - Safe area insets for iPhone notch
  - -webkit-tap-highlight-color: transparent
  - Horizontal scrolling tabs
  - 2-column dashboard grid on mobile
  - Optimized modal sizing (95% width mobile)

---

## Design & Branding

### Color Scheme (E³ Leadership)
**Main Colors:**
- Primary Dark: #1a1a1a (backgrounds, text)
- Accent Gold: #d4af37 (brand color, buttons, highlights)
- Secondary Dark: #2d2d2d (gradients)
- Light Background: #f5f5f0 (cream/off-white)
- Text: #333 (body text), #666 (secondary text), #555 (descriptions)

**Secondary Colors:**
- Gold Gradient: #f4d03f (bright gold accents)
- Border Gray: #ddd (borders)
- White: #ffffff (cards, backgrounds)

**Gradients:**
- Main gradient: #1a1a1a → #2d2d2d
- Button gradient: #d4af37 → #f4d03f

### UI Components
- **Header**: Black gradient background with gold accent border, responsive stats display
- **Buttons**: Gold gradient primary buttons, hover effects with box shadows
- **Cards**: White background, 1px gray border, left gold accent stripe
- **Calendar**: Color-coded days (gold for today, light gold for appointments)
- **Status Badges**: Colored pills (green=confirmed, orange=pending, red=cancelled)
- **Modal**: Centered overlay with white rounded card

### Typography
- Font: System fonts (-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto)
- Headers: 600 weight, varying sizes (responsive)
- Body: Regular weight, 14-16px (responsive)
- Uppercase labels: Letter-spacing for stats/badges

---

## Technical Implementation

### Tech Stack
- **Frontend**: Pure HTML, CSS, JavaScript (no frameworks)
- **Storage**: localStorage for local appointments
- **API**: Google Calendar API v3
- **Authentication**: OAuth 2.0 with Google
- **Service Worker**: For offline functionality and notifications
- **Manifest**: Web app manifest for PWA features

### File Structure
```
appointment-pwa/
├── index.html              # Main application
├── service-worker.js       # Offline mode & notifications
├── manifest.json          # PWA configuration
├── icon-192.png           # App icon (small)
├── icon-512.png           # App icon (large)
├── PWA_README.md          # User installation guide
├── GOOGLE_CALENDAR_SETUP.md  # Google API setup
└── TROUBLESHOOTING.md     # Common issues
```

### Key Functions Required

#### Date Handling (Critical!)
```javascript
// Helper to get local date WITHOUT timezone conversion
function getLocalDateString(date = new Date()) {
    const year = date.getFullYear();
    const month = String(date.getMonth() + 1).padStart(2, '0');
    const day = String(date.getDate()).padStart(2, '0');
    return `${year}-${month}-${day}`;
}
```
**Never use** `toISOString().split('T')[0]` as it converts to UTC!

#### Google Calendar Sync
- Fetch events from multiple calendars
- Parse dates with proper timezone handling
- Map Google events to app format
- Store calendar metadata (name, color, ID)
- Handle API errors gracefully

#### PWA Functions
- Service worker registration
- Install prompt handling
- Notification permission request
- Push notification scheduling
- Online/offline detection
- Cache management

### Storage Schema

**Local Appointments:**
```javascript
{
  id: timestamp,
  clientName: string,
  clientEmail: string,
  clientPhone: string,
  serviceType: string,
  date: "YYYY-MM-DD",
  time: "HH:MM",
  duration: string (minutes),
  status: string,
  notes: string,
  googleId?: string,  // If synced to Google
  source: "local"
}
```

**Google Events:**
```javascript
{
  id: "google_calendarId_eventId",
  googleId: string,
  calendarId: string,
  calendarName: string,
  calendarColor: string,
  clientName: string,
  date: "YYYY-MM-DD",
  time: "HH:MM",
  duration: string,
  htmlLink: string,
  source: "google"
}
```

---

## Google Calendar Setup Requirements

### API Configuration
1. Create Google Cloud Project
2. Enable Google Calendar API
3. Create OAuth 2.0 Client ID (Web application)
4. Configure OAuth consent screen (External, test users)
5. Add authorized JavaScript origins
6. Add authorized redirect URIs

### OAuth Scopes
- `https://www.googleapis.com/auth/calendar` (full calendar access)

### Client ID Format
- Looks like: `123456789-abc123def456.apps.googleusercontent.com`
- Stored in localStorage as `googleClientId`
- User enters during first-time setup

---

## User Experience Flow

### First-Time Setup
1. User visits app URL
2. Setup screen appears requesting Google Client ID
3. User enters Client ID
4. Main app loads
5. "Sign in with Google" button appears
6. User authenticates
7. Calendars load and display in sidebar
8. Install prompt appears after 10 seconds
9. Notification permission prompt after 15 seconds

### Daily Use
1. User taps app icon (installed PWA)
2. App opens instantly (cached)
3. View today's schedule
4. Create/edit appointments
5. Receive automatic reminders
6. Changes sync to Google Calendar
7. Works offline if needed

### Appointment Creation
1. Click "New Appointment" button
2. Modal form appears
3. Fill in client details
4. Select date/time/duration
5. Choose service type and status
6. Optionally sync to Google Calendar (checkbox)
7. Save → Creates locally and/or in Google Calendar

---

## Error Handling

### Must Handle:
- No internet connection (offline mode)
- Google API errors (rate limits, auth failures)
- Invalid Client ID
- Missing permissions (calendar, notifications)
- Timezone conversion issues
- DOM elements not found (null checks everywhere)
- Calendar sync conflicts
- Service worker registration failures

### User Feedback:
- Loading indicators
- Success/error messages
- Console logging for debugging
- Helpful error messages with solutions
- Network status indicator

---

## Accessibility & Performance

### Accessibility
- Semantic HTML
- ARIA labels where needed
- Keyboard navigation
- Touch-friendly tap targets (44px minimum)
- High contrast colors (WCAG AA)
- Focus indicators

### Performance
- Lazy loading
- Service worker caching
- Minimal JavaScript bundle
- No external dependencies for core functionality
- Efficient DOM updates
- Debounced search/filter

---

## Documentation Required

### PWA_README.md
- Installation instructions (iOS, Android, Desktop)
- Notification setup guide
- Offline mode explanation
- Feature overview
- Deployment instructions
- Browser compatibility

### GOOGLE_CALENDAR_SETUP.md
- Step-by-step Google Cloud setup
- OAuth configuration
- Client ID retrieval
- Authorized origins setup
- Troubleshooting common issues

### TROUBLESHOOTING.md
- Common error messages
- Solutions for each error
- Browser console debugging
- Reset instructions

---

## Testing Checklist

### Functionality
- [ ] Create appointment locally
- [ ] Create appointment in Google Calendar
- [ ] Edit appointment
- [ ] Delete appointment
- [ ] Filter by status/service/calendar
- [ ] Search by name
- [ ] Calendar navigation
- [ ] Multiple calendar selection
- [ ] Sign out and back in

### PWA Features
- [ ] Install on iOS
- [ ] Install on Android
- [ ] Install on Desktop
- [ ] App icon displays correctly
- [ ] Splash screen shows
- [ ] Works offline
- [ ] Notification permission prompt
- [ ] Receive appointment reminders
- [ ] Click notification opens app

### Responsive Design
- [ ] Test on iPhone SE (375px)
- [ ] Test on standard phone (390px)
- [ ] Test on large phone (430px)
- [ ] Test on tablet (768px)
- [ ] Test on desktop (1024px+)
- [ ] All buttons tappable
- [ ] No horizontal scroll
- [ ] Text readable at all sizes

### Timezone
- [ ] Today's date shows correctly
- [ ] Calendar highlights correct day
- [ ] Google events show on correct dates
- [ ] Appointments created on correct dates
- [ ] Works in different timezones

---

## Deployment

### Hosting Requirements
- HTTPS required (for service worker)
- Serve all files from root
- No server-side code needed
- Static hosting (Netlify, Vercel, GitHub Pages)

### Recommended: Netlify
1. Drag folder to Netlify Drop
2. Instant HTTPS deployment
3. Automatic updates
4. Free tier sufficient

### Post-Deployment
1. Copy live URL
2. Add to Google Cloud OAuth settings
3. Test installation on multiple devices
4. Share with users

---

## Future Enhancement Ideas

### Phase 2 Features
- Multiple notification times (customizable)
- Recurring appointments
- Client database with history
- Email appointment confirmations
- SMS reminders (Twilio integration)
- Calendar event editing in app
- Export appointments (CSV/PDF)
- Analytics dashboard

### Enterprise Features
- Team calendar sharing
- Resource booking
- Payment integration
- CRM integration
- Automated follow-ups
- Client portal
- Video call integration (Zoom/Meet)

---

## Success Criteria

The project is complete when:
- ✅ User can install app on phone/desktop
- ✅ All appointments sync with Google Calendar
- ✅ Multiple calendars work simultaneously
- ✅ Notifications arrive on time
- ✅ Works offline reliably
- ✅ Mobile interface is touch-friendly
- ✅ Dates show correctly in all timezones
- ✅ App loads in under 2 seconds
- ✅ No console errors
- ✅ Professional appearance matching brand

---

## Additional Notes

### Security
- OAuth tokens stored securely
- No sensitive data in URLs
- HTTPS enforced
- Client-side only (no backend)
- User data never leaves device (except Google sync)

### Browser Support
- Chrome/Edge: Full support
- Safari: Install works, notifications limited
- Firefox: Limited PWA features
- Recommended: Chrome for best experience

### Maintenance
- Monitor Google API quota
- Update dependencies annually
- Test on new iOS/Android versions
- Refresh OAuth credentials if needed
- Update service worker cache version

---

## Example Use Cases

### Solo Coach/Consultant
- Manage personal coaching sessions
- Track client appointments
- Send reminders automatically
- Access schedule anywhere
- Professional client experience

### Small Team
- Share office calendar
- Coordinate team schedules
- Track multiple service types
- View team member calendars
- Unified appointment view

### Multi-Location Business
- Location-specific calendars
- Service-based scheduling
- Cross-location visibility
- Centralized management

---

This prompt provides everything needed to recreate this appointment management PWA from scratch!
