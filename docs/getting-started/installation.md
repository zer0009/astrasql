# Installation Guide

Complete installation instructions for AstraSQL Agent.

## System Requirements

### Minimum Requirements
- **OS**: Linux, macOS, or Windows
- **Node.js**: 18.0.0 or higher
- **npm**: 9.0.0 or higher
- **MongoDB**: 5.0 or higher (or MongoDB Atlas)
- **RAM**: 2GB minimum (4GB recommended)
- **Disk**: 5GB free space

### Recommended Requirements
- **Node.js**: 20.x LTS
- **MongoDB**: 6.0 or higher
- **RAM**: 8GB or more
- **Disk**: 20GB free space (for logs and data)

## Installation Methods

### Method 1: Docker (Recommended)

The easiest way to get started:

```bash
# Clone repository
git clone https://github.com/zer0009/astrasql.git
cd astrasql

# Copy environment template
cp .env.example .env

# Edit .env with your configuration
nano .env

# Start with Docker Compose
docker-compose up -d
```

See [Docker Deployment](../deployment/docker.md) for detailed instructions.

### Method 2: Manual Installation

#### Step 1: Install Node.js

**macOS (Homebrew):**
```bash
brew install node@20
```

**Linux (Ubuntu/Debian):**
```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt-get install -y nodejs
```

**Windows:**
Download from [nodejs.org](https://nodejs.org/)

#### Step 2: Install MongoDB

**Option A: MongoDB Atlas (Cloud - Recommended)**
1. Sign up at [mongodb.com/cloud/atlas](https://www.mongodb.com/cloud/atlas)
2. Create a free cluster
3. Create a database user
4. Whitelist your IP (or `0.0.0.0/0` for development)
5. Get connection string

**Option B: Local MongoDB**
- **macOS**: `brew install mongodb-community`
- **Linux**: Follow [MongoDB installation guide](https://www.mongodb.com/docs/manual/installation/)
- **Windows**: Download from [mongodb.com](https://www.mongodb.com/try/download/community)

#### Step 3: Clone Repository

```bash
git clone https://github.com/zer0009/astrasql.git
cd astrasql
```

#### Step 4: Install Backend Dependencies

```bash
cd backend
npm install
```

#### Step 5: Install Frontend Dependencies

```bash
cd ../frontend
npm install
```

#### Step 6: Configure Environment

**Backend `.env` file:**
```env
# Required
MONGODB_URI=mongodb://localhost:27017/astrasql
# Or for Atlas:
# MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/astrasql

PORT=3000
FRONTEND_URL=http://localhost:5174

# LLM Provider (at least one required)
OPENAI_API_KEY=sk-...
# OR
ANTHROPIC_API_KEY=sk-ant-...
# OR
GOOGLE_API_KEY=...
# OR for local models
OLLAMA_URL=http://localhost:11434

# Optional: Email notifications
SMTP_HOST=smtp.gmail.com
SMTP_PORT=587
SMTP_USER=your-email@gmail.com
SMTP_PASS=your-app-password
NOTIFICATION_EMAIL=notifications@yourdomain.com
```

**Frontend `.env` file (optional):**
```env
VITE_API_URL=http://localhost:3000
```

#### Step 7: Start Services

**Terminal 1 - Backend:**
```bash
cd backend
npm run dev
```

**Terminal 2 - Frontend:**
```bash
cd frontend
npm run dev
```

#### Step 8: Verify Installation

1. Open `http://localhost:5174`
2. You should see the AstraSQL homepage
3. Check backend health: `http://localhost:3000/health`

## Production Installation

### Using PM2 (Process Manager)

```bash
# Install PM2 globally
npm install -g pm2

# Start backend
cd backend
pm2 start server.js --name astrasql-backend

# Start frontend (build first)
cd ../frontend
npm run build
pm2 serve dist 5174 --name astrasql-frontend
```

### Using Systemd (Linux)

See [Production Deployment](../deployment/self-hosted.md) for systemd service files.

## Verification Checklist

- [ ] Node.js 18+ installed
- [ ] MongoDB running and accessible
- [ ] Backend starts without errors
- [ ] Frontend builds successfully
- [ ] Can access frontend at configured port
- [ ] Health endpoint returns 200
- [ ] LLM API key configured and working
- [ ] Database connection test succeeds

## Next Steps

- [Configuration Guide](./configuration.md)
- [First Query](./first-query.md)
- [Production Deployment](../deployment/self-hosted.md)

## Troubleshooting

See [Common Issues](../troubleshooting/common-issues.md) for solutions to common problems.

---

**Next**: [Configuration Guide](./configuration.md)

