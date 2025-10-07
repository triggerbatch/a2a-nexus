# Nexus A2A System - Quick Reference Card

## 🚀 Quick Start (5 Minutes)

```bash
# 1. Setup
python scripts/setup_a2a.py

# 2. Test installation
python scripts/test_a2a_installation.py

# 3. Run example
python examples/a2a_quickstart.py

# 4. Start UI
nexus run
```

## 📁 File Structure

```
nexus/
├── nexus_base/
│   ├── a2a/                          # Core A2A components
│   │   ├── agent_card.py            # Agent metadata
│   │   ├── transport_layer.py       # Message routing
│   │   ├── agent_executor.py        # Task execution
│   │   ├── agent_registry.py        # Service discovery
│   │   ├── a2a_server.py            # Central server
│   │   ├── a2a_orchestrator.py      # Workflow engine
│   │   ├── orchestration_config.py  # Config loader
│   │   ├── conversation_manager.py  # Multi-turn chat
│   │   ├── delegation_manager.py    # Task delegation
│   │   └── collaborative_memory.py  # Shared context
│   ├── a2a_configs/                  # Orchestration configs
│   │   ├── research_team.yaml
│   │   └── content_team.yaml
│   └── a2a_manager.py                # Integration layer
├── api/
│   └── a2a_routes.py                 # REST API endpoints
├── streamlit_ui/
│   ├── a2a_orchestration_chat.py    # Chat interface
│   ├── a2a_network_monitor.py       # Network dashboard
│   └── a2a_config_editor.py         # Config editor
└── scripts/
    ├── setup_a2a.py                  # Setup script
    ├── test_a2a_installation.py     # Test script
    └── migrate_to_a2a.py            # Migration tool
```

## 🔧 Core Components

### Agent Card
```python
from nexus.nexus_base.a2a.agent_card import AgentCard

card = AgentCard(
    name="my_agent",
    agent_type="worker",  # or "orchestrator"
    avatar="🤖"
)
```

### Transport Layer
```python
transport = TransportLayer()
transport.register_agent("agent1")

await transport.send_message(
    from_agent="agent1",
    to_agent="agent2",
    message_type="request",
    payload="Hello"
)
```

### Agent Executor
```python
executor = AgentExecutor(agent, agent_card, transport)
result = await executor.execute_task("Do something")
await executor.send_to_agent("other_agent", "message")
```

### Registry
```python
registry = AgentRegistry()
registry.register(agent_card)
agents = registry.list_agents(status="online")
```

### Orchestrator
```python
config = OrchestrationConfig("config.yaml")
orch = A2AOrchestrator(config, server)
results = await orch.execute_workflow("workflow_name", "input")
```

## 📝 YAML Configuration

### Basic Structure
```yaml
orchestration:
  name: "team_name"
  description: "Team description"
  orchestrator: "coordinator_agent"
  
  agents:
    - name: "coordinator_agent"
      role: "orchestrator"
      profile: "adam"
    
    - name: "worker_agent"
      role: "worker"
      profile: "adam"
  
  communication_patterns:
    - from: "coordinator_agent"
      to: ["worker_agent"]
  
  workflows:
    - name: "task_workflow"
      steps:
        - agent: "coordinator_agent"
          action: "plan"
          output_to: ["worker_agent"]
        - agent: "worker_agent"
          action: "execute"
```

## 🌐 API Endpoints

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/a2a/start` | POST | Start A2A server |
| `/a2a/orchestrations` | GET | List orchestrations |
| `/a2a/orchestrations/{name}/load` | POST | Load orchestration |
| `/a2a/orchestrations/chat` | POST | Interactive chat |
| `/a2a/orchestrations/workflow` | POST | Execute workflow |
| `/a2a/network/status` | GET | Network status |
| `/a2a/agents/discover` | POST | Discover agents |
| `/a2a/messages/history` | GET | Message history |

## 💻 Python API

### Initialize and Run
```python
import asyncio
from nexus.nexus_base.nexus import Nexus

