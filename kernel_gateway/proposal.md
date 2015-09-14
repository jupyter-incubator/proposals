# Proposal for Incubation

## Subproject name

Jupyter Kernel Gateway (`jupyter-incubator/kernel_gateway`)

## Development team and Advocate

* Peter Parente, IBM (`@parente`)
* Dan Gisolfi, IBM (`@vinomaster`)
* Justin Tyberg, IBM (`@jtyberg`)
* Gino Bustelo, IBM (`@ginobustelo`)

Who is the Steering Council Advocate for this Subproject?

* Kyle Kelley (`@rgbkrk`)

## Subproject goals, scope and functionality

### Problem

Applications that use Jupyter kernels as execution engines outside of the traditional notebook / console user experience have started appearing on the web (e.g., [pyxie](https://github.com/oreillymedia/pyxie-static), [pyxie kernel server](https://github.com/oreillymedia/ipython-kernel), [Thebe](https://github.com/oreillymedia/thebe), [gist exec](https://github.com/rgbkrk/gistexec), [notebooks-to-dashboards](http://blog.ibmjstart.net/2015/08/22/dynamic-dashboards-from-jupyter-notebooks/), ). Today, these projects rely on:

1. A _client_ (e.g., Thebe) that includes JavaScript code from Jupyter Notebook to request and communicate with kernels

2. A _spawner_ (e.g., tmpnb) that provisions gateway servers to handle client kernel requests

3. A _gateway_ (e.g., the entirety of Jupyter Notebook) that accepts [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) requests for kernels, isolates kernel workspaces (e.g., via Docker), and proxies web-friendly protocols (e.g., Websocket) to the kernel protocol (0mq)

Maturing the Jupyter stack so that these efforts (and future ones) can find robust, common ground will require improvements in each of the above areas. Work is already underway to define a JavaScript library for communication with kernels and kernel provisioners (i.e., [jupyter/jupyter-js-services](https://github.com/jupyter/jupyter-js-services)). Discussion has started about defining an API for provisioning notebook servers as well ([binder project/binder#8](https://github.com/binder-project/binder/issues/8)), a topic that touches on the spawner concept above. This proposal focuses on the third area, the concept of a standard kernel provisioning and communication API.

### Goals

The goal of this incubator project is to **prototype and evaluate** possible solutions that satisfy the rising, novel kernel uses above, particularly with regard to the driving use cases documented below. The design and code of this incubator may become new Jupyter components, or folded into other relevant efforts underway, or discarded if better options arise as the evaluation proceeds.  At present, we do not know the **correct** design and implementation, but we believe there is value in trying the following:

1. Using jupyter_client, jupyter_core, and pieces of jupyter/notebook (e.g., MappingKernelManager, etc.) to construct a headless kernel gateway that can talk to a cluster manager (e.g., Mesos).
2. Implementing a websocket to 0mq bridge that can be placed in any Docker container that already runs a kernel, to allow web-friendly access to that kernel.
3. Adding a new jupyter_client.WebsocketKernelManager that can be plugged into Jupyter Notebook or consumed by other tools to talk to kernels frontend by a websocket to 0mq bridge. (See use case #3 below).

### Use Cases

#### Use Case #1: Simple, Static Web Apps w/ Modifiable Code

Alice has a scientific computing blog. In her posts, Alice often includes snippets of code demonstrating concepts in languages like R and Python. She sometimes writes these snippets inline in Markdown code blocks. Other times, she embeds GitHub gists containing her code. To date, her readers can view these snippets on her blog, clone her gists to edit them, and copy/paste other code for their own purposes.

Having heard about Thebe and gist exec, Alice is interested in making her code snippets executable and editable by her readers. She adds a bit of JavaScript code on her blog to include the Thebe JS library, and turn her code blocks into edit areas with Run buttons.  She also configures Thebe to contact a publicly addressable Jupyter Kernel Gateway (hosted by the good graces of the community and Rackspace ;) as the execution environment for her code.

When Bob visits Alice's blog, his browser loads the markup and JS for her latest post. The JS contacts the configured kernel gateway to request a new kernel. The gateway provisions a kernel in its compute cluster and sends information about the new kernel instance back to the requesting in-browser JS client. Most importantly, this response contains information about a Websocket endpoint on the kernel gateway to which the client can establish a connection for communication with the kernel. Thereafter, the gateway acts as a Websocket-to-0mq proxy for communication between Bob's browser and the kernel until Bob leaves the page and the kernel eventually shuts down.

![](https://hackpad-attachments.s3.amazonaws.com/jupyter.hackpad.com_sZx2qqNHnY1_p.454990_1440709697802_undefined)

Note that this use case is not much different from the current Thebe and gist exec sample applications; it simply serves to formalize the APIs and components used for the additional use cases stated next.

#### Use Case #2: Notebooks Converted to Standalone Dashboard Applications

Cathy uses Jupyter Notebook in her role as a data scientist at ACME Corp. She writes notebooks to munge data, create models, evaluate models, visualize results, and generate reports for internal stakeholders. Sometimes, her work informs the creation of dashboard web applications that allow other users to explore and interact with her results. In these scenarios, Cathy is faced with the task of rewriting her notebook(s) in the form of a traditional web application. Cathy would love to avoid this costly rewrite step.

One day, Cathy deploys a Jupyter Kernel Gateway to the same compute cluster where she authors her Jupyter notebooks. The next time she needs to build a web app, she creates a new notebook that includes interactive widgets, uses Jupyter extensions to position the widgets in a dashboard-like layout, and transforms the notebook into a standalone NodeJS web app.  Cathy deploys this web app to ACME Corp's internal web hosting platform, and configures it with the URL and credentials for the kernel gateway on her compute cluster. 

Cathy sends the URL of her running dashboard to David, a colleague from the ACME marketing department. When David visits the URL, the application prompts for his intranet credentials. After login, his browser loads the markup and JS for the frontend of the dashboard web app. In contrast to the open-access blog post example in the prior use case, the JavaScript in David's browser does not contact the kernel gateway directly. It does not contain the credentials to do so, and it does not contain any code from the original notebook. Instead, to limit David's control over kernels on the compute cluster, the JS in David's browser only communicates with the dashboard app NodeJS backend. The dashboard server requests the kernel, sends code to the kernel for execution upon David's interaction with the frontend, and proxies the responses back to the frontend JS for display to David.  Throughout all this interaction, the kernel gateway behaves in the same manner as in the previous example: it provisions kernels and proxies Websocket-to-0mq connections for all dashboard users.

![](https://hackpad-attachments.s3.amazonaws.com/jupyter.hackpad.com_sZx2qqNHnY1_p.454990_1440679339192_undefined)

#### Use Case #3: Notebook Authoring Separated from Kernel/Compute Clusters

Erin is a Jupyter Notebook and Spark user. She would like to pay a third-party provider for a hosted compute plus data storage solution, and drive Spark jobs on it using her local Jupyter Notebook server as the authoring environment. When Erin decides to convert some of her notebooks to dashboards, she also wants those deployed dashboards to use her compute provider to avoid having to move her data around.

In a bright and not-so-distant future, Erin chooses a Spark provider that provides a Jupyter kernel service API. Her provider allows Erin to launch and communicate with Jupyter kernels via Websockets. The kernels run in containers (kernel-stacks) that are pre-configured with Spark and typical scientific computing libraries. Erin points her local Jupyter Notebook server to her provider's kernel service API. When Erin launches a new notebook locally, her Notebook server does the work of requesting a kernel from the kernel service API, and establishing a Websocket connection to the provider's kernel gateway, which proxies her commands to her running kernel via 0mq.

When Erin converts one of her notebooks to a dashboard, she supplies credentials for the dashboard server to access her kernel provider. When users visit Erin's dashboard, the dashboard server contacts the kernel provider to manage the lifecycle and communication with kernels. 

When Frank, a colleague of Erin, learns about her great setup, he asks to share her compute provider account and make it a team account. Happy to help, Erin does so. Frank then spins up a VM in his current cloud provider, runs Jupyter Notebook server on it, and points it to the kernel gateway running in Erin's hosted environment in the same manner Erin did with her local Jupyter instance.

![](https://hackpad-attachments.s3.amazonaws.com/jupyter.hackpad.com_sZx2qqNHnY1_p.454990_1440679897295_undefined)

**N.B.:** The key difference in this scenario versus what exists in Jupyter Notebook today lies in the fact that the Jupyter Notebook server is no longer talking to kernels via 0mq. Rather, the Notebook **server** is a Websocket client itself, much like the Notebook frontend, and communicates with kernels via a kernel gateway. This setup makes it possible to run the Notebook web application outside of the compute cluster, across the web if need be. Of course, it would require new remote kernel provisioning and Websocket client code paths within the Jupyter Notebook Python code to realize.

## Audience

* Jupyter Notebook users who want to run their notebook server remote from their kernel compute cluster
* Cloud providers who want to provide remote access to kernel compute services to clients other than Jupyter Notebook (e.g., dashboards)
* Application developers that want to create new tools and systems that leverage the use of kernels.

## Other options

Other than the proof of concepts mentioned at the start of this proposal (e.g., tmpnb used headlessly from Thebe / gistexec), there are no other clear options for enabling the use cases described above. Other up-and-coming projects (i.e., mybinder.org) may begin to improve upon these existing proof of concepts, but it is not, at the moment, designed specifically to address the use cases outlined in this proposal.

## Integration with Project Jupyter

This incubation effort should help Jupyter developers make informed decisions about future refactoring, reimplementation, and extension efforts with respect to kernel provisioning and access (e.g., jupyter-js-services). As mentioned above, if code assets produced from this exploratory incubation effort have merit, they should be promoted to full Jupyter projects and maintained as such (e.g., jupyter/kernel-gateway).
