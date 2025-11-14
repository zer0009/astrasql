# Contributing to AstraSQL

Thank you for your interest in contributing to AstraSQL Agent! We welcome contributions from the community.

## How to Contribute

### Reporting Bugs

1. Check if the bug has already been reported in [Issues](https://github.com/zer0009/astrasql/issues)
2. If not, create a new issue with:
   - Clear title and description
   - Steps to reproduce
   - Expected vs actual behavior
   - Environment details (OS, Node version, etc.)
   - Screenshots if applicable

### Suggesting Features

1. Check [FEATURE_ROADMAP.md](FEATURE_ROADMAP.md) to see if it's already planned
2. Open an issue with the "Feature Request" label
3. Include:
   - Use case and problem it solves
   - Proposed solution
   - Alternatives considered

### Code Contributions

1. **Fork the repository**
2. **Create a branch**: `git checkout -b feature/your-feature-name`
3. **Make your changes**
4. **Test thoroughly**
5. **Commit**: Use clear, descriptive commit messages
6. **Push**: `git push origin feature/your-feature-name`
7. **Open a Pull Request**

### Documentation

We welcome documentation improvements:
- Fix typos or clarify explanations
- Add examples
- Improve structure
- Translate to other languages

### Blog Contributions

Want to write for our blog? We welcome:
- Tutorials and guides
- Use cases and case studies
- Technical deep dives
- Industry insights

See [blog/README.md](blog/README.md) for guidelines.

## Development Setup

1. Clone your fork:
```bash
git clone https://github.com/your-username/astrasql.git
cd astrasql
```

2. Install dependencies:
```bash
cd backend && npm install
cd ../frontend && npm install
```

3. Set up environment variables (see [README.md](README.md))

4. Start development:
```bash
# Terminal 1 - Backend
cd backend && npm run dev

# Terminal 2 - Frontend
cd frontend && npm run dev
```

## Code Style

- Follow existing code style
- Use TypeScript for frontend
- Add comments for complex logic
- Keep functions small and focused
- Write meaningful variable names

## Testing

- Test your changes thoroughly
- Add tests for new features
- Ensure existing tests pass
- Test on multiple databases if applicable

## Pull Request Process

1. Update documentation if needed
2. Add/update tests
3. Ensure all tests pass
4. Update CHANGELOG.md if applicable
5. Request review from maintainers

## Questions?

- Open an issue with the "Question" label
- Email: dev@astrasql.com
- Join our community discussions

Thank you for contributing! 🎉

