# Welcome to Meteor - Google Summer of Code 2026

Welcome to the Meteor project! We're excited that you're interested in contributing to Meteor through Google Summer of Code 2026.

---

## About Meteor

Meteor is a full-stack JavaScript platform for developing modern web and mobile applications. Since 2011, Meteor has been empowering developers to build real-time applications with less code and faster iteration cycles.

### Key Features

- **Full-Stack JavaScript**: Write your entire application in JavaScript/TypeScript, from database to UI
- **Real-Time by Default**: Data synchronization happens automatically through DDP (Distributed Data Protocol)
- **Isomorphic Code**: Share code between client and server seamlessly
- **Hot Code Push**: Deploy updates to users without app store approval (mobile)
- **Integrated Build System**: Bundling, minification, and transpilation out of the box
- **MongoDB Integration**: First-class support with reactive queries via Minimongo

### Meteor Today

Meteor continues to evolve with modern JavaScript ecosystem:
- **Meteor 3.x**: Full async/await support, Node.js 22 compatibility
- **Rspack Integration**: Modern bundler support for faster builds
- **TypeScript First**: Enhanced type definitions and tooling
- **Active Community**: Thousands of developers building production applications

---

## Google Summer of Code

Google Summer of Code (GSoC) is a global program focused on bringing more student developers into open source software development. Students work with an open source organization on a programming project during their break from school.

### Program Timeline 2026

| Date | Milestone |
|------|-----------|
| January - February | Organizations apply to Google |
| February 26 | Accepted organizations announced |
| March 24 - April 8 | Student application period |
| May 8 | Accepted students announced |
| May 8 - June 1 | Community bonding period |
| June 2 - August 25 | Coding period |
| September 3 | Final results announced |

