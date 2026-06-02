# AI Usage Declaration

## Overview

This declaration describes the use of AI tools during the development of the MCL Orchestrator programming assignment. Claude Sonnet 4.6 (Anthropic) was used throughout the project via claude.ai as an implementation accelerator, generating boilerplate code, resolving library compatibility issues, and validating conceptual decisions. All architectural choices, domain-specific decisions, and engineering judgments were made independently. AI was used in the same way a senior engineer would use documentation or a technical reference, as a tool to accelerate execution and not as a substitute for understanding or design.

## AI Software Details

**Tool :** Claude Sonnet 4.6 (Anthropic) via claude.ai / ChatGPT GPT-4o (OpenAI) via chatgpt.com

**Code Generation and Boilerplate :** Claude Sonnet 4.6

**Conceptual Validation and Brainstorming :** Claude Sonnet 4.6

**Bug Solving and Debugging :** Claude Sonnet 4.6

**Plotting and Visualisation :** ChatGPT GPT-4o

## Contribution Breakdown

| Category | Approximate Contribution | Examples from this Project |
|---|---|---|
| AI generated code | 60% of total lines | FastAPI endpoint definitions, Pydantic schemas, uvicorn threading setup, logging configuration, pipeline state tracker |
| Co-developed with AI | 25% of total lines | scikit-fem FEM solver, error handling layers, retry logic, physical plausibility checks, analysis functions in Service C |
| Independently written | 15% of total lines | Physical parameter ranges, modulus tolerance thresholds, FEM problem geometry, pipeline test logic |
| Architectural decisions | 100% human | Service architecture, mapping to MCL workflow diagram, FEM problem selection, framework justification, boundary condition setup |
| Domain decisions | 100% human | SiC/SiC material parameters, Voigt rule of mixtures selection, physical plausibility criteria, analysis check tolerances |

AI tools accelerated the implementation of standard software patterns, freeing focus for the architectural and domain-specific decisions that define the project. All engineering judgments and materials science knowledge applied in this project were independent of AI output.

## Purpose Categories

**Code Generation :**
Claude was used to generate FastAPI endpoint definitions, Pydantic request and response schemas, uvicorn threading setup, pipeline state tracker functions, and structured logging configuration. These are standard software engineering patterns where AI generation significantly reduced implementation time without replacing engineering judgment.

**Brainstorming :**
Claude was used to explore service architecture options, compare REST framework choices, design the error handling layer structure, and think through the parametric sweep approach. In each case the AI provided options and the final decision was made independently based on project requirements and domain knowledge.

**Solving Bugs :**
Three significant debugging sessions were conducted with AI assistance. The first resolved a scikit-fem 12.x API change where to_simplex(), boundary_facets(), and linear_form were deprecated. The second resolved a compatibility conflict between nest-asyncio and uvicorn in Google Colab caused by an unexpected loop_factory keyword argument. The third resolved Pydantic V2 deprecation warnings for the dict() method. In each case the error message and context were provided and the AI diagnosed the cause and suggested a fix which was then tested and verified independently.

**Conceptual Validation :**
Claude was used to confirm the physical setup of the 2D plane stress FEM problem, validate the boundary condition choices (Dirichlet on the left edge, Neumann traction on the right edge), confirm the Voigt rule of mixtures as the appropriate homogenisation method for a SiC/SiC composite, and validate the modulus consistency tolerance threshold of 15% between FEM output and Voigt prediction.

## Selected Prompts

**Prompt 1 : Architecture and Concept Mapping**

Context : Early in the project when deciding how to structure the three services and map them to the MCL workflow diagram.

Prompt : "I am building a three-service event-driven pipeline that maps to the MCL workflow diagram. Service A generates material parameters, Service B runs a physics-based simulation, and Service C postprocesses the results. Each service must be completely independent and communicate only via REST POST requests. I want to use scikit-fem for the simulation in Service B. Suggest how to structure a 2D tensile coupon FEM problem as the simulation task, including what inputs Service A should pass, what Service B should compute, and what outputs Service C should receive."

Purpose : Brainstorming the FEM problem setup and REST API integration pattern between services, with the MCL workflow diagram as the architectural reference.

AI Role : Brainstorming

---

**Prompt 2 : Framework Justification (FastAPI over Flask)**

Context : The assignment explicitly required a non-Flask framework with written justification.

Prompt : "The assignment requires a non-Flask REST framework with justification. I have shortlisted FastAPI based on its Pydantic schema validation and async support, which seem appropriate for a scientific computing pipeline where input correctness is critical. Confirm whether FastAPI is the right choice for this use case, identify any drawbacks I should be aware of, and suggest how to justify this choice clearly in the project documentation."

Purpose : Confirming the FastAPI decision against an already-formed view and generating a technically grounded justification.

AI Role : Brainstorming

---

**Prompt 3 : Conceptual Confirmation Before Coding**

Context : Before writing any code for the three services, the full concept was mapped out to confirm alignment with the MCL requirements.

