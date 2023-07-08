# Working with custom Controllers

## Contents

1. [Writing a custom controller](#writing-a-custom-controller)
2. [Interacting with the network](#interacting-with-the-network)
3. [Using a custom controller](#using-a-custom-controller)

---

## Writing a custom controller

1. Create a new header and source files:
    ```
    Flexcomm-Simulator/ns-3.35/src/ofswitch13/model/my-controller.h
    Flexcomm-Simulator/ns-3.35/src/ofswitch13/model/my-controller.cc
    ```

2. Include the new files into `Flexcomm-Simulator/ns-3.35/src/ofswitch13/wscript`
    ```python
        module.source = [
        # ...
        'model/my-controller.cc',
        ]

        # ...
        # ...

        header.source = [
        # ...
        'model/my-controller.h',
        ]

    ```

3. Create a new class extending `OFSwitch13Controller` and overriding the required handlers. For more details on the available handlers refer to [OFSwitch13's Documentation](http://www.lrc.ic.unicamp.br/ofswitch13/ofswitch13.pdf). Refer to [simple-controller.h](https://github.com/RuiCunhaM/Flexcomm-Simulator/blob/master/ns-3.35/src/ofswitch13/model/simple-controller.h) for an example:
    ```cpp
    class MyController : public OFSwitch13Controller {

    public:
        static TypeId GetTypeId (void);
        ofl_err HandlePacketIn (struct ofl_msg_packet_in *msg, Ptr<const RemoteSwitch> sw,
                                uint32_t xid);
        // ...

    protected:
        void HandshakeSuccessful (Ptr<const RemoteSwitch> sw);
        // ...
    }
    ```

4. Write your custom logic. Refer to [simple-controller.cc](https://github.com/RuiCunhaM/Flexcomm-Simulator/blob/master/ns-3.35/src/ofswitch13/model/simple-controller.cc) for an example.
    ```cpp
    TypeId MyController::GetTypeId (void) {
        static TypeId tid = TypeId ("ns3::MyController")
                                .SetParent<OFSwitch13Controller> ()
                                .SetGroupName ("OFSwitch13")
                                .AddConstructor<MyController> ();
        return tid;
    }

    void MyController::HandshakeSuccessful (Ptr<const RemoteSwitch> sw) {
        // TODO:
    }

    ofl_err MyController::HandlePacketIn (struct ofl_msg_packet_in *msg, 
                                          Ptr<const RemoteSwitch> sw, uint32_t xid) {
        // TODO:
    }

    // ...
    ```

---

## Interacting with the network

- The [Topology](https://github.com/RuiCunhaM/Flexcomm-Simulator/blob/master/ns-3.35/src/topology/model/topology.h) class provides a simple wrapper around a [boost::graph](https://www.boost.org/doc/libs/1_82_0/libs/graph/doc/index.html) representation of the entire network:
    - Access the Graph
        ```cpp
        Graph Topology::GetGraph ();     
        ```

    - Change edge weights
        ```cpp
        void Topology::UpdateEdgeWeight (Ptr<Node> n1, Ptr<Node> n2, int newWeight);
        ```

    - Calculate shortest path(s) using Dijkstra's Algorithm:
        ```cpp
        vector<Ptr<Node>> Topology::DijkstraShortestPath (Ptr<Node> src, Ptr<Node> dst);
        vector<Ptr<Node>> Topology::DijkstraShortestPaths (Ptr<Node> src);
        ```

    - Access a `Channel`
        ```cpp
        Ptr<Channel> Topology::GetChannel (Ptr<Node> n1, Ptr<Node> n2);
        ```

- Acsess Switch's data
    ```cpp
    // "node" is from type Ptr<Node>
    Ptr<OFSwitch13Device> device = node->GetObject<OFSwitch13Device> ();
    device->GetCpuUsage ();
    // ...
    ```

- Access Channel's data
    ```cpp
    // "channel" is from type Ptr<Channel>
    channel->GetChannelUsage ();
    // ...
    ```

- Sending commands to a switch. Refer to [ofsoftswitch13's Documentation](https://github.com/CPqD/ofsoftswitch13/wiki/Dpctl-Flow-Mod-Cases) for more details about `dpctl`:
    ```cpp
    std::ostringstream cmd;
    cmd << "flow-mod cmd=add,table=0,prio=0 apply:output=ctrl:128";
    DpctlExecute (swDpId, cmd.str ());
    ```

- Converting from `Data Path Id` to `Node Id` and vice-versa:
    ```cpp
    // Convert Data Path Id to Node Id
    uint32_t DpId2Id (uint64_t DpId);

    // Convert Node Id to Data Path Id 
    uint64_t Id2PdId (uint32_t Id);
    ```

---

## Using a custom controller

To use a custom controller in the simulation set the `CONTROLLER` flag equal to the `TypeId` of the intended controller:
```
make run CONTROLLER=ns3::MyController
```

---
