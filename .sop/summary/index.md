# Knowledge Base Index - OpenHAB Solar EV Charging System

## Instructions for AI Assistants

This documentation provides comprehensive information about an OpenHAB 4 solar excess charging system for electric vehicles. Use this index to understand which files contain specific information types.

### How to Use This Documentation
1. **Start here** for overview and navigation guidance
2. **Consult specific files** based on the type of question or task
3. **Cross-reference** between files for complete understanding
4. **Use metadata tags** to quickly identify relevant sections

## Documentation Files Overview

### 📋 **codebase_info.md** - Basic Project Information
**Purpose:** High-level project overview and technical stack  
**Contains:** Technology stack, file structure, supported languages, dependencies  
**Use for:** Understanding project scope, technology choices, file organization  
**Metadata:** `#overview #tech-stack #dependencies`

### 🏗️ **architecture.md** - System Architecture & Design
**Purpose:** System design patterns and architectural decisions  
**Contains:** Component relationships, data flow, integration patterns  
**Use for:** Understanding how components interact, system design questions  
**Metadata:** `#architecture #design-patterns #integration`

### 🔧 **components.md** - Component Details & Responsibilities  
**Purpose:** Detailed breakdown of system components and their functions  
**Contains:** Rule definitions, item mappings, component responsibilities  
**Use for:** Understanding specific component behavior, troubleshooting  
**Metadata:** `#components #rules #items #functionality`

### 🔌 **interfaces.md** - APIs & Integration Points
**Purpose:** External interfaces and communication protocols  
**Contains:** Hardware bindings, communication channels, data exchange  
**Use for:** Integration questions, hardware setup, binding configuration  
**Metadata:** `#interfaces #bindings #hardware #communication`

### 📊 **data_models.md** - Data Structures & Models
**Purpose:** Data types, item definitions, and state management  
**Contains:** Item types, state values, data flow patterns  
**Use for:** Understanding data structures, item configuration, state management  
**Metadata:** `#data-models #items #states #types`

### ⚡ **workflows.md** - Key Processes & Logic Flows
**Purpose:** Business logic and operational workflows  
**Contains:** Charging algorithms, decision trees, control flows  
**Use for:** Understanding system behavior, logic troubleshooting, optimization  
**Metadata:** `#workflows #logic #algorithms #control-flow`

### 📦 **dependencies.md** - External Dependencies & Requirements
**Purpose:** Required bindings, hardware, and system prerequisites  
**Contains:** OpenHAB bindings, hardware requirements, setup dependencies  
**Use for:** Installation planning, troubleshooting missing dependencies  
**Metadata:** `#dependencies #requirements #bindings #hardware`

## Quick Reference Guide

### For Different Question Types:

**"How does the system work?"** → Start with `architecture.md`, then `workflows.md`

**"What does component X do?"** → Check `components.md`

**"How do I configure Y?"** → Look in `interfaces.md` and `dependencies.md`

**"What data types are used?"** → Refer to `data_models.md`

**"What are the requirements?"** → Check `dependencies.md` and `codebase_info.md`

**"How is the logic structured?"** → Review `workflows.md` and `components.md`

## Cross-References

- **Architecture ↔ Components:** System design relates to component implementation
- **Interfaces ↔ Dependencies:** Hardware interfaces require specific bindings
- **Workflows ↔ Data Models:** Logic flows operate on defined data structures
- **Components ↔ Data Models:** Components manipulate specific item types

## System Context
- **Domain:** Home automation and energy management
- **Scale:** Single-family residential solar + EV charging
- **Complexity:** Medium (2 main rules, 40+ items, multiple hardware integrations)
- **Maintenance:** Configuration-driven with minimal code changes needed

## Documentation Completeness
✅ Core functionality documented  
✅ Hardware integration covered  
✅ Configuration parameters identified  
⚠️ Advanced troubleshooting scenarios may need expansion  
⚠️ Performance tuning guidance could be enhanced
