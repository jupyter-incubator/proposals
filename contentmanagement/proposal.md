# Proposal for Incubation

## Subproject name

Jupyter Content Management Extensions (jupyter-incubator/contentmanagement)

## Development team and Advocate

Subproject development team:

* Justin Tyberg, IBM (`@jtyberg `)
* Dan Gisolfi, IBM (`@vinomaster`)
* Peter Parente, IBM (`@parente`)

Steering Council Advocate:

* Brian Granger (`@ellisonbg`)

## Subproject goals, scope and functionality

### Concept 
The Jupyter Notebook allows for the publishing, editing, organizing and deleting of content from a central web interface. Depending on an individual's workflow and collaboration requirements when working on notebook related projects, the collection of content management features they desire may differ from other users.

This incubation proposal pertains to the creation of a package of self-installable content management features that extend the baseline Jupyter Notebook. 

### Goals
* Establish a collection of content management extensions that can be installed to customize a user instance of a Jupyter Notebook.
* Allow 1..n content management extensions to be combined to customize an instance of a Jupyter Notebook.  

### Scope

Refactor content management features that were used to build the [IBM Knowledge Anyhow Workbench](https://knowledgeanyhow.org) technology preview, and potentially graduate them to a Jupyter subproject, or integrate them into the [Jupyter Interactive Notebook](jupyter/notebook) repo.

### Functionality

As outlined in this [blog post](http://blog.ibmjstart.net/2015/08/20/jupyter-notebooks-content-management-contributions/), the intent of this proposal is to provide support for:

* Including secondary notebooks by reference. 
* Table of contents and intra-notebook navigation.
* Adding a search button on any or all of the user views (notebook, file-tree, text editor).
* Downloading file bundles (.zips) from the Jupyter dashboard
* Importing any web-accessible notebook via an omnibox
* Sharing notebooks from one Jupyter instance to another via share links
* Uploading files easily from any Jupyter Notebook page

## Audience

Notebook power users who tend to require a broader range of organization, publication, and collaboration features.

## Other options

While each of these potential content management features could be handled as individual pull requests against the baseline Jupyter Notebook, not all features would be desirable for all users. This incubation proposal allows the community to (a) validate the applicability of individual features; (b) gather feedback on when/where various feature combinations are desirable; (c) graduate popular features to [Jupyter Interactive Notebook](jupyter/notebook).

## Integration with Project Jupyter
In accordance with the [New Subproject Process](https://github.com/jupyter/governance/blob/master/newsubprojects.md#incubation-of-subprojects), this incubation proposal pertains to existing code that a vendor is willing to contribute but it is not clear yet how the Subproject will integrate with the rest of Jupyter.

During the incubation process the content management extensions could be vetted by the community by establishing an experimental-notebook as a turnkey docker container in the [jupyter/docker-stacks repo](https://github.com/jupyter/docker-stacks) that combines the baseline [Jupyter Interactive Notebook](https://github.com/jupyter/notebook) with the incubation repo.

A successful incubation process could yield the justification for the proposed extensions to be directly integrated via pull requests into the [Jupyter Interactive Notebook](https://github.com/jupyter/notebook).