async def main():
    # Initialize
    nexus = Nexus()
    await nexus.start_a2a_server()
    
    # Load orchestration
    orch = nexus.load_a2a_orchestration("research_team")
    
    # Execute workflow
    results = await nexus.a2a_manager.execute_workflow(
        "research_team",
        "research_and_report",
        "AI trends 2024"
    )
    
    # Interactive mode
    response = await nexus.a2a_manager.chat_with_orchestration(
        "research_team",
        "Summarize the findings"
    )
    
    # Check status
    status = nexus.get_a2a_network_status()

asyncio.run(main())
```

## 🎨 Streamlit UI

### Pages
1. **A2A Orchestration** - Chat with orchestrations
2. **A2A Network** - Monitor agents and messages
3. **A2A Config** - Edit configurations

### Usage
```bash
nexus run
# Navigate to A2A Orchestration page
# Select orchestration → Load → Start chatting
```

## 🔍 Common Patterns

### Pattern 1: Sequential Workflow
```yaml
steps:
  - agent: "agent1"
    action: "step1"
    output_to: ["agent2"]
  - agent: "agent2"
    action: "step2"
    output_to: ["agent3"]
  - agent: "agent3"
    action: "step3"
```

### Pattern 2: Parallel Processing
```yaml
steps:
  - agent: "coordinator"
    action: "distribute"
    output_to: ["worker1", "worker2", "worker3"]
  - agent: "worker1"
    action: "process"
    output_to: ["coordinator"]
  # worker2 and worker3 similar
  - agent: "coordinator"
    action: "aggregate"
```

### Pattern 3: Review Cycle
```yaml
steps:
  - agent: "creator"
    action: "create"
    output_to: ["reviewer"]
  - agent: "reviewer"
    action: "review"
    output_to: ["creator"]  # Can loop back
  - agent: "creator"
    action: "finalize"
```

## 🐛 Troubleshooting

| Problem | Solution |
|---------|----------|
| Import errors | Run `python scripts/test_a2a_installation.py` |
| Config not found | Check `nexus/nexus_base/a2a_configs/` |
| Agent not responding | Verify agent initialization and profile |
| High latency | Reduce message size, optimize prompts |
| Memory issues | Clear message history, limit queue size |

## 📊 Monitoring

### Key Metrics
```python
status = nexus.get_a2a_network_status()
print(f"Agents online: {status['online_agents']}")
print(f"Messages: {status['message_count']}")
```

### Logs
```python
import logging
logging.basicConfig(level=logging.DEBUG)
logger = logging.getLogger('nexus.a2a')
```

## 🔐 Security

- Store API keys in environment variables
- Validate all user inputs
- Implement rate limiting
- Use authentication for APIs
- Don't log sensitive data

## 📈 Performance Tips

1. **Use async/await** for all I/O operations
2. **Cache configurations** to avoid repeated YAML parsing
3. **Limit message history** to prevent memory growth
4. **Optimize prompts** for faster agent responses
5. **Monitor queue sizes** to detect bottlenecks

## 🚀 Deployment

### Docker
```bash
docker-compose up -d
```

### Environment Variables
```bash
export OPENAI_API_KEY="your-key"
export LOG_LEVEL="INFO"
export A2A_HEARTBEAT_TIMEOUT="120"
```

### Health Check
```bash
curl http://localhost:8000/health
```

## 📚 Resources

- **Documentation**: See artifacts for complete guide
- **Examples**: `examples/` directory
- **Tests**: `tests/test_a2a.py`
- **API Docs**: http://localhost:8000/docs

## 🎯 Common Use Cases

### 1. Research Team
Coordinate researchers, analysts, and writers

### 2. Content Creation
Ideation → Writing → Review → Publishing

### 3. Code Review
Security → Performance → Style → Approval

### 4. Customer Support
Triage → Investigate → Resolve → Follow-up

### 5. Data Pipeline
Extract → Transform → Analyze → Report

## ⚡ Quick Commands

```bash
# Setup
python scripts/setup_a2a.py

