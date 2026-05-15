# ExSmell-Train

## Table of Contents

* __[Introduction][introduction]__
* __[Design-related smells][design-related-smells]__
  * [GenServer Envy][genserver-envy]
  * [Agent Obsession][agent-obsession]
  * [Unsupervised process][unsupervised-process]
  * [Large messages][large-messages]
  * [Unrelated multi-clause function][unrelated-multi-clause-function]
  * [Complex extractions in clauses][complex-extractions-in-clauses]
  * [Using exceptions for control-flow][using-exceptions-for-control-flow]
  * [Untested polymorphic behaviors][untested-polymorphic-behaviors]
  * [Code organization by process][code-organization-by-process]
  * [Large code generation by macros][large-code-generation-by-macros]
  * [Data manipulation by migration][data-manipulation-by-migration]
  * [Using App Configuration for libraries][using-app-configuration-for-libraries]
  * [Compile-time global configuration][compile-time-global-configuration]
  * ["Use" instead of "import"][use-instead-of-import]
* __[Low-level concerns smells][low-level-concerns-smells]__
  * [Working with invalid data][working-with-invalid-data]
  * [Complex branching][complex-branching]
  * [Complex else clauses in with][complex-else-clauses-in-with]
  * [Alternative return types][alternative-return-types]
  * [Accessing non-existent map/struct fields][accessing-non-existent-mapstruct-fields]
  * [Speculative Assumptions][speculative-assumptions]
  * [Modules with identical names][modules-with-identical-names]
  * [Unnecessary macros][unnecessary-macros]
  * [Dynamic atom creation][dynamic-atom-creation]
* __[Traditional code smells][traditional-smells]__
* __[About][about]__
* __[Acknowledgments][acknowledgments]__

## Introduction

This repository is a database of Elixir code smell examples organized into different categories, such as Design-related Smells, Traditional Code Smells, and Low-level Concerns. Each code smell contains 10 examples designed to simulate realistic Elixir code scenarios.

The dataset was generated using ChatGPT-5.4 and validated by two Elixir experts to ensure the quality and relevance of the examples.

The main goal of this repository is to provide a structured dataset that can be used as a knowledge base for Retrieval-Augmented Generation (RAG) systems. These examples can help feed a Large Language Model (LLM) with relevant code smell patterns, supporting the automatic detection and analysis of code smells in Elixir projects.

<!-- Links -->

[introduction]: #introduction

[design-related-smells]: https://github.com/labd2m/ExSmell-Train/tree/main/design_related
[genserver-envy]: https://github.com/labd2m/ExSmell-Train/tree/main/design_related/genserver_envy
[agent-obsession]: https://github.com/labd2m/ExSmell-Train/tree/main/design_related/agent_obsession
[unsupervised-process]: https://github.com/labd2m/ExSmell-Train/tree/main/design_related/unsupervised_process
[large-messages]: https://github.com/labd2m/ExSmell-Train/tree/main/design_related/large_messages
[unrelated-multi-clause-function]: https://github.com/labd2m/ExSmell-Train/tree/main/design_related/unrelated_multi_clause_function
[complex-extractions-in-clauses]: https://github.com/labd2m/ExSmell-Train/tree/main/design_related/complex_extractions_in_clauses
[using-exceptions-for-control-flow]: https://github.com/labd2m/ExSmell-Train/tree/main/design_related/using_exceptions_for_control_flow
[untested-polymorphic-behaviors]: https://github.com/labd2m/ExSmell-Train/tree/main/design_related/untested_polymorphic_behaviors
[code-organization-by-process]: https://github.com/labd2m/ExSmell-Train/tree/main/design_related/code_organization_by_process
[large-code-generation-by-macros]: https://github.com/labd2m/ExSmell-Train/tree/main/design_related/large_code_generation_by_macros
[data-manipulation-by-migration]: https://github.com/labd2m/ExSmell-Train/tree/main/design_related/data_manipulation_by_migration
[using-app-configuration-for-libraries]: https://github.com/labd2m/ExSmell-Train/tree/main/design_related/using_app_configuration_for_libraries
[compile-time-global-configuration]: https://github.com/labd2m/ExSmell-Train/tree/main/design_related/compile_time_global_configuration
[use-instead-of-import]: https://github.com/labd2m/ExSmell-Train/tree/main/design_related/use_instead_of_import

[low-level-concerns-smells]: https://github.com/labd2m/ExSmell-Train/tree/main/low_level_concerns
[working-with-invalid-data]: https://github.com/labd2m/ExSmell-Train/tree/main/low_level_concerns/working_with_invalid_data
[complex-branching]: https://github.com/labd2m/ExSmell-Train/tree/main/low_level_concerns/complex_branching
[complex-else-clauses-in-with]: https://github.com/labd2m/ExSmell-Train/tree/main/low_level_concerns/complex_else_clauses_in_with
[alternative-return-types]: https://github.com/labd2m/ExSmell-Train/tree/main/low_level_concerns/alternative_return_types
[accessing-non-existent-mapstruct-fields]: https://github.com/labd2m/ExSmell-Train/tree/main/low_level_concerns/accessing_non_existent_mapstruct_fields
[speculative-assumptions]: https://github.com/labd2m/ExSmell-Train/tree/main/low_level_concerns/speculative_assumptions
[modules-with-identical-names]: https://github.com/labd2m/ExSmell-Train/tree/main/low_level_concerns/modules_with_identical_names
[unnecessary-macros]: https://github.com/labd2m/ExSmell-Train/tree/main/low_level_concerns/unnecessary_macros
[dynamic-atom-creation]: https://github.com/labd2m/ExSmell-Train/tree/main/low_level_concerns/dynamic_atom_creation

[traditional-smells]: https://github.com/labd2m/ExSmell-Train/tree/main/traditional

[about]: #about
[acknowledgments]: #acknowledgments