Prompt : "I did not want any code yet. I want to confirm the concepts first and then start with the code. The simulation involves a SiC/SiC composite tensile coupon. Provide the sequence of fixing the concepts for each workflow: what parameters Service A should generate and validate, what Service B should compute and extract from the FEM results, and what Service C should analyse and save. Make a comprehensive list for each service."

Purpose : Validating the full concept map across all three services before any implementation began, ensuring alignment with the MCL workflow diagram.

AI Role : Conceptual Validation

---

**Prompt 4 : Debugging scikit-fem Compatibility**

Context : Cell 5 of Notebook 02 failed with an AttributeError after scikit-fem 12.x removed the to_simplex() method.

Prompt : "I am getting AttributeError: MeshQuad1 object has no attribute to_simplex in scikit-fem 12.x. I believe this method was renamed or removed in a recent version update. What is the correct replacement for creating a triangular mesh from a quadrilateral grid in scikit-fem 12.x, and where else in the solver code does this same pattern appear that I should update at the same time?"

Purpose : Resolving a version compatibility issue in scikit-fem 12.x with an informed diagnosis already formed before asking.

AI Role : Bug Solving

---

**Prompt 5 : Debugging Uvicorn and nest-asyncio Conflict**

Context : Cell 8 of Notebook 02 failed when starting Service A with a TypeError related to loop_factory.

Prompt : "Starting uvicorn in a background thread in Google Colab fails with TypeError: _patch_asyncio locals run() got an unexpected keyword argument loop_factory. This appears to be a conflict between nest-asyncio patching Colab's event loop and a newer uvicorn version that passes loop_factory as a keyword argument. What is the safest fix that starts uvicorn correctly without interfering with Colab's existing event loop, and will this fix work consistently across all three services running simultaneously on different ports?"

Purpose : Resolving the asyncio conflict with a prior diagnosis already formed, and confirming the fix would generalise across all three services.

AI Role : Bug Solving

---

**Prompt 6 : Error Handling Architecture Design**

Context : After the MCL interview, where the team specifically asked how automated pipelines handle bad simulation outputs and service failures.

Prompt : "In the interview I was asked how I would handle errors or bad simulations in an automated pipeline. I want to design a robust error handling architecture across all three services. Focus particularly on how to handle physically implausible simulation outputs from the FEM solver and how to manage service unavailability when one service cannot reach the next one in the chain."

Purpose : Designing a comprehensive error handling strategy directly informed by a question raised in the MCL interview.

AI Role : Brainstorming

---

**Prompt 7 : Boundary Conditions Conceptual Validation**

Context : Before implementing the FEM solver in Service B, the boundary condition setup was confirmed to ensure physical correctness.

Prompt : "For a 2D plane stress linear elastic tensile coupon in scikit-fem, confirm the correct boundary conditions. The left edge simulates a fixed grip and the right edge simulates an applied tensile load. Should the left edge be fully fixed in both x and y directions or only in x? Should the traction on the right edge be applied as a point load or distributed uniformly across the full edge width? What is the physical consequence of getting either of these wrong in the FEM result?"

Purpose : Confirming the physical correctness of the boundary condition setup before scikit-fem implementation, with specific questions formed independently beforehand.

AI Role : Conceptual Validation

---

**Prompt 8 : Physical Plausibility Check Design**

Context : After the FEM solver was working correctly, designing the output validation layer before results are passed to Service C.

Prompt : "After the FEM solver runs I want to validate the outputs physically before passing them to Service C. I already know that axial displacement must be less than the coupon length and the effective modulus must be positive. For a linear elastic plane stress problem applied to a SiC/SiC ceramic composite, what additional physical consistency checks should I apply to the displacement field and computed modulus, and what tolerance thresholds are appropriate given the known discretisation error of a coarse triangular mesh?"

Purpose : Designing the output validation layer with prior thinking already established, asking the AI to extend and challenge that thinking.

AI Role : Conceptual Validation

## What Was Not AI Generated

The following decisions and content were made entirely independently without AI input:

- Overall service architecture and the decision to use three independent services
- Mapping of the pipeline stages to the MCL workflow diagram nodes
- Selection of the 2D plane stress tensile coupon as the FEM problem
- Physical parameter ranges for SiC/SiC input validation in Service A
- Choice of Voigt rule of mixtures as the homogenisation method
- Modulus consistency tolerance threshold of 15%
- Axial strain linear elastic limit of 10%
- Decision to sweep fiber volume fraction from 0.30 to 0.65 in the parametric study
- All debugging diagnosis and direction (AI was given the error, the cause and fix were directed by the engineer)
- Final review and verification of all AI generated code before use

## Key Statement

AI tools were used to accelerate implementation of standard software patterns, freeing time and focus for the architectural and domain-specific decisions that define this project. Every engineering judgment, materials science decision, and architectural choice was made independently. Claude Sonnet 4.6 was used as a tool in the same way a competent engineer uses documentation, Stack Overflow, or a technical reference, to execute faster and not to think differently.

*Varun Kamalapurkar : Augsburg, June 2026*
