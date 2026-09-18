# Research and Assessment Report on LLM Relay Services (2026) 

**Language:** [中文](./README.md) | English

A black-box assessment of 100 LLM relay services: 19 detectors across five dimensions — core capabilities, site security, model consistency, response completeness, and service stability — forming 849 model–relay test combinations. Of these 849 combinations, 135 were effectively unavailable (across 45 sites), 296 showed at least one piece of evidence of model authenticity anomalies (across 75 sites), 110 showed hidden instruction injection, and 239 showed significant latency anomalies.

Online leaderboard: <https://ai.trusttools.cn/benchmark>

Full report: see "Report downloads" below — it contains the complete methodology and the scores of all 100 sites.

---

## Report downloads

| Document | Language | Version | Notes |
| --- | --- | --- | --- |
| [LLM-Relay-Services-Research-and-Assessment-Report-2026-ZH.pdf](./LLM-Relay-Services-Research-and-Assessment-Report-2026-ZH.pdf) | 中文 | 2026 | Full report (Chinese) |
| [LLM-Relay-Services-Research-and-Assessment-Report-2026-EN.pdf](./LLM-Relay-Services-Research-and-Assessment-Report-2026-EN.pdf) | English | 2026 | Full report (English) |

## Assessment overview

| Item | Details |
| --- | --- |
| Sites assessed | 100, split evenly between China-based and international operations, covering both enterprise and non-enterprise operators |
| Models assessed | 26 mainstream models from OpenAI, Anthropic, DeepSeek, Z.AI (Zhipu), Qwen, Gemini, MiMo, xAI and others |
| Test combinations | 849 model–relay combinations |
| Detector suite | 5 dimensions, 19 black-box detectors |
| Data collection | September 2026 |

## Detectors

Relay services are treated as black boxes: test inputs are constructed for specific objectives, and the observed requests, responses and service behaviour are compared against the behaviour expected of the target model or protocol specification. An anomaly is recorded only when the deviation is stable and reproducible and when several independent detectors agree. Detection strategies are orchestrated per model, so identical models are tested under identical configurations and methods, which keeps results comparable across sites.

| Dimension | Detector | Inspection items | Scope of application |
| --- | --- | --- | --- |
| Core capabilities | Protocol availability probing | Availability and connectivity testing for the three major protocols. | All models |
| | Available model check | Cross-reference the model list against the actual number of models callable via the API. | All models |
| Site security | TLS certificate inspection | Check certificate validity, domain matching, and certificate integrity. | All models |
| | Hidden instruction injection detection | Inspect the usage field in model responses for output significantly exceeding input volume. | All models |
| | Known poisoned relay detection | Cross-reference the site against historical poisoning records found in public threat intelligence. | All models |
| Model consistency | Random token behavior fingerprinting | Perform comparative verification of random model fingerprints to identify deviations from official fingerprints. | Models whose thinking mode can be manually disabled |
| | Bimodal latency detection | Analyze the bimodal characteristics of latency distributions to detect dynamic routing switches across multiple backends or vendors. | All models |
| | Model self-identification detection | Cross-verify the model's self-reported identity using identity probes against the response's model field. | All models |
| | JSON output capability detection | Verify that structured output parameters are correctly passed through and executed by the target model. | Models with native support for structured JSON output |
| | Tokenizer special token fingerprinting | Verify underlying tokenizer consistency by leveraging token behaviors unique to specific model families. | Qwen, DeepSeek, and GLM model series |
| | Response-model consistency check | Check for consistency between the model field in the response body and the requested model. | All models |
| | Structured tool call detector | Use forced tool-calling probes to verify that tool-related responses comply with Anthropic Messages conventions. | Claude series models |
| | Usage consistency check | Compare input token usage across various lengths against official baselines to identify differences and patterns. | Models with baseline references, such as Claude, DeepSeek, and GLM. |
| Response completeness | OpenAI response skeleton specification check | Validate the structural characteristics and mandatory fields of OpenAI-compatible API responses against protocol specifications. | Models compatible with the OpenAI API protocol |
| | Anthropic response skeleton specification check | Validate the structural characteristics and mandatory fields of Anthropic Messages responses against protocol specifications. | Models compatible with the Anthropic Messages API protocol |
| | Error response leakage detection | Verify the compliance of error responses and check for potential sensitive information leakage. | All models |
| | Context integrity check | Use embedded marker codes to verify the integrity of long-context transmission and detect compression or truncation. | All models |
| Service stability | Request latency stability check | Continuously monitor response speed and tail latency, using P95 latency as the key metric. | All models |
| | Request success rate check | Track request success and failure events over the observation period to measure service availability. | All models |