# Test
python scripts/test_a2a_installation.py
pytest tests/test_a2a.py -v

# Migrate existing project
python scripts/migrate_to_a2a.py

# Run examples
python examples/a2a_quickstart.py
python examples/a2a_production_example.py

# Start services
nexus run                              # Streamlit UI
uvicorn nexus.api.main:app --reload   # API
```

## 🔄 Update Workflow

1. **Backup**: `python scripts/migrate_to_a2a.py`
2. **Update**: Copy new files
3. **Test**: `python scripts/test_a2a_installation.py`
4. **Deploy**: Restart services

---

# Complete Feature Checklist

## ✅ Implemented Features

### Core Features
- [x] Agent Card system
- [x] Transport Layer (Direct, HTTP-ready)
- [x] Agent Executor
- [x] Agent Registry & Discovery
- [x] A2A Server
- [x] A2A Orchestrator
- [x] Workflow Engine
- [x] Message Routing
- [x] Task Delegation

### Communication
- [x] Direct messaging
- [x] Broadcast messaging
- [x] Message queuing
- [x] Message history
- [x] Conversation management

### UI Components
- [x] Orchestration Chat page
- [x] Network Monitor page
- [x] Configuration Editor
- [x] Status dashboards
- [x] Message viewers

### API
- [x] REST endpoints
- [x] Workflow execution
- [x] Agent discovery
- [x] Network status
- [x] Health checks

### Configuration
- [x] YAML-based configs
- [x] Dynamic loading
- [x] Validation
- [x] Example configs
- [x] Config editor

### Integration
- [x] Nexus integration
- [x] Agent Manager integration
- [x] Profile Manager integration
- [x] Action Manager integration
- [x] Database integration

### Tooling
- [x] Setup scripts
- [x] Testing utilities
- [x] Migration tools
- [x] Example code
- [x] Documentation

### Advanced Features
- [x] Collaborative memory
- [x] Multi-turn conversations
- [x] Task delegation
- [x] Error handling
- [x] Logging
- [x] Metrics tracking

### Production Features
- [x] Health checks
- [x] Retry logic
- [x] Error recovery
- [x] Monitoring
- [x] Deployment guide

---

# Integration Roadmap

## Phase 1: Basic Setup ✅
- [x] Create directory structure
- [x] Copy core files
- [x] Create example configs
- [x] Run setup script

## Phase 2: Core Integration ✅
- [x] Modify nexus.py
- [x] Add API routes
- [x] Add UI pages
- [x] Test basic functionality

## Phase 3: Advanced Features ✅
- [x] Conversation management
- [x] Task delegation
- [x] Collaborative memory
- [x] Advanced monitoring

## Phase 4: Production Ready ✅
- [x] Error handling
- [x] Logging
- [x] Metrics
- [x] Deployment guide
- [x] Documentation

## Phase 5: Your Custom Features 🚀
- [ ] Add custom transport protocols
- [ ] Implement custom agents
- [ ] Create domain-specific orchestrations
- [ ] Add your business logic
- [ ] Deploy to production

---

# Success Criteria

✅ **Installation**: All files in correct locations  
✅ **Integration**: Nexus has A2A functionality  
✅ **Configuration**: At least one orchestration defined  
✅ **Testing**: All tests pass  
✅ **UI**: A2A pages accessible  
✅ **API**: Endpoints responding  
✅ **Workflow**: Can execute at least one workflow  
✅ **Monitoring**: Can view network status  
✅ **Documentation**: Complete guide available  

---

# Next Steps

1. ✅ Review all artifacts
2. ✅ Copy files to your project
3. ✅ Run setup script
4. ✅ Test installation
5. ✅ Create your first orchestration
6. ✅ Execute a workflow
7. 🚀 Build your multi-agent application!

---

**Need Help?**
- Check troubleshooting section
- Review examples
- Run test scripts
- Check logs

**Ready to Deploy?**
- Review deployment guide
- Setup monitoring
- Configure production settings
- Deploy with confidence!

🎉 **You now have a complete, production-ready A2A system!**
