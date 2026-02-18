# Creation of Model Evaluation / Qualification Reports directly on GitHub

## Preparation
* Fork this repository into your GitHub user account (if you have not done so yet).
* Enable Workflows on your forked repository

  <img width="600" alt="image" src="https://github.com/user-attachments/assets/e120663e-0916-4c79-b0eb-b16034601b9c" />

* [OPTIONAL] Synchronize the `main` branch of your fork with the parent OSP repository if required <br>(see [Syncing a fork](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/syncing-a-fork) in the GitHub documentation for details).

## Notes
* Do not use Excel to edit and save CSV files. Use a text editor only.

## How to create model evaluation reports

1. Create a new branch (e.g. `my-reports`) from the `main` branch of your fork.
1. Define the models by updating [`models.csv`](models.csv) (see the [Models](#models) section below for details)
   * Once you are finished, push the new branch to your fork and verify that the check [Validate models.csv](../../actions/workflows/check-models.yml) was successful!
1. [OPTIONAL] Adjust the OSP environment and tools by updating [`tools.csv`](tools.csv)<br>(see the [Tools](#tools) section below for details)
   * In case of modifications: verify that the check [Validate tools.csv](../../actions/workflows/check-tools.yml) was successful!
1. Go to the GitHub Action: [Create evaluation reports and projects](../../actions/workflows/create-evaluation_reports.yml)<br><br>
   * Click the __Run workflow__ button 
   * Select the branch defined in the first step (for instance, `my-reports`)
   * [OPTIONAL] Adjust the commit message that will appear later in the created pull request<br>(see the [What to do when reports are created](#next-step) section below).
   * Click the green __Run workflow__ button
     
        <img width="800" alt="Run workflow dialog showing branch selection and commit message options" src="https://github.com/user-attachments/assets/d787ddc6-db2a-47c0-ab98-82bcb0051bb4" />

## How to create qualification reports

1. Create a new branch (e.g. `my-reports`) from the `main` branch of your fork.
1. Define the qualifications by updating [`qualifications.csv`](qualifications.csv) (see the [Qualifications](#qualifications) section below for details)
   * Once you are finished, verify that the check [Validate qualifications.csv](../../actions/workflows/check-qualifications.yml) was successful!
1. [OPTIONAL] Adjust the OSP environment and tools by updating [`tools.csv`](tools.csv)<br>(see the [Tools](#tools) section below for details)
    * In case of modifications: verify that the check [Validate tools.csv](../../actions/workflows/check-tools.yml) was successful!
1. Go to the GitHub Action: [Qualification Reports](../../actions/workflows/create-qualification_reports.yml)
    * Click the __Run workflow__ button 
    * Select the branch defined in the first step (for instance, `my-reports`)
    * [OPTIONAL] Adjust the commit message that will appear later in the created pull request<br>(see the [What to do when reports are created](#next-step) section below).
    * Click the green __Run workflow__ button 

## What to do when reports are created<a id="next-step"></a>

When qualification reports are created, pull requests (PRs) are created (one PR for each qualification report) toward the branch defined in the first step (for instance, `my-reports`).<br>
These pull requests allow users to review report updates and adopt the new version.
For each created PR:
* Close and reopen the PR: this will trigger the automated checks (e.g. links and cross-references) of the created report.
* If you have a GitHub Copilot license: assign Copilot as a PR reviewer (Copilot will then check the report as well)

## Models

The [`models.csv`](models.csv) file indicates which models should be run and qualified.
The header includes the following fields:

- __Execute__: If `TRUE`, run the qualification. If `FALSE`, skip the qualification.
- __Repository name__: Name of the GitHub OSP repository from which to get the model, e.g. <br>`7E3-Model` for [https://github.com/Open-Systems-Pharmacology/7E3-Model](https://github.com/Open-Systems-Pharmacology/7E3-Model)
- __Released version__: Tag version of the model repository release, e.g. `1.0`. <br>Alternatively, use a **branch** name of the model repository (e.g. `main`).
- __Snapshot name__: Name of the snapshot (`.json`) file, e.g. <br>`7E3` in [https://github.com/Open-Systems-Pharmacology/7E3-Model/7E3.json](https://github.com/Open-Systems-Pharmacology/7E3-Model/7E3.json)
- __Folder name__: Name of the target folder where the model evaluation report (and the project file(s)) should be created.
- __Workflow name__: Path of the workflow R script that creates the function to run the qualification if not default.
> [!TIP]
> Leave the cell blank if the default path, `evaluation/workflow.R`, is used (this path is case insensitive). 
- __Additional projects__: URL(s) of additional project snapshots to export as `pksim5` projects, e.g. `Propofol-Pediatrics/refs/tags/v1.0/Propofol-Pediatrics.json`
> [!TIP]
> If multiple projects are exported, they need to be separated by a pipe character: `|`, e.g. `X-Pediatrics.json|Y-Pediatrics.json`

### Qualifications

The [`qualifications.csv`](qualifications.csv) file indicates which qualification plans should be processed.
The header includes the following fields:

- __Execute__: If `TRUE`, run the qualification. If `FALSE`, skip the qualification.
- __Repository name__: Name of GitHub OSP repository from which to get the qualification plan, e.g. `Qualification-CKD` for <br>[https://github.com/Open-Systems-Pharmacology/Qualification-CKD](https://github.com/Open-Systems-Pharmacology/Qualification-CKD)
- __Released version__: Tag version of the qualification plan repository release, e.g. `1.0`. <br>Alternatively, use a **branch** name of the qualification plan repository (e.g. `main`).
- __Workflow name__: Path of the workflow R script that creates the function to run the qualification if not default.
> [!TIP]
> Leave the cell blank if the default path, `Qualification/workflow.R`, is used (this path is case insensitive).
- __Folder name__: Name of the target folder where the qualification report should be created.
 
## Tools 

The [`tools.csv`](tools.csv) file indicates the software and versions to be installed in the environment before running model qualifications.
If a link is defined in the `URL` column, the installation uses the software from that link as is instead of resolving from the version.
Please ensure compatibility between their versions.
The available tools to be installed are listed below:

- [__ospsuite-R__](https://www.open-systems-pharmacology.org/OSPSuite-R/) : R package providing functionality for loading, manipulating, and simulating models created in the Open Systems Pharmacology Software tools PK-Sim and MoBi.
- [__Reporting Engine__](https://www.open-systems-pharmacology.org/OSPSuite.ReportingEngine/): R package providing a framework in R to design and create reports evaluating PBPK models developed in the Open Systems Pharmacology ecosystem.
- [__RUtils__](https://www.open-systems-pharmacology.org/OSPSuite.RUtils/): R package providing utility functions for Open Systems Pharmacology R packages
- [__TLF__](https://www.open-systems-pharmacology.org/TLF-Library/): R package providing an object-oriented framework to create tables and figures, which are used by R packages in the Open Systems Pharmacology ecosystem
- [DEPRECATED __rClr__](https://github.com/Open-Systems-Pharmacology/rClr): R package for accessing .NET. This package has been deprecated in favor of `{rSharp}`.
- [__rSharp__](https://www.open-systems-pharmacology.org/rSharp/): R package providing access to .NET libraries from R. It allows users to create .NET objects, access their fields, and call their methods
- [__Qualification Runner__](https://github.com/Open-Systems-Pharmacology/QualificationRunner): Qualification runner responsible for managing a qualification workflow
- [__Qualification Framework__](https://docs.open-systems-pharmacology.org/shared-tools-and-example-workflows/qualification): Enables automated validation of various scenarios (use-cases) supported by the OSP platform
- [__PK-Sim__](https://github.com/Open-Systems-Pharmacology/PK-Sim): Portable version of the comprehensive software tool for whole-body physiologically based pharmacokinetic modeling

## Code of conduct
Everyone interacting in the Open Systems Pharmacology community (codebases, issue trackers, chat rooms, mailing lists etc...) is expected to follow the Open Systems Pharmacology [code of conduct](https://github.com/Open-Systems-Pharmacology/Suite/blob/master/CODE_OF_CONDUCT.md).

## Contribution
We encourage contribution to the Open Systems Pharmacology community. Before getting started please read the [contribution guidelines](https://github.com/Open-Systems-Pharmacology/Suite/blob/master/CONTRIBUTING.md). If you are contributing code, please be familiar with the [coding standards](https://github.com/Open-Systems-Pharmacology/Suite/blob/master/CODING_STANDARDS.md).

## License
[GPLv2 License](https://github.com/Open-Systems-Pharmacology/Suite/blob/master/LICENSE).
