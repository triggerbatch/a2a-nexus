# Nexus A2A (Agent-to-Agent) System
## Complete Implementation Guide

---

## Table of Contents

1. [Overview](#overview)
2. [Architecture](#architecture)
3. [Installation](#installation)
4. [Quick Start](#quick-start)
5. [Core Components](#core-components)
6. [Configuration](#configuration)
7. [Streamlit UI](#streamlit-ui)
8. [API Reference](#api-reference)
9. [Examples](#examples)
10. [Testing](#testing)
11. [Troubleshooting](#troubleshooting)

---

## Overview

The Nexus A2A (Agent-to-Agent) system enables seamless collaboration between multiple AI agents through:

- **Agent Discovery**: Find and connect with agents based on capabilities
- **Message Routing**: Efficient communication between agents
- **Orchestration**: Coordinate complex multi-agent workflows
- **Task Delegation**: Distribute work across specialized agents
- **Collaborative Memory**: Share context and knowledge between agents

### Key Features

✅ **Non-Breaking Integration**: Works alongside existing Nexus functionality  
✅ **Flexible Communication**: Direct, broadcast, and queue-based messaging  
✅ **Workflow Engine**: Define multi-step agent collaborations in YAML  
✅ **Real-time Monitoring**: Track agent status and message flow  
✅ **API & UI**: Both programmatic and visual interfaces  
✅ **Extensible**: Easy to add new agents and orchestration patterns

---

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                     Nexus Application                       │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐    │
│  │   Streamlit  │  │  FastAPI     │  │  CLI Tools   │    │
│  │      UI      │  │     API      │  │              │    │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘    │
│         │                 │                 │              │
│         └─────────────────┼─────────────────┘              │
│                          │                                 │
│  ┌──────────────────────▼──────────────────────────┐      │
│  │            A2A Manager                          │      │
│  │  • Load Orchestrations                          │      │
│  │  • Manage Network                               │      │
│  │  • Execute Workflows                            │      │
│  └──────────────────────┬──────────────────────────┘      │
│                        │                                   │
│  ┌────────────────────▼────────────────────────┐         │
│  │         A2A Server (Core)                   │         │
│  ├─────────────────────────────────────────────┤         │
│  │  ┌──────────────┐  ┌──────────────┐        │         │
│  │  │   Registry   │  │  Transport   │        │         │
│  │  │  (Discovery) │  │    Layer     │        │         │
│  │  └──────────────┘  └──────────────┘        │         │
│  │                                             │         │
│  │  ┌────────────────────────────────────┐   │         │
│  │  │    Agent Executors                 │   │         │
│  │  │  • Agent 1  • Agent 2  • Agent 3   │   │         │
│  │  └────────────────────────────────────┘   │         │
│  │                                             │         │
│  │  ┌────────────────────────────────────┐   │         │
│  │  │    Orchestrators                   │   │         │
│  │  │  • Workflow Engine                 │   │         │
│  │  │  • Conversation Manager            │   │         │
│  │  │  • Delegation Manager              │   │         │
│  │  └────────────────────────────────────┘   │         │
│  └─────────────────────────────────────────────┘         │
│                                                           │
│  ┌─────────────────────────────────────────────────┐    │
│  │        Existing Nexus Components                │    │
│  │  • Agent Manager    • Profile Manager           │    │
│  │  • Action Manager   • Knowledge Manager         │    │
│  │  • Memory Manager   • Database                  │    │
│  └─────────────────────────────────────────────────┘    │
│                                                           │
└───────────────────────────────────────────────────────────┘
```

---

## Installation

### 1. File Structure

Create the following directory structure in your Nexus project:

```
nexus/
├── nexus_base/
│   ├── a2a/
│   │   ├── __init__.py
│   │   ├── agent_card.py
│   │   ├── transport_layer.py
│   │   ├── agent_executor.py
│   │   ├── agent_registry.py
│   │   ├── orchestration_config.py
│   │   ├── a2a_server.py
│   │   ├── a2a_orchestrator.py
│   │   ├── conversation_manager.py
│   │   ├── delegation_manager.py
│   │   └── collaborative_memory.py
│   ├── a2a_configs/
│   │   ├── research_team.yaml
│   │   └── content_team.yaml
│   ├── a2a_manager.py
│   └── nexus.py (modified)
├── api/
│   ├── main.py (modified)
│   └── a2a_routes.py (new)
├── streamlit_ui/
│   ├── a2a_orchestration_chat.py (new)
│   ├── a2a_network_monitor.py (new)
│   ├── a2a_config_editor.py (new)
│   └── main.py (modified)
└── tests/
    └── test_a2a.py
```

### 2. Modify Existing Files

#### nexus/nexus_base/nexus.py

Add to the `Nexus` class `__init__` method:

```python
from nexus.nexus_base.a2a_manager import A2AManager

# Add after existing initializations
self.a2a_manager = A2AManager(self)
```

Add these methods to the `Nexus` class:

```python
def get_a2a_manager(self):
    """Get A2A manager"""
    return self.a2a_manager

async def start_a2a_server(self):
    """Start A2A server"""
    await self.a2a_manager.start_server()

def load_a2a_orchestration(self, config_name: str):
    """Load A2A orchestration"""
    return self.a2a_manager.load_orchestration(config_name)

def list_a2a_orchestrations(self):
    """List available A2A orchestrations"""
    return self.a2a_manager.list_orchestration_configs()

def get_a2a_network_status(self):
    """Get A2A network status"""
    return self.a2a_manager.get_network_status()
```

#### nexus/api/main.py

Add the A2A routes:

```python
from nexus.api.a2a_routes import router as a2a_router

# After creating the app
app.include_router(a2a_router)
```

#### nexus/streamlit_ui/main.py

Add the new pages:

```python
from nexus.streamlit_ui.a2a_orchestration_chat import a2a_orchestration_page
from nexus.streamlit_ui.a2a_network_monitor import a2a_network_page
from nexus.streamlit_ui.a2a_config_editor import a2a_config_page

# Add to pages dictionary
pages = {
    "Agent Chat": agent_chat_page,
    "Assistants": assistants_page,
    "A2A Orchestration": a2a_orchestration_page,
    "A2A Network": a2a_network_page,
    "A2A Config": a2a_config_page,
    # ... other pages
}
```

### 3. Install Dependencies

No additional dependencies required! The A2A system uses only Python standard library and existing Nexus dependencies.

---

## Quick Start

### 1. Create Your First Orchestration

Create `nexus/nexus_base/a2a_configs/simple_team.yaml`:

```yaml
orchestration:
  name: "simple_team"
  description: "A simple two-agent team"
  orchestrator: "coordinator"
  
  agents:
    - name: "coordinator"
      role: "orchestrator"
      profile: "adam"
      
    - name: "worker"
      role: "worker"
      profile: "adam"
  
  communication_patterns:
    - from: "coordinator"
      to: ["worker"]
      
    - from: "worker"
      to: ["coordinator"]
  
  workflows:
    - name: "simple_task"
      description: "Simple workflow"
      steps:
        - agent: "coordinator"
          action: "plan"
          output_to: ["worker"]
          
        - agent: "worker"
          action: "execute"
          output_to: ["coordinator"]
          
        - agent: "coordinator"
          action: "finalize"
```

### 2. Run from Python

```python
import asyncio
from nexus.nexus_base.nexus import Nexus

async def main():
    # Initialize Nexus
    nexus = Nexus()
    
    # Start A2A server
    await nexus.start_a2a_server()
    
    # Load orchestration
    orch = nexus.load_a2a_orchestration("simple_team")
    
    # Execute workflow
    results = await nexus.a2a_manager.execute_workflow(
        "simple_team",
        "simple_task",
        "Build a web scraper"
    )
    
    # Print results
    for agent, result in results.items():
        print(f"{agent}: {result}")

asyncio.run(main())
```

### 3. Run from Streamlit

```bash
nexus run
```

Navigate to **A2A Orchestration** page, select your orchestration, and start chatting!

---

## Core Components

### 1. Agent Card

Represents agent metadata and capabilities:

```python
from nexus.nexus_base.a2a.agent_card import AgentCard, AgentCapability

# Create agent card
card = AgentCard(
    name="research_agent",
    display_name="Research Agent",
    agent_type="worker",
    avatar="🔍",
    persona="I am a research specialist"
)

# Add capability
cap = AgentCapability(
    name="web_search",
    description="Search the web for information",
    input_schema={"type": "string"},
    output_schema={"type": "string"}
)
card.capabilities.append(cap)
```

### 2. Transport Layer

Handles message routing:

```python
from nexus.nexus_base.a2a.transport_layer import TransportLayer

transport = TransportLayer()

# Register agents
transport.register_agent("agent1")
transport.register_agent("agent2")

# Send message
await transport.send_message(
    from_agent="agent1",
    to_agent="agent2",
    message_type="request",
    payload="Hello"
)

# Receive message
message = await transport.receive_message("agent2")
```

### 3. Agent Registry

Service discovery:

```python
from nexus.nexus_base.a2a.agent_registry import AgentRegistry

registry = AgentRegistry()

# Register agent
registry.register(agent_card)

# Discover agents
workers = registry.list_agents(agent_type="worker", status="online")
orchestrators = registry.get_orchestrators()

# Find by capability
search_agents = registry.discover_by_capability("search")
```

### 4. Agent Executor

Executes agent tasks:

```python
from nexus.nexus_base.a2a.agent_executor import AgentExecutor

executor = AgentExecutor(agent, agent_card, transport)

# Execute task
result = await executor.execute_task("Analyze this data")

# Send to another agent
await executor.send_to_agent("other_agent", "task", "data")

# Broadcast
await executor.broadcast("Important announcement")
```

### 5. Orchestrator

Manages workflows:

```python
from nexus.nexus_base.a2a.a2a_orchestrator import A2AOrchestrator
from nexus.nexus_base.a2a.orchestration_config import OrchestrationConfig

config = OrchestrationConfig("path/to/config.yaml")
orchestrator = A2AOrchestrator(config, server)

# Execute workflow
results = await orchestrator.execute_workflow(
    "workflow_name",
    "initial input"
)

# Interactive mode
response = await orchestrator.run_interactive("user input")
```

---

## Configuration

### YAML Configuration Schema

```yaml
orchestration:
  name: string                    # Unique orchestration name
  description: string             # Description
  orchestrator: string            # Name of orchestrator agent
  
  agents:
    - name: string                # Agent identifier
      role: string                # "orchestrator" or "worker"
      profile: string             # Profile name (without .yaml)
  
  communication_patterns:
    - from: string                # Source agent
      to: [string]                # List of target agents
      mode: string                # "direct", "broadcast", "parallel"
  
  workflows:
    - name: string                # Workflow name
      description: string         # Description
      steps:
        - agent: string           # Agent to execute step
          action: string          # Action to perform
          output_to: [string]     # Agents to send output to
```

### Example: Complex Orchestration

```yaml
orchestration:
  name: "ai_development_team"
  description: "Full AI development lifecycle"
  orchestrator: "tech_lead"
  
  agents:
    - name: "tech_lead"
      role: "orchestrator"
      profile: "adam"
    
    - name: "requirements_analyst"
      role: "worker"
      profile: "adam"
    
    - name: "architect"
      role: "worker"
      profile: "adam"
    
    - name: "developer"
      role: "worker"
      profile: "adam"
    
    - name: "tester"
      role: "worker"
      profile: "adam"
    
    - name: "documenter"
      role: "worker"
      profile: "adam"
  
  communication_patterns:
    - from: "tech_lead"
      to: ["requirements_analyst", "architect", "developer", "tester", "documenter"]
      mode: "broadcast"
    
    - from: "requirements_analyst"
      to: ["tech_lead", "architect"]
      mode: "direct"
    
    - from: "architect"
      to: ["tech_lead", "developer"]
      mode: "direct"
    
    - from: "developer"
      to: ["tech_lead", "tester"]
      mode: "direct"
    
    - from: "tester"
      to: ["tech_lead", "developer"]
      mode: "direct"
    
    - from: "documenter"
      to: ["tech_lead"]
      mode: "response"
  
  workflows:
    - name: "full_development_cycle"
      description: "Complete software development lifecycle"
      steps:
        - agent: "tech_lead"
          action: "initiate_project"
          output_to: ["requirements_analyst"]
        
        - agent: "requirements_analyst"
          action: "gather_requirements"
          output_to: ["architect"]
        
        - agent: "architect"
          action: "design_system"
          output_to: ["developer"]
        
        - agent: "developer"
          action: "implement_code"
          output_to: ["tester"]
        
        - agent: "tester"
          action: "test_implementation"
          output_to: ["documenter", "tech_lead"]
        
        - agent: "documenter"
          action: "create_documentation"
          output_to: ["tech_lead"]
        
        - agent: "tech_lead"
          action: "review_and_deploy"
    
    - name: "hotfix"
      description: "Quick bug fix workflow"
      steps:
        - agent: "tech_lead"
          action: "identify_issue"
          output_to: ["developer"]
        
        - agent: "developer"
          action: "fix_bug"
          output_to: ["tester"]
        
        - agent: "tester"
          action: "verify_fix"
          output_to: ["tech_lead"]
        
        - agent: "tech_lead"
          action: "deploy_hotfix"
```

---

## Streamlit UI

### A2A Orchestration Chat Page

Features:
- Select and load orchestrations
- Choose between Interactive and Workflow modes
- Real-time agent collaboration
- View orchestration status
- Track participating agents

Usage:
1. Navigate to "A2A Orchestration" page
2. Select orchestration from sidebar
3. Click "Load Orchestration"
4. Choose execution mode
5. Start chatting or execute workflows

### A2A Network Monitor

Features:
- Real-time network status
- Agent status dashboard
- Message history
- Active orchestrations view

### A2A Configuration Editor

Features:
- Create new orchestrations
- Edit existing configs
- YAML validation
- Delete configurations

---

## API Reference

### REST API Endpoints

#### Start A2A Server
```http
POST /a2a/start
```

#### List Orchestrations
```http
GET /a2a/orchestrations
```

Response:
```json
{
  "orchestrations": ["research_team", "content_team"]
}
```

#### Load Orchestration
```http
POST /a2a/orchestrations/{name}/load
```

#### Chat with Orchestration
```http
POST /a2a/orchestrations/chat

{
  "orchestration_name": "research_team",
  "input_data": "Research AI trends"
}
```

#### Execute Workflow
```http
POST /a2a/orchestrations/workflow

{
  "orchestration_name": "research_team",
  "workflow_name": "research_and_report",
  "input_data": "AI trends 2024"
}
```

#### Network Status
```http
GET /a2a/network/status
```

Response:
```json
{
  "total_agents": 4,
  "online_agents": 4,
  "orchestrations": 1,
  "message_count": 15,
  "agents": [...]
}
```

#### Discover Agents
```http
POST /a2a/agents/discover

{
  "agent_type": "worker",
  "status": "online",
  "capability": "search"
}
```

#### Message History
```http
GET /a2a/messages/history?limit=100
```

---

## Examples

### Example 1: Simple Task Delegation

```python
import asyncio
from nexus.nexus_base.nexus import Nexus

async def task_delegation_example():
    nexus = Nexus()
    await nexus.start_a2a_server()
    
    # Initialize agents
    server = nexus.a2a_manager.server
    exec1 = server.initialize_agent("OpenAIAgent", "adam")
    exec2 = server.initialize_agent("OpenAIAgent", "adam")
    
    # Delegate task
    await exec1.send_to_agent(
        to_agent=exec2.agent_card.agent_id,
        message_type="request",
        payload="Analyze this data: [1,2,3,4,5]"
    )
    
    # Check result
    result = await server.transport.receive_message(
        exec2.agent_card.agent_id
    )
    print(f"Received: {result.payload}")

asyncio.run(task_delegation_example())
```

### Example 2: Multi-Agent Conversation

```python
from nexus.nexus_base.a2a.conversation_manager import ConversationManager

# Create conversation
conv_mgr = ConversationManager()
conv = conv_mgr.create_conversation("Project Discussion")

# Add turns
conv.add_turn("agent1", "We need to design the API")
conv.add_turn("agent2", "I suggest REST with JWT auth")
conv.add_turn("agent3", "Let's also add GraphQL support")

# Get context
context = conv.get_context()
print(context)
```

### Example 3: Dynamic Orchestration

```python
async def dynamic_orchestration():
    nexus = Nexus()
    await nexus.start_a2a_server()
    
    # Load orchestration
    orch = nexus.load_a2a_orchestration("research_team")
    
    # Execute with different inputs
    topics = [
        "Machine Learning",
        "Quantum Computing",
        "Blockchain"
    ]
    
    for topic in topics:
        print(f"\n=== Researching {topic} ===")
        results = await orch.execute_workflow(
            "quick_research",
            f"Quick research on {topic}"
        )
        
        for agent, result in results.items():
            print(f"{agent}: {result[:100]}...")

asyncio.run(dynamic_orchestration())
```

---

## Testing

### Run Tests

```bash
# Run all A2A tests
pytest tests/test_a2a.py -v

# Run specific test
pytest tests/test_a2a.py::TestAgentCard::test_create_agent_card -v

# Run with coverage
pytest tests/test_a2a.py --cov=nexus.nexus_base.a2a
```

### Manual Testing Checklist

- [ ] Create orchestration config
- [ ] Load orchestration via UI
- [ ] Execute workflow
- [ ] Test interactive mode
- [ ] Verify message routing
- [ ] Check network status
- [ ] Test agent discovery
- [ ] Verify error handling
- [ ] Test with multiple agents
- [ ] Check message history

---

## Troubleshooting

### Common Issues

#### 1. "Orchestration not found"

**Solution**: Ensure YAML file exists in `nexus/nexus_base/a2a_configs/`

```bash
ls nexus/nexus_base/a2a_configs/
```

#### 2. "Agent not registered"

**Solution**: Check agent initialization in orchestration config

```python
# Verify agent names match
orch.get_agents()  # Should list all agents
```

#### 3. "A2A Server not initialized"

**Solution**: Start server before operations

```python
await nexus.start_a2a_server()
```

#### 4. Messages not routing

**Solution**: Verify communication patterns in YAML

```yaml
communication_patterns:
  - from: "agent1"
    to: ["agent2"]  # Ensure agent2 is in 'to' list
```

#### 5. Profile not found

**Solution**: Use profile name without extension

```yaml
# Correct
profile: "adam"

# Incorrect
profile: "adam.yaml"
profile: "profiles/adam.yaml"
```

### Debug Mode

Enable detailed logging:

```python
import logging

logging.basicConfig(level=logging.DEBUG)
logger = logging.getLogger('nexus.a2a')
```

---

## Best Practices

### 1. Orchestration Design

✅ **DO:**
- Keep workflows focused and modular
- Use descriptive agent and workflow names
- Define clear communication patterns
- Document each workflow's purpose

❌ **DON'T:**
- Create circular dependencies
- Use too many agents (start with 3-5)
- Skip communication pattern definitions
- Hardcode agent-specific logic

### 2. Agent Roles

- **Orchestrator**: One per orchestration, coordinates workflow
- **Worker**: Specialized agents that perform specific tasks
- **Specialist**: Optional, for highly specialized operations

### 3. Performance

- Use parallel communication when possible
- Limit message payload size
- Cache orchestration configs
- Monitor agent queue sizes

### 4. Error Handling

Always handle potential failures:

```python
try:
    results = await nexus.a2a_manager.execute_workflow(
        "team", "workflow", "input"
    )
except FileNotFoundError:
    print("Orchestration not found")
except ValueError as e:
    print(f"Invalid configuration: {e}")
except Exception as e:
    print(f"Unexpected error: {e}")
```

---

## Advanced Features

### Custom Transport Protocols

Implement your own transport:

```python
from nexus.nexus_base.a2a.transport_layer import TransportProtocol

class RedisTransport(TransportProtocol):
    async def send(self, message):
        # Implement Redis-based messaging
        pass
    
    async def receive(self, agent_id, timeout=None):
        # Implement message receiving
        pass

# Add to transport layer
transport.add_protocol("redis", RedisTransport())
```

### Agent Middleware

Add preprocessing/postprocessing:

```python
class LoggingMiddleware:
    async def process(self, executor, message):
        print(f"Processing: {message.payload}")
        result = await executor.execute_task(message.payload)
        print(f"Result: {result}")
        return result
```

### Dynamic Agent Loading

Load agents at runtime:

```python
async def dynamic_agent_loading(nexus, agent_specs):
    for spec in agent_specs:
        executor = nexus.a2a_manager.server.initialize_agent(
            spec['name'],
            spec['profile']
        )
        print(f"Loaded: {spec['name']}")
```

---

## Conclusion

The Nexus A2A system provides a powerful, flexible framework for multi-agent collaboration. Key advantages:

- **Seamless Integration**: Works with existing Nexus agents
- **Flexible Architecture**: Support multiple communication patterns
- **Easy Configuration**: YAML-based orchestration definitions
- **Scalable**: Add agents and orchestrations as needed
- **Observable**: Real-time monitoring and debugging

For support and contributions, visit the [Nexus GitHub repository](https://github.com/cxbxmxcx/Nexus).

---

## License

MIT License - see LICENSE file for details
