# Refactoring SpecKit to add new skill agents

This repository contains some additional agents or speckit commands that have been added by refactoring the original speckit repo for GitHub Copilot only.
The idea was to see if we could use the SpecKit framework that was generic and extensible enough; and can be refactored to use some of the open source skills and plugins used by claude code to improve the development experience with GitHub Copilot


## Additional Skills integrated
As a part of this exercise here, I have added three new skills (speckit commands):

1. `/speckit.frontend_design`: For creating production-grade frontend interfaces with high design quality
2. `/speckit.pptx`: For creating pptx files on any given topic
3. `/speckit.mcp_builder`: For building mcp servers from scratch, test and document the entire operation
4. `/speckit.code_simplify`: For simplifying complex and overwritten code to minimize tech debts


## Development Experience: With vs Without Frontend Design Skills

The addition of the frontend design skill significantly enhances the development experience by enabling the creation of visually striking, production-ready interfaces with bold aesthetic choices. Below are two demonstrations showcasing the difference:

### Without Frontend Design Skills
Standard development approach without specialized design guidance:

![Demo without frontend design skills](./assets/website_without_skills.mp4)

### With Frontend Design Skills
Enhanced development using `/speckit.frontend_design` for production-grade aesthetics:

![Demo with frontend design skills](./assets/website_with_skills.mp4)

The frontend design skill enables:
- **Bold Aesthetic Direction**: Intentional design choices that create memorable interfaces
- **Production-Grade Quality**: Refined typography, cohesive color schemes, and sophisticated animations
- **Creative Implementation**: Unexpected layouts, spatial composition, and visual details that go beyond generic templates


> **Note:** This repository contains some of Anthropic's implementation of open source skills for Claude. For information about the Agent Skills standard, see [agentskills.io](http://agentskills.io). Additionally, this repository is baseline fork of the open source [spec-kit](https://github.com/github/spec-kit) based development framework

