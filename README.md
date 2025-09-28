# Influencer Platform

En moderne platform hvor influencere og sponsorer mødes og laver deals. Platformen fungerer som et sikkert marketplace og samarbejdsværktøj.

## 🚀 Funktioner

### Brugerprofiler
- **Influencer-profiler**: Følgertal, engagement-rate, målgruppe-data, tidligere kampagner, priser
- **Sponsor-profiler**: Virksomhedstype, kampagnebudget, ønsket målgruppe
- **Verificering**: ID og sociale medier verificering

### Matchmaking & Søgning
- AI-matching mellem brands og influencere baseret på niche, målgruppe, engagement og budget
- Avanceret søgning (lokation, branche, platform, follower-range)

### Deal Flow
- Chat og forhandling direkte i platformen
- Standardiserede kontrakter (auto-genereret, kan tilpasses)
- Betalingssystem med escrow (pengene frigives først når samarbejdet er gennemført)
- Mulighed for milestones og delbetaling

### Kampagnestyring
- Brands kan oprette kampagner med KPI'er, deadlines og ønskede leverancer
- Influencere kan ansøge eller blive inviteret
- Dashboard med status, deadlines og leverancer

### Rapportering & Analyse
- Automatisk tracking af reach, engagement og performance (via API'er til SoMe-platforme)
- PDF/Excel-rapport til brands efter kampagnens afslutning

### Sikkerhed & Trust
- ID-verificering og anti-fraud-system
- Escrow-betaling sikrer begge parter
- Ratingsystem for både sponsorer og influencere
- GDPR-compliance, datasikkerhed, 2FA login

### Ekstra
- Indbygget kalender til planlægning
- AI-tekstgenerator til kampagnebeskrivelser
- Notifikationssystem (mail + app push)
- Flersproget support (engelsk, dansk m.fl.)

## 🛠️ Teknologistack

### Backend
- **Node.js** med **TypeScript**
- **Express.js** framework
- **Prisma** ORM med **PostgreSQL** database
- **JWT** authentication med **2FA** support
- **Stripe** betalingssystem
- **Socket.IO** for real-time messaging
- **Nodemailer** for email services

### Frontend (kommer senere)
- **React** med **Next.js**
- **TypeScript**
- **Tailwind CSS** for styling
- **Socket.IO Client** for real-time features

## 📦 Installation

1. **Klon repositoryet**
```bash
git clone <repository-url>
cd influencer-platform
```

2. **Installer dependencies**
```bash
npm install
```

3. **Opret .env fil**
```bash
cp env.example .env
```

4. **Konfigurer environment variabler**
Rediger `.env` filen med dine database credentials og API keys.

5. **Setup database**
```bash
# Generer Prisma client
npm run db:generate

# Push database schema
npm run db:push

# Eller kør migrations
npm run db:migrate
```

6. **Start development server**
```bash
npm run dev
```

## 🔧 Environment Variabler

```env
# Database
DATABASE_URL="postgresql://username:password@localhost:5432/influencer_platform"

# JWT
JWT_SECRET="your-super-secret-jwt-key-here"
JWT_EXPIRES_IN="7d"

# Server
PORT=3000
NODE_ENV="development"

# Email
SMTP_HOST="smtp.gmail.com"
SMTP_PORT=587
SMTP_USER="your-email@gmail.com"
SMTP_PASS="your-app-password"

# Stripe
STRIPE_SECRET_KEY="sk_test_..."
STRIPE_PUBLISHABLE_KEY="pk_test_..."
STRIPE_WEBHOOK_SECRET="whsec_..."

# Social Media APIs
INSTAGRAM_CLIENT_ID=""
INSTAGRAM_CLIENT_SECRET=""
YOUTUBE_API_KEY=""
TIKTOK_CLIENT_KEY=""
TIKTOK_CLIENT_SECRET=""
```

## 📚 API Endpoints

### Authentication
- `POST /api/auth/register` - Registrer ny bruger
- `POST /api/auth/login` - Login
- `POST /api/auth/verify-email` - Verificer email
- `POST /api/auth/forgot-password` - Glemt password
- `POST /api/auth/reset-password` - Reset password
- `POST /api/auth/setup-2fa` - Setup 2FA
- `POST /api/auth/verify-2fa` - Verificer 2FA
- `GET /api/auth/me` - Get current user

### Users
- `GET /api/users/profile` - Get user profile
- `PUT /api/users/profile` - Update user profile
- `POST /api/users/profile/image` - Upload profile image
- `DELETE /api/users/account` - Delete account

### Influencers
- `GET /api/influencers/profile` - Get influencer profile
- `PUT /api/influencers/profile` - Update influencer profile
- `GET /api/influencers/stats` - Get influencer stats
- `GET /api/influencers/search` - Search influencers

### Sponsors
- `GET /api/sponsors/profile` - Get sponsor profile
- `PUT /api/sponsors/profile` - Update sponsor profile
- `GET /api/sponsors/stats` - Get sponsor stats
- `GET /api/sponsors/search` - Search sponsors

### Campaigns
- `GET /api/campaigns` - Get campaigns
- `POST /api/campaigns` - Create campaign
- `GET /api/campaigns/:id` - Get campaign by ID
- `PUT /api/campaigns/:id` - Update campaign
- `DELETE /api/campaigns/:id` - Delete campaign

### Deals
- `GET /api/deals` - Get deals
- `POST /api/deals` - Create deal
- `GET /api/deals/:id` - Get deal by ID
- `PUT /api/deals/:id` - Update deal
- `POST /api/deals/:id/accept` - Accept deal
- `POST /api/deals/:id/reject` - Reject deal

### Messages
- `GET /api/messages/deal/:dealId` - Get messages for deal
- `POST /api/messages` - Send message
- `PUT /api/messages/:id/read` - Mark message as read

### Payments
- `POST /api/payments/create-intent` - Create payment intent
- `GET /api/payments/history` - Get payment history
- `POST /api/payments/release/:paymentId` - Release escrow payment
- `POST /api/payments/webhook` - Stripe webhook

### Reviews
- `GET /api/reviews/user/:userId` - Get reviews for user
- `POST /api/reviews` - Create review
- `PUT /api/reviews/:id` - Update review
- `DELETE /api/reviews/:id` - Delete review

### Notifications
- `GET /api/notifications` - Get notifications
- `PUT /api/notifications/:id/read` - Mark notification as read
- `PUT /api/notifications/read-all` - Mark all notifications as read
- `DELETE /api/notifications/:id` - Delete notification

## 🗄️ Database Schema

Platformen bruger PostgreSQL med Prisma ORM. Hovedtabellerne inkluderer:

- **Users** - Brugerdata og authentication
- **InfluencerProfiles** - Influencer-specifik data
- **SponsorProfiles** - Sponsor-specifik data
- **Campaigns** - Kampagne data
- **Deals** - Deal data og status
- **Messages** - Chat beskeder
- **Payments** - Betalingsdata
- **Reviews** - Anmeldelser
- **Notifications** - Notifikationer

## 🔒 Sikkerhed

- JWT authentication med refresh tokens
- 2FA support med TOTP
- Rate limiting
- Input validation
- SQL injection beskyttelse via Prisma
- CORS konfiguration
- Helmet.js for security headers

## 🚀 Deployment

1. **Build application**
```bash
npm run build
```

2. **Start production server**
```bash
npm start
```

## 📝 Licens

MIT License

## 🤝 Bidrag

Bidrag er velkomne! Åbn en issue eller pull request.

## 📞 Support

For support, kontakt os på [email@example.com](mailto:email@example.com)
