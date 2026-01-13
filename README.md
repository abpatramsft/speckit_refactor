> **Note:** This repository contains some of Anthropic's implementation of open source skills for Claude. For information about the Agent Skills standard, see [agentskills.io](http://agentskills.io).
> **Note:** This repository is baseline fork of the open source [spec-kit](https://github.com/github/spec-kit) based development framework

# Refactoring SpecKit to add new skill agents

This repository contains some additional agents or speckit commands that have beena dded by refactoring the original speckit repo for GitHhub Copilot only.
The idea was to see if we could use the SpecKit framework that was generic and extensible enough; and can be refactored to use some of the open source skills and plugins used by claude code to improve the development experience with Github Copilot


## Additonal Skills integrated
As a part of this exercise here, I have added three new skills (speckit commands):

1. speckit.frontend_design: For creating production-grade frontend interfaces with high design quality
2. speckit.pptx: For creating pptx files on any given topic
3. speckit.mcp_builder: For building mcp servers from scratch, test and document the entire operation
4. speckit.code_simplify: For simiplifying complex and overwritten code to minimise tech debts
