# JARVIS Implementation Roadmap
**Self-Building AI Assistant: Integration Plan**

Version: 1.0.0
Last Updated: 2025-11-12
Status: Planning Phase

---

## Vision

Transform JARVIS into a self-building AI system by integrating three complementary layers:

1. **JARVIS Core** - Persistent orchestrator with voice interface
2. **MCP Integration** - Tool execution layer (filesystem, git, APIs)
3. **ORTRTA Module** - Parallel analysis engine (spec-driven workflows)

**Result**: AI assistant that can analyze specifications, design solutions, write code, and improve itself.

---

## Architecture Overview

```
┌──────────────────────────────────────────────────────────────┐
│                    User (Voice/Text)                         │
└────────────────────────────┬─────────────────────────────────┘
                             │
                             ▼
┌──────────────────────────────────────────────────────────────┐
│              JARVIS Voice Interface (Port 3738)              │
│  • Speech-to-text (Whisper)                                  │
│  • Text-to-speech (OpenAI TTS)                               │
│  • Iron Man-inspired UI                                      │
└────────────────────────────┬─────────────────────────────────┘
                             │
                             ▼
┌──────────────────────────────────────────────────────────────┐
│              JARVIS Brain (Port 8055)                        │
│  ┌────────────────────────────────────────────────────────┐  │
│  │ Intent Interpretation (Claude)                         │  │
│  │ • Classify user requests                               │  │
│  │ • Determine complexity                                 │  │
│  │ • Route to appropriate handler                         │  │
│  └────────────────────────────────────────────────────────┘  │
│  ┌────────────────────────────────────────────────────────┐  │
│  │ Orchestration Layer (NEW - Phase 2.5+)                 │  │
│  │ • MCP tool selection                                   │  │
│  │ • ORTRTA workflow invocation                           │  │
│  │ • Multi-step task planning                             │  │
│  │ • Result synthesis                                     │  │
│  └────────────────────────────────────────────────────────┘  │
└──┬────────────────────────┬──────────────────────────────┬───┘
   │                        │                              │
   ▼                        ▼                              ▼
┌─────────┐         ┌──────────────┐              ┌──────────────┐
│ Archon  │         │ MCP Servers  │              │   ORTRTA     │
│Knowledge│         │ (Phase 2.5)  │              │   Module     │
│  Base   │         ├──────────────┤              │ (Phase 3.0)  │
└─────────┘         │ Filesystem   │              ├──────────────┤
                    │ • Read       │              │ Isildur's    │
                    │ • Write      │              │ Bane         │
                    │ • Edit       │              │              │
                    │ • Search     │              │ 7 Rings:     │
                    ├──────────────┤              │ • Nenya (PM) │
                    │ Git          │              │ • Narya (Res)│
                    │ • Status     │              │ • Vilya (Arc)│
                    │ • Commit     │              │ • Thrór (SM) │
                    │ • Branch     │              │ • Fëanor(Dev)│
                    │ • Push/Pull  │              │ • Nazgûl (QA)│
                    ├──────────────┤              │ • Annatar(UX)│
                    │ GitHub       │              └──────────────┘
                    │ • Issues     │
                    │ • PRs        │
                    │ • Comments   │
                    ├──────────────┤
                    │ Code Exec    │
                    │ • Python     │
                    │ • Node       │
                    │ • Shell      │
                    └──────────────┘
```

---

## Current State (Phase 2.1) ✅

**Status**: Implemented and tested

### What Works
- ✅ Voice interface (speech-to-text, text-to-speech)
- ✅ Intent interpretation via Claude
- ✅ Smart model selection (Haiku 4.5 / Sonnet 4.5)
- ✅ Cost optimization (72% savings via caching)
- ✅ Real agent spawning (Vilya, Nenya, Narya)
- ✅ Parallel agent execution (asyncio.gather)
- ✅ Claude Skills with prompt caching (90% savings)
- ✅ Knowledge base integration (Archon)

### What's Missing
- ❌ Cannot read/write files
- ❌ Cannot execute code
- ❌ Cannot use git
- ❌ Cannot create PRs
- ❌ Cannot modify itself
- ❌ Limited to analysis (no implementation)

---

## Phase 2.5: MCP Foundation
**Timeline**: 2-3 weeks
**Goal**: Enable JARVIS to interact with filesystem, git, and external tools

### Objectives

1. **MCP Server Integration**
   - Connect to existing MCP servers
   - Implement MCP tool discovery
   - Add MCP tool invocation logic

2. **Filesystem Capabilities**
   - Read files
   - Write files
   - Edit files
   - Search files (glob patterns)

