# Batch Processing

Async processing of large request volumes at 50% cost reduction.

## Limits
| Limit | Value |
|---|---|
| Max requests per batch | 100,000 OR 256 MB |
| Processing time | Most within 1 hour; max 24 hours |
| Result availability | 29 days |

## Pricing (50% of Standard)
| Model | Batch Input | Batch Output |
|---|---|---|
| Claude Opus 4.8 | $2.50/MTok | $12.50/MTok |
| Claude Sonnet 4.6 | $1.50/MTok | $7.50/MTok |
| Claude Haiku 4.5 | $0.50/MTok | $2.50/MTok |

## Search Results

Natural citations for RAG with source attribution. All-or-nothing citation rule.

## Multilingual Support

Performance relative to English baseline:
- Spanish: 98%
- French: 97.9%
- Chinese: 97.1%
- Japanese: 96.9%
- Hindi: 96.8%
- Swahili: 89.8%
- Yoruba: 80.3%

## Embeddings

Anthropic does not offer its own embedding model. Voyage AI is recommended:
- voyage-4-large: Best general-purpose
- voyage-4: Balanced
- voyage-4-lite: Latency optimized
