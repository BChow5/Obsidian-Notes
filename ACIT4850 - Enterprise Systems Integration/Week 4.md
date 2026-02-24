### Tool Evaluation
- may need tool evaluation when
	- dont know what tools are available for particular capability
	- have competing opinions
	- when you need to do your due diligence

#### process for tool evaluation and selection
- identify stakeholders
- gather requirements
- research and comparison
- prototype/proof of concept
- make a recommendation

**identify stakeholders**
- e.g
	- project manager
	- development manager/team lead
	- software dev team
	- IT team

**gather requirements**
- mandatory vs optional
- functional vs non functional 
![[Pasted image 20260127140105.png]]

**research and comparison**
- identify which tools are in market
- leverage dendor representative when possible
	- demos, etc.
- create a comparison table to show which products meet or do not meet the requirements 
![[Pasted image 20260127140212.png]]

**special considerations**
- pricing and cost
- cloud vs on prem
- build vs buy
- multiple products vs all in one
- familiarity
- team dynamics and politics 

#### tool eval best practices
- involve stakeholders
- know your constraints
	- budget
	- supported IT environment
	- regulations/standards
- open course are popular but enterprises often need extra features and support not in free versions
- do not just rely on research. talk to product vendor and do an on hand test 

### devops
- Development and Operations Engineers participating in the entire software lifecycle, from design through development and deployment to production support.  
- This is in contrast to Developers only participating in design and development and Operations only participating in deployment and production support. This creates a strong handover that often leads to friction and inefficiencies.  
- Other characteristics of DevOps include:  
	- Operations Staff making use of the same tools as developers for the systems work (i.e., source code management, infrastructure as code, automation)  
	- It aligns with agile values, principles, methods and practices.

#### continuous integration
- **Continuous Integration (CI)** – Every time code is changed in the source code repository (or at least daily) the code is checked out to a build machine where it is built and unit tests are run.  
- **Continuous Delivery (CD)** – The built software artefacts are automatically deployed to internal environments (dev/test) where further automated or manual tests are run.  
- **Continuous Deployment** – The built software artefacts are automatically deployed to production if the automated steps from the previous CI/CD steps all pass.

### CI Tool - Jenkins
![[Pasted image 20260127140818.png]]