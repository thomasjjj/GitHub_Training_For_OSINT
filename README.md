# Welcome to the GitHub OSINT Analyst Training

This project is designed to be a **template and starting point** for anyone feeling overwhelmed by the idea of learning to code or starting to use GitHub. If you’re an OSINT analyst, researcher, or investigator who’s skilled at finding information but hasn’t done much (or any) coding, you’re in exactly the right place.

My main motivation for creating this is pretty simple: I want to get better at using GitHub (and coding in general) in a more **collaborative** way. Rather than quietly reading tutorials and tinkering on my own, I thought it would be more useful—and honestly, more fun—to take the FOSS (Free and Open Source Software) approach and make my entire learning process open to everyone. This way, not only do I learn, but others can follow along, contribute, and share their own lessons and discoveries.

## Course Contents

### 1. Orientation
- [1.1 What is GitHub?](1_orientation/1_1_what_is_github.md)
- [1.2 Why Should OSINT Professionals Code?](1_orientation/1_2_why_should_osint_professionals_code.md)
- [1.3 The End-to-End Workflow You'll Learn](1_orientation/1_3_the_end_to_end_workflow_you_will_learn.md)

### 2. Account & Local Tooling
- [2.1 Creating a GitHub Account](2_account_and_local_tooling/2_1_creating_a_github_account.md)
- [2.2 Installing Git Locally](2_account_and_local_tooling/2_2_installing_git_locally.md)
- [2.3 Generating and Adding an SSH Key](2_account_and_local_tooling/2_3_generating_and_adding_an_ssh_key.md)

### 3. Python Environment Setup
- [3.1 Installing Python 3.x](3_python_environment_setup/3_1_installing_python_3_x.md)
- [3.2 Creating & Activating Virtual Environments](3_python_environment_setup/3_2_creating_and_activating_virtual_environments.md)
- [3.3 Installing PyCharm Community Edition](3_python_environment_setup/3_3_installing_pycharm_community_edition.md)
- [3.4 Configuring PyCharm for Best Practice](3_python_environment_setup/3_4_configuring_pycharm_for_best_practice.md)

### 4. Your First Python Script
- [4.1 Cloning a Starter Repo](4_your_first_python_script/4_1_cloning_a_starter_repo.md)
- [4.2 Opening & Running the Script in PyCharm](4_your_first_python_script/4_2_opening_and_running_the_script_in_pycharm.md)
- [4.3 Basic Troubleshooting](4_your_first_python_script/4_3_basic_troubleshooting.md)

### 5. Fundamentals of Git
- [5.1 Repositories, Forks, and Local Clones](5_fundamentals_of_git/5_1_repositories_forks_and_local_clones.md)
- [5.2 Branches: Why & How](5_fundamentals_of_git/5_2_branches_why_and_how.md)
- [5.3 Making Commits in PyCharm](5_fundamentals_of_git/5_3_making_commits_in_pycharm.md)
- [5.4 Pushing to GitHub](5_fundamentals_of_git/5_4_pushing_to_github.md)
- [5.5 Opening Pull Requests (PRs)](5_fundamentals_of_git/5_5_opening_pull_requests.md)
- [5.6 Keeping Your Branch Up-to-Date](5_fundamentals_of_git/5_6_keeping_your_branch_up_to_date.md)

### 6. Issues & Communication
- [6.1 Creating an Issue That Developers Love](6_issues_and_communication/6_1_creating_an_issue_that_developers_love.md)
- [6.2 Using AI to Polish Issues](6_issues_and_communication/6_2_using_ai_to_polish_issues.md)
- [6.3 Commenting and Closing Issues Gracefully](6_issues_and_communication/6_3_commenting_and_closing_issues_gracefully.md)

### 7. Project Structure & Code Style
- [7.1 Recommended Layout (`src/`, `tests/`, `docs/`)](7_project_structure_and_code_style/7_1_recommended_layout.md)
- [7.2 `config.py` and Constants](7_project_structure_and_code_style/7_2_config_py_and_constants.md)
- [7.3 Environment Variables & `.env.example`](7_project_structure_and_code_style/7_3_environment_variables_and_env_example.md)
- [7.4 Creating a Solid `.gitignore`](7_project_structure_and_code_style/7_4_creating_a_solid_gitignore.md)
- [7.5 Code Style & Pre-Commit Hooks](7_project_structure_and_code_style/7_5_code_style_and_pre_commit_hooks.md)
- [7.6 Writing Minimal Tests with `pytest`](7_project_structure_and_code_style/7_6_writing_minimal_tests_with_pytest.md)
- [7.7 Continuous Integration with GitHub Actions](7_project_structure_and_code_style/7_7_continuous_integration_with_github_actions.md)

### 8. Collaboration Workflow
- [8.1 Setting Up the First Shared Project](8_collaboration_workflow/8_1_setting_up_the_first_shared_project.md)
- [8.2 Code Review Etiquette](8_collaboration_workflow/8_2_code_review_etiquette.md)
- [8.3 Using GitHub Projects & Discussions](8_collaboration_workflow/8_3_using_github_projects_and_discussions.md)
- [8.4 Handling Merge Conflicts Together](8_collaboration_workflow/8_4_merge-conflicts-doc.md)

### 9. Leveling-Up the Toolkit
- [9.1 Semantic Versioning & Tags](9_levelling_up_the_toolkit/9_1_semantic-versioning-tags.md)
- [9.2 Dependency Management with Poetry or pip-tools](9_levelling_up_the_toolkit/9_2_dependency-management.md)
- [9.3 Packaging & Publishing to PyPI](9_levelling_up_the_toolkit/9_3_packaging-pypi.md)
- [9.4 Automated Documentation with MkDocs](9_levelling_up_the_toolkit/9_4_mkdocs-documentation.md)

## What is this project?

This is an **evolving set of guidelines, checklists, and mini-lessons** specifically aimed at analysts and investigators who are already experts in their field, but might be new to coding or collaborative software development. It’s structured as a collection of Markdown (`.md`) files, so you can read through it section by section, at your own pace, and even suggest changes as you learn.

The first iteration of this guidance is **heavily reliant on auto-generated explanations and templates** (aka ```ai-slop```), so you’ll see clear, step-by-step instructions, many of which are generated by AI (with edits and commentary added as I or others test things out). The idea is to start with something that works and build up real, human advice, best practices, and tips over time.

If you spot something that’s unclear or could be improved, please open an issue or make a pull request. The only way this guide will get better is by being used and updated by real people, for real people.


## Why this, and why now?

I’ve found that most guides on GitHub, Python, and collaborative coding are written for software developers, not for OSINT analysts. The workflows, language, and assumptions can be alienating if you’re not from a coding background. This project is my attempt to bridge that gap—a kind of “translator” between the world of OSINT and the world of open-source coding.

Let this be an experiment in turning ```ai-slop``` into something human. If the tools you see here are a bit rough around the edges at first, that’s intentional - with community contributions, we can make a really nice collaborative resource. 

## What can you do here?

* **Follow the lessons**: Go step by step, or jump to the part you need.
* **Try things out**: Use the code and commands in your own projects.
* **Contribute your experience**: If you find something confusing or have a tip that worked for you, add it!
* **Open issues**: If you get stuck or spot a mistake, open an issue. This is a no-blame space—questions are welcome.
* **Suggest improvements**: If you’ve found a better way, or want to add a section, make a pull request or leave a comment.

The best thing? These are all GitHub processes, so you can test issues, contributions, discussions etc to *this* project with the safe knowledge that you aren't going to destroy working code if it all goes wrong. 

## Who is this for?

* Analysts, investigators, and researchers who want to start collaborating more technically, especially with Python and GitHub.
* Anyone who wants to go from “copy-paste code” to **confident contributor**.
* People who want a more approachable, peer-supported way to learn coding fundamentals relevant to OSINT.

---

**Let’s see what happens when we make the learning journey itself open source. Dive in, make mistakes, ask questions, and help each other out.**

---
