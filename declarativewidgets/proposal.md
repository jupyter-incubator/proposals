# Proposal for Incubation

## Subproject name

Jupyter Declarative Widget Extension (jupyter-incubator/declarativewidgets)

## Development team and Advocate

Subproject development team:

* Gino Bustelo, IBM (`@ginobustelo`)
* Peter Parente, IBM (`@parente`)
* Justin Tyberg, IBM (`@jtyberg `)
* Dan Gisolfi, IBM (`@vinomaster`)

Steering Council Advocate:

* Brian Granger (`@ellisonbg`)

## Subproject goals, scope and functionality

### Concept 

>What if the ecosystem around Project Jupyter included a pervasive set of rich notebook user interfaces (a.k.a. widgets) using declarative markup that can interact with other code authored in the notebook? 

A very powerful feature of Jupyter Notebooks is the ability to mix interactive widgets along with your code and data. These widgets turn the notebook into more of a web application, allowing the author to create a User Interface (UI) to the code entered in the Notebook’s cells. Rather than editing code and executing cells, the consumer of the Notebook can interact with the code by moving sliders, filling a form, pressing a button, etc.

This incubation proposal pertains to the creation of a Jupyter extension that would allow developers to create a compendium of language agnostic widgets that can support the Jupyter ecosystem of language backends. The extension would leverage web-components and Polymer in combination with the existing IPywidgets libraries.

### Goals
* Give the author access to a vast toolbox of components and a simple yet powerful authoring experience.
	* Enable quick and easy integration between the  ecosystem of Web Components (e.g., widgets in the Polymer catalog) and the Jupyter Interactive Notebook.
	* Provide Polymer based binding support to allow widgets to be aware of interactivity within the scope of the notebook namespace. 
* Minimize the effort to fully support interactive widgets on all Notebook supported languages. 
	* Instantiation of widgets within the notebook using HTML. 
	* Achieve compatibility and portability between the ecosystem of widgets and the Jupyter Interactive Notebook, regardless of the language used in the Notebook (i.e. Python, R, Scala, etc.).  

### Scope
The objective of this incubation project is to establish a baseline prototype that begins to blur the lines between a notebook and an application while bootstrapping the ecosystem of notebook compatible widgets. It will build on top of the foundation set by IPywidgets.

This incubation effort can be scoped as:

1. Identification of the minimum essential pieces of IPyWidgets that need to exist in all language kernels
2. Building a portable layer can leverage an existing and ever growing ecosystem of web widgets.
3. Providing a widget framework that works across the various and growing set of languages that can be hosted in a Jupyter Interactive Notebook. 

### Functionality

As outlined in this [blog post](http://blog.ibmjstart.net/2015/08/21/declarative-widget-system-for-jupyter-notebooks/), the intent of this proposal is to provide support for:

* A **function element** which will allow the running of a segment of code within the notebook as a side effect of a user's interactivity with UI elements. By using a function defined in a code cell, the author can connect other visual elements to set the function arguments, invoke it and display its result.
* A Pandas or a Spark DataFrame defined is a way to interact with data. Using a **dataframe element** the author can bind other visual elements to display the data held by a dataFrame. The dataframe element can be set to listen to changes to the DataFrame instance in the kernel and trigger notifications that change other elements that are bound to it.
* Extensions to the data binding capabilities of Polymer to allow multiple cell output areas to share data and related events. Support for changing and watching data using the language of the kernel.
* An element and related server extension to support importing new 3rd party elements into the Notebook. The import mechanism used bower to install new Polymer element upon user's request.
 
## Audience

1. Developers of rich interactive web widgets.
2. Notebook users who desire to inject rich interactive widgets into their notebooks.
3. Notebook users who wish to create interactive dashboards in their notebooks.

## Other options
The [IPyWidgets](https://github.com/ipython/ipywidgets) project is the reference implementation of interactive widgets for the Python language. At its core, it defines a communication channel and protocol between the browser and the Python kernel. This channel is used to exchange messages between a Javascript and a Python representation of a widget. It keeps the state of the two in sync and triggers events. A user clicks on a button in the browser, a message is sent to the Python representation of the button and it performs a preconfigured action. However, the IPyWidgets project is not architected to achieve the compatibility and portability goals associated Jupyter language kernels.

## Integration with Project Jupyter
In accordance with the [New Subproject Process](https://github.com/jupyter/governance/blob/master/newsubprojects.md#incubation-of-subprojects), this incubation proposal pertains to an idea that has yet to be vetted with the community. 

A successful incubation process could yield the justification for a new Jupyter Subproject for declarative widgets.
