# Repository Guidelines

## Project Structure & Module Organization
The repository is centered on [`src/Luban.sln`](/D:/code/project/luban/src/Luban.sln). The CLI entry point lives in `src/Luban`, shared pipeline and schema logic live in `src/Luban.Core`, and language- or format-specific exporters are split into sibling projects such as `src/Luban.CSharp`, `src/Luban.Protobuf`, and `src/Luban.Typescript`. Documentation and images live under `docs/`. Formatting helpers are in `scripts/`.

## Build, Test, and Development Commands
Run commands from the repository root unless noted.

- `dotnet restore src/Luban.sln` restores all .NET 8 projects.
- `dotnet build src/Luban.sln -c Release` builds the full solution.
- `dotnet run --project src/Luban -- --help` starts the CLI locally and lists supported options.
- `scripts/format.bat` on Windows or `sh scripts/format.sh` on Unix applies the repository formatting rules.

This repository does not currently contain a dedicated test project, so contributors should at minimum build the full solution and run targeted CLI verification for the feature or exporter they changed.

## Coding Style & Naming Conventions
Follow the root `.editorconfig`: spaces for indentation, final newline required, and a maximum line length of 180. C# braces go on new lines, single-line statements should be expanded, and braces are mandatory (`IDE0011` is treated as an error). Match existing naming patterns: `PascalCase` for types and public members, `camelCase` for locals and parameters, and project folders named `Luban.<Feature>`.

## Testing Guidelines
Because there is no standalone test suite, keep changes narrow and verify behavior close to the touched module. For parser, schema, or exporter changes, run the CLI against a representative config and confirm both success paths and expected error handling. When fixing a bug, document the reproduction command or sample input in the pull request.

## Commit & Pull Request Guidelines
Recent history uses short conventional messages such as `fix: ...` and `chore: ...`. Keep commit subjects imperative and scoped to one change. Pull requests should include a brief summary, impacted modules (for example `Luban.Core` or `Luban.Protobuf`), validation steps performed, and screenshots only when docs or generated output visuals changed.

## Security & Configuration Tips
Do not commit generated secrets, local machine paths, or ad hoc build artifacts. Avoid changing workflow or deployment files unless the task explicitly requires it.
