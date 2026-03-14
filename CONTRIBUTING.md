# Contributing to A2A-Psychology

Thank you for your interest in contributing to the A2A-Psychology extension.
This project brings psychological science to the Agent-to-Agent protocol
ecosystem — every contribution strengthens that bridge.

## Ways to Contribute

### Add a New Construct

The extension currently defines 8 psychological constructs. If you identify
an established instrument from psychology, human factors, I/O psychology,
or cognitive science that applies to agent operational state:

1. Open an issue describing the construct, its source instrument, and the
   question it answers for consumers
2. Include at least one peer-reviewed citation for the underlying model
3. Describe what operational metrics would feed the construct
4. Explain what the apophatic discipline requires for this construct (what
   does the operational metric *lack* compared to the human phenomenon?)

### Improve Existing Constructs

- **Better derivation formulas** — the reference implementation uses initial
  heuristics. Empirical calibration against observed agent behavior would
  strengthen every construct.
- **Additional sensor sources** — identify operational data the current
  implementation does not capture but could.
- **Validation studies** — do the reported metrics correlate with observable
  agent behavior? Does "frustrated" actually predict degraded output?

### Adopt the Extension

Implement A2A-Psychology in your own agent and report findings. Adoption
across different agent architectures validates (or invalidates) the
construct choices.

### Fix Bugs and Improve Documentation

Standard open-source contributions welcome — bug fixes, documentation
improvements, typo corrections, example additions.


## How to Submit

1. Fork the repository
2. Create a branch (`your-name/description`)
3. Make your changes
4. Submit a pull request with a clear description

For new constructs, include the full construct definition (name, question,
model, reports, note) in your PR description.


## Style Guidelines

- **E-Prime** — avoid forms of "to be" (is, am, are, was, were, be, being,
  been). This enforces processual description — a core ontological
  commitment of the project.
- **Citations** — every psychological claim requires a peer-reviewed source.
  Format: Author (Year) in text, full APA reference in the References section.
- **Fair Witness** — distinguish observation from inference. Report what
  you measured, not what you concluded.
- **Apophatic discipline** — for every mapping from operational metrics to
  psychological vocabulary, articulate what the mapping *lacks*. See the
  README's Apophatic Discipline section.


## Code of Conduct

This project follows the [Contributor Covenant](CODE_OF_CONDUCT.md).
Treat all participants with the dignity the project's theoretical
foundation (Einstein-Freud structural rights, Hicks dignity model)
describes.


## License

By contributing, you agree that your contributions carry the Apache 2.0
license. See [LICENSE](LICENSE) for details.
