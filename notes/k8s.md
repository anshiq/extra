K8s  

K8s is built with 4 components:  
1. API Server  
2. Controller Manager  
3. Kube Scheduler  
4. etcd  

All these components are present on the head node.  

etcd is used to manage the state of the K8s cluster. The etcd stores all cluster information, such as its current state, desired state, configuration of resources, and runtime data. etcd is a highly consistent key-value data store. Data is stored in the form of hierarchical directories forming a standard file system. In practice, the keys likely refer to the resources, and the values refer to the state. The changes in the resources and their state are reflected in etcd, which facilitates operations such as scheduling tasks.  

etcd is consistent due to the Raft algorithm. According to this, if there are several nodes in the system, a leader node is selected through a vote. To elect a leader node, a majority of the nodes must vote for the specific node as the leader. The leader acts as the single source of truth. Whenever a read or write request is made to any node in a distributed system, the request is forwarded to the leader. The leader, which holds all the key-value pairs, will handle the request, make the changes in the key-value pair, and inform the follower nodes. In the event that a leader node fails or faces issues, a new leader is elected by the same algorithm. As long as the majority of the nodes in the system are functional, a new leader can be elected, and requests can be handled.  

The API Server interacts with etcd (it is like the database of K8s) and writes any occurring changes into it. The Controller Manager and Kube Scheduler also request changes from the API Server, which interacts with etcd.  

### Kube Controller Manager:  

A controller generally monitors and tracks the functioning of one or more Kubernetes resources. It specifically looks at the desired state mentioned in the spec variable and, with the help of the Kube API Server, works to enforce the desired state. Depending on the type of resource the Kube Controller Manager monitors, the type of controller manager also changes.  

A few examples include:  

- **Node Controller:** Tracks node status, onboards new nodes, and determines whether a node is responsive or not. Based on this, it keeps the pods assigned to a specific node or reassigns them to a different, healthy node.  
- **Job Controller:** Waits for new jobs or one-time tasks to be created, and once they are, the Job Controller sends them to newly created pods for completion.  
- **EndpointSlice Controller:** EndpointSlice is a resource that represents a group of network endpoints, typically belonging to the same service. The EndpointSlice Controller is responsible for creating and managing EndpointSlice resources in the cluster.  
- **ServiceAccount Controller:** Creates default ServiceAccounts for new namespaces, as well as reconciling them with the actual state of the cluster.  

Other controllers include:  

- **Node Controller:** Checks whether a node inside a cloud is responding or not. Based on this, it checks whether the inactive node is deleted or not from the cloud. If it is, the controller removes the Node Object from the cluster.  
- **Route Controller:** Creates and manages routes within the cloud infrastructure for containers across nodes to communicate with each other.  
- **Service Controller:** Creates and manages service resources in the cluster.

The proccess of keeping all things in sync are called d-consilation


### kube Schedular
kube-scheduler is responsible for scheduling and assigns pods to nodes based on the following constraints:

- Time-sensitivity of a request
- Restrictions due to policies
- Data locality
- Inter-workload interference
- Hardware
- Software and more.
  if one node is high on traffic i will figure out other node to run newly create pod

















