# Cassandra Interface

 This is a Juju charm interface layer. This interface is used for
 connecting to an Jaeger Collector unit.

### Examples

#### Requires

If your charm needs to connect to Jaeger Collector:

  `metadata.yaml`

```yaml
requires:
  jaegercollector:
    interface: jaegercollector
```

  `layer.yaml`

```yaml
includes: ['interface:jaegercollector']
```  

  `reactive/code.py`

```python
@when('cassandra.available')
def connect_to_cassandra(jaegercollector):
    print(jaegercollector.host())
    print(jaegercollector.port())
    print(jaegercollector.cluster_name())

```


#### Provides

If your charm needs to provide Jaeger Collector connection details:

  `metadata.yaml`

```yaml
provides:
  jaegercollector:
    interface: jaegercollector
```

  `layer.yaml`

```yaml
includes: ['interface:jaegercollector']
```

  `reactive/code.py`

```python
@when('client.connected')
def connect_to_client(client):
    conf = config()
    cluster_name = conf['cluster-name']
    port = conf['port']
    client.configure(port,cluster_name)
    for c in client.list_connected_clients_data():
        host_ip = c.get_remote_ip()
```

### States

**{relation_name}.connected** - Denotes that the client has connected to the
Jaeger Collector node(s), but has not yet received the data to configure the
connection.

**{relation_name}.available** - Denotes that the client has connected and
received all the information from the provider to make the connection.

**{relation_name}.departed** - Denotes that the unit has departed from the
Jaeger Collector relationship, and should be removed from any configuration
files, etc.

### Data

- **host** - The units private address
- **port** - TCP Port to use
- **cluster_name** - The Blah

## Maintainers

 - John Arbash Meinel &lt;john@arbash-meinel.com&gt;
