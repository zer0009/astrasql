# Quick Start Guide

Get AstraSQL Agent up and running in 5 minutes!

## Prerequisites

- Node.js 18+ and npm 9+
- MongoDB (local or Atlas)
- A database to connect to (PostgreSQL, MySQL, SQLite, etc.)
- An LLM API key (OpenAI, Anthropic, or Google Gemini)

## Step 1: Clone the Repository

```bash
git clone https://github.com/zer0009/astrasql.git
cd astrasql
```

## Step 2: Install Dependencies

```bash
# Install backend dependencies
cd backend
npm install

# Install frontend dependencies
cd ../frontend
npm install
```

## Step 3: Configure Environment

Create a `.env` file in the `backend/` directory:

```env
# Database
MONGODB_URI=mongodb://localhost:27017/astrasql

# Server
PORT=3000
FRONTEND_URL=http://localhost:5174

# LLM Provider (choose one)
OPENAI_API_KEY=your-openai-key
# OR
ANTHROPIC_API_KEY=your-anthropic-key
# OR
GOOGLE_API_KEY=your-google-key
```

## Step 4: Start the Backend

```bash
cd backend
npm run dev
```

The backend will start on `http://localhost:3000`

## Step 5: Start the Frontend

In a new terminal:

```bash
cd frontend
npm run dev
```

The frontend will start on `http://localhost:5174`

## Step 6: Connect Your Database

1. Open `http://localhost:5174` in your browser
2. Navigate to Settings → Database Connections
3. Add your database connection:
   - **Type**: PostgreSQL, MySQL, SQLite, etc.
   - **Host**: Your database host
   - **Port**: Database port
   - **Database**: Database name
   - **Username/Password**: Your credentials

## Step 7: Write Your First Query

1. Go to the Query Interface
2. Type a natural language query:
   ```
   Show me the top 10 customers by revenue this month
   ```
3. Click "Generate SQL" or press Enter
4. Review the generated SQL
5. Click "Execute" to run the query
6. View results in the table or as a chart

## 🎉 You're Done!

You're now ready to use AstraSQL Agent! Try these example queries:

- "What are the total sales by region?"
- "Show me customers who haven't ordered in the last 30 days"
- "What's the average order value by product category?"

## Next Steps

- Read the [Installation Guide](./installation.md) for detailed setup
- Check out [Natural Language Queries](../user-guide/natural-language-queries.md) for query tips
- Explore [Dashboards](../user-guide/dashboards.md) to visualize your data

## Troubleshooting

### Backend won't start
- Check MongoDB is running: `mongosh` or check MongoDB Atlas connection
- Verify environment variables are set correctly
- Check port 3000 is not in use

### Frontend won't start
- Verify Node.js version: `node --version` (should be 18+)
- Clear node_modules and reinstall: `rm -rf node_modules && npm install`
- Check port 5174 is not in use

### Database connection fails
- Verify database credentials
- Check network connectivity
- Ensure database allows connections from your IP

### LLM API errors
- Verify API key is correct
- Check API quota/limits
- Ensure you have credits in your LLM account

## Need Help?

- [Documentation Index](../README.md)
- [Common Issues](../troubleshooting/common-issues.md)
- [GitHub Issues](https://github.com/zer0009/astrasql/issues)

---

**Next**: [Installation Guide](./installation.md)

