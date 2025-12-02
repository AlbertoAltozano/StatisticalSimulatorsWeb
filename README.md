# DEIOAC Statistics Simulators Website

**Welcome to the home of the DEIOAC/STATIO statistics simulators website**.

This repository hosts the source code and development environment for the website that holds the interactive statistical simulators available to the public. It represents a modern, serverless approach to statistical education tools.

## 🏗 Architecture & CI/CD

This repository acts as the central hub for the website and CI/CD engine. Its primary purpose is to aggregate simulator modules, compile them, and deploy the full static website.

The Container (Quarto): The website structure, navigation, and layout are built using Quarto.

The Content (Shiny & WebAssembly): The simulators are developed in R using shiny. Instead of relying on a backend server, we utilize ShinyLive to compile these apps into WebAssembly. This allows the R code to execute entirely within the client's browser.

The Deployment: Pushing to this repository triggers web hosting service to automatically pull this repository, updating the web and enabling CI/CD.

------------------------------------------------------------------------

## 🧰 Prerequisites

To contribute to the website structure or integrate new simulators, ensure you have the following installed:

-   **Git:** https://docs.github.com/en/get-started/git-basics/set-up-git\
-   **Quarto:** https://quarto.org/docs/download/\
-   **R 4.5.x:** https://cran.r-project.org/bin/windows/base/
-   **R Tools \>=4.5:** https://cran.r-project.org/bin/windows/Rtools/
-   **RStudio** (Recommended IDE)

------------------------------------------------------------------------

## 🚀 Host this Project

To host a similar web-based project to the one shown here, users should:

0.  Fork this repository

1.  Open RStudio and go to **File → New Project → Version Control.**

2.  Select *Git*. <img src="https://github.com/user-attachments/assets/61d6cc43-e26d-4213-8407-234fba6056e3" alt="git_r" width="546" height="392"/>

3.  Copy the forked **Repository URL.**

4.  Choose your local directory and click Create Project.

### 📂 Repository Structure

· `StatisticalSimulators/`: **The Module Hub**. This directory contains the individual simulator projects. Each folder here represents a distinct simulation tool.

· `_site/`: The fully rendered static website (automated output).

· `index.qmd`: The homepage source configuration.

· `renv/:` The project-level R environment management.

------------------------------------------------------------------------

## 🧱 Adding a new Simulator to the Website

Simulators are essentially modular units within this website. To add a new one:

1.  **Scaffold the Module**

      1.1. Navigate to the `StatisticalSimulators/` directory.

      1.2 Duplicate the `template/` folder.

      1.3 Rename the folder (e.g., `ttest`).

2.  **Configure Metadata**

Open the `index.qmd` file inside your new simulator folder and update:

-   **Title, Description, Category, Image**.

-   **Iframe Path**: Update the source path from `/template/` to your new folder name (e.g., `/ttest/`).

### 🖥 Developing your new Simulator

Navigate to your module's source (e.g., `StatisticalSimulators/ttest/appr/`) and open app.R.

**Environment Setup**

Before editing the R code, ensure the environment is synced by running this in the R Console:

``` r
renv::restore()
renv::activate()

# Install compilation tools if missing
install.packages(c("shinylive", "S7", "munsell", "shiny"))
```

*Tip: You can develop standard Shiny code here. Use Run App in RStudio to test logic interactively.*

# 🌐 Compilating your Simulator and WebAssembly Export

To integrate the simulator into the static website, it must be compiled from R to WebAssembly using `shinylive`.

**Run in R Console:**

``` r
shinylive::export(
  "./StatisticalSimulators/your_module_name/appr",   # Source R Code
  "./StatisticalSimulators/your_module_name/appsite" # Compiled WebAssembly
)
```

*Example:*

``` r
shinylive::export("./StatisticalSimulators/template/appr", "./StatisticalSimulators/template/appsite")
```

Once exported, you can verify the standalone build:

``` r
httpuv::runStaticServer("./StatisticalSimulators/your_module_name/appsite")
```

## 🧱 Build the Full Website

After the simulator module is compiled, you must rebuild the Quarto website wrapper to include the new content.

### Run in Terminal::

``` bash
quarto render
```

------------------------------------------------------------------------

## ⬆️ Deployment & Git

We use a standard version control flow to manage the website and its content.

### 1. Update Local Repo:

``` bash
git pull
```

### 2. Stage Changes:

``` bash
git add .
```

### 3. Commit and Push:

``` bash
git commit -m "Integrated new simulator: T-Test"
git push
```

------------------------------------------------------------------------

## 📌 Command Summary Cheat Sheet

```         
# --- START SESSION ---
git pull

# --- COMPILE SIMULATOR (R Console) ---
shinylive::export("./StatisticalSimulators/my_sim/appr", "./StatisticalSimulators/my_sim/appsite")

# --- BUILD WEBSITE (Terminal) ---
quarto render

# --- DEPLOY (Terminal) ---
git add .
git commit -m "Update website content"
git push
```

## 🌍 Hosting

This repository is designed to be hosted on any static web hosting service (GitHub Pages, Netlify, Plesk, etc.).

Because the R logic is pre-compiled to WebAssembly, no specialized R server (like Shiny Server or RStudio Connect) is required. The entire site is just HTML, CSS, and JS, making it robust, fast, and easy to scale.