3. **Git Operations**
   - Check status
   - Create branches
   - Commit changes
   - Push/pull to remote

4. **GitHub Integration**
   - Create issues
   - Create pull requests
   - Add comments
   - Check CI status

### Implementation Plan

#### 1. MCP Client in JARVIS Brain (Week 1)

**File**: `python/src/jarvis/mcp_client.py`

```python
"""
MCP Client for JARVIS

Handles communication with MCP servers and tool invocation.
"""

from typing import Dict, List, Optional, Any
import httpx
import logging

logger = logging.getLogger(__name__)

class MCPClient:
    """Client for interacting with MCP servers."""

    def __init__(self):
        self.servers: Dict[str, str] = {}
        self.available_tools: Dict[str, Dict] = {}

    def register_server(self, name: str, url: str):
        """Register an MCP server."""
        self.servers[name] = url
        logger.info(f"Registered MCP server: {name} at {url}")

    async def discover_tools(self, server_name: str) -> List[Dict]:
        """Discover available tools from an MCP server."""
        # Implementation: Call MCP server's tools/list endpoint
        pass

    async def call_tool(self, tool_name: str, arguments: Dict[str, Any]) -> Any:
        """Call an MCP tool with arguments."""
        # Implementation: Route to appropriate MCP server
        pass

    async def read_file(self, file_path: str) -> str:
        """Read a file via filesystem MCP server."""
        return await self.call_tool("filesystem:read", {"path": file_path})

    async def write_file(self, file_path: str, content: str) -> bool:
        """Write a file via filesystem MCP server."""
        return await self.call_tool("filesystem:write", {
            "path": file_path,
            "content": content
        })

    async def git_commit(self, message: str, files: List[str] = None) -> bool:
        """Commit changes via git MCP server."""
        return await self.call_tool("git:commit", {
            "message": message,
            "files": files or []
        })
```

**Integration in brain.py**:
```python
from .mcp_client import MCPClient

class JARVIS:
    def __init__(self, user_name: str = None):
        # ... existing init ...

        # MCP Integration (Phase 2.5)
        self.mcp = MCPClient()
        self._register_mcp_servers()

    def _register_mcp_servers(self):
        """Register available MCP servers."""
        # Filesystem MCP
        fs_url = os.getenv("MCP_FILESYSTEM_URL", "http://localhost:8060")
        self.mcp.register_server("filesystem", fs_url)

        # Git MCP
        git_url = os.getenv("MCP_GIT_URL", "http://localhost:8061")
        self.mcp.register_server("git", git_url)

        # GitHub MCP
        github_url = os.getenv("MCP_GITHUB_URL", "http://localhost:8062")
        self.mcp.register_server("github", github_url)
```

#### 2. New Intent Handler: `code_operation` (Week 1-2)

```python
async def _handle_code_operation(self, text: str, intent: Intent) -> str:
    """
    Handle code and file operations using MCP tools.

    Examples:
    - "Read the brain.py file"
    - "Create a new file for Phase 3.0 planning"
    - "Commit these changes to git"
    """
    logger.info("Handling code operation via MCP")

    # Use Claude to determine which MCP tools to use
    tool_plan = await self._plan_mcp_operations(text)

    # Execute tools in sequence or parallel
    results = []
    for tool in tool_plan["tools"]:
        result = await self.mcp.call_tool(
            tool["name"],
            tool["arguments"]
        )
        results.append(result)

    # Synthesize results
    return self._synthesize_mcp_results(results)
```

#### 3. MCP Server Setup (Week 2)

**Docker Compose Updates**:
```yaml
# docker-compose.yml additions

  mcp-filesystem:
    image: anthropic/mcp-filesystem:latest
    container_name: mcp-filesystem
    ports:
      - "8060:8060"
    environment:
      - MCP_ALLOWED_PATHS=/workspace
    volumes:
      - .:/workspace
    networks:
      - archon-network

  mcp-git:
    image: anthropic/mcp-git:latest
    container_name: mcp-git
    ports:
      - "8061:8061"
    environment:
      - GIT_USER_NAME=${GIT_USER_NAME}
      - GIT_USER_EMAIL=${GIT_USER_EMAIL}
    volumes:
      - .:/workspace
    networks:
      - archon-network

  mcp-github:
    image: anthropic/mcp-github:latest
    container_name: mcp-github
    ports:
      - "8062:8062"
    environment:
      - GITHUB_TOKEN=${GITHUB_TOKEN}
    networks:
      - archon-network
```

#### 4. Testing & Validation (Week 2-3)