> [!TIP]
> The detectors and scopes listed above reflect the state at the time of this assessment. Detection rules may be revised over time, and scopes and methods may change accordingly; the latest version prevails. Models differ in invocation formats and parameter support, so each detector runs only within its stated scope.

## Overall ranking — top 10

| Rank | Site | Site security | Model consistency | Response completeness | Service stability | Core capabilities | Overall score | Rating |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | [sectoken](https://sectoken.cqcyht.com) | 100.0         | 96.0              | 100.0                 | 86.0              | 97.8              | 93.2          | S |
| 2 | [requesty](https://www.requesty.ai/) | 100.0 | 87.2 | 91.5 | 85.6 | 99.0 | 90.8 | S |
| 3 | [aisa](https://auth.aisa.one) | 97.5 | 89.2 | 100.0 | 86.5 | 94.5 | 90.4 | S |
| 4 | [aihubmix](https://aihubmix.com) | 100.0 | 93.8 | 99.2 | 92.2 | 98.3 | 90.2 | S |
| 5 | [302-ai](https://302.ai) | 99.1 | 87.5 | 99.1 | 77.4 | 98.4 | 89.8 | A |
| 6 | [ohmygpt](https://www.ohmygpt.com) | 100.0 | 94.1 | 100.0 | 84.3 | 93.3 | 89.4 | A |
| 7 | [canalapi](https://www.canalapi.com) | 100.0 | 98.0 | 100.0 | 70.0 | 70.0 | 88.6 | A |
| 8 | [dmxapi](https://dmxapi.com) | 100.0 | 92.4 | 96.7 | 79.2 | 92.0 | 88.4 | A |
| 9 | [opencode-go](https://opencode.ai) | 94.3 | 88.2 | 96.2 | 82.1 | 87.1 | 86.9 | A |
| 10 | [nextbit](https://www.nextbit256.com) | 100.0 | 96.8 | 93.3 | 87.5 | 100.0 | 86.7 | A |

## Scope and disclaimers

- All testing was carried out from the client side in a black-box manner. Only externally observable information — interfaces, request parameters, response content, token usage and error messages — could be inspected; internal routing strategies, upstream provider details and invocation logs were not accessible. Some findings are therefore inferences drawn from external behavioural characteristics.
- High-risk conclusions such as model substitution are always premised on cross-validation by several mutually independent detectors, in order to limit misjudgements caused by model version differences, server-side configuration or sampling randomness.
- Token usage and response latency are side-channel signals that may also be affected by protocol conversion, request preprocessing, upstream implementation, network paths and load. They are used to surface anomalous patterns and to support judgement, not as standalone proof.
- "No anomaly found" means only that no evidence sufficient to support that risk judgement was observed within the scope and conditions of this assessment; it does not confirm that the risk is absent. Scores and ratings are not a certification that any site is "absolutely safe" or "absolutely trustworthy".
- Results reflect service behaviour observed during the September 2026 data collection window. Sites may change as a result of model version updates, upstream provider changes, routing or operational strategy adjustments; sites using multi-upstream dynamic routing may show different characteristics for the same model at different times.
- This report is intended for technical research and as a reference for developers choosing a service. It is not a commercial recommendation or investment advice. Site names and trademarks belong to their respective owners.

## References

1. <https://www.alphaxiv.org/abs/2604.08407>
2. <https://www.tomshardware.com/tech-industry/artificial-intelligence/chinese-grey-market-sells-claude-api-access-at-90-percent-off-through-proxy-networks-that-harvest-user-data>
3. <https://arxiv.org/pdf/2607.10252>
4. <https://zenodo.org/records/21278557>
5. <https://platform.claude.com/docs/en/api/messages>
6. <https://developers.openai.com/api/reference/resources/responses/methods/create>
7. <https://developers.openai.com/api/reference/resources/chat>
8. <https://github.com/canarybyte/veridrop>
9. <https://github.com/Mohamed7415/fpverify>
10. <https://x.com/shoucccc/status/2098169782541631871>
11. <https://apito.ai/zh/blog/getting-started/how-to-verify-claude-api-authenticity-fingerprint-detection>
12. <https://transformers.run/c2/2021-12-11-transformers-note-2/>
13. <https://github.com/october-coder/api-check>
