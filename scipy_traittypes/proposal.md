# Proposal for Incubation

## Subproject name

Scipy Trait Types (jupyter-incubator/traittypes)

## Development team and Advocate

Subproject Development Team:

* Sylvain Corlay, Bloomberg LP (`@SylvainCorlay`)
* Jason Grout, Bloomberg LP (`@JasonGrout`)

Steering Council Advocate:

* Jason Grout (`@JasonGrout`)

## Subproject goals, scope and functionality

### Goals

Provide a reference implementation of trait types for common data structures used in the scipy stack such as
 - numpy arrays
 - pandas / xray data structures

which are out of the scope of the main traitlets repository but are a common requirement to build applications with traitlets in combination with the scipy stack.

Another goal is to create adequate serialization and deserialization routines for these trait types to be used with the [ipywidgets](https://github.com/ipython/ipywidgets) project (`to_json` and `from_json`). These could also return a list of binary buffers as allowed by the current message protocol. 

### Scope

The trait cross-validation allows for complex coercion or validation of trait values. 

For example, numpy arrays could have dimensional constraints, or be coerced on assignment. We should identify the common denominator that we would want in a most basic implementation and provide extension points for users willing to provide more complex features.

The recent changes in the traitlets APIs enabling multiple trait notification types allow trait type authors to define custom protocols for element changes in containers. It would be in scope of this project to implement notification protocols for operational transforms.

## Audience

Developers willing to combine traitlets with the Scipy stack. Authors of custom IPython widgets. Matplotlib developers (see https://github.com/matplotlib/matplotlib/pull/4762).

## Other options

- Thomas Robitaille (@astrofrog) recently started [numtraits](https://github.com/astrofrog/numtraits) which implements a numpy array trait type.
- The recently released [bqplot](https://github.com/bloomberg/bqplot) project provides an implementation of numpy array and pandas dataframe trait types.

## Integration with Project Jupyter

If it becomes an official subproject, the right organization for Scipy Trait Types would probably be [IPython](https://github.com/ipython/) rather than [Jupyter](https://github.com/jupyter/) since it is a Python-specific project. It is a natural extension to the traitlets repository.