**Test Cases**:
1. Read file: "JARVIS, read the ROADMAP.md file"
2. Write file: "JARVIS, create a new TODO.md file"
3. Git commit: "JARVIS, commit these changes"
4. GitHub PR: "JARVIS, create a pull request"

**Success Criteria**:
- ✅ JARVIS can read files from project
- ✅ JARVIS can write new files
- ✅ JARVIS can commit to git
- ✅ JARVIS can create PRs
- ✅ All operations logged and trackable

### Deliverables

- [ ] `python/src/jarvis/mcp_client.py` - MCP client implementation
- [ ] Updated `python/src/jarvis/brain.py` - MCP integration
- [ ] Updated `docker-compose.yml` - MCP servers
- [ ] Updated `.env.example` - MCP configuration
- [ ] `docs/MCP-INTEGRATION.md` - MCP usage guide
- [ ] Test suite for MCP operations

---

## Phase 3.0: ORTRTA Integration
**Timeline**: 1-2 months
**Goal**: Enable JARVIS to invoke ORTRTA workflows for parallel analysis

### Objectives

1. **ORTRTA Module Integration**
   - Package ORTRTA as callable service
   - Expose ORTRTA workflows via API/MCP
   - Enable JARVIS to invoke workflows

2. **Workflow Orchestration**
   - Map JARVIS intents to ORTRTA workflows
   - Handle specification analysis requests
   - Synthesize ORTRTA results for voice output

3. **Hybrid Execution**
   - Simple tasks: JARVIS handles directly
   - Complex analysis: JARVIS delegates to ORTRTA
   - Combined tasks: JARVIS orchestrates both MCP + ORTRTA

### Architecture

```
User: "JARVIS, analyze this PRD for our new feature"
  ↓
JARVIS Brain interprets intent
  ↓
Intent type: "spec_analysis" (complex)
  ↓
JARVIS selects approach:
  ├─ Option 1: Use existing agents (Vilya, Nenya, Narya)
  └─ Option 2: Invoke ORTRTA workflow (4-7 parallel Rings!)
  ↓
JARVIS chooses ORTRTA (faster, more comprehensive)
  ↓
JARVIS calls: ortrta.execute_workflow("spec-to-analysis", prd_path)
  ↓
ORTRTA spawns 4 Rings in parallel:
  ├─ Nenya (requirements)
  ├─ Narya (feasibility)
  ├─ Vilya (architecture)
  └─ Thrór (timeline)
  ↓
ORTRTA binds results → comprehensive analysis
  ↓
JARVIS receives analysis
  ↓
JARVIS uses MCP to save analysis to file
  ↓
JARVIS speaks summary to user
```

### Implementation Plan

#### 1. ORTRTA Service Wrapper (Week 1-2)

**File**: `python/src/jarvis/ortrta_client.py`

```python
"""
ORTRTA Client for JARVIS

Enables JARVIS to invoke Project ORTRTA workflows.
"""

class ORTRTAClient:
    """Client for invoking ORTRTA workflows."""

    def __init__(self, ortrta_service_url: str):
        self.service_url = ortrta_service_url
        self.available_workflows = []

    async def list_workflows(self) -> List[str]:
        """Get available ORTRTA workflows."""
        return [
            "spec-to-analysis",
            "feature-to-implementation",
            "rapid-architecture",
            "parallel-discovery",
            "sprint-catalyst"
        ]

    async def execute_workflow(
        self,
        workflow_name: str,
        specification: str
    ) -> Dict:
        """
        Execute an ORTRTA workflow.

        Returns bound results from all forged Rings.
        """
        # Call ORTRTA service with workflow name and spec
        # ORTRTA spawns appropriate Rings in parallel
        # Returns comprehensive analysis
        pass

    async def analyze_spec(self, spec_path: str) -> Dict:
        """Execute spec-to-analysis workflow."""
        return await self.execute_workflow("spec-to-analysis", spec_path)
```

#### 2. Intent Routing Logic (Week 2-3)

```python
async def _handle_agent_task(self, text: str, intent: Intent) -> str:
    """
    Enhanced agent task handler with ORTRTA integration.

    Decision tree:
    - Simple analysis (1-2 agents) → Use JARVIS agents
    - Complex analysis (3+ perspectives) → Invoke ORTRTA
    - Implementation tasks → ORTRTA + MCP tools
    """

    # Analyze task complexity
    if self._should_use_ortrta(intent):
        # Use ORTRTA for parallel analysis
        return await self._execute_ortrta_workflow(text, intent)
    else:
        # Use existing JARVIS agents
        return await self._forge_ring(...)  # Existing logic
```

#### 3. Hybrid Workflows (Week 3-4)

