# Installation Guide

## Forudsætninger

Før du kan køre denne platform, skal du have følgende installeret:

### 1. Node.js og npm
Download og installer Node.js fra [nodejs.org](https://nodejs.org/)
- Anbefalet version: Node.js 18.x eller nyere
- npm kommer med Node.js

### 2. PostgreSQL Database
Download og installer PostgreSQL fra [postgresql.org](https://www.postgresql.org/download/)
- Opret en database kaldet `influencer_platform`
- Husk brugernavn og password

### 3. Git (valgfrit)
Download Git fra [git-scm.com](https://git-scm.com/)

## Installation Steps

### 1. Installer Node.js
```bash
# Download fra nodejs.org og installer
# Verificer installation
node --version
npm --version
```

### 2. Installer PostgreSQL
```bash
# Download fra postgresql.org og installer
# Opret database
createdb influencer_platform
```

### 3. Installer Dependencies
```bash
# I projektmappen
npm install
```

### 4. Konfigurer Environment
```bash
# Kopier env.example til .env
cp env.example .env

# Rediger .env filen med dine credentials
```

### 5. Setup Database
```bash
# Generer Prisma client
npm run db:generate

# Push database schema
npm run db:push
```

### 6. Start Development Server
```bash
npm run dev
```

## Troubleshooting

### Node.js ikke fundet
- Sørg for at Node.js er installeret og tilføjet til PATH
- Genstart terminal efter installation

### Database connection fejl
- Tjek at PostgreSQL kører
- Verificer DATABASE_URL i .env filen
- Sørg for at database eksisterer

### Port allerede i brug
- Ændr PORT i .env filen
- Eller stop den process der bruger porten

## Næste Skridt

Efter installation:
1. Opret en .env fil med dine credentials
2. Kør `npm run db:push` for at oprette database tabeller
3. Start serveren med `npm run dev`
4. Test API'et på http://localhost:3000/health
