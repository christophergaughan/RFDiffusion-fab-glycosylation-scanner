# AntibodyML Glycosylation Scanner v3.0

**Identifying N-glycosylation liabilities in AI-designed antibodies**

## Overview

Current AI-driven antibody design tools (RFdiffusion, ProteinMPNN, etc.) optimize for structure and sequence compatibility but have **no awareness of post-translational modification liabilities**. This scanner identifies N-linked glycosylation sequons (N-X-S/T) and progenitor sites that can compromise manufacturing in CHO cells.

## What's in This Repo

| File | Description |
|------|-------------|
| `AntibodyML_Glycosylation_Scanner_v3_Demo.ipynb` | Demo notebook scanning 15 RFantibody de novo designs |
| `Enhanced_Fab_Glycosylation_Scanner_v3_0.ipynb` | Full scanner with validation against FDA-approved therapeutics |

## Quick Start

```bash
# Open in Google Colab (recommended)
# Or run locally:
pip install antpack==0.3.8.6
jupyter notebook AntibodyML_Glycosylation_Scanner_v3_Demo.ipynb
```

## Key Features

### Detection
- **Active sequons** (N-X-S/T, X≠P): Direct glycosylation liabilities
- **Progenitor sites** (D-X-S/T, N-X-A, N-P-S/T): One mutation from becoming sequons

### Annotation
- IMGT numbering via AntPack
- Region classification (FR1/CDR1/FR2/CDR2/FR3/CDR3/FR4)
- Vernier zone flagging (IMGT 75-88)
- V-gene family risk assessment

### Risk Scoring
- Occupancy probability based on X-position efficiency (Shakin-Eshleman 1996)
- NXT vs NXS differentiation (~3x efficiency difference)
- Position-based risk stratification (CRITICAL/HIGH/MEDIUM/LOW)

## Demo Results

Scanning 15 de novo antibodies designed by RFantibody + ProteinMPNN:

| Metric | Result |
|--------|--------|
| Designs with active sequons | 7/15 (47%) |
| Designs with progenitor sites | 15/15 (100%) |
| Total liabilities detected | 48 |

**Neither RFdiffusion nor ProteinMPNN flagged any of these sites.**

## Limitations

⚠️ **Important caveats:**

1. **Scanner predicts LIABILITY, not OCCUPANCY.** Mass spectrometry required to confirm glycan presence.
2. **Scanner predicts RISK, not OUTCOME.** Functional impact requires experimental validation.
3. **Demo uses N=15 from single target/framework.** Results may not generalize.
4. **Progenitor sites ≠ manufacturing risk** for fixed therapeutic sequences.

## Scientific Basis

### Primary References

- **van de Bovenkamp FS, et al.** (2018) Adaptive antibody diversification through N-linked glycosylation of the immunoglobulin variable region. *PNAS* 115:1901-1906. [PMID: 29432145](https://pubmed.ncbi.nlm.nih.gov/29432145/)

- **Shakin-Eshleman SH, et al.** (1996) The amino acid at the X position of an Asn-X-Ser sequon is an important determinant of N-linked core-glycosylation efficiency. *JBC* 271:6363-6366. [PMID: 8626433](https://pubmed.ncbi.nlm.nih.gov/8626433/)

### Design Tools Evaluated

- **Bennett NR, et al.** (2025) Atomically accurate de novo design of antibodies with RFdiffusion. *Nature*. [DOI: 10.1038/s41586-025-08536-w](https://doi.org/10.1038/s41586-025-08536-w)

## The Gap We Fill

```
AI Design Pipeline                    Manufacturing Reality
─────────────────                    ─────────────────────
RFdiffusion      ───────────────────►  CHO Cell Expression
(backbone)            │                      │
     │                │                      │
     ▼                │                      ▼
ProteinMPNN     ◄─────┘               Glycoform Heterogeneity?
(sequence)                            Reduced Titer?
     │                                Binding Interference?
     ▼                                      │
  Output                                    │
     │                                      │
     └───────► AntibodyML Scanner ──────────┘
               Catches liabilities
               BEFORE they matter
```

## Acknowledgments

The RFantibody sequences used in this demo were generated using a Google Colab workflow adapted from **Andrew Smith's** excellent Docker-free RFantibody implementation:

- Blog post: [https://www.andrew-smith.me/blog/antibody/](https://www.andrew-smith.me/blog/antibody/)
- Colab notebook: [RFantibody Colab](https://colab.research.google.com/drive/1-zku6ZcDjK2p4rbyd8CS-ZU7ASeFS026)

Andrew's work made RFantibody accessible without requiring local Docker setup — a significant contribution to the community.

## License

MIT License. See LICENSE file.

## Contact

**AntibodyML Consulting LLC**  
*Bridging computational design and manufacturing reality*

---

*This tool is for research and educational purposes. Not intended for clinical decision-making.*