**Example: Feature Development**

```python
async def _develop_feature(self, feature_spec: str) -> str:
    """
    Complete feature development using ORTRTA + MCP.

    Steps:
    1. ORTRTA analyzes specification (parallel Rings)
    2. JARVIS uses MCP to create feature branch
    3. ORTRTA generates implementation plan
    4. JARVIS uses MCP to write code files
    5. JARVIS uses MCP to commit and create PR
    """

    # Step 1: Analyze with ORTRTA
    analysis = await self.ortrta.execute_workflow(
        "feature-to-implementation",
        feature_spec
    )

    # Step 2: Create branch via MCP
    branch_name = f"feature/{analysis['feature_name']}"
    await self.mcp.git_create_branch(branch_name)

    # Step 3: Write code via MCP
    for file in analysis['files_to_create']:
        await self.mcp.write_file(file['path'], file['content'])

    # Step 4: Commit via MCP
    await self.mcp.git_commit(
        f"feat: {analysis['feature_name']}\n\n{analysis['summary']}"
    )

    # Step 5: Create PR via MCP
    pr_url = await self.mcp.github_create_pr(
        title=analysis['feature_name'],
        body=analysis['pr_description']
    )

    return f"Feature implemented! PR created: {pr_url}"
```

### Deliverables

- [ ] `python/src/jarvis/ortrta_client.py` - ORTRTA client
- [ ] ORTRTA service wrapper (FastAPI or MCP server)
- [ ] Updated intent routing in brain.py
- [ ] Hybrid workflow implementations
- [ ] `docs/ORTRTA-INTEGRATION.md` - Integration guide

---

## Phase 4.0: Self-Building JARVIS
**Timeline**: 2-3 months
**Goal**: Enable JARVIS to modify and improve itself

### Objectives

1. **Self-Modification Capabilities**
   - JARVIS can read its own code
   - JARVIS can design improvements (via Vilya/ORTRTA)
   - JARVIS can implement changes (via MCP)
   - JARVIS can test changes
   - JARVIS can commit improvements

2. **Safety Mechanisms**
   - Sandbox for testing self-modifications
   - Rollback on failures
   - User approval gates for critical changes
   - Automated testing before deployment

3. **Meta-Orchestration**
   - JARVIS plans its own development roadmap
   - JARVIS creates tasks for itself
   - JARVIS tracks progress
   - JARVIS reports on improvements

### Example Self-Building Workflow

```
User: "JARVIS, add a security analysis Ring to ORTRTA"

JARVIS Process:
1. Analyzes request
   └─> Intent: "self_improvement" (add capability)

2. Designs the solution (via Vilya)
   └─> Architecture: New Ring prompt in ORTRTA module
   └─> Prompt structure: Security specialist with penetration testing expertise
   └─> Integration: Update ring registry, add to workflows

3. Implements the change (via MCP)
   └─> Read existing Ring prompts for template
   └─> Write new security-ring.md prompt
   └─> Update config.yaml to register new Ring
   └─> Update workflow definitions to include security Ring

4. Tests the change (via ORTRTA)
   └─> Invoke test workflow with security Ring
   └─> Verify output quality
   └─> Check integration with existing Rings

5. Commits improvement (via MCP)
   └─> Create branch: feature/add-security-ring
   └─> Commit changes with detailed message
   └─> Create PR for user review

6. Reports to user
   └─> "I've designed and implemented the security Ring.
        PR #3 is ready for your review at [URL].
        The new Ring can perform security audits,
        penetration testing analysis, and vulnerability assessments."
```

### Safety Mechanisms

#### Approval Gates
```python
class SelfModificationGuard:
    """Safety guard for JARVIS self-modifications."""

    CRITICAL_FILES = [
        "python/src/jarvis/brain.py",
        "python/src/jarvis/main.py",
        ".env",
        "docker-compose.yml"
    ]

    async def requires_approval(self, file_path: str) -> bool:
        """Check if modification requires user approval."""
        return any(
            critical in file_path
            for critical in self.CRITICAL_FILES
        )

    async def test_modification(self, changed_files: List[str]) -> bool:
        """Test modifications in sandbox before applying."""
        # Run in Docker container
        # Execute test suite
        # Verify JARVIS still functions
        # Return True if safe, False if broken
        pass
```

