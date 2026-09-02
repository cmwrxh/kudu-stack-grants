# SENTINEL AI — TECHNICAL SPECIFICATION
## Edge-Based Species Identification for Biodiversity Funding

---

## 1. OVERVIEW

Sentinel AI is a computer vision pipeline that identifies endangered species from camera trap footage in real time, at the edge. It issues cryptographically signed "Proof of Life" certificates that unlock biodiversity funding.

---

## 2. MODEL ARCHITECTURE

### 2.1 Base Model

- **Architecture:** SpeciesNet 2026 (Google / iNaturalist)
- **Input:** 224x224 RGB image
- **Output:** Species classification + confidence score + bounding box
- **Base accuracy:** 92% on global dataset

### 2.2 Fine-Tuning

- **Dataset:** 50,000 images from Kenyan conservancies (elephant, rhino, lion, giraffe, leopard, buffalo, zebra)
- **Augmentation:** Rotation, brightness, occlusion (simulating dust, rain, night)
- **Fine-tuned accuracy:** 96.3% on East African megafauna

### 2.3 Edge Optimization

- **Quantization:** INT8 (4x smaller, 2x faster than FP32)
- **Hardware:** Google Coral TPU USB Accelerator ($100)
- **Inference time:** 45ms per image
- **Power draw:** 2.5W (solar-compatible)

---

## 3. PIPELINE

```
[Camera Trap] -> [Motion Detection] -> [Image Capture] -> [Preprocessing]
                                                              |
                                                    [Sentinel AI Inference]
                                                              |
                                                    [Species Classification]
                                                              |
                                                    [Confidence Threshold (>0.85)]
                                                              |
                                                    [Proof of Life Certificate]
                                                              |
                                                    [Fortress Ledger Write]
                                                              |
                                                    [Ranger Alert (if anomaly)]
```

---

## 4. PROOF OF LIFE CERTIFICATE

```json
{
  "certificate_id": "pol-ken-2026-001",
  "species": "Loxodonta africana",
  "common_name": "African Elephant",
  "confidence": 0.97,
  "location": {
    "lat": -1.406,
    "lon": 35.244,
    "accuracy_m": 10
  },
  "timestamp": "2026-09-02T09:15:00Z",
  "image_hash": "sha256:a1b2c3...",
  "node_id": "kudu-node-042",
  "signature": "ed25519:...",
  "ledger_index": 1847293
}
```

---

## 5. ANOMALY DETECTION

Sentinel AI also detects anomalies that trigger ranger alerts:

| Anomaly | Detection Method | Alert Priority |
|---|---|---|
| Human presence | SpeciesNet "human" class + geofence | CRITICAL |
| Vehicle (unauthorized) | Object detection + license plate OCR | HIGH |
| Fire / smoke | Color histogram analysis | CRITICAL |
| Injured animal | Gait analysis + posture deviation | HIGH |
| Species out of range | GPS + known range database | MEDIUM |

---

## 6. PRIVACY & ETHICS

- **No facial recognition:** We identify species, not individual humans
- **Data minimization:** Images deleted after 7 days; only hashes and certificates retained
- **Community consent:** Camera placement approved by conservancy committees
- **Ranger safety:** Alerts include estimated threat level, not just raw detections

---

## 7. PERFORMANCE BENCHMARKS

| Metric | Value |
|---|---|
| Species accuracy (top-1) | 96.3% |
| Species accuracy (top-5) | 99.1% |
| False positive rate | 2.1% |
| Inference latency | 45ms |
| Daily throughput per node | 50,000 images |
| Energy per 1,000 inferences | 0.07 Wh |

---

*Sentinel AI Spec v1.0 | September 2026 | Brilliant Unicorn LLC*
