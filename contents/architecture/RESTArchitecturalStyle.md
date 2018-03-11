# REST Architectural Style

Author: [Wen Xin](https://github.com/wenmogu)

This book chapter introduces the REST architectural style, which is the guideline of development for the Web we use today. The following content is a summary of selected parts from the famous [Roy Fielding's paper about REST](https://www.ics.uci.edu/~fielding/pubs/dissertation/fielding_dissertation.pdf) with my own understanding, which I hope can serve as a relatively brief, easy-to-understand and at the same time detailed introduction to REST.

**Requirement of Web development --- Background of REST architectural style**

The forefather of Web Sir Tim Berners-Lee envisioned the Web to be &quot;a shared information space through which people and machines could communicate.&quot; Fielding&#39;s understanding of &quot;this shared information space&quot; is that the Web is a place where everyone&#39;s information is structured and stored in a way such that their own information &quot;could be usable to themselves and others&quot;, and they can &quot;reference and structure&quot; others&#39; information such that they do not need to keep a local copy of it.

The design of the Web needs to consider the properties of its users and their information. The target audience of the Web are people in different organizations all over the world connected via the Internet. Their machines to access the Web might be very different, with different operating systems and requesting different file formats. These people&#39;s information also has a large variety in terms of the content type and the format. These restraints give rise to the following requirements of the Web:

- Low entry barrier: the Web should have a simple, uniform interface for everyone to use
- Extensibility: the Web service should be able to evolve to cater to the users&#39; evolving needs
- Distributed Hypermedia: Hypermedia is a medium for information like graphics, audio, video, plain text and hyperlinks. The Web should use hypermedia as the user interface &quot;because of its simplicity and generality&quot;. Hypermedia has &quot;the same interface&quot; which can be used &quot;regardless of the information source&quot;, and can be used to structure information flexibly. Moreover, the hyperlinks embedded can guide the reader through the information. However, since hypermedia contains information like graphics, audio etc., the amount of information to be transferred is huge. Hence, the Web should be able to transfer a large amount of information within acceptable time limits.
- Internet Scale:
  - Anarchic scalability: There is no central entity to control the whole system. Hence, firstly, the Web elements should be able to cope with unexpected load. As a result, it is not feasible for the clients to keep the knowledge about all the servers, and for the servers to keep track of the state of information at each of their clients. It is also costly to keep &quot;back-pointers&quot; inside the hypermedia data elements to refer back to the elements which reference them, as potentially there can be a huge number of references. Secondly, the Web elements should be able to ensure security against malformed or maliciously structured, which requires the Web to be able to &quot;communicate authentication data and authorization controls&quot;.
  - Independent Deployment: to cater to the needs of different users, the Web has to allow the co-existence of both old and new implementations of the Web elements and undergo &quot;gradual and fragmented change&quot;. Hence the design of the Web should be able to ease the deployment of the Web elements &quot;in a partial, iterative fashion, since it is not possible to force deployment in an orderly manner&quot;.

**REST architectural style**

Fielding defined architectural style as &quot;a coordinated set of architectural constraints that restricts the roles/features of architectural elements and the allowed relationships among those elements within any architecture that conforms to that style&quot;.

After surveying some common architectures for network-based applications on how the architectural constraints induce their corresponding architectural properties, Fielding came up with a new architectural style – Representational State Transfer, which satisfies the requirements of the Web development.

The table below describes the REST architectural constraints and the architectural properties these constraints induce. Detailed information about the architectural properties can be found in **Appendix**.

<table>
	<tr>
		<th>REST Constraint (Requirements satisfied)</th>
		<th>Explanation</th>
		<th>Properties induced/enhanced</th>
	</tr>
	<tr>
		<td>
			<p>Client-Server</p>
			<p>(Extensibility)</p>
		</td>
		<td>
			Separation of concerns: separate the user interface concern from the data storage concern  
		</td>
		<td>
			<ul>
				<li>
					Portability of user interface across platforms which might differ in their data storage structures
				</li>
				<li>
					Free the server from managing the user interface =&gt; simplified server component =&gt; enhanced scalability
				</li>
				<li>
					Allow the architectural components to evolve independently =&gt; enhanced modifiability 
				</li>
			</ul>
		</td>
	</tr>
	<tr>
		<td>
			<p>Stateless</p>
			<p>(Internet Scale)</p>
		</td>
		<td>
			Communications must be stateless: all the messages must be self-sufficient, containing all the necessary information to understand the message without referring to information outside the message
		</td>
		<td>
			<ul>
				<li>
					Other architectural components can monitor the interactions with the self-sufficient messages =&gt; enhanced visibility
				</li>
				<li>
					Free the server from managing different states of resources across requests =&gt; enhanced reliability and scalability
				</li>
			</ul>
		</td>
	</tr>
	<tr>
		<td>
			<p>Cache</p>
			<p>(Distributed Hypermedia)</p>
		</td>
		<td>
			Data within the response has to be indicated if it is cacheable. If cacheable, the cache will store the data and reuse if for identical requests later.
		</td>
		<td>
			<ul>
				<li>
					Eliminate some interactions between server and client =&gt; improved efficiency
				</li>
				<li>
					Reduce average latency of interactions i.e. faster response =&gt; improved user-perceived performance
				</li>
			</ul>
		</td>
	</tr>
	<tr>
		<td>
			<p>Layered System</p>
			<p>(Internet Scale)</p>
		</td>
		<td>
			Multiple layers between client and server: each architectural component in the system does not care what happens beyond the layers it has immediate interactions with
		</td>
		<td>
			<ul>
				<li>
					reduce complexity of the system =&gt; enhanced simplicity
				</li>
				<li>
					promote independence of the architectural components =&gt; enhanced modifiability
				</li>
				<li>
					allow for load-balancing intermediaries to distribute the workloads =&gt; enhanced scalability
				</li>
			</ul>
		</td>
	</tr>
	<tr>
		<td>
			<p>Uniform Interface</p>
			<p>(Extensibility, Internet Scale, Distributed Hypermedia)</p>
		</td>
		<td>
			A uniform interface between the architectural components, defined by 4 interface constraints
		</td>
		<td>
			<ul>
				<li>
					simplify the architecture
				</li>
				<li>
					interactions between the architectural components can be monitored as the set of interactions are predefined =&gt; increased visibility
				</li>
				<li>
					decouple service from the implementation (how the service is produced) =&gt; allow the architectural components to evolve independently, which increases modifiability
				</li>
			</ul>
		</td>
	</tr>
</table>

The 4 interface constraints defining the Uniform Interface architectural constraint for REST are elaborated in the table below:

<table>
	<tr>
		<th> Interface Constraint </th>
		<th style="width:40%"> Definitions </th>
		<th> Explanation </th>
	</tr>
	<tr>
		<td>
			Identification of resources
		</td>
		<td>
			<p>Resource: &quot;any information that can be named can be a resource&quot;. The state/value of the resource can vary at different points of time, but the &quot;semantic of mapping&quot; (i.e. the mapping of the resource to its identifier) must remain unchanged. </p>
			<p>For example, the student list of CS3281 can be named as &quot;CS3281StudentList&quot;, and hence can be a resource. The student list might change over time, but the identifier &quot;CS3281StudentList&quot; always refers to the student list regardless of its state/value. </p>
		</td>
		<td rowspan="2">
			Benefits:
			<ul> 
				<li> 
					provides generality by including many sources of information without needing to distinguish their types or implementation 
				</li> 
				<li> 
					free the server from managing different states of resources across requests 
				</li> 
				<li> 
					different <a href="https://en.wikipedia.org/wiki/Media_type#Common_examples">data formats</a> of the representation are available 
				</li> 
				<li> 
					allows <a href="https://en.wikipedia.org/wiki/Late_binding">dynamic binding</a> of the reference to a representation of the resource, hence allowing the server to serve the representation as specified in the request 
				</li> 
				<li> 
					reference the resource instead of its representation saves the trouble of changing the identifier (i.e. the name) when the representation of the resource changes 
				</li> 
			</ul>
		</td>
	</tr>
	<tr>
		<td>
			Manipulation of resources through representations
		</td>
		<td>
			<p>Representation of resource: &quot;a sequence of bytes, plus the representation metadata to describe those bytes&quot;. The representation of the resource captures the state of the resource, and is transferred between the architectural components. </p>
			<p>For example, the client requests for a HTML version of &quot;CS3281StudentList&quot; at the start of the semester, the representation of the student list, which includes a string of names and the metadata telling the client that the message is a string of names to be formatted in HTML, will be sent back to the client in the response. </p>
		</td>
	</tr>
	<tr>
		<td>
			Self-descriptive messages
		</td>
		<td>
		</td>
		<td>
			To support the intermediate processing of interactions. This is related to Stateless and Layered System architectural constraints.
		</td>
	</tr>
	<tr>
		<td>
			Hypermedia as the engine of application state (HATEOAS)
		</td>
		<td>
		</td>
		<td>
			The client can use the links provided by the server in the response to dynamically navigate to related resources. Benefits:
			<ul> 
				<li> 
					prevent hardcoding of links 
				</li> 
				<li> 
					the server does not drive the change. Hence, the server does not need to store the state of resources across different requests 
				</li> 
			</ul>
		</td>
	</tr>
</table>

**Appendix: Summary of Part of Chapter 2**

Architectural properties of key interest in general:

1. **Performance**: network performance(throughput, bandwidth), user-perceived performance (latency and completion), network efficiency(the most efficient architectural styles for a network-based application are those that can effectively minimize use of the network when it is possible to do so, through reuse of prior interactions (caching), reduction of the frequency of network interactions in relation to user actions (replicated data and disconnected operation), or by removing the need for some interactions by moving the processing of data closer to the source of the data (mobile code))
2. **Scalability**: Scalability refers to the ability of the architecture to support large numbers of components, or interactions among components, within an active configuration. Scalability can be improved by simplifying components, by distributing services across many components (decentralizing the interactions), and by controlling interactions and configurations as a result of monitoring. Scalability is also impacted by the frequency of interactions, whether the load on a component is distributed evenly over time or occurs in peaks, whether an interaction requires guaranteed delivery or a best-effort, whether a request involves synchronous or asynchronous handling, and whether the environment is controlled or anarchic (i.e., can you trust the other components?)
3. **Simplicity**: If functionality can be allocated such that the individual components are substantially less complex, then they will be easier to understand and implement.
4. **Modifiability**: Modifiability is about the ease with which a change can be made to an application architecture. Modifiability can be further broken down into **evolvability**, **extensibility**, **customizability**, **configurability**, and **reusability**. **Evolvability** represents the degree to which a component implementation can be changed without negatively impacting other components. **Extensibility** is defined as the ability to add functionality to a system. **Customizability** refers to the ability to temporarily specialize the behavior of an architectural element, such that it can then perform an unusual service. Customizability is a property induced by the remote evaluation and code-on-demand styles. **Configurability** is related to both **extensibility** and **reusability** in that it refers to post-deployment modification of components, or configurations of components, such that they are capable of using a new service or data element type. 
5. **Visibility**: Visibility in this case refers to the ability of a component to monitor or mediate the interaction between two other components. Visibility can enable improved performance via shared caching of interactions, scalability through layered services, reliability through reflective monitoring, and security by allowing the interactions to be inspected by mediators (e.g., network firewalls).
6. **Portability**: Software is portable if it can run in different environments.
7. **Reliability**: the degree to which an architecture is susceptible to failure at the system level in the presence of partial failures within components, connectors, or data.