#### Rollback System
```python
async def safe_self_modify(self, modification_plan: Dict) -> bool:
    """
    Safely modify JARVIS with automatic rollback on failure.
    """
    # Create checkpoint
    checkpoint = await self.mcp.git_create_branch("checkpoint/pre-modification")

    try:
        # Apply modifications
        for file in modification_plan['files']:
            await self.mcp.write_file(file['path'], file['content'])

        # Test modifications
        if await self.test_self_modifications():
            # Success! Commit changes
            await self.mcp.git_commit("Self-improvement: " + modification_plan['description'])
            return True
        else:
            # Tests failed, rollback
            await self.mcp.git_checkout(checkpoint)
            logger.error("Self-modification failed tests, rolled back")
            return False

    except Exception as e:
        # Error during modification, rollback
        await self.mcp.git_checkout(checkpoint)
        logger.error(f"Self-modification error: {e}, rolled back")
        return False
```

### Deliverables

- [ ] `python/src/jarvis/self_builder.py` - Self-modification logic
- [ ] `python/src/jarvis/safety_guard.py` - Safety mechanisms
- [ ] Sandbox testing environment
- [ ] Rollback system
- [ ] `docs/SELF-BUILDING.md` - Self-building guide

---

## Timeline Summary

| Phase | Duration | Start | End | Status |
|-------|----------|-------|-----|--------|
| **Phase 2.1** | 2 weeks | Completed | Completed | ✅ Done |
| **Phase 2.5** | 2-3 weeks | Week 1 | Week 3 | 📋 Planned |
| **Phase 3.0** | 1-2 months | Week 4 | Week 11 | 📋 Planned |
| **Phase 4.0** | 2-3 months | Week 12 | Week 23 | 📋 Planned |

**Total Timeline**: ~6 months to fully self-building JARVIS

---

## Success Metrics

### Phase 2.5 Success
- [ ] JARVIS can read files from project
- [ ] JARVIS can write new files
- [ ] JARVIS can commit to git
- [ ] JARVIS can create GitHub PRs
- [ ] 5+ MCP tools integrated and tested

### Phase 3.0 Success
- [ ] JARVIS can invoke ORTRTA workflows
- [ ] Spec analysis 4x faster with ORTRTA
- [ ] End-to-end feature development (spec → code → PR)
- [ ] 3+ hybrid workflows implemented

### Phase 4.0 Success
- [ ] JARVIS successfully modifies its own code
- [ ] JARVIS adds new capability without human coding
- [ ] 100% safety record (no broken deployments)
- [ ] JARVIS manages its own development backlog

---

## Risk Mitigation

### Technical Risks

| Risk | Impact | Mitigation |
|------|--------|------------|
| MCP servers unstable | High | Use established MCP implementations, add retry logic |
| Self-modification breaks JARVIS | Critical | Robust testing, approval gates, automatic rollback |
| ORTRTA integration complex | Medium | Start with simple workflow, iterate |
| Cost escalation with more agents | Medium | Monitor usage, set budget limits, use caching |

### Architectural Risks

| Risk | Impact | Mitigation |
|------|--------|------------|
| Over-engineered solution | Medium | Build incrementally, validate each phase |
| Performance degradation | Medium | Load testing, optimize critical paths |
| Maintenance burden | High | Strong documentation, modular design |

---

## Next Steps (For Tomorrow)

### Immediate Actions

1. **Review this roadmap**
   - Validate approach
   - Adjust timeline if needed
   - Prioritize features

2. **Set up development environment**
   - Install MCP server dependencies
   - Configure Docker compose
   - Test MCP server connectivity

3. **Start Phase 2.5 Week 1**
   - Create `mcp_client.py` skeleton
   - Implement basic tool discovery
   - Test file read operations

4. **Create tracking system**
   - GitHub project board for roadmap
   - Issues for each phase
   - Milestones for deliverables

---

## Questions to Answer Tomorrow

1. Should we build ORTRTA as a standalone service or integrate directly?
2. Which MCP servers should we prioritize (filesystem, git, github)?
3. What's the approval process for self-modifications?
4. How do we handle JARVIS development while it's self-building?
5. What's the testing strategy for self-modifications?

---

## Resources

### Documentation
- [Anthropic MCP Documentation](https://docs.anthropic.com/mcp)
- [Claude Skills API](https://docs.anthropic.com/skills)
- [Project ORTRTA README](../project-otrta/bmad/project-ortrta/README.md)

### Repositories
- JARVIS: `sjohns2/project-jarvis`
- ORTRTA: `bmad/project-ortrta` (local module)
- MCP Servers: `anthropic/mcp-*`

### Related Work
- [JARVIS.md](JARVIS.md) - Current feature documentation
- [Phase 2.1 Implementation](https://github.com/sjohns2/project-jarvis/pull/2)

---

*"By the end of Phase 4.0, JARVIS won't just assist with development - JARVIS will BE the developer."*

**Ready to build the future? 🚀**