> **Note**: Dates are tentative. Please check the [official GSoC timeline](https://summerofcode.withgoogle.com/) for confirmed dates.

---

## How to Get Started

### 0. Say Hello in our `gsoc` [discord channel](https://discord.gg/hZkTCaVjmT)

### 1. Explore the Codebase

```bash
# Clone the repository
git clone https://github.com/meteor/meteor.git
cd meteor

# Read the development guide
cat DEVELOPMENT.md

# Explore the packages
ls packages/
```

**Key directories to explore:**
- `packages/` - Core Meteor packages (meteor, mongo, ddp, accounts, etc.)
- `tools/` - CLI and build system
- `npm-packages/` - Published npm packages
- `.github/workflows/` - CI/CD configuration

### 2. Set Up Your Development Environment

Follow the instructions in [DEVELOPMENT.md](DEVELOPMENT.md) to set up your local development environment.

**Requirements:**
- Node.js 22.x
- Git

### 3. Run the Tests

```bash
# Run self-tests
./meteor self-test

# Test a specific package
./meteor test-packages <package-name>
```

### 4. Find a Good First Issue

Look for issues labeled with:
- `good first issue` - Perfect for newcomers
- `help wanted` - Community contributions welcome
- `gsoc` - Specifically tagged for GSoC

Browse issues: [github.com/meteor/meteor/issues](https://github.com/meteor/meteor/issues)

---

## Project Ideas

We have prepared several project ideas for GSoC 2026, ranging from small (90 hours) to large (350 hours) projects, it's not related with the project dificult, just with the size.

**See the full list:** [gsoc-2026-projects.md](gsoc-2026-projects.md)

### Quick Overview

| Project | Difficulty | Hours |
|---------|------------|-------|
| Client-Side Type Safety | Easy | 90 |
| Release CI/CD Speed & Reliability | Medium | 175 |
| TypeScript Improvements | Medium | 175 |
| OpenTelemetry Support | Hard | 175 |
| .env Support | Hard | 175 |
| Core Code Quality / Linting | Easy | 350 |
| Test Support Improvements | Hard | 350 |

### Proposing Your Own Idea

We also welcome original project ideas! If you have an idea that would benefit the Meteor ecosystem:

1. Present your idea in the **#gsoc channel on Discord**
2. Get feedback from the community and mentors
3. Refine your proposal based on feedback
4. Submit your final proposal through the #gsoc channel

---

## Writing Your Proposal

A strong proposal should include:

### 1. About You
- Your background and experience
- Relevant projects or contributions
- Why you're interested in Meteor
- Your availability during the coding period

### 2. Project Details
- Which project you're applying for (or your own idea)
- Your understanding of the problem
- Proposed solution and approach
- Technical details and implementation plan

### 3. Timeline
- Week-by-week breakdown of tasks
- Milestones and deliverables
- Buffer time for unexpected challenges
- How you'll handle any planned absences

### 4. Communication
- How you plan to stay in touch with mentors
- Your preferred communication style
- Timezone and available hours

### Proposal Tips

- **Be specific**: Show that you understand the codebase
- **Start early**: Begin engaging with the community before applications open
- **Make contributions**: Even small PRs show initiative
- **Ask questions**: Don't be afraid to clarify requirements
- **Be realistic**: Better to under-promise and over-deliver

---

## Communication Channels

### Discord #gsoc Channel - Primary Communication Hub

**All GSoC communication happens in the #gsoc channel on Discord.**

[Join Meteor Discord Server](https://discord.gg/hZkTCaVjmT)

**Important:** The **#gsoc channel** is the official and primary communication channel for the entire GSoC program. All activities must happen there:

- All proposal presentations and submissions
- Questions about projects and requirements
- Discussions with mentors and other candidates
- Progress updates during the coding period
- Weekly check-ins and status reports
- Final project presentations

**Why Discord?**
- Real-time communication with mentors
- Easy collaboration with other contributors
- Quick feedback on ideas and questions
- Community support and networking
- Transparent process visible to all participants

### GitHub
**Code and issues**

[github.com/meteor/meteor](https://github.com/meteor/meteor)

- Submit pull requests
- Report and discuss issues
- Review existing code

### Meteor Forums
**Additional discussions**

[forums.meteor.com](https://forums.meteor.com/)

- Long-form technical discussions
- Archive of decisions and proposals
- Community announcements

---

## Important Links

### GSoC Official
- [Google Summer of Code Website](https://summerofcode.withgoogle.com/)
- [GSoC Student Guide](https://google.github.io/gsocguides/student/)
- [GSoC FAQ](https://developers.google.com/open-source/gsoc/faq)

### Meteor Resources
- [Meteor website](https://meteor.com/)
- [Meteor Documentation](https://docs.meteor.com/)
- [Meteor GitHub Repository](https://github.com/meteor/meteor)
- [Meteor Forums](https://forums.meteor.com/)
- [Meteor Discord](https://discord.gg/hZkTCaVjmT)
- [Contributing Guide](CONTRIBUTING.md)
- [Development Guide](DEVELOPMENT.md)

### Project Specific
- [GSoC 2026 Project Ideas](gsoc-2026-projects.md)


---

## What We Look For in Contributors

### Technical Skills
- Proficiency in JavaScript/TypeScript
- Understanding of Node.js and npm ecosystem
- Familiarity with Git and GitHub workflow
- Experience with testing and debugging

### Soft Skills
- Clear written communication
- Ability to work independently
- Willingness to ask questions and seek feedback
- Commitment to meeting deadlines

### Bonus Points
- Previous contributions to Meteor or other open source projects
- Experience with real-time applications
- Knowledge of MongoDB
- Understanding of build systems and tooling

---

## Mentorship

Each GSoC student will be paired with one or more mentors from the Meteor community. Mentors will:

- Guide you through the project
- Review your code and provide feedback
- Help you navigate the codebase
- Support your growth as an open source contributor

### What We Expect From Students

- Regular communication with mentors (at least weekly)
- Timely responses to feedback
- Proactive problem-solving
- Honest reporting of progress and blockers
- Respect for the community code of conduct

---

## Code of Conduct

All participants in the Meteor community are expected to follow our [Code of Conduct](CODE_OF_CONDUCT.md). We are committed to providing a welcoming and inclusive environment for everyone.

---

## FAQ

### Q: I'm new to open source. Can I still apply?

**A:** Absolutely! GSoC is designed for new and beginner contributors. Start by exploring the codebase, fixing small issues, and engaging with the community.

### Q: Do I need to know Meteor before applying?

**A:** Not necessarily, but familiarity helps. We recommend building a small Meteor app to understand the framework before diving into core development.

### Q: Can I apply for multiple projects?

**A:** You can submit up to 3 proposals to different organizations, but only one project can be accepted. Focus on quality over quantity.

### Q: What if I have questions about a project?

**A:** Ask in the **#gsoc channel on Discord**! This is where all GSoC communication happens. Asking good questions shows initiative and helps you write a better proposal.

### Q: How much time should I dedicate?

**A:** GSoC projects require significant time commitment:
- Small (90 hours): ~6-8 hours/week
- Medium (175 hours): ~12-15 hours/week
- Large (350 hours): ~25-30 hours/week

### Q: Can I work on my own project idea?

**A:** Yes! We welcome original ideas that benefit the Meteor ecosystem. Discuss your idea with the community before submitting.

---

## Next Steps

1. **Join Discord**: Go to [discord.gg/hZkTCaVjmT](https://discord.gg/hZkTCaVjmT) and join the **#gsoc channel**
2. **Introduce Yourself**: Say hi in #gsoc and tell us about your interests
3. **Explore**: Read the [project ideas](gsoc-2026-projects.md) and explore the codebase
4. **Engage**: Ask questions and discuss projects in the #gsoc channel
5. **Contribute**: Start with small contributions to get familiar with the process
6. **Present**: Share your proposal draft in #gsoc for feedback
7. **Apply**: Submit your final proposal through the #gsoc channel

---

## Contact

**All GSoC communication must happen in the #gsoc channel on Discord.**

[Join Discord and go to #gsoc](https://discord.gg/hZkTCaVjmT)

This is where you will:
- Ask questions and get support
- Present your proposal to mentors
- Receive feedback and guidance
- Participate in all GSoC activities

---

We look forward to working with you this summer!

**The Meteor Team**

