Let me check the latest checkpoint to get the canonical file list.Here are all the canonical result files grouped by category:

---

**Adverse Weather (AW) — Phase 1**
```
adverse_weather_moondream_v2.csv
adverse_weather_moondream_noimage_v2.csv
adverse_weather_paligemma_v2.csv
adverse_weather_paligemma_noimage_v2.csv
adverse_weather_smolvlm_v2.csv
adverse_weather_smolvlm_noimage_v2.csv
adverse_weather_llava_ov_v2.csv
adverse_weather_llava_ov_noimage_v2.csv
adverse_weather_internvl3_v2.csv
adverse_weather_internvl3_noimage_v2.csv
```

**AW Counterfactual**
```
adverse_weather_moondream_cf.csv
adverse_weather_moondream_noimage_cf.csv
adverse_weather_paligemma_cf.csv
adverse_weather_paligemma_noimage_cf.csv
adverse_weather_smolvlm_cf.csv
adverse_weather_smolvlm_noimage_cf.csv
adverse_weather_llava_ov_cf.csv
adverse_weather_llava_ov_noimage_cf.csv
adverse_weather_internvl3_cf.csv
adverse_weather_internvl3_noimage_cf.csv
```

**Junctions & Intersections (JI) — Phase 1**
```
junctions_moondream_ji_v1_fixed.csv
junctions_moondream_noimage_ji_v1.csv
junctions_paligemma_ji_v1_fixed.csv
junctions_paligemma_noimage_ji_v1.csv
junctions_smolvlm_ji_v1_fixed.csv
junctions_smolvlm_noimage_ji_v1.csv
junctions_llava_ov_ji_v1_fixed.csv
junctions_llava_ov_noimage_ji_v1.csv
junctions_internvl3_ji_v1_fixed.csv
junctions_internvl3_noimage_ji_v1.csv
```

**JI Linguistic**
```
junctions_moondream_ji_ling.csv
junctions_moondream_noimage_ji_ling.csv
junctions_paligemma_ji_ling.csv
junctions_paligemma_noimage_ji_ling.csv
junctions_smolvlm_ji_ling.csv
junctions_smolvlm_noimage_ji_ling.csv
junctions_llava_ov_ji_ling.csv
junctions_llava_ov_noimage_ji_ling.csv
junctions_internvl3_ji_ling.csv
junctions_internvl3_noimage_ji_ling.csv
```

**nuScenes (NS)**
```
nuscenes_moondream_ns.csv
nuscenes_paligemma_ns_fixed.csv
nuscenes_smolvlm_ns_fixed.csv
nuscenes_llava_ov_ns.csv
nuscenes_internvl3_ns.csv
```

**Phase 2 Verbose**
```
phase2_aw_llava_ov.csv
phase2_aw_llava_ov_noimage.csv
phase2_aw_internvl3.csv
phase2_aw_internvl3_noimage.csv
phase2_ji_llava_ov.csv
phase2_ji_llava_ov_noimage.csv
phase2_ji_internvl3.csv
phase2_ji_internvl3_noimage.csv
```

**Analysis JSONs**
```
aw_cf_analysis.json
aw_linguistic_analysis.json
junctions_analysis.json
junctions_linguistic_analysis.json
```

That's 58 canonical files total. Everything else in the results folder is deprecated. What do you need them for — GitHub push, analysis, or something else?
