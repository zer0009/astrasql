# Getting Started with Natural Language SQL

**Published**: November 15, 2025  
**Author**: AstraSQL Team  
**Category**: Getting Started  
**Tags**: `tutorial`, `getting-started`, `natural-language`, `sql`

---

## Introduction

Have you ever wished you could just ask your database a question in plain English and get an answer? That's exactly what AstraSQL Agent enables. In this guide, we'll walk you through your first natural language SQL query.

## What is Natural Language SQL?

Natural Language SQL (NL-SQL) allows you to query databases using everyday language instead of writing complex SQL statements. Instead of:

```sql
SELECT customer_name, SUM(order_total) as revenue
FROM customers c
JOIN orders o ON c.id = o.customer_id
WHERE o.created_at >= DATE_TRUNC('month', CURRENT_DATE)
GROUP BY c.id, c.name
ORDER BY revenue DESC
LIMIT 10;
```

You can simply ask:

> "Show me the top 10 customers by revenue this month"

## Why Use Natural Language SQL?

### 1. **Accessibility**
Not everyone knows SQL. With natural language, anyone on your team can query data:
- Product managers can explore metrics
- Sales teams can analyze performance
- Executives can get insights on demand

### 2. **Speed**
Writing SQL takes time, especially for complex queries. Natural language is faster:
- No need to remember table names
- No need to understand joins
- Just ask what you want to know

### 3. **Reduced Errors**
SQL syntax errors are common. Natural language queries are validated and optimized automatically.

### 4. **Privacy-First**
AstraSQL uses metadata-only AI processing, meaning:
- Your actual data never leaves your database
- Only schema information (table/column names) is sent to AI
- Complete privacy and security

## Your First Query

Let's start with a simple example. Imagine you have a database with `customers` and `orders` tables.

### Step 1: Connect Your Database

1. Open AstraSQL Agent
2. Navigate to Settings → Database Connections
3. Add your database:
   - **Type**: PostgreSQL, MySQL, or your database type
   - **Host**: Your database host
   - **Database**: Your database name
   - **Credentials**: Username and password

### Step 2: Write Your First Query

In the query interface, type:

```
How many customers do we have?
```

AstraSQL will:
1. Understand your question
2. Generate the appropriate SQL
3. Execute it safely
4. Show you the result

**Generated SQL:**
```sql
SELECT COUNT(*) as customer_count
FROM customers;
```

### Step 3: Try More Complex Queries

Once you're comfortable, try these:

**Revenue Analysis:**
```
What's our total revenue this month?
```

**Customer Insights:**
```
Show me customers who haven't ordered in the last 30 days
```

**Product Performance:**
```
Which products have the highest sales this quarter?
```

**Time-Based Queries:**
```
Compare this month's sales to last month
```

## Best Practices

### 1. Be Specific
✅ Good: "Show me the top 10 customers by revenue in January 2025"  
❌ Vague: "Show customers"

### 2. Include Context
✅ Good: "What are the total sales by region for Q4 2024?"  
❌ Missing context: "Sales by region"

### 3. Use Natural Language
✅ Natural: "Who are our best customers?"  
❌ Too technical: "SELECT customers WHERE revenue > threshold"

### 4. Ask Follow-Up Questions
AstraSQL remembers context, so you can ask:
- "Show me more details"
- "What about last month?"
- "Break it down by category"

## Common Query Patterns

### Aggregations
```
What's the average order value?
How many orders did we get today?
What's the total revenue this year?
```

### Filtering
```
Show me orders from the last week
Which customers are in California?
What products cost more than $100?
```

### Grouping
```
Sales by region
Revenue by product category
Orders by month
```

### Comparisons
```
Compare this month to last month
Show me the difference between Q3 and Q4
Which products perform better than average?
```

## Advanced Features

### Multi-Table Queries
AstraSQL automatically handles joins:
```
Show me customer names and their total order value
```

### Time-Based Analysis
```
What's our revenue trend over the last 6 months?
Show me daily active users this week
```

### Conditional Logic
```
Show me customers who ordered more than 5 times
Which products have low inventory?
```

## Troubleshooting

### Query Not Understanding?
- Be more specific about what you want
- Include table or column names if you know them
- Break complex questions into simpler ones

### Wrong Results?
- Check your database connection
- Verify table and column names exist
- Review the generated SQL before executing

### Performance Issues?
- Use specific date ranges
- Limit result sets ("top 10", "first 100")
- Add indexes to frequently queried columns

## Next Steps

Now that you've written your first natural language query:

1. **Explore Dashboards**: Create visualizations from your queries
2. **Save Queries**: Build a library of frequently used queries
3. **Share Insights**: Export results or share dashboards with your team
4. **Read More**: Check out our [Advanced Query Techniques](./advanced-query-techniques.md) guide

## Conclusion

Natural language SQL makes data accessible to everyone. With AstraSQL Agent, you can:
- Query databases without SQL knowledge
- Get answers faster
- Maintain complete privacy
- Build powerful dashboards

Ready to get started? [Try AstraSQL Agent free for 14 days](https://astrasql.com).

---

## Related Resources

- [Documentation: Natural Language Queries](../../docs/user-guide/natural-language-queries.md)
- [Tutorial: Building Your First Dashboard](./building-first-dashboard.md)
- [Use Case: Self-Service BI](./self-service-bi.md)

---

**Questions?** Join our [Discord community](https://discord.gg/astrasql) or [open an issue](https://github.com/zer0009/astrasql/issues).

