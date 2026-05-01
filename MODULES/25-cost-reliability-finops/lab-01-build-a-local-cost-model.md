---
title: 'Build a Local Cost Model'
tags:
  - lab
  - finops
  - cost
module: "25"
---

# Lab 01 — Build a Local Cost Model (CSV + What-If)

## Goal

Build a small, explicit cost model you can use during reliability tradeoffs:

- represent costs as data (CSV)
- compute monthly cost from resource assumptions
- do a “what-if” change and quantify impact

## Prereqs

- `python3`

## Setup

```bash
mkdir -p ~/work/devops-labs/mod25-cost
cd ~/work/devops-labs/mod25-cost
mkdir -p dist
```

Create a simple cost input file:

```bash
cat > costs.csv <<'EOF'
component,units,cost_per_unit_month,notes
api_nodes,3,25.00,small instances
db_nodes,1,60.00,managed db equivalent
redis_nodes,1,20.00,cache
egress_gb,200,0.09,network egress
observability,1,15.00,logs/metrics budget
EOF
```

Create a cost calculator:

```bash
cat > cost_model.py <<'EOF'
import csv
import json
import sys

def main():
    path = sys.argv[1] if len(sys.argv) > 1 else "costs.csv"
    rows = []
    total = 0.0
    with open(path, newline="", encoding="utf-8") as f:
        r = csv.DictReader(f)
        for row in r:
            units = float(row["units"])
            c = float(row["cost_per_unit_month"])
            cost = units * c
            rows.append({"component": row["component"], "units": units, "cost_per_unit_month": c, "monthly_cost": cost})
            total += cost
    out = {"total_monthly_cost": round(total, 2), "items": rows}
    print(json.dumps(out, indent=2, sort_keys=True))

if __name__ == "__main__":
    main()
EOF
```

## Steps

### 1) Compute Baseline Monthly Cost

```bash
python3 cost_model.py costs.csv | tee dist/cost.json
```

Expected:

- output includes `total_monthly_cost`
- file `dist/cost.json` exists

### 2) Run a What-If: Add Capacity

Simulate scaling API nodes from 3 → 5:

```bash
python3 - <<'PY'
import csv
rows=[]
with open("costs.csv", newline="", encoding="utf-8") as f:
  r=csv.DictReader(f)
  for row in r:
    if row["component"]=="api_nodes":
      row["units"]="5"
    rows.append(row)
with open("costs.whatif.csv","w", newline="", encoding="utf-8") as f:
  w=csv.DictWriter(f, fieldnames=rows[0].keys())
  w.writeheader()
  w.writerows(rows)
print("wrote costs.whatif.csv")
PY
python3 cost_model.py costs.whatif.csv | tee dist/cost.whatif.json
```

Expected:

- what-if total cost increases

### 3) Quantify Delta

```bash
python3 - <<'PY'
import json
a=json.load(open("dist/cost.json"))
b=json.load(open("dist/cost.whatif.json"))
print("baseline", a["total_monthly_cost"])
print("whatif", b["total_monthly_cost"])
print("delta", round(b["total_monthly_cost"]-a["total_monthly_cost"],2))
PY
```

## Verify

```bash
python3 - <<'PY'
import json
a=json.load(open("dist/cost.json"))
b=json.load(open("dist/cost.whatif.json"))
assert b["total_monthly_cost"] > a["total_monthly_cost"]
print("ok: what-if increased cost")
PY
```

Expected signals:

- prints `ok: what-if increased cost`

## Cleanup

```bash
cd ~
rm -rf ~/work/devops-labs/mod25-cost
```

## Troubleshooting

### Symptom: cost model feels too abstract

Fix:

- add unit economics inputs (requests/month, storage GB, CPU hours)
- include SLO-driven capacity assumptions so cost ties back to reliability
