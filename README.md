# Assessing electricity market scenarios with the PyPSA ecosystem

This repository contains learning material for the model-based project assignment of the TU Delft course [SEN1522 - Electricity and Gas: Market Design and Policy Issues](). The material on this repository is largely based on the open-source energy system modelling framework [PyPSA](https://github.com/PyPSA/PyPSA) and on other tools within the [PyPSA ecosystem](https://pypsa.org/#frameworks).

The goal of the project assignment is to learn to **analyse the impact of electricity system changes on hourly prices, emissions, and other relevant metrics**. To do so, as a course participant, you are expected to:
- Develop an hourly dispatch optimisation model for a national electricity system of your choice with PyPSA.
- Use it to assess the impact of plausible policy(-driven) scenarios (e.g., introduction of a carbon tax, growth in the share of renewables).

## The repository in a nutshell

The repository includes:
- Files for installing the PyPSA-ecosystem environment via `pixi`
- A dummy file called `smoke_test_installation.ipynb` which you can run to test if your installation is functioning as expected (see 1.2)
- An illustrative `my_model` directory, which includes:
    - Pre-generated example input data for your national electricity system model
    - A tutorial on how to set-up such a model in PyPSA based on a reference historical year, then run it and analyse it (`tutorial_1_create_national_model.ipynb`)
    - A tutorial on how to introduce relevant policy(-driven) changes into the model (`tutorial_2_enrich_national_model.ipynb`)
    - A tutorial on how to self-generate model input data for a different country via the PyPSA ecosystem and other open-source resources (`tutorial_0_get_input_data.ipynb`)

## 1. Installation and quickstart

The only thing you need to use the PyPSA-ecosystem environment in this repository is an up-to-date `pixi` installation on your computer. You can verify if you have pixi on your computer by opening a new terminal and typing:
`pixi --version`. If you don’t have any `pixi` installation, you can install it following the instruction at: [https://pixi.prefix.dev/latest/installation/](https://pixi.prefix.dev/latest/installation/).

### 1.1 Opening a terminal pointing to the folder that contains the downloaded content
Once you have ensured you have `pixi`, all you need to do is download and extract the contents of this repository on your local machine. Then, Open a new terminal in the same folder. Or, alternatively, just launch a new generic terminal, and change the directory to point to the folder that contains the downloaded content. The command to change directory is `cd` (which stands for ‘change directory’), followed by the absolute or relative path to your directory. For example, to navigate to the Documents, you should do something like: `cd C:\Users\yourusername\Documents`. 

The exact path might change depending on how your computer is structured. Use the `cd` command to point to where the downloaded content is. Suppose it’s in a folder called SEN1522 within Documents, then you should do:
`cd C:\Users\yourusername\Documents\SEN1522`.

### 1.2 Using pixi to install and use the environment
Now, from the terminal, just run `pixi install`, and the environment will be installed. You can now type `pixi run jupyter lab`, which will open an interactive Jupyter Notebook and will give you the chance to open and play with the tutorials, as well as to create your own additional notebooks to perform new analyses.

You can also quickly verify that the installation works as expected by running, after opening Jupyter Lab as above, the notebook `smoke_test_installation.ipynb`. This is a dummy notebook with simple commands that should just work if the installation is successful.


## 2. Taking advantage of the tutorials

As mentioned eabove, there are three tutorials. 

### 2.1 How to create an electricity system dispatch model in PyPSA

The key tutorial to understand how to set up a model and run it is **tutorial #1**, where we build a model of the electricity system of the Netherlands. We do so from scratch, but based on pre-generated `input_data` (regarding electricity demand, weather-dependent renewables capacity factors, and techno-economic technology parameters) for the same country contained in this repository. To model a different country, you could simply gather similar input data from any source for your country of interest, and re-run the same tutorial based on such data.

The tutorial also offers examples of how to extract meaningful metrics from the model, such as electricity prices, the energy balance, and the cost recovery of generators.

### 2.2 How to introduce policy(-driven) changes in the system

In **tutorial #2**, we re-load the same model we created (and exported for later use) in tutorial #1. Then, we introduce relevant changes to it, such as: 1) introducing a carbon tax; or 2) simulating the growth of renewables and battery storage. For each case, we assess what its impact is on the same metrics we looked at in the reference case, such as prices, energy balance and cost recovery.

### 2.3 How to automatise the generation of input data

If you want to take further advantage of the possibilities offered by the PyPSA ecosystem, with tools such as **powerplantmatching** and **technology-data**, you can have a look at **tutorial 0**. There, we show how to automatise the search for existing power plants in a given (European) country, filtering by the desired year of decommissioning. And we show how to automatically download techno-economic parameters for all sorts of power plant and technology types. 


## Contributors and license

Unless otherwise indicated, the educational materials in this repository, including tutorial text and explanations, are licensed under the [Creative Commons Attribution 4.0 International License (CC BY 4.0)](/LICENSE.md). Contributors to the content developed exclusively for this repository are listed on the [Contributors](https://github.com/FLomb/electricity-market-scenarios-in-PyPSA/graphs/contributors?from=18%2F07%2F2026) page.

Third-party software, code, figures, datasets, and other materials retain their respective licenses and copyright notices. In particular:
- The PyPSA code is released as free software under an [MIT license](https://github.com/PyPSA/PyPSA/blob/master/LICENSES/MIT.txt), and it's the copyright of [PyPSA contributors](https://docs.pypsa.org/latest/contributing/contributors/).
- The powerplantmatching code is released as free software under an [MIT license](https://github.com/PyPSA/powerplantmatching/blob/master/LICENSES/MIT.txt), and it's the copyright of [powerplantmatching contributors](https://github.com/PyPSA/powerplantmatching/graphs/contributors).
- The technology-data code is released as free software under an [GPLv3 license](http://www.gnu.org/licenses/gpl-3.0.en.html), and it's the copyright of [technology-data contributors](https://github.com/PyPSA/technology-data/graphs/contributors).

Please retain the applicable attribution and license notices when reusing material from this repository.



