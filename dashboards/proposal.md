# Proposal for Incubation

## Subproject name

Jupyter Dynamic Dashboards from Notebooks (jupyter-incubator/dashboards)

## Development team and Advocate

Subproject development team:

* Peter Parente, IBM (`@parente`)
* Gino Bustelo, IBM (`@ginobustelo`)
* Justin Tyberg, IBM (`@jtyberg `)
* Dan Gisolfi, IBM (`@vinomaster`)

Steering Council Advocate:

* Brian Granger (`@ellisonbg`)

## Subproject goals, scope and functionality

### Concept 

>What if the notebook author could quickly and easily publish rich interactive dashboard applications that allow users to perform interactive visual data exploration? 

Today, notebook authors can share their notebooks for others to view and run in the Jupyter Notebook web application. Authors can also transform their notebooks into a variety of static formats for ease-of-viewing in common tools like web browsers or PDF readers. However, if the notebook author desired to generate a dashboard application that would interact with the insights derived in the notebook, she would have to step outside the authoring environment to build a traditional web application. Such a task involves a great deal of copy/pasting and rewriting code that already exists in the notebook.

This incubation proposal pertains to a research sandbox associated with investigating ways of transforming notebooks directly into dashboards. The proposed repo would be based on a proof-of-concept extension for Jupyter Notebook that builds upon existing community work like [Thebe](https://github.com/oreillymedia/thebe) and the [recent refactoring of the Jupyter Notebook](http://blog.jupyter.org/2015/04/15/the-big-split/) in order to demonstrate the concept of producing dynamic dashboards from notebooks.

### Goals

* Provide within-notebook enhancements for laying out notebook cell outputs in a dashboard format, capturing layout metadata in the notebook document, and converting notebook documents into deployable web apps.
* Lay the foundation for non-trivial infrastructure topics associated with the deployment of dynamic dashboards (e.g., security, kernel execution model, scalability, and widget ecosystem). 

### Scope

The objective of this incubation project is to establish a baseline prototype that demonstrates how notebook authors can publish dashboards and how application users can access and interact with dashboards.

This incubation effort can be scoped as:

1. Extending the Jupyter Notebook user experience to enable drag/drop layout of cell outputs
2. Extending the Jupyter Notebook backend to enable the conversion of notebook documents to a new, web application format using nbconvert
3. Driving requirements for other components (e.g., [thebe](https://github.com/oreillymedia/thebe), [jupyter-js-services](https://github.com/jupyter/jupyter-js-services), [kernel gateway](https://github.com/jupyter-incubator/proposals/pull/3)) to enable robust, operation of deployed dashboards

### Functionality

As outlined in this [blog post](http://blog.ibmjstart.net/2015/08/22/dynamic-dashboards-from-jupyter-notebooks/), the intent of this proposal is to seed a repository with a proof of concept extension providing the following features:

* **Dashboard Layout** where the notebook author can drag/drop and resize notebook cells in a grid layout to dynamically create the web application within Jupyter.
* **Dashboard Transformation** where the notebook author can convert the notebook document into a web app that executes the same code as the origina notebook and retains the desired application layout. Packaging of the web app assets includes:
	* Static web resources necessary for operation of the dashboard frontend
	* Configuration for directing the frontend to an appropriate kernel execution environment (e.g., [tmpnb](https://github.com/jupyter/tmpnb), [kernel gateway](https://github.com/jupyter-incubator/proposals/pull/3) )
	* Assets to ease deployment of the dashboard frontend using common cloud technologies (e.g., Dockerfile, Cloud Foundry manifest)

After publishing the additional prototype, improvements will occur within the incubator project in order to mature the features for later integration into Jupyter Notebook or promotion as other mainline components.

## Audience

* Notebook users who wish to switch to a dashboard view for easier interaction with widgets in their notebooks (e.g., during data exploration)
* Notebook users who wish to layout and present their results in a dashboard view
* Notebook users who wish to share interactive dashboard views with other notebook users when passing around notebook documents
* Non-notebook users who want to interact with findings from analyses done in notebooks without running Jupyter Notebook themselves
* New Notebook users who wish to use it as a light-weight application composition environment

## Other options

* Any number of commercial and open source dashboarding applications exist (e.g., Tableau). Some have the ability to produce dashboards specifically from notebooks (e.g., Mathematica Cloud, Apache Zeppelin, Databricks).
* The traditional approach of doing analysis in notebooks and then stepping outside to create a traditional web application is always an option.
* One could use [thebe](https://github.com/oreillymedia/thebe) or an equivalent JS library to manually create a web application that uses Jupyter kernels for code execution. This project automates this otherwise manual effort. 

## Integration with Project Jupyter
In accordance with the [New Subproject Process](https://github.com/jupyter/governance/blob/master/newsubprojects.md#incubation-of-subprojects), this incubation proposal pertains to an idea that has yet to be vetted with the community. 

A successful incubation process should provide justification for the inclusion of the dashboard layout and conversion features into the baseline [Jupyter Interactive Notebook](https://github.com/jupyter/notebook). In addition, successful incubation should drive the creation of new Jupyter components that enable the robust execution of deployed notebook-dashboards, as well as other next generation Jupyter client applications.