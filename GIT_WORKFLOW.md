# Git workflow (owner: Ramya Sree Kasula, merging: Ramya & Denya)

Every member must commit **their own part from their own GitHub account**. Individual marks look at your commit history.

## 1. Create the repository (Ramya, once)
1. Repo: https://github.com/rk26abs/perfectpetclub-churn (Settings → set visibility to **Private**).
2. **Settings → Collaborators → Add people** → invite Bhavana, Siddu, Pravalika, Kavya and Denya.
3. Locally:
```bash
git clone https://github.com/rk26abs/perfectpetclub-churn.git
cd perfectpetclub-churn
# copy in: README.md, requirements.txt, .gitignore, GIT_WORKFLOW.md, data/
git add . && git commit -m "Initial repo structure, README, requirements, data" && git push
```

## 2. Branch per person
| Branch | Owner | Files |
|---|---|---|
| `feature/extract` | Bhavana | section 1 of `01_ETL_Pipeline.ipynb` |
| `feature/transform-quality` | Siddu | section 2 |
| `feature/transform-features-load` | Pravalika | sections 3–4 |
| `feature/dashboard` | Kavya | `02_Churn_Dashboard.ipynb` |
| `feature/tests-export` | Denya | section 5 + `run_and_export.py` |

```bash
git checkout main && git pull
git checkout -b feature/extract           # use your own branch name
# ... add your cells / files ...
git add 01_ETL_Pipeline.ipynb
git commit -m "Extract: add SQLite and CSV extract functions with existence checks"
git push -u origin feature/extract
```
Commit **little and often** with clear messages (one commit per function or fix is ideal, not one big commit at the end).

## 3. Merge order (Ramya & Denya)
Notebooks are JSON files, so two people editing the same notebook can conflict. To avoid this, merge **in pipeline order**,
and each person pulls `main` before adding their section:

`extract` → `transform-quality` → `transform-features-load` → `tests-export` → `dashboard`

On GitHub: **Pull requests → New** → base `main` ← your branch → ask one teammate to **review** → *Merge*.
If there is a conflict in a `.ipynb`: open both versions in Jupyter, copy your cells into the latest `main` version, then commit.
Tip: **Kernel → Restart & Clear Output** before committing keeps the diffs small.

## 4. Final submission (team leader: Bhavana)
After every branch is merged:
```bash
git checkout main && git pull
# add the final combined notebook (everyone's parts + outputs) and its HTML export
git add PerfectPetClub_Churn_Final.ipynb html/PerfectPetClub_Churn_Final.html
git commit -m "Final submission: combined ETL + dashboard notebook with outputs and HTML export"
git tag -a v1.0 -m "Final submission version" && git push && git push --tags
```
The **team leader** uploads `html/PerfectPetClub_Churn_Final.html` on Canvas.
